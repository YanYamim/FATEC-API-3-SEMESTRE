<template>
    <div class="modal-overlay">
      <div class="modal-content">
        <h2 class="modal-title">Editando Api:</h2>
        <form @submit.prevent="editarApi" class="modal-form">
          <label for="nome">Nome:</label>
          <input 
            v-model="apiEmEdit.nome"
            type="text"
            id="nome"
            required 
            class="modal-input"
          />
  
          <label for="frequencia">Frequência:</label>
          <div class="select-container">
            <select class="modal-input" id="capture-period" v-model="apiEmEdit.frequencia">
              <option value="diariamente">Diariamente</option>
              <option value="semanalmente">Semanalmente</option>
              <option value="mensalmente">Mensalmente</option>
            </select>
          </div>
  
          <label for="url">URL:</label>
          <input v-model="apiEmEdit.url" class="modal-input" type="url" id="url" />

          <label for="descricao">Descrição:</label>
          <input v-model="apiEmEdit.descricao" class="modal-input" type="text" id="url" />
  
          <div class="active-group">
            <label for="active">Ativo:</label>
            <input v-model="apiEmEdit.active" class="modal-input" type="checkbox" id="active" />
          </div>        
  
          <div class="modal-actions">
            <button type="submit" class="salvar-btn">Salvar</button>
            <button type="button" class="cancelar-btn" @click="fecharModal">Cancelar</button>
          </div>
        </form>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    emits: ['close', 'save'],
    props: {
      api: Object,
      modalAberto: Boolean
    },
    data() {
      return {
        apiEmEdit: {
          id: this.api.id,
          nome: this.api.nome,
          url: this.api.url,
          active: this.api.active,
          frequencia: this.api.frequencia,
          descricao: this.api.descricao
        }
      }
    },
    methods: {
      fecharModal() {
        this.$emit('close')
      },
      saveChanges() {
        this.$emit('save')
        this.fecharModal()
      },
      editarApi() {
        const apiAtualizado = this.apiEmEdit;
        fetch(`http://localhost:8080/apis/editar/${apiAtualizado.id}`, {
          method: 'PUT',
          headers: {
            'Content-Type': 'application/json'
          },
          body: JSON.stringify(apiAtualizado)
        })
          .then((response) => {
            if (!response.ok) {
              throw new Error('Erro ao atualizar API')
            }
            return response.json()
          })
          .then((data) => {
            this.saveChanges()
          })
          .catch((error) => {
            console.error('Erro ao editar API:', error)
          })
      }
    },
  watch: {
    api(api) {
      this.apiEmEdit = { ...api }
    }
  }
  }
  </script>
  
  <style scoped>
  .modal-overlay {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(0, 0, 0, 0.5);
    display: flex;
    justify-content: center;
    align-items: center;
    z-index: 9999;
  }
  
  .modal-content {
    background: white;
    padding: 40px;
    border-radius: 10px;
    width: 60%;
    max-width: 800px;
    height: auto;
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
    position: relative;
    display: flex;
    flex-direction: column;
  }
  
  .modal-title {
    color: #4a148c;
    font-size: 26px;
    margin-bottom: 20px;
  }
  
  .modal-form {
    display: flex;
    flex-direction: column;
  }
  
  .modal-input {
    border: 1px solid #d1c4e9;
    border-radius: 5px;
    padding: 12px;
    margin-bottom: 20px;
    font-size: 16px;
    background-color: #f3e5f5;
  }
  
  .modal-select {
    border: 1px solid #d1c4e9;
    border-radius: 5px;
    padding: 12px;
    margin-bottom: 20px;
    font-size: 16px;
    background-color: #f3e5f5;
  }
  
  .modal-actions {
    margin-top: 20px;
    display: flex;
    justify-content: flex-start;
    gap: 15px;
  }
  
  .salvar-btn {
    background-color: #6a1b9a;
    color: white;
    padding: 10px 20px;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    font-size: 16px;
  }
  
  .salvar-btn:hover {
    background-color: #4a148c;
  }
  
  .cancelar-btn {
    background-color: transparent;
    color: #6a1b9a;
    padding: 10px 20px;
    cursor: pointer;
    font-size: 16px;
    border-radius: 5px;
    border: 2px solid black;
  }
  
  .active-group {
    display: flex;
    align-items: baseline;
    gap: 10px; 
  }
  
  .cancelar-btn:hover {
    text-decoration: underline;
  }
  </style>
  