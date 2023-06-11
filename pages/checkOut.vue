<template>
    <div class="container mt-5">
        <h5>Total Price {{ price }}</h5>
        <div class="table-content table-responsive m-auto">
  <table class="table table-hover text-center table-responsive">
    <thead>
      <tr class="text-center"> 
        <th scope="col"> </th>
        <th scope="col">Product</th>
        <th scope="col">Price</th>
        <th scope="col">QTY</th>
        <th scope="col">SUBTOTAL</th>
       
      </tr>
    </thead>
    <tbody>
      <tr  v-for="productt in this.orderproduct">
       
        <th scope="row"><div style="width: 200px; height:40vh; margin: auto;">
          <img class="w-100 h-100" :src="productt.product.product.thumbnail">
        </div></th>
        <td >{{productt.product.product.name}}</td>
        <td>{{ productt.product.product.price }}</td>
        <td>{{ productt.product.mount }}</td>
        <td>{{productt.product.product.price * productt.product.mount}}</td>
        
      </tr>
      
    </tbody>
  </table>
</div>
    </div>
     
</template>
<script>
export default {
    data(){
        return{
            orderproduct:[],
            price:0
        }
    },
    mounted(){
      this.orderproduct= JSON.parse(localStorage.getItem("order"))
      this.totalprice()
    },
    methods:{
      totalprice(){
      
      if("order" in localStorage){
        this.orderproduct= JSON.parse(localStorage.getItem("order"))
      for(let i of this.orderproduct)
      this.price+=i.product.product.price * i.product.mount 
      }
      
    
  }
    }
}
</script>

<style scoped>

</style>