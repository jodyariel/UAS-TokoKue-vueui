<template>
  <div class="card text-center mt-4" v-for="d in data" :key ="d.id">
  <div class="card-header">Detail Category</div>
  <div class="card-body">
    <h5 class="card-title">{{d.name}}</h5>
  </div>
  <div class="card-footer">
   <router-link
              class="btn btn-success"
              :to="{ name: 'Editdata', params: { id: d.id } }"
              >Edit</router-link
            >
            <button
              @click.prevent="dDelete(d.id)"
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
    
    let d =ref([]);
    const router = useRouter();
    const route = useRoute();
    onMounted(() => {
      axios
        .get(` http://127.0.0.1:8000/api/data/${route.params.id}`)
        .then((Response) => {
          console.log(Response.data.data.nama);
          d.value = Response.data.data;
         
        })
        .catch((error) => {
          console.log(error.Response.data);
        });
    });
   function dDelete(id){
      axios
        .delete(`http://127.0.0.1:8000/api/data/${id}`)
        .then(() => {
          router.go(-1);
        })
        .catch((error) => {
          console.log(error);
        });
    }
    
    return {
      d,
      router,
      dDelete,
      route,
    };
  },
};
</script>

<style>
</style>