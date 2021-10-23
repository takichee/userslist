<template>
  <div id="app">
    <h1>連絡先アプリ</h1>
    <div class="new">
      <h2>新規作成</h2>
      <div class="name">
        <label for="name">お名前：</label>
        <input id="name" v-model="newName" type="text" name="name">
      </div>
      <div class="email">
        <label for="email">メールアドレス：</label>
        <input id="email" v-model="newEmail" type="email" name="email">
      </div>
      <button @click="insertContact">
        新規作成
      </button>
    </div>
    <div class="table">
      <h2>連絡先リスト</h2>
      <table>
        <tr>
          <th>ID</th>
          <th>NAME</th>
          <th>EMAIL</th>
          <th>UPDATE</th>
          <th>DELETE</th>
        </tr>
        <tr v-for="item in contactLists" :key="item.id">
          <td>{{ item.id }}</td>
          <td><input v-model="item.name" type="text"></td>
          <td><input v-model="item.email" type="email"></td>
          <td>
            <button @click="updateContact(item.id, item.name, item.email)">
              更新
            </button>
          </td>
          <td>
            <button @click="deleteContact(item.id)">
              削除
            </button>
          </td>
        </tr>
      </table>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      newName: '',
      newEmail: '',
      contactLists: []
    }
  },
  methods: {
    async getContact () {
      const resData = await this.$axios.get(
        'http://127.0.0.1:8000/api/v1/contact/'
      )
      this.contactLists = resData.data.data
    },
    async insertContact () {
      const sendData = {
        name: this.newName,
        email: this.newEmail
      }
      await this.$axios.post('http://127.0.0.1:8000/api/v1/contact/', sendData)
      this.getContact()
    },
    async updateContact (id, name, email) {
      const sendData = {
        name,
        email
      }
      await this.$axios.put(
        'http://127.0.0.1:8000/api/v1/contact/' + id,
        sendData
      )
      this.getContact()
    },
    async deleteContact (id) {
      await this.$axios.delete('http://127.0.0.1:8000/api/v1/contact/' + id)
      this.getContact()
    },
    created () {
      this.getContact()
    }
  }
}
</script>

<style>
table,td,th {
  border: 1px solid #000;
  border-collapse: collapse;
  text-align: center;
}
td,th {
  padding: 5px;
}
th {
  background: #f0e6cc;
}
</style>
