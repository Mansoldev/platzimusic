<template lang="pug">
  #app
    header.flex-header
      img.logo(src='dist/logo.png')
      h1 PlatziMusic
      div
        p Selecciona el País:
        select(v-model="selectedCountry")
          option(v-for="country in countries" :value="country.value") {{ country.name }}
    spinner(v-show="loading")
    ul.flex-galery
      artist(v-for="artist in artists" v-bind:artist="artist" v-bind:key="artist.mbid") {{ artist.name }}
</template>

<script>
import Artist from './components/Artist.vue'
import Spinner from './components/Spinner.vue'
import getArtists from './api'

export default {
  name: 'app',
  data () {
    return {
      artists: [],
      countries: [
        {name: 'España', value: 'spain'},
        {name: 'Colombia', value: 'colombia'},
        {name: 'Argentina', value: 'argentina'}
      ],
      selectedCountry: 'spain',
      loading: true
    }
  },
  components: {
    Artist,
    Spinner
  },
  methods: {
    refreshArtists() {
      const self = this
      this.loading = true
      getArtists(this.selectedCountry)
        /*.then(function(artists) {
          self.artists = artists
          self.loading = false
        })*/
        .then(artists => self.artists = artists)
        .catch(error => console.error(error))
        .finally(() => self.loading = false );
    }
  },
  mounted: function () {
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
    color #2c3e50
    margin-top 20px

.flex-header
    max-width 500px
    margin 0 auto
    display -ms-flexbox
    display -webkit-flex
    display flex
    -webkit-flex-direction row
    -ms-flex-direction row
    flex-direction row
    -webkit-flex-wrap nowrap
    -ms-flex-wrap nowrap
    flex-wrap nowrap
    -webkit-justify-content space-around
    -ms-flex-pack distribute
    justify-content space-around
    -webkit-align-content flex-start
    -ms-flex-line-pack start
    align-content flex-start
    -webkit-align-items center
    -ms-flex-align center
    align-items center

.logo
    width 80px

h1, h2
    font-weight normal

.flex-galery
    display: -ms-flexbox;
    display: -webkit-flex;
    display: flex;
    -webkit-flex-direction: row;
    -ms-flex-direction: row;
    flex-direction: row;
    -webkit-flex-wrap: wrap;
    -ms-flex-wrap: wrap;
    flex-wrap: wrap;
    -webkit-justify-content: space-around;
    -ms-flex-pack: distribute;
    justify-content: space-around;
    -webkit-align-content: flex-start;
    -ms-flex-line-pack: start;
    align-content: flex-start;
    -webkit-align-items: flex-start;
    -ms-flex-align: start;
    align-items: flex-start;

ul
    list-style-type none
    padding 0

li
    display inline-block

a
    color #42b983
</style>
