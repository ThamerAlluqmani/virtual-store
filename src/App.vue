<template>
   <Header/>
  <div class=" mt-3 mb-3">
    <div class="container-fluid">
      <div class="container shadow-lg p-0">
        <div class="w-100 mt-2 mb-4"></div>
        <div class="row">

          <div class="w-100 mt-2 mb-4"></div>
          <div class="text-center col">

            <category class="category" :class="{'dimmed': currentCategory !== null}" v-if="categories.length"
                      @click="fetchProducts">
              <template v-slot:text>
                all
              </template>
            </category>


            <category v-for="category in categories" class="category"
                      :class="{'dimmed': currentCategory && currentCategory != category}"
                      @click="fetchCategory(category)">
              <template v-slot:text>
                {{ category }}
              </template>
            </category>

          </div>

          <div class="w-100 mt-2 mb-4"></div>


          <div class="col-12 col-md-3 mb-3" v-for="product in products">
            <card class=".card">
              <template v-slot:header>
                <h5 class="card-header bg-white border-0 text-info">{{ product.title }}</h5>
              </template>

              <template v-slot:body>
                <div class="image" :style="{backgroundImage: `url(${product.image})`}">
                </div>
              </template>

              <template v-slot:footer>
                <div>{{ product.description }}</div>
                <div class="text-success position-sticky">{{ product.price }}</div>
                <div class="button" @click="fetchCategory(product.category)">{{ product.category }}</div>
              </template>

            </card>
          </div>
        </div>
        <br>
        <br>
      </div>
    </div>
    <br>
    <br>
  </div>

  <Footer />

</template>

<script>
const API = 'https://fakestoreapi.com';
import Card from "./components/Card.vue";
import Category from "./components/Category.vue";
import Header from "./components/Header.vue";
import Footer from "./components/Footer.vue";

export default {
  components: {
    Card,
    Category,
    Header,
    Footer
  },
  data() {
    return {
      products: [],
      categories: [],
      currentCategory: null
    }
  },
  created() {
    this.fetchProducts();
    this.fetchCategories();

  },

  mounted() {

  },

  methods: {
    async fetchData(query) {
      let response = await fetch(`${API}/${query}`);
      let data = await response.json();
      return data;


    }
    ,
    async fetchProducts() {
      this.currentCategory = null;
      this.products = await this.mapProductData(
          await this.fetchData('products')
      );
    }
    ,
    async fetchCategory(category) {
      this.currentCategory = category
      this.products = await this.mapProductData(
          await this.fetchData(`products/category/${category}`)
      );
    }
    ,
    async fetchCategories() {
      this.categories = await this.fetchData('products/categories')
    },
    mapProductData(data) {
      return data.map(product => ({
        title: product.title.slice(0, 20),
        description: product.description.slice(0, 100),
        image: product.image,
        price: '$' + product.price,
        category: product.category
      }));
    }
  }
}
</script>

<style>
@import "https://cdn.jsdelivr.net/npm/bootstrap@5.0.0-beta3/dist/css/bootstrap.min.css";

.image {
  height: 200pt;
  background-size: cover;
  background-repeat: no-repeat;
  background-position: center center;
}

.button {
  transition-duration: 0.4s;
  color: #248fb0;
}

.button:hover {
  background-color: #248fb0; /* Green */
  color: white;
  cursor: pointer;
}

.category:hover {
  cursor: pointer;
  opacity: 1;
}

.dimmed {
  opacity: .5;
}

.card {
  height: 100%;
}
</style>
