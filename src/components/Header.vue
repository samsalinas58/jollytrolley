<template>
    <div class="header">
      <div class="flex-item"><img class="header-graphic" alt="Header-Graphic" src="../assets/header-graphic.svg"></div>
      <div class="flex-item credits"><p>
        Made with ❤️ by some nerds
        who had too much spare time.<br>
        www.weareasterisk.com<br>
        hello@weareasterisk.com
      </p></div>
    </div>
</template>

<script>

export default {
  name: 'Header',
  props: {
    msg: String
  }
}

</script>

<style scoped>
.header {
    position: relative;
    vertical-align: top;
    margin: 0 5% 0 5%;
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    align-items: center;
}
p {
  margin: 0;
  font-family: Roboto;
  font-style: normal;
  font-weight: 300;
  font-size: 16px;
  line-height: 22px;
  text-align: right;
  right: 0;
  color: #000000;
}
.flex_item {
  flex: 1;
}
.header-graphic {
  transform: matrix(0.75, 0, 0, 0.75, 64, 24);
}
.credits {
  width: 30%;
  max-width: 260px;
}
</style>