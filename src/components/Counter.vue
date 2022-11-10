<script>
export default {
  data: () => ({ count: 6 })
}
</script>

<template>
  <button @click="count++">
    Count {{ count }}
  </button>
</template>