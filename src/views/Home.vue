<template>
  <main v-if="!loading">
    <DataTitle :text="title" :dataDate="dataDate"/>
    <DataBoxes :stats="stats"/>
  </main>

  <main class="flex flex-col align-center justify-center text-center" v-else>
    <div class="text-gray-500 text-3xl mt-10 mb-6">
      Fetching Data
    </div>
    <img :src="loadingImage" class="w-24 m-auto" alt="Loading icon">
  </main>
</template>

<script>
// @ is an alias to /src
import DataTitle from '@/components/DataTitle'
import DataBoxes from '@/components/DataBoxes'

export default {
  name: 'Home',
  components: {
    DataTitle, DataBoxes
  },
  data() {
    return {
      loading: true,
      title: 'Global',
      dataDate: '',
      stats: {},
      countries: [],
      loadingImage: require('../assets/hourglass.gif')
    }
  },
  methods: {
    async fetchCovidData(){
      const res = await fetch('https://api.covid19api.com/summary')
      const data = await res.json()
      return data
    }
  },
  async created(){
    const data = await this.fetchCovidData()

    this.dataDate = data.Date
    this.stats = data.Global
    this.countries = data.countries
    this.loading = false
  }
}
</script>
