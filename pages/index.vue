<template>
  <div class="body">
    <div class="card">
      <div>
        <div class="basket mx-5 mt-3">
          <button @click="showicons()">
          <h6 class="ordersize">{{ productcart.length}}</h6>
          <b-icon-basket2-fill class="icon"></b-icon-basket2-fill>
        </button>
        </div>
        
        <div class="mx-5">
        
            <div class="filter mb-5">
                <button v-for="(filter, index) in filterdata"
                  @click="filterCategory(filter.key, index)"
                  :class="{ activee: active[index] }"
                >
                  {{ filter.value }}
                </button>
              </div>
       
          <div class="row justify-content-between mx-2">
            <div class="d-flex justify-content-around mb-3">
              <div class="pr-2">{{ this.products.length }}</div>
              <div class="pr-2">products</div>
              <b-icon-grid-fill
                class="pl-1 gridfill colorgreen"
              ></b-icon-grid-fill>
              <b-icon-list-ul class="pl-1 gridfill"></b-icon-list-ul>
            </div>
            <div >Sort By <span class="newfirst">Newest First</span></div>
          </div>
          <hr />
          <div class="row">
            <div
              v-for=" (product,index) in this.products"
              class="col-12 col-md-6 col-lg-3 col-xl-2 mb-4"
            >
              <productCard
                :data="product"
                @addproduct="addtocart"
                :change="change"
                :index="index"
                :counter="counter"
              ></productCard>
            </div>
          </div>
        </div>

        <cart
          :showicon="showicon"
          :productcart="productcart"
          :subtotal="subtotal"
          @changeQuantity="changeQuantity"
          @changecolor="changecolor($event)"
        ></cart>
      </div>
      
    </div>
  </div>
</template>

<script>
import { BIconlistUl, BIcongridFill, BIconbasket2Fill } from "bootstrap-vue";
import { productCard } from "@/components/productCard";
import { cart } from "@/components/cart";
export default {
  component: {
    productCard,
    BIcongridFill,
    BIconlistUl,
    cart,
    BIconbasket2Fill,
  },
  data() {
    return {
      products: [],
      allproducts: [],
      active: ["true"],
      productcart: [],
      filterdata: [
        { key: "all", value: "All" },
        { key: "food", value: "Food" },
        { key: "non-food", value: "Non Food" },
        { key: "fashion", value: "Fashion&Linen" },
      ],
      subtotal: 0,
      showicon: false,
      mount: 1,
      change:[],
      counter:0
    };
  },
  mounted() {
    this.$axios.$get("http://localhost:5000/products").then((res) => {
      this.products = res;
      this.allproducts = res;
    });
    this.total();
  },

  methods: {
    changecolor(val){
      let x =JSON.parse(localStorage.getItem("Wishlist"))
      let y = x.find(el=>el.id == val)
      if(y){
        this.change[val]=true
      }
      
    },
    filterCategory(vall, i) {
      this.active = this.active.map((el) => {
        el == "true" ? "false" : "false";
      });
      if (vall == "all") {
        this.products = this.allproducts;
        this.active[i] = true;
      } else {
        this.products = this.allproducts;
        this.active[i] = true;
        this.products = this.products.filter((el) => el.category == vall);
      }
    },
    total() {
      this.subtotal=0
      this.productcart=[]
      if ("cart" in localStorage) {
        this.productcart = JSON.parse(localStorage.getItem("cart"));
        for (let i of this.productcart)
          this.subtotal += i.product.price * i.mount;
      }
    },
    changeQuantity(){
      this.total()
    },
    addtocart(val) {
      let product = {
        id: val.id,
        name: val.name,
        thumbnail: val.thumbnail,
        price: val.price,
      };

      if (val.weights != undefined) this.$router.push(`/products/${val.id}`);
      else {
        if ("cart" in localStorage) {
          this.productcart = [];
          this.productcart = JSON.parse(localStorage.getItem("cart"));
          let exist = this.productcart.find((el) => el.product.id == val.id);
          if (!exist) {
            this.productcart.push({ product, mount: this.mount });
            localStorage.setItem("cart", JSON.stringify(this.productcart));
          }
          else{
            exist.mount++
            console.log(exist.mount) 
            localStorage.setItem("cart", JSON.stringify(this.productcart));
          }
          
        } else {
          this.productcart.push({ product, mount: this.mount });
          localStorage.setItem("cart", JSON.stringify(this.productcart));
        }
        this.total();
        let body = "Success";
            this.$bvToast.toast(body, {
              //  title: this.$t('form.postingRequest.selectedDestination.fillCity'),
              variant: 'success',
              toaster: 'b-toaster-top-center',
              solid: true,
              autoHideDelay: 2000,
            });
      }
      
    },
    showicons() {
      if (this.showicon) this.showicon = false;
      else this.showicon = true;
    },
  },
};
</script>
<style scoped>
.body {
  overflow-x: hidden;
}
.basket{    
  display: flex;
  justify-content: end;
  z-index: 44;
  position: relative;
}
.card button{
 
    background-color: transparent;
    border: none;
    outline: none;

}
.filter{
  display: flex;
    justify-content: end;
}
.card h6{margin-bottom: -10%;}
.icon {    
   
    font-size: 40px;
    z-index: 3;
    color: #007a5e;
}
.basket button {
  background-color: transparent;
  border: none;
  outline: none;
}

.activee {
  color: white;
  background-color: #e46a42 !important;
}
.colorgreen {
  color: #1f9982;
}

.filter button {
  background-color: rgb(241 243 244);
  border: none;
  outline: none;
  padding: 5px 10px;
  margin: 2px;
}
.newfirst {
  color: #1f9982;
  font-weight: 700;
}

.gridfill {
  height: 25px;
  width: 35px;
}
.ordersize {
  color: #df1818;
  z-index: 4;
}
</style>
