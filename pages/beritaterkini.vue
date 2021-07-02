<template>
  <div>
    <v-container class="pa-6">
      <v-row>
        <h2>Berita Terkini</h2>
      </v-row>
    </v-container>
    <v-container class="pa-6">
      <v-row>
        <v-col v-for="(item, index) in news" :key="index" cols="4" mb-2>
          <v-card>
            <v-img
              height="250"
              :lazy-src="item.urlToImage"
              :src="item.urlToImage"
            ></v-img>
            <v-card-title>
              {{ item.title }}
            </v-card-title>
            <v-card-actions>
              <v-spacer />
              <v-btn
                color="primary"
                elevation="2"
                href="item.url"
                target="_blank"
              >
                Baca
              </v-btn>
            </v-card-actions>
          </v-card>
        </v-col>
      </v-row>
    </v-container>
  </div>
</template>

<script>
export default {
  data() {
    return {
      keyword: "",
      isShowing: false,
      isLoading: false,
      news: []
    };
  },

  methods: {
    getNewsData() {
      this.isLoading = true;
      const apiKey = "";
      const topic = "covid";
      fetch(`https://newsapi.org/v2/top-headlines?country=id&apiKey=${apiKey}`)
        .then(res => {
          return res.json();
        })
        .then(result => {
          console.log(result);
          this.news = result.articles;
          this.isLoading = false;
        });
    }
  },

  mounted() {
    this.getNewsData();
  }
};
</script>
