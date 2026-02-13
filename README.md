# Chatbot de Clima no Telegram com n8n

Este projeto implementa um chatbot de clima no Telegram, desenvolvido no n8n, que consulta a API **OpenWeather** para retornar a temperatura atual de uma cidade brasileira informada pelo usuário.

O bot valida o formato da entrada, consulta a API e responde automaticamente no Telegram com uma mensagem personalizada via **Google Gemini** ou através de um sistema de **fallback** determinístico.

## 📌 Funcionalidades

* **Integração com Telegram Bot**: Recebimento e envio de mensagens em tempo real.
* **Consulta de clima via OpenWeather**: Dados precisos de temperatura e condições climáticas.
* **Melhoria de saída com IA**: Uso do Google Gemini (model 2.5-flash) com temperatura 0.1 para reescrita natural das mensagens.
* **Sistema de Fallback (Obrigatório)**: Garantia de resposta determinística formatada pelo nó `Format Sucesso` caso a API de IA falhe ou não possua credenciais.
* **Tratamento de Erros**: Mensagem amigável para cidades não encontradas ou formatos inválidos.

## 🏗️ Estrutura do Workflow

O fluxo principal segue estas etapas:

1. **Telegram Trigger**: Inicia o fluxo ao receber uma mensagem.
2. **Edit Fields**: Normaliza o texto e cria a variável `queue`.
3. **HTTP Request**: Consulta a OpenWeather usando `units: metric` e `lang: pt_br`.
4. **If**: Valida o sucesso da requisição (Status 200).
5. **Format Sucesso**: Arredonda a temperatura via `Math.round()` e gera a `fallback_message`.
6. **Preparar Mensagem (IA)**: Refina o texto final (opcional).
7. **Telegram Send**: Entrega a resposta final utilizando a lógica de fallback.

## 🚀 Como importar o workflow no n8n

1. Acesse o painel do seu n8n.
2. Vá em **Workflows** -> **Import from file**.
3. Selecione o arquivo `workflow-telegram-chatbot.json`.
4. Salve e ative o workflow.

## 🔐 Configuração das credenciais

### 1. Telegram Bot

* Crie um bot via [@BotFather](https://t.me/botfather) e obtenha o `TELEGRAM_BOT_TOKEN`.
* No n8n, configure a credencial **Telegram API** com o nome `Telegram account`.

### 2. OpenWeather API

* Obtenha sua chave no site oficial da OpenWeather.
* No n8n, crie uma credencial **HTTP Query Auth** chamada `OpenWeather API` e insira o parâmetro `appid` com sua chave.

### 3. Google Gemini (Opcional)

* Gere sua chave no Google AI Studio.
* Configure a credencial **Google Gemini(PaLM) Api** no n8n.
* **Nota**: O sistema de fallback garante que o bot funcione mesmo sem esta chave.

## 📊 Variáveis esperadas

| Variável | Descrição |
| --- | --- |
| `OPENWEATHER_API_KEY` | Chave da API OpenWeather |
| `TELEGRAM_BOT_TOKEN` | Token do bot do Telegram |

## 🛠️ Como usar o chatbot

No Telegram, envie o nome da cidade:

* **Exemplo**: `Curitiba, PR` ou `São Paulo, SP`.
* **Resposta de sucesso**: "🌤️ A temperatura em Curitiba é de 24°C com céu limpo."
* **Resposta de erro**: "❌ Cidade não encontrada. Use o formato Cidade,UF."