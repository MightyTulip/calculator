import MenuIcon from 'vue-material-design-icons/Menu.vue';
<template>
  <div>
    <div class="display-input">
      <HistoryIcon />
      <div class="result">
        <div class="total">
          {{total}}
        </div>
      </div>
      <div class="input-wrapper">
        <div class="input-inner">
          <span class="input">
            {{input}}
          </span>
        </div>
      </div>
    </div>
    <div class="keypad">
      <table class="left-pad">
        <tbody>
        <tr v-for="(leftPad, index) in leftKeyPads" :key="index">
          <td v-for="pad in leftPad.keys"
              :key="pad.num"
              :colspan="pad.isLarge ? 2 : 1"
              @click="handlePress(pad)" >
            <div class="num-pad">
              <div class="num" :class="pad.class" v-html="pad.num"></div>
            </div>
          </td>
        </tr>
        </tbody>
      </table>
      <table class="right-pad">
        <tbody>
        <tr v-for="rightPad in rightKeyPads" :key="rightPad">
          <td v-for="pad in rightPad.keys"
              :key="pad.num"
              v-on:click="handlePress(pad)" >
            <div class="num-pad">
              <div class="num" :class="pad.class" role="button">
                {{pad.num}}
              </div>
            </div>
          </td>
        </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
import HistoryIcon from 'vue-material-design-icons/History.vue';
import stringMath from 'string-math';
export default {

  name: "Calculator.vue",
  components: {HistoryIcon},
  data: function() {
    return {
      input: '',
      total: '',
      rightKeyPads: [{
        keys: [{
          class: 'operator not-used',
          num: '('
        }, {
          class: 'operator not-used',
          num: ')'
        },{
          class: 'operator not-used',
          num: '%'
        },{
          class: 'operator clear',
          num: 'CE'
        }]
      }, {
        keys: [{
          class: 'num-key',
          num: '7'
        }, {
          class: 'num-key',
          num: '8'
        },{
          class: 'num-key',
          num: '9'
        },{
          class: 'operator symbol',
          num: '÷',
          value: '/'
        }]
      }, {
        keys: [{
          class: 'num-key',
          num: '4'
        }, {
          class: 'num-key',
          num: '5'
        },{
          class: 'num-key',
          num: '6'
        },{
          class: 'operator symbol',
          num: 'x',
          value: '*'
        }]
      }, {
        keys: [{
          class: 'num-key',
          num: '1'
        }, {
          class: 'num-key',
          num: '2'
        },{
          class: 'num-key',
          num: '3'
        },{
          class: 'operator symbol',
          num: '-'
        }]
      }, {
        keys: [{
          class: 'num-key',
          num: '0'
        }, {
          class: 'num-key symbol bold',
          num: '.'
        }, {
          class: 'equal symbol',
          num: '='
        }, {
          class: 'operator symbol',
          num: '+'
        }]
      }],
      leftKeyPads: [{
        keys: [{
          class: 'operator deg-rad',
          isLarge: true,
          num: '<div class="rad">Rad</div><div class="pipe"></div><div class="deg">Deg</div>'
        }, {
          class: 'operator',
          num: 'x!'
        }]
      }, {
        keys: [{
          class: 'operator',
          num: 'Inv'
        }, {
          class: 'operator',
          num: 'sin'
        },{
          class: 'operator',
          num: 'ln'
        }]
      }, {
        keys: [{
          class: 'operator',
          num: 'π'
        }, {
          class: 'operator',
          num: 'cos'
        },{
          class: 'operator',
          num: 'log'
        }]
      }, {
        keys: [{
          class: 'operator',
          num: 'e'
        }, {
          class: 'operator',
          num: 'tan'
        },{
          class: 'operator',
          num: '√'
        }]
      }, {
        keys: [{
          class: 'operator',
          num: 'Ans'
        }, {
          class: 'operator',
          num: 'EXP'
        }, {
          class: 'operator',
          num: 'X<sup>y</sup>'
        }]
      }]
    }
  },
  methods: {
    handlePress: function(keyInput) {
      if (this.total.indexOf('=') !== -1 && this.total.indexOf('Ans') === -1) {
        this.total = `Ans = ${this.input}`;
        this.input = '';
      }
      if (this.total.indexOf('invalid') !== -1) {
        this.total = '';
        this.input = '';
      }

      if (keyInput.class.indexOf('num-key') !== -1) {
        this.input += keyInput.num;
      } else if (keyInput.class === 'operator symbol') {
        this.input += ' '  + keyInput.num + ' ';
      } else if (keyInput.class === 'operator clear') {
        this.input = '';
      } else if (keyInput.class === 'equal symbol') {
        this.input += ' ='
        this.calculate();
      }
    },
    calculate: function() {
      const component = this;
      if (this.input === '') {
        return;
      }
      const formattedInput = this.input.replace('x', '*').replace('÷', '/').replace('=', '');
      const total = stringMath(formattedInput, function(error, result) {
        if (error) {
          component.input = 'Input is invalid, try something else!';
        } else {
          return result;
        }
      });
      this.total = this.input;
      this.input = total;
    }
  }
}
</script>

<style scoped>
  .keypad {
    display: table;
    position: relative;
    width: 100%;
  }
  .left-pad {
    float: left;
    width: 42%;
    border-collapse: collapse;
    table-layout: fixed;
  }

  .right-pad {
    float: right;
    width: 58%;
    border-collapse: collapse;
    table-layout: fixed;
    font-size: 14px;

  }
  .num-pad {
    cursor: pointer;
    font-family: arial,sans-serif;
    text-align: center;
    line-height: 34px;
    margin: 4px;
    box-sizing: border-box;
    position: relative;
    border-radius: 4px;
    touch-action: manipulation;
  }

  .num-key {
    background: #f1f3f4 !important;
    border: 1px solid #f1f3f4 !important;
    box-sizing: initial !important;
  }

  .num {
    background: #dadce0;
    color: #202124;
    border: 1px solid #dadce0;
    border-radius: 4px;
    box-sizing: border-box;
  }

  .num:focus {
    border-color: #70757a;
  }

  .display-input {
    width: 98.5%;
    height: 72px;
    border: 1px solid #ebebeb;
    border-radius: 8px;
    margin: auto;
    padding: 10px 14px 0 10px;
    box-sizing: border-box;
    position: relative;
  }

  .result {
    transition: height 0.2s,font-size 0.2s,color 0.2s;
    width: 100%;
    height: 20px;
    color: #70757a;
    display: table;
    font-size: 13px;
    position: relative;
    table-layout: fixed;
  }

  .total {
    width: 100%;
    text-align: right;
    font-family: arial,sans-serif;
    float: right;
    white-space: nowrap;
    margin-top: 3px;
  }
  .input-wrapper {
    transition: line-height 0.2s;
    display: table;
    font-size: 30px;
    height: 32px;
    vertical-align: bottom;
    position: relative;
    table-layout: fixed;
    width: 100%;
    z-index: 0;
  }

  .input-inner {
    display: table-cell;
    vertical-align: middle;
    width: 100%;
  }

  .symbol {
    font-size: 18px;
  }

  .bold {
    font-weight: bold;
  }

  .input {
    user-select: text;
    color: unset;
    float: right;
    font-family: arial,sans-serif;
    white-space: nowrap;
  }

  .deg-rad {
    overflow: hidden;
  }

  .deg-rad::v-deep .rad {
    position: relative;
    float: left;
    width: 50%;
  }


  .deg-rad::v-deep .pipe {
    content: "";
    position: absolute;
    top: 10px;
    bottom: 10px;
    left: 50%;
    border-left: 1px solid #70757a;
    transform: translateX(-50%);
  }

  .deg-rad::v-deep .deg {
    color: #70757a;
    position: relative;
    float: right;
    width: 50%;
  }

  .clear {
    font-size: 13px;
  }

  .sup {
    font-size: 8px;
    line-height: 8px;
  }

  .equal {
    background: #4285f4;
    color: #fff;
    border: 1px solid #4285f4;
    border-radius: 4px;
  }

  .history-icon {
    color: #70757a;
    position: absolute;
    left: 10px;
  }

</style>
