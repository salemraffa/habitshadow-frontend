<template>
  <h3> {{ msg }} </h3>
  <div>
    <input v-model="idField" placeholder="ID">
    <input v-model="countField" placeholder="Count" @keyup.enter="save()">
    <input v-model="nameField" placeholder="Name" type="text" ref="nameInput">
    <input v-model="descriptionField" placeholder="Description">
    <input v-model="stackField" placeholder="Stack">
    <input v-model="chargeField" placeholder="Charge">
  </div>

  <div><button type="button" @click="save()">Save</button></div>

  <div>
    <table>
      <thead>
      </thead>
      <tbody>
      <tr v-if="items.length === 0">
        <td colspan="2">No habits yet</td>
      </tr>
      <tr v-for="item in myFilterFunc(filterCrit)" :key="item.idField">
        <td>{{ item.id }}</td>
        <td>{{ item.count }}</td>
        <td>{{ item.name }}</td>
        <td>{{ item.description }}</td>
        <td>{{ item.stack }}</td>
        <td>{{ item.charge }}</td>

      </tr>
      <tr>
        <td>{{ idField }}</td>
        <td>{{ countField }}</td>
        <td>{{ nameField }}</td>
        <td>{{ descriptionField }}</td>
        <td>{{ stackField }}</td>
        <td>{{ chargeField }}</td>
      </tr>
      </tbody>
    </table>
  </div>

  <button class="btn btn-success sticky-button" data-bs-toggle="offcanvas" data-bs-target="#HabitData" aria-controls="#HabitData">
    <i class="bi bi-person-plus-fill"></i>
  </button>
  <div class="offcanvas offcanvas-end" tabindex="-1" id="habits-create-offcanvas" aria-labelledby="offcanvas-label">
    <div class="offcanvas-header">
      <h2 id="offcanvas-label">New Habit</h2>
    </div>
    <div class="offcanvas-body">
      <form class="text-start needs-validation" id="habits-create-form" novalidate>
        <div class="mb-3">
          <label for="nameField" class="form-label">Name</label>
          <input type="text" class="form-control" id="nameField" v-model="nameField" required>
          <div class="invalid-feedback">
            Please provide the name.
          </div>
        </div>
        <div class="mb-3">
          <label for="countField" class="form-label">Count</label>
          <input type="text" class="form-control" id="countField" v-model="countField" required>
          <div class="invalid-feedback">
            Please provide the count till now.
          </div>
        </div>
        <div class="mb-3">
          <label for="descField" class="form-label">Description</label>
          <input type="text" class="form-control" id="descField" v-model="descriptionField" required>
          <div class="invalid-feedback">
            Please provide the description.
          </div>
        </div>
        <div class="mb-3">
          <label for="stackField" class="form-label">Stack</label>
          <input type="text" class="form-control" id="stackField" v-model="stackField" required>
          <div class="invalid-feedback">
            Please provide a stack.
          </div>
        </div>
        <div class="mb-3">
          <label for="chargeField" class="form-label">Charge</label>
          <select id="charge" class="form-select" v-model="chargeField" required>
            <option value="" selected disabled>Choose...</option>
            <option value="Positive">True</option>
            <option value="Negative">False</option>
            <option value="Neutral">Neutral</option>
          </select>

        </div>


        <div class="mt-5">
          <button class="btn btn-primary me-3" type="submit" @click.prevent="createHabit">Create</button>
          <button class="btn btn-danger" type="reset">Reset</button>
        </div>
      </form>
    </div>
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
      chargeField:''
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
        stack: this.stackField,
        charge: this.chargeField
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


.sticky-button {
  position: fixed;
  bottom: 20px;
  right: 20px;
  padding: 10px 15px;
  border-radius: 30px;
}
button {
  color: green;
}

table{
  margin-left: auto;
  margin-right: auto;
}


</style>
