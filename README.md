# Distributed URL Shortener

A high-performance, distributed URL shortener built using **Go** and **Redis**, designed for scalability and efficiency. It provides a reliable way to shorten long URLs, similar to TinyURL, while incorporating distributed system principles like **round-robin load balancing** and **concurrency**.

## Features
- **URL Shortening**: Generate unique short links for any given long URL.
- **Distributed Architecture**: Utilizes multiple instances with round-robin load balancing to handle requests efficiently.
- **Fast Lookups**: Uses Redis for low-latency storage and retrieval of URL mappings.
- **Concurrency Optimized**: Leverages Go’s goroutines for handling multiple requests simultaneously.
- **Scalability**: Can be extended horizontally by adding more nodes to the system.

## Tech Stack
- **Go**: High-performance backend service.
- **Redis**: In-memory key-value store for fast lookups.
- **Docker**: Containerized deployment.
- **NGINX (optional)**: Reverse proxy for load balancing.

## Installation & Setup

### Prerequisites
- **Go** (>=1.18)
- **Redis** (running instance required)
- **Docker** (optional for deployment)

### Steps
1. Clone the repository:
   ```sh
   git clone https://github.com/SHAKSHIY/distributed-url-shortener.git
   cd distributed-url-shortener
   ```
2. Install dependencies:
   ```sh
   go mod tidy
   ```
3. Run Redis (if not already running):
   ```sh
   redis-server
   ```
4. Start the service:
   ```sh
   go run main.go
   ```

## API Endpoints

### Shorten a URL
**POST /shorten**
- **Request:**
  ```json
  {
    "url": "https://example.com/long-url"
  }
  ```
- **Response:**
  ```json
  {
    "shortened_url": "http://localhost:8080/abcd1234"
  }
  ```

### Retrieve Original URL
**GET /{shortened_id}**
- **Example:** `GET /abcd1234`
- **Redirects to:** `https://example.com/long-url`

## Load Balancing (Round-Robin)
To distribute traffic efficiently, you can deploy multiple instances of the service and use **NGINX** for load balancing. Example `nginx.conf`:
```nginx
upstream url_shortener {
    server 127.0.0.1:8081;
    server 127.0.0.1:8082;
}

server {
    listen 80;
    location / {
        proxy_pass http://url_shortener;
    }
}
```

## Deployment with Docker
Build and run the containerized application:
```sh
docker build -t url-shortener .
docker run -p 8080:8080 url-shortener
```

