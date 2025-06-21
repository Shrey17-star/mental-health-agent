# mental-health-agent
{
  "name": "My workflow",
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
      "type": "n8n-nodes-base.scheduleTrigger",
      "typeVersion": 1.2,
      "position": [
        -1200,
        40
      ],
      "id": "98e575cb-447b-46ac-9ced-f14185cea06c",
      "name": "Schedule Trigger"
    },
    {
      "parameters": {
        "chatId": "1931234330",
        "text": "Greetings from your well being buddy! How are you feeling right now?",
        "additionalFields": {}
      },
      "type": "n8n-nodes-base.telegram",
      "typeVersion": 1.2,
      "position": [
        -1000,
        40
      ],
      "id": "f7bb0b98-126b-4328-b29b-e58f58153e86",
      "name": "Telegram",
      "webhookId": "bb943788-026d-4767-baae-392c6d5609ff",
      "credentials": {
        "telegramApi": {
          "id": "0gLWy3xGjyP3quAD",
          "name": "Telegram account"
        }
      }
    },
    {
      "parameters": {
        "updates": [
          "message"
        ],
        "additionalFields": {}
      },
      "type": "n8n-nodes-base.telegramTrigger",
      "typeVersion": 1.2,
      "position": [
        -640,
        200
      ],
      "id": "92d8e8fa-469c-4141-bbd4-d7b03249657e",
      "name": "Telegram Trigger",
      "webhookId": "a632ab9a-0833-481c-9c6c-381c9b55bb15",
      "credentials": {
        "telegramApi": {
          "id": "0gLWy3xGjyP3quAD",
          "name": "Telegram account"
        }
      }
    },
    {
      "parameters": {
        "model": "mindcraft-gpt4o",
        "options": {}
      },
      "type": "@n8n/n8n-nodes-langchain.lmChatAzureOpenAi",
      "typeVersion": 1,
      "position": [
        -320,
        320
      ],
      "id": "0e136fbf-b16f-481a-8412-e4c765f40df9",
      "name": "Azure OpenAI Chat Model",
      "credentials": {
        "azureOpenAiApi": {
          "id": "WcNtvDyqThVLTmKs",
          "name": "Azure Open AI account"
        }
      }
    },
    {
      "parameters": {
        "chatId": "1931234330",
        "text": "={{ $json.output }}",
        "additionalFields": {}
      },
      "type": "n8n-nodes-base.telegram",
      "typeVersion": 1.2,
      "position": [
        100,
        100
      ],
      "id": "9af7c5e2-9dd7-4a1c-8874-9928ee101642",
      "name": "Telegram1",
      "webhookId": "b53b014d-8c59-4f8c-b3b5-11348b15e0b1",
      "credentials": {
        "telegramApi": {
          "id": "0gLWy3xGjyP3quAD",
          "name": "Telegram account"
        }
      }
    },
    {
      "parameters": {
        "promptType": "define",
        "text": "=You are a friendly AI mental health companion. Analyze the user's emotional state and suggest a small, positive action.\nThe user said:\n {{ $json.message.text }}",
        "options": {}
      },
      "type": "@n8n/n8n-nodes-langchain.agent",
      "typeVersion": 2,
      "position": [
        -320,
        60
      ],
      "id": "e06eb590-8bce-4726-b912-18ea7eda996a",
      "name": "AI Agent"
    },
    {
      "parameters": {
        "sessionIdType": "customKey",
        "sessionKey": "=1931234330",
        "contextWindowLength": 10
      },
      "type": "@n8n/n8n-nodes-langchain.memoryBufferWindow",
      "typeVersion": 1.3,
      "position": [
        -180,
        320
      ],
      "id": "60b999a1-7c88-4879-a826-6888a7918bbe",
      "name": "Simple Memory"
    },
    {
      "parameters": {
        "options": {}
      },
      "type": "@n8n/n8n-nodes-langchain.toolSerpApi",
      "typeVersion": 1,
      "position": [
        -60,
        280
      ],
      "id": "38a305c3-5820-46b0-b145-8b7c1bdf47e7",
      "name": "SerpAPI",
      "credentials": {
        "serpApi": {
          "id": "Qm5veXuM5M3iaYK2",
          "name": "SerpAPI account"
        }
      }
    }
  ],
  "pinData": {},
  "connections": {
    "Schedule Trigger": {
      "main": [
        [
          {
            "node": "Telegram",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Telegram Trigger": {
      "main": [
        [
          {
            "node": "AI Agent",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Azure OpenAI Chat Model": {
      "ai_languageModel": [
        [
          {
            "node": "AI Agent",
            "type": "ai_languageModel",
            "index": 0
          }
        ]
      ]
    },
    "Simple Memory": {
      "ai_memory": [
        [
          {
            "node": "AI Agent",
            "type": "ai_memory",
            "index": 0
          }
        ]
      ]
    },
    "AI Agent": {
      "main": [
        [
          {
            "node": "Telegram1",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Telegram1": {
      "main": [
        []
      ]
    },
    "Telegram": {
      "main": [
        [
          {
            "node": "AI Agent",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "SerpAPI": {
      "ai_tool": [
        [
          {
            "node": "AI Agent",
            "type": "ai_tool",
            "index": 0
          }
        ]
      ]
    }
  },
  "active": false,
  "settings": {
    "executionOrder": "v1"
  },
  "versionId": "b9973b08-2948-4816-ba36-e417f439a181",
  "meta": {
    "templateCredsSetupCompleted": true,
    "instanceId": "3a482d0eeb87b995a2ef543c57da4228d26e3faa2b886ac44aa689623e7553f8"
  },
  "id": "fXTOSdDs58TFRpQR",
  "tags": []
}
