<template>
  <Layout>
    <template #header>
      <Header></Header>
    </template>
    <template #resume>
      <Resume
        :label="'Ahorro total'"
        :totalAmount="totalAmount"
        :amount="amount"
      >
        <template #graphic>
          <GraphicPage :amounts="amounts" @select="select" />
        </template>
        <template #action>
          <ActionPage @create="create" />
        </template>
      </Resume>
    </template>
    <template #movements>
      <Movements :movements="movements" @remove="remove" />
    </template>
  </Layout>
</template>

<script>
import Layout from "./LayoutPage.vue";
import Header from "./HeaderPage.vue";
import ActionPage from "./ActionPage.vue";
import Resume from "./Resume/IndexPage.vue";
import Movements from "./Movements/IndexPage.vue";
import GraphicPage from "./Resume/GraphicPage.vue";

export default {
  components: {
    Layout,
    Header,
    Resume,
    Movements,
    ActionPage,
    GraphicPage,
  },
  data() {
    return {
      amount: null,
      label: null,
      movements: [],
    };
  },
  computed: {
    amounts() {
      const lastDays = this.movements
        .filter((m) => {
          const today = new Date();
          const oldDate = today.setDate(today.getDate() - 30);
          return m.time > oldDate;
        })
        .map((m) => m.amount);

      return lastDays.map((m, i) => {
        const lastMovements = lastDays.slice(0, i + 1);

        return lastMovements.reduce((suma, movement) => {
          return suma + movement;
        }, 0);
      });
    },
    totalAmount() {
      return this.movements.reduce((suma, m) => {
        return suma + m.amount;
      }, 0);
    },
  },
  mounted() {
    const movements = JSON.parse(localStorage.getItem("movements"));

    if (Array.isArray(movements)) {
      this.movements = movements?.map((m) => {
        return { ...m, time: new Date(m.time) };
      });
    }
  },
  methods: {
    create(movement) {
      this.movements.push(movement);
      this.save();
    },
    remove(id) {
      const index = this.movements.findIndex((m) => m.id === id);
      this.movements.splice(index, 1);
      this.save();
    },
    save() {
      localStorage.setItem("movements", JSON.stringify(this.movements));
    },
    select(el) {
      this.amount = el;
    },
  },
};
</script>
