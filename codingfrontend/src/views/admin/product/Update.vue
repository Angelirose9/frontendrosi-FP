<template>
  <div class="container">
    <h1>Product Update</h1>
    <form method="post" @submit.prevent="submitData()">
      <div class="form-group row">
        <label for="name" class="col-sm-2 col-form-label">Name</label>
        <div class="col-sm-10">
          <input type="text" class="form-control" id="name" v-model="formSubmitData.name">
        </div>
      </div>
      <div class="form-group row">
        <label for="brand" class="col-sm-2 col-form-label">Brand</label>
        <div class="col-sm-10">
          <textarea id="brand" class="form-control" cols="30" rows="10" v-model="formSubmitData.brand"></textarea>
        </div>
      </div>
      <div class="form-group row">
        <label for="image" class="col-sm-2 col-form-label">Image</label>
        <div class="col-sm-10">
          <input type="file" id="image">
          <img :src="formSubmitData.image" alt="">
        </div>
       <div class="form-group row">
        <label for="price" class="col-sm-2 col-form-label">Price</label>
        <div class="col-sm-10">
          <textarea id="price" class="form-control" cols="30" rows="10" v-model="formSubmitData.price"></textarea>
        </div>
       <div class="form-group row">
        <label for="desc" class="col-sm-2 col-form-label">Description</label>
        <div class="col-sm-10">
          <textarea id="desc" class="form-control" cols="30" rows="10" v-model="formSubmitData.desc"></textarea>
        </div>
      </div>
      <div class="form-group row">
        <button type="button" class="btn btn-danger">Cancel</button>
        <button type="submit" class="btn btn-primary">Submit</button>
      </div>
    </form>
  </div>
</template>
<script>
export default {
  data () {
    return {
      formSubmitData: {
        name: 'Onero',
        brand: 'Greenlake City',
        image: '',
        price: '',
        desc: ''
      },
      id: this.$route.params.id
    }
  },
  created() {
    this.getDetailData()
  },
  methods: {
    getDetailData () {
      var self = this
      this.$axios.get( `http://localhost:3000/findOne/${this.id}`).then
      (function (response) {
        if (response.status === 200) {
          self.formSubmitData.name = response.data.name
          self.formSubmitData.image = response.data.image
        }
      }).catch(function (error) {
        console.log(error)
      })
    },
    submitData () {
      var self = this
      var post = {
        name: this.formSubmitData.name,
        brand: this.formSubmitData.brand,
        image: document.querySelector('#image').files[0],
        price: this.formSubmitData.price,
        desc: this.formSubmitData.desc
      }

      var formData = new FormData ()
      for (var key in post) {
        formData.append (key, post[key])
      }
      this.$axios.patch(`http://localhost:3000/updateOne/${this.id}`, formData).then
      (function (response) {
        if (response.status === 200) {
          self.$router.push('/admin/product')
        }
      }).catch(function (error) {
        console.log (error)
      })
    }
  }
}
</script>