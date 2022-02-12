<template>
  <div>
    Burgers
    <Burger v-for="burger in burgers"
            v-bind:burger="burger" 
            v-bind:key="burger.name"/>
  </div>
  <div id="map" v-on:click="addOrder">
    click here
  </div>
  <div id="myID">
    <p v-if="booleanExpression">This text is conditionally visible</p>
  </div>

  <li v-for="burger in burgers" v-bind:key="burger.name">
    {{ burger.name }} 
    <span v-if="burger.kCal > 400">
      BIG MEAL
    </span>
  </li>
</template>

<script>
import Burger from '../components/Burger.vue'
import io from 'socket.io-client'

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

function MenuItem(name, kCal, url, gluten, lactose) {
  this.name = name;
  this.kCal = kCal;
  this.url = url;
  this.gluten = gluten;
  this.lactose = lactose;

}

const menuBurgers = [new MenuItem("California", 600, "p", true, false), new MenuItem("MCegg", 500, "p", true, false)];
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
      booleanExpression: true
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
  #map {
    width: 300px;
    height: 300px;
    background-color: red;
  }
</style>
