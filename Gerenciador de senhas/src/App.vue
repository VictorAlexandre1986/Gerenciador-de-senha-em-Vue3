<template>
  <div id="app">
    <div class="titulo">
      <h1>Gerenciador de Senhas</h1>
    </div>
    <div class="container">
      <h2>Cadastro de Senhas</h2>
        <div class="form">
            <form @submit.prevent="savePassword">
            <input v-model="formData.company" type="text" placeholder="Empresa" />
            <input v-model="formData.username" type="text" placeholder="Usuário" />
            <input v-model="formData.password" type="text" placeholder="Senha" />
            <button type="submit">{{ isEditing.value ? 'Salvar' : 'Adicionar' }}</button>
          </form>
        </div>
        <div class="table">
        <table>
          <thead>
            <tr>
              <th>Empresa</th>
              <th>Usuário</th>
              <th>Senha</th>
              <th>Ações</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(entry, index) in passwords" :key="index">
              <td>{{ entry.company }}</td>
              <td>{{ entry.username }}</td>
              <td>{{ entry.password }}</td>
              <td>
                <button class="edit" @click="editPassword(index)">Editar</button>
                <button class="remove" @click="removePassword(index)">Remover</button>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
      <div class="footer">
          <p>&copy; 2024 Gerenciador de Senhas. Todos os direitos reservados.</p>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';

const formData = ref({
  company: '',
  username: '',
  password: ''
});
const passwords = ref([]);
const editingIndex = ref(null);
const isEditing = ref(false);

const loadPasswords = () => {
  const storedPasswords = JSON.parse(localStorage.getItem('passwords')) || [];
  passwords.value = storedPasswords;
};

const savePasswords = () => {
  localStorage.setItem('passwords', JSON.stringify(passwords.value));
};

const savePassword = () => {
  if (!formData.value.company.trim() || !formData.value.username.trim() || !formData.value.password.trim()) return;

  if (isEditing.value) {
    passwords.value[editingIndex.value] = { ...formData.value };
    isEditing.value = false;
    editingIndex.value = null;
  } else {
    passwords.value.push({ ...formData.value });
  }

  formData.value = { company: '', username: '', password: '' };
  savePasswords();
};

const removePassword = (index) => {
  passwords.value.splice(index, 1);
  savePasswords();
};

const editPassword = (index) => {
  formData.value = { ...passwords.value[index] };
  isEditing.value = true;
  editingIndex.value = index;
};

onMounted(loadPasswords);
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  text-align: center;
  color: #2c3e50;
  display: flex;
  flex-direction: column;
  /* align-items: space-around;
  justify-content: space-around; */
  height: 100vh;
}

.titulo{
  border-width: 0px 0px 1px 0px; /* top right bottom left */
  border-style: solid; /* Define o estilo da borda */
  border-color: #2c3e50; /* Define a cor da borda */

  margin-top:1rem;
  margin-bottom: 20px;;
}

.container h2{
  margin-bottom: 1.2rem;
}

.container {
  display: flex;
  flex-direction: column;
  align-items: space-around;
  justify-content: space-around;
  margin-top: 5rem 
}

.table {
  margin-top: 3rem;;
}

.footer{
  margin-top: 3rem;

}

form input {
  margin: 0 5px;
  padding: 8px;
}

table {
  width: 100%;
  border-collapse: collapse;
  margin-top: 20px;
}

th{
  border: 1px solid #ddd;
  padding: 8px;
  
}
td{
  border: 1px solid #ddd;
  padding: 8px;
  
} 

th {
  background-color: #f4f4f4;
}

.remove{
  background-color: #FF6347;
}

.edit{
  background-color: #20B2AA;
}
button {
  margin: 0 5px;
  background-color: #8A2BE2;
  height: 2rem;
  border: none;
  color: #fff;
}
</style>
