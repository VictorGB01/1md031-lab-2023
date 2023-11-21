<template>
  
  <div>
    <header class="heading-with-image">
        <h1>Burger Joint</h1>
        <img src="/img/JapanSkyline.jpg" id= "skyline" alt="Bugger" width= 2000 height=400>
    </header>


    <section id="Section1">
      <h2>Oh so many burgers!</h2>
      <p>This is where you select your burger</p>
    
    <div class="wrapper" >
      <Burger v-for="burger in menu"
              v-bind:burger="burger" 
              v-bind:key="burger.name"
              v-on:orderedBurger="addToOrder($event)" />
    </div>
    </section>

    <section id="section2" >
    <h3>Customer information</h3>
    Here you provide all information before you purchase your burger
    <br />
    <form> <p>
        <label for="name">First- and Last Name</label><br>
        <input type="text" id="firstname" v-model="fn" required="required" placeholder="First- and Last name">
    </p>
    <p>
        <label for="email">Email</label><br>
        <input type="email" id="email" v-model="email" required="required" placeholder="E-mail address">
    </p>

<!--
    <p>
        <label for="street">Street</label><br>
        <input type="text" id="street" name="str" required="required" placeholder="Street Name">
    </p>
    <p>
        <label for="House">House</label><br>
        <input type="house" id="houuse" name="hse" required="required" placeholder="House Number">
    </p>
    -->

    <p>
        <label for="payment">Payment Method</label>
        <select id="recipient" v-model="pm">
            <option>American Express</option>
            <option>Mastercard</option>
            <option>Cash</option>
            <option>Bitcoin</option>
            <option>Doggo</option>
        </select>
     </p>
<br />
        <label>
          <input type="radio" v-model="gender" value="male" checked="checked">
          Male
        </label>
      <br />
        <label>
          <input type="radio" v-model="gender" value="female">
          Female
        </label>
        <br />
        <label>
          <input type="radio" v-model="gender" value="not_specified">
          Do not wish to answer
        </label>
     </form>


     <button type="submit" v-on:click="submitForm">
        <img src="img/checkmark.png" width="30" height="20">
        Submit
      </button>
    </section>
    <br />
    <hr>
    <footer>
    Customer service contact information &copy; Victor AB
    </footer>



    <div class="map-container">
    <div id="map" v-on:click="setLocation">
       click on the map to choose adress
    <div id="dots" >
     <!-- <img src="/img/polacks.jpg" width= 1920 height=1078> -->
    <div v-bind:style="{ left: location.x + 'px', top: location.y + 'px' }">
      T
    </div>
    </div>
    </div>
    </div>

<!--
    <div id="map-container">

    <div id="map" v-on:click="addOrder">
       click here to choose delivery adress
    <div id="dots" >
    <div v-bind:style="{ left: location.x + 'px', top: location.y + 'px' }">
      T
    </div>
    </div>
    </div>
    </div>
-->








  </div>
</template>

<script>
import Burger from '../components/OneBurger.vue'
import io from 'socket.io-client'
import menu from '../assets/menu.json'
const socket = io();


/*
const burgerArray = [
    new MenuItem("SuperMegaHuge Burger", "4000 kCal", "/img/SuperMegaHuge Burger.jpg", "BOOOOM BIG BURGER!", true, true),
    new MenuItem("Mini Burger", "40 kCal", "/img/MiniBurger.jpg", "Almost Free!", true, true),
    new MenuItem("LeafBread Burger", "-100 kCal", "/img/LeafbreadBurger.jpg", "No Returns!", true, false)
];
*/
function MenuItem(name, kCal, url, description, lactose, gluten) {
    this.name = name; // The *this* keyword refers to the object itself
    this.url = url;
    this.kCal = kCal;
    this.lactose = lactose;
    this.gluten = gluten;
    this.description=description;
}

export default {
  name: 'HomeView',
  components: {
    Burger
  },
  data: function () {
    return {
      menu: [],
      fn: '', // Individual data properties for form values
      email: '',
      //street: '',
      //sn: '',
      pm: '',
      gender: '',
      orderedBurgers:{},
      location: { x: 0,y: 0}
    }
  },
  mounted() {
    this.menu = menu; // Assign the fetched menu data to the component's menu property
  },

  methods: {
    getOrderNumber: function () {
      return Math.floor(Math.random()*100000);
    },

    addOrder: function (event) {
      var offset = {x: event.currentTarget.getBoundingClientRect().left,
                    y: event.currentTarget.getBoundingClientRect().top};
      /* 
      socket.emit("addOrder", { orderId: this.getOrderNumber(),
                                details: { x: event.clientX - 10 - offset.x,
                                           y: event.clientY - 10 - offset.y },
                                orderItems: ["Beans", "Curry"]
                              }
                 );
      */

      this.location.x = event.clientX -5 - offset.x;
      this.location.y = event.clientY - 5 - offset.y;

    },
    addToOrder: function (event) {
      this.orderedBurgers[event.name] = event.amount;
    },
    setLocation: function (event) {
      this.location.x = event.clientX - 10 - event.currentTarget.getBoundingClientRect().left;
      this.location.y = event.clientY - 35 - event.currentTarget.getBoundingClientRect().top;
      console.log("reached in func")
    },
    submitForm: function (){
      socket.emit("addOrder", { orderId: this.getOrderNumber(),
                                details: {   x: this.location.x,
                                            y: this.location.y
                                          },
                                orderItems: this.orderedBurgers,
                                customerDetails:{Name:this.fn, "e-mail":this.email, "Payment Method":this.pm, Gender:this.gender}
                              }
                 );



      console.log('Submitted order: ');
      console.log('Name: ' + this.fn);
      console.log('Email: ' + this.email);
      //console.log('Address:' + this.street + this.sn);
      console.log('Ordered Burgers:'+ JSON.stringify(this.orderedBurgers));
      console.log('Payment Option:'+ this.pm);
      console.log('Gender:'+ this.gender)
    }
  }
}
/*
  data: function () {
    return {
      menu: [],
      fn: '', // Individual data properties for form values
      email: '',
      street: '',
      sn: '',
      payment: '',
      gender: ''
    }
  },
  mounted() {
    this.menu = menu; // Assign the fetched menu data to the component's menu property
  },*/ 

</script>

<style>
  @import 'https://fonts.googleapis.com/css?family=Pacifico|Dosis';

body {
    font-family: "Arial";
    font-size: 1.5em;
}

.heading-with-image {
    position: relative; /* Set the positioning context to relative */
  }
.heading-with-image img {
  opacity:0.9;
}
.heading-with-image h1 {
    position: absolute; /* Position the heading absolutely within its container */
    top: 50%; /* Adjust the top position as needed */
    left: 50%; /* Adjust the left position as needed */
    transform: translate(-50%, -50%); /* Center the heading both horizontally and vertically */
    z-index: 1; /* Set a higher z-index to make sure the text appears on top of the image */
    text-align: center;
    color: rgb(0, 0, 0);

}

#Section1 {
    background-color: black;
    padding: 10px 23px 40px;
    color: rgb(255, 255, 255);
    border: 10px solid rgb(201, 81, 7);
}

#section2 {
    background-color: rgb(173, 128, 20);
    padding: 10px 23px 40px;
    border: 10px solid rgb(201, 81, 7);
}

.box{
    margin: 50px 50px 10px 20px;
    border: 5px dotted rgb(201, 81, 7);
    padding: 10px 10px 10px;
}

.wrapper {
    display: grid;
    grid-gap: 10px;
    grid-template-columns: repeat(3, 1fr); 
    background-color: #000000;
    color: #ffffff;
}
@media (max-width: 900px) {
  .wrapper {
    grid-template-columns: repeat(2, 1fr);
  }
}

/* Further adjustment for even smaller screens if needed */
@media (max-width: 600px) {
  .wrapper {
    grid-template-columns: 1fr;
  }
}

.box {
    background-color: #444;
    color: #fff;
    border-radius: 5px;
    padding: 20px;
    font-size: 150%;
}



button{
    margin: 10px 10px 10px 10px;
}

button:hover {
    color: blue;
    background-color: black;
    cursor: pointer;
  }

  #map {
width: 1920px;
height: 1078px;
    background:url("../../public/img/polacks.jpg");
 
  }
  .map-container{
    width: 100%;
    height: 500px;
    overflow: scroll;
  }

</style>




























