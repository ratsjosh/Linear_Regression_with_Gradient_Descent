<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <!-- <button v-on:click="learn">Iterate 1</button> -->
    <hr >
    <h1>thetaZero: {{ thetaZero }}</h1>
    <h1>thetaOne: {{ thetaOne }}</h1>
    <h1>cost: {{ error }}</h1>
  </div>
</template>

<script>
export default {
  name: 'Home',
  data() {
    return {
      msg: 'Linear regression with gradient descent',
      collection: [],
      x: [],
      y: [],
      m: 10,
      learningRate: 0.0003,
      thetaZero: 0,
      thetaOne: 0,
      error: 0,
    };
  },
  methods: {
    getRandomIntFromInterval: (min, max) => Math.floor((Math.random() * (max - (min + 1))) + min),
    createRandomPortlandHouse() {
      const squareMeter = this.getRandomIntFromInterval(0, 100);
      const price = this.getRandomIntFromInterval(0, 100);
      return {
        squareMeter,
        price,
      };
    },
    hypothesis(x) {
      return this.thetaZero + (this.thetaOne * x);
    },
    cost() {
      let sum = 0;
      for (let i = 0; i < this.m; i++) {
        sum += (this.hypothesis(this.x[i]) - this.y[i]) ** 2;
      }
      this.error = sum / (2 * this.m);
    },
    learn() {
      let thetaZeroSum = 0;
      let thetaOneSum = 0;
      for (let i = 0; i < this.m; i++) {
        thetaZeroSum += this.hypothesis(this.x[i]) - this.y[i];
        thetaOneSum += (this.hypothesis(this.x[i]) - this.y[i]) * this.x[i];
      }
      this.thetaZero = this.thetaZero - ((this.learningRate / this.m) * thetaZeroSum);
      this.thetaOne = this.thetaOne - ((this.learningRate / this.m) * thetaOneSum);
      this.cost();
    },
  },

  created() {
    for (let i = 0; i < this.m; i++) {
      this.collection.push(this.createRandomPortlandHouse());
    }
    this.x = this.collection.map(date => date.squareMeter);
    this.y = this.collection.map(date => date.price);
  },

  mounted() {
    const self = this;
    setInterval(() => {
      self.learn();
    }, 500);
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
