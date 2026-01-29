# 🌤️ Bot Clima BR (Telegram + n8n)

Bot de Telegram simples que retorna a **temperatura atual de cidades brasileiras** usando a API do **OpenWeatherMap**, orquestrado via **n8n**.

O usuário envia uma mensagem no formato `Cidade,UF` (ex.: `São Paulo,SP`) e o bot responde com a temperatura atual e, quando relevante, a sensação térmica.

---

## ✨ Funcionalidades

* Integração com Telegram (mensagens privadas)
* Consulta em tempo real ao OpenWeatherMap
* Respostas em português (pt-BR)
* Unidade de temperatura em Celsius
* Tratamento de erros:
  * Cidade não encontrada
  * Erros inesperados da API

---

## 🧩 Arquitetura do Workflow (n8n)

Fluxo resumido:

1. **Telegram Trigger**

   * Escuta mensagens recebidas no bot

2. **Ignore Bots Response (IF)**

   * Garante que apenas mensagens de humanos sejam processadas

3. **Edit Fields (Set)**

   * Extrai o texto da mensagem e armazena na variável `queue`

4. **HTTP Request (OpenWeatherMap)**

   * Consulta o clima da cidade informada

5. **IF (Tratamento de erro)**

   * Se status `404` → cidade não encontrada
   * Caso contrário → erro inesperado

6. **Telegram Response**

   * Retorna a temperatura da cidade ou mensagens de erro

---

## 📦 Pré-requisitos

Antes de importar o workflow, você precisa:

* Uma instância do **n8n** funcionando
* Um **bot criado no Telegram** (via @BotFather) e a **Token** gerado
* Uma **API Key do OpenWeatherMap**

---

## 🚀 Importando o Workflow no n8n

1. Copie o JSON do workflow
2. Acesse o n8n
3. Clique em **Import Workflow**
4. Cole o JSON e confirme

Após a importação, o workflow aparecerá como **Bot Clima BR**.

⚠️ Importante: o workflow vem como **inactive** por padrão.

---

## 🔐 Configuração de Credenciais

### 1️⃣ Telegram

Crie um bot no Telegram usando o **@BotFather** e obtenha o **Token**.

No n8n:

1. Vá em **Credentials** → **New**
2. Selecione **Telegram API**
3. Preencha:

   * **Access Token**: token fornecido pelo BotFather
4. Salve a credencial

Depois disso, garanta que essa credencial está associada aos nós:

* `Telegram Trigger`
* `Temperatura da cidade`
* `Cidade não encontrada`
* `Erro inesperado`

---

### 2️⃣ OpenWeatherMap

Crie uma conta em **OpenWeatherMap** e gere sua **API Key**.

No n8n:

1. Vá em **Credentials** → **New**
2. No campo **Authentication** selecione **Generic Credential Type**
3. Em **Generic Auth Type** selecione **HTTP Query Auth**
4. Configure:
   * **Name**: `appid`
   * **Value**: sua API Key do OpenWeather
5. Salve

Essa credencial será usada no nó:

* `HTTP Request`

---

## 🔧 Variáveis e Parâmetros Esperados

### Entrada do Usuário (Telegram)

Formato esperado:

```
Cidade,UF
```

Exemplos válidos:

* `São José dos Campos,SP`
* `Rio de Janeiro,RJ`
* `Curitiba,PR`

## 💬 Mensagens de Resposta

### ✅ Sucesso

```
A temperatura atual em São José dos Campos é 23.3 ˚C.
```

Caso a diferença entre temperatura real e sensação térmica seja maior que 5˚C:

```
Mas a sensação térmica é de 18.1 ˚C
```

---

### ❌ Cidade não encontrada (404)

```
Não encontramos a cidade que você digitou ☹️
Use o formato Cidade,UF (ex.: São Paulo,SP). 💪
```

---

### ⚠️ Erro inesperado

```
⚠️ Um erro inesperado aconteceu! Contate nosso suporte ⚠️
```

---

## ▶️ Ativando o Bot

Após configurar as credenciais:

1. Ative o workflow no n8n
2. Abra o bot no Telegram
3. Envie uma mensagem com o nome da cidade
