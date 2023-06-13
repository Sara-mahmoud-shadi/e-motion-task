<!-- Please remove this file from your project -->
<template>
  <div>
    <div
      class="cart container"
      :class="[this.showicon == true ? 'displayshow' : '']"
    >
      <div class="row justify-content-between m-4">
        <div class="fs">Subtotal</div>
        <div class="fs">{{ subtotal.toFixed(2) }} EGP</div>
      </div>
      <nuxt-link to="/checkOut" class="btncheck mb-4">Check out</nuxt-link>
      <div class="container" v-for="(productcar, index) in this.productcart">
        <div class="row justify-content-between containn">
          <div class="col-3">
            <img class="w-100 h-100" :src="productcar.product.thumbnail" />
          </div>
          <div class="col-9">
            <p>{{ productcar.product.name }}</p>
            <span class="price"
              >{{ productcar.product.price.toFixed(2) * productcar.mount }} EGP</span
            >
            <span class="price" v-if="productcar.weight">
              / {{ productcar.weight }} gm</span
            >

            <div class="d-flex justify-content-between container">
              <updateButtonOrder
                :quantity="productcar.mount"
                :index="index"
                @totalPrice="totalPrice"
              ></updateButtonOrder>
              <div class="d-flex justify-content-between">
                <button class="remove button" @click="deleteproduct(index)">
                  Remove
                </button>
                <button class="add button" @click="Wishlist(productcar)">
                  Move to wishlist
                </button>
              </div>
            </div>
          </div>
        </div>
      </div>
      <div class="row justify-content-between container mt-2">
        <div>Subtotal</div>
        <div>{{ subtotal.toFixed(2) }} EGP</div>
      </div>
      <div class="row justify-content-between container mt-2">
        <div>Tax</div>
        <div>included</div>
      </div>
      <div class="row justify-content-between container mt-2">
        <div class="total">Order Total</div>
        <div class="total">{{ subtotal.toFixed(2) }} EGP</div>
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
  props: ["showicon", "productcart", "subtotal"],
  data() {
    return {
      subtotal:0,
      Wishlistproducts: [],
    };
  },
 
  methods: {
    totalPrice() {
      this.subtotal = 0;
      this.productcart = [];
      this.productcart = JSON.parse(localStorage.getItem("cart"));
      for (let i of this.productcart) {
        this.subtotal += i.product.price * i.mount;
      }
    },
    total() {
      this.subtotal = 0;
      if ("cart" in localStorage) {
         this.productcart = [];
        this.productcart = JSON.parse(localStorage.getItem("cart"));
        for (let i of this.productcart)
          this.subtotal += i.product.price * i.mount; 
      }
    },

    deleteproduct(i) {
      this.productcart.splice(i, 1);
      localStorage.setItem("cart", JSON.stringify(this.productcart));
      this.$emit("changeQuantity")
    //  this.total();
    },
    Wishlist(data) {
      let Wishlist = {
        id: data.product.id,
        name: data.product.name,
        price: data.product.price,
        thumbnail: data.product.thumbnail,
      };
      if ("Wishlist" in localStorage) {
        this.Wishlistproducts = [];
        this.Wishlistproducts = JSON.parse(localStorage.getItem("Wishlist"));
        let exist = this.Wishlistproducts.find(
          (el) => el.id == data.product.id
        );
        if (!exist) {
          this.Wishlistproducts.push(Wishlist);
          localStorage.setItem(
            "Wishlist",
            JSON.stringify(this.Wishlistproducts)
          );
        }
      } else {
        this.Wishlistproducts.push(Wishlist);
        localStorage.setItem("Wishlist", JSON.stringify(this.Wishlistproducts));
      }
      this.removefromcart(Wishlist.id);
      
      this.$emit("changeQuantity")
      this.$emit("changecolor",data.product.id)
    },
    removefromcart(id) {
      let newproduct = this.productcart.filter((el) => el.product.id != id);
      localStorage.setItem("cart", JSON.stringify(newproduct));
     // this.total();
    },
  },
};
</script>
<style>
.cart .button {
  background-color: transparent;
  border: none;
  outline: none;
  width: 130px;
}
.cart .price {
  color: #027a62;
  line-height: 0.1;
  margin-bottom: 60px;
  font-weight: 600;
}
.cart {
  position: fixed;
  overflow-y: auto;
  transition: right 1s ease-in;
  top: 0px;
  bottom: 0;
  right: -800px;
  width: 50%;
  background-color: white;
  border: 0.5px solid #027a62;
  z-index: 2;
}
.cart .btncheck {
  width: 70%;
  background-color: #027a62;
  text-align: center;
  display: block;
  border-radius: 100px;
  padding: 5px;
  margin: auto;
  color: white;
  font-weight: 500;
  border: none;
  outline: none;
}
.cart .containn {
  border-top: 1px solid;
  border-bottom: 0.3px solid #04735b;
  padding: 30px 2px;
}
.fs {
  font-size: 0.9em;
  font-weight: 500;
}
.displayshow {
  right: 0px !important;
}
.remove {
  color: #d51414;
  font-weight: 500;
  font-size: 0.9em;
}
.add {
  color: #027a62;
  font-weight: 500;
  font-size: 0.9em;
}
</style>
