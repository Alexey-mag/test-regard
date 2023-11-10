<template>
  <td>
    <slot name="cell" :item="item" :index="index" />
  </td>
</template>

<script>
export default {
  name: 'AppCell',

  data() {
    return {
      item: 'Cell text',
      index: 1,
    }
  },
}
</script>
