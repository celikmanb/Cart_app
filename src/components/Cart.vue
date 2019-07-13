<template>
    <div class="cart-wrapper">
        <h2>{{title}}</h2>
        <p v-if="!cart.length">No item in cart.</p>
        <div class="cart">
            <div class="item" v-for="(item, index) in cart">
                <div class="image">
                    <a v-bind:href="item.url">
                        <img v-bind:src="item.image" />
                    </a>
                </div>
                <div class="info">
                    <h4>{{item.name}}</h4>
                    <p class="seller">by {{item.seller}}</p>
                    <p class="status available" v-if="item.isAvailable">In Stock</p>
                    <p class="shipping" v-if="item.isEligible">Eligible for FREE Shipping & FREE Returns</p>
                    <a href="#" v-on:click="removeFromCart(index)">Delete</a>
                    <a href="#" class="secondary" v-on:click="changeCart(index)" v-if="isShoppingCart">Save for later</a>
                    <a href="#" class="secondary" v-on:click="changeCart(index)" v-if="isSavedCart">Move to cart</a>
                </div>
                <p class="price">${{item.price}}</p>
            </div>
        </div>
        <div class="subtotal" v-if="cart.length">
            Subtotal ({{cart.length}} items): <span class="price">${{cartTotal}}</span>
        </div>
    </div>

</template>

<script>
    export default {
        name: "Cart",
        props: {
            cart: {type: Array, required: true, },
            title: {type: String, required: true, },
            type: {type: String, required: true, },
        },
        methods: {
            removeFromCart(index) {
                return this.cart.splice(index,1);
            },
            changeCart(index) {
                const item = this.removeFromCart(index);
                this.$emit('ChangeCart', item[0], this.type);
            }
        },
        computed: {
            cartTotal() {
                let total = 0;
                this.cart.forEach((item) => {
                    total += parseFloat(item.price, 10);
                });
                return total.toFixed(2);
            },
            isShoppingCart() {
                return this.type == 'shoppingCart';
            },
            isSavedCart() {
                return this.type == 'savedCart';
            },
        }
    }
</script>

<style scoped>
    .price {
        color: darkred;
        position: relative;
        left: 1%;
        font-weight: bold;
        font-size: 14px;
    }
</style>