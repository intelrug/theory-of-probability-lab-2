<template>
    <div class="scheme-content">
      <div class="name-laba">
        <h1>ЛАБОРАТОРНАЯ РАБОТА № 2 ТЕОРЕМЫ СЛОЖЕНИЯ И УМНОЖЕНИЯ ВЕРОЯТНОСТЕЙ. <br> ФОРМУЛЫ ПОЛНОЙ
          ВЕРОЯТНОСТИ И БАЙЕСА</h1>
        <p>Найти вероятность безотказной работы схемы, приведенной на рисунке, считая, что отказы
          отдельных элементов независимы и вероятность отказа элемента с номером i равна qi.<br>
          Пример своей цепи </p>
      </div>
      <div class="scheme-block scheme__main-unit">
        <div class="scheme__main-unit__left">
          <div class="main-unit__left-dot">
            <div class="scheme__additional-unit scheme__additional-unit_1 scheme-block">
              <input-number id="first" v-model="probability[1]" :label="1"/>
              <div class="scheme__additional-unit_1__dot"></div>
            </div>
            <div class="scheme__additional-unit scheme__additional-unit_2 scheme-block">
              <input-number id="second" v-model="probability[2]" :label="2"/>
            </div>
          </div>
        </div>
        <div class="scheme__additional-unit scheme__additional-unit_3 scheme-block">
          <input-number id="third" v-model="probability[3]" :label="3"/>
        </div>
        <div class="scheme__additional-unit scheme__additional-unit_4 scheme-block">
          <input-number id="fourth" v-model="probability[4]" :label="4"/>
        </div>
        <div class="scheme__main-unit__right">
          <div class="main-unit__right-dot">
            <div class="scheme__additional-unit scheme__additional-unit_5 scheme-block">
              <input-number id="fifth" v-model="probability[5]" :label="5"/>
            </div>
            <div class="scheme__additional-unit scheme__additional-unit_6 scheme-block">
              <input-number id="sixth" v-model="probability[6]" :label="6"/>
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
  name: 'TaskTwo',
  components: { InputNumber },
  data() {
    return {
      probability: [
        0.8,
        0.7,
        0.5,
        0.1,
        0.3,
        0.2,
      ],
    };
  },
  computed: {
    p() {
      return i => new Decimal(1).minus(this.q(i));
    },
    q() {
      return i => new Decimal(this.probability[i - 1]);
    },
    result() {
      // p1 * p2 * (1 - q3 * q4) * p5 * p6
      return new Decimal(this.q(3))
        .mul(this.q(4))
        .neg()
        .plus(1)
        .mul(this.p(1))
        .mul(this.p(2))
        .mul(this.p(5))
        .mul(this.p(6));
    },
  },
};
</script>

<style scoped>
  .scheme-content {
    flex-direction: column;
    display: flex;
    align-items: center;
  }

  .scheme-block {
    background: #ffffff;
    border: 2px solid #000000;
  }

  .scheme__main-unit {
    width: 150px;
    height: 150px;
    position: relative;
  }

  .scheme__additional-unit {
    position: absolute;
    width: 60px;
    height: 30px;
  }

  .scheme__additional-unit_1 {
    top: -10px;
    left: -220px;
  }

  .scheme__additional-unit_2 {
    top: -10px;
    left: -90px;
  }

  .scheme__additional-unit_1:before,
  .scheme__additional-unit_2:before,
  .scheme__additional-unit_5:before,
  .scheme__additional-unit_6:before {
    content: '';
    position: absolute;
    width: 66px;
    height: 2px;
    background: #000000;
    display: block;
    top: 50%;
    margin-top: -1px;
    left: -68px;
  }

  .scheme__additional-unit_1:after,
  .scheme__additional-unit_2:after,
  .scheme__additional-unit_5:after,
  .scheme__additional-unit_6:after {
    content: '';
    position: absolute;
    width: 66px;
    height: 2px;
    background: #000000;
    display: block;
    top: 50%;
    margin-top: -1px;
    left: 58px;
  }

  .scheme__additional-unit_3 {
    top: -16px;
    left: 50%;
    margin-left: -30px;
  }

  .scheme__additional-unit_4 {
    bottom: -16px;
    left: 50%;
    margin-left: -30px;
  }

  .scheme__additional-unit_5 {
    top: -10px;
    left: 70px;
  }

  .scheme__additional-unit_6__dot {
    width: 13px;
    height: 13px;
    background: #ffffff;
    border: 1px solid #000000;
    border-radius: 50%;
    position: absolute;
    top: 50%;
    margin-top: -7px;
    left: 120px;
    z-index: 1;
  }

  .scheme__additional-unit_6__dot:after {
    width: 20px;
    height: 1px;
    background: #000000;
    content: '';
    position: absolute;
    transform: rotate(115deg);
    top: 50%;
    left: -4px;
    margin-top: -1px;
  }

  .scheme__additional-unit_6 {
    top: -10px;
    right: -250px;
  }

  .scheme__additional-unit_1__dot {
    width: 13px;
    height: 13px;
    background: #ffffff;
    border: 1px solid #000000;
    border-radius: 50%;
    position: absolute;
    top: 50%;
    margin-top: -7px;
    right: 120px;
    z-index: 1;
  }

  .scheme__additional-unit_1__dot:after {
    width: 20px;
    height: 1px;
    background: #000000;
    content: '';
    position: absolute;
    transform: rotate(115deg);
    top: 50%;
    margin-top: -1px;
    right: -5px;
  }

  .scheme-content .scheme__input {
    width: 100%;
    height: 100%;
    border: 0;
    text-align: center;
    padding: 0 3px;
  }

  .scheme-content .scheme__label {
    position: absolute;
    top: -24px;
    font-weight: bold;
    left: 50%;
    margin-left: -3px;
    font-family: Arial, sans-serif;
  }

  .main-unit__left-dot {
    width: 10px;
    height: 10px;
    background: #000000;
    position: absolute;
    border-radius: 50%;
    left: -6px;
    top: 50%;
    margin-top: -5px;
  }

  .main-unit__right-dot {
    width: 10px;
    height: 10px;
    background: #000000;
    position: absolute;
    border-radius: 50%;
    right: -6px;
    top: 50%;
    margin-top: -6px;
  }
</style>
