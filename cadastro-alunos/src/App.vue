<template>
  <div class="app-wrapper">
    <!-- Tela A / B - Lista de alunos ou mensagem vazia -->
    <div class="panel list-panel">
      <div class="panel-header">
        <span class="panel-title">Total de alunos: {{ alunos.length }}</span>
        <button class="btn btn-success" @click="novoAluno">+ Novo aluno</button>
        <button class="btn btn-secondary" @click="mostrarForm = !mostrarForm">Mostrar/Ocultar formulário</button>
      </div>

      <div class="panel-body">
        <ul v-if="alunos.length > 0" class="student-list">
          <li v-for="(a, i) in alunos" :key="a.id" class="student-item">
            <span>#{{ i + 1 }} — {{ a.nome }} | {{ a.matricula }} | {{ a.curso }} | {{ a.ativo ? 'Ativo' : 'Inativo' }}</span>
            <div class="actions">
              <button class="btn btn-warning" @click="editar(a)">Editar</button>
              <button class="btn btn-danger" @click="excluir(a.id)">Excluir</button>
            </div>
          </li>
        </ul>
        <div v-else class="empty-list">
          <p>"Nenhum aluno cadastrado ainda. Cadastre o primeiro!"</p>
          <button class="btn btn-success" @click="novoAluno">+ Novo aluno</button>
        </div>
      </div>
    </div>

    <!-- Tela C - Formulário -->
    <div v-show="mostrarForm" class="panel form-panel">
      <div class="panel-header">
        <h2 class="panel-title" v-if="editandoId === null">Cadastrar Aluno</h2>
        <h2 class="panel-title" v-else>Editar Aluno</h2>
      </div>
      
      <div class="panel-body">
        <div class="form-group">
          <label>Nome:</label>
          <input v-model="nome" type="text" placeholder="Maria Souza">
        </div>
        
        <div class="form-group">
          <label>Matrícula:</label>
          <input v-model="matricula" type="text" placeholder="2026001">
        </div>
        
        <div class="form-group">
          <label>Curso:</label>
          <input v-model="curso" type="text" placeholder="Sistemas de Informação">
        </div>
        
        <div class="form-group checkbox-group">
          <label>Ativo:</label>
          <input v-model="ativo" type="checkbox">
        </div>
        
        <div class="form-actions">
          <button class="btn btn-success" @click="salvar">Salvar</button>
          <button class="btn btn-secondary" @click="limparFormulario">Cancelar</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue';

const alunos = ref([
  { id: 1, nome: 'Maria Souza', matricula: '2026001', curso: 'Sistemas de Informação', ativo: true },
  { id: 2, nome: 'João Lima', matricula: '2026002', curso: 'Sistemas de Informação', ativo: false },
  { id: 3, nome: 'Ana Pires', matricula: '2026003', curso: 'Sistemas de Informação', ativo: true }
]);
const mostrarForm = ref(false);
const editandoId = ref(null);

const nome = ref('');
const matricula = ref('');
const curso = ref('');
const ativo = ref(true);

function novoAluno() {
  limpar();
  mostrarForm.value = true;
}

function salvar() {
  if (!nome.value || !matricula.value || !curso.value) {
    alert("Preencha todos os campos obrigatórios (nome, matrícula e curso).");
    return;
  }

  if (editandoId.value === null) {
    // Create
    alunos.value.push({
      id: Date.now(),
      nome: nome.value,
      matricula: matricula.value,
      curso: curso.value,
      ativo: ativo.value
    });
  } else {
    // Update
    const index = alunos.value.findIndex(a => a.id === editandoId.value);
    if (index !== -1) {
      alunos.value[index] = {
        id: editandoId.value,
        nome: nome.value,
        matricula: matricula.value,
        curso: curso.value,
        ativo: ativo.value
      };
    }
  }
  limparFormulario();
}

function editar(a) {
  editandoId.value = a.id;
  nome.value = a.nome;
  matricula.value = a.matricula;
  curso.value = a.curso;
  ativo.value = a.ativo;
  mostrarForm.value = true;
}

function excluir(id) {
  if (confirm("Tem certeza que deseja excluir este aluno?")) {
    // Delete
    alunos.value = alunos.value.filter(a => a.id !== id);
  }
}

function limpar() {
  editandoId.value = null;
  nome.value = '';
  matricula.value = '';
  curso.value = '';
  ativo.value = true;
}

function limparFormulario() {
  limpar();
  mostrarForm.value = false;
}
</script>

<style>
* {
  box-sizing: border-box;
}

body {
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
  background-color: #f5f7fa;
  margin: 0;
  padding: 40px 20px;
}

.app-wrapper {
  max-width: 800px;
  margin: 0 auto;
}

.panel {
  background: white;
  border-radius: 8px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  overflow: hidden;
  margin-bottom: 20px;
}

.panel-header {
  background-color: #34495e;
  color: white;
  padding: 15px 20px;
  display: flex;
  align-items: center;
  gap: 10px;
}

.panel-title {
  font-weight: bold;
  margin: 0;
  font-size: 16px;
  margin-right: auto;
}

.panel-body {
  padding: 20px;
}

.btn {
  padding: 8px 16px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 14px;
  font-weight: 500;
  color: white;
  transition: background-color 0.2s;
}

.btn-success { background-color: #2ecc71; }
.btn-success:hover { background-color: #27ae60; }

.btn-secondary { background-color: #7f8c8d; }
.btn-secondary:hover { background-color: #95a5a6; }

.btn-warning { background-color: #f1c40f; color: #333; }
.btn-warning:hover { background-color: #f39c12; }

.btn-danger { background-color: #e74c3c; }
.btn-danger:hover { background-color: #c0392b; }

.student-list {
  list-style: none;
  padding: 0;
  margin: 0;
}

.student-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 15px;
  border: 1px solid #ecf0f1;
  margin-bottom: 10px;
  border-radius: 6px;
  background-color: #fcfcfc;
}

.student-item:last-child {
  margin-bottom: 0;
}

.actions {
  display: flex;
  gap: 8px;
}

.empty-list {
  text-align: center;
  padding: 30px;
  color: #7f8c8d;
}

.empty-list p {
  font-style: italic;
  margin-bottom: 20px;
  font-size: 16px;
}

.form-group {
  margin-bottom: 15px;
  display: flex;
  align-items: center;
}

.form-group label {
  width: 100px;
  font-weight: 600;
  color: #2c3e50;
}

.form-group input[type="text"] {
  flex: 1;
  padding: 10px;
  border: 1px solid #bdc3c7;
  border-radius: 4px;
  font-size: 14px;
}

.form-group input[type="text"]:focus {
  outline: none;
  border-color: #3498db;
  box-shadow: 0 0 0 2px rgba(52, 152, 219, 0.2);
}

.checkbox-group {
  justify-content: flex-start;
}

.checkbox-group input[type="checkbox"] {
  margin-left: 0;
  width: 18px;
  height: 18px;
  cursor: pointer;
}

.form-actions {
  margin-top: 25px;
  display: flex;
  gap: 10px;
}
</style>
