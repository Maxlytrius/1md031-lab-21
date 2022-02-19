<template>
  
  
<!--   <section :class="Welcome">
    <header>
    Welcome to burger heaven
    </header>
    <img src="http://serendipitymommy.com/wp-content/uploads/2019/10/Cosy-Home.jpg" ;>
  </section>
     -->



  <nav> Menu items</nav>
  <main>
    <section id="Menu">
      <Burger v-for="burger in burgers"
            v-bind:burger="burger" 
            v-bind:key="burger.name"
            v-on:orderedBurger="addToOrder($event)"/>
    </section>
      <section id="Delivery">
        <h3>Delivery information:</h3>
      <form>
        <p>
          <label for="firstname">First</label><br>
          <input type="text" id="firstname" v-model="fn" required="required" placeholder="First name">

      </p>
      <div>{{this.fn}}</div>
      <p>
          <label for="lastname">Last</label><br>
          <input type="text" id="lastname" v-model="ln" placeholder="Last name">
      </p>
      <p>
        <label for="email">E-mail</label><br>
          <input type="email" id="email" v-model="em" required="required" placeholder="E-mail address">

          
      </p>
      <p>
        <label for="Street">Street</label><br>
        <input type="text" id="Street" v-model="st" required="required" placeholder="Street name">  
      </p>
      <p>
        <label for="House">House</label><br>
        <input type="number" id="House" v-model="hs" required="required" placeholder="House nr">  
      </p>
      <p>
        <label for="payment method">Payment method</label><br>
        <select id="payment method" name="rcp">
            <option>Head Office</option>
            <option>London Branch</option>
            <option>Philadelphia Branch</option>
            <option>Malmö Branch</option>
            <option>Melbourne Branch</option>
        </select>
      </p>
      <p>Customer gender:</p>

    <div>
      <input type="radio" id="male" name="drone" value="huey">
      <label for="male">male</label>
    </div>

    <div>
      <input type="radio" id="woman" name="drone" value="dewey">
      <label for="woman">female</label>
    </div>

    <div>
      <input type="radio" id="other" name="drone" value="louie" autocomplete="off"
      checked>
      <label for="other">other</label>
    </div><br>
      </form>
  </section>
    <div id="mapCont">
      <div id="map" v-on:click="setLocation($event)">
        <div id="mapTarget" v-bind:style="{ left: this.location.x + 'px', top: this.location.y + 'px'}">

        </div>
      </div>
    </div>
  <input type="submit" v-on:click="logOrder()" value="Send">
  </main>
  <hr>
    <footer>
      End notes
    </footer>
</template>

<script>
import Burger from '../components/Burger.vue'
import io from 'socket.io-client'
import menu from '../assets/menu.json'
//import func from 'vue-editor-bridge';

const socket = io();
const emp = { firstName: 'Maike', 
               lastName: 'Paetzel', 
                 branch: 'Uppsala', 
                    pos: 'PhD Student',
                   name: function () {
                           return this.firstName + ' ' + this.lastName;
                         }
            }

console.log(emp.firstName);

/* function MenuItem(name, kCal, img, gluten, lactose) {
  this.name = name;
  this.kCal = kCal;
  this.img = img;
  this.gluten = gluten;
  this.lactose = lactose;

} */

var menuBurgers = menu; //[new MenuItem("California", 600, "https://external-content.duckduckgo.com/iu/?u=https%3A%2F%2Ftse1.mm.bing.net%2Fth%3Fid%3DOIP.Y5oRYJycTasb9E_6R2Q38gHaHa%26pid%3DApi&f=1", true, false), new MenuItem("MCegg", 500, "https://external-content.duckduckgo.com/iu/?u=https%3A%2F%2Ftse1.mm.bing.net%2Fth%3Fid%3DOIP.WJn6QOszJgvB1yqXXYbxPgHaE1%26pid%3DApi&f=1", true, false)];
console.log(menuBurgers);


export default {
  
  name: 'Home',
  components: {
    Burger
  },
  
  data: 
    function () {
    return {
      burgers: menuBurgers,
      booleanExpression: true,
      fn: "",
      ln: "",
      em: "",
      st: "",
      hs: "",
      orderedBurgers: {},
      location: { x: 0,
                  y: 0
                }
    }
    
  },
  methods: {
    setLocation: function (event) {
      var offset = {x: event.currentTarget.getBoundingClientRect().left,
                    y: event.currentTarget.getBoundingClientRect().top};
      this.location = {x: event.clientX - 10 - offset.x,
      y: event.clientY - 10 - offset.y}
            console.log(event.pageX);
            console.log(event.pageY);
            console.log(this.location);
    },
    addToOrder: function (event) {
      this.orderedBurgers[event.name] = event.amount;
    },
    logOrder: function (){
      var orderInfo = [this.fn, this.ln, this.em, this.st, this.hs];
      for (const key in this.orderedBurgers) {
        if (Object.hasOwnProperty.call(this.orderedBurgers, key)) {
          const s = key + ":" + this.orderedBurgers[key];
          orderInfo += s;
          
        }
      }
      console.log(orderInfo);
      console.log(this.orderedBurgers["California"]);
      
    },
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
  .Welcome {}
  #Menu {}
  #Delivery {}
  .myDiv {
    color: blue;
    background-color:powderblue;
    }
.Ingredience_list{}

html{
  font-size: 20px;
}

body {
  font-family: arial;
  font-style: italic;
  /* font-size: 1.5vh; */
}

header{
  position: absolute;
  top: 2.5em;
  left: 3em;
  font-size: 3em;
}


.Welcome{  
  height: 25em;
  border: unset;

}

.Welcome img{
  opacity: 0.25;
  width: 100%;
  height: inherit;
}

nav{
  margin: 5px;
  padding: 5px;
}

section{
  margin: 5px;
  padding: 5px;
  border: 2px dotted black;
  
}

div{
  margin: 3px 25px 3px 4px;
}

img{
  width: 200px;
  height: 200px;
}

dt{
  text-transform: uppercase;
}

.Ingredience_list{
  color: #ff5500;
}

#Menu{
  background-color: black;
  color: white;
}


/* General input rule  */
input:hover {
  background-color: green;
}

input[type="Submit"], select{
  cursor:pointer;
}
input[type="Submit"]{
  width: 100px;
  height: 50px;
  margin-bottom: 20px;
  font-size: inherit;
}


button:hover {
  background-color: blue;
}

.wrapper {
     display: inline-grid;
     grid-gap: 50px;
     grid-template-columns: 200px 200px 200px;
     background-color: inherit;
     color: inherit;
 }

 .box {
     background-color: #444;
     color: #fff;
     border-radius: 5px;
     padding: 20px;
     font-size: 150%;
 }

 #mapCont{
  position: relative;
  margin: 0;
  padding: 0;
  overflow: scroll;
}

#map {
    object-fit: contain;
    width: 1920px;
    height: 1078px;
    background-color: red;
    background: url(/img/polacks.jpg);
  }

#map div{
  position: absolute;
  background: black;
  color: white;
  border-radius: 10px;
  width:20px;
  height:20px;
  text-align: center;
}

</style>
