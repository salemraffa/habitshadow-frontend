<template>
  <h3> {{ msg }} </h3>
  <div>
    <input v-model="idField" placeholder="ID">
    <input v-model="countField" placeholder="Count" @keyup.enter="save()">
    <input v-model="nameField" placeholder="Name" type="text" ref="nameInput">
    <input v-model="descriptionField" placeholder="Description">
    <input v-model="stackField" placeholder="Stack">
    <button type="button" @click="save()">Save</button>
  </div>

  <div>
    <table>
      <thead>
      </thead>
      <tbody>
      <tr v-if="items.length === 0">
        <td colspan="2">No products yet</td>
      </tr>
      <tr v-for="item in myFilterFunc(filterCrit)" :key="item.idField">
        <td>{{ item.id }}</td>
        <td>{{ item.count }}</td>
        <td>{{ item.name }}</td>
        <td>{{ item.description }}</td>
        <td>{{ item.stack }}</td>

      </tr>
      <tr>
        <td>{{ idField }}</td>
        <td>{{ countField }}</td>
        <td>{{ nameField }}</td>
        <td>{{ descriptionField }}</td>
        <td>{{ stackField }}</td>
      </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import HabitData from "@/components/HabitData.vue";

export default {
  name: "HabitData",
  computed: {
    HabitData() {
      return HabitData
    }
  },
  props: ['msg'],
  data() {
    return {
      items: [],
      nameField: '',
      idField: '',
      countField: '',
      descriptionField: '',
      stackField: '',
    }
  }, methods: {
    myFilterFunc(crit) {
      return this.items.filter(
          it => crit.length < 1 ||
              it.name.toLowerCase().includes(crit.toLowerCase()))
    },
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
        count: this.countField,
        description: this.descriptionField,
        stack: this.stackField
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
    }, mounted() {
      this.loadThings()
    }
  }

}


</script>


<style scoped>
table {
  margin-left: auto;
  margin-right: auto;
}

button {
  color: green;
}
</style>
