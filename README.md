# VKing Casino API Documentation

## Overview
This document describes the API endpoints for integrating with VKing Casino games.

## Base URL
```
https://arabianbet.net/vking-casino/
```

## Endpoints

### 1. Get All Games List

**Endpoint:** `GET /get_game_list`

**Description:** Retrieves a list of all available games from VKing Casino.

**URL:** `https://arabianbet.net/vking-casino/get_game_list`

**Request Type:** `GET`

**Response:** The response contains game information including:
- `gameCode` - Unique identifier for the game
- `tableCode` - Unique identifier for the game table

**Note:** Extract the `gameCode` and `tableCode` parameters from this response. These values will be used in the Get Game URL request.

---

### 2. Get Game URL

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

**Important:** From the response, you only need to extract the `gameURL` parameter. This URL will redirect the user to the game lobby where they can start playing.

---

## Integration Flow

1. **Step 1:** Call `GET /get_game_list` to retrieve available games
2. **Step 2:** Extract `gameCode` and `tableCode` from the response
3. **Step 3:** Call `POST /login` with the extracted codes to get the game URL
4. **Step 4:** Extract the `gameURL` from the response and redirect the user to that URL

## Error Handling

The API returns appropriate HTTP status codes:
- `200` - Success
- `400` - Bad Request (missing or invalid parameters)
- `401` - Unauthorized
- `500` - Internal Server Error

## Notes

- All requests should include proper authentication headers as required by your system
- The `gameCode` and `tableCode` are dynamic values that change based on the game selection
- Ensure proper error handling for cases where games are unavailable or parameters are invalid
