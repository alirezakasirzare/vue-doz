<template>
  <div class="container">
    <div class="info">
      <p ref="info-turn"></p>
      <p>my score : {{ myScore }}</p>
      <p>my score : {{ enemyScore }}</p>
    </div>
    <div class="game">
      <div
        v-for="(item, index) in items"
        :key="index"
        @click="
          () => {
            clickItemHandeler(index);
          }
        "
        class="item"
        :class="{ [`item-${item}`]: item, 'item-hover': turn == 'me' }"
      ></div>
    </div>
  </div>
</template>

<script>
const initialItems = () => [
  null,
  null,
  null,
  null,
  null,
  null,
  null,
  null,
  null,
];

const arrayContain = (chooses, should) => {
  let willBack = true;
  should.forEach((item) => {
    if (!chooses.includes(item)) {
      willBack = false;
    }
  });

  return willBack;
};

const checkEndGame = (chooses) => {
  return (
    arrayContain(chooses, [1, 2, 3]) ||
    arrayContain(chooses, [4, 5, 6]) ||
    arrayContain(chooses, [7, 8, 9]) ||
    arrayContain(chooses, [1, 4, 7]) ||
    arrayContain(chooses, [2, 5, 8]) ||
    arrayContain(chooses, [3, 6, 9]) ||
    arrayContain(chooses, [1, 5, 9]) ||
    arrayContain(chooses, [3, 5, 7])
  );
};

export default {
  name: "App",
  data() {
    return {
      turn: "me",
      items: initialItems(),
      enemyScore: 0,
      myScore: 0,
      myChooses: [],
      enemyChooses: [],
    };
  },
  methods: {
    clickItemHandeler(index) {
      if (this.turn === "me" && !this.items[index]) {
        const newItems = [...this.items];
        newItems[index] = "me";
        this.items = newItems;
        this.myChooses = [...this.myChooses, index];
      }
    },
    enemyChoose() {
      setTimeout(() => {
        const notChoosedYet = [...this.items]
          .map((item, index) => {
            if (!item) return index;
          })
          .filter((item) => item !== undefined);

        const newItems = [...this.items];
        const index =
          notChoosedYet[Math.floor(Math.random() * notChoosedYet.length)];
        newItems[index] = "enemy";
        this.items = newItems;
        this.enemyChooses = [...this.enemyChooses, index];
      }, 3000);
    },
  },
  watch: {
    items() {},
    myChooses(newValue) {
      if (checkEndGame(newValue.map((item) => item + 1))) {
        // win
        alert("win");
      } else if (!this.items.filter((item) => item === null).length) {
        // same
        alert("same");
      } else {
        this.turn = "enemy";
        this.enemyChoose();
      }
    },
    enemyChooses(newValue) {
      console.log(newValue);
      if (checkEndGame(newValue.map((item) => item + 1))) {
        // loose
        alert("loose");
      } else if (!this.items.filter((item) => item === null).length) {
        // same
        alert("same");
      } else {
        this.turn = "me";
      }
    },
  },
  mounted() {
    let points = "";
    setInterval(() => {
      points = points === "..." ? "" : points + ".";
      this.$refs["info-turn"].innerHTML = `turn : ${this.turn} ${
        this.turn == "enemy" ? points : ""
      }`;
    }, 300);
  },
};
</script>

<style>
.container {
  width: calc(100vw - 40px);
  height: calc(100vh - 40px);
  margin: 20px;
  border: 1px solid #777;
  border-radius: 10px;
  display: flex;
  justify-content: center;
  align-items: center;
  position: relative;
}

.info {
  position: absolute;
  top: 10%;
  left: 50%;
  transform: translateX(-50%);
  display: flex;
  justify-content: center;
  gap: 40px;
  font-family: sans-serif;
  padding: 10px;
  border-radius: 10px;
}

.game {
  width: 400px;
  height: 400px;
  display: flex;
  flex-wrap: wrap;
}
.item {
  width: calc(100% / 3);
  height: calc(100% / 3);
  border-right: 1px solid #999;
  border-bottom: 1px solid #999;
  display: flex;
  justify-content: center;
  align-items: center;
}

.item-hover:not(.item-me, .item-enemy):hover {
  background-color: #eee;
  cursor: pointer;
}

.item:nth-child(3n) {
  border-right: none;
}

.item:nth-child(n + 7) {
  border-bottom: none;
}

.item-me,
.item-enemy {
  position: relative;
}

.item-me::after {
  content: "";
  width: 50%;
  height: 50%;
  border: 3px solid #4caf50;
  border-radius: 50%;
}

.item-enemy::after,
.item-enemy::before {
  content: "";
  position: absolute;
  left: 50%;
  top: 50%;
  width: 3px;
  height: 60%;
  background-color: #f44336;
}

.item-enemy::after {
  transform: translate(-50%, -50%) rotate(45deg);
}

.item-enemy::before {
  transform: translate(-50%, -50%) rotate(-45deg);
}

/* .item-me::after {
  width: 50%;
  height: 50%;
  border: 3px solid #4caf50;
  border-radius: 50%;
}

.item-enemy {
  width: 50%;
  height: 50%;
  position: relative;
}

.item-enemy::after,
.item-enemy::before {
  content: "";
  position: absolute;
  left: 50%;
  top: 50%;
  width: 3px;
  height: 100%;
  background-color: #f44336;
}

.item-enemy::after {
  transform: translate(-50%, -50%) rotate(45deg);
}

.item-enemy::before {
  transform: translate(-50%, -50%) rotate(-45deg);
} */
</style>
