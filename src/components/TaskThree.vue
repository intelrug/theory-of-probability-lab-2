<template>
    <div>
      <div class="name-laba">
        <h1>ЛАБОРАТОРНАЯ РАБОТА № 2 ТЕОРЕМЫ СЛОЖЕНИЯ И УМНОЖЕНИЯ ВЕРОЯТНОСТЕЙ. <br> ФОРМУЛЫ ПОЛНОЙ
          ВЕРОЯТНОСТИ И БАЙЕСА</h1>
        <p>Пять стрелков производят по одному выстрелу в цель.<br>Вероятности попадания в цель i-ым
          стрелком соответственно равны pi.<br>Найти вероятность попадания в цель: </p>
      </div>

      <input-number :label="`Вероятность попадания в цель ${n}-ым стрелком: `"
                   v-for="n in 5"
                   :key="n"
                   v-model="probability[n - 1]"/>

      <div class="content-answer">
        <p v-for="n in 5" :key="n">a) Вероятность попадания в цель только {{n}}-м стрелком:
          {{a(n - 1)}}</p>
        <p>b) Вероятность попадания в цель только одним стрелком: {{b}}</p>
        <p>c) Вероятность попадания в цель только двумя стрелками: {{c}}</p>
        <p>d) Вероятность попадания в цель не менее, чем четыремя стрелками: {{d}}</p>
        <p>e) Вероятность попадания в цель хотя бы одним стрелком: {{e}}</p>
      </div>
    </div>
</template>

<script>
import { Decimal } from 'decimal.js';
import InputNumber from './InputNumber.vue';

export default {
  name: 'TaskThree',
  components: { InputNumber },
  data() {
    return {
      probability: [
        0.8,
        0.7,
        0.5,
        0.1,
        0.1,
      ],
    };
  },
  computed: {
    q() {
      return i => new Decimal(1).minus(this.p(i));
    },
    p() {
      return i => new Decimal(this.probability[i]);
    },
    a() {
      return (index) => {
        let P = new Decimal(1);
        for (let i = 0; i < 5; i += 1) {
          P = P.mul(i === index ? this.p(i) : this.q(i));
        }
        return P;
      };
    },
    b() {
      let P = new Decimal(0);
      for (let i = 0; i < 5; i += 1) {
        let part = new Decimal(1);
        for (let j = 0; j < 5; j += 1) {
          part = part.mul(i === j ? this.p(j) : this.q(j));
        }
        P = P.plus(part);
      }
      return P;
    },
    c() {
      let P = new Decimal(0);
      for (let i = 0; i < 4; i += 1) {
        for (let j = i + 1; j < 5; j += 1) {
          let part = new Decimal(1);
          for (let k = 0; k < 5; k += 1) {
            part = part.mul(k === i || k === j ? this.p(k) : this.q(k));
          }
          P = P.plus(part);
        }
      }
      return P;
    },
    d() {
      let P = new Decimal(1);
      for (let i = 0; i < 5; i += 1) {
        P = P.mul(this.p(i));
      }

      for (let i = 0; i < 5; i += 1) {
        let part = new Decimal(1);
        for (let j = 0; j < 5; j += 1) {
          part = part.mul(i === j ? this.q(j) : this.p(j));
        }
        P = P.plus(part);
      }
      return P;
    },
    e() {
      let P = new Decimal(1);
      for (let i = 0; i < 5; i += 1) {
        P = P.mul(this.q(i));
      }
      return new Decimal(1)
        .minus(P);
    },
    C() {
      return (n, m) => this.factorial(n) / this.factorial(m) / this.factorial(n - m);
    },
  },
  methods: {
    factorial(n) {
      return (n !== 1) ? n * this.factorial(n - 1) : 1;
    },
  },
};
</script>

<style scoped>
  .content-answer {
    margin-top: 40px;
  }

  .content-answer > p {
    text-align: left;
    font-size: 18px;
    line-height: 0.7;
  }
</style>
