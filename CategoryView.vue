<template>
  <nav class="breadcrumb" aria-label="breadcrumbs">
    <ul>
      <li><router-link to="/">Amcart</router-link></li>
      <li><a href="#">Categories</a></li>
      <li class="is-active"><a href="#" aria-current="page">{{ category.name }}</a></li>
    </ul>
  </nav>

  <div class="page-category">
    <div class="col-md-8">

<div class="row">
        <div>
                  <transition-group name="fade" class="row" tag="div">
                     <div v-for="product in category.products" v-bind:key="product.id" v-bind:product="product"
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
  </div>
</template>

<script>
import axios from 'axios'
import { toast } from 'bulma-toast'

import ProductBox from "@/components/ProductBox";

export default {
  name: "CategoryView",
  components: {
    ProductBox
  },
  data() {
    return {
      category: {
        products: []
      }
    }
  },
  watch: {
    $route(to, from) {
      if (to.name === 'Category') {
        this.getCategory()
      }
    }
  },
  mounted() {
        this.getCategory()
  },
  methods: {
    async getCategory() {
      const categorySlug = this.$route.params.category_slug

      this.$store.commit('setIsLoading', true)

      axios
          .get(`/api/v1/products/${categorySlug}/`)
          .then(response => {
            this.category = response.data

            document.title = this.category.name + ' | Amcart'
          })
          .catch(error => {
            console.log(error)

            toast({
              message: 'Something went wrong. Please try again.',
              type: 'is-danger',
              dismissible: true,
              pauseOnHover: true,
              duration: 2000,
              position: 'bottom-right',
            })
          })

      this.$store.commit('setIsLoading', false)
    }
  },
}
</script>

<style scoped>
.column{
    margin-left: 1%;
    height: fit-content;
    width: fit-content;
    /* width: 50%; */
    /* width: fit-content; */
    /* height: auto; */
   
  }

  .page-category{
  margin-left: 20%;
}

</style>