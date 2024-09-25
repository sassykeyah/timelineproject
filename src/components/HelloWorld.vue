<script lang="ts">
// Use a Pinia store to hold the API results through the whole app
// Access the data by importing the store
import { useSearchDataStore } from '@/stores/searchData'
var store: any

function updateStore(newData: any) {
  // Define a variable to represent the store
  store = useSearchDataStore()
  // Then use store.theStoredData to get/set the collection
  store.theStoredData = newData
  return {
    theStoredData: store.theStoredData
  }
}
// var theCollection = store.theStoredData
export default {
 name: 'HelloWorld',
 props: {
 msg: String
 },
 data() {
  return {
    theData: {},
    tempData: {},
    resultSet: [],
    tempResultSet: [],
    currentPage: 1,
    total: 0,
    imgURL: 'https://media.nfsacollection.net/',
    query: 'https://api.collection.nfsa.gov.au/search?limit=25&query=',
    searchString: 'lobby'
  }

  },
  methods: {
    fetchData() {
      // use dynamic data to modify the API call
      // in this case we use a text box which sets searchString
      // and the currentPage to allow us to loop through the paginated results
      let queryString = this.query + this.searchString + '&page=' + this.currentPage
      console.log('API call: ' + queryString)
      fetch(queryString)
        .then((response) => {
          response.json().then((res) => {
            // build a temporary object, add the data from the current page on each call of fetchData()
            this.$data.tempData = { ...this.$data.tempData, ...res }
            // the same as above but with just the results array instead of the whole data object
            this.$data.tempResultSet = this.$data.tempResultSet.concat(res.results)
            // total items from the meta object (total number of items found in the search)
            this.$data.total = res.meta.count.total
            // if there are items
            if (this.$data.total > 0) {
              // check how many pages of results @ 25 per page
              if (this.currentPage * 25 < 500 && this.currentPage * 25 < this.$data.total) {
                // go to the next page
                this.currentPage++
                // call this function on itself (recursive) ! be careful, this can cause an infinite loop
                this.fetchData()
              } else {
                this.$data.theData = this.$data.tempData
                this.$data.tempData = {}
                this.$data.resultSet = this.$data.tempResultSet
                this.$data.tempResultSet = []
                // all items loaded, reset page, ready for next query
                this.currentPage = 1
                console.log('Pages: ' + Math.ceil(this.$data.total / 25))
                console.log('finished')
                updateStore(this.$data.resultSet)
              }
            } else {
              console.log('no results')
            }
          })
        })
        .catch((err) => {
          console.error(err)
        })
    }
  }
}
</script>


<template>
  <div class="search">
    <h1 class="white">{{ msg }}</h1>

    <input v-model="searchString" placeholder="query" />
    <button @click="fetchData">fetch data</button>

    <p>Total: {{ total }}</p>

    <ul role="list" class="list-v">
      <li v-for="(result, index) in resultSet" :key="result[index]">
        <p>{{ result['title'] }}</p>
        <p>{{ result['name'] }}</p>
        <img
          v-if="result['preview'] && result['preview'][0]"
          v-bind:src="imgURL + result['preview'][0]['filePath']"
          v-bind:alt="result['name']"
          v-bind:title="result['name']"
        />
      </li>
    </ul>
  </div>
</template>

<style scoped>
h1 {
  font-weight: 500;
  font-size: 2.6rem;
  position: relative;
  top: -10px;
  text-align: center;
}
.n1 {
  font-weight: 500;
  font-size: 1.2rem;
  position: relative;
  top: -10px;
  text-align: center;
}

h3 {
  font-size: 1.2rem;
}

.greetings h1,
.greetings h3 {
  text-align: center;
}

@media (min-width: 1024px) {
  .greetings h1,
  .greetings h3 {
    text-align: left;
  }
}
</style>
