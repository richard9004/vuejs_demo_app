<template>
  <div class="submit-form">
    <p class="h3">Edit Product</p>
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

      <button @click="updateProduct" class="btn btn-success">Update</button>
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
  name: "edit",
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

     retrieveSingleProduct() {
     // alert(this.$route.params.id);
      axios.get('http://localhost/productmanager/api-single-product/'+this.$route.params.id, 
      { headers: {
      'Content-type': 'application/x-www-form-urlencoded',
      }
      }).then(response =>{
        
          console.log(response);
         this.product.name=response.data.name;
         this.product.price=response.data.price;
         this.product.gst=response.data.gst;
         this.product.gst_type=response.data.gst_type;
         this.product.quantity=response.data.quantity;

      });
    },
    updateProduct() {

      

      var bodyFormData = new FormData();
      bodyFormData.append('name', this.product.name);
      bodyFormData.append('price', this.product.price);
      bodyFormData.append('gst', this.product.gst);
      bodyFormData.append('gst_type', this.product.gst_type);
      bodyFormData.append('quantity', this.product.quantity);
      bodyFormData.append('update_id', this.$route.params.id);
  
      axios.post('http://localhost/productmanager/api-update-product', 
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
    
   
 
  },
   mounted:function() {
    this.retrieveSingleProduct();
    }
};
</script>

<style>
.submit-form {
  max-width: 300px;
  margin: auto;
}
</style>