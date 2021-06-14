<template>
  <div>
    <h1>Berikut adalah semua pengguna kita</h1>
    <ul>
      <li v-for="item in users" :key="item.id">
        {{ item.username }}<button @click="hapus(item.id)">X</button>
      </li>
    </ul>
    <input v-model="username" />
    <input v-model="password" />
    <button @click="tambah">Add</button>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data: function () {
    return {
      users: [],
      username: "",
      password: "",
    };
  },
  created: function () {
    const username = localStorage.getItem("usr");
    const password = localStorage.getItem("pwd");
    axios
      .get("http://localhost:3000/user", { headers: { username, password } })
      .then((result) => {
        this.users = result.data;
      });
  },
  methods: {
    tambah: function () {
      const newItem = { username: this.username, password: this.password };
      const username = localStorage.getItem("usr");
      const password = localStorage.getItem("pwd");
      axios.post("http://localhost:3000/user", newItem, {
        headers: { username, password },
      });
      this.users.push(newItem);
    },
    hapus: function (id) {
      const username = localStorage.getItem("usr");
      const password = localStorage.getItem("pwd");
      axios.delete(`http://localhost:3000/user/${id}`, {
        headers: { username, password },
      });
      this.users.splice(id - 1, 1);
    },
  },
};
</script>

