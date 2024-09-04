<template>
  <div class="modal-overlay" @click.self="close">
    <div class="modal">      
      <div class="modal-header">
        <h4>Добавление пользователя</h4>      
        <button class="close-button" @click="close"></button>
      </div>
      <div class="modal-content">
        <div class="input-row">
          <label for="name">Имя:</label>
          <input id="name" v-model="name" type="text" />
        </div>
        <div class="input-row">
          <label for="phone">Телефон:</label>
          <input id="phone" v-model="phone" type="text" />
        </div>
        <div class="input-row">
          <label for="parent">Начальник:</label>
          <select id="parent" v-model="parentId">
            <option v-for="person in personArray" :key="person.id" :value="person.id">
              {{ person.name }}
            </option>
          </select>
        </div>
        <div class="modal-footer">
          <button @click="addPerson">Сохранить</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    personArray: {
      type: Array,
      required: true
    }
  },
  data() {
    return {
      name: '',
      phone: '',
      parentId: null
    };
  },
  methods: {
    close() {
      this.$emit('close');
    },
    addPerson() {
      if (this.name && this.phone) {
        this.$emit('add-person', {
          name: this.name,
          phone: this.phone,
          parentId: this.parentId
        });
        this.close();
      }
    }
  }
};
</script>

<style scoped>
.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
}

.modal {
  background-color: #fff;
  padding: 20px;
  border-radius: 8px;
  width: 300px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
}

.modal-header {
  display: flex;
  justify-content: space-between
}

.modal-content {
  display: flex;
  flex-direction: column;
  gap: 20px;
}

label {
  display: block;
  margin-bottom: 5px;
  color: #666;
}

.input-row {
  display: flex;
  align-items: center;
  justify-content: space-between;
}

input, select {
  width: 60%;
  padding: 8px;
  box-sizing: border-box;
  border: 1px solid #ddd;
  border-radius: 4px;
}

button {
  padding: 8px 16px;
  border: none;
  border-radius: 4px;
  margin-right: 10px;
  cursor: pointer;  
  background-color: #00796b;
  color: white;
}

button:hover {
  opacity: 0.9;
}

.close-button {
  background-color: transparent;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  height: 12px;
  margin: 0;
}
.close-button:before, .close-button:after {
  content: '';
  position: absolute;
  width: 2px;
  height: 12px;
  background-color: black;
}
.close-button:before {
  transform: rotate(45deg);
}
.close-button:after {
  transform: rotate(-45deg);
}
</style>