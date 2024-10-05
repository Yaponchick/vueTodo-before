<template>
  <div id="app1">
    <h1>Список дел</h1>
    <input v-model="itemForAdd" class="mainInput" placeholder="Добавьте задачу" />
    <button @click="add">Добавить задачу</button>
    <button @click="del">Удалить</button>

    <div class="filter">
      <div>
        <input type="radio" name="check" id="completed" @click="filterTasks('completed')" />
        <label for="completed">Выполненные</label>
      </div>
      <div>
        <input type="radio" name="check" id="notCompleted" @click="filterTasks('notCompleted')" />
        <label for="notCompleted">Невыполненные</label>
      </div>
      <div>
        <input type="radio" name="check" id="allTasks" @click="filterTasks('all')" />
        <label for="allTasks">Показать все</label>
      </div>
    </div>

    <div>
      <label for="sort">Сортировать по имени </label>
      <input type="radio" name="sort" id="sort1" @click="sortTasks('title')" />
      <p>Сортировать по дате:</p>
      <label for="sort2">Самые ранние </label>
      <input type="radio" name="sort" id="sort2" @click="sortTasks('asc')" />
      <label for="sort3">Самые поздние </label>
      <input type="radio" name="sort" id="sort3" @click="sortTasks('desc')" />
    </div>

    <h1>Мои задачи:</h1>
    <div v-for="(todo, index) in filteredTasks" :key="todo.id" style="display: flex; align-items: center;">
      <todo-item
        :todo="todo"
        :index="index"
        @complete-done="makeDone(index)"
        :class="{ noActive: todo.noActive }"
      />
    </div>
  </div>
</template>

<script>
import TodoItem from './TodoItem.vue';
export default {
  components: {
    TodoItem,
  },
  data() {
    return {
      todoList: [],
      itemForAdd: '',
      filter: 'all',
      sortBy: 'id',
    };
  },
  computed: {
    filteredTasks() {
      let tasks = this.todoList.slice();
      if (this.filter === 'completed') {
        tasks = tasks.filter(todo => todo.completed);
      } else if (this.filter === 'notCompleted') {
        tasks = tasks.filter(todo => !todo.completed);
      }

      if (this.sortBy === 'title') {
        return tasks.sort((a, b) => a.title.localeCompare(b.title));
      } else if (this.sortBy === 'asc') {
        return tasks.sort((a, b) => a.id - b.id);
      } else if (this.sortBy === 'desc') {
        return tasks.sort((a, b) => b.id - a.id);
      }
      return tasks;
    },
  },
  methods: {
    add() {
      if (this.itemForAdd.trim() === '') return;
      const newTodo = {
        title: this.itemForAdd,
        completed: false,
        id: this.todoList.length + 1,
        noActive: false,
      };
      this.todoList.push(newTodo);
      this.itemForAdd = '';
    },
    del() {
      this.todoList = this.todoList.filter(todo => !todo.completed);
    },
    makeDone(index) {
      this.todoList[index].completed = !this.todoList[index].completed;
    },
    filterTasks(filter) {
      this.filter = filter;
    },
    sortTasks(sortBy) {
      this.sortBy = sortBy;
    },
  },
};
</script>

<style scoped></style>
