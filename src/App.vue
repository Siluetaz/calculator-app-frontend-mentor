<script setup>
import { ref } from 'vue'

let processes = ref("0")
let keysCount0 = ref(["7", "8", "9", "DEL"])
let keysCount1 = ref(["4", "5", "6", "+"])
let keysCount2 = ref(["1", "2", "3", "-"])
let keysCount3 = ref([".", "0", "/", "x"])
let keysCount4 = ref(["RESET", "="])
let operations = ref([])
let themeSelected = ref(1)

const actions = (index) => {
  processes.value = processes.value.replace(/,/g, "");
  let sign = processes.value.substring(
    processes.value.length - 1,
    processes.value.length
  );
  switch (index) {
    case "RESET":
      processes.value = "0";
      operations = [];
      break;
    case "DEL":
      if (processes.value.length === 1) {
        processes.value = "0";
      } else {
        processes.value = processes.value.substring(
          0,
          processes.value.length - 1
        );
      }
      break;
    case ".":
      processes.value += index;
      break;
    case "+":
      if (
        processes.value !== "0" &&
        sign !== "+" &&
        sign !== "-" &&
        sign !== "/" &&
        sign !== "x"
      ) {
        processes.value += index;
      }
      break;
    case "-":
      if (sign !== "+" && sign !== "-" && sign !== "/" && sign !== "x") {
        processes.value += index;
      }
      break;
    case "/":
      if (
        processes.value !== "0" &&
        sign !== "+" &&
        sign !== "-" &&
        sign !== "/" &&
        sign !== "x"
      ) {
        processes.value += index;
      }
      break;
    case "x":
      if (
        processes.value !== "0" &&
        sign !== "+" &&
        sign !== "-" &&
        sign !== "/" &&
        sign !== "x"
      ) {
        processes.value += index;
      }
      break;
    case "=":
      processes.value = eval(processes.value.replace(/x/g, "*"));
      processes.value = processes.value.toString();
      break;
    default:
      if (processes.value === "0") {
        processes.value = "";
        processes.value += index;
      } else {
        processes.value += index;
      }
      break;
  }

  let str = processes.value.toString().split(".");
  str[0] = str[0].replace(/\B(?=(\d{3})+(?!\d))/g, ",");
  processes.value = str.join(".");
}

const changeTheme = () => {
  if (themeSelected.value === 1) {
    document.body.removeAttribute("class");
    document.body.classList.add("theme2");
    document.getElementById("round").style.transform = "translateX(135%)";
    themeSelected.value = 2;
  } else if (themeSelected.value === 2) {
    document.body.removeAttribute("class");
    document.body.classList.add("theme3");
    document.getElementById("round").style.transform = "translateX(280%)";
    themeSelected.value = 3;
  } else {
    document.body.removeAttribute("class");
    document.getElementById("round").style.transform = "translateX(0)";
    themeSelected.value = 1;
  }
}
</script>
<template>
  <div class="container">
    <div class="header">
      <div class="header-title">calc</div>
      <div class="toggle">
        <div class="toggle-title">THEME</div>
        <div class="theme">
          <div class="theme-numbers">
            <div class="numbers">1</div>
            <div class="numbers">2</div>
            <div class="numbers">3</div>
          </div>
          <div class="bg-theme" @click="changeTheme()">
            <div class="round-red" id="round"></div>
          </div>
        </div>
      </div>
    </div>
    <div class="screen">
      <input class="input-numbers" type="text" v-model="processes" />
    </div>
    <div class="width-100">
      <div class="keypad">
        <div v-for="index in keysCount0" :key="index" :class="[index === 'DEL' ? 'key blue' : 'key white']"
          @click="actions(index)">
          {{ index }}
        </div>
        <div v-for="index in keysCount1" :key="index" class="key white" @click="actions(index)">
          {{ index }}
        </div>
        <div v-for="index in keysCount2" :key="index" class="key white" @click="actions(index)">
          {{ index }}
        </div>
        <div v-for="index in keysCount3" :key="index" class="key white" @click="actions(index)">
          {{ index }}
        </div>
        <div v-for="index in keysCount4" :key="index" :class="[
          index === 'RESET' ? 'key blue w-lg-reset' : 'key red w-lg-equal',
        ]" @click="actions(index)">
          {{ index }}
        </div>
      </div>
    </div>
  </div>
</template>

<style lang="scss">
body {
  background-color: var(--background-main);
  color: var(--text-1);

  .container {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    max-width: 16rem;
    width: 100%;
    margin: 20px;

    .keypad {
      background-color: var(--background-toggle);
      user-select: none;
      border-radius: 10px;
      padding: 0.8rem;
      display: grid;
      grid-template-columns: repeat(4, minmax(0, 1fr));
      gap: 0.8rem;

      .white {
        background-color: var(--background-key-1);
        box-shadow: var(--shadow-key-1);

        &:hover {
          background-color: var(--hover-key-1);
        }
      }

      .key {
        cursor: pointer;
        display: flex;
        align-items: center;
        justify-content: center;
        border-radius: 5px;
        padding: 0.3rem 0.5rem;
      }

      .red {
        font-size: 26px;
        color: var(--text-2);
        background-color: var(--background-key-2);
        box-shadow: var(--shadow-key-2);

        &:hover {
          background-color: var(--hover-key-2);
        }
      }

      .blue {
        font-size: 26px;
        color: var(--text-2);
        background-color: var(--background-key-3);
        box-shadow: var(--shadow-key-3);

        &:hover {
          background-color: var(--hover-key-3);
        }
      }

      .w-lg-reset {
        grid-column: 1/3;
      }

      .w-lg-equal {
        grid-column: 3/5;
      }
    }

    .screen {
      width: 100%;
      height: 3rem;
      background-color: var(--background-screen);
      border-radius: 10px;
      margin-bottom: 0.8rem;
      display: flex;

      .input-numbers {
        font-size: 38px;
        padding: 0px 0.6rem 0px 0px;
        width: 100%;
        text-align: end;
        background: none;
        border: none;
        color: var(--text-screen);
      }
    }

    .header {
      user-select: none;
      display: flex;
      width: 100%;
      align-items: flex-end;
      margin: 0rem 0rem 0.8rem 0rem;
      color: var(--text-header);

      .header-title {
        margin-left: 0.2rem;
        font-size: 30px;
      }

      .toggle {
        display: flex;
        width: 100%;
        justify-content: end;
        align-items: center;
        gap: 0.8rem;

        .toggle-title {
          padding-top: 0.6rem;
          font-size: 10px;
          letter-spacing: 0.1rem;
        }

        .theme {
          display: flex;
          flex-direction: column;
          align-items: center;

          .theme-numbers {
            width: 80%;
            display: flex;
            justify-content: space-between;
            margin: 0rem 0rem 0.2rem 0rem;

            .numbers {
              font-size: 10px;
            }
          }

          .bg-theme {
            background-color: var(--background-toggle);
            padding: 0.15rem 0.15rem;
            border-radius: 20px;
            width: 1.8rem;
            cursor: pointer;
            display: flex;
          }
        }
      }
    }

    .round-red {
      width: 15px;
      height: 15px;
      background-color: var(--background-key-2);
      border-radius: 900px;
      transition: 0.5s;
      transform: translateX(0);

      &:hover {
        background-color: var(--hover-key-2);
      }
    }
  }

  .width-100 {
    width: 100%;
  }
}
</style>
