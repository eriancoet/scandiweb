<template>
  <div>
  <navbar-component title="Product List"></navbar-component>
  <div id="opt">
    <router-link to="" v-on:click="submit"><button class="btn">Save</button></router-link>
    <router-link to="/"><button class="btn">Cancel</button></router-link>
  </div>

  <form class="container" id="product_form">
    <div id="simple-form">
      <div class="container-inp">
        <div class="sub-container-inp">
          <input class="inp" id="sku" v-model="productSku" placeholder="SKU">
          <div class="tag-inp">SKU</div>
          <div class="error-inp" v-if="skuErr">{{ skuErr }}</div>
        </div>
      </div>
      <div class="container-inp">
        <div class="sub-container-inp">
          <input class="inp" id="name" v-model="productName" placeholder="Name">
          <div class="tag-inp">Name</div>
          <div class="error-inp" v-if="nameErr">{{ nameErr }}</div>
        </div>
      </div>
      <div class="container-inp">
        <div class="sub-container-inp">
          <input class="inp" id="price" v-model="productPrice" placeholder="Price">
          <div class="tag-inp">Price($)</div>
          <div class="error-inp" v-if="priceErr">{{ priceErr }}</div>
        </div>
      </div>

      <div class="container-inp">
        <div class="sub-container-inp">
          <select class="inp" name="productType" id="productType" v-model="selected">
            <option value="DVD" id="DVD">DVD</option>
            <option value="Furniture" id="Furniture">Furniture</option>
            <option value="Book" id="Book">Book</option>
          </select>
          <div class="tag-inp">Type</div>
          <div class="error-inp" v-if="selectedErr">{{ selectedErr }}</div>
        </div>
      </div>
    </div>

    <div id="extended-form">

      <div id="dvd-form" v-if="selected=='DVD'">
        <div class="container-inp">
          <div class="sub-container-inp">
            <input class="inp" id="size" v-model="productSize" placeholder="Size">
            <div class="tag-inp">Size(MB)</div>
            <div class="error-inp" v-if="sizeErr">{{ sizeErr }}</div>
          </div>
        </div>
        <div class="sub-container-inp">
          <p class="description">Please provide storage capacity in megabyte format</p>
        </div>
      </div>

      <div id="furniture-form" v-if="selected=='Furniture'">
        <div class="container-inp">
          <div class="sub-container-inp">
            <input class="inp" id="height" v-model="productHeight" placeholder="Size">
            <div class="tag-inp">Height(CM)</div>
            <div class="error-inp" v-if="heightErr">{{ heightErr }}</div>
          </div>
        </div>
        <div class="container-inp">
          <div class="sub-container-inp">
            <input class="inp" id="width" v-model="productWidth" placeholder="Width">
            <div class="tag-inp">Width(CM)</div>
            <div class="error-inp" v-if="widthErr">{{ widthErr }}</div>
          </div>
        </div>
        <div class="container-inp">
          <div class="sub-container-inp">
            <input class="inp" id="length" v-model="productLength" placeholder="Length">
            <div class="tag-inp">Length(CM)</div>
            <div class="error-inp" v-if="lengthErr">{{ lengthErr }}</div>
          </div>
        </div>
        <div class="sub-container-inp">
          <p class="description">Please provide dimensions in HxWxLx format and centimeters</p>
        </div>
      </div>

      <div id="book-form" v-if="selected=='Book'">
        <div class="container-inp">
          <div class="sub-container-inp">
            <input class="inp" id="weight" v-model="productWeight" placeholder="Weight">
            <div class="tag-inp">Weight(KG)</div>
            <div class="error-inp" v-if="weightErr">{{ weightErr }}</div>
          </div>
        </div>
        <div class="sub-container-inp">
          <p class="description">Please provide weight in kilograms</p>
        </div>
      </div>

    </div>
  </form>
  </div>
</template>

<script>
import NavbarComponent from "@/components/NavbarComponent";
import axios from "axios";

export default {
  name: "AddProductView",
  components: {
    NavbarComponent,
  },
  data: function () {
    return {
      res: null,
      resErr: "",

      productData: {},

      selected: "",
      productSku: "",
      productName: "",
      productPrice: "",
      productSize: "",
      productHeight: "",
      productWidth: "",
      productLength: "",
      productWeight: "",

      skuErr: false,
      nameErr: false,
      priceErr: false,
      selectedErr: false,
      sizeErr: false,
      heightErr: false,
      widthErr: false,
      lengthErr: false,
      weightErr: false,
    }
  },
  methods: {
    submit: function () {
      this.validateForm();

      axios.post("https://192.236.162.91/api/product/add-product", this.productData)
          .then((response) => {
            if (response.data) {
              window.location.href = "/";
            }
          })
          .catch((e) => {
            if (e.response.data.skuErr) {
              this.skuErr = "Please submit a valid unique sku";
            }
          })
    },
    valSku: function () {
      if (this.productSku.length === 0) {
        this.skuErr = "Please submit required SKU data";
      } else if (!(/^\S+$/.test(this.productSku))) {
        this.skuErr = "Please submit an unique SKU without spaces"
      } else {
        this.skuErr = false;
      }
    },
    valName: function () {
      if (this.productName.length === 0) {
        this.nameErr = "Please submit required Name data";
      } else {
        this.nameErr = false;
      }
    },
    valPrice: function () {
      if (this.productPrice.length === 0) {
        this.priceErr = "Please submit required Price data";
      } else if (!(/^\d+(\.\d{2})?$/.test(this.productPrice))) {
        this.priceErr = "Please submit price in n.xx format";
      } else {
        this.priceErr = false;
      }
    },
    valType: function() {
      if (this.selected === "") {
        this.selectedErr = "Please select a product type";
      } else {
        this.selectedErr = false;
      }
    },
    valSize: function () {
      if (this.productSize.length === 0) {
        this.sizeErr = "Please submit required Size data";
      } else if (!(/^\d+$/.test(this.productSize))) {
        this.sizeErr = "Please submit Size data in n format";
      } else {
        this.sizeErr = false;
      }
    },
    valHeight: function () {
      if (this.productHeight.length === 0) {
        this.heightErr = "Please submit required Height data";
      } else if (!(/^[1-9]\d*$|^[0-9]+\.[0-9]+$/.test(this.productHeight))) {
        this.heightErr = "Please submit Height data in n or n.n format";
      } else {
        this.heightErr = false;
      }
    },
    valWidth: function () {
      if (this.productWidth.length === 0) {
        this.widthErr = "Please submit required Width data";
      } else if (!(/^[1-9]\d*$|^[0-9]+\.[0-9]+$/.test(this.productWidth))) {
        this.widthErr = "Please submit Width data in n or n.n format";
      } else {
        this.widthErr = false;
      }
    },
    valLength: function () {
      if (this.productLength.length === 0) {
        this.lengthErr = "Please submit required Length data";
      } else if (!(/^[1-9]\d*$|^[0-9]+\.[0-9]+$/.test(this.productLength))) {
        this.lengthErr = "Please submit Length data in n or n.n format";
      } else {
        this.lengthErr = false;
      }
    },
    valWeight: function () {
      if (this.productWeight.length === 0) {
        this.weightErr = "Please submit required Weight data";
      } else if (!(/^[1-9]\d*$|^[0-9]+\.[0-9]+$/.test(this.productWeight))) {
        this.weightErr = "Please submit Weight data in n or n.n format";
      } else {
        this.weightErr = false;
      }
    },

    validateForm: function () {
      this.valSku();
      this.valName();
      this.valPrice();
      this.valType();
      this.valSize();
      this.valHeight();
      this.valWidth();
      this.valLength();
      this.valWeight();

      this.productData = {
        sku: this.productSku,
        name: this.productName,
        price: this.productPrice,
        type: this.selected,
        size: this.productSize,
        height: this.productHeight,
        width: this.productWidth,
        length: this.productLength,
        weight: this.productWeight,
      };
    }
  }
}
</script>

<style scoped>
  #opt {
    position: fixed;

    top: 2vh;
    right: 2vw;

    background-color: transparent;

    float: right;
  }
  .btn {
    margin-left: 5pt;

    height: 95%;
    width: auto;

    font-size: 1.5vh;

    border-radius: 4px 4px 4px 4px;
    border: 2pt solid rgb(20, 20, 50);
    color: rgb(20, 20, 50);
    background-color: transparent;

    font-weight: bold;
    text-align: center;
    vertical-align: middle;
    line-height: 250%;

    padding-left: 4pt;
    padding-right: 4pt;

    transition-duration: 0.25s;
  }
  .btn:hover {
    border-radius: 4px 4px 4px 12px;

    border: 2pt solid rgb(20, 20, 50);
    color: white;
    background-color: rgb(20, 20, 50);
  }

  .container {
    background-color: transparent;
    z-index: -1;
    position: fixed;

    overflow-y: hidden;

    width: 450pt;

    margin-top: 4vh;
    left: 50%;
    transform: translateX(-50%);

  }

  .container-inp {
    position: relative;
    display: inline-block;

    background-color: transparent;

    margin-bottom: 8pt;

    width: 100%;
  }

  .sub-container-inp {
    position: relative;

    float: right;
    right: 10%;

  }
  .inp {
    position: relative;

    font-family: Roboto;
    font-size: 12pt;

    border: none;
    border-radius: 8pt;
    padding-left: 10pt;


    color: rgb(20, 20, 50);
    background-color: lavender;

    float: right;
    margin-left: 8pt;

  }
  select > option {
    font-family: Roboto;
    font-size: 12pt;
    border-radius: 8pt;
    border: none;

    background-color: lavender;
    color: rgb(20, 20, 50);
  }
  .tag-inp {
    position: relative;

    font-family: Roboto;
    font-size: 12pt;

    color: rgb(20, 20, 50);

    float: right;
  }

  .description {
    position: relative;

    font-family: Roboto;
    font-size: 10pt;

    float: right;
    width: 80%;

    color: lightsteelblue;

    border: 1pt solid lightsteelblue;
    border-radius: 8pt;

    padding: 4pt;
  }

  #simple-form {
    position: relative;
    float: left;

    top: 10%;

    padding-top: 4pt;

    width: 50%;

  }

  #extended-form {
    position: relative;
    float: left;

    top: 10%;

    padding-top: 4pt;

    width: 50%;

  }

  .error-inp {
    position: relative;
    width: auto;
    float: right;

    margin-top: 8pt;

    border: 1pt solid rgb(250, 100, 100);
    color: rgb(250, 100, 100);
    border-radius: 8pt;
    padding: 4pt;
  }

  @media only screen and (max-width: 550pt) {
    .container {
      width: 250pt;
    }

    #simple-form {
      width: 100%;
    }
    #extended-form {
      width: 100%;
      margin-top: 10pt;
    }
  }
</style>