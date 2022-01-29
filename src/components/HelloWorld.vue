<template>
<div class="bg-fundo" :style="{backgroundImage: 'url(' + image + ')'}">
  <div class="menu-bg">
    <div class="menu">
      <div class="menu-logo">
        <h1>
          <a href="#">MemoryVue</a>
        </h1>
      </div>
      <nav>
        <ul>
          <li>
            <h4>
              <a class="btn-menu" href="#" @click="reiniciar()">Reiniciar</a>
            </h4>
          </li>
        </ul>
      </nav>
    </div>
    <!-- End Menu -->
  </div>

    <div class="menu-area">
      <ul>
        <li class="card" :class="[item.disable,item.fontSize,item.color, item.effect]" v-for="(item, i) in array" :key="i" @click="click(item,i)">
          <!-- {{item.type}} -->
          <i :class='"fa "+item.icon'></i>
        </li>
      </ul>
    </div>

    <div class="info-bg">
      <div class="info">
        <h2 style="padding: 0 5px"><i class="fa fa-chess-knight"></i>  </h2>
        <h2>{{moves}} <span class="yellow">Jogadas</span></h2>
      </div>
      <div class="info">
        <h2 style="padding: 0 5px"><i class="fa fa-clock"></i>  </h2>
        <h2 v-if="this.minute > 0">{{minute}}<span class="yellow">min</span></h2>
        <h2>{{second}}<span class="yellow">seg</span></h2>
      </div>

    </div>


    <div id="popup1" :class="'overlay '+show">
      <div class="popup">
        <h1>🎉 Congratulations 🎉</h1>
        <a class="close" @click="reiniciar()">×</a>
        <br>
        <div class="content-1">
          <p>Você fez: {{this.moves}} jogadas </p>
          <span class="black">Tempo: </span>
          <span class="black" v-if="this.hour">em: {{hour}} Horas </span>
          <span class="black" v-if="this.minute">{{minute}} Minuto(s) e </span>
          <span class="black" v-if="this.second">{{second}} Segundo(s) </span>
          <br><br>
        </div>
        <br>
          <a class="btn-menu" href="#" @click="reiniciar()">Jogar Novamente 😄</a>
        <br>
      </div>
    </div>







  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  data() {
    return {
      array: [],
      choose: [],
      i: [],
      moves: 0,
      minute: 0,
      second: 0,
      hour: 0,
      interval: [],
      show: '',
      moveHit: 0,
      image: require('@/assets/simple-hand.png')
    }
  },
  methods: {
    click(value,i) {

      if (this.choose.length > 1) {
        console.log("Wait");
      } else {

        this.choose.push(value);
        this.i.push(i);

        this.array[i].disable = 'disable';
        this.array[i].fontSize = 'fs36';
        this.array[i].effect = 'open';

        if (this.choose.length > 1) {
  
          if (this.choose[0].type === this.choose[1].type) {
            this.movesAdd();
            this.movesHit();
            // PARES IGUAIS

            this.choose[0].disable = 'disable';
            this.choose[1].disable = 'disable';
            this.choose[0].color = 'match';
            this.choose[1].color = 'match';

            this.choose.splice(0, this.choose.length); 
            this.i.splice(0, this.i.length); 

          } else {
            this.movesAdd();
            this.choose[0].color = 'unmatched';
            this.choose[1].color = 'unmatched';

            setTimeout(() => {
              this.choose[0].disable = '';
              this.choose[1].disable = '';
              this.choose[0].fontSize = '';
              this.choose[1].fontSize = '';
              this.choose[0].color = '';
              this.choose[1].color = '';
              this.choose[0].effect = '';
              this.choose[1].effect = '';
              this.choose.splice(0, this.choose.length); 
              this.i.splice(0, this.i.length); 
            }, 1200);

          }

        }

      } //else 

    },

    reiniciar() {
      this.dataArray();
      this.shuffle(this.array);
      this.choose.splice(0, this.choose.length); 
      this.moves = 0;
      clearInterval(this.interval);
      this.second = 0;
      this.minute = 0;
      this.hour = 0;
      this.moveHit = 0;
      this.show='';
    },

    movesAdd() {
      this.moves+=1;
      if (this.moves == 1) {
        this.startTimer();
      }
    },

    movesHit() {
      this.moveHit+=1;
      if (this.moveHit == 8) {
        this.congratulations();
      }
    },

    dataArray() {
      this.array = [
        {type: 'diamond',icon: 'fa-diamond', disable: '', fontSize: '', effect: ''},
        {type: 'diamond',  icon: 'fa-diamond', disable: '', effect: ''},
        {type: 'bolt',icon: 'fa-bolt', disable: ''},
        {type: 'bolt',  icon: 'fa-bolt', disable: ''},
        {type: 'leaf',  icon: 'fa-leaf', disable: ''},
        {type: 'leaf',  icon: 'fa-leaf', disable: ''},
        {type: 'bicycle',  icon: 'fa-bicycle', disable: '', color: ''},
        {type: 'bicycle',  icon: 'fa-bicycle', disable: '', color: ''},
        {type: 'anchor',  icon: 'fa-anchor', disable: '', color: ''},
        {type: 'anchor',  icon: 'fa-anchor', disable: '', color: ''},
        {type: 'bomb',  icon: 'fa-bomb', disable: '', color: ''},
        {type: 'bomb',  icon: 'fa-bomb', disable: '', color: ''},
        {type: 'rocket',  icon: 'fa-rocket', disable: '', color: ''},
        {type: 'rocket',  icon: 'fa-rocket', disable: '', color: ''},
        {type: 'key',  icon: 'fa-key', disable: '', color: ''},
        {type: 'key',  icon: 'fa-key', disable: '', color: ''}
      ]
    },

    congratulations() {
      clearInterval(this.interval);
      setTimeout(() => {
        this.show = 'show';
      }, 1500);
    },

    startTimer() {
      this.second = 0;
      this.minute = 0;
      this.hour = 0;
      this.interval = setInterval(() => {
        this.second++;
        if (this.second == 60) {
          this.minute++;
          this.second = 0;
        }
        if (this.minute == 60) {
          this.hour++;
          this.minute = 0;
        }
      }, 1000);
    },

    shuffle(array) {
      let currentIndex = array.length,  randomIndex;
      while (currentIndex != 0) {
        randomIndex = Math.floor(Math.random() * currentIndex);
        currentIndex--;
        [array[currentIndex], array[randomIndex]] = [
          array[randomIndex], array[currentIndex]];
      }
      return this.array = array;
    },
  },
  
  watch: {
    
  },
  created() {
    this.dataArray();
    this.shuffle(this.array);
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
body, h1, h2, p, ul, li, a {
  margin: 0;
  padding: 0;
}

ul {
  list-style: none;
  display: flex;
  justify-content: center;
  flex-wrap: wrap;
}

a {
  text-decoration: none;
}

li {
  text-decoration: none;
  display: inline-block;
}


span {
  color: #fff
}

.menu-logo h1 a {
  color: #31485a;
}

.menu-bg {
  width: 100%;
  background: #f0f0f0; 
}

/* .menu-bg */
.menu {
  max-width: 960px;
  margin: 0 auto;
  display: flex;
  justify-content: space-around;
  align-items: center;
  padding: 20px 0;
  height: 50px;
}

/* Área */
.menu-area {
  margin: 15px auto;
  padding: 10px 0;
  max-width: 560px;
  background-color: rgba(255,255,255, 1);
  border-radius: 15px;
}

.menu-area li {
  width: 40px;
  height: 40px;
  display: inline-grid;
  align-items: center;
  padding: 25px;
  border-radius: 50%;
  font-size: 0;
  color: #fff;
  background-color: #31485a;
  margin: 15px;
  cursor: pointer;
}

.btn-menu {
  background-color: #fda142;
  color: #fff;
  padding: 15px 25px;
  border-radius: 25px;
}

.disable {
  pointer-events: none;
}

.menu-area li.open {
  transform: rotateY(0);
  background: #bbd0dc;
  cursor: default;
  animation-name: flipInY;
  -webkit-backface-visibility: visible !important;
  backface-visibility: visible !important;
  animation-duration: 0.45s;
  
}

.menu-area li.unmatched{
  animation-name: pulse;
  -webkit-backface-visibility: visible !important;
  backface-visibility: visible !important;
  animation-duration: 0.75s;
  background: #e2043b;
}

.menu-area li.match{
  cursor: default;
  background-color: #fda142 !important;
  font-size: 33px;
  animation-name: rubberBand;
  -webkit-backface-visibility: visible !important;
  backface-visibility: visible !important;
  animation-duration: 0.75s;
  
}


.fs36 {
  font-size: 36px !important;
}





.info {
  width: 200px;
  background-color: #e1e6ec;
  vertical-align: middle;
  align-self: center;
  padding: 15px 5px;
  margin: 0 10px;
  display: flex;
  justify-content: center;
  gap: 5px;
  

}

.yellow {
   color: #fda142;
   padding: 0 3px;
}

.black {
  color: #2C3E50;
}

.bg-fundo {
  height: 100vh;
  /* background-image: url("../assets/simple-hand.png"); */
  background-size: contain;
}



/****** Media queries
***************************/


@media (min-width: 1279px) {

  .menu-area {
    margin: 15px auto;
    padding: 10px 0;
    max-width: 620px;
    background-color: rgba(255,255,255, 1);
    border-radius: 15px;
  }


  .menu-area li {
    width: 80px;
    height: 80px;
    display: inline-grid;
    align-items: center;
    padding: 15px;
    border-radius: 50%;
    font-size: 0;
    color: #fff;
    background-color: #31485a;
    margin: 15px;
    cursor: pointer;
  }

}

@media (min-width: 770px) {
  .info-bg {
    width: 100%;
    margin: 0 auto;
    display: flex;
    justify-content: center;
    bottom: 0;
    padding-bottom: 20px;
    position: absolute;
  }

  .menu-area ul li {
    transition: 0.3s ease-in;
  }

  .menu-area ul li:hover {
    transition: 0.3s ease-out;
    transform: scale(1.1);
  }
}

@media (max-width: 769px) {

  .info-bg {
    width: 100%;
    margin: 0 auto;
    display: flex;
    justify-content: center;
    bottom: 0;
    padding-bottom: 20px;
  }

  .menu {
    max-width: 360px;
    margin: 0 auto;
    display: flex;
    justify-content: space-around;
    align-items: center;
    padding: 20px 0;
    height: 50px;
  }

  /* Área */
  .menu-area {
    margin: 15px auto;
    padding: 10px 0;
    max-width: 560px;
    background-color: rgba(255,255,255, 1);
    border-radius: 15px;
  }

  .menu-area li {
    width: 20px;
    height: 20px;
    display: inline-grid;
    align-items: center;
    padding: 25px;
    border-radius: 50%;
    font-size: 0;
    color: #fff;
    background-color: #31485a;
    margin: 15px;
    cursor: pointer;
  }

  .fs36 {
    font-size: 20px !important;
  }

}































/*
 * Styles for congratulations modal
 */

.overlay {
  position: fixed;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  background: rgba(0, 0, 0, 0.7);
  transition: opacity 500ms;
  visibility: hidden;
  opacity: 0;
}

.overlay:target {
  visibility: visible;
  opacity: 1;
}

.popup {
  margin: 50px auto;
  padding: 50px 0;
  background: #ffffff;
  border-radius: 5px;
  width: 85%;
  position: relative;
  transition: all 2s ease-in-out;
}

.popup h2 {
  margin-top: 0;
  color: #333;
  font-family: Tahoma, Arial, sans-serif;
}

.popup .close {
  position: absolute;
  top: 20px;
  right: 30px;
  transition: all 500ms;
  font-size: 30px;
  font-weight: bold;
  text-decoration: none;
  color: #333;
  cursor: pointer;
}

.popup .close:hover {
  color: #e5f720;
}

.popup .content-1,
.content-2 {
  max-height: 30%;
  overflow: auto;
  text-align: center;
}

.show {
  visibility: visible !important;
  opacity: 100 !important;
}

.content-1
.content-2 {
  max-height: 30%;
  overflow: auto;
  text-align: center;
}


/* animations */
@keyframes flipInY {
  from {
    transform: perspective(400px) rotate3d(0, 1, 0, 90deg);
    animation-timing-function: ease-in;
    opacity: 0;
  }

  40% {
    transform: perspective(400px) rotate3d(0, 1, 0, -20deg);
    animation-timing-function: ease-in;
  }

  60% {
    transform: perspective(400px) rotate3d(0, 1, 0, 10deg);
    opacity: 1;
  }

  80% {
    transform: perspective(400px) rotate3d(0, 1, 0, -5deg);
  }

  to {
    transform: perspective(400px);
  }
}

@keyframes pulse {
  from {
    transform: scale3d(1, 1, 1);
  }

  50% {
    transform: scale3d(1.2, 1.2, 1.2);
  }

  to {
    transform: scale3d(1, 1, 1);
  }
}

@keyframes rubberBand {
  from {
    transform: scale3d(1, 1, 1);
  }

  30% {
    transform: scale3d(1.25, 0.75, 1);
  }

  40% {
    transform: scale3d(0.75, 1.25, 1);
  }

  50% {
    transform: scale3d(1.15, 0.85, 1);
  }

  65% {
    transform: scale3d(0.95, 1.05, 1);
  }

  75% {
    transform: scale3d(1.05, 0.95, 1);
  }

  to {
    transform: scale3d(1, 1, 1);
  }
}
</style>
