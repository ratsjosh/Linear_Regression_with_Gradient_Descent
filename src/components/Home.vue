<template>
  <div>
    <div class="wrapper">
      <div class="one">
        <h1>{{ msg }}</h1>
        <hr >
        <h1>thetaZero: {{ thetaZero }}</h1>
        <h1>thetaOne: {{ thetaOne }}</h1>
        <h1>cost: {{ error }}</h1>
      </div>
      <div class="two">
        <scatter-chart
          :chartData="{ datasets: [
          {
            label: 'Hyopthesis',
            data: [{
              x: 0,
              y: this.hypothesis(0)
            }, {
              x: 100,
              y: this.hypothesis(100)
            }],
            backgroundColor: '#3e95cd',
            borderColor: '#3e95cd',
            // Changes this dataset to become a line
            type: 'line',
            showLine: true,
            fill: false,
          },
          {
            label: 'Scatter Dataset',
            data: this.collection,
          }]
          }"
        />
      </div>
    </div>
  </div>
</template>

<script>
import ScatterChart from './ScatterChart';

export default {
  name: 'Home',
  components: {
    ScatterChart,
  },
  data() {
    return {
      msg: 'Linear regression with gradient descent',
      collection: [],
      x: [],
      y: [],
      m: 100,
      learningRate: 0.0003,
      thetaZero: 0,
      thetaOne: 0,
      error: 0,
    };
  },
  methods: {
    getRandomIntFromInterval: (min, max) => Math.floor((Math.random() * (max - (min + 1))) + min),
    createRandomPoints() {
      const squareMeter = this.getRandomIntFromInterval(0, 100);
      const price = this.getRandomIntFromInterval(0, 100);
      return {
        x: squareMeter,
        y: price,
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
      this.collection.push(this.createRandomPoints());
    }
    this.x = this.collection.map(date => date.x);
    this.y = this.collection.map(date => date.y);
  },

  mounted() {
    const self = this;
    setInterval(() => {
      self.learn();
    }, 1);
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
.wrapper {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  grid-gap: 10px;
  grid-auto-rows: minmax(100px, auto);
}
.one {
  grid-column: 1;
  grid-row: 1;
}
.two { 
  grid-column: 2;
  grid-row: 1;
}
</style>
