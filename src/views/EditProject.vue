<template>
  <form v-on:submit.prevent="handleSubmit">
    <label>Title</label>
    <input type="text" v-model="title" required>
    <label>Details</label>
    <textarea v-model="details" required></textarea>
    <input type="checkbox" v-model="complete">
    <button>Edit Project</button>
  </form>
</template>

<script>
export default {
    props: {
        id: String
    },
    data() {
        return {
        uri: `http://localhost:3000/projects/${this.id}`,
        title: '',
        details: '',
        complete: null,
        }
    },
    methods:{
        handleSubmit(){
            let project = {
                title: this.title,
                details: this.details,
                complete: this.complete,
            }
            //console.log(project)

            fetch(this.uri, {
                method: 'PATCH',
                headers:{
                'Content-Type': 'application/json'
                },
                body: JSON.stringify(project)
            })
            .then(() => {
                this.$router.push('/')
            })
            .catch((err) => console.warn(err))
            }
    },
    mounted(){
        fetch(this.uri)
        .then((res) => res.json())
        .then((data) => {
            //console.log(data)
            //insert data to inputs
            this.title = data.title
            this.details = data.details
            this.complete = data.complete
        })
    }
}
</script>

<style>

</style>