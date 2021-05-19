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
            {{ selectedNumber }}
          </div>
          <div>
            {{ displayNumber }}
          </div>
        </v-col>
      </v-row>
      <v-row>
        <v-col>
          <div>
            <v-btn
              @click="
                setNumbers();
                deleteSelectedNumbers();
              "
              >reload</v-btn
            >
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

  computed: {
    //数字・計算記号リストから表示用の数式文字列を生成する
    displayNumber: function () {
      //今いくつ数字が選択されているかで表示すべき数式が変わるのでifで分岐
      //1つめのとき
      if (this.selectNumberCounter === 1) {
        const wk1 = this.selectedNumber[0];
        return String(wk1.number);
      }
      //2つめのとき
      else if (this.selectNumberCounter === 2) {
        const wk1 = this.selectedNumber[0];
        const wk2 = this.selectedNumber[1];
        return (
          String(wk1.number) +
          this.selectedSymbol[0].symbol +
          String(wk2.number)
        );
      }
      //3つめのとき
      else if (this.selectNumberCounter === 3) {
        const wk1 = this.selectedNumber[0];
        const wk2 = this.selectedNumber[1];
        const wk3 = this.selectedNumber[2];
        return (
          String(wk1.number) +
          this.selectedSymbol[0].symbol +
          String(wk2.number) +
          this.selectedSymbol[1].symbol +
          String(wk3.number)
        );
      }
      //4つめのとき
      else if (this.selectNumberCounter === 4) {
        const wk1 = this.selectedNumber[0];
        const wk2 = this.selectedNumber[1];
        const wk3 = this.selectedNumber[2];
        const wk4 = this.selectedNumber[3];
        return (
          String(wk1.number) +
          this.selectedSymbol[0].symbol +
          String(wk2.number) +
          this.selectedSymbol[1].symbol +
          String(wk3.number) +
          this.selectedSymbol[2].symbol +
          String(wk4.number)
        );
      } else {
        return null;
      }
    },
  },

  methods: {
    deleteSelectedNumbers() {
      this.selectedNumber = [];
      this.selectedSymbol = [];
      this.selectCompleteFlg = false;
      this.selectNumberCounter = 0;
      this.selectSymbolCounter = 0;
      this.nowSelectNumberOrSymbol = 'number';
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
      this.selectedSymbol.push({
        seq: this.selectSymbolCounter,
        symbol: symbol,
      });
      //次は数字を選ぶ
      this.nowSelectNumberOrSymbol = 'number';
    },

    selectNumber(num, seq) {
      //選ばれたのが何番目かをカウントする
      this.selectNumberCounter += 1;
      //選択された数字を配列に入れる
      this.selectedNumber.push({ seq: this.selectNumberCounter, number: num });
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
