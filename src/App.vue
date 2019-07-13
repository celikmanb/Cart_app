<template>
  <div id="app">

    <p v-if="isLoading">Loading...</p>

    <Cart
      :cart="cart"
      title="Shopping Cart"
      type="shoppingCart"
      @ChangeCart="handleItemChange"
    >
    </Cart>

    <Cart
      :cart="saved"
      title="Saved for Later"
      type="savedCart"
      @ChangeCart="handleItemChange"
    >
    </Cart>

    <button class="buy">Add to Basket</button>

  </div>
</template>

<script>
import Cart from './components/Cart'
import json from './data'

export default {
  name: 'app',
  data () {
    return {
      isLoading: true,
      cart: [],
      saved: [],
      json: json
    }
  },
  components: {
    Cart
  },
  methods: {
    handleItemChange(item, cartType) {
      if (cartType === 'shoppingCart') {
        this.saved.push(item);
      }
      else {
        this.cart.push(item);
      }
    }
  },
  created() {
    fetch("data.json")
            .then(r => { return this.json })
            .then(res => {
                this.isLoading = false;
                this.cart = res.cart;
                this.saved = res.saved;
            });

  }

}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
body {
  font-size: 13px;
  line-height: 19px;
  color: #111;
  font-family: Arial, sans-serif;
  background: #FFF;
}

h4, p {
  padding: 0;
  margin: 0;
}

.item {
  border-bottom: 1px solid #DDD;
  padding: 15px 0;
  overflow: hidden;
}

.item:first-child {
  border-top: 1px solid #DDD;
}

.item img {
  width: 100px;
  height: 100px;
}

h4 {
  color: #0066c0;
  font-size: 16px;
  line-height: 1.255;
}

.image, .info {
  float: left;
}

.image {
  margin-right: 20px;
}

.seller {
  font-size: 13px;
  line-height: 19px;
}

.status.available {
  color: #008a00;
  font-size: 12px;
  line-height: 1.5;
}

.shipping {
  color: #555;
  font-size: 12px;
  line-height: 1.5;
}

.info a {
  color: #0066c0;
  font-size: 12px;
  text-decoration: none;
  line-height: 24px;
}

.saved-header {
  margin-top: 50px;
}

a.secondary {
  padding-left: 5px;
  margin-left: 3px;
  border-left: 1px solid #CCC;
}
.buy {
  background-color: #008a00;
  color: white;
  border: 1px solid #008a00;
  border-radius: 4px;
  width: 100px;
  height: 30px;
  box-shadow: 1px 3px #555555;
  position: relative;
  left: 82%;
}

</style>
