<template>
  <div class="home">
    <FilterNav @filterChange="current = $event" :current="current"/>
   <div v-if="projects.length">
     <div v-for="project in filterdProjects" :key="project.id">
        <SingleProject 
        :project="project" 
        @delete="handleDelete" 
        @complete="handleComplete"/>
     </div>
   </div>
  </div>
</template>

<script>
import SingleProject from '../components/SingleProject.vue'
import FilterNav from '../components/FilterNav.vue'


export default {
  name: 'Home',
  data(){
    return{
      projects:[],
      current: 'all'
    }
  },
  components: {
    SingleProject,
    FilterNav
  },
  mounted(){
    fetch('http://localhost:3000/projects')
      .then(res=> res.json())
      .then(data=>this.projects = data)
      .catch(error=>console.log(error))
  },
  methods:{
    handleDelete(id){
      this.projects = this.projects.filter((project)=> project.id !== id)
    },
    handleComplete(id){
      let p = this.projects.find(project=> project.id === id)
      p.complete = !p.complete
    },
  },
  computed:{
    filterdProjects(){
      if(this.current === 'completed'){
        return this.projects.filter(project => project.complete)
      }
      if(this.current === 'ongoing'){
        return this.projects.filter(project => !project.complete)
      }
      
      return this.projects
    }
  }
}
</script>

