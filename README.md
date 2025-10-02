# Langchain Demo with Llama3.2

A simple AI assistant built with Streamlit and LangChain, using Ollama's Llama3.2 model for local AI inference.

## Features

- Interactive web interface using Streamlit
- Local AI inference with Ollama Llama3.2
- LangChain integration for prompt management
- Environment variable configuration

## Prerequisites

1. **Python 3.8+** installed on your system
2. **Ollama** installed and running locally
   - Download from: https://ollama.ai/
   - Install Llama3.2 model: `ollama pull llama3.2`

## Local Setup

1. **Install dependencies:**
   ```bash
   pip install -r requirements.txt
   ```

2. **Set up environment variables:**
   Create a `.env` file in the project root:
   ```
   LANGCHAIN_TRACING_V2=true
   LANGCHAIN_API_KEY=your_langchain_api_key_here
   ```

3. **Start Ollama service:**
   ```bash
   ollama serve
   ```

4. **Run the application:**
   ```bash
   streamlit run app.py
   ```

5. **Open your browser** and go to `http://localhost:8501`

## Deployment Options

### Option 1: Streamlit Cloud (Recommended for Streamlit apps)

**Note:** Streamlit Cloud doesn't support Ollama since it requires local model hosting. You'll need to modify the app to use a cloud-based API.

1. Push your code to GitHub
2. Go to [share.streamlit.io](https://share.streamlit.io)
3. Connect your GitHub repository
4. Add environment variables in the Streamlit Cloud dashboard

### Option 2: Railway

Railway supports Docker deployments and can host Ollama:

1. Create a `Dockerfile`:
   ```dockerfile
   FROM python:3.9-slim

   # Install Ollama
   RUN curl -fsSL https://ollama.ai/install.sh | sh

   WORKDIR /app
   COPY requirements.txt .
   RUN pip install -r requirements.txt

   COPY . .

   EXPOSE 8501

   CMD ["streamlit", "run", "app.py", "--server.port=8501", "--server.address=0.0.0.0"]
   ```

2. Deploy to Railway:
   - Connect your GitHub repository
   - Railway will automatically build and deploy

### Option 3: Local Server with Ngrok

For quick sharing without cloud deployment:

1. Install ngrok: https://ngrok.com/
2. Run your Streamlit app locally
3. In another terminal: `ngrok http 8501`
4. Share the ngrok URL

### Option 4: VPS/Cloud Server

Deploy on a VPS (DigitalOcean, AWS EC2, etc.):

1. Set up a Linux server
2. Install Python, pip, and Ollama
3. Clone your repository
4. Install dependencies and run the app
5. Use a reverse proxy (nginx) for production

## Cloud API Alternative

If you want to deploy on platforms like Vercel or Netlify, consider switching to a cloud-based API:

```python
# Replace Ollama with OpenAI
from langchain_openai import ChatOpenAI

llm = ChatOpenAI(
    model="gpt-3.5-turbo",
    openai_api_key=os.getenv("OPENAI_API_KEY")
)
```

## File Structure

```
├── app.py              # Main Streamlit application
├── requirements.txt    # Python dependencies
├── .env               # Environment variables (create this)
└── README.md          # This file
```

## Environment Variables

- `LANGCHAIN_TRACING_V2`: Enable LangChain tracing (optional)
- `LANGCHAIN_API_KEY`: Your LangChain API key for tracing (optional)

## Troubleshooting

1. **Ollama connection issues:**
   - Ensure Ollama is running: `ollama serve`
   - Check if Llama3.2 is installed: `ollama list`
   - Install if missing: `ollama pull llama3.2`

2. **Port already in use:**
   - Kill existing Streamlit processes
   - Or use a different port: `streamlit run app.py --server.port 8502`

3. **Dependencies issues:**
   - Update pip: `pip install --upgrade pip`
   - Install in virtual environment

## Performance Tips

- Ollama works best with sufficient RAM (8GB+ recommended)
- For better performance, use a GPU-enabled version of Ollama
- Consider using smaller models like `llama3.2:1b` for faster responses

## Security Notes

- Never commit your `.env` file to version control
- Use environment variables for sensitive information
- Consider rate limiting for production deployments

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Submit a pull request

## License

This project is open source and available under the MIT License.