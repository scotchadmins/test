# 🎰 VKing Casino API Documentation

## 📋 Overview
This document describes the API endpoints for integrating with VKing Casino games, including game list retrieval and game URL generation.

## 🌐 Base URL
```
https://arabianbet.net/vking-casino/
```

## 🚀 Core Integration Endpoints

### 1️⃣ Get All Games List

**Endpoint:** `GET /get_game_list`

**Description:** Retrieves a list of all available games from VKing Casino.

**URL:** `https://arabianbet.net/vking-casino/get_game_list`

**Request Type:** `GET`

**Response:** The response contains game information including:
- `gameCode` - Unique identifier for the game
- `tableCode` - Unique identifier for the game table

> **Note:** Extract the `gameCode` and `tableCode` parameters from this response. These values will be used in the Get Game URL request.

---

### 2️⃣ Get Game URL

**Endpoint:** `POST /login`

**Description:** Authenticates the user and returns the game URL for playing a specific game.

**URL:** `https://arabianbet.net/vking-casino/login`

**Request Type:** `POST`

**Required Parameters:**
- `tpGameId` - The game code obtained from the Get All Games List response
- `tpGameTableId` - The table code obtained from the Get All Games List response

**Example Request Body:**
```json
{
    "tpGameId": "GAME_CODE_FROM_RESPONSE",
    "tpGameTableId": "TABLE_CODE_FROM_RESPONSE"
}
```

**Response:**
```json
{
    "status": "0",
    "errorMessage": "",
    "gameURL": "https://staging.supernowa.net/lobby?Token=B027192C-2908-46B8-ABE4-6FBED0A52BEC&Code=TP"
}
```

> **Important:** From the response, you only need to extract the `gameURL` parameter. This URL will redirect the user to the game lobby where they can start playing.

---

## 🔄 Integration Flow

### Step 1: Get Games List
Call `GET /get_game_list` to retrieve available games

### Step 2: Extract Codes
Extract `gameCode` and `tableCode` from the response

### Step 3: Get Game URL
Call `POST /login` with the extracted codes to get the game URL

### Step 4: Redirect User
Extract the `gameURL` from the response and redirect the user to that URL

---

## 🎮 Game List Filtering Endpoints

This document also describes additional API endpoints for retrieving game lists filtered by game types.

## 🔐 Authentication
Most endpoints require user authentication. Ensure you are logged in before making requests.

---

## 🎯 Game List Filtering Endpoints

### 1. Get Game List by Type

**Endpoint:** `GET /get_game_list_by_type`

**Description:** Retrieves a filtered list of games based on the specified game type.

**URL:** `https://arabianbet.net/vking-casino/get_game_list_by_type`

**Query Parameters:**
| Parameter | Type | Required | Description | Allowed Values |
|-----------|------|----------|-------------|----------------|
| `type` | string | Yes | Game type filter | `live`, `virtual` |

**Example Requests:**

**Live Games:**
```
GET https://arabianbet.net/vking-casino/get_game_list_by_type?type=live
```

**Virtual Games:**
```
GET https://arabianbet.net/vking-casino/get_game_list_by_type?type=virtual
```

**Response Format:**
```json
{
  "gameList": [
    {
      "gameCode": "string",
      "gameName": "string",
      "data": [
        {
          "type": "live|virtual",
          "gameId": "string",
          "gameName": "string",
          "provider": "string",
          "minBet": "number",
          "maxBet": "number",
          "currency": "string"
        }
      ]
    }
  ]
}
```

**Response Fields:**
- `gameList`: Array of game providers
  - `gameCode`: Unique identifier for the game provider
  - `gameName`: Display name for the game provider
  - `data`: Array of games filtered by the specified type
    - `type`: Game type (live or virtual)
    - `gameId`: Unique game identifier
    - `gameName`: Display name of the game
    - `provider`: Game provider name
    - `minBet`: Minimum bet amount
    - `maxBet`: Maximum bet amount
    - `currency`: Currency code

**Error Response:**
```json
{
  "status": "106",
  "errorMessage": "Internal server error"
}
```

---

### 2. Get Game List by Multiple Types

**Endpoint:** `POST /get_game_list_by_types`

**Description:** Retrieves a filtered list of games based on multiple game types.

**URL:** `https://arabianbet.net/vking-casino/get_game_list_by_types`

**Request Body:**
```json
{
  "types": ["live", "virtual"]
}
```

**Request Fields:**
| Field | Type | Required | Description | Allowed Values |
|-------|------|----------|-------------|----------------|
| `types` | array | Yes | Array of game types to filter by | Array containing `live` and/or `virtual` |

**Example Request:**
```bash
curl -X POST https://arabianbet.net/vking-casino/get_game_list_by_types \
  -H "Content-Type: application/json" \
  -d '{"types": ["live", "virtual"]}'
```

**Response Format:** Same as single type endpoint

---

### 3. Get All Games

**Endpoint:** `GET /get_game_list`

**Description:** Retrieves the complete list of all available games without filtering.

**URL:** `https://arabianbet.net/vking-casino/get_game_list`

**Example Request:**
```
GET https://arabianbet.net/vking-casino/get_game_list
```

**Response Format:** Same as filtered endpoints, but includes all games regardless of type.

---

## Game Types

### Live Games
- **Type:** `live`
- **Description:** Real-time casino games with live dealers
- **Examples:** Live Blackjack, Live Roulette, Live Baccarat, Live Poker

### Virtual Games
- **Type:** `virtual`
- **Description:** Computer-generated casino games
- **Examples:** Virtual Slots, Virtual Table Games, Virtual Card Games

---

## Usage Examples

### cURL Examples
```bash
# Get live games
curl "https://arabianbet.net/vking-casino/get_game_list_by_type?type=live"

# Get virtual games
curl "https://arabianbet.net/vking-casino/get_game_list_by_type?type=virtual"

# Get games by multiple types
curl -X POST https://arabianbet.net/vking-casino/get_game_list_by_types \
  -H "Content-Type: application/json" \
  -d '{"types": ["live", "virtual"]}'
```

---

## Error Handling

### Common HTTP Status Codes
- `200 OK`: Request successful
- `400 Bad Request`: Invalid parameters
- `401 Unauthorized`: Authentication required
- `500 Internal Server Error`: Server error

### Error Response Format
```json
{
  "status": "106",
  "errorMessage": "Error description"
}
```

---

## Rate Limiting
- **Cache Duration:** 1 hour (3600 seconds)
- **Request Limits:** Not specified in current implementation
- **Recommendation:** Implement appropriate rate limiting based on your application needs

---

## Notes
- All endpoints require user authentication
- Game data is cached for 1 hour to improve performance
- The API supports both GET and POST methods for different use cases
- Game types are strictly validated (`live` or `virtual` only)
- Response data includes game provider information and individual game details

---

## Support
For technical support or questions about the API, please contact the development team.

---

## ⚠️ Error Handling

The API returns appropriate HTTP status codes:

| Status Code | Description |
|-------------|-------------|
| `200` | Success |
| `400` | Bad Request (missing or invalid parameters) |
| `401` | Unauthorized |
| `500` | Internal Server Error |

---

## 📝 Notes

- All requests should include proper authentication headers as required by your system
- The `gameCode` and `tableCode` are dynamic values that change based on the game selection
- Ensure proper error handling for cases where games are unavailable or parameters are invalid

---

## 🔗 Quick Reference

| Action | Method | Endpoint | Purpose |
|--------|--------|----------|---------|
| Get Games | `GET` | `/get_game_list` | Retrieve available games |
| Get Game URL | `POST` | `/login` | Get game lobby URL |
| Filter by Type | `GET` | `/get_game_list_by_type` | Get games by type (live/virtual) |
| Filter by Types | `POST` | `/get_game_list_by_types` | Get games by multiple types |

---
