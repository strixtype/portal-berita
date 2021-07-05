<template>
  <v-row>
    <v-col v-for="(item, index) in newsData" :key="index" cols="4" mb-2>
      <v-card height="425">
        <v-img
          height="250"
          :lazy-src="item.urlToImage"
          :src="item.urlToImage"
        ></v-img>
        <v-card-title>
          {{ item.title }}
        </v-card-title>
        <v-card-actions
          class="pa-4"
          style="position: absolute; bottom: 0px; width: 100%;"
        >
          <v-spacer />
          <v-btn color="primary" :href="item.url" target="_blank">
            Read
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-col>
  </v-row>
</template>

<script>
export default {
  props: {
    newsData: {
      type: Array,
      default: []
    }
  }
};
</script>
