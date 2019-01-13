<template>
<main>
  <board />
</main>
</template>

<style lang="scss">
@import url('https://fonts.googleapis.com/css?family=Rubik:400,700');

body {
  margin: 0;
  font-family: 'Rubik', sans-serif;
  overflow: none;
}

main {
  position: relative;
  display: flex;
  min-width: 100vw;
  min-height: 100vh;
  background: #5E5E5E;
  justify-content: center;
  align-items: center;
  overflow: auto;
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
