<template>
  <v-app>
    <v-container>
      <v-row>
        <v-col>
          <div>
            <v-btn
              @click="selectNumber(number1, 1)"
              :disabled="
                nowSelectNumberOrSymbol !== 'number' || numberSelectedFlgs.one
              "
              >{{ number1 }}</v-btn
            >
            <v-btn
              @click="selectNumber(number2, 2)"
              :disabled="
                nowSelectNumberOrSymbol !== 'number' || numberSelectedFlgs.two
              "
              >{{ number2 }}</v-btn
            >
            <v-btn
              @click="selectNumber(number3, 3)"
              :disabled="
                nowSelectNumberOrSymbol !== 'number' || numberSelectedFlgs.three
              "
              >{{ number3 }}</v-btn
            >
            <v-btn
              @click="selectNumber(number4, 4)"
              :disabled="
                nowSelectNumberOrSymbol !== 'number' || numberSelectedFlgs.four
              "
              >{{ number4 }}</v-btn
            >
          </div>
        </v-col>
      </v-row>
      <v-row>
        <v-col>
          <div>
            <v-btn
              @click="selectSymbol('+')"
              :disabled="
                selectCompleteFlg || nowSelectNumberOrSymbol !== 'symbol'
              "
              >+</v-btn
            >
            <v-btn
              @click="selectSymbol('-')"
              :disabled="
                selectCompleteFlg || nowSelectNumberOrSymbol !== 'symbol'
              "
              >-</v-btn
            >
            <v-btn
              @click="selectSymbol('×')"
              :disabled="
                selectCompleteFlg || nowSelectNumberOrSymbol !== 'symbol'
              "
              >×</v-btn
            >
            <v-btn
              @click="selectSymbol('÷')"
              :disabled="
                selectCompleteFlg || nowSelectNumberOrSymbol !== 'symbol'
              "
              >÷</v-btn
            >
          </div>
        </v-col>
      </v-row>
      <v-row>
        <v-col>
          <div>
            <v-btn @click="selectParentheses('(')"> ( </v-btn>
            <v-btn @click="selectParentheses(')')"> ) </v-btn>
          </div>
        </v-col>
      </v-row>
      <v-row>
        <v-col>
          <span v-for="item in displayFormula" :key="item.id">
            {{ item }}
          </span>
        </v-col>
      </v-row>
      <v-row>
        <v-col>
          <div>
            <v-btn
              @click="
                deleteSelectedNumbers();
              "
              >clear
            </v-btn>
            </div>
            <div>
            <v-btn
              @click="
                setNumbers();
                deleteSelectedNumbers();
              "
              >reload
            </v-btn>
          </div>
        </v-col>
      </v-row>
    </v-container>
  </v-app>
</template>

<script>
export default {
  name: 'Home',

  components: {},
  data: function () {
    return {
      number1: 0,
      number2: 0,
      number3: 0,
      number4: 0,
      numberSelectedFlgs: { one: false, two: false, three: false, four: false },
      selectedNumber: [],
      selectedSymbol: [],
      displayFormula: [],
      selectNumberCounter: 0,
      selectSymbolCounter: 0,
      selectCompleteFlg: false,
      nowSelectNumberOrSymbol: 'number',
    };
  },

  created: function () {
    //4つの異なる乱数(1~9までの整数）を生成する
    this.setNumbers();
  },

  methods: {
    deleteSelectedNumbers() {
      this.selectedNumber = [];
      this.selectedSymbol = [];
      this.selectCompleteFlg = false;
      this.selectNumberCounter = 0;
      this.selectSymbolCounter = 0;
      this.nowSelectNumberOrSymbol = 'number';
      this.displayFormula = [];
      this.numberSelectedFlgs = {
        one: false,
        two: false,
        three: false,
        four: false,
      };
    },
    setNumbers() {
      //4つの異なる乱数(1~9までの整数）を生成する
      //1つ目
      this.number1 = Math.floor(Math.random() * 9) + 1;
      //2つ目
      do {
        this.number2 = Math.floor(Math.random() * 9) + 1;
      } while (this.number2 === this.number1);
      //3つ目
      do {
        this.number3 = Math.floor(Math.random() * 9) + 1;
      } while (this.number3 === this.number2 || this.number3 === this.number1);
      //4つ目
      do {
        this.number4 = Math.floor(Math.random() * 9) + 1;
      } while (
        this.number4 === this.number3 ||
        this.number4 === this.number2 ||
        this.number4 === this.number1
      );
    },
    selectSymbol(symbol) {
      //選ばれたのが何番目かをカウントする
      this.selectSymbolCounter += 1;
      //選択された記号を配列に入れる
      this.displayFormula.push(symbol);
      //選択された記号を配列に入れる
      this.selectedSymbol.push({
        seq: this.selectSymbolCounter,
        symbol: symbol,
      });
      //次は数字を選ぶ
      this.nowSelectNumberOrSymbol = 'number';
    },

    selectParentheses(parentheses) {
      //選択された記号を配列に入れる
      this.displayFormula.push(parentheses);
    },

    selectNumber(num, seq) {
      //選ばれたのが何番目かをカウントする
      this.selectNumberCounter += 1;
      //選択された数字を配列に入れる
      this.displayFormula.push(num);
      //選ばれた記号のボタンを選択済みとして非活性にする
      if (seq === 1) {
        this.numberSelectedFlgs.one = true;
      } else if (seq === 2) {
        this.numberSelectedFlgs.two = true;
      } else if (seq === 3) {
        this.numberSelectedFlgs.three = true;
      } else if (seq === 4) {
        this.numberSelectedFlgs.four = true;
      }

      //4つ選んだらselectNumberCompleteFlgを立てて完了扱いにする
      if (this.selectNumberCounter === 4) {
        this.selectCompleteFlg = true;
      }
      //次は記号を選ぶ
      this.nowSelectNumberOrSymbol = 'symbol';
    },
  },
};
</script>
