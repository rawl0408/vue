<template>
  <div class="list row">
    <div class="col-md-8">
      <div class="input-group mb-3">
        <input
          type="text"
          class="form-control"
          placeholder="Search by name"
          v-model="name"
        />
        <div class="input-group-append">
          <button
            class="btn btn-outline-secondary"
            type="button"
            @click="searchName"
          >
            Search
          </button>
        </div>
      </div>
    </div>
    <div class="col-md-6">
      <h4>Lista de productos</h4>

      <ul class="list-group">

        <li
          class="list-group-item"
          :class="{ active: index == currentIndex }"
          v-for="(product, index) in products"
          :key="index"
          @click="setActiveProduct(product, index)"
        >
        <div class="d-flex align-items-center">

          <div class="flex-shrink-0"> <img width="100px" height="100px" :src="product.img"> </div>
          <div class="flex-grow-1 ms-3"> {{ product.name }} </div>

        </div>

          
        </li>
      </ul>
<!-- 
      <button class="m-3 btn btn-sm btn-danger" @click="removeAllProducts">
        Remove All
      </button> -->
    </div>
    <div class="col-md-6 fixed">
      <div  v-if="currentProduct">
        <h4>Producto</h4>

        <div>
          <label><strong>Nombre:</strong></label> {{ currentProduct.name }}
        </div>

        <div>
          <label><strong>Descripcion:</strong></label>
          {{ currentProduct.description }}
        </div>

        <div>
          <label><strong>Precio:</strong></label>
          {{ currentProduct.price }} €
        </div>
        
        <div>
          <label><strong>Tipo:</strong></label>
          {{ currentProduct.type }}
        </div>

        <div>
          <label><strong>Material:</strong></label>
          {{ currentProduct.material }}
        </div>

        <div>
          <label><strong>Talla:</strong></label>
          {{ currentProduct.size }}
        </div>
<br>
        <div>
          <label><strong>Status:</strong></label>
          {{ currentProduct.published ? "Published" : "Pending" }}
        </div>

        <router-link :to="'/products/' + currentProduct.id" class="m-3 btn btn-sm btn-warning">Edit</router-link>
        
        
      </div>
      <div v-else>
        <br />
        <p>Selecciona un producto</p>
      </div>
    </div>
  </div>
</template>

<script>
import ProductDataService from "../services/ProductDataService";

export default {
  name: "products-list",
  data() {
    return {
      products: [],
      currentProduct: null,
      currentIndex: -1,
      name: "",
    };
    
  },
  methods: {
    retrieveProducts() {
      ProductDataService.getAll()
        .then((response) => {
          this.products = response.data;
          console.log(response.data);
        })
        .catch((e) => {
          console.log(e);
        });
    },

    refreshList() {
      this.retrieveProducts();
      this.currentProduct = null;
      this.currentIndex = -1;
    },

    setActiveProduct(product, index) {
      this.currentProduct = product;
      this.currentIndex = index;
    },

    removeAllProducts() {
      ProductDataService.deleteAll()
        .then((response) => {
          console.log(response.data);
          this.refreshList();
        })
        .catch((e) => {
          console.log(e);
        });
    },

    searchName() {
      ProductDataService.findByName(this.name)
        .then((response) => {
          this.products = response.data;
          console.log(response.data);
        })
        .catch((e) => {
          console.log(e);
        });
    },
  },
  mounted() {
    this.retrieveProducts();
    
  },
};
</script>

<style>
.list {
  text-align: left;
  max-width: 750px;
  margin: auto;
}

.fixed {
  position: fixed;
  right: 0;
  top: 15%;
  
}
</style>
