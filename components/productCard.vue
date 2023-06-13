<template>
    <div class="product">
        <div class="position-relative " style="width: 15rem;">
            
            <b-icon-heart class="heart" :class="{ redheart:changeheartt[data.id] }"  @click="changeHeart(data.id)"></b-icon-heart>
            <p>{{ changeheartt[data.id] }} {{ active[data.id] }}</p>
            <nuxt-link  :to="`/products/${data.id}`">
                <div class="image" >
                <img class="card-img-top w-100 h-100" :src="data.thumbnail" :alt="data.slug">
            </div>
            </nuxt-link>
          
            
            <div class="card-body">
                <p class="card-title">{{data.name}}</p>
                <h6 class="card-text"> {{ data.price }} EGP</h6>
                <button href="#" class="btn addcart " @click="addcart(data)">add to cart</button>
            </div>
        </div>
    </div>
</template>

<script>
import { BIconHeart } from "bootstrap-vue";
export default {
    component:{
        BIconHeart
    },
 props:['data','change'],
 mounted(){
    this.active = this.active.map((el) => {
        el == "true" ? "false" : "false";
      });
 },
 data(){
    return{
        d:"sdsd",
        active: ["true"],
        changeheartt:[],
        
    }
 },
 methods:{
    addcart(data){
        this.$emit("addproduct",data)
    },
    changeHeart(index){
        if(this.changeheartt[index]) {
            this.changeheartt[index]=false 
            console.log(this.changeheartt[index])
        }
        else  {
            this.changeheartt[index]=true
            console.log(this.changeheartt[index])
        }  

    },
 
 }
}
</script>
<style >
.heart{
position: absolute;
right: 0;
cursor: pointer;
color:#046c52;
top:10px
}
.image{
    width: 70%;
    margin: auto;
}
.addcart{
    border: 1px solid #036e57;
    border-radius: 50px;
    padding: 1px 20px;
}
.redheart{
    color: red !important;
    font-size: 20px;
    background-color: #036e57;
}
a{
    font-weight: 500 !important;
}
.product{box-shadow: 1px 1px 9px #9bcbc1;
    border-radius: 7px;}
h6{
    color:#017b62;
    border-bottom: none ;
}
.red{
    color:red
}

</style>