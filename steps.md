#### React Movie App: Step-by-Step Instructions

#### 1. Setup the React Project

```sh
bash
Copy
Edit
npx create-react-app movie-app
cd movie-app
```

#### 2. Clean Up Starter Files

Remove unneeded files:

Delete App.test.js, logo.svg, setupTests.js, and reportWebVitals.js.

In index.js, make sure it only renders the <App />.

#### Update index.js:

```js
js;
Copy;
Edit;
import React from 'react';
import ReactDOM from 'react-dom/client';
import App from './App';
import './index.css';

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(<App />);
```

#### 3. Project Structure

Create a folder for components:

```sh
bash
Copy
Edit
mkdir src/components
touch src/components/Movie.js
```

#### 4. Movie Component

src/components/Movie.js:

```js
import React from 'react';

const Movie = ({ title, poster_path, overview, vote_average }) => {
  const IMAGE_API = 'https://image.tmdb.org/t/p/w500';

  return (
    <div className='movie'>
      <img src={IMAGE_API + poster_path} alt={title} />
      <div className='movie-info'>
        <h3>{title}</h3>
        <span>{vote_average}</span>
      </div>
      <div className='movie-overview'>
        <h4>Overview:</h4>
        <p>{overview}</p>
      </div>
    </div>
  );
};

export default Movie;
```

#### 5. App Component Logic

In App.js:

```js
import React, { useState, useEffect } from 'react';
import Movie from './components/Movie';
import './App.css';

const FEATURED_API =
  'https://api.themoviedb.org/3/discover/movie?sort_by=popularity.desc&api_key=YOUR_API_KEY';
const SEARCH_API =
  'https://api.themoviedb.org/3/search/movie?&api_key=YOUR_API_KEY&query=';

function App() {
  const [movies, setMovies] = useState([]);
  const [searchTerm, setSearchTerm] = useState('');

  useEffect(() => {
    fetch(FEATURED_API)
      .then((res) => res.json())
      .then((data) => setMovies(data.results));
  }, []);

  const handleSearch = (e) => {
    e.preventDefault();

    if (searchTerm.trim()) {
      fetch(SEARCH_API + searchTerm)
        .then((res) => res.json())
        .then((data) => {
          setMovies(data.results);
          setSearchTerm('');
        });
    }
  };

  return (
    <>
      <header>
        <form onSubmit={handleSearch}>
          <input
            type='text'
            placeholder='Search for a movie...'
            value={searchTerm}
            onChange={(e) => setSearchTerm(e.target.value)}
          />
        </form>
      </header>
      <div className='movie-container'>
        {movies.length > 0 &&
          movies.map((movie) => <Movie key={movie.id} {...movie} />)}
      </div>
    </>
  );
}

export default App;
```

#### Replace YOUR_API_KEY with your TMDB API key.

#### 6. Basic CSS Styling

Create or update App.css:

```css
body {
  background-color: #222;
  font-family: 'Poppins', sans-serif;
  color: #fff;
  margin: 0;
  padding: 0;
}

header {
  padding: 1rem;
  text-align: center;
}

header input {
  padding: 0.5rem;
  width: 50%;
  max-width: 400px;
  border-radius: 5px;
  border: none;
  font-size: 1rem;
}

.movie-container {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  padding: 1rem;
}

.movie {
  background-color: #333;
  margin: 1rem;
  width: 250px;
  border-radius: 10px;
  overflow: hidden;
  transition: transform 0.2s ease;
}

.movie:hover {
  transform: scale(1.05);
}

.movie img {
  width: 100%;
}

.movie-info {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 1rem;
}

.movie-overview {
  padding: 1rem;
  font-size: 0.9rem;
  line-height: 1.4;
}
```

#### 7. Run Your App

```sh
npm start
```

#### You should see a list of trending movies, and be able to search for others.

#### 8. Optional Enhancements

Add rating color (green/yellow/red based on score).

Add loading indicator.

Add error handling for failed API calls.

Refactor using custom hooks for fetching.
