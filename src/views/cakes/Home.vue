<template>
  <div class="home">
    <!--img alt="Vue logo" src="../assets/logo.png">-->
    <Slider />
    <hr class="my-3" />
    <router-link class="btn btn-primary" to="/createcakes"
      >Add Cake</router-link
    >


<Cardcakes :cakes="cakes"/>

  </div>
</template>

<script>
import axios from "axios";
// @ is an alias to /src
import Slider from "@/components/Slider.vue";
import Cardcakes from "@/components/Cardcakes.vue";
import { onMounted, ref } from "vue";

export default {
  name: "Home",
  components: {
    Slider,
    Cardcakes,
  },

  setup() {
    let cakes = ref([]);

    onMounted(() => {
      axios
        .get('http://127.0.0.1:8000/api/cakes')
        .then((Response) => {
          cakes.value = Response.data.data;
        })
        .catch((error) => {
          console.log(error);
        })
    })

    function cakeDelete(id){
      axios
        .delete(`http://127.0.0.1:8000/api/cakes/${id}`)
        .then(() => {
          let z = this.cakes.map((cakes) => cakes.id).index(id);
          this.cakes.splice(z, 1);
        })
        .catch((error) => {
          console.log(error);
        });
    }

    return {
      cakes,
      cakeDelete,
    };
  },
};
</script>
