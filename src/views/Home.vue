<template>
  <div class="home">
    <FilterNav @filterChange="current = $event" :current="current" />
    <div id="projects" v-if="projects.length">
      <div id="project" v-for="project in filteredProjects" :key="project.id">
        <SingleProject :project="project" @complete="handleComplete" @delete="handleDelete" />
      </div>
    </div>
  </div>
  
</template>

<script>
// @ is an alias to /src
import SingleProject from "../components/SingleProject.vue"
import FilterNav from "../components/FilterNav.vue"
export default {
  name: 'Home',
  components: { SingleProject, FilterNav },
  data () {
    return {
      projects: [],
      current: 'all'
    }
  },
  mounted () {
    fetch('http://localhost:3000/projects')
    .then(response => response.json())
    .then(data => this.projects = data)
    .catch(err => console.log(err.message))
  },
  computed: {
    filteredProjects(current) {
      if (this.current === 'completed') {
        return this.projects.filter(project => project.complete)

      }
      if (this.current === 'ongoing') {
        return this.projects.filter(project => !project.complete)
      }
      return this.projects
    }
  },
  methods: {
    handleDelete(id, title) {
      console.log(title + " deleted")
      this.projects = this.projects.filter((project) => {
        return project.id !== id
      })
    },
    handleComplete (id) {
      let p = this.projects.find(project => {
        return project.id == id
      })
      p.complete = !p.complete
    }
  }
}
</script>
