<template>
  <div id="topbar">
    <v-app-bar
      color="blue"
      dark
    >
      <v-toolbar-title id="title">Furniture Finder</v-toolbar-title>
      <v-spacer></v-spacer>

      <v-form
        @submit.prevent="submitKeyword"
        style="display: flex; flex-direction: row; width: 55vw;"
      >
        <v-text-field
          placeholder="Search Furniture"
          v-model="keyword"
          style="max-width: 20vw; margin-top: 17px; padding: 5px;"
        ></v-text-field>

        <v-select
          v-model="select"
          :items="items"
          label="Furniture Styles"
          dense
          multiple
          style="max-width: 20vw; margin-top: 21px; padding: 5px;"
        >
          <template v-slot:selection="{ item, index }">
            <v-chip
              v-if="index === 0"
              label
              x-small
              outlined
            >
              <span>{{ item }}</span>
            </v-chip>
            <span
              v-if="index === 1"
              class="white--text caption"
            >(+{{ select.length - 1 }} others)</span>
          </template>
        </v-select>
        <v-select
          v-model="time"
          :items="times"
          label="Delivery Time"
          style="max-width: 13vw; margin-top: 17px; padding: 5px;"
        ></v-select>
        <v-btn type="submit" icon class="mt-3">
          <v-icon>mdi-magnify</v-icon>
        </v-btn>
      </v-form>
    </v-app-bar>
  </div>
</template>

<script>
export default {
  name: 'Topbar',
  props: [
    'items'
  ],
  data () {
    return {
      keyword: '',
      select: [],
      time: '',
      times: [
        '1 week',
        '2 week',
        '1 month',
        'more'
      ]
    }
  },
  methods: {
    submitKeyword () {
      this.$emit('submitKeyword', {
        keyword: this.keyword,
        category: this.select,
        time: this.time
      })
    }
  }
}
</script>

<style>
#topbar {
  max-height: auto;
}
</style>
