<template>
    <div id="orders">
      <div id="orderList">
        
        <div v-for="(order, key) in orders" v-bind:key="'order'+key">
          <hr>
          {{"Order Number: "+ key }}
          <ul>
          <li v-for="(item, itemKey) in order.orderItems" :key="itemKey">
            {{ itemKey }}: {{ item }}
          </li>
          </ul>

          <ul>
          <li v-for="(item,itemKey) in order.customerDetails" :key="itemKey">
            {{ itemKey }}: {{ item }}
          </li>
          </ul>
          <hr>
        </div>



        <button v-on:click="clearQueue">Clear Queue</button>
      </div>
      <div id="dots" v-bind:style="{ background: 'url(' + require('../../public/img/polacks.jpg')+ ')' }">
          <div v-for="(order, key) in orders" v-bind:style="{ left: order.details.x + 'px', top: order.details.y + 'px'}" v-bind:key="'dots' + key">
            {{ key }}
          </div>
      </div>
    </div>
  </template>
  <script>
  import io from 'socket.io-client'
  const socket = io();
  
  export default {
    name: 'DispatcherView',
    data: function () {
      return {
        orders: null
      }
    },
    created: function () {
      socket.on('currentQueue', data =>
        this.orders = data.orders);
    },
    methods: {
      clearQueue: function () {
        socket.emit('clearQueue');
      }
    }
  }
  </script>
  <style>
  #orderList {
    top:1em;
    left:1em;
    position: absolute;
    z-index: 2;
    color:black;
    background: rgba(23, 6, 6, 0.5);
    padding: 1em;

    font-family: "Lobster ";
    font-size: 14px;
    font-weight: bold;




  }
  #dots {
    position: relative;
    margin: 0;
    padding: 0;
    background-repeat: no-repeat;
    width:1920px;
    height: 1078px;
    cursor: crosshair;
  }
  
  #dots div {
    position: absolute;
    background: black;
    color: rgb(15, 173, 91);
    border-radius: 10px;
    width:20px;
    height:20px;
    text-align: center;
  }
  </style>
  