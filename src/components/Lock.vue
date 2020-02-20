<template>
  <div>
    <div onclick="void(0);" style="height: 100%; -webkit-tap-highlight-color: transparent;">
      <div style="height: 100%; -webkit-tap-highlight-color: initial;">
        <div class="device">
          <div class="device-bezel">
            <div class="device-viewport">
              <!-- Begin custom content -->
              <div class="content">
                <img src="../assets/irish.jpg" />
                <div class="body-content">
                  <h1>Congratulations!</h1>
                  <h3>You are the winner!</h3>
                  <p>Good luck with all your lotto tickets!</p>
                  <button id="reset-button">Reset Lock!</button>
                </div>
              </div>
              <div class="container">
                <div class="pin-info">
                  <h1 class="page-title">
                    Enter Your<br/>
                    <img src="../assets/logo.png" height="24" width="24" /> Lucky Lottery <img src="../assets/logo.png" height="24" width="24" /> <br/>
                    Code
                  </h1>
                </div>
                <div class="pin-display">
                  <div class="circle-lock--container">
                    <div class="circle-lock">
                      <i class="material-icons lock-icon">lock</i>
                    </div>
                  </div>
                  <div class="confirmation-dots">
                    <svg>
                      <g>
                        <circle class="pin-circle" cx="10" cy="10" r="8"></circle>
                        <circle class="pin-circle" cx="50" cy="10" r="8"></circle>
                        <circle class="pin-circle" cx="90" cy="10" r="8"></circle>
                        <circle class="pin-circle" cx="130" cy="10" r="8"></circle>
                      </g>
                    </svg>
                  </div>
                  <div class="pin">
                    {{ displayPin }}
                  </div>
                  <div class="keypad">
                    <div class="keypad--row">
                      <div class="keypad--button" data-value="1">1</div>
                      <div class="keypad--button" data-value="2">2</div>
                      <div class="keypad--button" data-value="3">3</div>
                    </div>
                    <div class="keypad--row">
                      <div class="keypad--button" data-value="4">4</div>
                      <div class="keypad--button" data-value="5">5</div>
                      <div class="keypad--button" data-value="6">6</div>
                    </div>
                    <div class="keypad--row">
                      <div class="keypad--button" data-value="7">7</div>
                      <div class="keypad--button" data-value="8">8</div>
                      <div class="keypad--button" data-value="9">9</div>
                    </div>
                    <div class="keypad--row">
                      <div class="keypad--button keyboard--button__back-arrow"><i class="material-icons">arrow_back</i></div>
                      <div class="keypad--button" data-value="0">0</div>
                      <div class="keypad--button keyboard--button__x">x</div>
                    </div>
                  </div>
                </div>
              </div>
              <!-- end custom content -->
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {},
  data: () => ({
    myPinArray: []
  }),
  mounted: function () {
    let touchEvent = 'ontouchstart' in window ? 'touchstart' : 'click';
    
    const self = this;
    const correctPin = "0000";
    let disableInput = false;
    let _pinArray = [];
    self.myPinArray = _pinArray;

    function reset() {
      closeLock().then(() => {
        disableInput = false;
        _pinArray = [];
        self.myPinArray = _pinArray;
        bindPinToDisplay(_pinArray);
      });
    }

    function closeLock() {
      const topSection = document.querySelector(".pin-info");
      const bottomSection = document.querySelector(".pin-display");

      const promises = [
        anime({
          targets: bottomSection,
          translateY: "0%",
          duration: 600,
          easing: "easeOutCubic"
        }).finished,
        anime({
          targets: topSection,
          translateY: "0%",
          duration: 600,
          easing: "easeOutCubic"
        }).finished
      ];

      return Promise.all(promises);
    }
    function openLock() {
      const topSection = document.querySelector(".pin-info");
      const bottomSection = document.querySelector(".pin-display");

      const promises = [
        anime({
          targets: bottomSection,
          translateY: "125%",
          duration: 600,
          easing: "easeInCubic"
        }).finished,
        anime({
          targets: topSection,
          translateY: "-101%",
          duration: 600,
          easing: "easeInCubic"
        }).finished
      ];

      return Promise.all(promises);
    }

    function errorShake() {
      return Promise.resolve();
    }
    function bindPinToDisplay(pinArray, pinStatus) {
      document.querySelectorAll(".pin-circle").forEach((el, index) => {
        if (pinStatus === "success") {
          el.classList.add("success");
        } else if (pinStatus === "error") {
          el.classList.add("error");
        } else if (index > pinArray.length - 1) {
          el.classList.remove("entered");
          el.classList.remove("success");
          el.classList.remove("error");
        } else {
          el.classList.add("entered");
        }
      });

      if (pinStatus === "error") {
        document.querySelector(".confirmation-dots").classList.add("error");
      } else {
        document.querySelector(".confirmation-dots").classList.remove("error");
      }
    }

    function evaluatePin(pinArray) {
      const enteredPin = pinArray.join("");
      if (enteredPin === correctPin) {
        disableInput = true;
        setTimeout(() => {
          bindPinToDisplay(pinArray, "success");
          setTimeout(() => {
            openLock();
          }, 500);
        }, 250);
        console.log("correct PIN");
      } else {
        disableInput = true;
        setTimeout(() => {
          bindPinToDisplay(pinArray, "error");
          setTimeout(() => {
            _pinArray = [];
            self.myPinArray = _pinArray;
            bindPinToDisplay(_pinArray);
            disableInput = false;
          }, 350);
        }, 250);
      }
    }

    function initKeypad() {
      document.querySelectorAll(".keypad--button[data-value]").forEach(el => {
        el.addEventListener(touchEvent, evt => {
          if (disableInput) {
            return;
          }
          const value = evt.target.attributes["data-value"].value;
          if (_pinArray.length < 4) {
            _pinArray.push(value);
            bindPinToDisplay(_pinArray);
            if (_pinArray.length === 4) {
              evaluatePin(_pinArray);
            }
          }
        });
      });

      document
        .querySelector(".keyboard--button__back-arrow")
        .addEventListener(touchEvent, () => {
          if (disableInput) {
            return;
          }
          _pinArray.pop();
          bindPinToDisplay(_pinArray);
        });

      document
        .querySelector(".keyboard--button__x")
        .addEventListener(touchEvent, () => {
          if (disableInput) {
            return;
          }
          _pinArray = [];
          self.myPinArray = _pinArray;
          bindPinToDisplay(_pinArray);
        });
      
      document.querySelector('#reset-button').addEventListener(touchEvent, () => {
        reset();
      })
    }

    function initLayout() {
      const containerHeight = document.querySelector(".container").offsetHeight;
      const keypadHeight = document.querySelector(".pin-display").offsetHeight;
      document.querySelector(".pin-info").style.height = `${containerHeight -
        keypadHeight +
        1}px`;
    }

    function init() {
      initKeypad();
      initLayout();
    }

    init();
  },
  computed: {
    displayPin() {
      return (this.myPinArray.length) ? this.myPinArray.join(' ') : '- - - -';
    }
  },
  methods: {
  }
}
</script>

<style lang="scss">
.pin {
  text-align: center;
  margin-bottom: 20px;
}
$body-background: #A2FF93;

// DEVICE OPTIONS
// 18:9 aspect ratio;
$aspect-width: 9;
$aspect-height: 16;
$device-border-radius: 16px;
$viewport-width: 350px;
$viewport-height: ($viewport-width * $aspect-height) / $aspect-width;
$device-scale: 1;
$bezel-top: 36px;
$bezel-bottom: 24px;
$bezel-side: 10px;
$bezel-color: linear-gradient(
  240deg,
  #8da4c3 0%,
  #4f8285 30%,
  #070f0a 35%,
  #0a0606 100%
);
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body {
  display: flex;
  flex-direction: row;
  justify-content: center;
  align-items: center;
  width: 100vw;
  height: 100vh;
  background: $body-background;
}

.device {
  position: relative;
  border-radius: $device-border-radius;
  margin: 0 auto;
  display: inline-block;
  transform: scale($device-scale);

  .device-bezel {
    border-radius: inherit;
    padding: $bezel-top $bezel-side $bezel-bottom $bezel-side;
    overflow: hidden;
    background: $bezel-color;
    position: relative;
    box-sizing: content-box;
  }
  .device-viewport {
    contain: strict;
    width: $viewport-width;
    height: $viewport-height;
    background: white;
    border-radius: inherit;
    transform: scale(1);
  }
}

// ADD YOUR SCSS HERE

//.device-viewport will modify the screen container
// you can adjust the variables above for a different device look and feel.

//
* {
  font-family: "Montserrat", sans-serif;
  user-select: none;
}

.container {
  background: transparent;
  height: 100%;
}

.content {
  background: white;
  img {
    height: 200px;
    width: 100%;
    object-fit: cover;
    object-position: center;
  }
}

.body-content {
  padding: 16px;
  h3 {
    margin-bottom: 16px;
  }
  p {
    line-height: 1.5em;
    margin-bottom: 8px;
  }
  button {
    width: 100%;
    height: 48px;
    border-radius: 1000px;
    background: green;
    border: none;
    color: white;
    outline: none;
    margin-top: 24px;
    cursor: pointer;
  }
}

.pin-info {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  background: white;
  background: #EAEAEA;
}
.pin-display {
  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
  background: white;
}
.keypad {
  padding: 16px;

  background: white;
}
.keypad--row {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  align-items: center;
}
.keypad--button {
  font-family: "Montserrat", sans-serif;
  height: 48px;
  width: 100%;
  display: flex;
  flex-direction: row;
  justify-content: center;
  align-items: center;
  font-size: 24px;
  font-weight: 600;
  cursor: pointer;
  transition: all 100ms ease-out;
  &:active {
    background: #EAEAEA;
  }
}

.confirmation-dots {
  margin-bottom: 28px;
  display: flex;
  flex-direction: row;
  justify-content: center;
  align-items: center;

  &.error {
    animation-name: shake;
    animation-duration: 300ms;
  }

  svg {
    height: 24px;
    max-height: 24px;
    width: 139px;
  }

  .pin-circle {
    transition: all 100ms ease-out;
    fill: white;
    stroke-width: 2px;
    stroke: black;
    outline-offset: -2px;
    outline-width: 2px;

    &.entered {
      fill: black;
    }

    &.success {
      fill: green;
      stroke: green;
    }

    &.error {
      fill: red;
      stroke: red;
    }
  }
}

.circle-lock--container {
  height: 72px;
  position: relative;
  display: flex;
  justify-content: center;
  flex-direction: row;
}

.circle-lock {
  width: 80px;
  height: 80px;
  background: white;
  border-radius: 1000px;
  position: relative;
  top: -40px;
  box-shadow: 0px 1px 1px 1px rgba(0, 0, 0, 0.12);

  display: flex;
  flex-direction: row;
  justify-content: center;
  align-items: center;

  .lock-icon {
    color: #333;
  }
}

.page-title {
  font-size: 28px;
  line-height: 32px;
  text-align: center;
  font-weight: 700;
  padding: 20px 0;
}

.pin-info {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

@keyframes shake {
  10%,
  90% {
    transform: translate3d(-1px, 0, 0);
  }

  20%,
  80% {
    transform: translate3d(2px, 0, 0);
  }

  30%,
  50%,
  70% {
    transform: translate3d(-4px, 0, 0);
  }

  40%,
  60% {
    transform: translate3d(4px, 0, 0);
  }
}

</style>
