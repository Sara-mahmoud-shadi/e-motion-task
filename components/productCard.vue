<template>
    <div class="product">
        <div class="position-relative " style="width: 15rem;">
            <b-icon-heart class="heart"></b-icon-heart>
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
    console.log(this.change)
 },
 methods:{
    addcart(data){
     
        this.$emit("addproduct",data)
        
    }
 }
}
</script>
<style >
.heart{
position: absolute;
right: 0;
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