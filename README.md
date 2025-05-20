# AfricanStreamingApp

# African Streaming App Concept

A streaming platform to bring African movies to global audiences, focusing on Nollywood and Ghallywood.

## Vision
- Curate high-quality African films with multilingual subtitles.
- Enable offline downloads for accessibility in low-bandwidth regions.
- Foster community-driven content recommendations.

## Current Progress
- Market research: Analyzed competitors like IrokoTV and Netflix Africa.
- Prototype: Basic wireframes for user interface (created with Figma).
- Next Steps: Build a Python-based demo for content recommendation logic.

## Tech Stack (Planned)
- Frontend: React
- Backend: Python (Flask/Django)
- Cloud: AWS for content delivery

Looking for co-founders to validate and scale this idea!
![image](https://github.com/user-attachments/assets/9ebe4579-9239-4950-bd7d-c7ef109a43ea)

import random
from collections import defaultdict

class AfricanStreamingRecommender:
    def __init__(self):
        # Expanded African content database with ratings
        self.content_db = {
            "Nollywood": [
                {"title": "The Wedding Party", "genre": "Comedy", "language": "English", "rating": 4.5},
                {"title": "King of Boys", "genre": "Drama", "language": "Yoruba", "rating": 4.8},
                {"title": "Lionheart", "genre": "Drama", "language": "Igbo", "rating": 4.3},
            ],
            "Swahili Films": [
                {"title": "Veve", "genre": "Drama", "language": "Swahili", "rating": 4.0},
                {"title": "Supa Modo", "genre": "Comedy", "language": "Swahili", "rating": 4.7},
            ],
            "Afrobeats": [
                {"title": "Essence - Wizkid", "genre": "Music", "language": "English", "rating": 4.9},
                {"title": "Last Last - Burna Boy", "genre": "Music", "language": "Pidgin", "rating": 4.8},
            ],
            "South African": [
                {"title": "Queen Sono", "genre": "Action", "language": "Zulu", "rating": 4.2},
                {"title": "Blood & Water", "genre": "Drama", "language": "English", "rating": 4.4},
            ],
            "Francophone": [
                {"title": "Atlantique", "genre": "Drama", "language": "French", "rating": 4.1},
                {"title": "Félicité", "genre": "Drama", "language": "Lingala", "rating": 4.6},
            ],
        }
        
        # Mock user watch history (for personalized recs)
        self.user_history = defaultdict(list)
        self.user_history["user1"] = ["The Wedding Party", "Veve", "Essence - Wizkid"]

    def recommend_based_on_language(self, language, max_recommendations=3):
        """Recommend top-rated content in a specific language"""
        recs = [
            item["title"] 
            for category in self.content_db.values() 
            for item in category 
            if item["language"].lower() == language.lower()
        ]
        return random.sample(recs, min(max_recommendations, len(recs)))

    def recommend_based_on_genre(self, genre, max_recommendations=3):
        """Recommend content by genre, sorted by rating"""
        recs = [
            item["title"] 
            for category in self.content_db.values() 
            for item in category 
            if item["genre"].lower() == genre.lower()
        ]
        return random.sample(recs, min(max_recommendations, len(recs)))

    def trending_in_region(self, region="Nollywood", max_recommendations=3):
        """Top-rated content in a region"""
        recs = sorted(
            self.content_db.get(region, []),
            key=lambda x: x["rating"],
            reverse=True
        )[:max_recommendations]
        return [item["title"] for item in recs]

    def personalized_recommendations(self, user_id, max_recommendations=3):
        """Recommend similar to user's watch history"""
        user_watched = self.user_history.get(user_id, [])
        if not user_watched:
            return self.trending_in_region("Nollywood")  # Fallback
        
        # Find genres/languages the user likes
        user_preferences = defaultdict(int)
        for title in user_watched:
            for category in self.content_db.values():
                for item in category:
                    if item["title"] == title:
                        user_preferences[item["genre"]] += 1
                        user_preferences[item["language"]] += 1
        
        # Recommend based on preferences
        preferred_genre = max(user_preferences.items(), key=lambda x: x[1])[0]
        return self.recommend_based_on_genre(preferred_genre, max_recommendations)

if __name__ == "__main__":
    recommender = AfricanStreamingRecommender()

    print("🔥 Trending in Nollywood (Top Rated):")
    print(recommender.trending_in_region("Nollywood"))

    print("\n🎤 Recommended Pidgin Music:")
    print(recommender.recommend_based_on_language("Pidgin"))

    print("\n🎭 Drama Picks:")
    print(recommender.recommend_based_on_genre("Drama"))

    print("\n✨ Personalized for user1:")
    print(recommender.personalized_recommendations("user1"))
