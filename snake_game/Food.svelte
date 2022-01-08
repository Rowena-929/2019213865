<script>
  export let x;
  export let foodTop;
  export let foodLeft;
</script>

<style>
  .food0 {
    width: 50px;
    height: 50px;
    position: absolute;
    background-image: url(planet0.png);
    background-size: 50px 50px; 
    z-index: 0;
    /* border: solid black 1px; 
    background-color: red;
    border-radius: 50%; */
  }
  .food1 {
    width: 50px;
    height: 50px;
    position: absolute;
    background-image: url(planet1.png);
    background-size: 50px 50px;
    z-index: 0;
  }
  .food2 {
    width: 50px;
    height: 50px;
    position: absolute;
    background-image: url(planet2.png);
    background-size: 50px 50px;
    z-index: 0;
  }
  .food3 {
    width: 50px;
    height: 50px;
    position: absolute;
    background-image: url(planet3.png);
    background-size: 50px 50px;
    z-index: 0;
  }
  .food4 {
    width: 50px;
    height: 50px;
    position: absolute;
    background-image: url(planet4.png);
    background-size: 50px 50px;
    z-index: 0;
  }
  .food5 {
    width: 50px;
    height: 50px;
    position: absolute;
    background-image: url(planet5.png);
    background-size: 50px 50px;
    z-index: 0;
  }
  .food6 {
    width: 50px;
    height: 50px;
    position: absolute;
    background-image: url(planet6.png);
    background-size: 50px 50px;
    z-index: 0;
  }
</style>

<div class="food{x}" style="left: {foodLeft}px; top: {foodTop}px" />
