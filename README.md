## GoodFoods Reservation Assistant

GoodFoods is an AI-powered restaurant discovery and table-reservation demo built with Streamlit, FastAPI, and Groq function calling.

The chat assistant helps users find restaurants by area, cuisine, operating hours, and capacity, then creates a reservation using the selected restaurant's details.


## Features

- Conversational restaurant search and recommendations
- Restaurant filters for location, cuisine, hours, and seating capacity
- Table-reservation flow with basic validation
- Live display of agent tool activity
- FastAPI backend and Streamlit frontend
- JSON-based demo catalogue and reservation storage

## Tech Stack

- Python 3.9+
- Streamlit
- FastAPI and Uvicorn
- Groq API
- Pydantic

## Project Structure

```text
.
├── agent/
│   ├── conversation_engine.py  # Groq calls and backend tool requests
│   ├── prompt_library.py       # Assistant instructions and examples
│   └── toolkit.py              # Tool schemas exposed to the AI
├── data/
│   ├── service_api.py          # Search and reservation API
│   ├── restaurant_list.json    # Restaurant catalogue
│   └── bookings_list.json      # Demo reservation records
├── app_goodfoods.py            # Streamlit chat application
├── start.py                    # Starts API and UI locally
└── requirements.txt
```

## Run Locally

### 1. Clone the repository

```bash
git clone https://github.com/YOUR-USERNAME/YOUR-REPOSITORY.git
cd YOUR-REPOSITORY
```

### 2. Create and activate a virtual environment

**Windows PowerShell**

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
```

**macOS / Linux**

```bash
python3 -m venv .venv
source .venv/bin/activate
```

### 3. Install dependencies

```bash
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
```

### 4. Add your Groq API key

Create a `.env` file in the project root:

```env
GROQ_API_KEY=your_groq_api_key_here
```

Never commit `.env` or your API key to GitHub.

### 5. Start the application

```bash
python start.py
```

Open the Streamlit URL shown in the terminal, normally [http://localhost:8501](http://localhost:8501). API documentation is available at [http://localhost:8000/docs](http://localhost:8000/docs).

## Customize for any other location restaurant

### 1. Replace the restaurant catalogue

Edit `data/restaurant_list.json`. Each restaurant needs the same fields as this example:

```json
{
  "restaurant_id": "d001",
  "name": "GoodFoods Rajpur Road",
  "location": {
    "address": "Your real street address, Dehradun",
    "landmark": "A useful nearby landmark"
  },
  "cuisine": ["North Indian", "Chinese"],
  "operating_hours": {
    "open": "11:00",
    "close": "23:00",
    "display": "11:00 AM - 11:00 PM"
  },
  "phone": "YOUR_REAL_PHONE_NUMBER",
  "restaurant_max_seating_capacity": 60,
  "max_booking_party_size": 8,
  "operating_days": ["Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday", "Sunday"]
}
```

Use only restaurant information that you have verified and are permitted to publish. Make every `restaurant_id` unique.

### 2. Update city names and AI examples

Update these files so the visible text and the AI's instructions match the new catalogue:

| File | Update |
| --- | --- |
| `app_goodfoods.py` | Change the heading, subtitle, and welcome message to Dehradun. |
| `agent/prompt_library.py` | Replace Bangalore details and examples with your Dehradun restaurants. |
| `agent/toolkit.py` | Replace location examples with real Dehradun areas. |
| `data/restaurant_list.json` | Replace all sample Bangalore restaurants with your verified data. |

Validate the JSON after editing:

```bash
python -c "import json; json.load(open('data/restaurant_list.json', encoding='utf-8')); print('JSON is valid')"
```

## Deploy

Deploy the API and UI as two services on a Python-compatible platform such as Railway or Render.

### API service

```text
Build command: pip install -r requirements.txt
Start command: uvicorn data.service_api:app --host 0.0.0.0 --port $PORT
```

### Streamlit UI service

```text
Build command: pip install -r requirements.txt
Start command: streamlit run app_goodfoods.py --server.address 0.0.0.0 --server.port $PORT
```

Set these environment variables in the UI service settings:

```text
GROQ_API_KEY=your_groq_api_key_here
RESTAURANT_API_URL=https://your-api-service-url
```

`RESTAURANT_API_URL` defaults to `http://localhost:8000` during local development.

## Important Limitations

- Reservations are stored in `data/bookings_list.json`, which is suitable only for local testing. Many hosts erase this file during restarts or deployments.
- Move reservations to a database such as PostgreSQL before using this with real customers.
- This is a demo, not a production payment or reservation system. Add authentication, rate limiting, validation, backups, and privacy controls before public use.

## License

Add a license file before making this repository public.
