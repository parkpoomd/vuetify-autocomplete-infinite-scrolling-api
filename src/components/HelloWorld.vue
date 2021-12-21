<template>
  <v-container>
    <v-row class="text-center">
      <v-col cols="12">
        <h1 class="primary--text">AutoComplete Infinite Scroll Example</h1>
        <v-autocomplete
          v-model="selected"
          :items="beers"
          item-text="name"
          item-value="id"
          label="Search da beers.."
          return-object
          autocomplete="off"
        >
          <template v-slot:append-item>
            <div
              v-if="beers.length > 0"
              v-intersect="onIntersect"
              class="px-4 grey--text"
            >
              Loading more items ...
            </div>
          </template>
        </v-autocomplete>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import axios from 'axios'

export default {
  name: 'HelloWorld',

  data() {
    return {
      beers: [],
      selected: null,
      perPage: 10,
      page: 1,
    }
  },

  created() {
    this.getBeers()
  },

  methods: {
    getBeers() {
      console.log('page', this.page)
      const apiUrl = `https://api.punkapi.com/v2/beers?page=${this.page}&per_page=${this.perPage}`
      axios.get(apiUrl).then((response) => {
        this.beers = [...this.beers, ...response.data]
      })
    },
    onIntersect() {
      console.log('load more...')
      this.page += 1
      this.getBeers()
    },
  },
}
</script>
