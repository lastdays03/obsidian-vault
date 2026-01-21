---
tags: [n8n, workflow, youtube, automation, troubleshooting]
Up: [[n8n_Automation_Use_Cases_2026]]
---

# n8n YouTube Search Workflow (Multi-Keyword)

여러 개의 키워드를 입력받아, 각각에 대해 YouTube 영상을 검색하고 결과를 합쳐서 반환하는 워크플로우입니다.
n8n의 **자동 반복(Looping)** 특성을 활용합니다.

## 📋 Prerequisites (준비사항)

1. **Google Cloud Console**: `YouTube Data API v3` 활성화.
2. **n8n Credential**: `Google API Console`에서 OAuth 클라이언트 ID 생성 및 n8n 연결.

## 🔄 Workflow Logic

1. **When clicking 'Execute Workflow'**: 수동 실행.
2. **Define Keywords (Code Node)**: 검색할 키워드 목록을 JavaScript 배열로 정의합니다. n8n은 이 배열의 각 항목(Item)에 대해 다음 노드를 자동으로 반복 실행합니다.
   - 예: `['python', 'javascript', 'rust']` → 3번 실행됨.
3. **YouTube**: 각 키워드별로 검색 수행 (Video, getAll).
4. **Simply Data**: 필요한 데이터만 정제.

## 💻 Workflow JSON

아래 코드를 복사하여 n8n 캔버스에 붙여넣으세요. 기존 노드는 삭제하고 붙여넣는 것이 좋습니다.

```json
{
  "name": "YouTube Multi-Keyword Search",
  "nodes": [
    {
      "parameters": {},
      "id": "trigger",
      "name": "When clicking 'Execute Workflow'",
      "type": "n8n-nodes-base.manualTrigger",
      "typeVersion": 1,
      "position": [250, 300]
    },
    {
      "parameters": {
        "jsCode": "return [\n  { json: { keyword: 'n8n automation' } },\n  { json: { keyword: 'claude ai coding' } },\n  { json: { keyword: 'obsidian workflows' } }\n];"
      },
      "id": "define-keywords",
      "name": "Define Keywords",
      "type": "n8n-nodes-base.code",
      "typeVersion": 2,
      "position": [450, 300]
    },
    {
      "parameters": {
        "resource": "video",
        "operation": "getAll",
        "limit": 5,
        "filters": {
          "q": "={{ $json.keyword }}"
        },
        "options": {
          "part": [
            "snippet",
            "id"
          ]
        }
      },
      "id": "youtube",
      "name": "YouTube",
      "type": "n8n-nodes-base.youTube",
      "typeVersion": 1,
      "position": [650, 300],
      "credentials": {
        "youTubeOAuth2Api": {
          "id": "YOUR_CREDENTIAL_ID",
          "name": "YouTube account"
        }
      }
    },
    {
      "parameters": {
        "keepOnlySet": true,
        "values": {
          "string": [
            {
              "name": "Title",
              "value": "={{ $json.snippet.title }}"
            },
            {
              "name": "Video URL",
              "value": "={{ 'https://www.youtube.com/watch?v=' + $json.id.videoId }}"
            },
            {
              "name": "Search Keyword",
              "value": "={{ $('Define Keywords').item.json.keyword }}"
            },
            {
              "name": "Channel",
              "value": "={{ $json.snippet.channelTitle }}"
            }
          ]
        },
        "options": {}
      },
      "id": "simplify",
      "name": "Simply Data",
      "type": "n8n-nodes-base.set",
      "typeVersion": 2,
      "position": [850, 300]
    }
  ],
  "connections": {
    "When clicking 'Execute Workflow'": {
      "main": [
        [
          {
            "node": "Define Keywords",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Define Keywords": {
      "main": [
        [
          {
            "node": "YouTube",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "YouTube": {
      "main": [
        [
          {
            "node": "Simply Data",
            "type": "main",
            "index": 0
          }
        ]
      ]
    }
  }
}
```

### 💡 Tip
- **Define Keywords** 노드를 더블 클릭하여 `return [...]` 안의 키워드 목록을 자유롭게 수정하세요.
- 결과에는 `Search Keyword` 필드가 추가되어, 어떤 검색어의 결과인지 구분할 수 있습니다.
