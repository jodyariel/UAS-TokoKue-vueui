<template>
  <div class="card text-center mt-4" v-for="t in trans" :key ="t.id">
  <div class="card-header">Detail Transaksi</div>
  <div class="card-body">
    <h5 class="card-title">{{t.name}}</h5>
    <p class="card-text">{{t.cake}}</p>
    <p class="card-text">{{t.ukuran}}</p>
    <p class="card-text">{{t.price}}</p>
    <p class="card-text">{{t.groups.name}}</p>
  </div>
  <div class="card-footer">
   <router-link
              class="btn btn-success"
              :to="{ name: 'Edittrans', params: { id: t.id } }"
              >Edit</router-link
            >
            <button
              @click.prevent="tDelete(t.id)"
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
    
    let t =ref([]);
    const router = useRouter();
    const route = useRoute();
    onMounted(() => {
      axios
        .get(` http://127.0.0.1:8000/api/trans/${route.params.id}`)
        .then((Response) => {
          console.log(Response.data.data.nama);
          t.value = Response.data.data;
         
        })
        .catch((error) => {
          console.log(error.Response.data);
        });
    });
   function tDelete(id){
      axios
        .delete(`http://127.0.0.1:8000/api/trans/${id}`)
        .then(() => {
          router.go(-1);
        })
        .catch((error) => {
          console.log(error);
        });
    }
    
    return {
      t,
      router,
      tDelete,
      route,
    };
  },
};
</script>

<style>
</style>