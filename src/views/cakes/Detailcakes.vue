<template>
  <div class="card text-center mt-4" v-for="cake in cakes" :key ="cake.id">
  <div class="card-header">Detail Cakes</div>
  <div class="card-body">
    <h5 class="card-title">{{cake.nama}}</h5>
    <p class="card-text">{{cake.image}}</p>
    <p class="card-text">{{cake.size}}</p>
     <p class="card-text">{{cake.harga}}</p>
    <p class="card-text">{{cake.groups.name}}</p>
  </div>
  <div class="card-footer">
   <router-link
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
  </div>
</div>
</template>

<script>
import { onMounted, ref } from "vue";
import { useRoute, useRouter } from "vue-router";
import axios from "axios";
export default {
  setup() { 
    
    let cake =ref([]);
    const router = useRouter();
    const route = useRoute();
    onMounted(() => {
      axios
        .get(` http://127.0.0.1:8000/api/cakes/${route.params.id}`)
        .then((Response) => {
          console.log(Response.data.data.nama);
          cake.value = Response.data.data;
         
        })
        .catch((error) => {
          console.log(error.Response.data);
        });
    });
   function cakeDelete(id){
      axios
        .delete(`http://127.0.0.1:8000/api/cakes/${id}`)
        .then(() => {
          router.go(-1);
        })
        .catch((error) => {
          console.log(error);
        });
    }
    
    return {
      cake,
      router,
      cakeDelete,
      route,
    };
  },
};
</script>

<style>
</style>