<template>
  <div>
    <h1>Task List</h1>
    <div class="task-input">
  <input type="text" placeholder="Add a new task" v-model="newTask" @keyup.enter="addTask">
  <button @click="addTask" :class="{ 'clicked': newTask }">Add</button>
</div>
<div class="filters">
  <span :class="{ 'active': filter == 'all' }" @click="setFilter('all')">All</span>
  <span :class="{ 'active': filter == 'pending' }" @click="setFilter('pending')">Pending</span>
  <span :class="{ 'active': filter == 'completed' }" @click="setFilter('completed')">Completed</span>
  <span class="sort-icon" @click="toggleSortDirection">Sort {{ sortDirection }}</span>
</div>

<ul>
  <li v-for="task in filteredTasks" :key="task.id">
    <input type="checkbox" v-model="task.completed" @change="updateTask(task)">
    <span :class="{ 'completed': task.completed }">{{ task.text }}</span>
    <button @click="deleteTask(task)">Delete</button>
  </li>
</ul>
  </div>
</template>
<script>
export default {
  name: 'TaskList',
  data() {
    return {
      newTask: '',
      tasks: [

      ],
      filter: 'all',
      sort: 'asc',
    };
  },
  computed: {
    filteredTasks() {
      if (this.filter === 'all') {
        return this.tasks;
      } else if (this.filter === 'pending') {
        return this.tasks.filter((task) => !task.completed);
      } else if (this.filter === 'completed') {
        return this.tasks.filter((task) => task.completed);
      }
    },
    sortDirection() {
      return this.sort === 'asc' ? 'A-Z' : 'Z-A';
    },
  },
  methods: {
    addTask() {
      if (!this.newTask) return;

      const newTask = {
        id: Math.max(...this.tasks.map((task) => task.id)) + 1,
        text: this.newTask,
        completed: false,
      };

      this.tasks.push(newTask);
      this.newTask = '';
    },
    updateTask(task) {
      task.completed = !task.completed;
    },
    deleteTask(task) {
      const index = this.tasks.indexOf(task);
      this.tasks.splice(index, 1);
    },
    setFilter(filter) {
      this.filter = filter;
    },
    toggleSortDirection() {
      this.sort = this.sort === 'asc' ? 'desc' : 'asc';
      this.sortTasks();
    },
    sortTasks() {
      this.tasks.sort((a, b) => {
        if (this.sort === 'asc') {
          return a.text.localeCompare(b.text);
        } else {
          return b.text.localeCompare(a.text);
        }
      });
    },
  },
  mounted() {
    this.sortTasks();
  },
};
</script>

<style>
  .task-input {
    display: flex;
    align-items: center;
    margin-bottom: 10px;
  }

  .task-input input[type="text"] {
    padding: 10px;
    font-size: 16px;
    border-radius: 5px;
    border: 1px solid #ccc;
    margin-right: 10px;
    flex: 1;
  }

  .task-input button {
    padding: 10px;
    font-size: 16px;
    border-radius: 5px;
    background-color: #4CAF50;
    color: #fff;
    border: none;
    cursor: pointer;
  }

  .task-input button.clicked {
    background-color: #008CBA;
  }

  .filters {
    margin-bottom: 10px;
  }

  .filters span {
    padding: 5px 10px;
    font-size: 16px;
    border-radius: 5px;
    border: 1px solid #ccc;
    margin-right: 10px;
    cursor: pointer;
  }

  .filters span.active {
    background-color: #4CAF50;
    color: #fff;
  }

  .sort-icon {
    padding: 5px 10px;
    font-size: 16px;
    border-radius: 5px;
    border: 1px solid #ccc;
    cursor: pointer;
  }

  .sort-icon:hover {
    background-color: #eee;
  }

  ul {
    list-style: none;
    margin: 0;
    padding: 0;
  }

  li {
    display: flex;
    align-items: center;
    margin-bottom: 10px;
  }

  li input[type="checkbox"] {
    margin-right: 10px;
  }

  li span {
    flex: 1;
    font-size: 16px;
  }

  li span.completed {
    text-decoration: line-through;
    color: #ccc;
  }

  li button {
    padding: 5px 10px;
    font-size: 16px;
    border-radius: 5px;
    background-color: #f44336;
    color: #fff;
    border: none;
    cursor: pointer;
  }

  li button:hover {
    background-color: #e53935;
  }

  li button.clicked {
    background-color: #008CBA;
  }
</style>
