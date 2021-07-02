<template>
  <div>
    <v-container class="pa-6">
      <v-row>
        <h2>Cari Berita</h2>
      </v-row>
    </v-container>
    <v-container class="pa-6">
      <v-row>
        <v-col class="pt-8">
          <input type="text" v-model="search" placeholder="Cari" />
          <v-btn @click="getNewsData()" color="primary" elevation="2"
            >Cari</v-btn
          >
        </v-col>
        <v-col>
          <v-overflow-btn
            :items="dropdown_sort"
            label="Urutkan Berdasarkan"
            item-value="text"
          ></v-overflow-btn>
        </v-col>
        <v-col>
          <v-overflow-btn
            :items="dropdown_source"
            label="Sumber Berita"
            item-value="text"
          ></v-overflow-btn>
        </v-col>
      </v-row>
    </v-container>
    <v-container>
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
            <v-spacer></v-spacer>
            <v-btn
              color="primary"
              elevation="2"
              href="item.url"
              target="_blank"
            >
              Baca
            </v-btn>
          </v-card>
        </v-col>
      </v-row>
    </v-container>
  </div>
</template>

<script>
export default {
  data: () => ({
    dropdown_sort: [
      {
        id: 1,
        text: "Popularitas"
      },
      {
        id: 2,
        text: "Terbaru"
      }
    ],
    dropdown_source: [
      {
        id: 1,
        text: "BBC News"
      },
      {
        id: 2,
        text: "CNN Indonesia"
      },
      {
        id: 3,
        text: "Kompas"
      },
      {
        id: 4,
        text: "Radar"
      }
    ],
    news: []
  }),

  methods: {
    getNewsData() {
      this.isLoading = true;
      const apiKey = "";
      const topic = search;
      fetch(`https://newsapi.org/v2/everything?q=${topic}&apiKey=${apiKey}`)
        .then(res => {
          return res.json();
        })
        .then(result => {
          console.log(result);
          this.news = result.articles;
          this.isLoading = false;
        });
    }
  }
};
</script>
