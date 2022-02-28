<template>
  <q-page class="">
    <ui5-list id="infiniteScrollEx" ref="listRef" style="height: calc( 100vh - 150px )" growing="Scroll" v-on:load-more="handleLoadMore" v-on:item-click="scrollTo">
      <ui5-li
        v-for="(index, todo) in todos" :key="index" :datakey="index">
          {{todo}}
      </ui5-li>
      <ui5-li>
        Loading
      </ui5-li>
    </ui5-list>
</q-page>
</template>

<script lang="ts">

import { defineComponent, ref } from 'vue';
import { scroll } from 'quasar'
const { getScrollTarget, setVerticalScrollPosition } = scroll
import '@ui5/webcomponents/dist/List.js'
import '@ui5/webcomponents/dist/StandardListItem.js'

export default defineComponent({
  name: 'PageIndex',
  components: { },
  setup() {
    const todos = ref<string[]>([]);
    for (let i = 0; i <= 30; i++) {
      todos.value.push('Buy milk')
    }
    const handleLoadMore = () => {
      const newItemsEndIndex = todos.value.length + 50
      setTimeout(() => {
        for (let i = todos.value.length; i <= newItemsEndIndex; i++) {
          todos.value.push('Buy more milk')
        }
      }, 1)
    }
    const listRef = ref<InstanceType<typeof Element> | null>(null)
    const scrollTo = () => {
      if (listRef.value) {
        const scrollTarget = getScrollTarget(listRef.value)
        setVerticalScrollPosition(scrollTarget, 100, 0)
      }
    }

    return {
      todos,
      handleLoadMore,
      listRef,
      scrollTo
    }
  },
});
</script>
