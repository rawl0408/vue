<template>
  <div v-if="currentProduct" class="edit-form">

    <h4>Productos</h4>

    <form>

      <div class="form-group">
        <label for="name">Nombre</label>
        <input type="text" class="form-control" id="name"
          v-model="currentProduct.name"
        />
      </div>

      <div class="form-group">
        <label for="description">Descriction</label>
        <input type="text" class="form-control" id="description"
          v-model="currentProduct.description"
        />

      </div>

      <div class="form-group">
        <label for="price">Precio</label>
        <input type="text" class="form-control" id="price"
          v-model="currentProduct.price"
        />
      </div>

      <div class="form-group">
        <label for="type">Tipo</label>
        <input type="text" class="form-control" id="type"
          v-model="currentProduct.type"
        />
      </div>

      <div class="form-group">
        <label for="material">Material</label>
        <input type="text" class="form-control" id="material"
          v-model="currentProduct.material"
        />
      </div>

      <div class="form-group">
        <label for="size">Tamaño</label>
        <input type="text" class="form-control" id="size"
          v-model="currentProduct.size"
        />
      </div>

      <div class="form-group">
        <label for="img">Imagen (Usar URL) </label>
        <input type="text" class="form-control" id="img"
          v-model="currentProduct.img"
        />
      </div>

      <div class="form-group">
        <label><strong>Status:</strong></label>
        {{ currentProduct.published ? "Published" : "Pending" }}
      </div>
    </form>

    <button class="badge badge-primary mr-2"
      v-if="currentProduct.published"
      @click="updatePublished(false)"
    >
      DesPublicar
    </button>

    <button type="button" class="btn btn-outline-success"
      @click="updatePublished(true)"
    >
      Publicar
    </button>

    <button type="button" class="m-3 btn btn-outline-danger"
      @click="deleteProduct"
    >
      Eliminar
    </button>

    <button type="button" class="btn btn-outline-primary"
      @click="updateProduct"
    >
      Guardar
    </button>

    <p>{{ message }}</p>
  </div>

  <div v-else>
    <br />
    <p>Selecciona un producto</p>
  </div>
</template>

<script>
import ProductDataService from "../services/ProductDataService";

export default {
  name: "product",
  data() {
    return {
      currentProduct: null,
      message: ''
    };
  },
  methods: {
    getProduct(id) {
      
      ProductDataService.get(id)
        .then(response => {
          this.currentProduct = response.data;
          console.log(response.data);
        })
        .catch(e => {
          console.log(e);
        });
    },

    updatePublished(status) {
      var data = {
        id: this.currentProduct.id,
        name: this.currentProduct.name,
        description: this.currentProduct.description,
        type: this.currentProduct.type,
        material: this.currentProduct.material,
        size: this.currentProduct.size,
        img: this.currentProduct.img,
        published: status
      };

      ProductDataService.update(this.currentProduct.id, data)
        .then(response => {
          this.currentProduct.published = status;
          console.log(response.data);
        })
        .catch(e => {
          console.log(e);
        });
    },

    updateProduct() {
      this.currentProduct.price = +this.currentProduct.price
      ProductDataService.update(this.currentProduct.id, this.currentProduct)
        .then(response => {
          console.log(response.data);
          this.message = 'El producto se ha modificado correctamente!';
        })
        .catch(e => {
          console.log(e);
        });
    },

    deleteProduct() {
      ProductDataService.delete(this.currentProduct.id)
        .then(response => {
          console.log(response.data);
          this.$router.push({ name: "products" });
        })
        .catch(e => {
          console.log(e);
        });
    }
  },
  mounted() {
    this.message = '';
    this.getProduct(this.$route.params.id);
  }
};
</script>

<style>
.edit-form {
  max-width: 300px;
  margin: auto;
}
</style>
