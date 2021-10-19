<template>
  <div id="people">
    <b-row class="p-3 bg-secondary" align-h="center">
      <b-col align-self="center" cols="6">
        <b-input-group>
          <b-form-input
            placeholder="Search here..."
            type="search"
            v-model="search"
          ></b-form-input>
          <b-input-group-text>
            <b-icon-search></b-icon-search>
          </b-input-group-text>
        </b-input-group>
      </b-col>
    </b-row>
    <b-row class="p-2">
      <b-col v-bind:cols="cols_viewer">
        <b-table hover striped :items="people" :fields="fields">
          <template #cell(actions)="data">
            <create-person
              :id="'model' + data.index"
              type="udpate"
              :item="data.item"
            ></create-person>
            <b-button class="m-1" v-on:click="removeData(data.item.id)">
              <b-icon-trash></b-icon-trash>
            </b-button>
          </template>
        </b-table>
        <create-person type="create" id="modalcrear"></create-person>
      </b-col>
      <b-col v-bind:cols="cols_viewer_pdf" v-if="show_pdf"> </b-col>
    </b-row>
  </div>
</template> 
<script>
import CreatePerson from "./CreatePerson.vue";

export default {
  name: "people",
  components: {
    CreatePerson,
  },
  data() {
    return {
      search: "",
      show_pdf: false,
      cols_viewer: 12,
      cols_viewer_pdf: 0,
      people: [],
      fields: ["name", "age", "sex", "actions"],
    };
  },
  methods: {
    refresh() {
      fetch("http://192.168.0.42:8083/people")
        .then((data) => data.json())
        .then((data) => {
          this.people = data;
        });
    },
    removeData(id) {
      fetch("http://192.168.0.42:8083/people/" + id, {
        method: "DELETE",
      });
      this.refresh();
    },
  },
  created() {
    this.refresh();

    setInterval(
      function () {
        this.refresh();
      }.bind(this),
      5000
    );
  },
};
</script>