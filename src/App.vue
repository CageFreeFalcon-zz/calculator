<template>
  <div id="app">
    <div class="container">
      <div class="calculator">
        <div class="answer">{{ answer }}</div>

        <div class="display" id="expression">{{ current }}</div>
        <div class="btn" @click="backspace">
          <svg viewBox="0 0 640 512" class="backspace">
            <path
                d="M576 64H205.26A63.97 63.97 0 0 0 160 82.75L9.37 233.37c-12.5 12.5-12.5 32.76 0 45.25L160 429.25c12 12 28.28 18.75 45.25 18.75H576c35.35 0 64-28.65 64-64V128c0-35.35-28.65-64-64-64zm-84.69 254.06c6.25 6.25 6.25 16.38 0 22.63l-22.62 22.62c-6.25 6.25-16.38 6.25-22.63 0L384 301.25l-62.06 62.06c-6.25 6.25-16.38 6.25-22.63 0l-22.62-22.62c-6.25-6.25-6.25-16.38 0-22.63L338.75 256l-62.06-62.06c-6.25-6.25-6.25-16.38 0-22.63l22.62-22.62c6.25-6.25 16.38-6.25 22.63 0L384 210.75l62.06-62.06c6.25-6.25 16.38-6.25 22.63 0l22.62 22.62c6.25 6.25 6.25 16.38 0 22.63L429.25 256l62.06 62.06z"></path>
          </svg>
        </div>

        <div @click="clear" id="clear" class="btn clear">C</div>
        <div @click="append('(')" id="sign" class="btn">(</div>
        <div @click="append(')')" id="percent" class="btn">)</div>
        <div @click="append('/', true)" id="divide" class="btn operator">/</div>

        <div @click="append('7')" id="n7" class="btn">7</div>
        <div @click="append('8')" id="n8" class="btn">8</div>
        <div @click="append('9')" id="n9" class="btn">9</div>
        <div @click="append('*', true)" id="times" class="btn operator">X</div>

        <div @click="append('4')" id="n4" class="btn">4</div>
        <div @click="append('5')" id="n5" class="btn">5</div>
        <div @click="append('6')" id="n6" class="btn">6</div>
        <div @click="append('-', true)" id="minus" class="btn operator">-</div>

        <div @click="append('1')" id="n1" class="btn">1</div>
        <div @click="append('2')" id="n2" class="btn">2</div>
        <div @click="append('3')" id="n3" class="btn">3</div>
        <div @click="append('+', true)" id="plus" class="btn operator">+</div>

        <div @click="append('0')" id="n0" class="btn zero">0</div>
        <div @click="append('.')" id="dot" class="btn">.</div>
        <div @click="equal" id="equal" class="btn equal">=</div>
      </div>
      <div class="history">
        <div class="header">
          <h3>History</h3>
          <button class="btn download" @click="downloadcsv">
            Download CSV
          </button>
        </div>
        <div class="list">
          <template v-for="log in logList">
            <div :key="log[0]" class="item">
              <h4 class="exp">{{ log[1] }}</h4>
              <h1 class="result">{{ log[2] }}</h1>
            </div>
          </template>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      logList: [],
      current: "",
      answer: "",
      count: 1,
      isanswred: false
    };
  },
  methods: {
    append(number, isop) {
      if ( isop && this.answer !== "" && this.answer !== "Error!" && this.isanswred) {
        this.current = this.answer;
      } else if (this.answer === "Error!") {
        this.clear();
        return;
      }
      this.current = `${this.current}${number}`;
      this.isanswred = false;
    },
    clear() {
      this.current = "";
      this.answer = "";
    },
    backspace() {
      this.current = this.current.slice(0, -1);
    },
    equal() {
      try {
        this.answer = eval(this.current);
      } catch (e) {
        this.answer = "Error!";
      }
      this.logList.unshift([this.count, this.current, this.answer]);
      this.count++;
      this.isanswred = true;
    },
    downloadcsv() {
      let csvContent = "";
      if (this.logList.length !== 0) {
        this.logList.forEach(function(rowArray) {
          let row = rowArray.join(",");
          csvContent += row + "\r\n";
        });
        var download = document.createElement("a");
        var blob = new Blob([csvContent], { type: "text/csv;charset=utf-8;" });
        download.href = URL.createObjectURL(blob);
        download.setAttribute("download", "history.csv");
        download.click();
      }
    }
  }
};
</script>


<style lang="scss">
@import "./assets/scss/main.scss";
</style>
