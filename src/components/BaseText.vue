<template>
  <textarea class="resize-none w-full h-[162px] p-5 text-white border-slate-50 bg-slate-500" value="Click here to change copy" @focus="handleFocusIn($event)" @focusout="handleFocusOut($event)" @change="updateParentCopy($event.target.value)"></textarea>
</template>
<script>
export default {
  name: "BaseText",
  methods: {
    updateParentCopy(value) {
      this.$emit("newCopy", value);
    },

    handleFocusIn(event) {
      if(event.target.value == "Click here to change copy")
        event.target.value = "";
    },

    handleFocusOut(event) {
      if(event.target.value == "")
        event.target.value = "Click here to change copy";
    }
  },
}
</script>