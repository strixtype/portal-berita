<template>
  <div>
    <v-container class="pa-6">
      <v-row>
        <h2>Top Headlines</h2>
      </v-row>
    </v-container>
    <v-container class="pa-6">
      <v-row>
        <v-btn
          v-for="(item, index) in category"
          :key="index"
          class="ma-2"
          :color="
            `primary ${item.value === categorySelected ? 'darken-4' : ''}`
          "
          @click="categorySelected = item.value"
          >{{ item.label }}</v-btn
        >
      </v-row>
    </v-container>
    <v-container class="pa-6">
      <news-item :news-data="news" />

      <v-row justify="center" v-if="totalResult !== 0">
        <v-col cols="8">
          <v-container class="max-width">
            <v-pagination
              v-model="page"
              class="my-4"
              :length="Math.ceil(totalResult / pageSize)"
            ></v-pagination>
          </v-container>
        </v-col>
      </v-row>
    </v-container>
  </div>
</template>

<script>
import newsItem from "../components/news";
export default {
  components: {
    newsItem
  },

  data: () => ({
    isLoading: false,
    category: [
      {
        label: "General",
        value: "general"
      },
      {
        label: "Business",
        value: "business"
      },
      {
        label: "Entertainment",
        value: "entertainment"
      },
      {
        label: "Health",
        value: "health"
      },
      {
        label: "Science",
        value: "science"
      },
      {
        label: "Sports",
        value: "sports"
      },
      {
        label: "Technology",
        value: "technology"
      }
    ],
    categorySelected: "general",
    news: [],
    pageSize: 24,
    page: 1,
    totalResult: 0
  }),

  methods: {
    getNewsData() {
      this.isLoading = true;
      //const apiKey = "450614c49e6d4579a2fa378945cda4c0";
      const apiKey = "";
      fetch(
        `https://newsapi.org/v2/top-headlines?country=id&category=${this.categorySelected}&page=${this.page}&pageSize=${this.pageSize}&apiKey=${apiKey}`
      )
        .then(res => {
          return res.json();
        })
        .then(result => {
          console.log(result);
          this.news = result.articles;
          this.totalResult = result.totalResults;
          this.isLoading = false;
        });
    }
  },

  watch: {
    categorySelected() {
      this.page = 1;
      this.getNewsData();
    }
  },

  mounted() {
    this.getNewsData();
  }
};
</script>
