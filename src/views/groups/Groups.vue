<template>
  <div class="home">
    <!--img alt="Vue logo" src="../assets/logo.png">-->
    <Slider />
    <hr class="my-3" />
    <router-link class="btn btn-primary" to="/creategroups"
      >Add Ketarangan</router-link
    >


<Cardgroups :groups="groups"/>


  </div>
</template>

<script>
import axios from "axios";
// @ is an alias to /src
import Slider from "@/components/Slider.vue";
import Cardgroups from "@/components/Cardgroups.vue";
import { onMounted, ref } from "vue";

export default {
  name: "Home",
  components: {
    Slider,
    Cardgroups,
  },

  setup() {
    let groups = ref([]);

    onMounted(() => {
      axios
        .get('http://127.0.0.1:8000/api/groups')
        .then((Response) => {
          groups.value = Response.data.data;
        })
        .catch((error) => {
          console.log(error);
        })
    })

    function groupDelete(id){
      axios
        .delete(`http://127.0.0.1:8000/api/groups/${id}`)
        .then(() => {
          let z = this.groups.map((groups) => groups.id).index(id);
          this.groups.splice(z, 1);
        })
        .catch((error) => {
          console.log(error);
        });
    }

    return {
      groups,
      groupDelete,
    };
  },
};
</script>
