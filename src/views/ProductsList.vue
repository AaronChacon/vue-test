<!-- eslint-disable eqeqeq -->
<template>
  <div class="product-list">

    <nav class="navbar bg-body-tertiary">
      <div class="container-fluid">
        <a class="navbar-brand">Welcome</a>
        <button 
          class="btn btn-outline-danger" 
          type="button" 
          @click="logout"
          >Logout</button>
      </div>
    </nav>


    <h1 class="my-5">Product List</h1>

    <div v-if="loading" class="loading">Loading...</div>
    <div v-if="error" class="error">{{ error }}</div>
    
    <div v-if="!loading && !error" class="products">
      <div v-for="product in products" :key="product.id">
        <product-card 
          v-bind:product="product"
          v-on:product-favorite-clicked="toggleProductFavorite(products, product.id)"
        />
      </div>
    </div>
  </div>
</template> 

<script>
import axios from 'axios'
import ProductCard from '../components/ProductCard.vue'
import { GET_PRODUCTS_LIST_ENDPOINT } from '@/helpers/constants';

export default {
  name: 'ProductsList',
  components: {
    ProductCard
  },
  created () {
    this.fetchProducts()
  },
  mounted () {
    this.fetchProducts()
  },
  data () {
    return {
      products: [], 
      loading: true,
      error: null
    }
  },
  methods: {

    async fetchProducts () {
      try {
        const response = await axios.get(GET_PRODUCTS_LIST_ENDPOINT) 
        // add the favorite value to the products
        this.products = response.data.slice(0, 5).map(product => {
          return {
            ...product,
            favorite: product.favorite ?? false
          }
        })
      } catch (err) {
        this.error = 'Failed to load products'
      } finally {
        this.loading = false
      }
    },
    toggleProductFavorite (products, productSelectedId) {
      // Update the favorite state of the selected product
      this.products = this.products.map(product => {
        if (product.id === productSelectedId) {
          // Toggle the favorite state
          return {
            ...product,
            favorite: !product.favorite
          }
        }
        return product
      })
    },
    logout () {
      // Clear the token from the local storage
      localStorage.removeItem('token')
      // Redirect to the login page
      this.$router.push('/login')
    }
  }
}
</script>

<style scoped>
  .product-list {
    font-family: Arial, sans-serif;
    max-width: 800px;
    margin: 0 auto;
    padding: 20px;
  }

  .loading {
    font-size: 18px;
    color: gray;
  }

  .error {
    color: red;
    font-size: 18px;
  }

  .products {
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    gap: 20px;
  }
</style>
