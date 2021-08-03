<template>
<div class="row">
  <div class="col-lg-4 mt-3" 
  v-for="(d, index) in data" :key="index">
  <div class="card-header"> {{d.name}}
  </div>
  <div class="card">
  <div class="card-body">
    <h5 class="card-title">{{d.name}}</h5>
  </div>
  <div class="card-footer text-muted">
   <router-link
              class="btn btn-primary"
              :to="{ name: 'Detaildata', params: { id: d.id } }"
              >Detail</router-link
            >
  </div>
</div>
</div>
</div>
</template>

<script>
export default {
    props: ["data"],
}
</script>

<style>

</style>