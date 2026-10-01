# Travel Planner with LangGraph

A FastAPI travel-planning app that combines flight search, hotel research, and itinerary generation in a LangGraph workflow. The web interface is served from the same application.

## Features

- Searches live flight status data through AviationStack. Flight status data may not include ticket prices.
- Finds hotel suggestions with Tavily Search.
- Generates an itinerary and final trip summary with OpenAI (`gpt-4o-mini`).
- Persists LangGraph state and conversations using PostgreSQL checkpointing.
- Uses Delhi (`DEL`) as the default flight origin. Set `DEFAULT_ORIGIN_IATA` to another IATA airport code to change it.

## Requirements

- Python 3.14 or later
- PostgreSQL database
- `uv`
- An OpenAI API key and a Tavily API key

AviationStack is optional; without its key, flight search reports that live flight data is unavailable.

## Setup

Install dependencies:

```powershell
uv sync
```

Create a `.env` file in the project root with the required credentials and database URL:

```dotenv
OPENAI_API_KEY=your-openai-api-key
TAVILY_API_KEY=your-tavily-api-key
DATABASE_URL=postgresql://user:password@host:5432/database
AVIATIONSTACK_API_KEY=your-aviationstack-api-key
DEFAULT_ORIGIN_IATA=DEL
```

`AVIATIONSTACK_API_KEY` and `DEFAULT_ORIGIN_IATA` are optional. If the origin is omitted, the app defaults to `DEL`. The database must be reachable when the application starts. Do not commit `.env` or share its credentials.

## Run

```powershell
uv run app.py
```

Open [http://127.0.0.1:8000](http://127.0.0.1:8000) in a browser.

## API

`POST /api/travel` accepts a travel request and an optional thread ID:

```json
{
  "message": "Plan a 7-day trip to Japan from Delhi with hotels and sightseeing",
  "thread_id": null
}
```

The response includes the generated answer, thread ID, flight and hotel results, itinerary, and LLM call count. Reuse the returned `thread_id` to continue a conversation.
