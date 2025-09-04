# Docker Setup Guide for AgentGraph

## Prerequisites

1. **Download Required Databases:**

   - Download Chinook database from: [Sample Database](https://database.guide/2-sample-databases-sqlite/)
   - Download Travel database from: [Kaggle Link](https://www.kaggle.com/code/mpwolke/airlines-sqlite)
   - Place both files in the `data/` directory

2. **Environment Variables:**
   - Copy `env_example` to `.env`
   - Fill in your API keys:
     ```
     OPEN_AI_API_KEY=your_openai_key_here
     TAVILY_API_KEY=your_tavily_key_here
     LANGCHAIN_API_KEY=your_langchain_key_here
     ```

## Building and Running

### Option 1: Using Docker Compose (Recommended)

```bash
# Build and run
docker-compose up --build

# Run in background
docker-compose up -d --build

# View logs
docker-compose logs -f

# Stop
docker-compose down
```

### Option 2: Using Docker directly

```bash
# Build the image
docker build -t agentgraph .

# Run the container
docker run -p 7860:7860 \
  -e OPEN_AI_API_KEY=your_key \
  -e TAVILY_API_KEY=your_key \
  -e LANGCHAIN_API_KEY=your_key \
  -v $(pwd)/data:/app/data \
  -v $(pwd)/memory:/app/memory \
  agentgraph
```

## Important Notes

1. **Database Files**: Make sure to download and place the required database files in the `data/` directory before building
2. **Vector Databases**: The vector databases will be created automatically when you first run the application
3. **Memory Persistence**: Chat history is saved to the `memory/` directory
4. **Port**: The application runs on port 7860 (Gradio default)

## Troubleshooting

1. **Missing Database Files**: Ensure `Chinook.db` and `travel.sqlite` are in the `data/` directory
2. **API Key Issues**: Verify all environment variables are set correctly
3. **Port Conflicts**: Change the port mapping if 7860 is already in use
4. **Memory Issues**: Increase Docker memory allocation if the container fails to start

## Health Check

The container includes a health check that verifies the application is responding on port 7860. You can check the health status with:

```bash
docker ps
```

Look for the "healthy" status in the STATUS column.
