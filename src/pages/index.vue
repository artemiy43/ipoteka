<template>
  <!-- <HelloWorld /> -->
  <div class="main__title">
    <h1 class="main__header">
      Ипотечный калькулятор
    </h1>
    <v-img
      class="main__coin"
      src="../assets/ruble-svgrepo-com.svg"
      :width="50"
      :height="50"
    />
  </div>
  <main class="main">
    <div class="main__main-parameters">
      <div>
        <h4>Стоимость недвижимости</h4>
        <v-text-field
          v-model="totalCredit"
          density="compact"
          style="width: 320px; margin: 0"
          type="number"
          hide-details
          single-line
        />
        <v-slider
          v-model="totalCredit"
          :max="20000000"
          :min="1000000"
          :step="100000"
          class="align-center"
          hide-details
          width="320"
        >
          <template #prepend>
            <p>1 млн</p>
          </template>
          <template #append>
            <p>20 млн</p>
          </template>
        </v-slider>
      </div>
      <div>
        <h4>Первоначальный взнос</h4>
        <v-text-field
          v-model="firstPayment"
          density="compact"
          style="width: 320px; margin: 0"
          type="number"
          hide-details
          single-line
        />
        <v-slider
          v-model="firstPayment"
          :max="6000000"
          :min="500000"
          :step="100000"
          class="align-center"
          hide-details
          width="320"
        >
          <template #prepend>
            <p>500 тыс</p>
          </template>
          <template #append>
            <p>6 млн</p>
          </template>
        </v-slider>
      </div>
      <div>
        <h4>Срок кредита</h4>
        <v-text-field
          v-model="totalTime"
          density="compact"
          style="width: 320px; margin: 0"
          type="number"
          hide-details
          single-line
        />
        <v-slider
          v-model="totalTime"
          :max="30"
          :min="1"
          :step="1"
          class="align-center"
          hide-details
          width="320"
        >
          <template #prepend>
            <p>1 год</p>
          </template>
          <template #append>
            <p>30 лет</p>
          </template>
        </v-slider>
      </div>
    </div>
    <div class="main__results">
      <div>
        <h4>Процентная ставка</h4>
        <v-text-field
          v-model="normalPercents"
          density="compact"
          style="width: 320px; margin: 0"
          type="number"
          hide-details
          single-line
        >
          <template #append>
            <p>%</p>
          </template>
        </v-text-field>
      </div>
      <div>
        <p>Сумма кредита:</p>
        <p>{{ totalCredit - firstPayment }} ₽</p>
      </div>
      <div>
        <p>Ежемесячный платёж:</p>
        <p>{{ Math.ceil(monthPayment) }} ₽</p>
      </div>
    </div>
  </main>
</template>

<script setup>
import { computed, ref } from "vue";

//
const totalCredit = ref(1000000);
const firstPayment = ref(500000);
const totalTime = ref(1);
const percents = computed(() => normalPercents.value / 100);
const normalPercents = ref(20);
const hfd = computed(() =>
  Math.pow(1 + percents.value / 12, totalTime.value * 12)
);
const monthPayment = computed(
  () =>
    ((totalCredit.value - firstPayment.value) *
      (percents.value / 12) *
      hfd.value) /
    (hfd.value - 1)
);
</script>

<style lang="scss" scoped>
@mixin centered {
  display: flex;
  justify-content: center;
  align-items: center;
}
.main {
  @include centered();

  flex-direction: row;
  padding: 50px 20px 30px;
  gap: 70px;
}
.main__title {
  @include centered();
  flex-direction: column;
}
.main__header {
  text-align: center;
  padding: 50px 30px 10px;
}
.main__main-parameters {
  @include centered();
  flex-direction: column;
  gap: 50px;
}
.main__coin {
  margin: 0;
  padding: 0;
}
.main__results {
  @include centered();
  flex-direction: column;
  gap: 15px;
}

@media (max-width: 1000px) {
  .main {
    flex-direction: column;
    padding: 10px;
  }
}
@media (max-width: 450px) {
  .main {
    padding: 0;
    padding-bottom: 30px;
    margin: 0;
    text-align: center;
  }
  .main__header {
    text-align: center;
    padding: 20px;
  }
}
</style>
