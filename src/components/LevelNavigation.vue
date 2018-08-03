<template>
  <nav class="Level">
    <button type="button"
           v-for="(i, index) in items"
            @click="levelChange(index+1)"
            :class="{'is-active': level === (index + 1)}"
           class="Level-button"
           :style="{
             backgroundColor: data[i].color
           }"
           :title="data[i].alias"
    ></button>
  </nav>
</template>

<script>
export default {
  name: "LevelNavigation",
  props: {
    items: Array,
    data: Object,
    level: Number,
    foo: String
  },
  data() {
    return {
    };
  },
  methods: {
    levelChange(requestedLevel) {
      if (this.level === requestedLevel){
        return;
      }
      this.$emit("levelChange", requestedLevel);
    }
  }
};
</script>

<style lang="scss" scoped>
.Level {
  // display: flex;
  &-button {
    border-radius: 100%;
    display: block;
    width: 46px;
    height: 46px;
    overflow: hidden;
    text-indent: 200%;
    white-space: nowrap;

    &.is-active {
      border: 4px solid red;
    }

  }
}
</style>
