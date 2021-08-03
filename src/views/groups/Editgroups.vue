<template>
  <div class="card shadow mt-3">
    <div class="card-body">
      <h5 class="card-title">Edit</h5>
      <form class="row g-3" @submit.prevent="update">
        <div class="col-md-6">
          <label for="inputEmail4" class="form-label">Keterangan</label>
          <input
            type="text"
            class="form-control"
            id="inputEmail4"
            v-model="group.ket"
          />
          <div class="alert alert-danger" v-if="validation.ket">
            {{ validation.ket[0] }}
          </div>
        </div>
        <div class="col-12">
          <button type="submit" class="btn btn-primary">Edit</button>
        </div>
      </form>
    </div>
  </div>
</template>
<script>
import { onMounted, reactive, ref } from "vue";
import { useRoute, useRouter } from "vue-router";
import axios from "axios";
export default {
  setup() {
    const group = reactive({
 
      ket: "",
    
      data_id: "",
    });

    let groups = ref([]);
    const validation = ref([]);

    const router = useRouter();

    const route = useRoute();

    onMounted(() => {
      axios
        .get(` http://127.0.0.1:8000/api/groups/${route.params.id}/edit`)
        .then((Response) => {
          console.log(Response.data.data.nama);

          group.ket = Response.data.data.ket;
          group.groups_id = Response.data.data.groups_id;
        })
        .catch((error) => {
          console.log(error.Response.data);
        });

        axios
      .get("http://127.0.0.1:8000/api/data")
      .then((response) => {
       groups.value = response.data.data;
       console.log(response);
      })
      .catch(error => {
        console.log(error);
      });
    });

    function update() {
     
      let ket = group.ket;
      
      let data_id = group.data_id;

      axios
        .put(` http://127.0.0.1:8000/api/groups/${route.params.id}`, {
        
          ket: ket,

          data_id:data_id,
        })
        .then(() => {
          router.push({
            name: "Home",
          });
        })
        .catch((error) => {
          console.log(error);
        });
    }
    return {
      group,
      validation,
      router,
      update,
      route,
      groups,
    };
  },
};
</script>