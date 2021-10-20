<template>
  <div id="people">
    <b-row class="p-3 bg-secondary" align-h="center">
      <b-col align-self="center" cols="6">
        <b-input-group>
          <b-form-input
            placeholder="Search here..."
            type="search"
            v-model="search"
            @keyup="searchBy"
          ></b-form-input>
          <b-input-group-text>
            <b-icon-search></b-icon-search>
          </b-input-group-text>
        </b-input-group>
      </b-col>
    </b-row>
    <b-row>
      <b-col v-bind:cols="cols_viewer">
        <b-container>
          <b-row class="p-2">
            <b-col>
              <b-table
                responsive
                sticky-header
                :busy="isBusy"
                hover
                striped
                :items="filteredData"
                :fields="fields"
              >
                <template #cell(actions)="data">
                  <create-person
                    :id="'model' + data.index"
                    type="udpate"
                    :item="data.item"
                  ></create-person>
                  <b-button size="sm" class="m-1" @click="togglePdf"
                    ><b-icon-paperclip></b-icon-paperclip
                  ></b-button>
                  <b-button
                    size="sm"
                    class="m-1"
                    variant="danger"
                    v-on:click="removeData(data.item.id)"
                  >
                    <b-icon-trash></b-icon-trash>
                  </b-button>
                </template>
              </b-table>
              <create-person type="create" id="modalcrear"></create-person>
              <b-button size="sm" @click="refresh()"
                ><b-icon-arrow-clockwise></b-icon-arrow-clockwise
              ></b-button>
            </b-col>
          </b-row>
        </b-container>
      </b-col>
      <b-col v-if="show_pdf" v-bind:cols="cols_viewer_pdf">
          <b-container class="bg-info">a</b-container>
      </b-col>
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
      isBusy: false,
      search: "",
      show_pdf: false,
      cols_viewer: 12,
      cols_viewer_pdf: 0,
      people: [],
      filteredData: [],
      fields: ["name", "age", "sex", "actions"],
    };
  },
  methods: {
    refresh() {
      this.isBusy = true;
      fetch("http://192.168.0.42:8083/people")
        .then((data) => data.json())
        .then((data) => {
          this.people = data;
          this.searchBy();
          this.isBusy = false;
        });
    },
    removeData(id) {
      fetch("http://192.168.0.42:8083/people/" + id, {
        method: "DELETE",
      });
      this.refresh();
    },
    searchBy() {
      this.filteredData = this.people.filter((d) =>
        d.name.toLowerCase().startsWith(this.search.toLowerCase())
      );
    },
    togglePdf() {
      this.show_pdf = !this.show_pdf;
      if (this.show_pdf) {
        this.cols_viewer = 6;
        this.cols_viewer_pdf = 6;
      } else {
        this.cols_viewer = 12;
        this.cols_viewer_pdf = 0;
      }
    },
  },
  created() {
    this.refresh();

    setInterval(
      function () {
        this.refresh();
      }.bind(this),
      30000
    );
  },
};
</script>