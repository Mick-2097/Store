<script>
import { isSimpleIdentifier } from "@vue/compiler-core"
import { ref } from "vue"

export default {
    name: 'LogIn',
    setup() {
        let users = [
            { 
                name: 'Mick', 
                username: 'Mick-2097', 
                password: 'admin', 
                shippingAddress: '1 Hope St, myTown NSW 2097', 
                email: 'Mix2444@gmail.com', 
                image: '/src/assets/mick.jpg' 
            }, 
            { 
                name: 'Max', 
                username: 'VanGuard88', 
                password: 'admin', 
                shippingAddress: '1 Home St, myTown Alanya 007', 
                email: 'max@email.com', 
                image: '/src/assets/max.jpg' 
            },
            {
                name: 'guest',
                username: 'guest',
                password: '1234',
                shippingAddress: '',
                email: '',
                image: '/src/assets/smile.png'
            }
        ]
        const user = ref()
        let productsArray = []
        const isLogInForm = ref(true)
        const isSignUpForm = ref(false)
        const isLoggedIn = ref(false)
        const isCart = ref(false)
        const isWelcome = ref(false)
        const isFarFromHome = ref(false)
        const welcome = ref()
        const isProductPage = ref(false)
        const isHomePage = ref(false)
        const showButton = ref()
        const username = ref()
        const password = ref()
        const list = ref()
        const preview = ref(false)
        const previewTitle = ref()
        const previewImage = ref()
        const previewDesc = ref()
        const previewPrice = ref()
        const title = ref('log in')
        const purchase = ref(false)
        let cartArray = []
        const purchaseItem = ref()
        const cartList = ref()
        const cartTotal = ref(0)
        const modal = ref()
        const imageInput = ref()
        const getProducts = () => {
            fetch('https://fakestoreapi.com/products')
            .then(res => res.json())
            .then(data => {
                data.forEach(x => productsArray.push(x))
                console.log('data recieved')
                })
        }
        getProducts()
        // SHOW PASSWORD
        const showHide = () => {
            if (showButton.value.innerText === 'SHOW') {
                showButton.value.innerText = 'HIDE'
                password.value.type = 'text'
            } else {
                showButton.value.innerText = 'SHOW'
                password.value.type = 'password'
            }
        }
        // TOGGLE LOG IN SIGN UP
        const signUpLogIn = () => {
            if (isLogInForm.value) {
                isLogInForm.value = !isLogInForm.value
                setTimeout(() => {
                    isSignUpForm.value = !isSignUpForm.value
                    title.value = 'Sign up'
                }, 150);
            } else {
                isSignUpForm.value = !isSignUpForm.value
                setTimeout(() => {
                    isLogInForm.value = !isLogInForm.value
                    title.value = 'Log in'
                }, 150);
            }
        }
        // LOG IN ATTEMPT
        const loginAttempt = () => {
            for (let i = 0; i < users.length; i++) {
                if (username.value.value === users[i].name && password.value.value === users[i].password) {
                    title.value = ''
                    user.value = users[i]
                    isLogInForm.value = false
                    setTimeout(() => {
                        isWelcome.value = true
                    }, 200);
                    setTimeout(() => {
                        welcome.value.classList.add('opacity1')
                        loadProducts()
                    }, 300)
                    setTimeout(() => welcome.value.classList.remove('opacity1'), 2700)
                    setTimeout(() => {
                        isWelcome.value = false
                        title.value = 'Products'
                        isLoggedIn.value = true
                        isProductPage.value = true
                    }, 3000)
                }
            }
        }
        // PRODUCT MODAL
        const showDetails = (event) => {
            if (event.target.classList.contains('details-link')) {
                isFarFromHome.value = false
                let x = event.target.id
                purchaseItem.value = productsArray[x]
                preview.value = true
                previewTitle.value = productsArray[x].title
                previewImage.value = productsArray[x].image
                previewDesc.value = productsArray[x].description
                previewPrice.value = productsArray[x].price
                setTimeout(() => {
                    modal.value.focus()
                }, 500);
            }
        }
        const closeModal = () => {
            preview.value = false
        }
        // POPULATE PRODUCT PAGE
        const loadProducts = () => {
            productsArray.forEach(x => {
                list.value.innerHTML += 
                `<li>
                    <p class="product-title">
                        ${x.title.slice(0, 40)}...
                    </p>
                    <div class="spacer">
                        <img src="${x.image}" class="product-image" alt="#"/>
                    </div>
                    <a class="details-link" id="${x.id - 1}">
                        Details...
                    </a>
                    <p class="product-price">
                        $ ${x.price.toFixed(2)}
                    </p>
                </li>`
            })
            console.log('list populated')
        }
        // NAV BUTTONS
        const homeButton = () => {
            title.value = 'Home'
            isProductPage.value = false
            isCart.value = false
            setTimeout(() => {
                isHomePage.value = true
            }, 200);
        }
        const productsButton = () => {
            title.value = 'Products'
            isHomePage.value = false
            isCart.value = false
            setTimeout(() => {
                isProductPage.value = true
            }, 200);
        }
        const cartButton = () => {
            title.value = 'Cart'
            isHomePage.value = false
            isProductPage.value = false
            setTimeout(() => {
                isCart.value = true
            }, 200);
        }
        // POPULATE CART PAGE
        const fillCart = () => {
            cartArray.forEach((x, index) => {
                cartList.value.innerHTML += 
                `<li>
                    <div class="cart-spacer">
                        <img class="delete-item" id="${index}" src="/src/assets/close-icon.png" alt="">
                        <div class="cart-image-spacer">
                            <img class="item-image" id="${index}" src="${x.image}" alt="">
                        </div>
                        <p>${x.title.slice(0, 16)}</p>
                    </div>
                </li>`
            })
        }
        // ADD ITEM TO CART
        const addToCart = () => {
            cartArray.push(purchaseItem.value)
            console.log(purchaseItem.value.price)
            cartTotal.value += purchaseItem.value.price
            console.log(cartTotal.value)
            cartList.value.innerHTML = ``    
            fillCart()
            purchase.value = true
            setTimeout(() => {
                purchase.value = false
            }, 1000);
        }
        // DELETE ITEM FROM CART
        const clickCartItem = (event) => {
            if (event.target.classList.contains('delete-item')) {
                cartTotal.value -= cartArray[event.target.id].price
                cartArray.splice(event.target.id, 1)
                cartList.value.innerHTML = ``    
                fillCart()
                console.log(cartTotal.value)
            }
            if (event.target.classList.contains('item-image')) {
                isFarFromHome.value = false
                let x = event.target.id
                preview.value = true
                previewTitle.value = productsArray[x].title
                previewImage.value = productsArray[x].image
                previewDesc.value = productsArray[x].description
                previewPrice.value = productsArray[x].price
                setTimeout(() => {
                    modal.value.focus()
                }, 500);
            }
        }
        // SCROLL TO TOP BUTTON
        let topButton = () => {
            if (preview.value) return
            if (document.body.scrollTop > 800 || document.documentElement.scrollTop > 800) {
                isFarFromHome.value = true
            } else {
                isFarFromHome.value = false
            }
        }
        window.onscroll = function() {topButton()}
        let scrollToTop = () => {
            document.body.scrollTop = 0
            document.documentElement.scrollTop = 0
        }
        // A FUNCTION FOR TESTING LOGIC
        const test = (event) => {
            console.log(imageInput.value)
            title.value = ''
            user.value = users[2]
            isSignUpForm.value = false
            document.body.scrollTop = 0
            document.documentElement.scrollTop = 0
            setTimeout(() => {
                isWelcome.value = true
            }, 200);
            setTimeout(() => {
                welcome.value.classList.add('opacity1')
                loadProducts()
            }, 300)
            setTimeout(() => welcome.value.classList.remove('opacity1'), 2700)
            setTimeout(() => {
                isWelcome.value = false
                title.value = 'Products'
                isLoggedIn.value = true
                isProductPage.value = true
            }, 3000)
        }
        return { 
            isLoggedIn,
            isCart,
            welcome,
            isProductPage,
            isHomePage,
            isWelcome,
            isFarFromHome,
            isCart,
            title,
            isLogInForm, 
            isSignUpForm, 
            showButton, 
            username,
            password, 
            users,
            user,
            imageInput,
            loadProducts,
            homeButton,
            productsButton,
            cartButton,
            showHide,
            signUpLogIn,
            loginAttempt,
            getProducts, 
            showDetails,
            addToCart,
            topButton,
            scrollToTop,
            test,
            clickCartItem,
            closeModal,
            productsArray,
            list,
            preview,
            previewTitle,
            previewImage,
            previewDesc,
            previewPrice,
            purchase,
            cartArray,
            purchaseItem,
            cartList,
            cartTotal,
            modal
        }
    }
}
</script>

<template>

<!-- NAVBAR -->
<nav class="navbar">
    <h1 class="logo" aria-label="Archer tech">Archertech</h1>
    <div v-if="isLoggedIn" class="buttons">
        <button @click="homeButton">Home</button>
        <button @click="productsButton">Products</button>
        <button @click="cartButton" class="cart-menu">Cart</button>
    </div>
    <h2>{{ title }}</h2>
</nav>

<!-- LOG IN FORM -->
<transition name="fade">
<form v-if="isLogInForm" ref="logInForm" class="login-form" action="#" method="#">

    <label for="username" class="user-label">Username</label>
    <input ref="username" type="text" class="user-input" id="username" placeholder="username" maxlength="16">

    <label for="password">Password</label>
    <input ref="password" type="password" class="password-input" id="password" placeholder="password" maxlength="16">

    <label ref="showButton" @click="showHide" class="show-hide" for="checkbox">SHOW</label>
    <input type="checkbox" class="checkbox" name="show-hide" id="checkbox">

    <button @click.prevent="loginAttempt" class="login-button">Log in</button>

    <div class="sign-up">
        <h3>Not a member?</h3>
        <a @click="signUpLogIn" href="#">Sign up</a>
    </div>
</form>
</transition>

<!-- SIGN UP FORM -->
<transition name="fade">
<form v-if="isSignUpForm" ref="signUpForm" class="signup-form" action="#" method="#">

    <label for="name">Name</label>
    <input type="text" id="name">

    <label for="email">Email</label>
    <input type="email" id="email">

    <label for="username">Username</label>
    <input type="text" id="username">

    <label for="password">Password</label>
    <input type="text" id="password">

    <label for="confirm">Confirm Password</label>
    <input type="text" id="confirm">

    <label for="image-input">Profile picture</label>
    <input ref="imageInput" id="image-input" class="image-input" type="file" accept="image/*">

    <button @click.prevent="test" class="signup-button">Create account</button>

    <div class="log-in" id="">
        <h3>Already a member?</h3>
        <a @click="signUpLogIn" href="#">Log in</a>
    </div>
</form>
</transition>

<!-- WELCOME MESSAGE -->
<header v-if="isWelcome" ref="welcome" class="welcome">Welcome {{user.name}}!</header>

<!-- HOME PAGE -->
<transition name="fade">
<div v-if="isLoggedIn" v-show="isHomePage" class="profile-card">
    <div class="row">
        <img class="profile-picture" :src="user.image" alt="">
        <div class="box">
            <p class="name">{{user.name}}</p>
            <p class="email">{{user.email}}</p>
        </div>
    </div>
    <p class="address-label">Shipping Address</p>
    <p class="address">{{user.shippingAddress}}</p>
</div>
</transition>

<!-- SHOPPING CART -->
<transition name="fade">
<div v-if="isLoggedIn" v-show="isCart" class="cart-card">
    <h1 v-show="cartArray.length > 0" class="cart-title">Cart</h1>
    <h1 v-show="cartArray.length === 0" class="empty">Empty</h1>
    <p v-show="cartArray.length > 0" class="cart-total">
        Total ${{cartTotal.toFixed(2)}}
        <button class="checkout">Checkout</button>
    </p>
    <ul @click="clickCartItem" ref="cartList" class="cart-list"></ul>
</div>
</transition>

<!-- PRODUCTS LIST -->
<transition name="fade">
<ul @click="showDetails" v-show="isProductPage" ref="list" class="product-list">
</ul>
</transition>

<!-- PRODUCT MODAL -->
<transition name="fade">
<div v-if="preview" class="overlay">
    <div @keydown.esc="closeModal" class="modal" ref="modal" tabindex="0">
        <img @click="preview = false" class="close" src="/src/assets/close-icon.png" alt=""/>
        <h1>{{previewTitle}}</h1>
        <div class="previewSpacer">
            <img class="preview-image" :src="previewImage" alt=""/>
        </div>
        <p>{{previewDesc}}</p>
        <div class="divider">
            <h2 :class="{ 'preview-price': purchase }">$ {{previewPrice.toFixed(2)}}</h2>
            <img v-show="isProductPage" @click="addToCart" class="cart-button" src="/src/assets/cart.png" alt=""/>
        </div>
    </div>
</div>
</transition>

<!-- TOP BUTTON  -->
<transition name="fade">
<button v-if="isFarFromHome" @click="scrollToTop" class="top-button">Top</button>
</transition>

</template>

<style>
.fade-enter-from,
.fade-leave-to {
    opacity: 0;
}
.fade-enter-active {
    transition: opacity 350ms ease-out;
}
.fade-leave-active {
    transition: opacity 150ms ease-in;
}

/* TO DO */

 /* 
    cart transitions 
    home page content 
*/

</style>