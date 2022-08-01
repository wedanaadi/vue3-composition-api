<template>
  <div class="container mt-5">
    <div class="card">
      <div class="card-body">
        <h5 class="card-title">SIMPLE TODO</h5>
        <div class="row">
          <div class="col-10">
            <input type="text" v-model="todo" class="form-control" @keyup.enter="add" />
          </div>
          <div class="col-2">
            <button class="btn btn-success" @click="add">Add</button>
          </div>
        </div>
        <List :todos="list" @delete="deleteTODO" @done="doneTODO" />
        <br />
        <small>Total TODO : {{ totalTODO }} </small>
      </div>
    </div>
  </div>
</template>

<script>
import { ref, reactive, onMounted, computed, toRefs } from "vue";
import List from "./components/List.vue";
export default {
  components: { List },
  setup() {
    const todo = ref("");
    const todos = reactive({
      list: [],
    });

    onMounted(() => {
      todos.list =
        JSON.parse(localStorage.getItem("todos")) !== null
          ? JSON.parse(localStorage.getItem("todos"))
          : [];
    });

    const totalTODO = computed(() => {
      return todos.list.length;
    });

    const add = () => {
      todos.list.unshift({
        activity: todo.value,
        isDone: false,
      });
      todo.value = "";
      saveToLocalStorage();
    };

    const deleteTODO = (todoIndex) => {
      todos.list = todos.list.filter((item, index) => {
        if (index !== todoIndex) {
          return item;
        }
      });
      saveToLocalStorage();
    };

    const doneTODO = (todoIndex) => {
      todos.list = todos.list.filter((item, index) => {
        if (index === todoIndex) {
          item.isDone = true;
        }
        return item;
      });
      saveToLocalStorage();
    };

    const saveToLocalStorage = () => {
      localStorage.setItem("todos", JSON.stringify(todos.list));
    };

    return { todo, ...toRefs(todos), totalTODO, add, deleteTODO, doneTODO };
  },
};
</script>
