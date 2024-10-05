<template>
  <div class="item">
 <h3>{{ detail['title'] }}</h3>      
 <p>{{ detail['summary'] }}</p>    
   
        <img
              v-if="detail['media']"
              v-bind:src="'https://media.nfsacollection.net/' + previewItems[0]['preview']['filePath']"
              v-bind:alt="detail['name']"
              v-bind:title="detail['name']"
            />
      
     
    </div>
  
</template>

<script>
export default {
  props: ['id'],
  data() {
    return {
      detail: [],  // Array to hold fetched users
      loading: true,  // Boolean to show loading state
      error: null,  // To capture any error that occurs
    };
  },
  // Vue lifecycle hook to fetch data when component is mounted
  created() {
    this.fetchDetail();
  },
  computed: {
    previewItems: function() {
      try {
        return this.detail['media'].filter(function(item) {
          return item['preview'] != null && item['preview']['filePath'] != null;
        });      
        }
    catch {}
    }
  },
  methods: {
    // Function to fetch users from API
    async fetchDetail() {
      try {
        console.log('https://api.collection.nfsa.gov.au/title/' + this.id)
        const response = await fetch('https://api.collection.nfsa.gov.au/title/' + this.id);
        if (!response.ok) {
          throw new Error('Failed to fetch data');
        }
        const data = await response.json();
        this.detail = data;  // Assign data to users
      } catch (error) {
        this.error = error.message;  // Handle any error
      } finally {
        this.loading = false;  // Turn off loading indicator
      }
    }
  }
};

</script>

<style scoped>
.item{
  margin-top: 100px;
  color: #3e3d3d;
  
}
body {
    display: none;
    place-items: start;
    background-color: #3E3D3D;
  }
img {
  padding-top: 20px;
  max-width: 100%;
  height: auto;
}
p {
  font-size: 20px;
}
@media screen and (max-width: 500px) {
  p {
    font-size: 10px;

  }
}
</style>
