<template>
  <div class="home">
    <FilterNav @filterChange="setFilter" v-bind:current="current" />
    <div v-if="projects.length">
      <div v-for="project in filteredProjects" v-bind:key="project.id">
        <SingleProject v-bind:project="project" @reRenderProjects="reRenderProjects" @complete="handleComplete" />
      </div>
    </div>
  </div>
</template>

<script>
import SingleProject from '../components/SingleProject'
import FilterNav from '../components/FilterNav'
import { computed } from 'vue'

export default {
  name: 'Home',
  components: {
    SingleProject,
    FilterNav,
  },
  data(){
    return{
      projects: [],
      current: 'all'
    }
  },
  methods:{
    //we get the id of the deleted project
    reRenderProjects(id){
      //for every item in the array, filter them
      //if id param is equal to the id in the existing array it gets removed
      this.projects = this.projects.filter((project) => {
        return project.id !== id
      })
    },
    //get id as param
    handleComplete(id){
      //create p for every project
      let p = this.projects.find(project => {
        //true if param id matches project id
        return project.id === id
      })
      p.complete = !p.complete
    },
    setFilter($event){
      this.current = $event
    }
  },
  mounted(){
    const url = 'http://localhost:3000/projects'
    fetch(url)
    .then((res) => res.json())
    .then(data => this.projects = data)
    .catch((err) => console.warn(err))
  },
  computed:{
    filteredProjects(){
      if(this.current === 'completed'){
        return this.projects.filter(project => project.complete)
      }

      if(this.current === 'ongoing'){
        return this.projects.filter(project => !project.complete)
      }

      //if all
      return this.projects
    }
  }
}
</script>
