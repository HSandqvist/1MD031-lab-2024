<template>
<body>
       <header>
           <div class="header-content">
               <h1>Welcome to Hampus's Hamburgers</h1>
               <img src="/public/img/background.png" alt="Header Image">
           </div>
       </header>
       
       <main>
           <section id="burger-selection">
              <h2>Select Your Burger</h2>
              <p>Choose from our wide variety of delicious burgers to satisfy your cravings.</p>
        
              <div v-for="burger in burgers" :key="burger.name">
                 <Burger :burger="burger" />
              </div>
          </section>
           
           <section id="customer-information">
               <h2>Customer Information</h2>
               <p>Provide your delivery address and contact details to ensure a smooth delivery process.</p>
               <h3>Delivery Information</h3>
               
               <form>
                   <label for="name">First and Last Name:</label><br>
                   <input type="text" id="name" name="name" placeholder="John Doe"><br><br>
                   
                   <label for="email">E-mail Address:</label><br>
                   <input type="email" id="email" name="email" placeholder="john.doe@example.com"><br><br>
                   
                   <label for="street">Street:</label><br>
                   <input type="text" id="street" name="street" placeholder="123 Main St"><br><br>
                   
                   <label for="house-number">House Number:</label><br>
                   <input type="text" id="house-number" name="house-number" placeholder="123" pattern="\d+" title="Please enter a valid house number"><br><br>
                   
                   <label for="payment-method">Payment Method:</label><br>
                   <select id="payment-method" name="payment-method">
                       <option value="credit-card" selected>Credit Card</option>
                       <option value="paypal">PayPal</option>
                       <option value="bank-transfer">Bank Transfer</option>
                       <option value="cash-on-delivery">Cash on Delivery</option>
                   </select><br><br>
                   
                   <label>Gender:</label><br>
                   <input type="radio" id="male" name="gender" value="male" checked>
                   <label for="male">Male</label><br>
                   <input type="radio" id="female" name="gender" value="female">
                   <label for="female">Female</label><br>
                   <input type="radio" id="no-gender" name="gender" value="no-gender">
                   <label for="no-gender">Do not wish to provide</label><br><br>
               </form>
           </section>
           
           <button type="submit">
               <img src="/public/img/order.png" alt="Order" style="width: 20px; height: 20px;">
               Submit Order
           </button>
       </main>
       <hr>
       <footer>
           <p>&copy; 2024 Hampus's Hamburgers. All rights reserved.</p>
       </footer>
   </body>
</template>

<script>
import Burger from '../components/OneBurger.vue'
import io from 'socket.io-client'
import menu from '../assets/menu.json'

const socket = io("localhost:3000");

function MenuItem(name, imageUrl, kCal, containsGluten, containsLactose) {
  this.name = name;
  this.imageUrl = imageUrl;
  this.kCal = kCal;
  this.containsGluten = containsGluten;
  this.containsLactose = containsLactose;
}

// Array of burgers
const burgers = [
  new MenuItem("Classic Burger", "public/img/classicburger.png", 250, true, true),
  new MenuItem("Vegan Burger", "public/img/veganburger.png", 450, false, false),
  new MenuItem("Halloumi Burger", "public/img/halloumiburger.png", 850, true, true)
];

// TODO: fix the path to the images

export default {
  name: 'HomeView',
  components: {
    Burger
  },
  data: function () {
    return {
      burgers: menu
    }
  },
  methods: {
    getOrderNumber: function () {
      return Math.floor(Math.random()*100000);
    },
    addOrder: function (event) {
      var offset = {x: event.currentTarget.getBoundingClientRect().left,
                    y: event.currentTarget.getBoundingClientRect().top};
      socket.emit("addOrder", { orderId: this.getOrderNumber(),
                                details: { x: event.clientX - 10 - offset.x,
                                           y: event.clientY - 10 - offset.y },
                                orderItems: ["Beans", "Curry"]
                              }
                 );
    }
  }
}
</script>

<style>
  @import url('https://fonts.googleapis.com/css2?family=DM+Sans:ital,opsz,wght@0,9..40,100..1000;1,9..40,100..1000&display=swap');
body {
    font-family: 'DM ans', sans-serif;
    font-size: 12pt;
}

p {
    color: red;
}

h1 {
    font-family: 'Agbalumo';
    font-size: 36pt;
}

main, header, footer, nav ul {
    max-width: 40rem;
    margin: 0 auto 0 auto;
}

main {
    background-color: white;
}

/* nav ul li {
    display: inline-block;
    background-color: grey;
    padding: 1em;
    margin: 1em;
} */

header {
    overflow: hidden;
    position: relative;
}

.header-content {
    text-align: center;
    margin: 20px;
}

.header-content img {
    width: 100%;
    height: auto;
    opacity: 0.5; 
}

.header-content h1 {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    width: 100%;
    text-align: center;
    color: white; /* Ensure the text is readable on the image */
    z-index: 1;
    text-shadow: 
        -1px -1px 0 #000, 
        1px -1px 0 #000, 
        -1px 1px 0 #000, 
        1px 1px 0 #000;
}

header h1 {
    width:40rem;
    margin: 0 auto;
    text-align: center;
}

nav ul {
    display: grid;
    grid-template-columns: repeat(auto-fill, 9.25em);
    gap: 1em;
}

section {
    margin: 20px;
    padding: 20px;
    border: 2px dashed;
}

#burger-selection {
    background-color: black;
    color: white;
    border-color: white;
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(300px, 1fr)); /* Increased the minimum size */
    gap: 20px;
}

#customer-information {
    border-color: black;
}

.ingredient {
    font-weight: bold;
}

button {
    cursor: pointer;
    margin-top: 20px;
}

button:hover {
    background-color: lightgray;
}

#burger-selection div {
    padding: 10px;
    margin: 10px;
}
#burger-selection img {
    width: 100%;
    height: 150px; /* Set a fixed height for the images */
    object-fit: cover; /* Ensure the images maintain their aspect ratio */
}
</style>