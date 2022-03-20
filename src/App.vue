<template>
  <div class="container">
    <div class="info">
      <p>turn : me</p>
      <p>my score : 6</p>
      <p>my score : 9</p>
    </div>
    <div class="game">
      <div class="item"></div>
      <div class="item"></div>
      <div class="item item-me"></div>
      <div class="item"></div>
      <div class="item"></div>
      <div class="item item-enemy"></div>
      <div class="item"></div>
      <div class="item"></div>
      <div class="item"></div>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  components: {},
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
.item:not(.item-me, .item-enemy):hover {
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
