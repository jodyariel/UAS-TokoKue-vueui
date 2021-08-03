<template>
  <div class="card shadow mt-3">
    <div class="card-body">
      <h5 class="card-title">Edit Transaksi</h5>
      <form class="row g-3" @submit.prevent="update">
        <div class="col-md-6">
    <label for="inputEmail4" class="form-label">Nama</label>
    <input type="text" class="form-control" id="inputEmail4" placeholder="Masukkan Nama"
    v-model="tran.name" />
      <div class="alert alert-danger" v-if="validation.name">
        {{ validation.name[0] }}
      </div>
  </div>
  <div class="col-md-6">
    <label for="inputPassword4" class="form-label">Cake</label>
    <input type="text" class="form-control" id="inputPassword4" 
    v-model="tran.cake"/>
    <div class="alert alert-danger" v-if="validation.cake">
        {{ validation.cake[0] }}
      </div>
  </div>
  <div class="col-6">
    <label for="inputAddress" class="form-label">Size</label>
    <input type="text" class="form-control" id="inputAddress"
    v-model="tran.ukuran" />
    <div class="alert alert-danger" v-if="validation.ukuran">
        {{ validation.ukuran[0] }}
      </div>
  </div>
  <div class="col-6">
    <label for="inputAddress" class="form-label">Jumlah</label>
    <input type="text" class="form-control" id="inputAddress" placeholder="Masukkan Jumlah"
    v-model="tran.jmlh" />
    <div class="alert alert-danger" v-if="validation.jmlh">
        {{ validation.jmlh[0] }}
      </div>
  </div>
  <div class="col-6">
    <label for="inputAddress" class="form-label">Harga</label>
    <input type="text" class="form-control" id="inputAddress" 
    v-model="tran.price" />
    <div class="alert alert-danger" v-if="validation.price">
        {{ validation.price[0] }}
      </div>
  </div>
  <div class="col-6">
     <label for="inputAddress" class="form-label">Keterangan</label>
  <select class="form-select" aria-label="Default select example" v-model="tran.groups_id">
 
  <option v-for="group in groups" :key="group.id" :value="group.id">{{ group.ket }}</option>

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
    const tran = reactive({
      nama: "",
      cake: "",
      ukuran: "",
      jmlh: "",
      price: "",
      groups_id: "",
    });

    let groups = ref([]);
    const validation = ref([]);

    const router = useRouter();

    const route = useRoute();

    onMounted(() => {
      axios
        .get(` http://127.0.0.1:8000/api/trans/${route.params.id}/edit`)
        .then((Response) => {
          console.log(Response.data.data.nama);

          tran.name = Response.data.data.name;
          tran.cake = Response.data.data.cake;
          tran.ukuran = Response.data.data.ukuran;
          tran.jmlh = Response.data.data.jmlh;
          tran.price = Response.data.data.price;
          tran.groups_id = Response.data.data.groups_id;
        })
        .catch((error) => {
          console.log(error.Response.data);
        });

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

    function update() {
      let name = tran.name;
      let cake = tran.cake;
      let ukuran = tran.ukuran;
      let jmlh = tran.jmlh;
      let price = tran.price;
      let groups_id = tran.groups_id;

      axios
        .put(` http://127.0.0.1:8000/api/trans/${route.params.id}`, {
        name: name,
        cake: cake,
        ukuran: ukuran,
        jmlh: jmlh,
        price: price,
        groups_id : groups_id,
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
      tran,
      validation,
      router,
      update,
      route,
      groups,
    };
  },
};
</script>