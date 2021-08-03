<template>
<div class="row">
  <div class="col-lg-4 mt-3" 
  v-for="(cake, index) in cakes" :key="index">
  <div class="card-header"> {{cake.nama}}
  </div>
  <div class="card">
  <div class="card-body">
    <p class="card-text">{{cake.image}}</p>
    <h5 class="card-title">{{cake.size}}</h5>
    <p class="card-text">{{cake.harga}}</p>
    <p class="card-text">{{cake.data.name}}</p>
    
  </div>
  <div class="card-footer text-muted">
           <center> <router-link
              class="btn btn-success"
              :to="{ name: 'Editcakes', params: { id: cake.id } }"
              >Edit</router-link
            >
            <button
              @click.prevent="cakeDelete(cake.id)"
              class="btn btn-danger"
            >
              Delete
            </button>
            </center>
  </div>
</div>
</div>
</div>
</template>

<script>
export default {
    props: ["cakes"],
}
</script>

<style>

</style>