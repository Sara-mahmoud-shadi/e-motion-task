<template>
    <div class="product position-relative">
        <div>
            <template class="position-relative ">
                <b-icon-heart-fill class="heart" :class="{ redheart:changeheartt[index] }"  @click="changeHeart(index)"></b-icon-heart-fill>
            
            <nuxt-link  :to="`/products/${data.id}`">
                <div class="image" >
                <img class="card-img-top w-100 h-100" :src="data.thumbnail" :alt="data.slug">
            </div>
            </nuxt-link>
            </template>
            <div class="card-body position-absolute">
                <p class="card-title">{{data.name}}</p>
                <h6 class="card-text"> {{ data.price }} EGP</h6>
                <div class="position-relative">
                   
                  <button href="#" class="btn addcart position-relative" @click="addcart(data)">add to cart
                    <p class="position-absolute count" v-if="this.counter[this.index]>0"> sdsdsd {{counter[index]}}</p>
                </button>
                </div>
                
            </div>
        </div>
    </div>
</template>

<script>
import { BIconHeartFill } from "bootstrap-vue";
export default {
    component:{
        BIconHeartFill
    },
 props:['data','change','index',],
 
 data(){
    return{
        d:"sdsd",
        changeheartt:[],
        product:[],
        counter:[]
    }
 },
 
 
 methods:{
    addcart(data){
        this.$emit("addproduct",data)
        if ("cart" in localStorage) {
            this.productcart = JSON.parse(localStorage.getItem("cart"));
          let exist = this.productcart.find((el) => el.product.id == data.id);
          if (!exist) {
           this.counter[this.index]=1
           console.log(this.counter[this.index])
          }
          else{
            this.counter[this.index]=exist.mount
            console.log(this.counter[this.index])
          }
        }
    },
    changeHeart(index){
        this.changeheartt=this.changeheartt.map(el=>
        el == "true" ? "false" : "false")
        if("Wishlist" in localStorage){
            this.product=JSON.parse(localStorage.getItem("Wishlist"))
            if(this.changeheartt[index]) {
            this.changeheartt[index]=false 
            this.product=this.product.filter(el=>el.product.id != this.data.id)
            
            localStorage.setItem("Wishlist",JSON.stringify(this.product))}
        
            else  {
             
                this.changeheartt[index]=true
                this.product.push({product:this.data,mount:1})
                localStorage.setItem("Wishlist",JSON.stringify(this.product))
            }  
        }
        else{
            this.changeheartt[index]=true
            this.product.push({product:this.data,mount:1})
            localStorage.setItem("Wishlist",JSON.stringify(this.product))
        }
           
        

    },
 
 }
}
</script>
<style >
.heart{
    position: absolute;
    right: 5%;
    cursor: pointer;
    color: #046c52;
    top: 30px;
    font-size: 1.5em;
}
.image{
    width: 70%;
    margin: auto;
    height: 250px !important;
}
.card-body{
    bottom: 0 !important;
}
.addcart{
    border: 1px solid #036e57;
    border-radius: 50px;
    padding: 1px 20px;
}
.redheart{
    color: red !important;
}
a{
    font-weight: 500 !important;
}
.product{box-shadow: 1px 1px 9px #9bcbc1;
    border-radius: 7px;
    min-height: 360px !important}
h6{
    color:#017b62;
    border-bottom: none ;
}
.red{
    color:red
}
.count{
    right: -10px;
    top: -10px;
    font-size: 1em;
    color: red;
    border-radius: 50%;
    background-color: yellow;
    padding: 0 8px;
}

</style>