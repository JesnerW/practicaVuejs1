<template>
  <header>
    <img @click="reload" src="@/assets/cashflow-icon.svg" alt="logo" />
    <h1><span>Cash</span>Flow</h1>
    <span></span>
  </header>
</template>

<script>
export default {
  methods: {
    reload() {
      location.reload();
    },
  },
};
</script>

<style scope>
header {
  display: flex;
  justify-content: space-around;
  align-items: center;
  width: 50vw;
}
img,
span {
  width: 48px;
}
h1 {
  font-size: 1.4rem;
  color: var(--brand-blue);
}
h1 span {
  color: var(--brand-green);
}
@media (max-width: 700px) {
  header {
    width: 100vw;
  }
}
</style>
