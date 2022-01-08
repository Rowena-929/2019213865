<script>
  import Snake from "./Snake.svelte";
  import Food from "./Food.svelte";
  let foodLeft = 0;
  let foodTop = 0;
  let direction = "right";
  let snakeBodies = [];
  let count = 0;
  let key = false;
  let state = "move";
  let target = 10;
  let round = 1;
  let interval = 200;
  let x = 0;

  $: score = (snakeBodies.length - 3)*10;

  setInterval(() => {
    if (state === "move") snakeBodies.pop();

    let { left, top } = snakeBodies[0];
    
    if(key) {
      if (key === "space") {
        count += 1; 
        if (count % 2 === 0) {
          state = "move";
          if (direction === "up") {
            top -= 50;
          } else if (direction === "down") {
            top += 50;
          } else if (direction === "left") {
            left -= 50;
          } else if (direction === "right") {
            left += 50;
          }
        }else {
          state = "stop";
        }
      }
      else if (key != "space") {
        direction = key;
        if (direction === "up") {
          top -= 50;
        } else if (direction === "down") {
          top += 50;
        } else if (direction === "left") {
          left -= 50;
        } else if (direction === "right") {
          left += 50;
        }
      }
      key = false;
    }else {
      if (direction === "up") {
          top -= 50;
      } else if (direction === "down") {
          top += 50;
      } else if (direction === "left") {
          left -= 50;
      } else if (direction === "right") {
          left += 50;
      }
    }


    if(state === "move"){
      const newHead = { left, top };
      snakeBodies = [newHead, ...snakeBodies];
      if (isCollide(newHead, { left: foodLeft, top: foodTop })) {
        moveFood();
        snakeBodies = [...snakeBodies, snakeBodies[snakeBodies.length - 1]];
      }

      if (score>=target) {
          alert("succeed");
          round += 1;
          nextround();
        }

      if (isGameOver()) {
        alert("game over");
        resetGame();
      }
    }
    
  }, interval);

  function isCollide(a, b) {
    return (a.top===b.top && a.left===b.left);
  }

  function moveFood() {
    foodTop = Math.floor(Math.random() * 10) * 50;
    foodLeft = Math.floor(Math.random() * 20) * 50;
    x = Math.floor(Math.random()*7);
  }

  function resetGame() {
    moveFood();
    direction = "right";
    snakeBodies = [
      {
        left: 100,
        top: 0
      },
      {
        left: 50,
        top: 0
      },
      {
        left: 0,
        top: 0
      }
    ];
  }

  function isGameOver() {
    const snakeBodiesNoHead = snakeBodies.slice(1);

    const snakeCollisions = snakeBodiesNoHead.filter(sb =>
      isCollide(sb, snakeBodies[0])
    );

    if (snakeCollisions.length > 0) {
      console.log(snakeCollisions);
      return true;
    }

    const { top, left } = snakeBodies[0];

    if (top >= 500 || top < 0 || left < 0 || left >= 1000) {
      return true;
    }

    return false;
  }

  function getDirectionFromKeyCode(keyCode) {
    if (keyCode === 38) {
      return "up";
    } else if (keyCode === 39) {
      return "right";
    } else if (keyCode === 37) {
      return "left";
    } else if (keyCode === 40) {
      return "down";
    } else if(keyCode === 32) {
      return "space";
    }

    return false;
  }

  function onKeyDown(e) {
    const newDirection = getDirectionFromKeyCode(e.keyCode);
    if (newDirection) {
      key = newDirection;
    }
    else key = false;
  }

  function nextround(){
    target *= 2;
    resetGame();
  }

   resetGame();
</script>

<style>
  * {
    margin: 0;
    padding: 0;
  }
  .whole {
    display: grid;
    grid-template-columns: 20% 80%;
  }
  .left {
    background-color: rgb(9, 73, 110, 0.5);
  }
  .right {
    width: 1003px;
    height: 500px;
    border: solid black 1px;
    position: relative;
    background-color: rgb(40, 40, 224, 0.5);
  }
  .name {
    font-family: Yellowtail;
    font-weight: bold;
    font-style: italic;
    text-align: center;
    color: yellow;
    margin-top: 70px;
    font-size: 35px;
    text-shadow: 0 0 0.5em #0ae642, 0 0 0.2em #5c5c5c;
  }
  .font {
    margin-top: 50px;
    text-align: center;
    font-size: 25px;
  }
  .score {
    text-align: center;
    font-size: 25px;
    margin-top: 30px;
  }
  .target {
    text-align: center;
    font-size: 25px;
    margin-top: 30px;
  }
  .background {
    margin: 0;
    padding: 0;
    height: 100%;
    width: 100%;
    background-image: url(bcc.jfif);
    background-size: 1143.2px 500px;
  }
</style>

<div class="background">
  <div class="whole">
    <div class="left">
      <div class="name">Snake game</div>
      <div class="font">round {round}</div>
      <div class="score">score:{score}</div>
      <div class="target">target:{target}</div>
    </div>
    <div class="right">
    <Snake {direction} {snakeBodies} />
    <Food {x} {foodLeft} {foodTop}/>
    </div>
  </div>
</div>
<svelte:window on:keydown={onKeyDown} />
