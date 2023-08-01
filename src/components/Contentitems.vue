<template>
  <section class="content container">
    <div class="content-top">
      <h2>{{words.contentTitle[lang]}}</h2>
      <button class="toggle-btn" @click="grid = !grid">
        <img src="@/assets/images/grid.png" alt="" v-if="!grid">
        <img src="@/assets/images/list.png" alt="" v-else>
        <span>{{!grid ? words.grid[lang] : words.list[lang]}}</span>
      </button>
    </div>
    <div class="content-items" :class="!grid ? 'flex' : 'grid'">
      <Item
        v-for="(item, index) in itemList"
        :key="index"
        :item="item"
        :grid="grid"
        @changeItem="$emit('changeItem', item.id)"
        @delItem="$emit('delItem', item.id)"
        :lang="lang"
      />
    </div>
  </section>
</template>

<script>
import Item from '@/components/Item.vue'
export default {
  components: {Item},
  props: ['itemList', 'lang'],
  inject: ['words'],
  data() {
    return {
      grid: true
    }
  },
}
</script>
