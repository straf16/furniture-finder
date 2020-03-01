<template>
  <div id="home">
    <TopBar @submitKeyword="filterSearch" :items="products.category"/>
    <v-container fluid class="grey lighten-5">
      <v-col cols="12">
        <v-row
          align="center"
          class="grey lighten-5"
        >
          <v-card
            v-for="(item, index) in filteredProducts"
            :key="index"
            class="ma-2 pa-2"
            max-width="420"
            outlined
            tile
          >
            <v-card-title>
              <h4 class="title">{{ item.name }}</h4>
              <v-spacer></v-spacer>
              <span class="subtitle-1">Rp. {{ formatPrice(item.price) }}</span>
            </v-card-title>
            <v-list-item three-line>
              <v-list-item-content>
                <v-list-item-subtitle>{{ item.description }}</v-list-item-subtitle>
              </v-list-item-content>
            </v-list-item>
            <v-chip-group mandatory style="padding: 0 13px;">
              <v-chip
                v-for="(styles, i) in item.furniture_style"
                :key="i"
                label
              >
                {{ styles }}
              </v-chip>
            </v-chip-group>
            <v-card-text style="text-align: right">
              <p>Delivery Days: {{ item.delivery_time }}</p>
            </v-card-text>
          </v-card>
        </v-row>
      </v-col>
    </v-container>
  </div>
</template>

<script>
// @ is an alias to /src
import axios from 'axios'

import TopBar from '../components/TopBar'

export default {
  name: 'Home',
  components: {
    TopBar
  },
  data () {
    return {
      filter: {
        keyword: '',
        category: [],
        time: ''
      },
      products: {
        category: [],
        items: []
      }
    }
  },
  computed: {
    filteredProducts: function () {
      const filtered = this.products.items.map(obj => obj)
      if (!this.filter.keyword && this.filter.category.length === 0 && !this.filter.time) {
        return this.products.items
      }
      for (let i = 0; i < filtered.length; i++) {
        let check = true
        if (this.filter.category) {
          for (let j = 0; j < this.filter.category.length; j++) {
            if (filtered[i].furniture_style.indexOf(this.filter.category[j]) === -1) {
              check = false
              break
            }
          }
        }
        if (this.filter.keyword && filtered[i].name.toUpperCase().indexOf(this.filter.keyword.toUpperCase()) === -1) {
          check = false
        }
        if (this.filter.time) {
          switch (this.filter.time) {
            case '1 week':
              if (filtered[i].delivery_time > 7) {
                check = false
              }
              break
            case '2 week':
              if (filtered[i].delivery_time <= 7 || filtered[i].delivery_time > 14) {
                check = false
              }
              break
            case '1 month':
              if (filtered[i].delivery_time <= 14 || filtered[i].delivery_time > 30) {
                check = false
              }
              break
            default:
              if (filtered[i].delivery_time <= 30) {
                check = false
              }
              break
          }
        }
        if (!check) {
          filtered.splice(i, 1)
          i--
        }
      }
      return filtered
    }
  },
  methods: {
    fetchProducts: function () {
      axios({
        method: 'GET',
        url: 'http://www.mocky.io/v2/5c9105cb330000112b649af8'
      })
        .then(({ data }) => {
          this.products.category = data.furniture_styles
          this.products.items = data.products
        })
        .catch(({ response }) => {
          console.log(response)
        })
    },
    formatPrice: function (value) {
      const val = (value / 1).toFixed(2).replace('.', ',')
      return val.toString().replace(/\B(?=(\d{3})+(?!\d))/g, '.')
    },
    filterSearch: function (filter) {
      this.filter = filter
    }
  },
  created () {
    this.fetchProducts()
  }
}
</script>

<style>
.v-card {
  margin: 10px;
}
.v-list--three-line .v-list-item, .v-list-item--three-line {
  min-height: 40px;
}
</style>
