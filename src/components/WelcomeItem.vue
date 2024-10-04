<template>
  <div class="item">
    <i>
      <slot name="icon"></slot>
    </i>
    <div class="details">
      <h3>
        <slot name="heading">n: {{ id }} {{ previewItems[0] }}</slot>
        <img
              v-if="detail['media']"
              v-bind:src="'https://media.nfsacollection.net/' + previewItems[0]['preview']['filePath']"
              v-bind:alt="detail['name']"
              v-bind:title="detail['name']"
            />
      </h3>
      <slot></slot>
    </div>
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
.item {
  margin-top: 2rem;
  display: flex;
  position: relative;
}

.details {
  flex: 1;
  margin-left: 1rem;
}

i {
  display: flex;
  place-items: center;
  place-content: center;
  width: 32px;
  height: 32px;

  color: var(--color-text);
}

h3 {
  font-size: 1.2rem;
  font-weight: 500;
  margin-bottom: 0.4rem;
  color: var(--color-heading);
}

@media (min-width: 1024px) {
  .item {
    margin-top: 0;
    padding: 0.4rem 0 1rem calc(var(--section-gap) / 2);
  }

  i {
    top: calc(50% - 25px);
    left: -26px;
    position: absolute;
    border: 1px solid var(--color-border);
    background: var(--color-background);
    border-radius: 8px;
    width: 50px;
    height: 50px;
  }

  .item:before {
    content: ' ';
    border-left: 1px solid var(--color-border);
    position: absolute;
    left: 0;
    bottom: calc(50% + 25px);
    height: calc(50% - 25px);
  }

  .item:after {
    content: ' ';
    border-left: 1px solid var(--color-border);
    position: absolute;
    left: 0;
    top: calc(50% + 25px);
    height: calc(50% - 25px);
  }

  .item:first-of-type:before {
    display: none;
  }

  .item:last-of-type:after {
    display: none;
  }
}
</style>
