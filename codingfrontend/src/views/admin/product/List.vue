<template>
  <section>
    <div class="container">
      <h1>Customer List</h1>
      <table class="table">
        <tr>
          <th scope="col">Name</th>
          <th scope="col">Brand</th>
          <th scope="col">Image</th>
          <th scope="col">Price</th>
          <th scope="col" colspan="2">Action</th>
        </tr>
        <tr v-for="(customersRow, index) in customers" :key="index">
          <td v-text="customersRow.name"></td>
          <td v-text="customersRow.brand"></td>
          <td v-text="customersRow.price"></td>
          <td><img :src="customersRow.image" style="width: 100%;"></td>
          <td><router-link :to="'/admin/product/update/'+customersRow._id"><button class="btn btn-info">Edit</button></router-link></td>
          <td><button class="btn btn-danger" @click="deleteData(customersRow._id)">Delete</button></td>
        </tr>
      </table>
    </div>
  </section>
</template>

<script>
export default {
  data () {
    return {
      customers: [
        {
          name: 'Binus Kemanggisan',
          brand: 'JL. Kemanggisan'
        },
        {
          name: 'Binus Alam Sutera',
          brand: 'Alam Sutera, Tangerang'
        }
      ]
    }
  },
  created () {
    this.getData()
  },
  methods: {
    getData () {
      var self = this
      this.$axios.get('http://localhost:3000/findAll').then(function (response)
      {
       self.customers = response.data
       console.log(self.customers)
      })
    },
    deleteData (dataId) {
      var self = this
      this.$axios.delete(`http://localhost:3000/deleteOne/${dataId}`).then (function
      (response){
        if (response.status === 200) {
          self.getData()
        }
      }).catch(function (error) {
        console.log(error)
      })
    }
  }
}
</script>