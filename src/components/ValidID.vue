<template>
  <div>
    <input type="text" v-model="number" />
    <button @click="getId">產生</button>
  </div>
  <div>
    <label>{{ fullName }}</label>
    <span v-html="msg"></span>
  </div>
</template>

<script>
export default {
  data() {
    return {
      number: "",
      labelNumber: "",
    };
  },
  methods: {
    //最小(包含)與最大(包含)值間的亂數
    getRandom: function (min, max) {
      return Math.floor(Math.random() * (max - min + 1)) + min;
    },
    //取得身份證字號
    getId: function () {
      var a = [];
      var value = "";
      for (var i = 65; i < 91; i++) {
        a.push(String.fromCharCode(i));
      }
      value = a[this.getRandom(0, 25)] + this.getRandom(1, 2);
      // eslint-disable-next-line no-redeclare
      for (var i = 0; i < 7; i++) {
        value += this.getRandom(1, 9);
      }
      this.number = value += this.getchknum(value);
    },
    //取得身份證字號最後一個檢查碼
    getchknum: function (x) {
      try {
        var a = [];
        var b = [
          10, 11, 12, 13, 14, 15, 16, 17, 34, 18, 19, 20, 21, 22, 35, 23, 24,
          25, 26, 27, 28, 29, 32, 30, 31, 33,
        ];

        for (var i = 65; i < 91; i++) {
          a.push(String.fromCharCode(i));
        }

        var num = a.indexOf(x.substr(0, 1));
        var b1 = parseInt(b[num].toString().substr(0, 1));
        var b2 = parseInt(b[num].toString().substr(1, 1));

        var fnum = b1 + b2 * 9; //英文字母算出的數字
        var ff = 1;
        for (var j = 8; j > 0; j--) {
          fnum += x.substr(j, 1) * ff;
          ff++;
        }
        var final = fnum % 10 == 0 ? 0 : 10 - (fnum % 10); //檢查碼
        return final;
      } catch {
        return null;
      }
    },
  },
  watch:{

  },
  computed: {
    msg: {
      get() {
        let id = this.number.trim();
        const msg = '<i class="fa-solid fa-xmark"></i>';
        const verification = id.match("^[A-Z][12]\\d{8}$");
        if (!id) {
          return "";
        } else if (!verification) {
          return msg;
        } else {
          let conver = "ABCDEFGHJKLMNPQRSTUVXYWZIO";
          let weights = [1, 9, 8, 7, 6, 5, 4, 3, 2, 1, 1];

          id = String(conver.indexOf(id[0]) + 10) + id.slice(1);

          let checkSum = 0;
          for (let i = 0; i < id.length; i++) {
            let c = parseInt(id[i]);
            let w = weights[i];
            checkSum += c * w;
          }
          return checkSum % 10 == 0 ? '<i class="fa-solid fa-check"></i>' : msg;
        }
      },
    },
    fullName: {
      get() {
        return "身分證字號：" + this.number;
      },
      set(newValue) {
        this.labelNumber = newValue;
      },
    },
  },
};
</script>
<style>
.fa-check {
  color: green;
}
.fa-xmark {
  color: red;
}
</style>
