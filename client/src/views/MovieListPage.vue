<template>
  <div class="row gy-5">
    <div class="col-12">
      <movie-card-container :movies="movies"></movie-card-container>
      <pagination
        :total_page="total_page"
        :current_page="current_page"
        @changePage="change_page($event)"
      ></pagination>
    </div>
  </div>
</template>

<script>
import MovieCardContainer from "../components/MovieCardContainer.vue";
import MovieCard from "../components/MovieCard.vue";
import Pagination from "../components/Pagination.vue";

export default {
  name: "MovieListPage",
  components: {
    MovieCardContainer,
    MovieCard,
    Pagination,
  },
  created: function () {
    console.log("MovieListPage created");
    this.$watch(
      () => this.$route.query,
      (newQuery, oldQuery) => {
        this.search_params = {};
        const query = this.$route.query;
        if (Object.keys(query).length != 0) {
          for (let key in query) {
            this.search_params[key] = query[key];
          }
          this.search();
        }
      },
      { immediate: true }
    );
  },
  data: function () {
    return {
      api: "/api/movies/search",
      movies: null,
      total_number: null,
      current_page: 1,
      search_params: {},
      movies_per_page: 20,
    };
  },
  methods: {
    change_page(page) {
      window.scrollTo(0, 0);
      this.search_params["page"] = page;
      const searchURL = new URLSearchParams(this.search_params);
      console.log(`${this.api}?${searchURL}`);
      fetch(`${this.api}?${searchURL}`)
        //   fetch(`/api/movies/search?${searchURL}`)
        .then((response) => response.json())
        .then((data) => {
          console.log(data.response);
          const movies = data.movies;
          console.log(movies.length);
          this.movies = movies;
          this.current_page = data.current_page;
        });
    },
    search() {
      if (this.search_params.All == undefined) {
        const searchURL = new URLSearchParams(this.search_params);
        console.log(`${this.api}?${searchURL}`);
        window.scrollTo(0, 0);
        fetch(`${this.api}?${searchURL}`)
          .then((response) => response.json())
          .then((data) => {
            if (data.response == "success") {
              this.movies = data.movies;
              this.total_number = data.total_number;
              this.current_page = data.current_page;
            } else {
              this.$router.replace({ path: "/notfound/1" });
            }
          });
      }
    },
  },
  computed: {
    total_page() {
      return Math.ceil(this.total_number / this.movies_per_page);
    },
  },
};
</script>

<style>
</style>