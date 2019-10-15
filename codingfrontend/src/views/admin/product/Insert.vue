<template>
  <div class="container">
    <h1>Product Insert</h1>
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
        </div>
      </div>
      <div class="form-group row">
        <label for="price" class="col-sm-2 col-form-label">Price</label>
        <div class="col-sm-10">
          <textarea id="price" class="form-control" cols="30" rows="10" v-model="formSubmitData.price"></textarea>
        </div>
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
        name: '',
        brand: '',
        image: document.querySelector('#image').files[0],
        price: '',
        desc: ''
      }
    }
  },
  methods: {
    submitData () {
      var self = this
      var postData = {
        name: this.formSubmitData.name,
        brand: this.formSubmitData.brand,
        image: document.querySelector('#image').files[0],
        price: '',
        desc: ''
      }
      var formData = new FormData()
      for (var key in postData) {
        formData.append(key,postData[key])
      }

      this.$axios.post('http://localhost:3000/insert', formData).then(function (response) {
        if (response.status === 201) {
          // pindah halaman ke list produk
          self.$router.push('/admin/product')
        }
        console.log(response)
      }).catch(function (eror) {
        console.log (eror)
      })
    }
  }
}
</script>