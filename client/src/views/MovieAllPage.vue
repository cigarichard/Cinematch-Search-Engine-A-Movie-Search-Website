<template>
    <div class="row gy-5">
      <div class="col-10">
        <div class="bg-grey">
          <h4 class="abc">Title</h4>
          <div class="rightright">
            <router-link :to="`/search?Title=${search_params}`">
              <h4 class="def">More</h4>
            </router-link>
          </div>
          <movie-card-container
            :movies="title_movies"
            v-if="this.title_movies != null"
          ></movie-card-container>
          <div class="card" v-if="this.title_movies == null" style="color: red">
            No result
          </div>
        </div>
      </div>
      <div class="col-10">
        <div class="bg-grey">
          <h4 class="abc">Celebrity</h4>
          <div class="rightright">
            <router-link :to="`/search?Celebrity=${search_params}`">
              <h4 class="def">More</h4>
            </router-link>
          </div>
          <movie-card-container
            :movies="celes_movies"
            v-if="this.celes_movies != null"
          ></movie-card-container>
          <div class="card" v-if="this.celes_movies == null" style="color: red">
            No result
          </div>
        </div>
      </div>
      <div class="col-10">
        <div class="bg-grey">
          <h4 class="abc">Genre</h4>
          <div class="rightright">
            <router-link :to="`/search?Genre=${search_params}`">
              <h4 class="def">More</h4>
            </router-link>
          </div>
          <movie-card-container
            :movies="genre_movies"
            v-if="this.genre_movies != null"
          ></movie-card-container>
          <div class="card" v-if="this.genre_movies == null" style="color: red">
            No result
          </div>
        </div>
      </div>
      <div class="col-10">
        <div class="bg-grey">
          <h4 class="abc">Year</h4>
          <div class="rightright">
            <router-link :to="`/search?Year=${search_params}`">
              <h4 class="def">More</h4>
            </router-link>
          </div>
          <movie-card-container
            :movies="year_movies"
            v-if="this.year_movies != null"
          ></movie-card-container>
          <div class="card" v-if="this.year_movies == null" style="color: red">
            No result
          </div>
        </div>
      </div>
    </div>
</template>

<script>
import MovieCardContainer from "../components/MovieCardContainer.vue";
import MovieCard from "../components/MovieCard.vue";

export default {
  name: "MovieAllPage",
  components: {
    MovieCardContainer,
    MovieCard,
  },
  created: function () {
    console.log("MovieAllPage created");
    this.$watch(
      () => this.$route.query.All,
      (newQuery, oldQuery) => {
        console.log(`${oldQuery}->${newQuery}`);
        if (newQuery != undefined) {
          this.search_params = newQuery;
          this.search();
        }
      },
      { immediate: true }
    );
  },
  data: function () {
    return {
      api: "api/movies/search",
      title_movies: null,
      celes_movies: null,
      genre_movies: null,
      year_movies: null,
      search_params: "",
    };
  },
  methods: {
    search() {
      console.log("Search");
      window.scrollTo(0, 0);
      fetch(`${this.api}?All=${this.search_params}`)
        .then((response) => response.json())
        .then((data) => {
          if (data.response == "success") {
            this.title_movies = data.movies.title;
            this.celes_movies = data.movies.celebrity;
            this.genre_movies = data.movies.genre;
            this.year_movies = data.movies.year;
          } else {
            this.title_movies = "fail";
          }
        });
    },
  },
};
</script>

<style scoped>
.container {
  left: 4%;
  position: relative;
}

.abc {
  color: rgb(255, 215, 0);
}

.def {
  color: #ffd4a9;
}

.leftleft {
  float: left;
  display: inline;
}

.rightright {
  float: right;
  display: inline;
}

.card {
  background-color: rgb(128, 209, 200);
}
</style>