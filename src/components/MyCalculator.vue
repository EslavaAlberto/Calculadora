<template>
    <div class="container" :class="currentTheme">
      <div class="calculator">
        <header class="header">
          <div class="header__logo">Calc</div>
  
          <!-- Toggle -->
          <div class="theme__switcher">
            <div class="theme__switcher-label">
              <q-radio v-model="currentTheme" val="theme_1" label="1" color="white" @click="switchTheme('theme_1')" />
              <q-radio v-model="currentTheme" val="theme_2" label="2" color="white" @click="switchTheme('theme_2')" />
              <q-radio v-model="currentTheme" val="theme_3" label="3" color="white" @click="switchTheme('theme_3')" />
            </div>
  
            <span class="theme__switcher-title">Theme</span>
  
            <div class="theme__switcher-toggle">
              <input
                type="radio"
                id="theme-1"
                :class="{ active: currentTheme === 'theme_1' }"
                @click="switchTheme('theme_1')"
              />
              <input
                type="radio"
                id="theme-2"
                :class="{ active: currentTheme === 'theme_2' }"
                @click="switchTheme('theme_2')"
              />
              <input
                type="radio"
                id="theme-3"
                :class="{ active: currentTheme === 'theme_3' }"
                @click="switchTheme('theme_3')"
              />
            </div>
          </div>
        </header>
  
        <div class="display">
          <span>{{ current || "0" }}</span>
        </div>
  
        <div class="keys">
          <q-btn @click="appendTo('7')" class="numbers" label="7" />
          <q-btn @click="appendTo('8')" class="numbers" label="8" />
          <q-btn @click="appendTo('9')" class="numbers" label="9" />
          <q-btn @click="deletes" class="delete" label="Del" />
  
          <q-btn @click="appendTo('4')" class="numbers" label="4" />
          <q-btn @click="appendTo('5')" class="numbers" label="5" />
          <q-btn @click="appendTo('6')" class="numbers" label="6" />
          <q-btn @click="add" class="operators" label="+" />
  
          <q-btn @click="appendTo('1')" class="numbers" label="1" />
          <q-btn @click="appendTo('2')" class="numbers" label="2" />
          <q-btn @click="appendTo('3')" class="numbers" label="3" />
          <q-btn @click="minus" class="operators" label="-" />
  
          <q-btn @click="dots" class="dots" label="." />
          <q-btn @click="appendTo('0')" class="numbers" label="0" />
          <q-btn @click="divide" class="operators" label="/" />
          <q-btn @click="multiplication" class="numbers" label="x" />
  
          <q-btn @click="reset" class="reset" label="Reset" />
          <q-btn @click="equals" class="equals" label="=" />
        </div>
      </div>
    </div>
  </template>
<script>
import { ref } from "vue";
export default {
  name: "MyCalculator",
  setup() {
    const previousCurrent = ref(false);
    const current = ref("");
    const operator = ref(null);
    const operatorClicked = ref(false);
    const currentTheme = ref(localStorage.getItem("theme-color"));

    // Theme Switcher
    const switchTheme = (theme) => {
      localStorage.setItem("theme-color", theme);
      currentTheme.value = localStorage.getItem("theme-color");
    };

    // Reset Button
    const reset = () => {
      current.value = "";
    };

    // Delete Button
    const deletes = () => {
      current.value = current.value.slice(0, -1);
    };

    // All Numbers Button Append To The Inputs
    const appendTo = (keys) => {
      if (operatorClicked.value) {
        current.value = "";
        operatorClicked.value = false;
      }
      current.value = `${current.value}${keys}`;
    };

    // Dots Button
    const dots = () => {
      if (current.value.indexOf(".") === -1) {
        appendTo(".");
      }
    };

    const setPrevious = () => {
      previousCurrent.value = current.value;
      operatorClicked.value = true;
    };

    // Divide Function
    const divide = () => {
      operator.value = (a, b) => a / b;
      setPrevious();
      current.value = "";
    };

    // Times Function
    const multiplication = () => {
      operator.value = (a, b) => a * b;
      setPrevious();
      current.value = "";
    };

    // Minus Function
    const minus = () => {
      operator.value = (a, b) => a - b;
      setPrevious();
      current.value = "";
    };

    // Add Function
    const add = () => {
      operator.value = (a, b) => a + b;
      setPrevious();
      current.value = "";
    };

    // Equals Function
    const equals = () => {
      current.value = `${operator.value(
        parseFloat(previousCurrent.value),
        parseFloat(current.value)
      )}`;
      previousCurrent.value = null;
    };

    return {
      previousCurrent,
      current,
      operator,
      operatorClicked,
      currentTheme,
      switchTheme,
      reset,
      deletes,
      appendTo,
      dots,
      setPrevious,
      divide,
      multiplication,
      minus,
      add,
      equals,
    };
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.container {
  min-height: 100vh;
  width: 100%;

  display: flex;
  flex-direction: column;
  align-items: center;

  background-color: var(--theme1-main-background);
}

.calculator {
  padding: 0rem 1rem;
  width: 23.75rem;
}

.header {
  width: 100%;
  padding: 1.5625rem 0rem 1.25rem;

  display: flex;
  justify-content: space-between;
  align-items: flex-end;

  & .header__logo {
    font-size: 1.75rem;
    font-weight: var(--font-bold);
    color: var(--theme1-text-white);
  }

  & .theme__switcher {
    position: relative;

    display: flex;
    flex-direction: column;
    align-items: center;

    .theme__switcher-label label {
      font-size: 0.75rem;
      color: var(--theme1-text-white);
      cursor: pointer;

      &:nth-child(2) {
        margin: 0rem 0.875rem;
      }
    }

    .theme__switcher-toggle {
      position: relative;
      width: 3.75rem;
      height: 1.4375rem;
      background: var(--theme1-toggle-background);
      border-radius: 1.25rem;
      margin-top: 0.3125rem;

      display: flex;
      flex-direction: row;
      align-items: center;
      justify-content: space-around;

      & input {
        appearance: none;
        display: inline-block;
        width: 0.8125rem;
        height: 0.8125rem;
        border-radius: 50%;
        margin-left: 3px;
        opacity: 0;

        &:hover {
          cursor: pointer;
        }
      }
      & #theme-1,
      & #theme-1.active {
        opacity: 1;
        background: var(--theme1-toggle);
        transition: all 400ms;
      }
    }

    .theme__switcher-title {
      position: absolute;
      top: 63%;
      left: -3.125rem;

      font-size: 0.75rem;
      font-weight: 700;
      color: var(--theme1-text-white);
      text-transform: uppercase;
    }
  }
}

.display {
  width: 100%;
  height: 5.625rem;
  background: var(--theme1-screen-background);
  border-radius: 0.625rem;
  padding: 1rem;

  display: flex;
  justify-content: flex-end;
  align-items: center;
  overflow-x: hidden;
  user-select: none;

  & span {
    font-size: 2.25rem;
    font-weight: var(--font-bold);
    color: var(--theme1-text-white);
    text-align: right;
    overflow: hidden;
  }
}

.keys {
  width: 100%;
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-template-rows: repeat(5, 1fr);
  align-items: center;
  justify-content: center;
  margin-top: 1.5rem;
  background: var(--theme1-keypad-background);
  border-radius: 0.625rem;
  padding: 1.25rem;

  div {
    width: 4.0625rem;
    height: 3.75rem;
    background: var(--theme1-main-key-background);

    display: flex;
    justify-content: center;
    align-items: center;

    font-size: 2rem;
    font-weight: 700;
    color: var(--theme1-text-black);

    border-radius: 0.3125rem;
    box-shadow: 0 4px var(--theme1-main-key-shadow);
    margin: 0.4375rem;

    cursor: pointer;
    user-select: none;
    transition: background-color 400ms ease;

    &:hover {
      background-color: var(--theme1-main-key-hover);
    }
  }

  .reset,
  .equals {
    grid-column: span 2;
    width: 9.0625rem;
  }

  .delete {
    font-size: 1.25rem;
    color: var(--theme1-text-white);
    text-transform: uppercase;
    background-color: var(--theme1-delete-key-background);
    box-shadow: 0 4px var(--theme1-delete-key-shadow);
    transition: background-color 400ms ease;

    &:hover {
      background-color: var(--theme1-delete-key-hover);
    }
  }

  .reset {
    font-size: 1.25rem;
    color: var(--theme1-text-white);
    text-transform: uppercase;
    background-color: var(--theme1-delete-key-background);
    box-shadow: 0 4px var(--theme1-delete-key-shadow);
    transition: background-color 400ms ease;

    &:hover {
      background-color: var(--theme1-delete-key-hover);
    }
  }

  .equals {
    font-weight: 500;
    color: var(--theme1-text-white);
    background: var(--theme1-equal-key-background);
    box-shadow: 0 4px var(--theme1-equal-key-shadow);
    transition: background-color 400ms ease;

    &:hover {
      background-color: var(--theme1-equal-key-hover);
    }
  }
}

.attribution {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  text-align: center;
  margin-top: 2rem;

  & p {
    font-size: 1.25rem;
    font-weight: 500;
    color: var(--theme1-text-white);

    & span {
      font-size: 2rem;
      color: var(--theme1-toggle);
    }

    & a {
      font-size: 1.75rem;
      font-weight: 700;
      color: var(--theme1-toggle);
      text-decoration: none;
    }
  }
}

// ===================
//    Theme 2
// ===================

.container.theme_2 {
  background: var(--theme2-main-background);

  .header {
    .header__logo {
      color: var(--theme2-text-black);
    }

    .theme__switcher {
      .theme__switcher-label label {
        color: var(--theme2-text-black);
      }

      .theme__switcher-toggle {
        background: var(--theme2-toggle-background);

        #theme-2.active {
          opacity: 1;
          background: var(--theme2-toggle);
          transition: all 400ms;
        }
        #theme-1 {
          opacity: 0;
        }
      }
      .theme__switcher-title {
        color: var(--theme2-text-black);
      }
    }
  }

  .display {
    background: var(--theme2-screen-background);

    & span {
      color: var(--theme2-text-black);
    }
  }

  .keys {
    background: var(--theme2-keypad-background);

    div {
      color: var(--theme2-text-black);
      background: var(--theme2-main-key-background);
      box-shadow: 0 4px var(--theme2-main-key-shadow);

      &:hover {
        background: var(--theme2-main-key-hover);
      }
    }

    .delete {
      color: var(--theme2-text-white);
      background-color: var(--theme2-delete-key-background);
      box-shadow: 0 4px var(--theme2-delete-key-shadow);

      &:hover {
        background-color: var(--theme2-delete-key-hover);
      }
    }

    .equals {
      color: var(--theme2-text-white);
      background-color: var(--theme2-delete-key-background);
      box-shadow: 0 4px var(--theme2-delete-key-shadow);

      &:hover {
        background-color: var(--theme2-delete-key-hover);
      }
    }

    .reset {
      color: var(--theme2-text-white);
      background: var(--theme2-equal-key-background);
      box-shadow: 0 4px var(--theme2-equal-key-shadow);

      &:hover {
        background-color: var(--theme2-equal-key-hover);
      }
    }
  }

  .attribution {
    & p {
      color: var(--theme2-text-black);

      & span {
        color: var(--theme2-delete-key-background);
      }

      & a {
        color: var(--theme2-delete-key-background);
      }
    }
  }
}

// ===================
//    Theme 3
// ===================

.container.theme_3 {
  background: var(--theme3-main-background);

  .header {
    .header__logo {
      color: var(--theme3-text-yellow);
    }

    .theme__switcher {
      .theme__switcher-label label {
        color: var(--theme3-text-yellow);
      }

      .theme__switcher-toggle {
        background: var(--theme3-toggle-background);

        #theme-3.active {
          opacity: 1;
          background: var(--theme3-toggle);
          transition: all 400ms;
        }
        #theme-1 {
          opacity: 0;
        }
      }
      .theme__switcher-title {
        color: var(--theme3-text-yellow);
      }
    }
  }

  .display {
    background: var(--theme3-screen-background);

    & span {
      color: var(--theme3-text-yellow);
    }
  }

  .keys {
    background: var(--theme3-keypad-background);

    div {
      color: var(--theme3-text-yellow);
      background: var(--theme3-main-key-background);
      box-shadow: 0 4px var(--theme3-main-key-shadow);

      &:hover {
        background-color: var(--theme3-main-key-hover);
      }
    }

    .delete {
      color: var(--theme3-text-yellow);
      background-color: var(--theme3-delete-key-background);
      box-shadow: 0 4px var(--theme3-delete-key-shadow);

      &:hover {
        background-color: var(--theme3-delete-key-hover);
      }
    }

    .equals {
      color: var(--theme3-text-black);
      background-color: var(--theme3-equal-key-background);
      box-shadow: 0 4px var(--theme3-equal-key-shadow);

      &:hover {
        background-color: var(--theme3-equal-key-hover);
      }
    }

    .reset {
      color: var(--theme3-text-yellow);
      background: var(--theme3-delete-key-background);
      box-shadow: 0 4px var(--theme3-delete-key-shadow);

      &:hover {
        background: var(--theme3-delete-key-hover);
      }
    }
  }

  .attribution {
    & p {
      color: var(--theme3-text-yellow);

      & span {
        color: var(--theme3-equal-key-background);
      }

      & a {
        color: var(--theme3-equal-key-background);
      }
    }
  }
}

// =============================
//        Media Queries
// =============================

@media screen and (min-width: 40rem) {
  .calculator {
    width: 30.625rem;
  }

  .display {
    height: 7.5rem;

    & span {
      font-size: 3rem;
    }
  }

  .keys {
    place-items: center;
    padding: 1.875rem;

    div {
      width: 5.3125rem;
      height: 4.0625rem;
      margin: 0.625rem;
    }

    .reset,
    .equals {
      width: 11.75rem;
    }
  }
}
</style>
