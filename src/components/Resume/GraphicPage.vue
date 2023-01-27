<template>
  <div>
    <!-- 
      x=horizontal
      y=vertical
      0 0 = esquina superior izquierda
      300 200 = 300 ancho y 200 alto
     -->
    <svg
      @touchstart="tap"
      @touchmove="tap"
      @touchend="untap"
      viewBox="0 0 300 200"
    >
      <!-- 
        x1 = inicio 0 = izquierda horizontal
        y1 = inicio 100 = mitad de altura de 200
        x2 = final 300 = derecha horizontal
        y2 = mantiene 100 = mitad de altura de 200
     -->
      <line
        stroke="#c4c4c4"
        stroke-width="2"
        x1="0"
        :y1="zero || 0"
        x2="300"
        :y2="zero || 0"
      />
      <polyline
        fill="none"
        stroke="#0686b0"
        stroke-width="2"
        :points="points"
      />
      <line
        v-show="showPointer"
        stroke="#04b500"
        stroke-width="2"
        :x1="pointer || 0"
        y1="0"
        :x2="pointer || 0"
        y2="200"
      />
    </svg>
    <p>Últimos 30 días</p>
  </div>
</template>

<script setup>
import { toRefs, defineProps, watch, defineEmits, computed, ref } from "vue";

const props = defineProps({
  amounts: {
    type: Array,
    defaul: () => [],
  },
});

const { amounts } = toRefs(props);

const amountToPixels = (amount) => {
  const min = Math.min(...amounts.value);
  const max = Math.max(...amounts.value);

  const amountAbs = amount + Math.abs(min);
  const minmax = Math.abs(max) + Math.abs(min);

  return 200 - ((amountAbs * 100) / minmax) * 2;
};

const zero = computed(() => {
  return amountToPixels(0);
});

const points = computed(() => {
  const total = amounts.value.length;
  if (total > 0) {
    return amounts.value.reduce((points, amount, i) => {
      const x = (300 / total) * (i + 1);
      const y = amountToPixels(amount);
      return `${points} ${x},${y}`;
    }, `0, ${amountToPixels(amounts.value.length ? amounts.value[0] : 0)}`);
  } else {
    return "0, 0";
  }
});

const showPointer = ref(false);
const pointer = ref(0);

const emit = defineEmits(["select"]); //REVISAR ESTE PUNTO, QUEDO INCONCLUSO
watch(pointer, (value) => {
  const index = Math.ceil(value / (300 / amounts.value.length));
  if (index < 0 || index > amounts.value.length) return;
  emit("select", amounts.value[index - 1]);
});

const tap = ({ target, touches }) => {
  showPointer.value = true;
  const elementWidth = target.getBoundingClientRect().width;
  const elementX = target.getBoundingClientRect().x;
  const touchX = touches[0].clientX;
  pointer.value = ((touchX - elementX) * 300) / elementWidth;
  //emit("select", amounts); -REVISAR ESTE PUNTO, QUEDO INCONCLUSO
};

const untap = () => {
  showPointer.value = false;
};
</script>

<style scoped>
svg {
  width: 100%;
}
p {
  text-align: center;
}
</style>
