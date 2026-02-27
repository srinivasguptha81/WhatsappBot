# 🤖 WhatsApp AI Agent Bot

A WhatsApp AI agent built with **Node.js**, **venom-bot**, and **n8n** — capable of receiving WhatsApp messages and responding intelligently using an AI language model via **OpenRouter**.

---

## 🧠 How It Works

```
WhatsApp Message
      ↓
  venom-bot (receives message)
      ↓
  Express Server (bot.js)
      ↓
  n8n Workflow (AI processing)
      ↓
  OpenRouter LLM API (generates response)
      ↓
  venom-bot (sends reply back)
      ↓
WhatsApp Reply
```

1. **venom-bot** connects to WhatsApp Web and listens for incoming messages
2. **Express** runs a local HTTP server that bridges the bot and n8n
3. **n8n** orchestrates the AI workflow — routing messages to the LLM and back
4. **OpenRouter** provides access to LLM models to generate intelligent replies
5. The response is sent back to the user on WhatsApp

---

## 🛠️ Tech Stack

| Tool | Purpose |
|------|---------|
| Node.js | Runtime environment |
| venom-bot | WhatsApp Web automation |
| Express | Local HTTP server |
| Axios | HTTP requests between bot and n8n |
| n8n | Visual AI workflow automation (self-hosted) |
| OpenRouter | LLM API (AI responses) |
| Docker | Running n8n locally |

---

## 📁 Project Structure

```
WhatsappBot/
├── bot.js               # Main bot script (venom-bot + Express server)
├── AI_Bot.json          # n8n workflow file (import this into n8n)
├── package.json         # Node.js dependencies
├── package-lock.json    # Lockfile
├── tokens/              # venom-bot session tokens (auto-generated)
├── node_modules/        # Installed packages
└── .gitignore
```

---

## ⚙️ Prerequisites

Before getting started, make sure you have the following installed:

- [Node.js](https://nodejs.org/) (includes npm)
- [Docker](https://www.docker.com/)
- [Visual Studio Code](https://code.visualstudio.com/) *(recommended)*
- An [OpenRouter](https://openrouter.ai/) account and API key

---

## 🚀 Setup Instructions

### 1. Clone the Repository

```bash
git clone https://github.com/srinivasguptha81/WhatsappBot.git
cd WhatsappBot
```

---

### 2. Install Dependencies

```bash
npm install express venom-bot axios
```

---

### 3. Set Up n8n with Docker

Pull and run n8n locally using Docker:

```bash
docker pull n8nio/n8n

docker run -it --rm \
  -p 5678:5678 \
  -v ~/.n8n:/home/node/.n8n \
  n8nio/n8n
```

To manage n8n later:

```bash
docker start n8n
docker stop n8n
```

Once running, open n8n in your browser at:

👉 **http://localhost:5678**

---

### 4. Import the AI Workflow into n8n

1. Open n8n at `http://localhost:5678`
2. Go to **Workflows → Import**
3. Upload the `AI_Bot.json` file from this repository
4. The full AI agent workflow will be loaded

---

### 5. Configure OpenRouter API Key

1. Sign up at [openrouter.ai](https://openrouter.ai/) and get your API key
2. In n8n, go to **Credentials** and add your OpenRouter API key
3. Link the credential to the AI node inside the imported workflow

---

### 6. Run the Bot

```bash
node bot.js
```

On first run, venom-bot will display a **QR code** in the terminal. Scan it with WhatsApp on your phone to authenticate.

---

### 7. Test the Bot

You can simulate a message using `curl`:

```bash
curl -X POST http://localhost:3000/simulate-self \
  -H "Content-Type: application/json" \
  -d '{"body": "Hi", "from": "your-phone-number@c.us"}'
```

Replace `your-phone-number` with your actual number in international format (e.g., `919876543210@c.us`).

---

## ✅ Expected Behavior

Once everything is running:

- Send a WhatsApp message to the authenticated number
- The bot receives the message via venom-bot
- It forwards the message to n8n for AI processing
- OpenRouter generates a response
- The bot sends the reply back to you on WhatsApp

---

## ⚠️ Important Notes

- **Keep `tokens/` out of version control** — it stores your WhatsApp session. The `.gitignore` handles this.
- **Do not share your OpenRouter API key** publicly.
- venom-bot uses WhatsApp Web — your phone must stay connected to the internet.
- This project is intended for **personal/educational use**. Automating WhatsApp at scale may violate WhatsApp's Terms of Service.

---

## 📦 Dependencies

```json
{
  "express": "HTTP server",
  "venom-bot": "WhatsApp Web client",
  "axios": "HTTP requests to n8n"
}
```
