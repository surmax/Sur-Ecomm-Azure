<template>
   <div class="home">
      <div class="sec1">

         <div id="app1">
            <Slider />

         </div>

      </div>

   </div>

   <div class="shop">

      <div class="page">


         <div class="sub-title mb-2">
            <h3 class="is-size-2 has-text-centered has-text-black mb-4"><br/>Best Seller Products</h3>

         </div>  
      </div>
   </div>




   <div class="container-grid">

      <div class="row justify-content-around">
      <div class="row col-3 pb-4 pr-1">
         
        <div class="dropdown show ">
          <a class="btn btn-light dropdown-toggle align-right" role="button" id="dropdownMenuLink" 
          data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">SORT BY
            <span style="color:#f2be00;">{{ sortButton }}</span>
          </a>
          <div class="dropdown-menu" aria-labelledby="dropdownMenuLink">
            <a class="dropdown-item" @click="sortDate">Date</a>
            <a class="dropdown-item" @click="sortPrice" >Price</a>
            <a class="dropdown-item" @click="sortTrend">Trending</a>
          </div>
        </div>
        </div>
        <div class="row col-6 flex-row-reverse"> </div>

      </div> 

    

      <div class="row justify-content-around">




         <div class="col-md-9s order-1">

       


            <div class="row col-xl-9 col-lg-9 col-md-12 col-sm-12 col-xs-12 text-center">
               <!-- <div class="col-md-12 mb-5"> -->

       
               <div>
                  <transition-group name="fade" class="row" tag="div">
                     <div v-for="product in latestProducts" v-bind:key="product.id" v-bind:product="product"
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

         <div class="c1">
            <div class="border p-4 rounded mb-4">
               <h3 class="mb-3 h6 text-uppercase text-black d-block">Categories</h3>
               <ul class="list-unstyled mb-0">
                  <li class="mb-1"><router-link to="/men" class="d-flex"><span>Men</span> <span
                           class="text-black ml-auto">(25)</span></router-link></li>
                  <li class="mb-1"><router-link to="/women" class="d-flex"><span>Women</span> <span
                           class="text-black ml-auto">(25)</span></router-link></li>

               </ul>
            </div>

            <div class="border p-4 rounded mb-4">
               <div class="mb-4">
                  <h3 class="mb-3 h6 text-uppercase text-black d-block">Filter by Price</h3>
                  <div id="slider-range" class="border-primary"></div>
                  <input type="text" name="text" id="amount" class="form-control border-0 pl-0 bg-white" disabled="" />
               </div>

               <div class="mb-4">
                  <h3 class="mb-3 h6 text-uppercase text-black d-block">Size</h3>
                  <label for="s_sm" class="d-flex">
                     <input type="checkbox" id="s_sm" class="mr-2 mt-1"> <span class="text-black">Small (20)</span>
                  </label>
                  <label for="s_md" class="d-flex">
                     <input type="checkbox" id="s_md" class="mr-2 mt-1"> <span class="text-black">Medium (25)</span>
                  </label>
                  <label for="s_lg" class="d-flex">
                     <input type="checkbox" id="s_lg" class="mr-2 mt-1"> <span class="text-black">Large (25)</span>
                  </label>
               </div>

               <div class="mb-4">
                  <h3 class="mb-3 h6 text-uppercase text-black d-block">Color</h3>
                  <a href="#" class="d-flex color-item align-items-center">
                     <span class="bg-danger color d-inline-block rounded-circle mr-2"></span>
                     <router-link to="/search?query=Red"> <span class="text-black">Red (2)</span></router-link>
                  </a>
                  <a href="#" class="d-flex color-item align-items-center">
                     <span class="bg-success color d-inline-block rounded-circle mr-2"></span><router-link
                        to="/search?query=Green"> <span class="text-black">Green (2)</span></router-link>
                  </a>
                  <a href="#" class="d-flex color-item align-items-center">
                     <span class="bg-info color d-inline-block rounded-circle mr-2"></span> <router-link
                        to="/search?query=blue"><span class="text-black">Blue (3)</span></router-link>
                  </a>
                  <a href="#" class="d-flex color-item align-items-center">
                     <span class="bg-warning color d-inline-block rounded-circle mr-2"></span><router-link
                        to="/search?query=Yellow"> <span class="text-black">Yellow (2)</span></router-link>
                  </a>
               </div>

            


            </div>


      </div>


      
        </div>
        
        <!-- <div class="row text-center justify-content-around">
        <div class="col-5 py-5">
            <button type="button" @click="incCardNumber" class="btn btn-outline-secondary btn-lg btn-block ">More +</button>
          </div>
</div> -->

   </div>
</template>

<script>
import axios from 'axios'
import Slider from '@/components/Slider'
import ProductBox from "@/components/ProductBox";
import Card from '@/components/ProductsPage/Card.vue'

export default {
   name: 'HomeView',
   

   created() {
      const authInfo = JSON.parse(localStorage.getItem('authToken'));
      if (authInfo) {
         this.customerName = authInfo.account.idToken.given_name;
         this.authenticated = true
      }
   },

   data() {
      return {
         latestProducts: [],
         customerName: '',
         authenticated: false,
         cards: [],
      showCards: 6,
      sortButton: 'DEFAULT'
      }
   },
   components: {
      Slider,
      ProductBox,
      Card
   },
   mounted() {
      this.getLatestProducts()

      document.title = 'Home | Amcart'
   },
   created(){
    this.cards = this.getLatestProducts
  },

   methods: {
      async getLatestProducts() {
         this.$store.commit('setIsLoading', true)

         await axios
            .get('/api/v1/latest-products/'
            )
            .then(response => {
               this.latestProducts = response.data
            })
            .catch(error => {
               console.log(error)
            })
         this.$store.commit('setIsLoading', false)
      },
      incCardNumber() {
      return this.showCards += 6
    },
   
    sortDate() {
       this.latestProducts.sort((a, b) => (a.name.length * 2)-(b.name.length * 4))
       console.log("latestProducts")
       return this.sortButton = 'DATE'

    },
    sortPrice() {
       this.latestProducts.sort((a, b) => a.price-b.price)
       return this.sortButton = 'PRICE'
    },
    sortTrend() {
       this.latestProducts.sort((a, b) => a.id-b.id)
       return this.sortButton = 'TRENDING'
    },
    sortI(name){
      this.latestProducts = this.it.filter((e) => e.type.match(name) || e.color.match(name))
    },
    reSet() {
      return this.latestProducts = this.it
    } 
   },
   

}
</script>

<style scoped>
.image {

   margin-top: -0.25rem;
   margin-left: -0.25rem;
   margin-right: -0.25rem;



}

.col-md-9s {
   -webkit-box-flex: 0;
   -ms-flex: 0 0 75%;
   flex: 0 0 80%;
   max-width: 75%;
}


.container-grid {

   /* margin-top: 1%; */
   margin-left: 8%;

}


.fade-move {
  transition: transform 1s;
}
/* Card Style */
.card {
  transition: 500ms;
  position: relative;
  overflow: hidden;
}

.card img {
  z-index: 1;
  height: 25rem;
}

.card button {
  width: 140px;
  margin-bottom: 10px;
}

.card:hover img {
  filter: blur(4px);
}

.card:hover .overlay {
  opacity: 0.8;

}

.card .overlay {
  position: absolute;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  width: 100%;
  height: 50%;
  background-color: #232b34;
  opacity: 0;
  z-index: 100;
  transition: all 0.3s ease-in;
}

.card:hover, .card:active {
  transform: scaleY(1.1) scaleX(1.06);
  box-shadow: 0 14px 98px rgba(0, 0, 0, 0.25), 0 0px 60px rgba(0, 0, 0, 0.22);
}
</style>
