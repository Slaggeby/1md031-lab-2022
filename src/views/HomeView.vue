<template>
 
 


  <div>

  <header>
    <img class="Headerbild" src="bobburger.jpg">
    <h1>BurgerBoi Online</h1>

  </header>

<main>
<audio controls autoplay hidden>

      <source src="spongbub.mp3" type="audio/mpeg">
      </audio>

      <div>
      Burgers
      <section class="wrapper">
      <Burger v-for="burger in burgers"
              v-bind:burger="burger" 
              v-bind:key="burger.name"
              v-bind:src="burger.url"
              v-on:orderedBurger="addToOrder($event)"
              />
            </section>
    </div>
       <!--
<section class="wrapper">

        <h2>Beställ BurgerBoi burgers!</h2>
        <p> Här betäller du -_- för bästa upplevelse tillåt audio för hemsidan i din browser<p>
         <div class="divs" > 
            <h3>The Standard</h3>


            <img class="productpic" src="https://media.istockphoto.com/id/927593762/sv/foto/en-verklig-liv-hemmagjord-hamburgare-hamburgare-verkligen-ser-ut.jpg?s=612x612&w=is&k=20&c=QiKctl6N-ZMzs5V7lBa409hX80Ql0zYzlp286VHZnWc=" alt="The Standard" title="The Standard Burger" style="width: 100%;">

            <section class="Allergier">
              <p><span id="Allergierid">


                <ul>



                  <li>50% Gluten</li>
                  <li>50% Laktos</li>

                </ul>
              </span></p>
            </section>

          </div>
          <div class="divs">

            <h3>The Living</h3>


            <img class="productpic" src="https://media.istockphoto.com/id/1277712627/sv/foto/sk%C3%B6ldpadda.jpg?s=1024x1024&w=is&k=20&c=Dg8XaORmKC883NZ2BwRLgYxNeXlmSM_vk2E2WXVU7aY=" alt="The Living" title="The Living Burger" style="width: 100%;">

            <ul>
              <li>Har hårt skall</li>
              <li>Behöver mat och vatten</li><li>Kan innehålla spår av djurprodukter</li>
            </ul>
          </div>
          <div class="divs">
            <h3>The Sweet</h3>


            <img class="productpic" src="https://www.isof.se/images/18.282eed401758d09d82c1b86/1604476928798/semlor.jpg" alt="The Sweet" title="The sweet Burger" style="width: 100%;">

            <ul>
              <li>Glutenfri</li>
              <li>Laktosfri</li>
              <li>Nötfri</li>
              <li>Kalorifri</li>
            </ul>
          </div>

        </section>
-->
<section class="BB">
          <span id="BBid">

            <h2>Bli en BurgerBuddy!</h2>
            <p> Bli medlem för att få gratis burgare ibland kanske. <p>
              <h3>Ge mig din info:</h3>
              <p><label for="namn">Namn</label><br>
                <input type="text" id="namn" v-model="formdata.n" required="required" placeholder="namn"></p>

                <p>
                  <label for="Email">Email</label><br>
                  <input type="email" id="email" v-model="formdata.em" required="required" placeholder="E-mail address">
                </p>
                <p>
                 
                  <p>
                  
                    <p>
                      <label for="Betalsätt">Betalsätt</label><br />
                        <select id="betalsätt" v-model="formdata.bs">
                          <option selected="selected">Kort</option>
                          <option>Swish</option>
                          <option>Byteshandel</option>
                          <option>Check</option>
                          <option>Gå in i skuld</option>
                        </select>

                      </p>
                      <fieldset>
                        <legend>Vad e ditt kön?</legend>

                        <div>
                          <input type="radio" id="grabb" v-model="formdata.drone" value="grabb"
                          checked>
                          <label for="grabb">grabb</label>
                        </div>

                        <div>
                          <input type="radio" id="tjej" v-model="formdata.drone" value="tjej">
                          <label for="tjej">tjej</label>
                        </div>

                        <div>
                          <input type="radio" id="Icke-binär" v-model="formdata.drone" value="ick-binär">
                          <label for="Icke-binär">Icke-binär</label>
                        </div>
                        <div>
                          <input type="radio" id="Annat" v-model="formdata.drone" value="Annat">
                          <label for="Annat">Annat</label>
                        </div>
                        <div>
                          <input type="radio" id="Vill inte svara" v-model="formdata.drone" value="Vill inte svara">
                          <label for="Vill inte svara">Vill inte svara</label>
                        </div>
                      </fieldset>
                    </span>
                  </section>
<button type="submit" v-on:click="placeOrder()">

                    <img src="https://upload.wikimedia.org/wikipedia/commons/5/51/Green-checkmark.svg" alt="Green Check Mark Transparent" style="width: 20px">

                    Send Info
                  </button>
{{orderedBurgers}}

                </main>
                <footer>

                  <p>&copy;2022 BurgerBoi ltd. </p>
                </footer>
        


</main>

    <div class=mapWrapper>
    <div id="map" v-on:click="setLocation($event)">

     <div id="dots" v-bind:style="{ position: 'relative', background: 'url(' + require('../../public/img/polacks.jpg')+ ')' }">
          <div v-bind:location="location" v-bind:style="{ left: location.x + 'px', top: location.y + 'px', position: 'absolute'}" v-bind:key="'dots'">
            T 
          </div>
      </div>



    </div>
    </div>
  </div>



</template>

<script>
import menu from '../assets/menu.json'
import Burger from '../components/OneBurger.vue'
import io from 'socket.io-client'


const socket = io();


function MenuItem(name, url, cal, lactose, gluten) {
  this.name = name; 
  this.url = url;
  this.kCal = cal;
  this.lactose = lactose;
  this.gluten = gluten;
}

/*var theStandard=new MenuItem(menu[0].name, menu[0].img ,menu[0].kCal, menu[0].lactose, menu[0].gluten);

var theLiving= new MenuItem(menu[1].name, menu[1].img ,menu[1].kCal, menu[1].lactose, menu[1].gluten);

var theSweet= new MenuItem(menu[2].name, menu[2].img ,menu[2].kCal, menu[2].lactose, menu[2].gluten);

var theStandard=new MenuItem(menu[0].name, menu[0].img ,menu[0].kCal, menu[0].lactose, menu[0].gluten);

const theLiving= new MenuItem("The Living","https://media.istockphoto.com/id/1277712627/sv/foto/sk%C3%B6ldpadda.jpg?s=1024x1024&w=is&k=20&c=Dg8XaORmKC883NZ2BwRLgYxNeXlmSM_vk2E2WXVU7aY=",102,true, false);
const theSweet= new MenuItem("The Sweet","https://www.isof.se/images/18.282eed401758d09d82c1b86/1604476928798/semlor.jpg",0,false, false);

let burgerArray=[theStandard, theLiving, theSweet];*/


export default {
  name: 'HomeView',
  components: {
    Burger
  },
  data: function () {
    return {
formdata:{n:"",em:"",ad:"",gn:"",bs:"",drone:""},
orderedBurgers:{},
location: { x: 0,
            y: 0
          },

      burgers: menu,
    }
  },
  methods: {
    placeOrder: function(){
console.log("Placing Order:", this.formdata)
socket.emit("addOrder", { orderId: this.getOrderNumber(),
                                details: { x: this.location.clientX,
                                           y: this.location.clientY },
                                orderItems: [this.orderedBurgers],
                                customer:{
                                  name: this.formdata.n,
                                  mail: this.formdata.em,
                                  gender: this.formdata.gn,
                                  payment: this.formdata.bs,
                                }
                              

                              }
                 );

    },
    setLocation: function(event){
      var offset = {x: event.currentTarget.getBoundingClientRect().left,
                    y: event.currentTarget.getBoundingClientRect().top};
      this.location.x=event.clientX-10-offset.x;
      this.location.y=event.clientY-10-offset.y;

    },
addToOrder: function (event) {
  this.orderedBurgers[event.name] = event.amount;
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

@font-face {
  font-family: Spongeboy;
  src: url("../../public/SpongeboyMeBob.ttf");
}

body {
  font-family: "Spongeboy";
  font-size: 12pt;
  color: #fff56c ;
  background-image: url("../../public/wp2636472-background-spongebob.jpg");
  background-attachment: fixed;
}



.Allergier {
   color: #fff56c;
}

#Allergierid {
   font-weight: bold;

}

header{
   width: 95%;
   height: 10%;
   margin:  20px 20px 20px 20px;
   padding:  20px 20px 20px 20px;
   display: inline-block;
   position: absolute 20px;



}


header > h1{
   position:  absolute;
   margin-top: -120px;
   padding: 50px 50px;
   color: #fff56c;


}

.wrapper{
   display: grid;
   grid-gap: 10px;
   grid-template-columns: 33% 33% 33%;
}




.Headerbild{


   width: 100%;
   height: 100px;
   object-fit: fill;
   opacity: 0.8;
}


.divs{
   margin:  10px 20px 20px 20px;
   padding:  20px 20px 20px 20px;
   display: inline-block;
   border: 10px solid #fff56c;
   background-color: #26b9c8;

}


.BB {
   background-color:#0457a0
   ;
   color: #aead0d;
   border: 10px solid #aead0d;

}
#BBid{

   color: #aead0d;
}

button:hover {
   background-color:#0457a0 ;
   color: blueviolet;
   cursor:pointer;

}
button{
   margin: 30px;
   padding: 20px;
}

section{
   margin: 30px;

}
head{

   margin: 300;
}


  #map {
    width: 1920px;
    height: 1078px;
    background: url("../../public/img/polacks.jpg");
    
  }
  .mapWrapper{
overflow: scroll;
    width: 1500px;
    height: 800px;
  }
</style>