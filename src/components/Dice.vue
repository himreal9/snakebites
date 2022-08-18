<template>
  <Board :num="num" :currPosition="currPosition" />
  <div class="row">
    <div class="col-4 dicebox">
      <div
        style="font-weight: 300; font-size: 40px; padding-left: 250px"
        v-if="currPosition == 0"
      >
        🐥 - Roll 6 to Start the Game
      </div>
      <button v-else class="btn btn-warning resetbtn" @click="reset">
        Reset Game
      </button>
    </div>
    <div class="col-4">
      <div class="dicebox">
        {{ dicetype }} Dice
        <div :class="{ dice: true, 'dice animation': !rollprocess }">
          {{ num }}
        </div>
        <button v-if="rollprocess" class="btn btn-info rollbtn" @click="roll">
          Roll
        </button>
      </div>
    </div>
    <div class="col-4 dicebox" style="margin-left: -100px">
      <div class="console" id="console">
        {{ this.log }}...<small class="dot">&#9647;</small>
      </div>
    </div>
  </div>

  <div class="popup" v-if="!dicetype">
    <div class="selectdice">
      Select the Dice Type
      <br />
      <br />
      <button class="btn btn-info rollbtn" @click="dicetype = 'Normal'">
        Normal
      </button>
      <br />
      <br />
      <button class="btn btn-info rollbtn" @click="dicetype = 'Cooked'">
        Cooked
      </button>
    </div>
  </div>
</template>

<script>
import Board from "../components/Board.vue";
export default {
  data() {
    return {
      num: "🎲",
      rollprocess: true,
      currPosition: 0,
      log: `Roll the Dice`,
      dicetype: undefined,
      snakes: [
        [30, 7],
        [58, 45],
        [99, 9],
      ],
    };
  },
  methods: {
    reset() {
      this.num = "🎲";
      this.currPosition = 0;
      this.log = `Roll the Dice`;
      this.rollprocess = true;
      this.dicetype = undefined;
    },
    roll() {
      this.rollprocess = false;
      this.num = "🎲";
      setTimeout(() => {
        this.rollprocess = true;
        let x = Math.floor(Math.random() * 6 + 1);
        if (this.dicetype == "Cooked") {
          if (x % 2 != 0) x += 1;
        }
        this.num = x;
        if (this.currPosition + this.num < 101 && this.currPosition != 0)
          this.currPosition += this.num;
        for (let i=0; i<this.snakes.length; i++) {
          if (this.currPosition == this.snakes[i][0]) {
            this.currPosition = this.snakes[i][1];
            this.log += `
Snake Bite`;
          }
        }
        if (this.currPosition == 0 && x == 6) this.currPosition += 1;
        this.log += `
Number on Dice: ${x}, Position: ${this.currPosition}`;
        if (this.currPosition == 100) {
          this.log += `
Congratulations You Won!`;
          this.rollprocess = false;
        }
      }, 1000);
      setTimeout(() => {
        var elem = document.getElementById("console");
        elem.scrollTop = elem.scrollHeight + 10;
      }, 1200);
    },
  },
  components: {
    Board,
  },
  name: "DicE",
};
</script>

<style>
@import url(https://cdn.jsdelivr.net/npm/bootstrap@5.0.0-beta1/dist/css/bootstrap.min.css);
.dicebox {
  margin-top: 20px;
  height: 200px;
}
.dice {
  width: 100px;
  height: 100px;
  border-radius: 25px;
  border: 3px solid;
  margin: auto;
  margin-bottom: 20px;
  font-family: "Lucida Handwriting";
  font-weight: 300;
  font-size: 60px;
  text-align: center;
}
.console {
  background-color: black;
  height: 150px;
  width: 300px;
  padding: 10px;
  overflow-y: auto;
  text-align: left;
  color: white;
  border-radius: 5px;
  white-space: pre;
}

.console::-webkit-scrollbar {
  width: 10px;
}

.console::-webkit-scrollbar-track {
  background-color: #e7e7e7;
}

.console::-webkit-scrollbar-thumb {
  background-color: rgba(48, 46, 46, 0.69);
  border: 1px solid rgba(92, 92, 92, 0.5);
  border-radius: 10px;
}

.console::-webkit-scrollbar-thumb:hover {
  background-color: #000;
}
.animation {
  animation: roll 0.5s linear 0.5s 1 alternate;
}
.rollbtn {
  width: 100px;
}

.resetbtn {
  width: 200px;
  margin-top: 60px;
  margin-left: 220px;
}

.dot {
  animation: blink 1s infinite;
  font-size: 18px;
}

@keyframes blink {
  0%,
  100% {
    opacity: 0;
  }
  /* more concise! */
  50% {
    opacity: 1;
  }
}

@keyframes roll {
  0% {
    transform: translateZ(-100px) rotateX(0deg) rotateY(0deg) rotateZ(0deg);
  }
  16% {
    transform: translateZ(-100px) rotateX(180deg) rotateY(180deg) rotateZ(0deg);
  }
  33% {
    transform: translateZ(-100px) rotateX(360deg) rotateY(90deg) rotateZ(180deg);
  }
  50% {
    transform: translateZ(-100px) rotateX(360deg) rotateY(360deg)
      rotateZ(360deg);
  }
  66% {
    transform: translateZ(-100px) rotateX(180deg) rotateY(360deg)
      rotateZ(270deg);
  }
  83% {
    transform: translateZ(-100px) rotateX(270deg) rotateY(180deg)
      rotateZ(180deg);
  }
  100% {
    transform: translateZ(-100px) rotateX(360deg) rotateY(360deg)
      rotateZ(360deg);
  }
}

.popup {
  position: absolute;
  width: 98vw;
  height: 116vh;
  top: 0;
  left: 0;
  background: #3030305a;
}

.selectdice {
  background: #fff;
  width: 400px;
  text-align: center;
  padding: 20px;
  border-radius: 5px;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}
</style>
