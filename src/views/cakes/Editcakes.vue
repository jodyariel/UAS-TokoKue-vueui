<template>
  <div class="card shadow mt-3">
    <div class="card-body">
      <h5 class="card-title">Edit Cake</h5>
      <form class="row g-3" @submit.prevent="update">
        <div class="col-md-6">
          <label for="inputEmail4" class="form-label">Nama</label>
          <input
            type="text"
            class="form-control"
            id="inputEmail4"
            v-model="cake.nama"
          />
          <div class="alert alert-danger" v-if="validation.nama">
            {{ validation.nama[0] }}
          </div>
        </div>
         <div class="col-6">
          <label for="inputAddress" class="form-label">image</label>
          <input
            type="file"
            class="form-control"
            id="inputAddress"
           v-on="cake.image"
          />
          <div class="alert alert-danger" v-if="validation.image">
            {{ validation.image[0] }}
          </div>
        </div>
         <div class="col-md-6">
          <label for="inputEmail4" class="form-label">Size</label>
          <input
            type="text"
            class="form-control"
            id="inputEmail4"
            v-model="cake.size"
          />
          <div class="alert alert-danger" v-if="validation.size">
            {{ validation.size[0] }}
          </div>
        </div>
         <div class="col-md-6">
          <label for="inputEmail4" class="form-label">Harga</label>
          <input
            type="text"
            class="form-control"
            id="inputEmail4"
            v-model="cake.harga"
          />
          <div class="alert alert-danger" v-if="validation.harga">
            {{ validation.harga[0] }}
          </div>
        </div>
       
         <div class="col-6">
     <label for="inputAddress" class="form-label">Cake category</label>
  <select class="form-select" aria-label="Default select example" v-model="cake.data_id">
 
  <option v-for="d in data" :key="d.id" :value="d.id">{{ d.name }}</option>

</select>
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
    const cake = reactive({
      nama: "",
      image: "",
      size: "",
      harga: "",
      data_id: "",
    });

    let data = ref([]);
    const validation = ref([]);

    const router = useRouter();

    const route = useRoute();

    onMounted(() => {
      axios
        .get(` http://127.0.0.1:8000/api/cakes/${route.params.id}/edit`)
        .then((Response) => {
          console.log(Response.data.data.nama);

          cake.nama = Response.data.data.nama;
          cake.image = Response.data.data.image;
          cake.size = Response.data.data.size;
          cake.harga = Response.data.data.harga;
          cake.data_id = Response.data.data.data_id;
        })
        .catch((error) => {
          console.log(error.Response.data);
        });

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

    function update() {
      let nama = cake.nama;
      let size = cake.size;
      let image = cake.image;
      let data_id = cake.data_id;

      axios
        .put(` http://127.0.0.1:8000/api/cakes/${route.params.id}`, {
          nama: nama,
          size: size,
          image: image,
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
      cake,
      validation,
      router,
      update,
      route,
      data,
    };
  },
};
</script>