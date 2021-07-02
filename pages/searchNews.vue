gir<template>
  <div>
    <v-container class="pa-6">
      <v-row>
        <h2>Search News</h2>
      </v-row>
    </v-container>
    <v-container class="pa-6">
      <v-row>
        <v-col cols="2">
          <v-select
            :items="searchType"
            label="search by"
            @change="selectSearchType"
          ></v-select>
        </v-col>
        <v-col cols="3">
          <v-text-field v-model="search" :label="searchLabel"></v-text-field>
        </v-col>
        <v-col cols="2">
          <v-select
            :items="dropdown_sort"
            item-text="label"
            item-value="value"
            label="sort by"
            @change="applySort"
          ></v-select>
        </v-col>
        <v-col cols="1" class="pt-6">
          <v-btn @click="getNewsData()" color="primary">Search</v-btn>
        </v-col>
      </v-row>
    </v-container>
    <v-container>
      <template v-if="isLoading">
        <v-row>
          <v-col cols="4" v-for="(loader, index) in 6" :key="index">
            <v-skeleton-loader class="mx-auto" type="card"></v-skeleton-loader>
          </v-col>
        </v-row>
      </template>

      <news-item v-else :news-data="news" />

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
    search: "",
    searchType: [
      {
        text: "keyword",
        value: "keyword"
      },
      {
        text: "news source",
        value: "source"
      }
    ],
    searchTypeSelected: "keyword",
    dropdown_sort: [
      {
        id: 1,
        label: "Popularity",
        value: "popularity"
      },
      {
        id: 2,
        label: "Newest",
        value: "publishedAt"
      }
    ],
    sortSelected: "publishedAt",
    news: [],
    pageSize: 24,
    page: 1,
    totalResult: 0
  }),

  computed: {
    searchLabel() {
      return this.searchTypeSelected === "keyword"
        ? "keyword"
        : "news source, ex: abcnews.com";
    }
  },

  methods: {
    selectSearchType(current) {
      this.searchTypeSelected = current;
    },

    applySort(current) {
      this.sortSelected = current;
    },

    getNewsData() {
      this.isLoading = true;
      const apiKey = "450614c49e6d4579a2fa378945cda4c0";
      //const apiKey = "";
      const searchKey = this.searchTypeSelected === "keyword" ? "q" : "domains";

      const queryParams = [
        {
          key: searchKey,
          value: this.search
        },
        {
          key: "sortBy",
          value: this.sortSelected
        },
        {
          key: "page",
          value: this.page
        },
        {
          key: "pageSize",
          value: this.pageSize
        },
        {
          key: "apiKey",
          value: apiKey
        }
      ];

      let params = "";
      queryParams.forEach((item, index) => {
        params += `${item.key}=${item.value}${
          index !== queryParams.length - 1 ? "&" : ""
        }`;
      });

      fetch(`https://newsapi.org/v2/everything?${params}`)
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
    page() {
      this.getNewsData();
    }
  }
};
</script>
