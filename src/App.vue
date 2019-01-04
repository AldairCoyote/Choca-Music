<template lang="pug">
  #app
      img(src='./assets/logo.png')
      h1 Vue Music
      select(v-model="selectedCountry")
        option(v-for="country in countries" :value="country.value") {{ country.name }}
      
      spinner(v-show="loading")
      ul
        artist(v-for="artist in artists"
        :artist="artist" :key="artist.mbid")

</template>

<script>
import artist from './components/artist.vue'
import spinner from './components/spinner.vue'
import getArtists from './api'
export default {
  name: 'app',
  data () {
    return {
      artists: [],
      countries: [
        {name:'Argentina', value:'argentina'},
        {name:'Mexico', value:'mexico'},
        {name:'Spain', value:'spain'}
      ],
      selectedCountry: 'argentina',
      loading: true
    }
  },
  components: {
    artist,
    spinner
  },
  methods: {
    refreshArtists() {
      const self = this
      this.artist = []
      this.loading = true
      getArtists(this.selectedCountry)
        .then(function (artists) {
          self.loading = false
          self.artists = artists
      })
    }
  },
  mounted() {
    this.refreshArtists()
  },
  watch: {
    selectedCountry() {
      this.refreshArtists()
    }
  }
}
</script>

<style lang="stylus">
#app
    font-family 'Avenir', Helvetica, Arial, sans-serif
    -webkit-font-smoothing antialiased
    -moz-osx-font-smoothing grayscale
    text-align center
    color #085689 !important
    margin-top 60px

h1, h2
    font-weight normal

ul
    list-style-type none
    padding 0

li
    display inline-block
    margin 0 10px

a
    color #42b983
</style>
