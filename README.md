import requests
from bs4 import BeautifulSoup

# Example: scraping IMDB for movie ratings
URL = "https://www.imdb.com/chart/top/"

# Send HTTP request to the website
response = requests.get(URL)
if response.status_code != 200:
    print("Failed to retrieve page")
    exit()

# Parse the HTML content
soup = BeautifulSoup(response.text, "html.parser")

# Extract movie titles and ratings
movies = soup.select("td.titleColumn a")  # Movie titles
ratings = soup.select("td.ratingColumn.imdbRating strong")  # Ratings

# Store results
top_movies = []
for movie, rating in zip(movies, ratings):
    title = movie.get_text()
    rating_value = rating.get_text()
    link = "https://www.imdb.com" + movie['href']
    top_movies.append({"title": title, "rating": rating_value, "link": link})

# Print top 10 results
print("\nTop 10 Movies on IMDb:")
for i, movie in enumerate(top_movies[:10], start=1):
    print(f"{i}. {movie['title']} — {movie['rating']} ⭐ ({movie['link']})")
