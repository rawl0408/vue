<template>
  <div id="app">
    <nav class="navbar navbar-expand navbar-dark bg-dark">
      <router-link to="/products" class="navbar-brand">Arteva</router-link>
      <div class="navbar-nav mr-auto">
        <!-- <li class="nav-item">
          <router-link to="/products/:id" class="nav-link">Lista de Productos</router-link>
        </li> -->
        <li class="nav-item">
          <router-link to="/add" class="nav-link">Añadir producto</router-link>
        </li>
      </div>
    </nav>

    <div class="container mt-3">
      <router-view />
    </div>
  </div>
</template>

<script>
export default {
  name: "app"
};
</script>
