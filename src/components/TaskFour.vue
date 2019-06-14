<template>
  <div>
    <div class="name-laba">
      <h1>ЛАБОРАТОРНАЯ РАБОТА № 2 ТЕОРЕМЫ СЛОЖЕНИЯ И УМНОЖЕНИЯ ВЕРОЯТНОСТЕЙ. <br> ФОРМУЛЫ ПОЛНОЙ
        ВЕРОЯТНОСТИ И БАЙЕСА</h1>
      <p>Из X стрелков Y<sub>1</sub> попадает в цель с вероятностью Z<sub>1</sub>, Y<sub>2</sub> -
        с вероятностью Z<sub>2</sub> , ... , Y<sub>n</sub> – с вероятностью Z<sub>n</sub>. <br>
        Наудачу выбранный стрелок произвел выстрел, поразив цель.<br>
        К какой из групп вероятнее всего принадлежал этот стрелок?</p>
    </div>
    <input-number label="Количество стрелков X - "
                 min="1"
                 max="9999999"
                 step="1"
                 v-model="shooterCount"/>
    <input-number label="Количество групп стрелков N - "
                 min="1"
                 max="9999999"
                 step="1"
                 v-model="groupNumber"/>
    <ul>
      <li v-for="n in groupNumber" :key="n">
        <input-number :label="`Из ${shooterCount} стрелков Y<sub>${n}</sub> - `"
                     step="1"
                     min="1"
                     max="9999999"
                     v-model="groupShootersCount[n - 1]"
                     style="display: inline-block; margin-right: 5px"/>

        <input-number :label="`попадают в цель с вероятностью Z<sub>${n}</sub> - `"
                     v-model="groupProbability[n - 1]"
                     style="display: inline-block"/>
      </li>
    </ul>
    <div class="content-answer">
      <p>Вероятнее всего стрелок будет принадлежать группе под номером {{biggest}}</p>
    </div>
  </div>
</template>

<script>
import { Decimal } from 'decimal.js';
import InputNumber from './InputNumber.vue';

export default {
  name: 'TaskFour',
  components: { InputNumber },
  data() {
    return {
      shooterCount: 30,
      groupNumber: 3,
      groupProbability: [
        0.6,
        0.5,
        0.7,
      ],
      groupShootersCount: [
        12,
        8,
        10,
      ],
    };
  },
  computed: {
    c() {
      return i => new Decimal(this.groupShootersCount[i] ? this.groupShootersCount[i] : 1);
    },
    p() {
      return i => new Decimal(this.groupProbability[i] ? this.groupProbability[i] : 1);
    },
    h() {
      return i => this.c(i)
        .div(this.shooterCount);
    },
    P() {
      let P = new Decimal(0);

      for (let i = 0; i < this.groupNumber; i += 1) {
        P = P.plus(this.h(i)
          .mul(this.p(i)));
      }

      return P;
    },
    r() {
      return i => new Decimal(this.h(i))
        .mul(this.p(i))
        .div(this.P);
    },
    biggest() {
      let biggest = 0;

      for (let i = 1; i < this.groupNumber; i += 1) {
        if (this.r(i) > this.r(biggest)) biggest = i;
      }

      return biggest + 1;
    },
  },
  methods: {},
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
