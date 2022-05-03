<script>
export default {
    name: 'Logic',
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
        const showPassword = ref(false)
        const previewObject = ref()
        const user = ref()
        const productsArray = ref([])
        const isLogInForm = ref(true)
        const isSignUpForm = ref(false)
        const isLoggedIn = ref(false)
        const isCart = ref(false)
        const isWelcome = ref(false)
        const isFarFromHome = ref(false)
        const welcome = ref()
        const isProductPage = ref(false)
        const isHomePage = ref(false)
        const username = ref('')
        const password = ref('')
        const list = ref()
        const preview = ref(false)
        let recentArray = []
        const title = ref('log in')
        const purchase = ref(false)
        let cartArray = []
        const cartTotal = ref(0)
        const modal = ref()
        const imageInput = ref()
        
        fetch('https://fakestoreapi.com/products')
        .then(res => res.json())
        .then(data => {
            productsArray.value = data
            console.log('data recieved')
        })
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
                if (username.value === users[i].name && password.value === users[i].password) {
                    title.value = ''
                    user.value = users[i]
                    isLogInForm.value = false
                    setTimeout(() => {
                        isWelcome.value = true
                    }, 400);
                    setTimeout(() => isWelcome.value = false, 2700)
                    setTimeout(() => {
                        title.value = 'Products'
                        isLoggedIn.value = true
                        isProductPage.value = true
                    }, 3000)
                }
            }
        }
        // CREATE ACCOUNT
        const createAccount = () => {
            title.value = ''
            user.value = users[2]
            isSignUpForm.value = false
            scrollToTop()
            setTimeout(() => {
                isWelcome.value = true
            }, 300);
            setTimeout(() => {
                isWelcome.value = false
                title.value = 'Products'
                isLoggedIn.value = true
                isProductPage.value = true
            }, 3000)
        }
        // SHOW DETAILS
        const showDetails = (event) => {
            if (event.target.classList.contains('details-link')) {
                isFarFromHome.value = false
                previewObject.value = productsArray.value[+event.target.id.substring(8)]
                recentArray.push(previewObject.value)
                if (recentArray.length > 7) recentArray.splice(0, 1)
                preview.value = true
                setTimeout(() => {
                    modal.value.focus()
                }, 1)
            }
            if (event.target.classList.contains('recent-image')) {
                productsArray.value.find(x => {
                    if (x.id === +event.target.id) {
                        previewObject.value = x
                        preview.value = true
                        setTimeout(() => {
                        modal.value.focus()
                        }, 1)
                    }
                })
            }
        }
        // NAV BUTTONS
        const homeButton = () => {
            title.value = 'Home'
            isProductPage.value = false
            isCart.value = false
            setTimeout(() => {
                isHomePage.value = true
            }, 300);
            console.log()
        }
        const productsButton = () => {
            title.value = 'Products'
            isHomePage.value = false
            isCart.value = false
            setTimeout(() => {
                isProductPage.value = true
            }, 300);
        }
        const cartButton = () => {
            title.value = 'Cart'
            isHomePage.value = false
            isProductPage.value = false
            setTimeout(() => {
                isCart.value = true
            }, 300);
        }
        // ADD ITEM TO CART
        const addToCart = () => {
            cartArray.push(previewObject.value)
            cartTotal.value += +previewObject.value.price.toFixed(2)
            purchase.value = true
            setTimeout(() => {
                purchase.value = false
            }, 1000);
        }
        // CLICK CART ITEM
        const clickCartItem = (event) => {
            if (event.target.classList.contains('delete-item')) {
                cartTotal.value -= cartArray[+event.target.id.substring(5)].price
                cartArray.splice(+event.target.id.substring(5), 1)
            }
            if (event.target.classList.contains('item-image')) {
                isFarFromHome.value = false
                previewObject.value = cartArray[+event.target.id.substring(5)]
                preview.value = true
                setTimeout(() => {
                    modal.value.focus()
                }, 1)
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
        const scrollToTop = () => {
            document.body.scrollTop = 0
            document.documentElement.scrollTop = 0
        }
        // A FUNCTION FOR TESTING LOGIC
        const test = (event) => {
           
        }
        return { 
            showPassword,
            previewObject,
            productsArray,
            isLoggedIn,
            isCart,
            welcome,
            isProductPage,
            isHomePage,
            isWelcome,
            isFarFromHome,
            title,
            isLogInForm, 
            isSignUpForm, 
            username,
            password,
            users,
            user,
            imageInput,
            createAccount,
            homeButton,
            productsButton,
            cartButton,
            signUpLogIn,
            loginAttempt,
            showDetails,
            addToCart,
            topButton,
            scrollToTop,
            test,
            clickCartItem,
            productsArray,
            list,
            preview,
            recentArray,
            purchase,
            cartArray,
            cartTotal,
            modal
        }
    }
}
</script>

<template></template>
<style></style>