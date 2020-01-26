<template>
    <div class="container">
        <div class="descriptor">
            <p>
                Founded in 1983 and beginning operations in 1989, the M-Line Trolley provides safe, clean, reliable, and convenient public transportation <b>free of charge</b> in Dallas’ vibrant Uptown and Downtown Arts District Neighborhoods.
            </p>
        </div>
        <img class="skyline" alt="Skyline" src="../assets/skyline.svg">
        <!-- <img class="marker-and-city" alt="Marker-and-city" src="../assets/marker-and-city.svg"> -->
        <img class="trolley-path" alt="Trolley-Path" src="../assets/trolley-path.svg">
        <img class="trolley-graphic" alt="Trolley" src="../assets/filled_traincar.svg">
    </div>
</template>

<script>

export default {
  name: 'Footer',
  props: {
    msg: String
  }
}

</script>

<style scoped>
.container {
  text-align: left;
}
.trolley-path {
  position: fixed;
  bottom:0;
  z-index: 1;
}
.skyline {
  transform: matrix(0.75, 0, 0, 0.75, 120, 50);
  position: fixed;
  bottom:0;
  right: 0;
  z-index: 4;
}
@keyframes drive {
  from {left: -200px;}
  to {left: 1400px;}
}
.trolley-graphic {
  transform: scale(0.75,0.75);
  position: fixed;
  z-index: 2;
  bottom: -16px;
  animation: drive 10s infinite;
  animation-timing-function: linear;
}
p {
  font-family: Roboto;
  font-style: normal;
  font-weight: 30;
  font-size: 18px;
  line-height: 24px;
  text-align: left;
  color: #000000;
}
.descriptor {
  margin: 36px 5% 0 5%;
  width: 50%;
}
/* .traincar {
  position: fixed;
  transform: matrix(0.5, 0, 0, 0.5, 0, 0);
  left: 0;
  bottom: 0;
}
.beacon {
  position: fixed;
  transform: matrix(0.5, 0, 0, 0.5, 0, 0);
  left: 40%;
  bottom: 0;
}
.skyline {
  position: fixed;
  right:0px;
  bottom:0px;
  transform: matrix(-0.5, 0, 0, 0.5, 0, 0);
} */
</style>