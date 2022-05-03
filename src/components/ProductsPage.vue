<script>
export default {
    name: 'ProductsPage'
}
</script>

<template>
<ul @click="showDetails" v-show="isProductPage" ref="list" class="product-list">
    <li v-for="product in productsArray">
        <p class="product-title">
            {{product.title.slice(0, 40)}}...
        </p>
        <div class="spacer">
            <img :src="product.image" class="product-image" alt="#"/>
        </div>
        <a class="details-link" :id="'product-' + (product.id - 1)">
            Details...
        </a>
        <p class="product-price">
            $ {{product.price.toFixed(2)}}
        </p>
    </li>
</ul>

</template>

<style>

</style>