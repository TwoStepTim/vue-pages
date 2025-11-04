<template>
  <div id="app" class="container">
    <h1>Random Quote Generator</h1>

    <!-- Display the quote or a loading message -->
    <p v-if="loading">Loading...</p>

    <div v-else>
      <p>"{{ quote }}"</p>
      <p v-if="author" class="author">— {{ author }}</p>
    </div>

    <!-- Button to fetch new quote -->
    <button @click="getQuote">Get New Quote</button>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'App',

  data() {
    return {
      quote: '',      // stores the quote text
      author: '',     // stores the author name
      loading: false  // tracks loading status
    }
  },

  methods: {
    async getQuote() {
      this.loading = true
      try {
        const response = await axios.get(
          'https://quotes15.p.rapidapi.com/quotes/random/?language_code=en',
          {
            headers: {
              'x-rapidapi-host': 'quotes15.p.rapidapi.com',
              'x-rapidapi-key': '4507aebf71msh95475cb990bc1a6p1d0247jsnb3603e4b1ad0' // your key
            }
          }
        )

        // Update the quote and author
        this.quote = response.data.content
        this.author = response.data.originator?.name || ''
      } catch (error) {
        console.error('Error fetching quote:', error)
        this.quote = 'Oops! Something went wrong.'
        this.author = ''
      } finally {
        this.loading = false
      }
    }
  },

  mounted() {
    // Load a random quote on startup
    this.getQuote()
  }
}
</script>

<style>
.container {
  text-align: center;
  font-family: Arial, sans-serif;
  margin-top: 50px;
}

button {
  margin-top: 20px;
  padding: 10px 20px;
  font-size: 16px;
}

.author {
  font-style: italic;
  color: #555;
  margin-top: 5px;
}
</style>
