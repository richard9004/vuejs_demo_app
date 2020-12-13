
<template>
  <div class="list row">
 <p class="h3">Product Listing</p>
   <div class="col-sm-4">
       <input type="text" name="" placeholder="Search By Product Name" class="form-control"  v-model="search_by_product_name">
   </div>

   <div class="col-sm-3">
       <a class="btn btn-success" @click="search_by_product">Search</a>
   </div>

<div class="col-sm-12" v-if="message!=''">
    <p style="color:green">{{message}}</p>
</div>

<div class="col-sm-12" v-if="err_message!=''">
    <p style="color:green">{{err_message}}</p>
</div>


    <table class="table" style="margin-top:10px">
        <tr>
         
          <th>Name</th>
          <th>Price</th>
          <th>GST</th>
          <th>Action</th>
        </tr>

        <tr  v-for="product in products" :key="product.name">
           <td>{{ product.name }}</td>
           <td>{{ product.price }}</td>
           <td>{{ product.gst }}</td>
           <td> <a data-toggle="modal" data-target="#myModal" class="btn btn-sm btn-info"  @click="viewProduct(product)">View</a> | <router-link :to="{ name: 'edit', params: { id: product.id }}" class="btn btn-sm btn-primary">Edit</router-link> |  <a data-toggle="modal" data-target="#myModal2" class="btn btn-sm btn-danger"  @click="setDeleteId(product)">Delete</a></td>
          
        </tr>
    </table>




<!-- Modal -->
<div id="myModal2" class="modal fade" role="dialog">
  <div class="modal-dialog">

    <!-- Modal content-->
    <div class="modal-content">
      <div class="modal-header">
        
        <h4 class="modal-title">Delete Product</h4>
      </div>
      <div class="modal-body">
        <p>Are you sure you want to delete this product?</p>
      </div>
      <div class="modal-footer">
        <button type="button" class="btn btn-success" @click="deleteProduct" data-dismiss="modal">Yes</button>
        <button type="button" class="btn btn-danger" data-dismiss="modal">No</button>
      </div>
    </div>

  </div>
</div>

    <!-- The Modal -->
<div class="modal" id="myModal">
  <div class="modal-dialog">
    <div class="modal-content">

      <!-- Modal Header -->
      <div class="modal-header">
        <h4 class="modal-title">View Product</h4>
        <button type="button" class="close" data-dismiss="modal">&times;</button>
      </div>

      <!-- Modal body -->
      <div class="modal-body">
        <div class="row">
           <table class="table">
                <tr>
                   <th>Product Name</th>
                   <td>{{view_product_name}}</td>
                </tr>

                <tr>
                   <th>Price</th>
                   <td>{{view_price}}</td>
                </tr>

                <tr>
                   <th>GST</th>
                   <td>{{view_gst}}</td>
                </tr>


           </table>
        </div>
      </div>

      <!-- Modal footer -->
      <div class="modal-footer">
        <button type="button" class="btn btn-danger" data-dismiss="modal">Close</button>
      </div>

    </div>
  </div>
</div>

    
  </div>
</template>

<script>

import axios from "axios";

export default {
  name: "product-list",
  data() {
    return {
      products: [],
      currentProduct: null,
      currentIndex: -1,
      title: "",
      view_gst:"",
      view_product_name:"",
      view_price:"",
      search_by_product_name:"",
      delete_product_id:"",
      message:"",
      err_message:""
    };
  },
  methods: {
    retrieveProducts() {
      axios.get('http://localhost/productmanager/api-get-products', 
      { headers: {
      'Content-type': 'application/x-www-form-urlencoded',
      }
      }).then(response =>{
        //console.log(response.data[0].msg);
         this.products=response.data

      });
    },

    setDeleteId(product){
       this.delete_product_id=product.id;
    },

    deleteProduct(){
      var bodyFormData = new FormData();
      bodyFormData.append('id', this.delete_product_id);
    
        axios.post('http://localhost/productmanager/api-delete-product', 
          bodyFormData,
      { headers: {
      'Content-type': 'application/x-www-form-urlencoded',
      }
      }).then(response =>{
        console.log(response);
        if(response.data.msg=="success"){
           this.message="Product Deleted";
           this.retrieveProducts();
        }else{
          this.err_message="Error in deleting the product";
        }
        
      });
    },

    search_by_product(){
      console.log(this.search_by_product_name);
      var bodyFormData = new FormData();
      bodyFormData.append('name', this.search_by_product_name);
    
        axios.post('http://localhost/productmanager/api-search-products', 
          bodyFormData,
      { headers: {
      'Content-type': 'application/x-www-form-urlencoded',
      }
      }).then(response =>{
        //console.log(response.data[0].msg);
         this.products= [];
         this.products=response.data;
         console.log(this.products);

      });
    },

  

    viewProduct(product){
       this.view_product_name=product.name;
       this.view_price=product.price;
       this.view_gst=product.gst;
    },

  },
  mounted() {
    this.retrieveProducts();
  }
};
</script>

<style>
.list {
  text-align: left;
  max-width: 750px;
  margin: auto;
}
</style>