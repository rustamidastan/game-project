<template>
  <div class="main-wrapper">
    <div>
      <div>Next player: {{ active }}</div>
      <div class="wrapper">
        <div class="item" @click="addArray(0, 0)">{{ defaultArr[0][0] }}</div>
        <div class="item" @click="addArray(0, 1)">{{ defaultArr[0][1] }}</div>
        <div class="item" @click="addArray(0, 2)">{{ defaultArr[0][2] }}</div>
        <div class="item" @click="addArray(1, 0)">{{ defaultArr[1][0] }}</div>
        <div class="item" @click="addArray(1, 1)">{{ defaultArr[1][1] }}</div>
        <div class="item" @click="addArray(1, 2)">{{ defaultArr[1][2] }}</div>
        <div class="item" @click="addArray(2, 0)">{{ defaultArr[2][0] }}</div>
        <div class="item" @click="addArray(2, 1)">{{ defaultArr[2][1] }}</div>
        <div class="item" @click="addArray(2, 2)">{{ defaultArr[2][2] }}</div>
      </div>

      <div>
        <h1>Win: {{ win }}</h1>
      </div>

      <div>
        <button @click="restart">Restart</button>
      </div>
    </div>

    <div>
      <h3>Shags:</h3>
      <div v-for="(shag, index) in shags" :key="shag">
        <button
          :class="[this.defaultArr == this.shags[index] ? 'active' : '']"
          @click="getShag(index)"
        >
          shag #{{ index }}
        </button>
      </div>

      <button
        @click="live"
        :class="[this.defaultArr == this.activeArray ? 'active' : '']"
      >
        Live
      </button>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      active: "X",
      defaultArr: [
        ["", "", ""],
        ["", "", ""],
        ["", "", ""],
      ],
      activeArray: [
        ["", "", ""],
        ["", "", ""],
        ["", "", ""],
      ],

      shags: [],
      win: "None",
      disable: false,
    };
  },

  methods: {
    checkGame(itemArray) {
      for (let i = 0; i < 3; i++) {
        for (let j = 0; j < 3; j++) {
          if (
            itemArray[0][j] == itemArray[1][j] &&
            itemArray[0][j] == itemArray[2][j] &&
            itemArray[0][j] != "" &&
            itemArray[1][j] != "" &&
            itemArray[2][j] != ""
          ) {
            this.win = itemArray[0][j];
            this.disable = true;
          }

          if (
            itemArray[j][0] == itemArray[j][1] &&
            itemArray[j][0] == itemArray[j][2] &&
            itemArray[j][0] != "" &&
            itemArray[j][1] != "" &&
            itemArray[j][2] != ""
          ) {
            this.win = itemArray[j][0];
            this.disable = true;
          }

          if (
            itemArray[0][0] == itemArray[1][1] &&
            itemArray[0][0] == itemArray[2][2] &&
            itemArray[0][0] != "" &&
            itemArray[1][1] != "" &&
            itemArray[2][2] != ""
          ) {
            this.win = itemArray[0][0];
            this.disable = true;
          }

          if (
            itemArray[0][2] == itemArray[1][1] &&
            itemArray[0][2] == itemArray[2][0] &&
            itemArray[0][2] != "" &&
            itemArray[1][1] != "" &&
            itemArray[2][0] != ""
          ) {
            this.win = itemArray[0][2];
            this.disable = true;
          }
        }
      }
    },

    addArray(x, y) {
      if (!this.disable) {
        if (this.activeArray[x][y]) {
          return;
        }
        this.activeArray[x][y] = this.active;
        this.defaultArr = this.activeArray;
        this.shags.push(JSON.parse(JSON.stringify(this.activeArray)));
        this.checkGame(this.activeArray);
        if (this.active == "X") {
          this.active = "O";
        } else {
          this.active = "X";
        }
      }
    },

    getShag(index) {
      this.defaultArr = this.shags[index];
      this.disable = true;
    },

    live() {
      this.defaultArr = this.activeArray;
      if (this.win == "None") {
        this.disable = false;
      }
    },
    restart() {
      this.activeArray = [
        ["", "", ""],
        ["", "", ""],
        ["", "", ""],
      ];
      this.defaultArr = [
        ["", "", ""],
        ["", "", ""],
        ["", "", ""],
      ];
      this.shags = [];
      this.win = "None";
      this.disable = false;
      this.active = "X";
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  color: #2c3e50;
  margin-top: 60px;
}

body {
  padding: 50px;
}

.main-wrapper {
  display: flex;
}

.wrapper {
  max-width: 200px;
  margin-top: 20px;
  margin-right: 50px;
  display: grid;
  grid-template-columns: repeat(3, 50px);
  grid-auto-rows: auto;
}

.item {
  width: 50px;
  height: 50px;
  border: 1px solid #eee;
  display: flex;
  align-items: center;
  justify-content: center;
}

button.active {
  background-color: rgba(0, 0, 255, 0.7);
  color: #fff;
}
</style>
