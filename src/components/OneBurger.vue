<template>
    <div>

<div class="divs">

            <h3>{{ burger.name }} </h3>


            <img class="productpic" v-bind:src="burger.url"   style="width: 100%;">

          <ul>
            
          <li>Calories:{{ burger.kCal }}</li>
          <li>Lactose:{{ burger.lactose }}</li>
                   <li>Gluten:{{ burger.gluten }} </li>
                 </ul>



<button  v-on:click="subOrdered()">
                    -
                  </button>
                 
                 {{ amountOrdered }}

                 <button  v-on:click="addOrdered()">
                    +
                  </button>
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

methods:{
addOrdered: function(){
  this.amountOrdered+=1;
this.$emit('orderedBurger', { name:   this.burger.name, 
                                amount: this.amountOrdered 
                              }
  );
},


subOrdered: function(){
  this.amountOrdered-=1;
  this.$emit('orderedBurger', { name:   this.burger.name, 
                                amount: this.amountOrdered 
                              }
  );
},



}

}



  </script>
  
  <!-- Add "scoped" attribute to limit CSS to this component only -->
  <style scoped>
  

  .divs{
   margin:  10px 20px 20px 20px;
   padding:  20px 20px 20px 20px;
   display: inline-block;
   border: 10px solid #fff56c;
   background-color: #26b9c8;

}
.productpic{
   width: 100%;
}
  </style>
  