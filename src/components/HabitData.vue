<template>
  <h3> {{msg}} </h3>
  <div>
  <input v-model="idField" placeholder="ID">
    <input v-model="countField" placeholder="Count">
    <input v-model="nameField" placeholder="Name">
  </div>

  <div>
    <button type="button" @click = "save()">Save</button>
  </div>
</template>

<script>
export default {
  name: "HabitData",
  props: ['msg'],
  data () {
    return {
      items: [],
      nameField: '',
      idField: '',
      countField: ''
    }
  },  methods: {
    loadThings() {
      const endpoint = 'http://localhost:8080/habits'
      const requestOptions = {
        method: 'GET',
        redirect: 'follow'
      }
      fetch(endpoint, requestOptions)
          .then(response => response.json())
          .then(result => result.forEach(habits => {
            this.items.push(habits)
          }))
          .catch(error => console.log('error', error))
    },
    save() {
      const endpoint = 'http://localhost:8080/habits'
      const data = {
        name: this.nameField,
        id: this.idField,
        count: this.countField
      }
       const requestOptions = {
        method: 'POST',
         headers: {
          'Content-Type': 'application/json'
         },
         body: JSON.stringify(data)
       }
       fetch(endpoint, requestOptions)
           .then(response => response.json())
           .then(data => {
             console.log('Success', data)
           })
           .catch(error => console.log('error', error))
    }
  }
}
</script>


<style scoped>

</style>
