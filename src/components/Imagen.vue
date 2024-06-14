<template>
  <div class="cuadro">
    <img :src="url" alt="Cannot load image!" />
    <label>{{ text }}</label>
  </div>
</template>

<script>
export default {
  props: {
    url: {
      type: String,
      required: true,
    },
    text: {
      type: String,
      default: "XXXXXXX",
    },
  },
};
</script>

<style scoped>
.cuadro {
  display: flex;
  flex-direction: column;
}
img {
  width: 250px;
  height: 250px;
}
label{
    font-weight:bold ;
    margin: 15px 0px;
}
</style>
