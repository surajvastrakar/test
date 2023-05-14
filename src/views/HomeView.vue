<template>
  <div class="home">
    <div class="feature-card">
      <router-link to="/movie/tt0409591">
        <img
          src="https://m.media-amazon.com/images/M/MV5BNjM0NTc0NzItM2FlYS00YzEwLWE0YmUtNTA2ZWIzODc2OTgxXkEyXkFqcGdeQXVyNTgwNzIyNzg@._V1_SX300.jpg"
          alt="poster"
          class="featured-img"
        />
        <div class="detail">
          <h3>Naruto</h3>
          <p>
            Lorem ipsum dolor sit amet, consectetur adipisicing elit. Provident,
            quaerat. Dolore quisquam quas molestias sunt soluta ex impedit?
          </p>
        </div>
      </router-link>
    </div>

    <form @submit.prevent="SearchMovies()" class="search-box">
      <input
        type="text"
        placeholder="What are you looking for?"
        v-model="search"
      />
      <input type="submit" value="search" />
    </form>
    <div class="movies-list">
      <div class="movie" v-for="movie in movies" :key="movie.imdbID">
        <router-link :to="'/movie/' + movie.imdbID" class="movie-link">
          <div class="product-image">
            <img :src="movie.Poster" alt="movie poster" />
            <div class="type">
              {{ movie.Type }}
            </div>
          </div>
          <div class="details">
            <p class="year">
              {{ movie.Year }}
            </p>
            <h3>
              {{ movie.Title }}
            </h3>
          </div>
        </router-link>
      </div>
    </div>
  </div>
</template>

<script>
import { ref } from "vue";
import env from "@/env.js";

export default {
  setup() {
    const search = ref("");
    const movies = ref([]);

    const SearchMovies = () => {
      if (search.value != "") {
        fetch(`http://www.omdbapi.com/?apikey=${env.apikey}&s=${search.value}`)
          .then((response) => response.json())
          .then((data) => {
            movies.value = data.Search;
            search.value = "";
            console.log(movies.value);
          });
      }
    };

    return {
      search,
      movies,
      SearchMovies,
    };
  },
};
</script>
<style lang="scss">
.home {
  .feature-card {
    position: relative;

    .featured-img {
      display: block;
      width: 100%;
      height: 300px;
      object-fit: cover;
      position: relative;
      z-index: 0;
    }
  }
  .detail {
    position: absolute;
    left: 0;
    right: 0;
    bottom: 0;
    background-color: rgba(0, 0, 0, 0.6);
    padding: 16px;
    z-index: 1;

    h3 {
      color: white;
      margin-bottom: 16px;
    }
    p {
      color: white;
    }
  }
  .search-box {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    padding: 16px;

    input {
      display: block;
      appearance: none;
      border: none;
      outline: none;
      background: none;

      &[type="text"] {
        width: 100%;
        color: #fff;
        background-color: #496583;
        font-size: 20px;
        padding: 10px 16px;
        border-radius: 15px;
        margin-bottom: 15px;
        transition: 0.4s;
      }
      &::placeholder {
        color: #f3f3f3;
      }
      &:focus {
        box-shadow: 0px 3px 6px rgba(0, 0, 0, 0.2);
      }

      &[type="submit"] {
        width: 100%;
        max-width: 300px;
        background-color: #428883;
        padding: 10px;
        border-radius: 8px;
        color: white;
        font-size: 14px;
        text-transform: uppercase;
        trasition: 0.4s;

        &:active {
          background-color: #388070;
        }
      }
    }
  }
  .movies-list {
    display: flex;
    flex-wrap: wrap;
    margin: 0px 8px;

    .movie {
      max-width: 50%;
      flex: 1 1 50%;
      padding: 16px 8px;

      .movie-link {
        display: flex;
        flex-direction: column;
        height: 100%;

        .product-image {
          position: relative;
          display: block;

          img {
            display: block;
            width: 100%;
            height: 275px;
            object-fit: cover;
          }
          .type {
            position: absolute;
            padding: 8px 16px;
            background-color: #428883;
            color: white;
            bottom: 16px;
            left: 0;
            text-transform: capitalize;
          }
        }
        .details {
          background-color: #496583;
          padding: 20px 8px;
          flex: 1 1 100%;
          border-radius: 0 0 8px 8px;

          .year {
            color: white;
            font-size: 16px;
          }

          h3 {
            color: white;
            font-weight: 600;
            font-size: 16px;
          }
        }
      }
    }
  }
}
</style>
