<template>
  <v-app>
    <v-container>
      <v-row>
        <v-col>
          <div>
            <v-btn
              @click="selectNumber(number1)"
              :disabled="selectNumberCompleteFlg"
              >{{ number1 }}</v-btn
            >
            <v-btn
              @click="selectNumber(number2)"
              :disabled="selectNumberCompleteFlg"
              >{{ number2 }}</v-btn
            >
            <v-btn
              @click="selectNumber(number3)"
              :disabled="selectNumberCompleteFlg"
              >{{ number3 }}</v-btn
            >
            <v-btn
              @click="selectNumber(number4)"
              :disabled="selectNumberCompleteFlg"
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
              :disabled="selectSymbolCompleteFlg"
              >+</v-btn
            >
            <v-btn
              @click="selectSymbol('-')"
              :disabled="selectSymbolCompleteFlg"
              >-</v-btn
            >
            <v-btn
              @click="selectSymbol('×')"
              :disabled="selectSymbolCompleteFlg"
              >×</v-btn
            >
            <v-btn
              @click="selectSymbol('÷')"
              :disabled="selectSymbolCompleteFlg"
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
            {{ selectedSymbol }}
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
      selectedNumber: [],
      selectedSymbol: [],
      selectNumberCounter: 0,
      selectSymbolCounter: 0,
      selectNumberCompleteFlg: false,
      selectSymbolCompleteFlg: false,
    };
  },

  created: function () {
    //4つの異なる乱数(1~9までの整数）を生成する
    this.setNumbers();
  },

  methods: {
    deleteSelectedNumbers() {
      this.selectedNumber = [];
      this.selectNumberCompleteFlg = false;
      this.selectedSymbol = [];
      this.selectSymbolCompleteFlg = false;
      this.selectNumberCounter = 0;
      this.selectSymbolCounter = 0;
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
      this.selectedSymbol.push(symbol);
      //4つ選んだらselectSymbolCompleteFlgを立てて完了扱いにする
      this.selectSymbolCounter += 1;
      if (this.selectSymbolCounter === 4) {
        this.selectSymbolCompleteFlg = true;
      }
    },

    selectNumber(num) {
      this.selectedNumber.push(num);
      //4つ選んだらselectNumberCompleteFlgを立てて完了扱いにする
      this.selectNumberCounter += 1;
      if (this.selectNumberCounter === 4) {
        this.selectNumberCompleteFlg = true;
      }
    },
  },
};
</script>
