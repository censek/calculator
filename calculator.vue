<template>
  <div id="test-wrap">
    <div class="calculator">
      <div class="result-area">
        <span class="show-number">
          {{ number }}
        </span>
      </div>
      <div class="btn-area">
        <div class="computed-box">
          <div v-for="(item, index) in computedList" :key="index" class="computed-btn"
            @click="handleClickComputed(item.type)">
            {{ item.text }}
          </div>
        </div>
        <div class="number-box">
          <div v-for="(item, index) in numberList" :key="index" class="number-btn"
            @click="handleClickNumber(item.text)">{{ item.text }}</div>
          <div class="number-btn" @click="handleClear">AC</div>
          <div class="result-btn" @click="handleComputedResult">＝</div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      number: "0",
      calculation: "",
      calculationNumber: 0,
      computedRule: "",
      hasCalculation: false,
      awaitNumber: "",
      lastCalculation: "",
      computedList: [
        {
          text: "+",
          type: "+"
        },
        {
          text: "-",
          type: "-"
        },
        {
          text: "×",
          type: "*"
        },
        {
          text: "÷",
          type: "/"
        }
      ],
      numberList: [
        {
          text: "0"
        },
        {
          text: "1"
        },
        {
          text: "2"
        },
        {
          text: "3"
        },
        {
          text: "4"
        },
        {
          text: "5"
        },
        {
          text: "6"
        },
        {
          text: "7"
        },
        {
          text: "8"
        },
        {
          text: "9"
        }
      ]
    };
  },
  watch: {
    computedRule(newVal) {
      console.log(newVal);
    }
  },
  methods: {
    handleClickNumber(number) {
      if (this.number === "0") {
        this.number = number;
      } else {
        if (this.hasCalculation) {
          this.number = number;
          this.hasCalculation = false;
        } else {
          this.number = `${this.number}` + number;
        }
      }
      this.computedRule = `${this.computedRule}${number}`;
    },
    handleClickComputed(type) {
      this.hasCalculation = true;
      if (!this.lastCalculation) {
        this.lastCalculation = type;
      } else {
        this.getComputedResult();
      }
      this.computedRule = `${this.computedRule}${type}`;
    },
    getComputedResult() {
      try {
        // eslint-disable-next-line no-eval
        this.number = eval(this.computedRule);
      } catch (e) {
        this.computedRule = this.computedRule.substr(
          0,
          this.computedRule.length - 1
        );
        console.warn(e);
      }
    },
    handleComputedResult() {
      this.getComputedResult();
    },
    handleClear() {
      this.computedRule = "";
      this.number = "0";
      this.hasCalculation = false;
    }
  }
};
</script>

<style lang="scss">
#test-wrap {
  .calculator {
    width: 312px;
    padding: 20px;
    border-radius: 16px;
    background-color: #000;
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    .result-area {
      height: 100px;
      width: 100%;
      padding: 10px;
      margin-bottom: 20px;
      position: relative;
      .show-number {
        word-break: break-all;
        position: absolute;
        text-align: right;
        bottom: 0;
        right: 10px;
        width: 100%;
        color: white;
        font-size: 40px;
      }
    }
    .btn-area {
      .computed-box {
        display: flex;
        align-items: center;
        .computed-btn {
          width: 48px;
          height: 48px;
          border-radius: 100%;
          background-color: orange;
          text-align: center;
          color: white;
          line-height: 48px;
          font-size: 36px;
          margin: 0 10px;
          cursor: pointer;
          transition: 0.2s linear background-color;
          &:active {
            background-color: #ffd689;
          }
        }
      }
      .number-box {
        display: flex;
        flex-wrap: wrap;
        .number-btn {
          width: 48px;
          height: 48px;
          border-radius: 100%;
          background-color: #333333;
          text-align: center;
          color: white;
          line-height: 48px;
          font-size: 36px;
          margin: 10px 10px 0 10px;
          cursor: pointer;
          transition: 0.2s linear background-color;
          &:active {
            background-color: #909399;
          }
          &:nth-child(11) {
            font-size: 20px;
            background-color: #909399;
            &:active {
              background-color: #c4c9ce;
            }
          }
        }
        .result-btn {
          @extend .number-btn;
          background-color: orange;
        }
      }
    }
  }
}
</style>