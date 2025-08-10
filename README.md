# alx-project-0x14

MoviesDatabase API Documentation Overview
##API Overview

The MoviesDatabase API provides access to a comprehensive collection of movie and TV show data including titles, ratings, crew details, and images. Key features include:

    Retrieval of movie/TV show details by ID, title, or genre

    Access to cast/crew information and character details

    Image galleries for titles

    Advanced filtering options (year, genre, ratings, etc.)

    Title search functionality

##Version

The current API version is 1.0 (as specified in the base URL: https://moviesdatabase.p.rapidapi.com).
Available Endpoints

Main endpoints include:

    /titles: Get movie/TV show listings (supports filtering/pagination)

    /titles/{id}: Get details for a specific title by ID

    /titles/search/title/{title}: Search titles by name

    /titles/x/upcoming: Get upcoming releases

    /titles/{id}/ratings: Get ratings for a specific title

    /titles/{id}/cast: Get cast members for a title

    /titles/images/{id}: Get images for a title

    /actors/{id}: Get actor details by ID

    /genres: Get available genres

##Request and Response Format

Request Structure:
http

GET /titles?year=2020&genre=Action&limit=10
Host: moviesdatabase.p.rapidapi.com
X-RapidAPI-Key: your_api_key

Response Example (200 OK):
json

{
  "results": [
    {
      "id": "tt1234567",
      "title": "Sample Movie",
      "year": 2020,
      "genre": "Action",
      "rating": "8.5",
      // ...additional fields
    }
  ],
  "nextPage": 2
}

##Authentication

Authentication requires:

    API Key: Obtain from RapidAPI dashboard

    Headers:

        X-RapidAPI-Key: your_api_key

        X-RapidAPI-Host: moviesdatabase.p.rapidapi.com

Requests without valid headers will return 401 Unauthorized.
##Error Handling

Common HTTP errors:

    400 Bad Request: Invalid parameters (e.g., malformed filter)

    401 Unauthorized: Missing/invalid API key

    404 Not Found: Resource doesn't exist (e.g., invalid ID)

    429 Too Many Requests: Rate limit exceeded

    500 Internal Server Error: Server-side issue

Error Response Example:
json

{
  "status": 404,
  "message": "Title not found"
}

##Usage Limits and Best Practices

Rate Limits:

    Free tier: 500 requests/day

    Paid tiers: Up to 10,000 requests/day

Best Practices:

    Caching: Cache frequently accessed data locally

    Pagination: Use page and limit parameters for large datasets

    Filtering: Narrow results using available filters (year, genre, etc.)

    Error Handling: Implement retry logic for 429 errors with exponential backoff

    API Keys: Rotate keys periodically and never expose in client-side code

Example Request in TypeScript:
typescript

interface Title {
  id: string;
  title: string;
  year: number;
  // ...other fields
}

const fetchMovies = async (): Promise<Title[]> => {
  const response = await fetch(
    "https://moviesdatabase.p.rapidapi.com/titles?limit=5",
    {
      headers: {
        "X-RapidAPI-Key": API_KEY,
        "X-RapidAPI-Host": "moviesdatabase.p.rapidapi.com",
      },
    }
  );
  const data = await response.json();
  return data.results as Title[];
};

