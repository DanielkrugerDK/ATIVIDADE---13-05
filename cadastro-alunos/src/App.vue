<template>
  <div class="container">
    <h1>Cadastro de Alunos</h1>
    <p>Total de alunos: {{ alunos.length }}</p>
    <button @click="mostrarForm = !mostrarForm" class="btn-toggle">Mostrar/Ocultar formulário</button>

    <div v-show="mostrarForm" class="formulario">
      <h2 v-if="editandoId === null">Cadastrar Aluno</h2>
      <h2 v-else>Editar Aluno</h2>
      
      <div class="campo">
        <label>Nome: </label>
        <input v-model="nome" type="text" placeholder="Nome do aluno">
      </div>
      <div class="campo">
        <label>Matrícula: </label>
        <input v-model="matricula" type="text" placeholder="Ex: 2026001">
      </div>
      <div class="campo">
        <label>Curso: </label>
        <input v-model="curso" type="text" placeholder="Sistemas de Informação">
      </div>
      <div class="campo">
        <label>Ativo: </label>
        <input v-model="ativo" type="checkbox">
      </div>
      
      <div class="botoes-form">
        <button @click="salvar" class="btn-salvar">Salvar</button>
        <button @click="limparFormulario">Cancelar</button>
      </div>
    </div>

    <hr>

    <ul v-if="alunos.length > 0">
      <li v-for="(a, i) in alunos" :key="a.id">
        #{{ i + 1 }} — {{ a.nome }} | {{ a.matricula }} | {{ a.curso }} | 
        <span :class="{ 'text-ativo': a.ativo, 'text-inativo': !a.ativo }">
          {{ a.ativo ? 'Ativo' : 'Inativo' }}
        </span>
        
        <div class="acoes-lista">
          <button @click="editar(a)" class="btn-editar">Editar</button>
          <button @click="excluir(a.id)" class="btn-excluir">Excluir</button>
        </div>
      </li>
    </ul>
    <p v-else class="mensagem-vazia">
      Nenhum aluno cadastrado ainda. Cadastre o primeiro!
    </p>
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

function salvar() {
  if (!nome.value || !matricula.value || !curso.value) {
    alert("Por favor, preencha nome, matrícula e curso.");
    return;
  }

  if (editandoId.value === null) {
    // Cadastrando
    alunos.value.push({
      id: Date.now(),
      nome: nome.value,
      matricula: matricula.value,
      curso: curso.value,
      ativo: ativo.value
    });
  } else {
    // Editando
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
  if (confirm("Tem certeza que deseja excluir?")) {
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

<style scoped>
.formulario {
  margin: 15px 0;
}

.campo {
  margin-bottom: 5px;
}

.text-ativo {
  color: green;
}

.text-inativo {
  color: red;
}
</style>
