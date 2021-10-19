<template>
  <div id="app">
    <b-nav tabs fill>
      <b-nav-item v-on:click="changetab('home')" v-bind:active="tab['home']"
        >Home</b-nav-item
      >
      <b-nav-item v-on:click="changetab('people')" v-bind:active="tab['people']"
        >People</b-nav-item
      >
      <b-nav-item
        v-on:click="changetab('enterprise')"
        v-bind:active="tab['enterprise']">Enterprises</b-nav-item>
    </b-nav>
    <b-container v-if="tab['home']" fluid>
      <home />
    </b-container>
    <b-container v-if="tab['people']" fluid>
      <people />
    </b-container>
    <b-container v-if="tab['enterprise']" fluid>
      <enterprise />
    </b-container>
  </div>
</template>

<script>
import home from "./components/Home.vue";
import people from "./components/People.vue";
import enterprise from "./components/Enterprise.vue";
export default {
  data: () => ({
    tab: {
      home: false,
      people: true,
      enterprise: false,
    },
  }),
  components: {
    home,
    people,
    enterprise,
  },
  methods: {
    changetab: function (tabval) {
      Object.entries(this.tab).forEach(([key, val]) => {
        if (key == tabval) {
          val = true;
        } else {
          val = false;
        }
        this.tab[key] = val;
      });
    },
  },
};
</script>
