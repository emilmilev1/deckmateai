# API Documentation

## Fetching Cards

### Endpoint

- `GET /api/fetch-cards`

### Response

- **Status**: 200 OK
- **Body**:
  ```json
  {
      "status": 200,
      "cards": [
          {
              "id": 1,
              "iconUrls": "https://example.com/card1.png",
              "name": "Card Name",
              "level": 1,
              "rarity": "Common",
              "elixirCost": 3,
              "count": 5
          }
      ]
  }
  ```

### Error Handling

In case of an error, the response will include:
- **Status**: 400 Bad Request
- **Body**:
  ```json
  {
      "status": 400,
      "message": "Error message here"
  }
  ```