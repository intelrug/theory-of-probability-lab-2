<template>
  <div class="scheme-content">
    <div class="name-laba">
      <h1>ЛАБОРАТОРНАЯ РАБОТА № 2 ТЕОРЕМЫ СЛОЖЕНИЯ И УМНОЖЕНИЯ ВЕРОЯТНОСТЕЙ. <br> ФОРМУЛЫ ПОЛНОЙ
        ВЕРОЯТНОСТИ И БАЙЕСА</h1>
      <p>Найти вероятность безотказной работы схемы, приведенной на рисунке, считая, что отказы
        отдельных элементов независимы и вероятность отказа элемента с номером i равна qi.</p>
    </div>
    <div class="scheme-block scheme__main-unit">
      <div class="scheme__main-unit__left">
        <div class="main-unit__left-dot">
          <div class="scheme__additional-unit scheme__additional-unit_5 scheme-block">
            <input-number id="first" v-model="probability[1]" :label="1"/>
            <div class="scheme__additional-unit_5__dot"></div>
          </div>
        </div>
      </div>
      <div class="scheme__additional-unit scheme__additional-unit_1 scheme-block">
        <input-number id="second" v-model="probability[2]" :label="2"/>
      </div>
      <div class="scheme__additional-unit scheme__additional-unit_2 scheme-block">
        <input-number id="fourth" v-model="probability[4]" :label="4"/>
      </div>
      <div class="scheme__additional-unit scheme__additional-unit_3 scheme-block">
        <input-number id="fifth" v-model="probability[5]" :label="5"/>
      </div>
      <div class="scheme__additional-unit scheme__additional-unit_4 scheme-block">
        <input-number id="sixth" v-model="probability[6]" :label="6"/>
      </div>
      <div class="scheme__main-unit__right">
        <div class="main-unit__right-dot">
          <div class="scheme__additional-unit scheme__additional-unit_6 scheme-block">
            <input-number id="third" v-model="probability[3]" :label="3"/>
            <div class="scheme__additional-unit_6__dot"></div>
          </div>
        </div>
      </div>
    </div>
    <p><br><br><br>Вероятность безотказной работы схемы: {{result}}</p>
  </div>
</template>

<script>
import { Decimal } from 'decimal.js';
import InputNumber from './InputNumber.vue';

export default {
  name: 'TaskOne',
  components: { InputNumber },
  data() {
    return {
      probability: [
        0,
        0.1,
        0.2,
        0.3,
        0.4,
        0.3,
        0.2,
      ],
    };
  },
  computed: {
    q() {
      return i => new Decimal(this.probability[i]);
    },
    p() {
      return i => new Decimal(1).minus(this.q(i));
    },
    result() {
      // p1 * p3 * (1 - q2 * (1 - p4 * p5 * p6))
      return new Decimal(this.p(4))
        .mul(this.p(5))
        .mul(this.p(6))
        .neg()
        .plus(1)
        .mul(this.q(2))
        .neg()
        .plus(1)
        .mul(this.p(3))
        .mul(this.p(1));
    },
  },
};
</script>

<style scoped>

</style>
