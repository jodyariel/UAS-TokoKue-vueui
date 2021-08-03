<template>
  <div class="Home">
    <!--img alt="Vue logo" src="../assets/logo.png">-->
    <Slider />
    <hr class="my-3" />
    <router-link class="btn btn-primary" to="/createtrans"
      >Add Transaksi</router-link
    >


<Cardtrans :trans="trans"/>

  </div>

</template>

<script>
import axios from "axios";
// @ is an alias to /src
import Slider from "@/components/Slider.vue";
import Cardtrans from "@/components/Cardtrans.vue";
import { onMounted, ref } from "vue";

export default {
  name: "Home",
  components: {
    Slider,
    Cardtrans,
  },

  setup() {
    let trans = ref([]);

    onMounted(() => {
      axios
        .get('http://127.0.0.1:8000/api/trans')
        .then((Response) => {
          trans.value = Response.data.data;
        })
        .catch((error) => {
          console.log(error);
        })
    })

    function tranDelete(id){
      axios
        .delete(`http://127.0.0.1:8000/api/trans/${id}`)
        .then(() => {
          let z = this.trans.map((trans) => trans.id).index(id);
          this.trans.splice(z, 1);
        })
        .catch((error) => {
          console.log(error);
        });
    }

    return {
      trans,
      tranDelete,
    };
  },
};
</script>
