<template>
  <v-app>
    <v-container>
      <v-row>
        <v-col>
          <div>
            <v-btn
              @click="selectNumber(number1)"
              :disabled="selectCompleteFlg"
              >{{ number1 }}</v-btn
            >
            <v-btn
              @click="selectNumber(number2)"
              :disabled="selectCompleteFlg"
              >{{ number2 }}</v-btn
            >
            <v-btn
              @click="selectNumber(number3)"
              :disabled="selectCompleteFlg"
              >{{ number3 }}</v-btn
            >
            <v-btn
              @click="selectNumber(number4)"
              :disabled="selectCompleteFlg"
              >{{ number4 }}</v-btn
            >
          </div>
        </v-col>
      </v-row>
      <v-row>
        <v-col>
          <div>
            <v-btn>+</v-btn>
            <v-btn>-</v-btn>
            <v-btn>×</v-btn>
            <v-btn>÷</v-btn>
          </div>
        </v-col>
      </v-row>
      <v-row>
        <v-col>
          <div>
            {{ selectedNumber }}
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
      selectNumberCounter: 0,
      selectCompleteFlg: false,
    };
  },

  created: function () {
    //4つの異なる乱数(1~9までの整数）を生成する
    this.setNumbers();
  },

  methods: {
    deleteSelectedNumbers() {
      this.selectedNumber = [];
      this.selectCompleteFlg = false;
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

    selectNumber(num) {
      this.selectedNumber.push(num);
      //4つ選んだらselectCompleteFlgを立てて完了扱いにする
      this.selectNumberCounter += 1;
      if (this.selectNumberCounter === 4) {
        this.selectCompleteFlg = true;
      }
    },
  },
};
</script>
