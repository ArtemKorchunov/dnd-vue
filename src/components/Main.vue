<template>
  <div class="drawer-wrap">
    <div>
    <draggable v-model="constantArray"  :options="dragOptions" @start="drag=true" @end="drag=false">
      <div v-for="(item, index) in myArray" :key="index" :class="item.class"><i :class="item.icon"></i></div>
    </draggable>
    <v-stage ref="stage" :config="configKonva" class="canvas-stage">
        <v-layer ref="layer">
        </v-layer>
    </v-stage>
    <draggable v-model="picked" :options="dragOptions" :class="['dragged-to', {'top-layer': drag}]" @add="onShapeAdd"> 
    </draggable>
      <img src="https://img.the-village.com.ua/the-village.com.ua/post_image-image/WOsn1iBLP1g9Z76s8P1Rug-wide.jpg" alt="">
    </div>
  </div>
</template>

<script>
import draggable from "vuedraggable";
export default {
  name: 'Main',
  components: {
    draggable,
  },
  data() {
    return {
      configKonva: {
        width: 1020,
        height: 680
      },
      drag: false,
      myArray: [
        {
          class: 'shape line',
          icon: 'fa fa-external-link'
        },
        {
          class: 'shape rect',
          icon: 'fa fa-square'
        }
      ],
      picked: []
    }
  },
  methods: {
    onShapeAdd(newVal) {
      console.log(this.picked);
    }
  },
  computed: {
    constantArray: {
      get: function () {
        return this.myArray;
      },
      set: function() {}
    },
    dragOptions() {
      return {
        animation: 2,
        group: "shape",
        disabled: false,
        ghostClass: "ghost"
      };
    },
   },
  mounted() {
    const { stage } = this.$refs;

    var layer = new Konva.Layer();
    stage.getStage().add(layer);

    // darth vader
    var darthVaderImg = new Konva.Rect({
        width: 200,
        height: 137,
				fill: 'rgba(34, 105, 167, 0.68)',
        strokeWidth: 1,
        draggable: true
    });

    var redLine = new Konva.Line({
      points: [5, 70, 140, 23],
      stroke: 'red',
      strokeWidth: 5,
      lineCap: 'round',
      lineJoin: 'round',
      draggable: true,
    });

    layer.add(redLine);
    layer.add(darthVaderImg);

    var tr = new Konva.Transformer();
    var tr1 = new Konva.Transformer();
    layer.add(tr);
    layer.add(tr1);
    tr.attachTo(darthVaderImg);
    tr1.attachTo(redLine);
		layer.draw();
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.drawer-wrap {
  display: flex;
  justify-content: center;
}
.drawer-wrap > div {
  position: relative;
}
.canvas-stage {
  position: absolute;
  z-index: 5000;
}
.line,
.rect {
  position: absolute;
  left: -52px;
  width: 50px;
  display: flex;
  cursor: grab;
  align-items: center;
  justify-content: center;
  font-size: 40px;
  height: 50px;
  background: rgba(139, 139, 139, 0.21);
  border: 1px solid rgba(0, 0, 0, 0.188);
}
.rect {
  top: 0;
}

.line {
  top: 51px;
}
.flip-list-move {
  transition: transform 0.5s;
}

.no-move {
  transition: transform 0s;
}

.ghost {
  opacity: 0.8;
  background: #c8ebfb;
}

.dragged-to {
  position: absolute;
  height: 100%;
  width: 100%;
}

.top-layer {
  z-index: 10000;
}
</style>
