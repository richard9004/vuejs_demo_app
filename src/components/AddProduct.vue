<template>
  <div class="submit-form">
    <p class="h3">Create New Product</p>
    <div v-if="!submitted">
      <div class="form-group">
        <label for="name">Name</label>
        <input
          type="text"
          class="form-control"
          id="name"
          required
          v-model="product.name"
          name="name"
        />
      </div>

      <div class="form-group">
        <label for="price">Price</label>
        <input
          class="form-control"
          id="price"
          required
          v-model="product.price"
          name="price"
        />
      </div>

      <div class="form-group">
        <label for="gst">GST</label>
        <input
          class="form-control"
          id="gst"
          required
          v-model="product.gst"
          name="gst"
        />
      </div>

      <div class="form-group">
        <label for="gst_type">GST Type</label>
        <input
          class="form-control"
          id="gst_type"
          required
          v-model="product.gst_type"
          name="gst_type"
        />
      </div>

      <div class="form-group">
        <label for="quantity">Quantity</label>
        <input
          class="form-control"
          id="quantity"
          required
          v-model="product.quantity"
          name="quantity"
        />
      </div>

      <button @click="saveProduct" class="btn btn-success">Submit</button>
    </div>

    <div v-else>
      <h4>You submitted successfully!</h4>
      <button class="btn btn-success" @click="newProduct">Add</button>
    </div>
  </div>
</template>

<script>
//import ProductDataService from "../services/ProductDataService";
import axios from "axios";

export default {
  name: "add-product",
  data() {
    return {
      product: {
        name: "",
        price: "",
        gst: "",
        gst_type: "",
        quantity: ""
      },
      submitted: false
    };
  },
  methods: {
    saveProduct() {

    

      var bodyFormData = new FormData();
      bodyFormData.append('name', this.product.name);
      bodyFormData.append('price', this.product.price);
      bodyFormData.append('gst', this.product.gst);
      bodyFormData.append('gst_type', this.product.gst_type);
      bodyFormData.append('quantity', this.product.quantity);
  /* headers: {'Content-Type': 'multipart/form-data' }
    })
    */
      axios.post('http://localhost/productmanager/api-create-product', 
      bodyFormData, // the data to post
      { headers: {
      'Content-type': 'application/x-www-form-urlencoded',
      }
      }).then(response =>{
        //console.log(response.data[0].msg);
         if(response.data[0].msg=='success'){
this.$router.push({ name: 'products'});
         }else{
             this.submitted = false;
         }

      });

    },
    
    newProduct() {
      this.submitted = false;
      this.product = {};
    }
  }
};
</script>

<style>
.submit-form {
  max-width: 300px;
  margin: auto;
}
</style>