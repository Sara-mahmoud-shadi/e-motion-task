<template>
            <div class="mount">
                <button class="btnmin" @click="decreasemount(index)">-</button>
                <input type="number" min="1" v-model="mount"  @change="changemount(index)">
                <button class="btnplus" @click="increasemount(index)">+</button>
               </div>

</template>
<script>
export default {
    props:['quantity','index'],
    mounted(){
      
      if(this.$route?.name?.includes('index')){
       
        if("cart" in localStorage){
          this.productcart=JSON.parse(localStorage.getItem("cart"))
         this.mount=this.productcart[this.index].mount
        
        }
      }
      else{
     
        this.mount=this.quantity
      }
      
    },
    data(){
        return{
           mount:1,
           
        }
    },
    methods:{
    decreasemount(index){
      
      if(this.$route?.name?.includes('index')){
      this.productcart=JSON.parse(localStorage.getItem("cart"))
      this.productcart[index].mount--
      this.mount=this.productcart[index].mount
      localStorage.setItem("cart",JSON.stringify(this.productcart))
      this.$emit("totalPrice")}
      else{
        this.mount--
        this.$emit("decreas",this.mount)
      }
      
    },
    increasemount(index){
    
      if(this.$route?.name?.includes('index')){
      this.productcart=JSON.parse(localStorage.getItem("cart"))
      this.productcart[index].mount++
      this.mount=this.productcart[index].mount
      localStorage.setItem("cart",JSON.stringify(this.productcart))
      this.$emit("totalPrice")
      }
      else{
        this.mount++
        this.$emit("increase",this.mount)
      }
     
    },
    changemount(index){
      if(this.$route?.name?.includes('index')){
      this.productcart=JSON.parse(localStorage.getItem("cart"))
      this.productcart[index].mount=this.mount
      localStorage.setItem("cart",JSON.stringify(this.productcart))
      this.$emit("totalPrice")}
      
      else
      this.$emit("change",this.mount)
    }
}
}

</script>

<style>
.mount{
    display: inline-block !important;
}
.btnplus {
    border-radius: 0 20px 20px 0;
   
}
.btnmin{
    border-radius: 20px 0 0 20px;
}
.btnmin , .btnplus {
  background-color: transparent;
    border: 1.6px solid #026e57;
    padding: 5px 10px;
    font-weight: 700;
}
input{
    width: 30% !important;
    margin-left: -7px !important;
    margin-right: -5px !important;
    text-align: center;
    border: 1.6px solid #026e57;
    padding: 5px 10px;
    
}
</style>