# Tailspin Shelter API Documentation

This document provides comprehensive documentation for the Tailspin Shelter REST API. The API is built using Flask and provides endpoints for managing dog adoption information.

## Base URL

```
http://localhost:5100
```

## Common Response Codes

| Status Code | Description |
|-------------|-------------|
| 200 | Success |
| 404 | Resource not found |
| 500 | Internal server error |

## Endpoints

### 1. Get All Dogs

Retrieves a list of all dogs with their basic information.

**Endpoint:** `GET /api/dogs`

**Request Format:**
```bash
curl -X GET http://localhost:5100/api/dogs
```

**Response Schema:**
```json
[
  {
    "id": integer,
    "name": string,
    "breed": string
  }
]
```

**Success Response Example:**
```json
[
  {
    "id": 1,
    "name": "Buddy",
    "breed": "Golden Retriever"
  },
  {
    "id": 2,
    "name": "Luna",
    "breed": "Husky"
  }
]
```

**Empty Response Example:**
```json
[]
```

**Error Response (500):**
```json
{
  "error": "Internal server error"
}
```

---

### 2. Get Dog by ID

Retrieves detailed information about a specific dog.

**Endpoint:** `GET /api/dogs/<id>`

**URL Parameters:**
- `id` (integer, required): The unique identifier of the dog

**Request Format:**
```bash
curl -X GET http://localhost:5100/api/dogs/1
```

**Response Schema:**
```json
{
  "id": integer,
  "name": string,
  "breed": string,
  "age": integer,
  "description": string,
  "gender": string,
  "status": string  // "AVAILABLE", "PENDING", or "ADOPTED"
}
```

**Success Response Example:**
```json
{
  "id": 1,
  "name": "Buddy",
  "breed": "Golden Retriever",
  "age": 3,
  "description": "Friendly and energetic golden retriever who loves to play fetch",
  "gender": "Male",
  "status": "AVAILABLE"
}
```

**Error Response (404):**
```json
{
  "error": "Dog not found"
}
```

**Error Response (500):**
```json
{
  "error": "Internal server error"
}
```

---

### 3. Get Dog Human Age

Calculates and returns a dog's age in human years using the standard conversion formula.

**Endpoint:** `GET /api/dogs/<id>/human-age`

**URL Parameters:**
- `id` (integer, required): The unique identifier of the dog

**Request Format:**
```bash
curl -X GET http://localhost:5100/api/dogs/1/human-age
```

**Calculation Formula:**
- First 2 years: 10.5 human years each
- Years after 2: 4 human years each

**Response Schema:**
```json
{
  "dog_name": string,
  "dog_age": integer,
  "human_age": float
}
```

**Success Response Example (Dog age ≤ 2):**
```json
{
  "dog_name": "Puppy",
  "dog_age": 1,
  "human_age": 10.5
}
```

**Success Response Example (Dog age > 2):**
```json
{
  "dog_name": "Buddy",
  "dog_age": 5,
  "human_age": 33.0
}
```

**Error Response (404):**
```json
{
  "error": "Dog not found"
}
```

**Error Response (500):**
```json
{
  "error": "Internal server error"
}
```

---

### 4. Get All Breeds

Retrieves a list of all available dog breeds.

**Endpoint:** `GET /api/breeds`

**Request Format:**
```bash
curl -X GET http://localhost:5100/api/breeds
```

**Response Schema:**
```json
[
  {
    "id": integer,
    "name": string
  }
]
```

**Success Response Example:**
```json
[
  {
    "id": 1,
    "name": "Golden Retriever"
  },
  {
    "id": 2,
    "name": "Husky"
  },
  {
    "id": 3,
    "name": "Labrador"
  }
]
```

**Empty Response Example:**
```json
[]
```

**Error Response (500):**
```json
{
  "error": "Internal server error"
}
```

---

## Data Models

### Dog Status Enum

The adoption status of a dog can be one of the following values:

- `AVAILABLE` - Dog is available for adoption
- `PENDING` - Adoption is pending
- `ADOPTED` - Dog has been adopted

### Gender Values

Valid gender values:
- `Male`
- `Female`

### Age Range

Dog age is validated to be between 0 and 20 years.

---

## Error Handling

All endpoints include error handling for:

1. **Database Errors**: Returns 500 status code with `{"error": "Internal server error"}`
2. **Resource Not Found**: Returns 404 status code with `{"error": "Dog not found"}`
3. **Invalid Parameters**: Returns 404 for invalid IDs (e.g., non-numeric values)

---

## Complete Usage Examples

### Example 1: Get all dogs and then fetch details for the first dog

```bash
# Get all dogs
curl -X GET http://localhost:5100/api/dogs

# Response: [{"id": 1, "name": "Buddy", "breed": "Golden Retriever"}, ...]

# Get details for dog with id 1
curl -X GET http://localhost:5100/api/dogs/1

# Response: {"id": 1, "name": "Buddy", "breed": "Golden Retriever", "age": 3, ...}
```

### Example 2: Calculate human age for a dog

```bash
# Get human age for dog with id 1
curl -X GET http://localhost:5100/api/dogs/1/human-age

# Response: {"dog_name": "Buddy", "dog_age": 5, "human_age": 33.0}
```

### Example 3: Get all available breeds

```bash
# Get all breeds
curl -X GET http://localhost:5100/api/breeds

# Response: [{"id": 1, "name": "Golden Retriever"}, {"id": 2, "name": "Husky"}, ...]
```

### Example 4: Handle error cases

```bash
# Try to get a non-existent dog
curl -X GET http://localhost:5100/api/dogs/999

# Response: {"error": "Dog not found"}
# Status Code: 404

# Try to get human age for invalid ID
curl -X GET http://localhost:5100/api/dogs/invalid/human-age

# Response: (404 error page)
# Status Code: 404
```

---

## Notes

- All endpoints use the `GET` HTTP method
- Content-Type is `application/json` for all responses
- The API uses SQLAlchemy ORM for database operations
- Database operations are mocked in unit tests
- CORS is not currently configured; adjust if needed for production
- Port 5100 is used to avoid conflicts with macOS AirPlay Receiver

---

## Testing

For testing purposes, sample curl commands with headers:

```bash
# With verbose output
curl -v -X GET http://localhost:5100/api/dogs

# With timing information
curl -w "\nTime: %{time_total}s\n" -X GET http://localhost:5100/api/dogs

# Save response to file
curl -X GET http://localhost:5100/api/dogs -o dogs.json
```
