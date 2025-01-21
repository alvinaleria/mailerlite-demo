<template>
  <div class="relative w-full h-[162px] truncate">
    <div v-for="(item, index) in items" :key="index" class="absolute top-0 w-full" draggable="true" @dragstart="this.$emit('newImg', item.data)">
      <div class="absolute bg-cover bg-center w-full h-[162px]" :style="{'background-image': 'url(' + item.data + ')', left: (index*100) + '%'}" :id="`img${index}`"></div>
    </div>
    <svg @click="leftClick" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="currentColor" class="absolute top-[72px] left-[5px] size-5 text-white cursor-pointer">
      <path fillRule="evenodd" d="M11.03 3.97a.75.75 0 0 1 0 1.06l-6.22 6.22H21a.75.75 0 0 1 0 1.5H4.81l6.22 6.22a.75.75 0 1 1-1.06 1.06l-7.5-7.5a.75.75 0 0 1 0-1.06l7.5-7.5a.75.75 0 0 1 1.06 0Z" clipRule="evenodd" />
    </svg>
    <svg @click="rightClick" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="currentColor" class="absolute top-[72px] right-[5px] size-6 text-white cursor-pointer">
      <path fill-rule="evenodd" d="M12.97 3.97a.75.75 0 0 1 1.06 0l7.5 7.5a.75.75 0 0 1 0 1.06l-7.5 7.5a.75.75 0 1 1-1.06-1.06l6.22-6.22H3a.75.75 0 0 1 0-1.5h16.19l-6.22-6.22a.75.75 0 0 1 0-1.06Z" clip-rule="evenodd" />
    </svg>

  </div>  
</template>

<script>
import { ref } from "vue"


export default {
  name: "BaseText",
  setup () {
    const items =  ref([
      { id: 0, data: "masha1.jpg", dragged: 0},
      { id: 1, data: "masha2.jpg", dragged: 0},
      { id: 2, data: "masha3.jpg", dragged: 0},
      { id: 3, data: "masha4.jpg", dragged: 0},
    ])

    const currItem = ref(0);

    //Store data from dragged elements
    const leftClick = () => {
      document.getElementById("img" + currItem.value).style.left = "-100%";

      if(currItem.value >= items.value.length-1) {
        document.getElementById("img0").style.left = "0%"; 
        currItem.value = 0;
      } else {
        document.getElementById("img" + (currItem.value + 1)).style.left = "0%"; 
        currItem.value += 1;
      }  
    }

    const rightClick = () => {
      document.getElementById("img" + currItem.value).style.left = "100%";

      if(currItem.value <= 0) {
        document.getElementById("img" + (items.value.length - 1)).style.left = "0%"; 
        currItem.value = items.value.length - 1;
      } else {
        document.getElementById("img" + (currItem.value - 1)).style.left = "0%"; 
        currItem.value -= 1;
      }  
    } 

    return {
      items,
      leftClick,
      rightClick,
    }  
  },
  
}
</script>