<template>
  <div id="home">
    <b-row>
      <b-col md="auto">
        <b-calendar
          v-model="value"
          @context="onContext"
          locale="en-US"
        ></b-calendar>
      </b-col>
    </b-row>
  </div>
</template>

<script>
export default {
  name: "home",
  data() {
    return {
      value: "",
      context: null,
    };
  },
  methods: {
    onContext(ctx) {
      this.context = ctx;
    },
  },
};
</script>
