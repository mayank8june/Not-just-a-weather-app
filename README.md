# System Design: Not-Just-a-Weather-App

## Overview
"Not-just-a-weather-app" is a web application that integrates real-time weather data with Spotify playlists. The application suggests music based on the current weather, offering users an immersive and personalized experience.


## Architecture
The application follows a client-server architecture, with the frontend interacting directly with third-party APIs and the backend providing additional data processing capabilities.

#### Components
1. **Frontend**: 
   - Built using HTML, CSS, and JavaScript.
   - Interacts directly with weather and music APIs.
   - Dynamic UI rendering based on weather conditions.

2. **Backend**:
   - Python script (`get_playlist.py`) handles the Spotify API interaction.
   - Manages authentication and playlist retrieval based on weather data.

3. **APIs**:
   - **OpenWeatherMap API**: Fetches real-time weather data.
   - **Spotify API**: Retrieves playlists based on the current weather.

4. **Data Flow**:
   - User inputs location → Weather data fetched → Corresponding playlist fetched → UI updated with playlist and weather information.

### Diagram
```mermaid
graph LR
    A[User] -->|Enter Location| B[Frontend]
    B -->|Fetch Weather| C[OpenWeatherMap API]
    C -->|Return Weather Data| B
    B -->|Send Weather Data| D[Backend]
    D -->|Request Playlist| E[Spotify API]
    E -->|Return Playlist| D
    D -->|Send Playlist| B
    B -->|Display Playlist| A

```


## Frontend

### 1.1. HTML Structure
- **File**: `index.html`
- **Structure**:
  - `header`: Contains the app title and navigation.
  - `main`: 
    - `input` for location search.
    - `section` for displaying weather data.
    - `section` for displaying Spotify playlists.
  - `footer`: Includes credits and external links.

### 1.2. CSS Styling
- **File**: `style.css`
- **Main Styles**:
  - Responsive design with a mobile-first approach.
  - Custom styles for weather and playlist sections.
  - Animations for weather icons and playlist transitions.

### 1.3. JavaScript Logic
- **File**: `weather.js`
- **Functions**:
  - **fetchWeatherData(location)**: Fetches weather data from OpenWeatherMap API.
  - **fetchPlaylist(weatherCondition)**: Sends weather data to backend, retrieves playlist.
  - **updateUI(weatherData, playlist)**: Updates the UI with the fetched weather and playlist data.

## Backend

### 2.1. Python Script
- **File**: `get_playlist.py`
- **Modules**:
  - **spotipy**: Handles Spotify API interactions.
  - **requests**: Manages HTTP requests for the Spotify API.

### 2.2. Functions
- **get_oauth_token()**:
  - Authenticates with Spotify using OAuth and returns the access token.
  
- **get_playlist_for_weather(weather)**:
  - Maps weather conditions to a specific playlist category.
  - Fetches and returns a playlist based on the category.

### 2.3. Error Handling
- **Network Errors**: Retry logic for failed API calls.
- **Data Validation**: Ensures the received data from APIs is valid before processing.

## 3. API Details

### 3.1. OpenWeatherMap API
- **Base URL**: `https://api.openweathermap.org/data/2.5/weather`
- **Parameters**: `q` (location), `appid` (API key), `units` (metric/imperial)
- **Response**: 
  - `temperature`: Current temperature.
  - `weather`: Current weather condition (e.g., rain, clear).

### 3.2. Spotify API
- **Base URL**: `https://api.spotify.com/v1/`
- **Endpoints**: 
  - `/browse/categories/{category_id}/playlists`: Fetches playlists based on category.
- **Authentication**: OAuth 2.0
- **Response**: List of playlists for the given category.

## 4. Error Handling & Logging

### 4.1. Frontend
- **Weather Data Fetching**:
  - Handles cases where the location is invalid or the API request fails.
  - Provides user-friendly error messages.

### 4.2. Backend
- **API Limits**:
  - Implements rate limiting and retry logic for Spotify API calls.
  - Logs API call failures and retries for debugging.

## 5. Deployment & Environment

### 5.1. Deployment
- **Hosting**: GitHub Pages for the frontend, local execution for the backend.
- **Build Tools**: N/A, static site with manual deployment.

### 5.2. Environment Variables
- **SPOTIPY_CLIENT_ID**: Spotify API client ID.
- **SPOTIPY_CLIENT_SECRET**: Spotify API client secret.
- **WEATHER_API_KEY**: OpenWeatherMap API key.

---