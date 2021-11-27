<template>
  <form>
    <label>Title:</label>
    <input type="text" v-model="title" required />
    <label>Details:</label>
    <textarea v-model="details" required></textarea>
    <button>Update Project</button>
  </form>
</template>

<script>
export default {
  props: ['id'],
  data(){
    return{
      title:'',
      details:'',
      uri: 'http://localhost:3000/projects/'
    }
  },
  mounted(){
    fetch(this.uri + this.id)
      .then(res=>res.json())
      .then(data=>{
        this.title = data.title;
        this.details = data.details;
      })
  }
}
</script>

<style>

</style>