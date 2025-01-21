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
    <div v-for="(item, index) in landing" :key="index" class="relative mb-2.5 last:mb-0" draggable="true" @dragstart="startDrag($event, item)" @drop.stop="onInDrop($event, index)" @dragenter.prevent @dragover.prevent>
      <svg @click="deleteBlock(index)" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="currentColor" class="absolute top-[5px] right-[5px] size-5 text-white drop-shadow-xl cursor-pointer">
        <path fill-rule="evenodd" d="M16.5 4.478v.227a48.816 48.816 0 0 1 3.878.512.75.75 0 1 1-.256 1.478l-.209-.035-1.005 13.07a3 3 0 0 1-2.991 2.77H8.084a3 3 0 0 1-2.991-2.77L4.087 6.66l-.209.035a.75.75 0 0 1-.256-1.478A48.567 48.567 0 0 1 7.5 4.705v-.227c0-1.564 1.213-2.9 2.816-2.951a52.662 52.662 0 0 1 3.369 0c1.603.051 2.815 1.387 2.815 2.951Zm-6.136-1.452a51.196 51.196 0 0 1 3.273 0C14.39 3.05 15 3.684 15 4.478v.113a49.488 49.488 0 0 0-6 0v-.113c0-.794.609-1.428 1.364-1.452Zm-.355 5.945a.75.75 0 1 0-1.5.058l.347 9a.75.75 0 1 0 1.499-.058l-.346-9Zm5.48.058a.75.75 0 1 0-1.498-.058l-.347 9a.75.75 0 0 0 1.5.058l.345-9Z" clip-rule="evenodd" />
      </svg>
      
      <svg @click="duplicateBlock(index)" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="currentColor" class="absolute top-[5px] right-[32px] size-5 text-white drop-shadow-xl cursor-pointer">
        <path d="M7.5 3.375c0-1.036.84-1.875 1.875-1.875h.375a3.75 3.75 0 0 1 3.75 3.75v1.875C13.5 8.161 14.34 9 15.375 9h1.875A3.75 3.75 0 0 1 21 12.75v3.375C21 17.16 20.16 18 19.125 18h-9.75A1.875 1.875 0 0 1 7.5 16.125V3.375Z" />
        <path d="M15 5.25a5.23 5.23 0 0 0-1.279-3.434 9.768 9.768 0 0 1 6.963 6.963A5.23 5.23 0 0 0 17.25 7.5h-1.875A.375.375 0 0 1 15 7.125V5.25ZM4.875 6H6v10.125A3.375 3.375 0 0 0 9.375 19.5H16.5v1.125c0 1.035-.84 1.875-1.875 1.875h-9.75A1.875 1.875 0 0 1 3 20.625V7.875C3 6.839 3.84 6 4.875 6Z" />
      </svg>
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
      const itemType = event.dataTransfer.getData("itemType");
      const itemData = event.dataTransfer.getData("itemData");
      const itemDragged = Number(event.dataTransfer.getData("itemDragged"));
      let tempArr = [...landing.value];
      
      if(itemDragged === 1) {
        tempArr.splice(itemID, 1);
        tempArr.splice(index, 0, landing.value[itemID]);
        landing.value = tempArr;
      } else {
        //generate new block for landpage
        const item = {id: (landing.value.length+1), type: ((Number(itemType) === 0) ? "text" : "image"), data: itemData, dragged: 1};
        
        //Insert new block before existing one selected
        landing.value.splice(index, 0, item);
      }

      //Resort IDs based on key
      for (const [key, item] of Object.entries(landing.value))
          item.id = key;
    }

    const deleteBlock = (index) => {
      landing.value.splice(index, 1);

      //Resort IDs based on key
      for (const [key, item] of Object.entries(landing.value))
          item.id = key;
    }

    const duplicateBlock = (index) => {
      landing.value.splice((index + 1), 0, landing.value[index]);

      //Resort IDs based on key
      for (const [key, item] of Object.entries(landing.value))
          item.id = key;
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
      deleteBlock,
      duplicateBlock,
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
