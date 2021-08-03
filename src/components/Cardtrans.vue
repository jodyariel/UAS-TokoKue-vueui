<template>
<div class="row">
  <div class="col-lg-4 mt-3" 
  v-for="(tran, index) in trans" :key="index">
  <div class="card-header"> {{tran.name}}
  </div>
  <div class="card">
  <div class="card-body">
    <h5 class="card-title">{{tran.minuman}}</h5>
    <p class="card-text">{{tran.ukuran}}</p>
    <p class="card-text">{{tran.jmlh}}</p>
    <p class="card-text">{{tran.price}}</p>
     <p class="card-text">{{tran.groups.name}}</p>
    
  </div>
  <div class="card-footer text-muted">
   <center> <router-link
              class="btn btn-success"
              :to="{ name: 'Edittrans', params: { id: tran.id } }"
              >Edit</router-link
            >
            <button
              @click.prevent="tranDelete(tran.id)"
              class="btn btn-danger"
            >
              Delete
            </button>
            </center>
  </div>
</div>
</div>
</div>
</template>

<script>
export default {
    props: ["trans"],
}
</script>

<style>

</style>