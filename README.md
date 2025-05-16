Reels Recommendation/
│
├── src/
│   ├── recommendation/
│   │   ├── config.py              # Database & model setup, SQLAlchemy models
│   │   ├── controller.py          # Business logic for orchestrating recommendation workflow
│   │   ├── router.py              # API routes exposed via FastAPI
│   │   ├── service.py             # Core logic: embeddings, similarity, DB queries
│
├── data/
│   └── (optional CSVs or data files)
│
├── main.py                        # Swagger/FastAPI entry point
└── README.md                      # You're here 📄


------config.py---
Sets up:

SQLAlchemy engine & session (SessionLocal)

PostgreSQL connection string

Loads the SentenceTransformer model once

Defines:

Base SQLAlchemy models (PostsData)

Mixins for soft delete, timestamps, and user tracking



 ---service.py---

Handles all backend logic:

DatabaseService
get_user_posts(user_id): Fetches non-deleted posts of a specific user.

get_all_posts(limit): Returns a pool of all posts for recommendations.

 EmbeddingService
get_cached_embedding(text): Generates or returns a cached embedding of the text.

precompute_embeddings(posts): Returns embeddings for all post events.

 RecommendationService
calculate_similarity(...): Uses cosine similarity to find closest matches between user profile and post pool.

get_user_top_hashtags(...): Extracts most-used hashtags from user posts.

get_cross_niche_hashtags(...): Finds overlapping tags between user and all posts.

format_recommendations(...): Formats post data with score and label.

to_datetime_safe(...): Safely converts date strings.


----controller.py---
Main entry point for recommendation logic.

Function: recommend_reels_for_user(user_id)

Gets user posts and all posts.

Computes user embedding vector.

Recommends:

Similar posts (embedding-based)

Recent posts with user's hashtags

Random fallback posts (for new users or low data)

Formats and deduplicates final list (up to 20).



----router.py----
FastAPI route: /recommend

Endpoint:
GET /recommend?user_id=<USER_ID>
