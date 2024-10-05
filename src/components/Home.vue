<script lang="ts">

export default {
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
    query: 'https://api.collection.nfsa.gov.au/search?limit=25&hasMedia=yes&year=1970-2000&query=',
    searchString: 'women',
    selectedCategories: []
  }
  },
  computed: {
    filteredItems() {
      if (this.selectedCategories.length === 0) {
        return this.resultSet // Return all items if no category is selected
      }
      return this.resultSet.filter(
        (item) =>
          (item['forms'] && this.selectedCategories.includes(item['forms'][0])) ||
          (item['countries'] && this.selectedCategories.includes(item['countries'][0])) ||
          (item['parentTitle'] &&
            item['parentTitle']['genres'] &&
            this.selectedCategories.includes(item['parentTitle']['genres'][0]))
      )
    }
  },
  // render data without user input
  mounted() {
    this.fetchData();
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
                
              }
            } else {
              console.log('no results')
            }
          })
          
        })
        .catch((err) => {
          console.error(err)
        })
    },
    clearResults() {
      this.$data.resultSet = []
    }
  }
}
</script>


<template>
  
  <div class="search">
    <!-- In vue, loop through API data -->
    <div class="timeline" v-for="(result, index) in filteredItems" :key="result[index]">
       <!-- Use modulo function to return left or right containers, depending on row index being odd or even-->
      <div v-if="index % 2 === 0" class="container left">
         <!-- Return timeline card data-->
        <div class="content">
          <RouterLink :to="{ name: 'about', params: { id: result['id'] }}">
          <h2>{{ result['productionDates'][0]['fromYear'] }}</h2>
          <p>{{ result['name'] }} </p>
          <Transition>
            
            <img
              v-if="result['preview'] && result['preview'][0]"
              v-bind:src="imgURL + result['preview'][0]['filePath']"
              v-bind:alt="result['name']"
              v-bind:title="result['name']"
            />
          </Transition>
        </RouterLink>
        </div>
      </div>
       <!-- See comments above-->
      <div v-if="index % 2 === 1" class="container right">
        <div class="content">
          <RouterLink :to="{ name: 'about', params: { id: result['id'] }}">
          <h2>{{ result['productionDates'][0]['fromYear'] }}</h2>
          <p>{{ result['name'] }}</p>
          <Transition>
            <img
              v-if="result['preview'] && result['preview'][0]"
              v-bind:src="imgURL + result['preview'][0]['filePath']"
              v-bind:alt="result['name']"
              v-bind:title="result['name']"
            />
          </Transition>
        </RouterLink>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
img {
  width: 70%;
  height: 70%;
  object-fit: contain;
}

.search{
  margin-top: 100px;
  
}

/* The actual timeline (the vertical ruler) */
.timeline {
  position: relative;
  max-width: 1200px;
  margin: 0 auto;
}

/* The actual timeline (the vertical ruler) */
.timeline::after {
  content: '';
  position: absolute;
  width: 6px;
  background-color:#3E3D3D;
  top: 0;
  bottom: 0;
  left: 50%;
  margin-left: -3px;
}

/* Container around content */
.container {
  padding: 10px 40px;
  position: relative;
  background-color: inherit;
  width: 500px;
}

/* The circles on the timeline */
.container::after {
  content: '';
  position: absolute;
  width: 25px;
  height: 25px;
  right: -13px;
  background-color: #3E3D3D;
  border: 4px solid #C0C6B8;
  top: 15px;
  border-radius: 50%;
  z-index: 1;
}

/* Place the container to the left */
.left {
  left: -50%;
}

/* Place the container to the right */
.right {
  left: 50%;
}

/* Add arrows to the left container (pointing right) */
.left::before {
  content: " ";
  height: 0;
  position: absolute;
  top: 22px;
  width: 0;
  z-index: 1;
  right: 30px;
  border: medium solid #3E3D3D;
  border-width: 10px 0 10px 10px;
  border-color: transparent transparent transparent #3E3D3D;
}

/* Add arrows to the right container (pointing left) */
.right::before {
  content: " ";
  height: 0;
  position: absolute;
  top: 22px;
  width: 0;
  z-index: 1;
  left: 30px;
  border: medium solid #3E3D3D;
  border-width: 10px 10px 10px 0;
  border-color: transparent #3E3D3D transparent transparent;
}

/* Fix the circle for containers on the right side */
.right::after {
  left: -12px;
}

/* The actual content */
.content {
  padding: 20px 30px;
  background-color: #C0C6B8;
  position: relative;
  border-radius: 6px;
  color:#3E3D3D;
}

/* Media queries - Responsive timeline on screens less than 600px wide */
@media screen and (max-width: 1024px) {
/* Place the timelime to the left */
  .timeline::after {
    content: '';
  position: absolute;
  width: 6px;
  background-color:#3E3D3D;
  top: 0;
  bottom: 0;
  left: 28px;
  margin-left: -3px;
  }

/* Full-width containers */
  .container {
    width: 500px;
    padding-left: 70px;
    padding-right: 25px;
  }

/* Make sure that all arrows are pointing leftwards */
  .container::before {
    left: 60px;
    border: medium solid #3E3D3D;
  border-width: 10px 10px 10px 0;
  border-color: transparent #3E3D3D transparent transparent;
  }

/* Make sure all circles are at the same spot */
  .left::after, .right::after {
    left: 12px;
  }

  .left {
  left: 5px;
}

/* Make all right containers behave like the left ones */
  .right {
    left: 5px;
  }
}
/* media query for phones/small form factors*/
@media screen and (max-width: 600px) {


/* Full-width containers */
  .container {
    width: 300px;
   }
}
</style>
