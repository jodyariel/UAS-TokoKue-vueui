<template>
  <div class="home">
    <!--img alt="Vue logo" src="../assets/logo.png">-->
    <Slider />
    <hr class="my-3" />
    <router-link class="btn btn-primary" to="/createdata"
      >Add Category</router-link
    >


<Carddata :data="data"/>


    <table class="table">
      <thead>
        <tr>
          <th scope="col">Nama Category</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(d, index) in data" :key="index">
          <td>{{ d.name }}</td>
          <td>
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
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import axios from "axios";
// @ is an alias to /src
import Slider from "@/components/Slider.vue";
import Carddata from "@/components/Carddata.vue";
import { onMounted, ref } from "vue";

export default {
  name: "Home",
  components: {
    Slider,
    Carddata,
  },

  setup() {
    let data = ref([]);

    onMounted(() => {
      axios
        .get('http://127.0.0.1:8000/api/data')
        .then((Response) => {
          data.value = Response.data.data;
        })
        .catch((error) => {
          console.log(error);
        })
    })

    function dDelete(id){
      axios
        .delete(`http://127.0.0.1:8000/api/data/${id}`)
        .then(() => {
          let z = this.data.map((data) => data.id).index(id);
          this.data.splice(z, 1);
        })
        .catch((error) => {
          console.log(error);
        });
    }

    return {
      data,
      dDelete,
    };
  },
};
</script>
