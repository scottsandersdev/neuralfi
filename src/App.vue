<template>
  <div id="app">
    <canvas ref="paperCanvas" data-paper-resize="true"></canvas>
    <div>{{newDrawing.input.length/2}}</div>
    <div>{{xStart}}--{{yStart}}</div>
    <div class="point-display">{{drawnPoints}}</div>
  </div>
</template>

<script>
import paper from 'paper'

const methods = {};

methods.addPoints = function(event) {
  this.newDrawing.input.push(Math.floor(event.point.x - this.xStart), event.point.y - this.yStart);
}

methods.onMouseDrag = function(event) {
  path.add(event.point);
}

export default {
  name: 'app',
  data () {
    return {
      drawnPoints: [],
      newDrawing: {
        input: [],
        output: [1]
      },
      xStart: 0,
      yStart: 0
    }
  },
  mounted: function() {
    paper.install(window);
    paper.setup(this.$refs.paperCanvas)
    let tool = new Tool();
    let path;

    tool.onMouseDown = (event)=> {
      this.drawnPoints = []; //*** REMOVE! DEBUG CODE! TEMPORARY!
      this.newDrawing.input = [];
      path = new Path();
      path.strokeColor = 'black';
      path.strokeWidth = 7;
      path.add(event.point);
      this.xStart = Math.floor(event.point.x);
      this.yStart = event.point.y;
      this.addPoints(event);
    }

    tool.onMouseDrag = (event)=> {
      path.add(event.point);
      this.addPoints(event);
    }

    tool.onMouseUp = (event)=> {
      this.drawnPoints.push(this.newDrawing);
      project.activeLayer.removeChildren();
    }

  },
  methods: methods
}
</script>

<style lang="scss">
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

canvas {
  border: 1px solid grey;
  width: 60%;
}

.point-display {
  font-size: 14px;
  // white-space: nowrap;
  // overflow: hidden;
  text-overflow: ellipsis;
}

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
