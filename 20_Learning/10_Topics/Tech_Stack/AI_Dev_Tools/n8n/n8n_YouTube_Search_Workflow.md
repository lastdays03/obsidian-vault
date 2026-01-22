---
tags: [n8n, workflow, youtube, automation, notion]
Up: [[n8n_Automation_Use_Cases_2026]]
---

# n8n YouTube Search & Filter Workflow

YouTube 검색 결과 중 **Notion 데이터베이스에 이미 존재하는 영상(제목 기준)을 제외**하고 새로운 영상만 골라내는 워크플로우입니다.

## 📋 Prerequisites (준비사항)

1. **Google & YouTube**: API Key 및 OAuth 설정 완료.
2. **Notion**:
   - `Notion Integration` 생성 및 n8n 연결.
   - **Database ID** 준비:
     1. 웹 브라우저에서 노션 데이터베이스 페이지를 엽니다.
     2. URL을 확인합니다: `https://www.notion.so/myworkspace/a8aec43384f447ed84390e8e42c2e089?v=...`
     3. `notion.so/` 뒤의 **32자 문자열**(`a8aec433...`)이 데이터베이스 ID입니다.
   - **연결 확인**: 데이터베이스 페이지 우측 상단 `...` 클릭 > `Connect(연결)` > 내 Integration 선택 (이 과정이 없으면 n8n이 접근 불가).
   - 데이터베이스에 영상 제목이 저장된 속성 이름 확인 (기본값: `Name`).


## 🔄 Workflow Logic (Lookup 방식 - URL 기준)

1. **Schedule Trigger (Daily 9:00 AM)**: 매일 오전 9시에 자동으로 실행되어 YouTube 검색을 시작합니다.
   - **Filter Applied**: `publishedAfter` 옵션으로 **최근 7일**(`days: 7`) 영상만 검색합니다. 기간을 늘리거나 줄이려면 이 숫자를 수정하세요.
   - (참고: 기본 출력에는 `url` 필드가 없고 `videoId`만 있습니다).
2. **Loop & Check**:
   - **Construct URL**: `https://www.youtube.com/watch?v=` + `videoId` 조합으로 주소를 만듭니다.
   - **Notion Lookup**: 생성된 URL과 일치하는 데이터가 노션에 있는지 확인합니다.
     - *전제조건: Notion 데이터베이스에 `URL` 속성이 있어야 합니다.*
   - **If (Exist?)**:
     - **False (New)**: 노션에 추가 (Create Page)

> **💡 중요 Tip: "빈 항목(Empty Item)"은 정상입니다!**
> `If` 노드의 False로 빠진 데이터가 "Empty"로 보이는 것은 **정상**입니다. (노션에 없다는 뜻이니까요!)
> 
> **다음 단계(`Create Page`)에서 데이터 넣는 법:**
> 데이터가 비어있으므로 바로 앞 노드(Input)에서 드래그하면 안 됩니다.
> 1. 입력창 옆의 **Expression** 탭 클릭
> 2. 왼쪽 패널의 **Nodes** > **YouTube Search** > Output Data > JSON > snippet > title 선택
> 3. 즉, `$('YouTube Search').item.json...` 형태로 **원본 유튜브 검색 노드**를 직접 참조해야 합니다.

## 💻 Workflow JSON

```json
{
  "name": "YouTube Search with Lookup Filter (URL)",
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
      "id": "b522cccc-f638-4161-809d-e540eaa55704",
      "name": "Schedule Trigger",
      "type": "n8n-nodes-base.scheduleTrigger",
      "typeVersion": 1.1,
      "position": [
        -784,
        416
      ]
    },
    {
      "parameters": {
        "jsCode": "return [\n  { json: { keyword: '강아지' } },\n  { json: { keyword: '테슬라' } }\n];"
      },
      "id": "b1f854f1-0acd-4979-83c6-3615e81a13f3",
      "name": "Define Keywords",
      "type": "n8n-nodes-base.code",
      "typeVersion": 2,
      "position": [
        -576,
        416
      ]
    },
    {
      "parameters": {
        "resource": "video",
        "limit": 10,
        "filters": {
          "q": "={{ $json.keyword }}",
          "publishedAfter": "={{ $now.minus({ days: 7 }).toISO() }}"
        },
        "options": {}
      },
      "id": "50fec062-a978-4c07-96c8-9ce04ec65cbe",
      "name": "YouTube Search",
      "type": "n8n-nodes-base.youTube",
      "typeVersion": 1,
      "position": [
        -384,
        416
      ],
      "credentials": {
        "youTubeOAuth2Api": {
          "id": "YOUR_YOUTUBE_CREDENTIAL_ID",
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
          "value": "YOUR_NOTION_DATABASE_ID_HERE",
          "mode": "list",
          "cachedResultName": "YOUR_DATABASE_NAME",
          "cachedResultUrl": "https://www.notion.so/YOUR_DATABASE_ID"
        },
        "filterType": "json",
        "filterJson": "={\n  \"and\": [\n    {\n      \"property\": \"URL\",\n      \"url\": {\n        \"contains\": \"{{ $json.id.videoId }}\"\n      }\n    }\n  ]\n}",
        "options": {}
      },
      "id": "0f4228ad-b9e0-4837-a18f-b002a9aa16e4",
      "name": "Check Existence",
      "type": "n8n-nodes-base.notion",
      "typeVersion": 2,
      "position": [
        -224,
        512
      ],
      "alwaysOutputData": true,
      "credentials": {
        "notionApi": {
          "id": "YOUR_NOTION_CREDENTIAL_ID",
          "name": "Notion account"
        }
      }
    },
    {
      "parameters": {
        "model": "llama-3.1-8b-instant",
        "options": {}
      },
      "id": "25638919-695d-43fb-a1b1-4d412adf3043",
      "name": "Groq Chat Model",
      "type": "@n8n/n8n-nodes-langchain.lmChatGroq",
      "typeVersion": 1,
      "position": [
        464,
        608
      ],
      "credentials": {
        "groqApi": {
          "id": "YOUR_GROQ_CREDENTIAL_ID",
          "name": "Groq account"
        }
      }
    },
    {
      "parameters": {
        "resource": "databasePage",
        "databaseId": {
          "__rl": true,
          "value": "YOUR_NOTION_DATABASE_ID_HERE",
          "mode": "list",
          "cachedResultName": "YOUR_DATABASE_NAME",
          "cachedResultUrl": "https://www.notion.so/YOUR_DATABASE_ID"
        },
        "title": "={{ $('If Exists').item.json.snippet_1.title }}",
        "propertiesUi": {
          "propertyValues": [
            {
              "key": "URL|url",
              "urlValue": "=https://www.youtube.com/watch?v={{ $('If Exists').item.json.id_1.videoId }}"
            },
            {
              "key": "AI요약|rich_text",
              "textContent": "={{ $json.summary }}"
            },
            {
              "key": "채널|rich_text",
              "textContent": "={{ $('Merge').item.json.snippet_1.channelTitle }}"
            },
            {
              "key": "등록일|date",
              "date": "={{ $('Merge').item.json.snippet_1.publishedAt }}"
            },
            {
              "key": "수집일|date",
              "date": "={{ $now }}"
            },
            {
              "key": "자막여부|checkbox"
            },
            {
              "key": "기술태그|multi_select",
              "multiSelectValue": "={{ $json.tags }}"
            },
            {
              "key": "videoId|rich_text",
              "textContent": "={{ $('Merge').item.json.id_1.videoId }}"
            }
          ]
        },
        "options": {}
      },
      "id": "aa4a3833-d2f7-4880-bc14-47e09e748be9",
      "name": "Create Page",
      "type": "n8n-nodes-base.notion",
      "typeVersion": 2.2,
      "position": [
        976,
        416
      ],
      "credentials": {
        "notionApi": {
          "id": "YOUR_NOTION_CREDENTIAL_ID",
          "name": "Notion account"
        }
      }
    },
    {
      "parameters": {
        "promptType": "define",
        "text": "=아래 유튜브 영상 설명을 분석해서 내용을 요약하고 핵심 태그를 추출해줘.\n        결과는 반드시 **순수 JSON 포맷**(`{ ... }`)으로만 출력해야 해. 설명이나 번호 매기기, 마크다운(```json)을 절대 포함하지 마.\n\n        [출력 필드]\n        - summary: 영상 설명의 핵심 내용을 한국어 3줄 분량의 **하나의 문자열(String)**로 요약 (줄바꿈이 필요하면 \\n 사용, 배열 절대 금지)\n        - tags: 영상 내용과 관련된 핵심 기술 키워드 3~5개 (한국어) 리스트\n\n        [예시]\n        {\n          \"summary\": \"이 영상은 n8n을 사용하여 유튜브 자동화를 구축하는 방법을 설명합니다.\\n특히 LLM을 활용하여 영상 설명을 자동으로 요약하고, 핵심 태그를 추출하여 생산성을 높이는 과정에 집중합니다.\",\n          \"tags\": [\"n8n\", \"자동화\", \"LLM\", \"YouTube API\"]\n        }\n\n        제목: {{ $json.snippet.title }}\n        설명: {{ $json.snippet.description }}",
        "batching": {}
      },
      "type": "@n8n/n8n-nodes-langchain.chainLlm",
      "typeVersion": 1.9,
      "position": [
        464,
        416
      ],
      "id": "37bd2d36-1fcd-49a9-b6ce-07642e227b18",
      "name": "Basic LLM Chain"
    },
    {
      "parameters": {
        "mode": "combine",
        "advanced": true,
        "mergeByFields": {
          "values": [
            {
              "field1": "id.videoId",
              "field2": "property_video_id"
            }
          ]
        },
        "joinMode": "enrichInput1",
        "options": {
          "clashHandling": {
            "values": {
              "resolveClash": "addSuffix"
            }
          }
        }
      },
      "type": "n8n-nodes-base.merge",
      "typeVersion": 3.2,
      "position": [
        -96,
        320
      ],
      "id": "09b5593f-a1d4-484c-b969-1f19c8d6ac5f",
      "name": "Merge"
    },
    {
      "parameters": {
        "conditions": {
          "string": [
            {
              "value1": "={{ $json.id_2 }}",
              "operation": "isNotEmpty"
            }
          ]
        }
      },
      "id": "0f3c0a0a-3f66-4fc1-80ef-b7ae080ba87c",
      "name": "If Exists",
      "type": "n8n-nodes-base.if",
      "typeVersion": 1,
      "position": [
        96,
        320
      ]
    },
    {
      "parameters": {
        "jsCode": "const llmOutput = $input.item.json.text || \"{}\";\n        \n        // 1. 기초 정제: 마크다운 코드 블록 및 앞뒤 공백 제거\n        const cleanJson = llmOutput.replace(/```json/g, \"\").replace(/```/g, \"\").trim();\n        \n        let parsed = {};\n        \n        try {\n          // 2. 표준 JSON 파싱 시도\n          parsed = JSON.parse(cleanJson);\n        } catch (e) {\n          // 3. 파싱 실패 시 (예: 1. \"summary\": ... 같은 번호 목록 포맷), 정규식으로 강제 추출 시도\n          const summaryMatch = cleanJson.match(/\"summary\":\\s*\"((?:[^\"\\\\]|\\\\.)*)\"/s);\n          const tagsMatch = cleanJson.match(/\"tags\":\\s*(\\[[^\\]]*\\])/s);\n          \n          let extractedTags = [];\n          if (tagsMatch) {\n            try { \n                extractedTags = JSON.parse(tagsMatch[1]); \n            } catch(tagErr) {\n                // 태그 파싱 실패 시 콤마로 단순 분리 시도\n                extractedTags = tagsMatch[1].replace(/[\\[\\]\"]/g, '').split(',').map(t => t.trim());\n            }\n          }\n\n          parsed = { \n            summary: summaryMatch ? summaryMatch[1] : (cleanJson || \"요약 실패\"), \n            tags: extractedTags\n          };\n        }\n\n        // 4. 데이터 타입 보정\n        let summaryText = parsed.summary;\n        if (Array.isArray(summaryText)) {\n          summaryText = summaryText.join(\"\\n\");\n        } else if (typeof summaryText !== 'string') {\n           summaryText = String(summaryText || \"\");\n        }\n\n        let tagsArray = parsed.tags;\n        if (!Array.isArray(tagsArray)) {\n          tagsArray = [];\n        }\n\n        // 5. 결과 반환\n        return {\n          json: {\n            summary: summaryText,\n            tags: tagsArray,\n            ...$input.item.json // 기존 YouTube 데이터 보존\n          }\n        };"
      },
      "type": "n8n-nodes-base.code",
      "typeVersion": 2,
      "position": [
        784,
        416
      ],
      "id": "54c46edf-d1a6-4d94-8bac-399ecbcfc4c3",
      "name": "Parse LLM Response"
    },
    {
      "parameters": {
        "options": {}
      },
      "type": "n8n-nodes-base.splitInBatches",
      "typeVersion": 3,
      "position": [
        96,
        656
      ],
      "id": "e8483d6a-65ae-4a8e-add2-4f19fb986115",
      "name": "Loop Over Items"
    },
    {
      "parameters": {
        "amount": 1
      },
      "type": "n8n-nodes-base.wait",
      "typeVersion": 1.1,
      "position": [
        288,
        416
      ],
      "id": "d3311dca-d5ae-46e0-8f4c-83f37a557196",
      "name": "Wait",
      "webhookId": "7811cc35-4e30-4a0a-95f6-165f6b358601"
    }
  ],
  "connections": {
    "Schedule Trigger": { "main": [[{ "node": "Define Keywords", "type": "main", "index": 0 }]] },
    "Define Keywords": { "main": [[{ "node": "YouTube Search", "type": "main", "index": 0 }]] },
    "YouTube Search": { "main": [[{ "node": "Check Existence", "type": "main", "index": 0 }, { "node": "Merge", "type": "main", "index": 0 }]] },
    "Check Existence": { "main": [[{ "node": "Merge", "type": "main", "index": 1 }]] },
    "Merge": { "main": [[{ "node": "If Exists", "type": "main", "index": 0 }]] },
    "If Exists": { "main": [null, [{ "node": "Loop Over Items", "type": "main", "index": 0 }]] },
    "Loop Over Items": { "main": [[{ "node": "Wait", "type": "main", "index": 0 }]] },
    "Wait": { "main": [[{ "node": "Basic LLM Chain", "type": "main", "index": 0 }]] },
    "Groq Chat Model": { "ai_languageModel": [[{ "node": "Basic LLM Chain", "type": "ai_languageModel", "index": 0 }]] },
    "Basic LLM Chain": { "main": [[{ "node": "Parse LLM Response", "type": "main", "index": 0 }]] },
    "Parse LLM Response": { "main": [[{ "node": "Create Page", "type": "main", "index": 0 }]] },
    "Create Page": { "main": [[{ "node": "Loop Over Items", "type": "main", "index": 0 }]] }
  }
}
```

### 🛠️ Manual Node Setup Guide (LLM Summarization)

복사/붙여넣기가 안 될 경우, 아래 순서대로 3개의 노드를 직접 추가하고 설정하세요.

### 1. Loop Over Items & Wait (Batch & Rate Limit)
*   **이유**: 한 번에 여러 영상이 검색되었을 때, **순차적(1개씩)**으로 처리하여 **LLM API 속도 제한(Rate Limit)**을 피하기 위함입니다.
*   **추가 1**: `Loop Over Items` 노드 추가
    *   **Batch Size**: `1` (매우 중요)
*   **추가 2**: `Wait` 노드 추가
    *   **Wait Amount**: `1`
    *   **Unit**: `Seconds` (Groq API 제한 고려)
*   **연결**: `If Exists` (False/New) → `Loop Over Items` → `Wait`

### 2. Basic LLM Chain (영상 설명 요약 + 태그 생성)
*   **이유**: 자막 추출 대신 **영상 설명(Description)**을 분석하여 **요약**과 **기술 태그**를 동시에 생성합니다.
*   **추가**: `Basic LLM Chain` 검색 (LangChain) > 추가
*   **연결**:
    *   **Input**: `Wait` 노드의 출력 → `Basic LLM Chain`의 **왼쪽(Main)** 입력
*   **설정**:
    *   **Prompt**:
        ```text
        아래 유튜브 영상 설명을 분석해서 내용을 요약하고 핵심 태그를 추출해줘.
        결과는 반드시 **순수 JSON 포맷**(`{ ... }`)으로만 출력해야 해. 설명이나 번호 매기기, 마크다운(```json)을 절대 포함하지 마.

        [출력 필드]
        - summary: 영상 설명의 핵심 내용을 한국어 3줄 분량의 **하나의 문자열(String)**로 요약 (줄바꿈이 필요하면 \n 사용, 배열 절대 금지)
        - tags: 영상 내용과 관련된 핵심 기술 키워드 3~5개 (한국어) 리스트

        [예시]
        {
          "summary": "이 영상은 n8n을 사용하여 유튜브 자동화를 구축하는 방법을 설명합니다.\n특히 LLM을 활용하여 영상 설명을 자동으로 요약하고, 핵심 태그를 추출하여 생산성을 높이는 과정에 집중합니다.",
          "tags": ["n8n", "자동화", "LLM", "YouTube API"]
        }

        제목: {{ $json.snippet_1.title }}
        설명: {{ $json.snippet_1.description }}
        ```

### 3. Groq Chat Model (LLM Model)
*   **연결**:
    *   **`Basic LLM Chain`의 아래쪽 `Model *` 포트**에서 선을 끌어당겨서 놓습니다.
    *   나오는 메뉴에서 **`Groq Chat Model`**을 검색해서 추가합니다.
*   **설정**:
    *   **Credential**: `Groq API` 선택
    *   **Model**: `llama-3.1-8b-instant` (JSON 출력 성능이 우수함)

### 4. Parse LLM Response (데이터 정제)
*   **이유**: LLM이 생성한 텍스트(JSON)를 n8n이 이해할 수 있는 데이터로 깔끔하게 변환하고, 에러를 방지합니다. (Inline 수식보다 훨씬 안정적임)
*   **추가**: `Code` 노드 추가
*   **연결**: `Basic LLM Chain` 출력 → 이 노드의 입력
*   **설정**:
    *   **Language**: `JavaScript`
    *   **Code**:
        ```javascript
        const llmOutput = $input.item.json.text || "{}";
        
        // 1. 기초 정제: 마크다운 코드 블록 및 앞뒤 공백 제거
        const cleanJson = llmOutput.replace(/```json/g, "").replace(/```/g, "").trim();
        
        let parsed = {};
        
        try {
          // 2. 표준 JSON 파싱 시도
          parsed = JSON.parse(cleanJson);
        } catch (e) {
          // 3. 파싱 실패 시 (예: 1. "summary": ... 같은 번호 목록 포맷), 정규식으로 강제 추출 시도
          const summaryMatch = cleanJson.match(/"summary":\s*"((?:[^"\\]|\\.)*)"/s);
          const tagsMatch = cleanJson.match(/"tags":\s*(\[[^\]]*\])/s);
          
          let extractedTags = [];
          if (tagsMatch) {
            try { 
                extractedTags = JSON.parse(tagsMatch[1]); 
            } catch(tagErr) {
                // 태그 파싱 실패 시 콤마로 단순 분리 시도
                extractedTags = tagsMatch[1].replace(/[\[\]"]/g, '').split(',').map(t => t.trim());
            }
          }

          parsed = { 
            summary: summaryMatch ? summaryMatch[1] : (cleanJson || "요약 실패"), 
            tags: extractedTags
          };
        }

        // 4. 데이터 타입 보정
        let summaryText = parsed.summary;
        if (Array.isArray(summaryText)) {
          summaryText = summaryText.join("\n");
        } else if (typeof summaryText !== 'string') {
           summaryText = String(summaryText || "");
        }

        let tagsArray = parsed.tags;
        if (!Array.isArray(tagsArray)) {
          tagsArray = [];
        }

        // 5. 결과 반환
        return {
          json: {
            summary: summaryText,
            tags: tagsArray,
            ...$input.item.json // 기존 YouTube 데이터 보존
          }
        };
        ```

### 5. Notion Create Page (페이지 생성)
- **Node Type**: Notion
- **Action**: Create a Database Page
- **Database**: `(선택한 데이터베이스)`
- **Mapping (Property Name : Expression/Value)**:
    - **제목 (Title)**: `{{ $('If Exists').item.json.snippet_1.title }}`
    - **URL**: `https://www.youtube.com/watch?v={{ $('If Exists').item.json.id_1.videoId }}`
    - **AI요약**: `{{ $json.summary }}` (Code 노드에서 정제된 값)
    - **기술태그 (Multi-select)**: `{{ $json.tags }}` (Code 노드에서 정제된 값)
    - **채널**: `{{ $('If Exists').item.json.snippet_1.channelTitle }}`
    - **등록일 (Date)**: `{{ $('If Exists').item.json.snippet_1.publishedAt }}`
    - **수집일 (Date)**: `{{ $now }}`
    - **자막여부**: `false`
    - **수집방식**: `n8n_Auto`
    - **연결**: `Parse LLM Response` → `Notion` → `Loop Over Items` (루프 순환)

<details>
<summary><b>(JSON) Notion Create Page - Simplified Schema</b></summary>

```json
{
  "parameters": {
    "resource": "databasePage",
    "operation": "create",
    "databaseId": {
      "__rl": true,
      "mode": "list",
      "value": "YOUR_DATABASE_ID"
    },
    "title": "={{ $('If Exists').item.json.snippet_1.title }}",
    "propertiesUi": {
      "propertyValues": [
        {
          "key": "URL",
          "value": "https://www.youtube.com/watch?v={{ $('If Exists').item.json.id_1.videoId }}",
          "type": "url"
        },
        {
          "key": "AI요약",
          "value": "={{ $json.summary }}",
          "type": "rich_text"
        },
        {
          "key": "기술태그",
          "value": "={{ $json.tags }}",
          "type": "multi_select"
        },
        {
          "key": "채널",
          "value": "={{ $('If Exists').item.json.snippet_1.channelTitle }}",
          "type": "rich_text"
        },
        {
          "key": "등록일",
          "value": "={{ $('If Exists').item.json.snippet_1.publishedAt }}",
          "type": "date"
        },
        {
          "key": "수집일",
          "value": "={{ $now }}",
          "type": "date"
        },
        {
          "key": "자막여부",
          "value": false,
          "type": "checkbox"
        }
      ]
    }
  },
  "name": "Notion Create Page",
  "type": "n8n-nodes-base.notion",
  "position": [
    1400,
    0
  ]
}
```
</details>

---

### ℹ️ 참고: Merge 노드 동작 원리 (데이터 무결성 보장)

이 워크플로우는 **Merge** 노드를 사용하여 YouTube 검색 결과와 Notion 중복 체크 결과를 **Merge By Fields** 방식으로 정확하게 결합합니다.

1.  **동작 방식**
    *   **Mode**: `Combine`
    *   **Merge By**: `Matching Fields`
        *   Input 1 (YouTube): `id.videoId`
        *   Input 2 (Notion): `property_video_id` (Notion에 저장된 Video ID)
    *   **Clash Handling**: `Add Suffix`
        *   중복되는 필드명(예: `id`, `snippet` 등)이 있을 경우, 자동으로 `_1` 등의 접미사가 붙습니다.
        *   이 때문에 후속 노드에서 `snippet_1`, `id_1`과 같은 변수명을 사용합니다.
2.  **연결**
    *   `YouTube Search` → Merge `Input 1`
    *   `Check Existence` → Merge `Input 2`
3.  **If 노드 조건 변경**
    *   중복 판단 기준을 Notion 데이터 유무로 변경합니다.
    *   Condition: `String` / Value 1: `{{ $json.id_2 }}` / Operator: `Is Not Empty`
    *   (Notion 데이터가 Merge 되었으므로 `id_2`가 존재하면 중복임)

