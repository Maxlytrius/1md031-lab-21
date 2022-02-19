<template>
      <div class="wrapper">
        <div>
          <h2>
            {{ burger.name }}
          </h2>
          <img :src="burger.img">
          {{ burger.kCal }} kCal
          <div>
            <h3>Ingredients</h3>
            <ul class="Ingredience_list">
              
              <li>Pickles</li>
            </ul>
          </div>
          <input type="button" v-on:click="alterBurgerAmount(-1)">
          <input type="button" v-on:click="alterBurgerAmount(1)">
          {{ amountOrdered }}
        </div> 
        </div>
<!--   <div>
    {{ burger.name }} {{ burger.kCal }}
  </div> -->
</template>

<script>

export default {
  name: 'Burger',
  props: {
    burger: Object
  },
  data: function () {
  return {
    amountOrdered: 0,
  }
},
  methods: {
    alterBurgerAmount: function (value) {
      this.amountOrdered += value;
      this.$emit('orderedBurger', { name:   this.burger.name, 
                                  amount: this.amountOrdered 
                                }
  );
}
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.wrapper {
     display: inline-grid;
     grid-gap: 50px;
     grid-template-columns: 200px 200px 200px;
     background-color: inherit;
     color: inherit;
 }
</style>
