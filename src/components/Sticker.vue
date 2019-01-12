<template>
  <div class="grid-item">
    <img :src="character.image" />
  </div>
</template>

<script>
export default {
  name: 'Sticker',
  props: {
    character: {
      type: Object,
      required: true,
    },
  },
};
</script>

<style lang="scss" scoped>
  .grid-item {
    display: flex;
    align-items: center;
    justify-content: center;
    height: 5rem;

    margin-left: 5px;
    margin-right: 5px;
    flex: 1 1 200px;
  }
</style>
