<template>
  <div class="submit-form">
    <div v-if="!submitted">

      <div class="form-group">
        <label for="name">Nombre</label>
        <input
          type="text"
          class="form-control"
          id="name"
          required
          v-model="product.name"
          name="name"
        />
      </div>

      <div class="form-group">
        <label for="description">Descripcion</label>
        <input
          class="form-control"
          id="description"
          required
          v-model="product.description"
          name="description"
        />
      </div>

      <div class="form-group">
        <label for="price">Precio</label>
        <input
          class="form-control"
          id="price"
          required
          v-model="product.price"
          name="price"
        />
      </div>

      <div class="form-group">
        <label for="type">Tipo</label>
        <input
          class="form-control"
          id="type"
          required
          v-model="product.type"
          name="type"
        />
      </div>

      <div class="form-group">
        <label for="img">Imagen</label>
        <input
          class="form-control"
          id="img"
          required
          v-model="product.img"
          name="img"
        />
      </div>

      <div class="form-group">
        <label for="material">Material</label>
        <input
          class="form-control"
          id="material"
          required
          v-model="product.material"
          name="material"
        />
      </div>

      <div class="form-group">
        <label for="size">Tamaño</label>
        <input
          class="form-control"
          id="size"
          required
          v-model="product.size"
          name="size"
        />
      </div>
<br>
      <button @click="saveProduct" class="btn btn-success">Submit</button>
    </div>

    <div v-else>
      <h4>El producto se ha añadido correctamente!</h4>
      <button class="btn btn-success" @click="newProduct">Volver</button>
    </div>
  </div>
</template>

<script>
import ProductDataService from "../services/ProductDataService";

export default {
  name: "add-product",
  data() {
    return {
      product: {
        id: null,
        name: "",
        description: "",
        price: "",
        type: "",
        material: "",
        size: "",
        img: "",
        published: false
      },
      submitted: false
    };
  },
  methods: {
    saveProduct() {
      var data = {
        name: this.product.name,
        description: this.product.description,
        price: this.product.price,
        type: this.product.type,
        material: this.product.material,
        size: this.product.size,
        img: this.product.img,
      };

      ProductDataService.create(data)
        .then(response => {
          this.product.id = response.data.id;
          console.log(response.data);
          this.submitted = true;
        })
        .catch(e => {
          console.log(e);
        });
    },
    
    newProduct() {
      this.submitted = false;
      this.product = {};
    }
  }
};
</script>

<style>
.submit-form {
  max-width: 300px;
  margin: auto;
}
</style>
