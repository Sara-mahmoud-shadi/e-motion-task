<template>
  <div v-if="this.product" class="container mt-5 productDetail">
    <div class="row">
      <div class="col-12 col-md-6 mb-5 border">
        <img class="w-100 h-100" :src="product.thumbnail" />
      </div>
      <div class="col-12 col-md-6">
        <h2>{{ product.name }}</h2>
        <h6>cold cuts</h6>
        <h5 class="my-3" v-if="!isNaN(calculatePrice)">{{ calculatePrice }} EGP</h5>
        <p class="my-3 fw-bold">Grocery Weight</p>
        <select v-model="weightvalue" class="mb-3">
          <option v-for="weight in product.weights" :value="weight">
            {{ weight }} gm
          </option>
        </select>
        <p class="mb-2 quantity">Quantity</p>
        <div>
          <div class="mount mb-4">
            <updateButtonOrder
              :quantity="mount"
              :index="proid"
              @decreas="decreasMount($event)"
              @increase="increaseMount($event)"
              @change="changeMount($event)"
            ></updateButtonOrder>
          </div>
          <button class="addtocart" @click="addCart(product)">
            Add to cart
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { updateButtonOrder } from "@/components/updateButtonOrder";
export default {
  component: {
    updateButtonOrder,
  },
  created() {
    this.id = this.$route.params.id;
  },
  mounted() {
    this.$axios.$get("http://localhost:5000/products").then((res) => {
      this.products = res;
      this.products = this.products.filter((data) => data.id == this.id);
      this.product = this.products[0];
    });
  },
  data() {
    return {
      id: null,
      products: [],
      product: [],
      mount: 1,
      weightvalue: 1000,
      productcart: [],
      totalPrice:0

    };
  },
  computed:{
    calculatePrice(){
        let priceforgm =this.product.price/1000
        let priceSelectedgm = priceforgm * this.weightvalue
        this.totalPrice = priceSelectedgm * this.mount
        console.log(this.totalPrice)
        return this.totalPrice.toFixed(2)
        
    }
  },
  methods: {
    decreasMount(val) {
      this.mount = val;
      console.log(this.mount)
    },
    increaseMount(val) {
      this.mount = val;
    },
    changeMount(val) {
      this.mount = val;
    },
    addCart(data) {
      let product = {
        id: data.id,
        name: data.name,
        thumbnail: data.thumbnail,
        price: this.totalPrice,
      };

      if ("cart" in localStorage) {
        this.productcart = [];
        this.productcart = JSON.parse(localStorage.getItem("cart"));
        let exist = this.productcart.find((el) => el.product.id == data.id);
        if (exist) {
          let existweight = this.productcart.find(
            (el) => el.weight == this.weightvalue
          );
          if (!existweight) {
            this.productcart.push({
              product,
              mount: this.mount,
              weight: this.weightvalue,
            });
            localStorage.setItem("cart", JSON.stringify(this.productcart));
          }
          else{
           
            existweight.mount=this.mount+existweight.mount
            console.log(existweight.mount)
            localStorage.setItem("cart", JSON.stringify(this.productcart));
          }
        } else {
          this.productcart.push({
            product,
            mount: this.mount,
            weight: this.weightvalue,
          });
          localStorage.setItem("cart", JSON.stringify(this.productcart));
        }
      } else {
        this.productcart.push({
          product,
          mount: this.mount,
          weight: this.weightvalue,
        });
       
      }
      localStorage.setItem("cart", JSON.stringify(this.productcart));
        let body = "Success";
            this.$bvToast.toast(body, {
              //  title: this.$t('form.postingRequest.selectedDestination.fillCity'),
              variant: 'success',
              toaster: 'b-toaster-top-center',
              solid: true,
              autoHideDelay: 2000,
            });
    },
  
  },
};
</script>

<style>
.productDetail h6 {
  padding: 8px;
  border-bottom: 1px solid;
  display: inline-block;
}
.productDetail h5 {
  color: #017b62;
}
.quantity {
  font-weight: 500;
  font-size: 1.2em;
}

.btnplus {
  border-radius: 0 10px 10px 0;
}
.btnmin {
  border-radius: 10px 0 0 10px;
}
.addtocart {
  margin-left: -20%;
  background-color: #3a7b62;
  color: white;
  padding: 6px 35px;
  border-radius: 10px;
  outline: none;
  border: none;
}
input {
  width: 20%;
  margin-left: -7px !important;
  margin-right: -7px !important;
}
</style>
