<template>
  <div class="drop-zone">
    <div v-for="item of items" :key="item.id" class="drag-el" draggable="true" @dragstart="startDrag($event, item)">
      {{ item.title }}
    </div>
  </div>

  <div class="drop-zone" @drop="onDrop($event)" @dragenter.prevent @dragover.prevent>
    <div v-for="(item, index) in landing" :key="index" class="drag-el" draggable="true" @dragstart="startDrag($event, item)">
      {{ item.title }}
    </div>
  </div>
</template>

<script>
import { ref } from "vue"

export default {
  setup () {
    const items =  ref([
      { id: 0, title: "TEXT BOX", dragged: 0},
      { id: 1, title: "IMAGE 1", dragged: 0},
      { id: 2, title: "IMAGE 2", dragged: 0},
      { id: 3, title: "IMAGE 3", dragged: 0},
      { id: 4, title: "IMAGE 4", dragged: 0},
    ])

    const landing =  ref([])

    const getList = (list) => {
      return items.value.filter((item) => item.list == list)
    }

    const getLanding = (list) => {
      return landing.value.filter((item) => item.list == list)
    }

    const startDrag = (event, item) => {
      event.dataTransfer.dropEffect = "move";
      event.dataTransfer.effectAllowed = "move";
      event.dataTransfer.setData("itemID", item.id)
      event.dataTransfer.setData("itemDragged", item.dragged)
    }

    const onDrop = (event) => { 
      const itemID = event.dataTransfer.getData("itemID");
      const itemDragged = Number(event.dataTransfer.getData("itemDragged"));
      const item = items.value.find((item) => item.id == itemID);
      
      if(itemDragged === 0) {
        landing.value.push(Object.assign({}, item));
        landing.value[landing.value.length -1].dragged = 1;
      }
    }

    return {
      items,
      landing,
      getList,
      getLanding,
      startDrag,
      onDrop,
    }
  }
}

</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.drop-zone {
  width: 50%;
  margin: 50px auto;
  background-color: #ecf0f1;
  padding: 10px;
  min-height: 10px;
}

.drag-el {
  background-color: #3498db;
  color: #fff;
  padding: 5px;
  margin-bottom: 10px;
}

.drag-el:nth-last-of-type(1) {
  margin-bottom: 0;
}
</style>
