<template>
  <span>
    <b-button class="m-2" v-b-modal="id">
      <template v-if="this.type == 'create'">Create Person </template>
      <template v-if="this.type != 'create'">
        <b-icon-pencil></b-icon-pencil>
      </template>
    </b-button>
    <b-modal :id="id" :title="title" @ok="handleOk">
      <b-form-group label="Name">
        <b-form-input type="text" v-model="name"></b-form-input>
      </b-form-group>
      <b-form-group label="Age">
        <b-form-input type="number" v-model="age"></b-form-input>
      </b-form-group>
      <b-form-group label="Sex">
        <b-form-select v-model="sex" :options="options"></b-form-select>
      </b-form-group>
    </b-modal>
  </span>
</template>

<script>
export default {
  name: "createperson",
  props: {
    id: String,
    type: {
      type: String,
      default: "create",
    },
    item: {
      type: Object,
      default() {
        return {
          name: "",
          age: "",
          sex: "",
        };
      },
    },
  },
  data() {
    return {
      title: this.type == "create" ? "Create Person" : "Update Person",
      name: this.type == "create" ? "" : this.item.name,
      age: this.type == "create" ? "" : this.item.age,
      sex: this.type == "create" ? "" : this.item.sex,
      options: [
        { value: "0", text: "Femenino" },
        { value: "1", text: "Masculino" },
      ],
    };
  },
  methods: {
    handleOk() {
      fetch("http://192.168.0.42:8083/people", {
        method: this.type == "create" ? "POST" : "PUT",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify({
          name: this.name,
          age: this.age,
          sex: this.sex,
        }),
      })
        .then((data) => data.json())
        .then((data) => console.log(data));
    },
  },
};
</script>
