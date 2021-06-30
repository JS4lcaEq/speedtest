<template>
  <div class="hello">
    items: 
    <input v-model="recordsCount" />
    <button @click="create">CREATE</button>
    <button @click="sort">SORT (order by value)</button>
    <button @click="unSort">UNSORT (order by id)</button>
    
    {{arr.length}} items in {{length}} ms
    <p>{{ debug }}</p>
    <canvas id="canvas"></canvas>
  </div>
</template>

<script>
let canvas;
let context;
export default {
  name: "HelloWorld",
  props: {
    msg: String,
  },
  data() {
    return {
      recordsCount: 50000,
      arr: [],
      debug: "",
      start: 0,
      end: 0,
      length: 0
    };
  },
  methods: {
    render() {
      let k = 500 / this.arr.length;
      let i = 0
      context.clearRect(0, 0, canvas.width, canvas.height);
      this.arr.forEach((e) => {
        context.fillRect(i * k, e.dt * k, 1, 1);
        i++
      });
    },
    create() {
      this.start = new Date().valueOf()
      let arr_ = [];
      let i = 0;

      for (i = 0; i < this.recordsCount; i++) {
        let dot = { id: i, dt: Math.round(Math.random() * this.recordsCount) };
        arr_.push(dot);
      }
      this.arr = arr_;
      this.end = new Date().valueOf()
      this.length = this.end - this.start
      this.render();
    },
    sort() {
      this.start = new Date().valueOf()
      this.arr.sort(function (a, b) {
        return a.dt - b.dt;
      });
      this.end = new Date().valueOf()
      this.length = this.end - this.start
      this.render();
    },
    unSort() {
      this.start = new Date().valueOf()
      this.arr.sort(function (a, b) {
        return a.id - b.id;
      });
      this.end = new Date().valueOf()
      this.length = this.end - this.start
      this.render();
    },
  },
  mounted() {
    canvas = document.getElementById("canvas");
    canvas.width = 500;
    canvas.height = 500;
    context = canvas.getContext("2d");
    this.debug += "w=" + canvas.width + " h=" + canvas.height;
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
#canvas {
  background-color: #ccc;
  /* width: 500px; */
  /* height: 500px; */
}
button {
  margin: 5px;
}
</style>
