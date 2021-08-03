<template>
<div class="card shadow mt-3">
  <div class="card-body">
    <h5 class="card-title">Add Keterangan</h5>
     <form class="row g-3" @submit.prevent="store">
  <div class="col-md-6">
    <label for="inputEmail4" class="form-label">Keterangan</label>
    <input type="text" class="form-control" id="inputEmail4" 
    v-model="group.ket" />
      <div class="alert alert-danger" v-if="validation.ket">
        {{ validation.ket[0] }}
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
    const group = reactive({
      ket: '',
     
    });

    let groups = ref([]);
    const validation = ref([]);
    const router = useRouter();

    onMounted(()=>{
       axios
      .get("http://127.0.0.1:8000/api/groups")
      .then((response) => {
       groups.value = response.data.data;
       console.log(response);
      })
      .catch(error => {
        console.log(error);
      });
    });

    function store(){
      let ket = group.ket;
    

      axios.post("http://127.0.0.1:8000/api/groups", {
        ket: ket,
        
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
      group,
      validation,
      router, 
      store,
      groups,
    }
  },
}
</script>