<template>
  <div class="flex lg:flex-row max-md:flex-col flex-nowrap justify-center mx-auto my-6 w-7/12 max-lg:w-9/12 bg-[#ecf0f1] ">
    <div class="grow-4 shrink-4 basis-6/12 p-2.5 pr-1.5 pb-1 max-md:p-2 max-md:pb-0.5 bg-[#ecf0f1]" @dragstart="startDragText($event)">
      <BaseText draggable="true" />
    </div>
    <div class="grow-4 shrink-4 basis-6/12 p-2.5 pl-1.5 pb-1 max-md:p-2 max-md:pt-0.5 bg-[#ecf0f1]" >
      <BaseImage @newImg="startDragImg" />
    </div>
  </div>
  <div class="border-2 border-dashed border-gray-500 mx-auto my-6 p-5 w-7/12 max-lg:w-9/12 h-full min-h-60 bg-[#ecf0f1]" @drop="onOutDrop($event)" @dragenter.prevent @dragover.prevent>
    <div v-for="(item, index) in landing" :key="index" class="mb-2.5 last:mb-0" draggable="true" @dragstart="startDrag($event, item)" @drop="onInDrop($event, index)" @dragenter.prevent @dragover.prevent>
      <p v-if="item.type === 'text'">{{ item.data }}</p>
      <img v-else :src="item.data" width="100%" /> 
    </div>
  </div>
  
</template>

<script>
import BaseText from "@/components/BaseText.vue";
import BaseImage from "@/components/BaseImage.vue";

import { ref } from "vue"

export default {
  components: {
    BaseText,
    BaseImage,
  },
  setup () {
    const items =  ref([
      { id: 0, data: "IMAGE 1", dragged: 0},
      { id: 1, data: "IMAGE 2", dragged: 0},
      { id: 2, data: "IMAGE 3", dragged: 0},
      { id: 3, data: "IMAGE 4", dragged: 0},
    ])

    const landing =  ref([])
    let url = ref("");

    //Store data from dragged elements
    const startDragImg = (url) => {
      event.dataTransfer.dropEffect = "move";
      event.dataTransfer.effectAllowed = "move";
      
      event.dataTransfer.setData("itemID", "");
      event.dataTransfer.setData("itemType", 1
      );
      event.dataTransfer.setData("itemData", url);
      event.dataTransfer.setData("itemDragged", 0);
    }

     //Store data from dragged elements
     const startDragText = (event) => {
      event.dataTransfer.dropEffect = "move";
      event.dataTransfer.effectAllowed = "move";

      event.dataTransfer.setData("itemID", "undefined");
      event.dataTransfer.setData("itemType", "0");
      event.dataTransfer.setData("itemData", event.target.value);
      event.dataTransfer.setData("itemDragged", 0);
    }

    const startDrag = (event, item) => {
      event.dataTransfer.dropEffect = "move";
      event.dataTransfer.effectAllowed = "move";
      event.dataTransfer.setData("itemID", item.id)
      event.dataTransfer.setData("itemDragged", item.dragged)
    }

    //Drop base elements to landing page
    const onOutDrop = (event) => { 
      const itemType = event.dataTransfer.getData("itemType");
      const itemData = event.dataTransfer.getData("itemData");
      const itemDragged = event.dataTransfer.getData("itemDragged");

      //generate new block for landpage
      const item = {id: (landing.value.length+1), type: ((Number(itemType) === 0) ? "text" : "image"), data: itemData, dragged: 1};
      
      if(Number(itemDragged) === 0 && itemData !== "Click here to change copy") {
        landing.value.push(Object.assign({}, item));
        landing.value[landing.value.length -1].id = landing.value.length - 1;
      }
    }

    //Drop existing landing page elements for sorting
    const onInDrop = (event, index) => { 
      const itemID = event.dataTransfer.getData("itemID");
      const itemDragged = Number(event.dataTransfer.getData("itemDragged"));
      let tempArr = [...landing.value];
      
      if(itemDragged === 1) {
        tempArr.splice(itemID, 1);
        tempArr.splice(index, 0, landing.value[itemID]);
        landing.value = tempArr;
        
        //Resort IDs based on key
        for (const [key, item] of Object.entries(landing.value))
          item.id = key;
      }
    }

    return {
      items,
      landing,
      url,
      startDragImg,
      startDragText,
      startDrag,
      onOutDrop,
      onInDrop,
    }
  }
}

</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: left;
  color: #2c3e50;
  margin-top: 60px;
}

.drag-el:nth-last-of-type(1) {
  margin-bottom: 0;
}
</style>
