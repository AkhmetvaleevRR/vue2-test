<template>
  <div>
    <div class="table">
      <div class="header">
        <div class="cell first-column" @click="sortBy('name')">Имя</div>
        <div class="cell second-column" @click="sortBy('phone ')">Телефон</div>
      </div>
      <div class="body">
        <div  v-for="person in sortedArray" :key="person.id" class="row">
          <div class="cell first-column":style="{ 
            width: `calc(100% - ${getHierarchyLevel(person) * 10}px)`,
            marginLeft: `${getHierarchyLevel(person) * 10}px` 
          }">
            <span v-if="hasChild(person.id)"> + </span>
            {{ person.name }}
          </div>
          <div class="cell second-column">{{ person.phone }}</div>
        </div>
      </div>
    </div>
    <button @click="openModal">Добавить</button>
    <PersonModal
      v-if="isModalOpen"
      :personArray="personArray"
      @close="isModalOpen = false"
      @add-person="addPerson"
    />
  </div>
</template>

<script>
import PersonModal from './personModal.vue';

export default {
  components: {
    PersonModal
  },
  data() {
    return {
      personArray: [],
      isModalOpen: false,      
      sortKey: 'name',
      sortOrder: 1
    };
  },
  computed: {
    sortedArray() {
      const sorted = [...this.personArray];
      sorted.sort((a, b) => {
        const levelA = this.getHierarchyLevel(a);
        const levelB = this.getHierarchyLevel(b);

        if (levelA === levelB) {
          if (a[this.sortKey] < b[this.sortKey]) return -1 * this.sortOrder;
          if (a[this.sortKey] > b[this.sortKey]) return 1 * this.sortOrder;
          return 0;
        }
        return levelA - levelB;
      });
      return sorted;
    }
  },
  methods: {
    openModal() {
      this.isModalOpen = true;
    },
    getParentId(parentId){
      return this.personArray.findIndex(person => person.id === parentId);
    },
    addPerson(newPerson) {    
      const { name, phone, parentId } = newPerson;
      const newId = this.personArray.length + 1;
      const person = { id: newId, name, phone, parentId: parentId };
      if (parentId) {
        const parentIndex = this.getParentId(parentId)
        if (parentIndex !== -1) {
          this.personArray.splice(parentIndex + 1, 0, person);
        } else {
          this.personArray.push(person);
        }
      } else {
        this.personArray.push(person);
      }
      localStorage.setItem('personArray', JSON.stringify(this.personArray))
    },
    hasChild(id){
      return this.personArray.some(person => person.parentId === id)
    },
    getHierarchyLevel(person) {
      let level = 0;
      while (person.parentId) {
        level++;
        person = this.personArray.find(p => p.id === person.parentId);
      }
      return level;
    },
    loadData() {
      const personArray = localStorage.getItem('personArray');
      if (personArray) {
        this.personArray = JSON.parse(personArray);
      } else {
        this.personArray = [
          { id: 1, name: 'Иван', phone: '+7 941 123 21 42' },
          { id: 2, name: 'Петр', phone: '+7 941 123 21 42' }
        ];
      }
    },    
    sortBy(key) {
      if (this.sortKey === key) {
        this.sortOrder *= -1;
      } else {
        this.sortKey = key;
        this.sortOrder = 1;
      }
    }
  },  
  created() {
    this.loadData();
  },
};
</script>


<style scoped>
.table {
  display: flex;
  flex-direction: column;
  width: 300px;
  margin: 0 auto;
}

.header, .row {
  display: flex;
}

.cell {
  padding: 8px;
  border: 1px solid black;
  text-align: center;
}

.header {
  background-color: #f2f2f2;
}

.header .cell:hover {
  opacity: 0.7;
}

button {
  margin-top: 10px;
  padding: 10px 20px;
  background-color: #00796b;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

button:hover {
  background-color: #004d40;
}

.first-column {
  flex: 1;
}
.second-column {
  width: 150px;
}

</style>
