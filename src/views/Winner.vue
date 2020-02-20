<template>
  <div>
    <div class="writing">
      <h1>You Won!</h1>
    </div>
    <div class="box">
      <div class="circle">
        <div class="behindCircle"></div>  
        <div class="circleCopy">
          <div id="heart" class="leftLeaf"></div>
          <div id="heart" class="topLeaf"></div>
          <div id="heart" class="rightLeaf"></div>
          <div id="heart" class="bottomLeaf"></div>
          <div id="line" class="left"></div>
          <div id="line" class="top"></div>
          <div id="line" class="right"></div>
          <div id="line" class="bottom"></div>
          <div class="stem"></div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {}
</script>

<style scoped lang="scss">
  $DarkGreen: #113D19;
  $SatGreen: #249138;
  $Shamrock: #34C149;
  $Stem: #23CE6B;
  $Behind: #A2FF93;
  $Shadow: #4D564E;

  div {
    position: absolute;
  }

  body {
    background-color: $Behind;
  }

  h1 {
    position: relative;
    font-family: 'Chewy', cursive;
    font-size: 60px;
    margin: 50px 0px 0px 620px;
    color: #d67f2f;
  }

  .box {
    top: 25%;
    left: 37%;
    width: 450px;
    height: 600px;
    background: none;
  }

  .circle {
    top: 25%;
    left: 35%;
    width: 25%;
    height: 20%;
    background: $Shamrock;
    border-radius: 50%;
    animation: float 7s infinite;
  }

  .circleCopy {
    width: 100%;
    height:100%;
    background: $Shamrock;
    border-radius: 50%;
    z-index: 3;
  }

  .behindCircle {
    top: -10%;
    right: -10%;
    width: 120%;
    height: 120%;
    background: $DarkGreen;
    border-radius: 50%;
    z-index: 1;
  }

  #heart {
      width: 150%;
      height: 150%;
  }
  #heart:before,
  #heart:after {
      position: absolute;
      content: "";
      left: 83px;
      top: 0%;
      width: 50%;
      height: 80%;
      background: $SatGreen;
      -moz-border-radius: 50px 50px 0 0;
      border-radius: 50px 50px 0 0;
      -webkit-transform: rotate(-45deg);
        -moz-transform: rotate(-45deg);
          -ms-transform: rotate(-45deg);
          -o-transform: rotate(-45deg);
              transform: rotate(-45deg);
      -webkit-transform-origin: 0 100%;
        -moz-transform-origin: 0 100%;
          -ms-transform-origin: 0 100%;
          -o-transform-origin: 0 100%;
              transform-origin: 0 100%;
  }
  #heart:after {
      left: 0;
      -webkit-transform: rotate(45deg);
        -moz-transform: rotate(45deg);
          -ms-transform: rotate(45deg);
          -o-transform: rotate(45deg);
              transform: rotate(45deg);
      -webkit-transform-origin: 100% 100%;
        -moz-transform-origin: 100% 100%;
          -ms-transform-origin: 100% 100%;
          -o-transform-origin: 100% 100%;
              transform-origin :100% 100%;
  }

  #heart.leftLeaf{
    top: -25%;
    left: -100px;
    transform: rotate(-90deg);
  }

  #heart.topLeaf {
    top: -85px;
    left: -27px;
  }

  #heart.rightLeaf {
    top: -25%;
    right: -100px;
    transform: rotate(90deg);
  }

  #heart.bottomLeaf {
    bottom: -85px;
    left: -27px;
    transform: rotate(180deg);
    z-index: 3;
  }

  #line {
    top: 50%;
    left: 10%;
    width: 5px;
    height: 50%;
    background: $Stem;
    z-index: 4;
  }

  #line.left {
    top: 25%;
    left: -55px;
    transform: rotate(-90deg);
  }

  #line.top {
    top: -61px;
    left: 55px;
  }

  #line.right {
    top: 25%;
    left: 162px;
    transform: rotate(90deg);
  }

  #line.bottom {
    top: 122px;
    left: 56px;
  }

  .stem {
    bottom: -300px;
    right: 30px;
    width: 200%;
    height: 250%;
    background: transparent;
    border: 30px solid $Stem;
    border-color: transparent $Stem transparent transparent;
    border-radius: 40%;
    z-index: 0;
  }

  @keyframes hover {
    0% {
      bottom: -45%;
      height: 50%;
    }
    50% {
      bottom: -45%;
      height: 60%;
    }
    
    100% {
      bottom: -45%;
      height: 50%;    
    }
  }

  @keyframes float {
    0% {
      top: 25%;
    }
    
    50% {
      top: 35%;
    }
    
    100% {
      top: 25%;
    }
  }
</style>
