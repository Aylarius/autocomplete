<template lang
    input(type="t + .listext",v-model="query")
    .list
      .itemfor="serie in resultFilter", @click='select(serie.name)') {{serie.name}}
</template>

<script>
import debounce from 'lodash/debounce'
export default {
  data: function () {
    return {
      query:"",
      result: []
    }
  },
  computed: {
    resultFilter: function () {
      return this.result.splice(0,5)
    }
  },
  watch: {
    query: function () {
      if (this.query != '') {
        this.getMovie()
      } else {
        this.result = []
      }
    }
  },
  methods: {
    getMovie : debounce(function () {
      this.$http.get('https://api.themoviedb.org/3/search/tv?api_key=fc533e12b849e49e74ab5d046165bcc7&language=fr-FR&query='+ this.query)
        .then((response) => {
          console.log(response);
        this.result = response.data.results
      }).catch((error) => {
          console.log(error);
      });
    }, 300),
    select (value) {
        this.query = value
    }
  }
}
</script>

<style scoped>
  .list {
    background-color: white;
    box-shadow: 0 14px 28px rgba(0,0,0,0.25), 0 10px 10px rgba(0,0,0,0.22);
  }
  .list .item {
    padding: 5px;
  }
</style>
