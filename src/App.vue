<template>
  <div class="app">
    <header>
      <h1>The<strong>Anime</strong>Search</h1>

      <form class="search-box" @submit.prevent="handle_search">
        <input
          type="search"
          class="search-field"
          placeholder="Search for an anime..."
          required
          v-model="search_query"
        />
      </form>
    </header>

    <main>
      <div class="cards" v-if="anime_list.length > 0">
        <Card v-for="anime in anime_list" :key="anime.mal_id" :anime="anime" />
      </div>
      <div class="no-result" v-else>
        <h3>Sorry, we have no results...</h3>
      </div>
    </main>
  </div>
</template>

<script>
import { ref } from "vue";
import Card from "./components/Card";
export default {
  setup() {
    const search_query = ref("");
    const anime_list = ref([]);

    const handle_search = async () => {
      anime_list.value = await fetch(
        `https://api.jikan.moe/v3/search/anime?q=${search_query.value}`
      )
        .then((res) => res.json())
        .then((data) => data.results);

      search_query.value = "";
    };
    return { Card, search_query, handle_search, anime_list };
  },
};
</script>

<style lang="scss">
@import url("https://fonts.googleapis.com/css2?family=Fira+Sans:ital,wght@0,400;0,500;0,600;0,700;0,800;0,900;1,400;1,500;1,600;1,700;1,800;1,900&display=swap");
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: "Fira Sans", sans-serif;
}
a {
  text-decoration: none;
}
header {
  padding-top: 50px;
  padding-bottom: 50px;

  h1 {
    color: #888;
    font-size: 42px;
    font-weight: 400;
    text-align: center;
    text-transform: uppercase;
    margin-bottom: 40px;

    strong {
      color: #313131;
    }
    &:hover {
      color: #313131;
    }
  }

  .search-box {
    display: flex;
    justify-content: center;
    padding-left: 30px;
    padding-right: 30px;

    .search-field {
      appearance: none;
      background: none;
      outline: none;
      border: none;

      background-color: #f3f3f3;
      box-shadow: 0px 4px 8px rgba(0, 0, 0, 0.15);

      display: block;
      width: 100%;
      max-width: 600px;
      padding: 15px;
      border-radius: 8px;

      color: #313131;
      font-size: 20px;

      transition: 0.4s;

      &::placeholder {
        color: #aaa;
      }
      &:focus,
      &:valid {
        color: #fff;
        background-color: #313131;
        box-shadow: 0px 0px 0px rgba(0, 0, 0, 0.15);
      }
    }
  }
}

main {
  max-width: 800px;
  margin: 0 auto;
  padding-left: 30px;
  padding-right: 30px;

  .cards {
    display: flex;
    flex-wrap: wrap;
    margin: 0 -8px;
  }
}
</style>
