# 🤖 SOF.IA - Agente Inteligente para Participação Cidadã

## ✨ Visão Geral

A **SOF.IA** é uma agente inteligente intergeracional acessível via **WhatsApp**, projetada para modernizar a comunicação entre o cidadão e o poder público. [cite\_start]Utilizando automação (`N8N`) e Inteligência Artificial (LLMs), a SOF.IA transforma a reclamação comum em **dados estruturados de alta qualidade**, promovendo transparência e eficiência na gestão urbana e legislativa[cite: 3].

### 🎯 Missão e Valor

[cite\_start]A missão principal é transformar a **participação cidadã em impacto legislativo real**[cite: 3].

| Atributo | Descrição |
| :--- | :--- |
| **Público-Alvo** | Cidadãos de todas as gerações (intergeracionalidade). |
| **Principal Valor** | **Acessibilidade Universal** + **Filtragem de Dados de Qualidade**. |
| **Canal** | [cite\_start]**WhatsApp-First**[cite: 57]. |
| **KPIs de Impacto** | [cite\_start]Redução da burocracia e aumento da confiança no governo[cite: 12]. |

-----

## 💡 Funcionalidades-Chave

A SOF.IA integra módulos urbanos e legislativos, permitindo ao usuário:

  * [cite\_start]**Registro de Problemas Urbanos:** Enviar fotos e relatos de problemas (ex: buracos, lixo)[cite: 40, 93].
  * [cite\_start]**Consulta e Educação:** Receber explicações de leis e projetos em linguagem simples[cite: 43, 133].
  * [cite\_start]**Engajamento Legislativo:** Sugerir novas leis e avaliar projetos em votação[cite: 41, 42, 99].
  * [cite\_start]**Mapeamento:** Ocorrências são automaticamente georreferenciadas para visualização em Dashboard[cite: 45, 94].

-----

## 🏗️ Arquitetura Técnica (Workflow N8N)

O **N8N** é o orquestrador do fluxo, garantindo a resiliência e a coesão dos dados antes do processamento pela IA.

### 1\. Entrada e Triagem Inicial

O fluxo começa com a captura de dados e a triagem inicial do usuário:

  * **Webhook EVO (WhatsApp):** Recebe a mensagem e dados do usuário (`pushName`, `remoteJid`).
  * **Filtro/Lead:** Consulta o **PostgreSQL/Supabase** para verificar se o `lead` já existe, registrando um novo se necessário.

### 2\. Tratamento de Mídia e Contexto

O sistema prepara dados complexos (áudio e imagem) para a IA:

  * **Switch de Mídia:** Direciona a mensagem para processamento específico (`text`, `audioMessage`, `imageMessage`).
  * **Áudio/Imagens:** Áudios são transcritos (via HTTP Request/Groq) e Imagens são analisadas (`Analyze an image` - Gemini).
  * **Buffer (Redis):** O sistema de `push`, `Wait` e `junta_msgs` agrupa mensagens picotadas, criando um **contexto único** e coeso.

### 3\. Agente de IA e Decisão Estratégica

O agente de IA atua como um filtro de qualidade, usando o contexto completo do Buffer:

  * **AI Agent (LLM):** Utiliza um modelo da **OpenAI** com memória de conversa (`Chat` - PostgreSQL).
  * **Structured Output Parser:** Força a IA a retornar dados em **JSON** (eliminando ruído), permitindo que o N8N tome decisões automáticas.
  * **saída:** O JSON determina se a reclamação é válida para ser salva via `salvaBancoDados` ou se a SOF.IA deve fornecer educação legal.

-----

## 📂 Estrutura de Diretórios (Sugestão)

```
sof.ia/
├── .n8n/
│   └── main_workflow.json  # O arquivo JSON completo do workflow
├── assets/
│   └── logo.png
├── docs/
│   └── README.md           # Este arquivo
│   └── presentation.pdf    # Pitch de Apresentação
├── integrations/
│   └── tools/              # Ferramentas externas (Google Calendar, etc.)
│   └── prompt/             # Prompts de Sistema da Sof.IA (para fácil manutenção)
└── src/
    └── backend/            # Código de módulos adicionais (caso haja)
```

-----

## 🔒 Conformidade e Tecnologia

  * [cite\_start]**LGPD:** O pipeline é auditável, o armazenamento é seguro (PostgreSQL) e o consentimento é explícito via WhatsApp[cite: 105, 106, 107].
  * **Tecnologias:** N8N, Evolution API, OpenAI/LangChain, Redis, PostgreSQL/Supabase, Google Gemini/Groq.
