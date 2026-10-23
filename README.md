# TensorFilm: AI-Powered Cinematic Discovery & Recommendation Engine

[![Next.js](https://img.shields.io/badge/Next.js-16.2-black?logo=next.js&logoColor=white)](https://nextjs.org/)
[![React](https://img.shields.io/badge/React-19-61DAFB?logo=react&logoColor=black)](https://react.dev/)
[![Node.js](https://img.shields.io/badge/Node.js-20%2B-339933?logo=node.js&logoColor=white)](https://nodejs.org/)
[![Express](https://img.shields.io/badge/Express-5.x-000000?logo=express&logoColor=white)](https://expressjs.com/)
[![Google Gemini](https://img.shields.io/badge/Gemini-2.5%20Flash-4285F4?logo=google&logoColor=white)](https://ai.google.dev/)
[![MongoDB](https://img.shields.io/badge/MongoDB-6.0-47A248?logo=mongodb&logoColor=white)](https://www.mongodb.com/)
[![Redis](https://img.shields.io/badge/Redis-7-DC382D?logo=redis&logoColor=white)](https://redis.io/)
[![Docker](https://img.shields.io/badge/Docker-Compose-2496ED?logo=docker&logoColor=white)](https://www.docker.com/)
[![Tailwind CSS](https://img.shields.io/badge/Tailwind-v4-06B6D4?logo=tailwindcss&logoColor=white)](https://tailwindcss.com/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

**TensorFilm** is a modern, full-stack cinema discovery platform that merges Large Language Model (LLM) reasoning with real-time film metadata and high-throughput caching. Powered by **Google Gemini 2.5 Flash**, **TMDB**, **Redis**, and a **Next.js 16 / Express 5** microservice architecture, TensorFilm delivers context-aware conversational search, latent-space "bridge film" generation, mood-based recommendations, and user collection management.

---

## System Architecture

```text
                     ┌────────────────────────────────┐
                     │   Next.js 16 + React 19 Client │
                     │   (Tailwind CSS v4, Lucide)    │
                     └───────────────┬────────────────┘
                                     │
                             RESTful API / JSON
                                     │
                                     ▼
                     ┌────────────────────────────────┐
                     │    Node.js / Express 5 API     │
                     └───────┬───────────────┬────────┘
                             │               │
            ┌────────────────┴─────┐         │
            ▼                      ▼         ▼
  ┌───────────────────┐  ┌───────────────────┐  ┌───────────────────┐
  │   Redis 7 Cache   │  │   MongoDB 6.0     │  │  External APIs    │
  │  (TTLs: 60s-7200s)│  │ (Users, Catalogs) │  │  • Gemini 2.5 LLM │
  └───────────────────┘  └───────────────────┘  │  • TMDB v3 API    │
                                                └───────────────────┘
```

---

## Key Features

### 1. Intelligent Conversational Agent
- **7-Scenario Intent Classification:** Evaluates natural language inputs across specific titles, mood/genre/theme, cast/crew trivia, greetings, off-topic pivots, ambiguous queries, and guardrail moderation.
- **Strict Structured Outputs:** Utilizes Gemini's native `responseMimeType: "application/json"` schema enforcement to eliminate parsing hallucinations.
- **Dynamic Recommendation Scaling:** Intelligently adapts recommendations to user requests (e.g., "top 3", "give me 7") with a safeguard cap of 12 items.

### 2. "Double Feature" Bridge Film Engine
- Takes two distinct films (e.g., *Inception* and *Shutter Island*) and computes their stylistic, tonal, and thematic intersections.
- Recommends the single ideal "bridge film" with an in-depth curation rationale explaining the shared cinematic DNA.

### 3. Subjective Mood Recommender
- Translates nuanced emotional prompts (e.g., *"bittersweet nostalgia on a rainy night"*) into thematic movie selections paired with contextual justifications.

### 4. Real-Time TMDB Enrichment & Smart Filtering
- Automatically resolves AI title outputs to canonical TMDB records (posters, synopses, ratings, release dates).
- Employs heuristic filtering (filtering entries $< 5.5$ rating and sorting by popularity) to disambiguate common titles.
- Embeds high-definition YouTube trailer keys directly into movie objects.

### 5. Multi-Tier Distributed Caching
- **Redis 7 In-Memory Cache:** Implements aggressive caching strategies:
  - 60-second TTL for real-time conversational chat queries.
  - 300-second TTL for computationally heavy Double Feature lookups.
  - 1-hour to 2-hour TTLs for static catalog endpoints (Trending, Popular, Top Rated, Now Playing).
- Dramatically cuts third-party API latency and prevents rate-limit exhaustion.

### 6. User Personalization & Security
- Complete authentication flow using JSON Web Tokens (JWT) and `bcryptjs` password hashing.
- Cloud-persisted Watchlist and Favorites stored in MongoDB with atomic toggle operations.

---

## Tech Stack

| Domain | Technology | Description |
| :--- | :--- | :--- |
| **Frontend** | [Next.js 16](https://nextjs.org/) (App Router) | High-performance server/client rendered React 19 UI |
| **Styling** | [Tailwind CSS v4](https://tailwindcss.com/) | Modern utility-first CSS framework |
| **Backend** | [Express 5](https://expressjs.com/) on [Node.js 20+](https://nodejs.org/) | Modular RESTful API services (ES Modules) |
| **AI / LLM** | [Google Gemini 2.5 Flash](https://ai.google.dev/) | Low-latency, emotionally intelligent generative AI |
| **Database** | [MongoDB 6.0](https://www.mongodb.com/) via [Mongoose 9](https://mongoosejs.com/) | Document store for accounts, watchlists, and favorites |
| **Caching** | [Redis 7](https://redis.io/) via [ioredis](https://github.com/redis/ioredis) | High-throughput in-memory data store |
| **External API**| [The Movie Database (TMDB)](https://www.themoviedb.org/) | Film catalogs, posters, release schedules, and video trailers |
| **DevOps** | [Docker](https://www.docker.com/) & Docker Compose | Multi-container orchestration across all services |

---

## Repository Structure

```text
tensorfilm/
├── backend/
│   ├── config/             # Database & Redis client configuration
│   ├── controllers/        # Request handlers (auth, chat, movie, lists)
│   ├── middleware/         # JWT verification & route protection
│   ├── models/             # Mongoose schemas (User, MovieCache)
│   ├── routes/             # REST endpoint routing definitions
│   ├── services/           # Gemini AI, TMDB API, and recommendation logic
│   ├── Dockerfile          # Backend production container specification
│   ├── package.json        # Backend dependencies & scripts
│   └── server.js           # Server entry point & CORS configuration
├── frontend/
│   ├── app/                # Next.js 16 App Router (pages, layouts, globals)
│   ├── public/             # Static brand assets and SVGs
│   ├── Dockerfile          # Frontend container specification
│   ├── package.json        # Frontend dependencies & scripts
│   └── tsconfig.json       # TypeScript compiler configuration
├── docker-compose.yml      # Orchestration for Mongo, Redis, Backend, Frontend
├── package.json            # Monorepo management scripts
└── README.md               # Project documentation
```

---

## API Reference

### Authentication (`/api/auth`)
| Method | Endpoint | Description | Auth Required |
| :--- | :--- | :--- | :--- |
| `POST` | `/api/auth/register` | Create a new user account | No |
| `POST` | `/api/auth/login` | Authenticate user and return JWT | No |
| `GET` | `/api/auth/me` | Fetch authenticated user profile | Yes (Bearer Token) |

### AI Discovery & Chat (`/api/chat`)
| Method | Endpoint | Description | Auth Required |
| :--- | :--- | :--- | :--- |
| `POST` | `/api/chat` | Natural language cinema inquiry with TMDB metadata | Optional |
| `POST` | `/api/chat/insight` | Generates a 120-word AI viewer insight for any title | Optional |
| `POST` | `/api/chat/double-feature`| Computes the optimal bridge movie linking two films | Optional |

### Movies & Catalogs (`/api/movies`)
| Method | Endpoint | Description | Auth Required |
| :--- | :--- | :--- | :--- |
| `GET` | `/api/movies/search?query=` | Search movies with Redis caching | No |
| `GET` | `/api/movies/recommendations?mood=` | Fetch mood-based AI recommendations | No |
| `GET` | `/api/movies/trending` | Top 10 weekly trending films with trailers | No |
| `GET` | `/api/movies/popular` | Most popular films (cached 1 hr) | No |
| `GET` | `/api/movies/top-rated` | Critically acclaimed films (cached 2 hrs) | No |
| `GET` | `/api/movies/now-playing` | Currently in-theaters catalog | No |
| `POST` | `/api/movies/watchlist` | Toggle film in user watchlist | Yes (Bearer Token) |
| `POST` | `/api/movies/favorites` | Toggle film in user favorites | Yes (Bearer Token) |
| `GET` | `/api/movies/lists` | Retrieve user watchlist & favorites | Yes (Bearer Token) |

---

## Getting Started

### Prerequisites
- [Docker](https://docs.docker.com/get-docker/) and [Docker Compose](https://docs.docker.com/compose/) installed, **OR**
- [Node.js 20+](https://nodejs.org/), [MongoDB](https://www.mongodb.com/), and [Redis](https://redis.io/) installed locally.
- API keys for [Google Gemini](https://ai.google.dev/) and [The Movie Database (TMDB)](https://www.themoviedb.org/documentation/api).

---

### Option 1: Quickstart with Docker Compose (Recommended)

1. **Clone the repository:**
   ```bash
   git clone https://github.com/abdullahej5411/tensorfilm.git
   cd tensorfilm
   ```

2. **Configure Environment Variables:**

   Create `backend/.env`:
   ```env
   PORT=5000
   MONGO_URI=mongodb://admin:password@mongo:27017/smart_movie_db?authSource=admin
   REDIS_HOST=redis
   REDIS_PORT=6379
   JWT_SECRET=your_jwt_secret_key_here
   GEMINI_API_KEY=your_gemini_api_key_here
   TMDB_API_KEY=your_tmdb_api_key_here
   ```

   Create `frontend/.env`:
   ```env
   NEXT_PUBLIC_API_URL=http://localhost:5000
   ```

3. **Launch the entire stack:**
   ```bash
   docker compose up --build
   ```

4. **Access the applications:**
   - **Frontend:** [http://localhost:3000](http://localhost:3000)
   - **Backend API:** [http://localhost:5000](http://localhost:5000)
   - **MongoDB:** `localhost:27017`
   - **Redis:** `localhost:6379`

---

### Option 2: Manual Local Setup

#### 1. Backend Setup
```bash
cd backend
npm install
# Ensure local MongoDB and Redis instances are running
npm start
```

#### 2. Frontend Setup
```bash
cd frontend
npm install
npm run dev
```

Visit [http://localhost:3000](http://localhost:3000) in your browser.

---

## References & Data Sources

- **Generative AI:** Powered by the [Google Gemini 2.5 Flash API](https://ai.google.dev/).
- **Film Metadata & Imagery:** Provided by [The Movie Database (TMDB)](https://www.themoviedb.org/). This product uses the TMDB API but is not endorsed or certified by TMDB.
- **Video Trailers:** Streamed via the official YouTube player integration.

---

## License

This project is licensed under the [MIT License](LICENSE).
