<template>
  <nav class="breadcrumb" aria-label="breadcrumbs">
    <ul>
      <li><router-link to="/">Amcart</router-link></li>
      <li class="is-active"><a href="#" aria-current="page">Search</a></li>
    </ul>
  </nav>

  <div class="page-search">
    <div class="columns is-multiline">
      <div class="column is-10">
      
        <h1 class="title">Search</h1>

        <h2 class="is-size-3 has-text-grey">Search term: "{{ query }}"</h2>
      </div>

    </div>
  </div>
     
      <div class="col-md-8">

         <div class="row">
        
 
      <!-- <ProductBox
          v-for="product in products"
          v-bind:key="product.id"
          v-bind:product="product"/> 
       -->
         
       <div>
                  <transition-group name="fade" class="row" tag="div">
                     <div v-for="product in products" v-bind:key="product.id" v-bind:product="product"
                        class="col-6 col-xl-4 col-lg-4 col-md-4 col-sm-6 col-xs-4 pb-3" :key="product.id">
                        <div class="card">
                           <img class="card-img-top" :src="product.get_image" alt="Card image cap">
                           <div class="overlay">
                              <!-- <button type="button" class="btn btn-outline-secondary btn-lg" @click="addtoCart(item)">Add +</button> -->
                              <router-link v-bind:to="`/${product.get_absolute_url}`"><button type="button"
                                    class="btn btn-outline-secondary btn-lg">View Details</button></router-link>
                           </div>
                           <div class="card-body">
                              <h5 class="card-title">{{ product.name }}</h5>
                              <p class="card-text">Rs {{ product.price }}</p>
                           </div>
                        </div>
                     </div>
                  </transition-group>
               </div>
    

  </div>  
          </div>
        

   
</template>

<script>
import axios from 'axios'
import ProductBox from '@/components/ProductBox.vue'

export default {
  name: 'SearchView',
  components: {
    ProductBox
  },
  data() {
    return {
      latestProducts: [],
      products: [],
      query: ''
    }
  },
  mounted() {
    document.title = 'Search | Amcart'

    let uri = window.location.search.substring(1)
    let params = new URLSearchParams(uri)

    if (params.get('query')) {
      this.query = params.get('query')

      this.performSearch()
    }
  },
  methods: {
    async performSearch() {
      this.$store.commit('setIsLoading', true)

      await axios
          .post('/api/v1/products/search', {'query': this.query})
          .then(response => {
            this.products = response.data
          })
          .catch(error => {
            console.log(error)
          })

      this.$store.commit('setIsLoading', false)
    }
  }
}
</script>


<style scoped>

.page-search{
  margin-left: 3%;
}

/* #image {
  
  height: 35rem; 
 
 
    
    
 } */

 .row {
  margin-left: 3%;
 }

</style>
