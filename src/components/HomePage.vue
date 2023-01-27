<template>
  <Layout>
    <template #header>
      <Header></Header>
    </template>
    <template #resume>
      <Resume :label="'Ahorro total'" :totalAmount="100000" :amount="amount">
        <template #graphic>
          <GraphicPage :amounts="amounts" />
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
      movements: [
        {
          id: 0,
          title: "Movimiento 1",
          description: "Lorem ipsum dolor sit amet",
          amount: 100,
          time: new Date("01-26-2023"),
        },
        {
          id: 1,
          title: "Movimiento 2",
          description: "Lorem ipsum dolor sit amet",
          amount: 200,
          time: new Date("01-26-2023"),
        },
        {
          id: 2,
          title: "Movimiento 3",
          description: "Lorem ipsum dolor sit amet",
          amount: 500,
          time: new Date("01-26-2023"),
        },
        {
          id: 3,
          title: "Movimiento 4",
          description: "Lorem ipsum dolor sit amet",
          amount: 200,
          time: new Date("01-26-2023"),
        },
        {
          id: 4,
          title: "Movimiento 5",
          description: "Lorem ipsum dolor sit amet",
          amount: -400,
          time: new Date("01-26-2023"),
        },
        {
          id: 5,
          title: "Movimiento 6",
          description: "Lorem ipsum dolor sit amet",
          amount: -600,
          time: new Date("01-26-2023"),
        },
        {
          id: 6,
          title: "Movimiento 7",
          description: "Lorem ipsum dolor sit amet",
          amount: -300,
          time: new Date("01-26-2023"),
        },
        {
          id: 7,
          title: "Movimiento 8",
          description: "Lorem ipsum dolor sit amet",
          amount: 100,
          time: new Date("01-26-2023"),
        },
        {
          id: 8,
          title: "Movimiento 9",
          description: "Lorem ipsum dolor sit amet",
          amount: 300,
          time: new Date("12-26-2022"),
        },
        {
          id: 9,
          title: "Movimiento 10",
          description: "Lorem ipsum dolor sit amet",
          amount: 500,
          time: new Date("12-26-2022"),
        },
      ],
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
        const lastMovements = lastDays.slice(0, i);

        return lastMovements.reduce((suma, movement) => {
          return suma + movement;
        }, 0);
      });
    },
  },
  methods: {
    create(movement) {
      this.movements.push(movement);
    },
    remove(id) {
      const index = this.movements.findIndex((m) => m.id === id);
      this.movements.splice(index, 1);
    },
  },
};
</script>
