<template>
<main>
  <board />
</main>
</template>

<style lang="scss">
@import url('https://fonts.googleapis.com/css?family=Kanit');

body {
  margin: 0;
  font-family: 'Kanit', sans-serif;
}

main {
  position: relative;
  display: flex;
  min-width: 100vw;
  min-height: 100vh;
  background: #5E5E5E;
  justify-content: center;
  align-items: center;
}
</style>

<script>
import board from '@/components/board'
export default {
  name: 'CONTAINER',
  components: {
    board
  }
}
</script>
