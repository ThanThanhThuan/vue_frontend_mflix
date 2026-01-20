# 🍿 Mflix Frontend (Vue 3 + Vite)

A modern, responsive Single Page Application (SPA) built with Vue.js 3 and Vite that serves as the user interface for the Mflix Movie Tracker. 
It connects to my Node.js/Express backend to browse, search, and view details from the MongoDB sample_mflix dataset.
🚀 Live Backend

This frontend is currently configured to consume data from onrender.com

🛠 Tech Stack

    Framework: Vue 3 (Composition API)

    Build Tool: Vite

    Routing: Vue Router 4

    HTTP Client: Axios

    Styling: Custom CSS (Dark Theme)

<img width="1588" height="1083" alt="image" src="https://github.com/user-attachments/assets/ac3b5996-96c2-4deb-a17d-8bbd6ce3cd73" />


✨ Features

    Movie Browsing: Grid layout displaying movie posters, titles, and release years.

    Pagination: Navigate through thousands of movies efficiently (server-side pagination).

    Search: Real-time search functionality to find movies by title.

    Movie Details: Dedicated route (/movie/:id) showing full plot, cast, genres, ratings, and runtime.

    Error Handling: Fallback images for missing posters and loading states for better UX.

    Environment Config: API URLs managed via .env files for security and flexibility.

📂 Project Structure
code Text

mflix-frontend/
├── .env                # Environment variables (API URL)
├── public/             # Static assets
├── src/
│   ├── assets/         # Global styles (main.css)
│   ├── router/         # Vue Router configuration
│   ├── views/          # Page components
│   │   ├── HomeView.vue   # Main grid & search
│   │   └── MovieView.vue  # Single movie details
│   ├── App.vue         # Root layout & Navbar
│   └── main.js         # Entry point
└── package.json        # Dependencies

⚙️ Setup & Installation
1. Clone the Repository
code Bash

git clone <your-repo-url>
cd mflix-frontend

2. Install Dependencies
code Bash

npm install

3. Configure Environment

Note: In Vite, variables must start with VITE_.

VITE_API_BASE_URL=xxxx.onrender.com/api/movies

4. Run Development Server

npm run dev

Open your browser to http://localhost:5173.
5. Build for Production

To create a production-ready build:

npm run build

📝 Usage Guide

    Home Page: Upon loading, the app fetches the first 12 movies.

    Searching: Type a keyword (e.g., "Matrix") in the search bar and press Enter or click Search.

    Navigation: Click on any movie card to view its specific details.

    Back Button: Inside the movie detail view, use the "← Back" button to return to the search results or list.
