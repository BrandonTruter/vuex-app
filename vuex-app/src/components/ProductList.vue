<template>
  <div>
    <h1>Product List</h1>
    <img v-if="loading" src="https://media.giphy.com/media/3oEjI6SIIHBdRxXI40/giphy.gif" alt="loading">
    <ul v-else>
      <li v-for="product in products">
        {{ product.title }} - {{ product.price | currency }} - {{product.inventory}}
        <button @click="addProductToCart(product)" :disabled="!productIsInStock(product)">Add to Cart</button>
      </li>
    </ul>
  </div>
</template>

<script>
  import {mapState, mapGetters, mapActions} from "vuex";

  export default {
    data() {
      return {
        loading: false
      }
    },

    computed: {
      ...mapState({
        products: state => state.products
      }),

      ...mapGetters({
        productIsInStock: 'productIsInStock'
      })
    },

    methods: {
      ...mapActions({
        fetchProducts: 'fetchProducts',
        addProductToCart: 'addProductToCart'
      })
    },

    created() {
      this.loading = true
      this.fetchProducts()
        .then(() => {
          this.loading = false
        })
    }
  }
</script>
