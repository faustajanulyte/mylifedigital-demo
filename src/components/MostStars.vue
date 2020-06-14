<template>
  <div>
    <div class="container">
      <input class="form-control my-4" v-model="language" placeholder="Language" />
      <button type="submit" class="btn bg-dark text-light" @click="searchRepos">Search Repos</button>
      <div class="shadow p-3 mb-5 bg-white rounded my-4 border">
        <h1 class="font-weight-bold">Most Stars: {{ language }}</h1>
        <p class="font-italic">Repos created since {{simpleDate(today())}}</p>
        <div v-for="item in items" v-bind:key="item.id" class="card my-2">
          <div class="card-body">
            <h4 class="card-title">
              <a :href="item.html_url">{{ item.full_name }}</a>
            </h4>
            <div class="card-text mb-2 font-weight-bold">{{ item.description }}</div>
            <p class="card-text">Created: {{ simpleDate(item.created_at) }}</p>
            <svg
              class="bi bi-star-fill"
              width="1em"
              height="1em"
              viewBox="0 0 16 16"
              fill="currentColor"
              xmlns="http://www.w3.org/2000/svg"
            >
              <path
                d="M3.612 15.443c-.386.198-.824-.149-.746-.592l.83-4.73L.173 6.765c-.329-.314-.158-.888.283-.95l4.898-.696L7.538.792c.197-.39.73-.39.927 0l2.184 4.327 4.898.696c.441.062.612.636.283.95l-3.523 3.356.83 4.73c.078.443-.36.79-.746.592L8 13.187l-4.389 2.256z"
              />
            </svg>
            <p class="card-text">{{ item.stargazers_count }}</p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      language: "",
      items: []
    };
  },
  methods: {
    async searchRepos() {
      let promise = axios.get("https://api.github.com/search/repositories?q=language:" + this.language + "+created:>" + this.today() + "&sort=stars&order=desc");
      let resp = await promise;

      this.items = [];
      for (let i = 0; i < 3; i++) {
        this.items.push(resp.data.items[i]);
      }
    },
    today() {
      var today = new Date();
      today.setMonth(today.getMonth() - 1);
      var dd = String(today.getDate()).padStart(2, "0");
      var mm = String(today.getMonth() + 1).padStart(2, "0");
      var yyyy = today.getFullYear();
      return yyyy + "-" + mm + "-" + dd;
    },
    simpleDate(date) {
      const monthNames = [
        "January",
        "February",
        "March",
        "April",
        "May",
        "June",
        "July",
        "August",
        "September",
        "October",
        "November",
        "December"
      ];

      let simpleDate = new Date(date);
      return String(simpleDate.getDate()).padStart(2, "0") + " " + monthNames[simpleDate.getMonth()] + " " + simpleDate.getFullYear();
    }
  }
};
</script>

<style scoped>
</style>
