<script>
export default {
  data() {
    return {
      x: 0,
      y: 0,
      skin: "spy",
      name: "",
      result: "",
      actions: [],
      timer: 1000,
    };
  },
  methods: {
    moveTop(){

      setTimeout(()=>{if (this.y>0){
      this.y-=50
    }}, this.timer)
    },
    moveDown(){
      setTimeout(()=>{if(this.y<600){
      this.y+=50
      }}, this.timer)
      
    },
    moveRight(){
      setTimeout(()=>{if(this.x<1000){
      this.x+=50
      }}, this.timer)
      
    },
    moveLeft(){
      setTimeout(()=>{if(this.x>0){
      this.x-=50
      }}, this.timer)
      
    },
    removeAction(index){
      this.actions.splice(index, 1)
    },
    action() {
      this.timer = 1000; // обновляем таймер в каждый новый вызов
      
      // пройдёмся по всему массиву actions и запланируем шаги
      for (let i = 0; i < this.actions.length; i++) {
        if (this.actions[i] == "Вверх") {
          this.moveTop()
        } else if (this.actions[i] == "Вниз") {
          this.moveDown()
        } else if (this.actions[i] == "Вправо") {
          this.moveRight()
        }else if (this.actions[i] == "Влево") {
          this.moveLeft()}
        setTimeout(() => {
          this.actions.shift();
        }, this.timer);

        // каждый новый шаг, должен быть через секунду от предыдущего
        this.timer += 1000;
      }
    }
  }
};
</script>

<template>
  <main class="d-flex">
    <div class="card" style="width: 18rem; height: 100vh">
      <div class="card-header">Действия</div>
      <ul class="list-group list-group-flush" >
        <!-- Выводи сюда все действия из массива actions -->
        <li class="list-group-item" v-for="action in actions">{{ action }} <button @click="removeAction(index)">X</button></li> 
      </ul>
      <div class="card-body action-btns row">

        <!-- При нажатии на кнопку, добавляй строку с названием стороны в массив actions -->
        <button class="btn btn-outline-primary col-12 mb-1" @click="actions.push('Вверх')">
          Вверх
        </button>
        <button class="btn btn-outline-primary col-6" @click="actions.push(`Влево`)" >
          Влево
        </button>
        <button class="btn btn-outline-primary col-6" @click="actions.push(`Вправо`)">
          Вправо
        </button>
        <button class="btn btn-outline-primary col-12 mt-1" @click="actions.push(`Вниз`)">
          Вниз
        </button>

        <button class="btn btn-success mt-3 col-12" @click="action">
          Выполнить
        </button>
      </div>
    </div>
    <div class="container">
      <div class="game">Он всегда на шаг позади тебя...</div>

      <div class="spy" :style="{
        left: x + 'px',
        top: y + 'px',
      }">
        <p id="result-name"></p>
        <div id="skin-container">
          <!-- Формирование пути до изображения с помощью v-bind -->
          <img :src="'src/assets/' + skin + '.png'" alt="" />
        </div>
        <!-- Cюда выводятся текущии координаты персонажа на странице и его имя -->
        <p id="result-click">x: {{ x }}, y: {{ y }}</p>
        <p id="result-type">{{ name }}</p>
      </div>
      <div class="controls d-flex gap-1">
        <!-- Привяжи это поле к data-свойству name -->
        <input class="form-control" type="text" placeholder="Имя персонажа" v-model="name"/>
        <!-- Привяжи этот селект к data-свойству skin -->
        <select class="form-select" v-model="skin">
          <option value="spy">Шпион</option>
          <option value="dino">Динозаврик</option>
          <option value="ghost">Призрак</option>
        </select>
      </div>
    </div>
  </main>
</template>

<style>
body {
  margin: 0;
}

.container {
  display: flex;
  position: relative;
  flex-direction: column;
  height: 100vh;
  justify-content: center;
  align-items: center;
}

.spy {
  height: 100px;
  width: 100px;
  text-align: center;
  position: absolute;
  top: 0;
  left: 0;
  transition: 1s;
}

img {
  width: 100%;
}

.controls {
  /* Зафиксируем положение внизу экрана, независимо от скрола */
  position: absolute;
  bottom: 10px;
  right: 50px;
  left: 50px;
}

.action-btns {
  position: absolute;
  bottom: 10px;
}
</style>