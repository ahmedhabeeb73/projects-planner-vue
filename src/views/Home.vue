<template>


  <div class="home">
    <filter-nav @filterChange="current=$event" :current="current"/>
  <div v-if="projects.length">
    
     <div v-for="project in projects" :key="project.id">
      <SingleProject :project="project" @complete='handleComplete' @delete="handleDelete" />
     </div>
     
  </div>
   </div>
  
</template>

<script>
import SingleProject from '../components/SingleProject.vue'
import FilterNav from '../components/FilterNav.vue'

export default {
  name: 'Home',
  components: {
    SingleProject,
    FilterNav
  },
  
  data(){
    return {
      projects:[],
      current:'all'
    }
  },
  

  mounted(){
    fetch('http://localhost:3000/projects')
    .then(res =>res.json())
    .then(data => this.projects = data)
    .catch(err => console.log(err.message));
    
  },
  methods:{
    handleDelete(id){
      this.projects=this.projects.filter((project)=>{
        return project.id !== id
      })
    },
    handleComplete(id){
      let p= this.projects.find((project)=>{
           return project.id === id
      })
      p.complete = !p.complete
    }
  }
}
</script>


<style scoped>
.home {
max-width: 600px;
  margin: 0 auto; 
}
</style>