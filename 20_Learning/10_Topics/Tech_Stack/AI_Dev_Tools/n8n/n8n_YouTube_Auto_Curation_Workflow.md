---
tags:
  - n8n
  - workflow
  - automation
  - youtube
  - gemini
  - notion
  - ai
date: 2026-01-23
up: "[[AI_Dev_Tools_MOC]]"
---

# n8n YouTube: Auto-Curation & Daily Report (v2.0)

YouTube에서 관심 키워드(n8n, LangChain, MCP 등)가 포함된 최신 영상을 검색하고, **Gemini 2.5 Flash**를 이용해 **"Top 10 선정 및 3줄 요약"**을 수행한 뒤, **Notion 일일 리포트 페이지**를 자동으로 생성하는 AI 에이전트 워크플로우입니다.

## 🚀 v2.0 주요 변경 사항 (Architecture Shift)

| 특징          | v1.0 (기존)                           | **v2.0 (New)**                                   |
| :------------ | :------------------------------------ | :----------------------------------------------- |
| **처리 방식** | 개별 Loop 처리 (건건이 AI 요약)       | **Batch 처리 (목록 전체를 AI에게 전달)**         |
| **AI 모델**   | 소형 모델 반복 호출                   | **Gemini 2.5 Flash (Long Context)** 1회 호출     |
| **중복 체크** | Notion 전체 목록 조회 (메모리 과부하) | **최근 7일치만 조회 & n8n 메모리 병합** (초고속) |
| **결과물**    | DB에 행(Row)만 무한히 추가            | **매일 1개의 "Daily Report" 페이지** 생성        |

---

## 🛠️ 사전 준비 (Prerequisites)

1.  **n8n 설치**: Self-hosted 또는 Cloud 버전 (v1.0 이상 권장)
2.  **API 자격 증명 (Credentials)**:
    *   **Google Gemini (Vertex AI 또는 AI Studio)**: API Key 발급
    *   **YouTube Data API v3**: API Key 발급
    *   **Notion API**: Internal Integration Token 발급 및 페이지 연결
3.  **Notion 데이터베이스**: 아래 구조로 생성

---

## 📋 Notion 설정 가이드

### 1. 데이터베이스 생성
*   페이지 본문에 `/database inline` 입력하여 생성
*   이름: `YouTube AI Reports` (자유롭게 설정)

### 2. 속성(Properties) 설정
반드시 아래 컬럼명을 정확히 맞춰주세요 (워크플로우에서 참조함).

| 속성명       | 유형(Type)    | 용도                               |
| :----------- | :------------ | :--------------------------------- |
| **URL**      | URL           | 영상 링크 (중복 체크 키)           |
| **채널**     | 텍스트 (Text) | 유튜브 채널명                      |
| **등록일**   | 날짜 (Date)   | 원본 영상 게시일                   |
| **videoId**  | 텍스트 (Text) | 11자리 고유 ID (예: `dQw4w9WgXcQ`) |
| **수집방식** | 선택 (Select) | `n8n_Auto_Archived` (자동 필터용)  |
| **수집일**   | 날짜 (Date)   | n8n이 수집한 시점 (`$now`)         |

### 3. "월별 그루핑" 보기 설정 (강력 추천)
생성된 페이지가 사이드바 트리에 없어도 쉽게 찾을 수 있도록 **본문 내 트리**를 만듭니다.

1.  데이터베이스 우측 상단 `...` (옵션) → **그룹(Group)** 클릭
2.  **그룹화 기준**: `Date` (또는 수집일)
3.  **날짜 기준**: `월(Month)` 또는 `년(Year)`
4.  결과: **[2026년 1월]** 토글을 열면 해당 월의 리포트가 쫙 펼쳐집니다.

---

## 🤖 워크플로우 JSON (Copy & Paste)

아래 코드를 복사하여 n8n 캔버스(`cmd + v`)에 붙여넣으세요.
*   **주의**: 붙여넣기 후 `Credentials` (계정 연결) 빨간색 경고가 뜨면 본인의 계정으로 다시 선택해주세요.

```json
{
  "nodes": [
    {
      "parameters": {
        "rule": {
          "interval": [
            {
              "triggerAtHour": 9
            }
          ]
        }
      },
      "id": "ca7366b6-41b3-4960-aed2-7bf9d69a097e",
      "name": "Schedule Trigger",
      "type": "n8n-nodes-base.scheduleTrigger",
      "typeVersion": 1.1,
      "position": [
        -2048,
        736
      ]
    },
    {
      "parameters": {
        "jsCode": "return [\n  { json: { keyword: 'n8n' } },\n  { json: { keyword: 'LangChain' } },\n  { json: { keyword: 'MCP' } }\n];"
      },
      "id": "564ed1be-0d0d-4d2b-9ad7-f52081969c1f",
      "name": "Keywords",
      "type": "n8n-nodes-base.code",
      "typeVersion": 2,
      "position": [
        -1840,
        640
      ]
    },
    {
      "parameters": {
        "resource": "video",
        "limit": 50,
        "filters": {
          "publishedAfter": "={{ $now.minus({ days: 7 }).toISO() }}",
          "q": "={{ $json.keyword }}"
        },
        "options": {}
      },
      "id": "d8f246b3-bd3e-4e99-b125-70f224e210ee",
      "name": "YouTube Search",
      "type": "n8n-nodes-base.youTube",
      "typeVersion": 1,
      "position": [
        -1616,
        640
      ],
      "credentials": {
        "youTubeOAuth2Api": {
          "id": "YUhgWFc7nEhKWaz3",
          "name": "YouTube account"
        }
      }
    },
    {
      "parameters": {
        "resource": "databasePage",
        "operation": "getAll",
        "databaseId": {
          "__rl": true,
          "value": "2e8b9482d22680d9a747f1f71ec7c8dd",
          "mode": "id"
        },
        "limit": 100,
        "filterType": "json",
        "filterJson": "={\n  \"and\": [\n    {\n      \"property\": \"등록일\",\n      \"date\": {\n        \"on_or_after\": \"{{ $now.minus({ days: 7 }).toISO() }}\"\n      }\n    }\n  ]\n}",
        "options": {}
      },
      "id": "701261f1-58bb-450e-9fdd-e0a167d12f35",
      "name": "Check Duplication (7 Days)",
      "type": "n8n-nodes-base.notion",
      "typeVersion": 2,
      "position": [
        -1840,
        832
      ],
      "executeOnce": true,
      "credentials": {
        "notionApi": {
          "id": "5sAGhUKrWu4S8b8g",
          "name": "Notion account"
        }
      }
    },
    {
      "parameters": {
        "keepOnlySet": true,
        "values": {
          "string": [
            {
              "name": "notion_id",
              "value": "={{ $json.id }}"
            },
            {
              "name": "match_video_id",
              "value": "={{ $json.property_video_id }}"
            }
          ]
        },
        "options": {}
      },
      "id": "e709c8e2-8bf8-43c5-a3ad-22facb5e2c99",
      "name": "Clean Notion Data",
      "type": "n8n-nodes-base.set",
      "typeVersion": 2,
      "position": [
        -1616,
        832
      ]
    },
    {
      "parameters": {
        "mode": "combine",
        "advanced": true,
        "mergeByFields": {
          "values": [
            {
              "field1": "id.videoId",
              "field2": "match_video_id"
            }
          ]
        },
        "joinMode": "enrichInput1",
        "options": {}
      },
      "id": "ad031284-c3d1-4aa8-89f8-7b4743d4f5e3",
      "name": "Merge",
      "type": "n8n-nodes-base.merge",
      "typeVersion": 3,
      "position": [
        -1168,
        736
      ]
    },
    {
      "parameters": {
        "conditions": {
          "string": [
            {
              "value1": "={{ $json.notion_id }}",
              "operation": "isEmpty"
            }
          ]
        }
      },
      "id": "6bb77f2d-74d6-4f4e-b326-cf3f0ac533d4",
      "name": "Filter New Videos",
      "type": "n8n-nodes-base.if",
      "typeVersion": 1,
      "position": [
        -960,
        736
      ]
    },
    {
      "parameters": {
        "aggregate": "aggregateAllItemData",
        "destinationFieldName": "allVideos",
        "options": {}
      },
      "id": "7a78d99e-3fc5-4f46-82ff-deab2b013a95",
      "name": "Aggregate (For AI)",
      "type": "n8n-nodes-base.aggregate",
      "typeVersion": 1,
      "position": [
        -672,
        624
      ]
    },
    {
      "parameters": {
        "resource": "block",
        "blockId": {
          "__rl": true,
          "value": "={{ $json.id }}",
          "mode": "id"
        },
        "blockUi": {
          "blockValues": [
            {
              "type": "heading_1",
              "textContent": "📊 오늘의 기술 트렌드 요약"
            },
            {
              "textContent": "={{ $('JSON Parser (Cleaner)').item.json.daily_summary }}"
            },
            {
              "type": "heading_2",
              "textContent": "🏆 Top 10 선정 영상"
            },
            {
              "textContent": "={{ $('JSON Parser (Cleaner)').item.json.top_list_part1 }}"
            },
            {
              "textContent": "={{ $('JSON Parser (Cleaner)').item.json.top_list_part2 }}"
            },
            {
              "type": "heading_3",
              "textContent": "👀 그 외 주목할 만한 영상 (Honorable Mentions)"
            },
            {
              "textContent": "={{ $('JSON Parser (Cleaner)').item.json.mentions_markdown }}"
            }
          ]
        }
      },
      "id": "b408ef43-bb33-401b-9b50-288815cbef4c",
      "name": "Writes to Page",
      "type": "n8n-nodes-base.notion",
      "typeVersion": 2,
      "position": [
        272,
        624
      ],
      "credentials": {
        "notionApi": {
          "id": "5sAGhUKrWu4S8b8g",
          "name": "Notion account"
        }
      }
    },
    {
      "parameters": {
        "options": {}
      },
      "id": "e68d9898-d584-41cc-b088-a648ae0c2a98",
      "name": "Loop (Archiving)",
      "type": "n8n-nodes-base.splitInBatches",
      "typeVersion": 3,
      "position": [
        -672,
        928
      ]
    },
    {
      "parameters": {
        "resource": "databasePage",
        "databaseId": {
          "__rl": true,
          "value": "2e8b9482d22680d9a747f1f71ec7c8dd",
          "mode": "id"
        },
        "title": "={{ $json.snippet.title }}",
        "propertiesUi": {
          "propertyValues": [
            {
              "key": "URL|url",
              "urlValue": "=https://www.youtube.com/watch?v={{ $json.id.videoId }}"
            },
            {
              "key": "채널|rich_text",
              "textContent": "={{ $json.snippet.channelTitle }}"
            },
            {
              "key": "등록일|date",
              "date": "={{ $json.snippet.publishedAt }}"
            },
            {
              "key": "videoId|rich_text",
              "textContent": "={{ $json.id.videoId }}"
            },
            {
              "key": "수집방식|select",
              "selectValue": "n8n_Auto_Archived"
            },
            {
              "key": "수집일|date",
              "date": "={{ $now }}"
            }
          ]
        },
        "options": {}
      },
      "id": "872d8339-3b28-43b3-af6f-3025e95a49c9",
      "name": "Save Raw Data",
      "type": "n8n-nodes-base.notion",
      "typeVersion": 2,
      "position": [
        -448,
        928
      ],
      "credentials": {
        "notionApi": {
          "id": "5sAGhUKrWu4S8b8g",
          "name": "Notion account"
        }
      }
    },
    {
      "parameters": {
        "options": {}
      },
      "type": "@n8n/n8n-nodes-langchain.lmChatGoogleGemini",
      "typeVersion": 1,
      "position": [
        -448,
        784
      ],
      "id": "1db1e6ef-f465-43cd-90db-8dde090c90ee",
      "name": "Google Gemini Chat Model",
      "credentials": {
        "googlePalmApi": {
          "id": "ODDsxSehz4SYiXm0",
          "name": "Google Gemini(PaLM) Api account"
        }
      }
    },
    {
      "parameters": {
        "prompt": "=당신은 Tech 유튜브 큐레이터입니다.\\n제공된 {{ $json.allVideos.length }}개의 영상 목록(allVideos)을 분석해서 일일 리포트를 작성하세요.\\n\\n[필수 요청사항]\\n1. 전체 목록 중 가장 가치 있는 **Top 10** 영상을 선정하여 'top_list'에 담으세요. (상세 요약 필수)\\n2. Top 10에는 못 들었지만 좋은 **Next 10** 영상을 선정하여 'mentions_list'에 담으세요. (요약 없이 제목만)\\n3. 결과는 오직 **JSON** 형식으로만 출력하세요.\\n\\n[JSON 스키마]\\n{\\n  \\\"daily_summary\\\": \\\"오늘의 기술 트렌드 3줄 요약\\\",\\n  \\\"top_list\\\": [\\n    {\\n      \\\"rank\\\": 1,\\n      \\\"keyword\\\": \\\"n8n\\\",\\n      \\\"title\\\": \\\"영상 제목\\\",\\n      \\\"summary\\\": \\\"핵심 요약\\\",\\n      \\\"url\\\": \\\"https://youtu.be/...\\\"\\n    }, ...\\n  ],\\n  \\\"mentions_list\\\": [\\n    {\\n      \\\"title\\\": \\\"영상 제목\\\",\\n      \\\"url\\\": \\\"https://youtu.be/...\\\"\\n    }, ...\\n  ]\\n}\\n\\n[데이터]\\n{{ JSON.stringify($json.allVideos) }}"
      },
      "id": "ab215aec-de54-4359-8edb-df00565a76cd",
      "name": "Gemini 2.5 Flash (Curation)",
      "type": "@n8n/n8n-nodes-langchain.chainLlm",
      "typeVersion": 1,
      "position": [
        -448,
        624
      ]
    },
    {
      "parameters": {
        "jsCode": "// n8n Code 노드 - AI 출력 JSON 파싱 & Notion용 마크다운 포맷팅\n\n// 1. 입력에서 텍스트 가져오기 (여러 필드 대응)\nconst raw = $input.first().json.text \n         || $input.first().json.output \n         || $input.first().json.content \n         || \"\";\n\n// 2. 마크다운 코드 블록 제거 및 정리\nconst cleaned = raw\n  .replace(/```json\\s*/g, '')     // ```json 제거\n  .replace(/```/g, '')            // 나머지 ``` 제거\n  .trim();\n\n// 3. JSON 파싱 시도\nlet parsed;\ntry {\n  parsed = JSON.parse(cleaned);\n} catch (err) {\n  return [{\n    json: {\n      error: true,\n      message: \"JSON 파싱 실패\",\n      raw_input: raw,\n      cleaned_input: cleaned,\n      parse_error: err.message\n    }\n  }];\n}\n\n// 4. 필요한 데이터가 있는지 확인 (방어적 코딩)\nif (!parsed || typeof parsed !== 'object') {\n  return [{\n    json: { error: true, message: \"유효한 객체가 아님\", raw: raw }\n  }];\n}\n\n// ────────────────────────────────────────────────\n// 헬퍼 함수들\n// ────────────────────────────────────────────────\n\nconst formatTopItem = (item) => \n  `**${item.rank}. ${item.title}**\\n` +\n  `- 🏷️ ${item.keyword || '-'}\\n` +\n  `- 📝 ${item.summary || '요약 없음'}\\n` +\n  `- 🔗 ${item.url || '#' }`;\n\nconst formatTopList = (list) => \n  list.map(formatTopItem).join('\\n\\n');\n\nconst formatMentions = (list) => \n  (list || []).map(item => \n    `- ${item.title || '제목 없음'}: ${item.url || '#' }`\n  ).join('\\n');\n\n// ────────────────────────────────────────────────\n// 결과 구성\n// ────────────────────────────────────────────────\n\nconst result = {\n  daily_summary: parsed.daily_summary || \"요약 없음\",\n  \n  // Notion 2000자 제한 고려 → Top 10을 5개씩 분리\n  top_list_part1: formatTopList(parsed.top_list?.slice(0, 5)  || []),\n  top_list_part2: formatTopList(parsed.top_list?.slice(5, 10) || []),\n  \n  mentions_markdown: formatMentions(parsed.mentions_list),\n  \n  // 디버깅용 (필요 시 주석 해제)\n  // parsed_data: parsed,\n  // raw_input_for_debug: raw.substring(0, 500) + \"...\"\n};\n\nreturn [{ json: result }];"
      },
      "type": "n8n-nodes-base.code",
      "typeVersion": 2,
      "position": [
        -112,
        624
      ],
      "id": "3200a3b6-a547-46a0-8de2-760cfcbdd3aa",
      "name": "JSON Parser (Cleaner)"
    },
    {
      "parameters": {
        "resource": "databasePage",
        "databaseId": {
          "__rl": true,
          "value": "2f1b9482d22680e6bf33d0749551f872",
          "mode": "id"
        },
        "title": "={{ $now.toFormat('yyyy-MM-dd') }} Daily AI Report 📰",
        "propertiesUi": {
          "propertyValues": [
            {
              "key": "이름|title",
              "title": "={{ $now.toFormat('yyyy-MM-dd') }} Daily AI Report 📰"
            },
            {
              "key": "Date|date",
              "date": "={{ $now.toISO() }}"
            }
          ]
        },
        "options": {}
      },
      "type": "n8n-nodes-base.notion",
      "typeVersion": 2.2,
      "position": [
        96,
        624
      ],
      "id": "e1f9dab7-320f-442b-9d3a-3ac3586a23bf",
      "name": "Create Daily Page",
      "credentials": {
        "notionApi": {
          "id": "5sAGhUKrWu4S8b8g",
          "name": "Notion account"
        }
      }
    }
  ],
  "connections": {
    "Schedule Trigger": {
      "main": [
        [
          {
            "node": "Keywords",
            "type": "main",
            "index": 0
          },
          {
            "node": "Check Duplication (7 Days)",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Keywords": {
      "main": [
        [
          {
            "node": "YouTube Search",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "YouTube Search": {
      "main": [
        [
          {
            "node": "Merge",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Check Duplication (7 Days)": {
      "main": [
        [
          {
            "node": "Clean Notion Data",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Clean Notion Data": {
      "main": [
        [
          {
            "node": "Merge",
            "type": "main",
            "index": 1
          }
        ]
      ]
    },
    "Merge": {
      "main": [
        [
          {
            "node": "Filter New Videos",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Filter New Videos": {
      "main": [
        [
          {
            "node": "Aggregate (For AI)",
            "type": "main",
            "index": 0
          },
          {
            "node": "Loop (Archiving)",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Aggregate (For AI)": {
      "main": [
        [
          {
            "node": "Gemini 2.5 Flash (Curation)",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Loop (Archiving)": {
      "main": [
        [],
        [
          {
            "node": "Save Raw Data",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Save Raw Data": {
      "main": [
        [
          {
            "node": "Loop (Archiving)",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Google Gemini Chat Model": {
      "ai_languageModel": [
        [
          {
            "node": "Gemini 2.5 Flash (Curation)",
            "type": "ai_languageModel",
            "index": 0
          }
        ]
      ]
    },
    "Gemini 2.5 Flash (Curation)": {
      "main": [
        [
          {
            "node": "JSON Parser (Cleaner)",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "JSON Parser (Cleaner)": {
      "main": [
        [
          {
            "node": "Create Daily Page",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Create Daily Page": {
      "main": [
        [
          {
            "node": "Writes to Page",
            "type": "main",
            "index": 0
          }
        ]
      ]
    }
  },
  "pinData": {},
  "meta": {
    "templateCredsSetupCompleted": true,
    "instanceId": "e284c5e5fcc01d6abca2638dd95b3d663d59e95f35f4687f7e8bb8d9607eed51"
  }
}
```

---

## 🧩 Core Logic: Prompt & Script

워크플로우의 핵심인 **AI 프롬프트**와 **데이터 파싱 스크립트**입니다. 유지보수 시 이 부분을 참고하여 수정하세요.

### 1. Gemini AI System Prompt
`Gemini 2.5 Flash (Curation)` 노드에 설정된 프롬프트입니다.

```text
당신은 Tech 유튜브 큐레이터입니다.
제공된 {{ $json.allVideos.length }}개의 영상 목록(allVideos)을 분석해서 일일 리포트를 작성하세요.

[필수 요청사항]
1. 전체 목록 중 가장 가치 있는 **Top 10** 영상을 선정하여 'top_list'에 담으세요. (상세 요약 필수)
2. Top 10에는 못 들었지만 좋은 **Next 10** 영상을 선정하여 'mentions_list'에 담으세요. (요약 없이 제목만)
3. 결과는 오직 **JSON** 형식으로만 출력하세요.

[JSON 스키마]
{
  "daily_summary": "오늘의 기술 트렌드 3줄 요약",
  "top_list": [
    {
      "rank": 1,
      "keyword": "n8n",
      "title": "영상 제목",
      "summary": "핵심 요약",
      "url": "https://youtu.be/..."
    }, ...
  ],
  "mentions_list": [
    {
      "title": "영상 제목",
      "url": "https://youtu.be/..."
    }, ...
  ]
}

[데이터]
{{ JSON.stringify($json.allVideos) }}
```

### 2. JSON Parser (Javascript)
`JSON Parser (Cleaner)` 노드의 전체 코드입니다. 마크다운 제거, 리스트 분할, Raw URL 변환 로직이 포함되어 있습니다.

```javascript
// n8n Code 노드 - AI 출력 JSON 파싱 & Notion용 마크다운 포맷팅

// 1. 입력에서 텍스트 가져오기 (여러 필드 대응)
const raw = $input.first().json.text 
         || $input.first().json.output 
         || $input.first().json.content 
         || "";

// 2. 마크다운 코드 블록 제거 및 정리
const cleaned = raw
  .replace(/```json\s*/g, '')     // ```json 제거
  .replace(/```/g, '')            // 나머지 ``` 제거
  .trim();

// 3. JSON 파싱 시도
let parsed;
try {
  parsed = JSON.parse(cleaned);
} catch (err) {
  return [{
    json: {
      error: true,
      message: "JSON 파싱 실패",
      raw_input: raw,
      cleaned_input: cleaned,
      parse_error: err.message
    }
  }];
}

// 4. 필요한 데이터가 있는지 확인 (방어적 코딩)
if (!parsed || typeof parsed !== 'object') {
  return [{
    json: { error: true, message: "유효한 객체가 아님", raw: raw }
  }];
}

// ────────────────────────────────────────────────
// 헬퍼 함수들
// ────────────────────────────────────────────────

// URL을 'Text: URL' 형태(Raw Link)로 변환하여 Notion 클릭 문제 해결
const formatTopItem = (item) => 
  `**${item.rank}. ${item.title}**\n` +
  `- 🏷️ ${item.keyword || '-'}\n` +
  `- 📝 ${item.summary || '요약 없음'}\n` +
  `- 🔗 ${item.url || '#' }`;

const formatTopList = (list) => 
  list.map(formatTopItem).join('\n\n');

const formatMentions = (list) => 
  (list || []).map(item => 
    `- ${item.title || '제목 없음'}: ${item.url || '#' }`
  ).join('\n');

// ────────────────────────────────────────────────
// 결과 구성
// ────────────────────────────────────────────────

const result = {
  daily_summary: parsed.daily_summary || "요약 없음",
  
  // Notion 2000자 제한 고려 → Top 10을 5개씩 분리
  top_list_part1: formatTopList(parsed.top_list?.slice(0, 5)  || []),
  top_list_part2: formatTopList(parsed.top_list?.slice(5, 10) || []),
  
  mentions_markdown: formatMentions(parsed.mentions_list),
};

return [{ json: result }];
```

---

## 🚨 자주 발생하는 문제 (Troubleshooting)

### Q1. Notion에서 "Bad Request (400)" 에러가 나요.
*   **원인**: 문단(Block) 하나의 길이가 2000자를 넘어서 그렇습니다.
*   **해결**: 위 코드(`v3.2`)에는 이미 Top 10을 반으로 쪼개는 로직이 들어있어 발생하지 않습니다. 혹시나 발생한다면 `FormatTopList` 함수에서 더 잘게 쪼개야 합니다.

### Q2. Notion 링크가 클릭이 안 돼요.
*   **원인**: `textContent` 속성은 마크다운 링크(`[]()`)를 지원하지 않습니다.
*   **해결**: 위 코드는 URL을 `https://...` 형태로 그대로 노출하므로, Notion이 자동으로 링크로 변환해줍니다. (이미 해결됨)

### Q3. AI가 헛소리를 하거나 빈 칸으로 나와요.
*   **원인**: 입력 데이터(`allVideos`)가 너무 많아(100개 이상) 토큰 제한을 넘겼거나, 검색 결과가 0개인 경우입니다.
*   **해결**:
    1.  YouTube Search 노드에서 `Limit`을 50 정도로 조절하세요.
    2.  `Filter New Videos` 노드 뒤에 데이터가 0개면 종료하는 `If` 노드를 추가해도 좋습니다.

---

## 📚 관련 문서 (References)
*   **[[n8n_Workflow_Improvement_Plan]]**: 이 워크플로우의 v2.0 개선을 위한 초기 분석 및 제안 문서입니다.
