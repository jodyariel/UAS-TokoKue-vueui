<template>
<div class="row">
  <div class="col-lg-4 mt-3" 
  v-for="(group, index) in groups" :key="index">
  <div class="card-header"> {{group.ket}}
  </div>
  <div class="card">
  <div class="card-body">
    <p class="card-text">{{group.ket}}</p>
  </div>
  <div class="card-footer text-muted">
    <center> <router-link
              class="btn btn-success"
              :to="{ name: 'Editgroups', params: { id: group.id } }"
              >Edit</router-link
            >
            <button
              @click.prevent="groupDelete(group.id)"
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
    props: ["groups"],
}
</script>

<style>

</style>