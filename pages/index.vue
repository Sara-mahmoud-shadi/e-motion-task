<template>
  <div class="body">
    <div class="row">
      <div class="col-11">
        <div class="m-5">
          <div class="row justify-content-end mb-4">
            <div class="filter">
              <template v-for="(filter, index) in filterdata">
                <button
                  @click="filterCategory(filter.key, index)"
                  :class="{ activee: active[index] }"
                >
                  {{ filter.value }}
                </button>
              </template>
            </div>
          </div>
          <div class="row justify-content-between mx-2">
            <div class="d-flex justify-content-around">
              <div class="pr-2">{{ this.products.length }}</div>
              <div class="pr-2">products</div>
              <b-icon-grid-fill
                class="pl-1 gridfill colorgreen"
              ></b-icon-grid-fill>
              <b-icon-list-ul class="pl-1 gridfill"></b-icon-list-ul>
            </div>
            <div>Sort By <span class="newfirst">Newest First</span></div>
          </div>
          <hr />
          <div class="row">
            <div
              v-for=" (product,index) in this.products"
              class="col-12 col-md-6 col-lg-3 mb-4"
            >
              <productCard
                :data="product"
                @addproduct="addtocart"
                :change="change"
                :index="index"
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
      <div class="col-1 basket position-relative">
        <button @click="showicons()">
          <h6 class="ordersize">{{ productcart.length}}</h6>
          <b-icon-basket2-fill class="icon"></b-icon-basket2-fill>
        </button>
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
      change:[]
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
          console.log(this.productcart[0].product.price)
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
        } else {
          this.productcart.push({ product, mount: this.mount });
          localStorage.setItem("cart", JSON.stringify(this.productcart));
        }
        this.total();
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
.total {
  font-weight: 700;
  font-size: 1.2em;
}

.icon {
  position: absolute;
  top: 60px;
  right: 40px;
  font-size: 40px;
  z-index: 3;
  color: #007a5e;
}
.ordersize {
  position: relative;
  z-index: 3;
  right: 0;
}
.mount {
  display: inline-block !important;
}
.basket {
  background-color: #dfe0e1;
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
  position: absolute;
  top: 5% !important;
  right: 49%;
  color: #df1818;
}
</style>
