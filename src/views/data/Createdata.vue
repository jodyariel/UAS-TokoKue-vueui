<template>
<div class="card shadow mt-3">
  <div class="card-body">
    <h5 class="card-title">Add data</h5>
     <form class="row g-3" @submit.prevent="store">
  <div class="col-md-6">
    <label for="inputEmail4" class="form-label">Nama</label>
    <input type="text" class="form-control" id="inputEmail4" 
    v-model="cake.name" />
      <div class="alert alert-danger" v-if="validation.name">
        {{ validation.name[0] }}
      </div>
  </div>

  <div class="col-12">
    <button type="submit" class="btn btn-primary">Add</button>
  </div>
</form>
  </div>
</div>
 
</template>
<script>
import { reactive, ref, onMounted} from 'vue';
import { useRouter } from 'vue-router';
import axios from 'axios';
export default {
  setup() {
    const d = reactive({
      name: '',
    });

    let data = ref([]);
    const validation = ref([]);
    const router = useRouter();

    onMounted(()=>{
       axios
      .get("http://127.0.0.1:8000/api/data")
      .then((response) => {
       data.value = response.data.data;
       console.log(response);
      })
      .catch(error => {
        console.log(error);
      });
    });

    function store(){
      let name = d.name;

      axios.post("http://127.0.0.1:8000/api/data", {
        name: name,
      })
      .then(() => {
        router.push({
          name:'Home',
        });
      })
      .catch(error => {
        validation.value = error.response.data;
      });
    }
    return {
      d,
      validation,
      router, 
      store,
      data,
    }
  },
}
</script>