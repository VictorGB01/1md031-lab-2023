<template>
  <div id="burger">
    <h2>{{ burger.name }}</h2>
    <img v-bind:src="burger.url" style="height: 150px;">
<ul>
  <li>{{ burger.kCal }}</li>
  <li>{{ burger.description }}</li>
  <li v-if="burger.gluten"> Has gluten.</li>
  <li v-if="burger.lactose"> Has lactose.</li>
  <li v-if="!burger.gluten"> Doesn't have gluten.</li>
  <li v-if="!burger.lactose"> Doesn't have lactose.</li>
</ul>
<div>
        <button v-on:click="decreaseAmount">-</button>
        <span> {{ "  " + amountOrdered + "  "}} </span>
        <button v-on:click="increaseAmount">+</button>
        </div>
    
  </div>
</template>

<script>
export default {
  name: 'OneBurger',
  props: {
    burger: Object
  },
  data: function () {
    return {
      amountOrdered: 0,

      }
  }, 
  methods: {
    increaseAmount() {
      this.amountOrdered++
      this.addBurger();
    }, 
    decreaseAmount(){
      if (this.amountOrdered > 0) {
        this.amountOrdered--;
        this.addBurger();
      }
    },
    addBurger:function () {
        this.$emit("orderedBurger", {
        name: this.burger.name,
        amount: this.amountOrdered
      },

  

      );
    },
    
  }
}
</script>







<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
#burger {
    background-color: #444;
    color: #fff;
    border-radius: 5px;
    padding: 20px;
    margin: 50px 50px 10px 20px;
    border: 5px dotted rgb(201, 81, 7);
    font-size: 100%;
}
</style>

















