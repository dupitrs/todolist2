<template>
  <div id="app">
    <h1>TO DO LIST</h1>

    <!-- JAUNA FIGNA -->
    <div>
      <input v-model="newTask.name" type="text" placeholder="Uzdevuma nosaukums" />
      <select v-model="newTask.category">
        <option value="">Izvēlies kategoriju</option>
        <option value="Darbi">Darbi</option>
        <option value="Brīvais laiks">Brīvais laiks</option>
        <option value="Skola">Skola</option>
      </select>
      <button @click="addTask">Pievienot</button>
    </div>

    <!-- FILTRS -->
    <div id="filtri">
      <h3>Filtrēt pēc kategorijas:</h3>
      <select v-model="filterCategory">
        <option value="">Visas kategorijas</option>
        <option value="Darbi">Darbi</option>
        <option value="Brīvais laiks">Brīvais laiks</option>
        <option value="Skola">Skola</option>
      </select>
      <select v-model="filterStatus">
      <option value="">Visi statusi</option>
      <option value="Pabeigts">Pabeigts</option>
      <option value="Nepabeigts">Nepabeigts</option>
    </select>
    </div>

    <!-- SARAKSTS -->
        <ul>
      <li v-for="(task, index) in filteredTasks" :key="index">
        <div v-if="!task.isEditing">
          <span :style="{ textDecoration: task.status === 'Pabeigts' ? 'line-through' : 'none' }">
            {{ task.name }} ({{ task.category }})
          </span>
          <button @click="toggleStatus(index)">
            {{ task.status === "Pabeigts" ? "Atjaunot" : "Nosvītrot" }}
          </button>
          <button @click="editTask(index)">Rediģēt</button>
          <button @click="deleteTask(index)">Dzēst</button>
        </div>
        <div v-else>
          <input v-model="task.name" />
          <select v-model="task.category">
            <option value="Darbi">Darbi</option>
            <option value="Brīvais laiks">Brīvais laiks</option>
            <option value="Skola">Skola</option>
          </select>
          <button @click="saveTask(index)">Saglabāt</button>
        </div>
      </li>
    </ul>

  </div>
</template>




















<script>
export default {
  data() {
    return {
      newTask: {
        name: "",
        category: "",
        status: "Nepabeigts", // uzreiz kad izveido nepabeights
      },
      tasks: [], // uzdevumu saraksts
      filterCategory: "", // Filtrs kategorijas
      filterStatus: "", // Filtrs statusa
      searchQuery: "", // Meklē
    };
  },
  computed: {
    // Filtrētie uzdevumi
    filteredTasks() {
      return this.tasks.filter((task) => {
        const matchesCategory = this.filterCategory ? task.category === this.filterCategory : true;
        const matchesStatus = this.filterStatus ? task.status === this.filterStatus : true;
        const matchesSearch = task.name.toLowerCase().includes(this.searchQuery.toLowerCase());

        return matchesCategory && matchesStatus && matchesSearch;
      });
    },
  },
  methods: {
    // Pievieno jaunu uzdevumu
    addTask() {
      if (this.newTask.name.trim() && this.newTask.category) {
        this.tasks.push({
          ...this.newTask,
          status: "Nepabeigts", // nepabeights uzreiz
          isEditing: false, 
        });
        this.newTask.name = "";
        this.newTask.category = "";
      }
    },
    // Dzēš 
    deleteTask(index) {
      this.tasks.splice(index, 1);
    },
    // Pārslēdz statusu starp Pabeigts un Nepabeigts
    toggleStatus(index) {
      const task = this.tasks[index];
      task.status = task.status === "Pabeigts" ? "Nepabeigts" : "Pabeigts";
    },
    // Sāk rediģēt uzdevumu
    editTask(index) {
      this.tasks[index].isEditing = true;
    },
    // Saglabā rediģēto uzdevumu
    saveTask(index) {
      this.tasks[index].isEditing = false;
    },
  },
    toggleStatus(index) {
    const task = this.tasks[index];
    task.status = task.status === "Pabeigts" ? "Nepabeigts" : "Pabeigts";
  },
};
</script>




















<style scoped>

body {
  font-family: 'Roboto', Arial, sans-serif;
  background-color: #1e1e1e;
  color: #dcdcdc;
  margin: 0;
  padding: 0;
  display: flex;
  justify-content: center;
  align-items: flex-start;
  min-height: 100vh;
}

#app {
  background-color: #2b2b2b;
  border-radius: 15px;
  box-shadow: 0px 4px 10px rgba(0, 0, 0, 0.5);
  padding: 50px;
  width: 1200px;
  height: 600px;
}

h1 {
  font-size: 60px;
  text-align: left;
  color: #ffffff;
  margin-bottom: 20px;
  margin-left: 10px;
  font-family: "Comic Sans MS", "Comic Sans", cursive;
}

h3 {
  font-size: 1.2rem;
  margin-bottom: 10px;
  color: #b0b0b0;
}

input,
select {
  width: 100%;
  padding: 10px;
  margin-bottom: 10px;
  border: 1px solid #444444;
  border-radius: 5px;
  background-color: #3c3c3c;
  color: #ffffff;
  font-size: 1rem;
}

button {
  width: 100%;
  padding: 10px;
  font-size: 1rem;
  font-weight: bold;
  color: #ffffff;
  background-color: #425eb9;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

button:hover {
  background-color: #7a94eb;
}

/* Saraksta stils */
ul {
  list-style: none;
  padding: 0;
  margin: 20px 0 0 0;
}

li {
  background-color: #3a3a3a;
  border-radius: 5px;
  padding: 15px;
  margin-bottom: 10px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  box-shadow: 0px 2px 5px rgba(0, 0, 0, 0.3);
}

li span {
  flex-grow: 1;
  font-size: 1rem;
  color: #ffffff;
}

li button {
  margin-left: 10px;
  padding: 8px 12px;
  background-color: #f05454;
  border-radius: 5px;
}

li button:hover {
  background-color: #d04343;
}

/* Pielāgot filtrus */
select {
  display: inline-block;
  margin-bottom: 10px;
}

@media (min-width: 600px) {
  input,
  select {
    width: calc(50% - 10px);
    display: inline-block;
    margin-right: 10px;
  }
  button {
    width: auto;
    padding: 10px 20px;
  }
}

#filtri {
  background-color: #2b2b2b;
  border-radius: 15px;
  padding: 20px;
  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.7);
  color: #dcdcdc;
  width:380px;
  height:300px;
  margin-top: 100px;
}

#filtri h3 {
  font-size: 1.5rem;
  margin-bottom: 15px;
  color: #ffffff;
  text-align: center;
}

#filtri select {
  width: 48%;
  margin-right: 2%;
  margin-bottom: 10px;
  padding: 10px;
  background-color: #3c3c3c;
  color: #ffffff;
  font-size: 1rem;
}

#filtri select:last-child {
  margin-right: 0;
}

</style>
