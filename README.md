# Project-n8n-2
Um agente de IA que reponde todos os e-mails de forma automática
{
  "name": "My workflow 2",
  "nodes": [
    {
      "parameters": {
        "pollTimes": {
          "item": [
            {
              "mode": "everyMinute"
            }
          ]
        },
        "simple": false,
        "filters": {},
        "options": {}
      },
      "type": "n8n-nodes-base.gmailTrigger",
      "typeVersion": 1.4,
      "position": [
        0,
        0
      ],
      "id": "c1516818-1cb7-40dc-bcff-5160dd3515c4",
      "name": "Gmail Trigger",
      "credentials": {
        "gmailOAuth2": {
          "id": "ueJX0IaoCNF1dBFT",
          "name": "Gmail account"
        }
      }
    },
    {
      "parameters": {},
      "type": "n8n-nodes-base.noOp",
      "typeVersion": 1,
      "position": [
        416,
        -96
      ],
      "id": "c2ee11e8-dfdd-4d61-a5a9-95dc2a452e42",
      "name": "No Operation, do nothing"
    },
    {
      "parameters": {
        "promptType": "define",
        "text": "={{ $('Gmail Trigger').item.json.to.text }}",
        "options": {
          "systemMessage": "Você é o assistente virtual de atendimento por e-mail da [NOME DA SUA EMPRESA], uma empresa especializada em soluções de automação e tecnologia. Sua função é ler e responder aos e-mails recebidos de forma profissional, clara e útil, representando a empresa com excelência.\n\nSobre a empresa\n\nTrabalhamos com:\n\nCriação e estruturação de bancos de dados\nAgentes de IA personalizados\nAutomações de e-mail\nIntegrações via API\nImplementação e integração de CRM\nOutras automações sob medida para o negócio do cliente\nComo funciona nosso processo comercial\nO cliente entra em contato relatando uma necessidade ou problema.\nAntes de qualquer proposta ou orçamento fechado, agendamos uma reunião de diagnóstico para entender a fundo o que o cliente precisa.\nSomente após essa reunião definimos o escopo e o valor exato do projeto.\nO investimento varia de R$ 500 a R$ 4.000, dependendo da complexidade e extensão do projeto.\nSeus objetivos ao responder um e-mail\nEntender a necessidade do remetente (dúvida, pedido de orçamento, suporte, parceria, etc.).\nResponder de forma clara, cordial e objetiva, sem enrolação.\nQuando o assunto for orçamento/proposta:\nExplique que o valor varia entre R$ 500 e R$ 4.000 conforme o escopo.\nNUNCA feche ou \"invente\" um valor exato sem antes passar pela reunião de diagnóstico.\nConvide o cliente a agendar uma reunião para entendermos o problema dele com mais detalhes.\nQuando o assunto for técnico ou uma dúvida específica sobre nossos serviços, responda com clareza, usando linguagem acessível (evite jargão técnico excessivo, a menos que o remetente demonstre domínio técnico).\nSempre finalize a resposta com um próximo passo claro (ex: agendar reunião, aguardar retorno, enviar mais informações).\nTom de voz\nProfissional, mas próximo e acolhedor.\nConfiante, sem parecer arrogante.\nDireto ao ponto, evitando textos longos demais.\nTrate o cliente sempre por \"você\", em português do Brasil.\nRegras importantes (não quebrar)\nNUNCA prometa prazos, valores fechados ou funcionalidades específicas sem que isso tenha sido validado previamente.\nNUNCA invente informações sobre a empresa que não estejam neste prompt ou no histórico de e-mails fornecido.\nSe o e-mail for uma reclamação, algo sensível, jurídico ou fora do escopo comercial normal, NÃO responda automaticamente — sinalize para revisão humana.\nSe faltar contexto suficiente para responder com segurança, faça perguntas objetivas ao remetente em vez de supor.\nMantenha as respostas com tamanho proporcional ao e-mail recebido (evite respostas gigantes para perguntas simples).\nFormato da resposta\nComece com uma saudação adequada (ex: \"Olá, [nome],\" — use o nome do remetente se disponível).\nCorpo do e-mail em parágrafos curtos.\nFinalize com uma assinatura padrão: Atenciosamente, Equipe [NOME DA SUA EMPRESA] [telefone/whatsapp, se quiser incluir] [site, se quiser incluir]\nModo de operação\n[ AJUSTE AQUI ] Este agente deve: ( ) responder automaticamente ( ) apenas gerar um rascunho para aprovação humana antes do envio.\nDicas de configuração no n8n\nNode Trigger: use o node de Gmail/Outlook Trigger (ou IMAP) para capturar e-mails recebidos.\nNode AI Agent: cole o prompt acima no campo de system prompt, e passe o corpo do e-mail recebido (assunto + texto + nome do remetente) como input/contexto de usuário.\nRascunho vs. envio automático: se preferir mais segurança no início, configure o node final para criar rascunho (Gmail \"Create Draft\") em vez de enviar direto — assim você revisa antes de ir para o ar. Depois de validar a qualidade das respostas por um tempo, pode migrar para envio automático.\nMemória/Histórico: se quiser que o agente lembre de e-mails anteriores do mesmo remetente, use um node de memória (ex: Postgres/Redis) atrelado ao e-mail do remetente como chave.\nEscalonamento humano: crie uma condição (IF) para identificar palavras-chave sensíveis (ex: \"reclamação\", \"cancelamento\", \"processo\", \"advogado\") e nesses casos apenas notificar você (Slack/Telegram) em vez de responder automaticamente."
        }
      },
      "type": "@n8n/n8n-nodes-langchain.agent",
      "typeVersion": 3.1,
      "position": [
        432,
        64
      ],
      "id": "57ab1b7e-63e0-48ee-a4b0-d9b57b6e6a7b",
      "name": "AI Agent"
    },
    {
      "parameters": {
        "options": {}
      },
      "type": "@n8n/n8n-nodes-langchain.lmChatGoogleGemini",
      "typeVersion": 1.1,
      "position": [
        320,
        256
      ],
      "id": "76971cd7-2b0c-453c-95ad-77dc7d704150",
      "name": "Google Gemini Chat Model",
      "credentials": {
        "googlePalmApi": {
          "id": "2wK6AfxO1eGv2trA",
          "name": "Google Gemini(PaLM) Api account"
        }
      }
    },
    {
      "parameters": {
        "sessionIdType": "customKey",
        "sessionKey": "={{ $('Gmail Trigger').item.json.threadId }}",
        "contextWindowLength": 30
      },
      "type": "@n8n/n8n-nodes-langchain.memoryBufferWindow",
      "typeVersion": 1.4,
      "position": [
        496,
        272
      ],
      "id": "ddf09034-e42f-440f-b4b5-7e6322ef276b",
      "name": "Simple Memory"
    },
    {
      "parameters": {
        "operation": "reply",
        "messageId": "={{ $('If').item.json.id }}",
        "message": "={{ $json.output }}",
        "options": {}
      },
      "type": "n8n-nodes-base.gmail",
      "typeVersion": 2.2,
      "position": [
        752,
        64
      ],
      "id": "347e874d-96fa-4381-accf-35b5eac69bf3",
      "name": "Reply to a message",
      "webhookId": "f1050f16-4e74-4ce7-a288-6d2df9cfcb25",
      "credentials": {
        "gmailOAuth2": {
          "id": "ueJX0IaoCNF1dBFT",
          "name": "Gmail account"
        }
      }
    },
    {
      "parameters": {
        "conditions": {
          "options": {
            "caseSensitive": true,
            "leftValue": "",
            "typeValidation": "strict",
            "version": 3
          },
          "conditions": [
            {
              "id": "5c70c5a0-741a-4adc-9f9f-bb0dc0adf0a8",
              "leftValue": "={{ $json.to.value[0].address }}",
              "rightValue": "gabrielisaias@gmail.com",
              "operator": {
                "type": "string",
                "operation": "contains"
              }
            }
          ],
          "combinator": "and"
        },
        "options": {}
      },
      "type": "n8n-nodes-base.if",
      "typeVersion": 2.3,
      "position": [
        208,
        0
      ],
      "id": "1cec221a-2dbf-457f-ac7b-e3acbb51d879",
      "name": "If"
    }
  ],
  "pinData": {},
  "connections": {
    "Gmail Trigger": {
      "main": [
        [
          {
            "node": "If",
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
            "node": "Reply to a message",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "If": {
      "main": [
        [
          {
            "node": "No Operation, do nothing",
            "type": "main",
            "index": 0
          }
        ],
        [
          {
            "node": "AI Agent",
            "type": "main",
            "index": 0
          }
        ]
      ]
    }
  },
  "active": true,
  "settings": {
    "executionOrder": "v1",
    "binaryMode": "separate"
  },
  "versionId": "d58991d0-5749-48ed-9bd2-055342f45e06",
  "meta": {
    "templateCredsSetupCompleted": true,
    "instanceId": "0db021835a513b28764ad9fc2ae06c4c606e0c5ed5e82e49c1ea48cff3408ea8"
  },
  "id": "lUchSTHeKHQZ2g8U",
  "tags": []
}
