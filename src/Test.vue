<template>
  <div class="main-wrapper">
    <div>
      <div>Next player: {{ active }}</div>
      <div class="wrapper">
        <div class="item" @click="addArray(0, 0)">{{ activeArray[0][0] }}</div>
        <div class="item" @click="addArray(0, 1)">{{ activeArray[0][1] }}</div>
        <div class="item" @click="addArray(0, 2)">{{ activeArray[0][2] }}</div>
        <div class="item" @click="addArray(1, 0)">{{ activeArray[1][0] }}</div>
        <div class="item" @click="addArray(1, 1)">{{ activeArray[1][1] }}</div>
        <div class="item" @click="addArray(1, 2)">{{ activeArray[1][2] }}</div>
        <div class="item" @click="addArray(2, 0)">{{ activeArray[2][0] }}</div>
        <div class="item" @click="addArray(2, 1)">{{ activeArray[2][1] }}</div>
        <div class="item" @click="addArray(2, 2)">{{ activeArray[2][2] }}</div>
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
          @click="getShag(index)"
          :class="
            this.shags[index].activeArray == this.activeArray ? 'active' : ''
          "
        >
          shag #{{ index + 1 }}
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      active: "X",
      activeArray: [
        ["", "", ""],
        ["", "", ""],
        ["", "", ""],
      ],

      shags: [],
      win: "None",
      disable: false,
      resetShagMode: -2,
    };
  },

  mounted() {
    this.getFromLocalStorage();
  },

  methods: {
    getFromLocalStorage() {
      if (JSON.parse(localStorage.getItem("activeArray"))) {
        this.activeArray = JSON.parse(localStorage.getItem("activeArray"));
      }

      if (JSON.parse(localStorage.getItem("shags"))) {
        this.shags = JSON.parse(localStorage.getItem("shags"));
      }

      if (localStorage.getItem("active")) {
        this.active = localStorage.getItem("active");
      }

      if (localStorage.getItem("win")) {
        this.win = localStorage.getItem("win");
      }
    },

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

        if (this.resetShagMode > -1) {
          this.resetShags();
        }
        this.activeArray[x][y] = this.active;
        this.checkGame(this.activeArray);
        if (this.active == "X") {
          this.active = "O";
        } else {
          this.active = "X";
        }

        this.setLocalStorage();

        const shagItem = {
          activeArray: JSON.parse(localStorage.getItem("activeArray")),
          active: this.active,
          win: this.win,
        };

        this.shags.push(shagItem);
        localStorage.setItem("shags", JSON.stringify(this.shags));
      }
    },

    setLocalStorage() {
      localStorage.setItem("active", this.active);
      localStorage.setItem("activeArray", JSON.stringify(this.activeArray));
      localStorage.setItem("win", this.win);
    },

    getShag(index) {
      this.activeArray = JSON.parse(
        JSON.stringify(this.shags[index].activeArray)
      );
      this.active = this.shags[index].active;
      this.resetShagMode = index;
      this.win = this.shags[index].win;
      if (this.disable) {
        this.disable = false;
      }
    },

    resetShags() {
      this.shags = this.shags.filter((item, idx) => {
        if (this.resetShagMode < idx) {
          return false;
        }

        return true;
      });

      this.resetShagMode = -2;
    },

    restart() {
      this.activeArray = [
        ["", "", ""],
        ["", "", ""],
        ["", "", ""],
      ];
      this.shags = [];
      this.win = "None";
      this.disable = false;
      this.active = "X";
      window.localStorage.clear();
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
