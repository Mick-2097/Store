<script>
export default {
    name: 'CartPage'
}
</script>

<template>
<div v-if="isLoggedIn" v-show="isCart" class="cart-card">
    <h1 v-show="cartArray.length > 0" class="cart-title">Cart</h1>
    <h1 v-show="cartArray.length === 0" class="empty">Empty</h1>
    <p v-show="cartArray.length > 0" class="cart-total">
        Total ${{cartTotal.toFixed(2)}}
        <button class="checkout">Checkout</button>
    </p>
    <transition-group tag="ul" name="cart" @click="clickCartItem" class="cart-list" appear mode="out-in">
        <li v-for="item in cartArray" :key="item.id" :data-index="item.id">
            <div class="cart-image-spacer">
                <img class="item-image" :src="item.image" alt="" :id="'item-' + cartArray.indexOf(item)">
            </div>
            <div class="cart-spacer">
                <p>{{item.title.slice(0, 16)}}</p>
                <p class="cart-price">$ {{item.price.toFixed(2)}}</p>
            </div>
            <img class="delete-item" src="/src/assets/close-icon.png" alt="close button" :id="'item-' + cartArray.indexOf(item)">
        </li>
    </transition-group>
</div>
</template>

<style>

</style>