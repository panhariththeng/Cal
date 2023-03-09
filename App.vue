<template>
  <div id="app">
    <div class="container">
      <table>
        <tr>
          <td colspan="5">
            <div id="screen">
              <span id="screen_top">M=0</span>
              <div id="screen_bottom">
                <!-- v-text is a directive that is used to replace the content of HTML tag with private data -->
                <!-- It will update the content automatically when data is changed. It is called data reactive -->
                <span  id="operand1">{{current || '0'}}</span>
                <span id="operator"></span>
                <span id="operand2"></span>
              </div>
              <!-- <span id="screen_bottom">0</span> -->
            </div>
          </td>
        </tr>
        <tr>
          <td>
            <button type="button" class="btn btn-warning">MC</button>
          </td>
          <td>
            <button type="button" class="btn btn-warning">MR</button>
          </td>
          <td>
            <button type="button" class="btn btn-warning">M-</button>
          </td>
          <td>
            <button type="button" class="btn btn-warning">M+</button>
          </td>
          <td>
            <button @click="remove" class="btn btn-light">
              <i class="fa fa-long-arrow-right" aria-hidden="true"></i>
            </button>
          </td>
        </tr>
        <tr>
          <td>
            <button  @click="append('7')"
              class="btn btn-light">7</button>
          </td>
          <td>
            <button  @click="append('8')"
              class="btn btn-light">8</button>
          </td>
          <td>
            <button  @click="append('9')"
              class="btn btn-light">9</button>
          </td>
          <td>
            <button type="button" @click ="divide" class="btn btn-secondary">÷</button>
          </td>
          <td>
            <button type="button" @click="sign" class="btn btn-light">+/-</button>
          </td>
        </tr>
        <tr>
          <td>
            <button  @click="append('4')"
              class="btn btn-light">4</button>
          </td>
          <td>
            <button  @click="append('5')"
              class="btn btn-light">5</button>
          </td>
          <td>
            <button  @click="append('6')"
              class="btn btn-light">6</button>
          </td>
          <td>
            <button @click="times" class="btn btn-secondary">x</button>
          </td>
          <td>
            <button @click="minus" class="btn btn-secondary">-</button>
          </td>
        </tr>
        <tr>
          <td>
            <button
              @click="append('1')"
              class="btn btn-light"
            >
              1
            </button>
          </td>
          <td>
            <button  @click="append('2')"
              class="btn btn-light">2</button>
          </td>
          <td>
            <button  @click="append('3')"
              class="btn btn-light">3</button>
          </td>
          <td rowspan="2">
            <button @click="add" class="btn btn-secondary long-btn">+</button>
          </td>
          <td rowspan="2">
            <button @click ="equal" class="btn btn-primary long-btn">=</button>
          </td>
        </tr>
        <tr>
          <td>
            <button type="button" class="btn btn-danger " @click='clear'>C</button>
          </td>
          <td>
            <button @click="append('0')" class="btn btn-light">0</button>
          </td>
          <td>
            <button @click="dot" class="btn btn-light">.</button>
          </td>
        </tr>
      </table>
    </div>
    <div class="alert alert-danger" id="message_panel" role="alert">
      something wrong here
    </div>
  </div>
</template>

<script>
export default {
  name: 'App',
  components: {},
  data() {
    return {
      previous: null,
     current: '',
     opeator: null,
     operatorClick: false,
    };
  },
  methods: {
    append(number) {
      // Assign number when user click to the inputNumber data
      // To access private data from methods, use (this.)
      if (this.operatorClick){
        this.current = '';
        this.operatorClick = false;
      }
      this.current = `${this.current}${number}`;
    },
    clear(){
      this.current = '';
    },

    sign(){
      this.current = this.current.charAt(0) === '-'?
      this.current.slice(1) : `-${this.current}`; 
    },
    percent(){
      this.current = `${parseFloat(this.current) / 100}`
    },
    dot(){
      if (this.current.indexOf('.') ===-1){
        this.append('.');
      }
    },

    setPreviouse(){
      this.previous = this.current
      this.operatorClick = true

    },
    divide(){
      this.opeator = (a, b) => a/ b;
      this.setPreviouse();
    },
    times(){
      this.opeator = (a, b) => a* b;

        this.setPreviouse();
    },
    minus(){
      this.opeator = (a, b) => a- b;

      this.setPreviouse();
    },
    add(){
      this.opeator = (a, b) => a+ b;

    this.setPreviouse();
    },
    equal(){
      this.current = `${this.opeator(parseFloat(this.current), parseFloat(this.previous))}`;
      this.previous = null
    },
    remove(){
      this.current = this.current.slice(0,-1)
    },

  },
 
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.container {
  margin-top: 10em;
  width: 300px;
  border: 1px solid black;
  padding-top: 20px;
  padding-bottom: 20px;
}
table {
  border-spacing: 7px;
  border-collapse: separate;
}
#screen {
  border: 1px solid black;
  padding: 7px;
  width: 100%;
  height: 4em;
}
#screen_top {
  display: block;
  font-size: 0.8rem;
}
#screen_bottom {
  font-size: 1.8rem;
  display: block;
  text-align: right;
}
#operand2 {
  background-color: skyblue;
}
#operator {
  background-color: rosybrown;
}
.button-row {
  display: flex;
  justify-content: space-between;
}
button {
  width: 45px;
}
.long-btn {
  display: inline-block;
  height: 80px;
}

/* Message panel */
#message_panel {
  width: 300px;
  margin-top: 1em;
  display: none;
  margin-left: auto;
  margin-right: auto;
}
</style>
