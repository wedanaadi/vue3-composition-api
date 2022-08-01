<template>
  <div class="mt-2">
    <ul class="list-group">
      <li class="list-group-item" v-for="(todo, i) in todos" :key="i">
        <div class="row">
          <div class="col">
            <span v-if="todo.isDone">
              <del> {{ todo.activity }} </del>
            </span>
            <span v-else> {{ todo.activity }} </span>
          </div>
          <div class="col-auto">
            <div class="row gx-2">
              <div class="col" v-show="!todo.isDone">
                <button class="btn btn-info" @click="doneTODO(i)">Done</button>
              </div>
              <div class="col">
                <button class="btn btn-danger" @click="deleteTODO(i)">x</button>
              </div>
            </div>
          </div>
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  props: {
    todos: {
      type: Array,
      default: [],
    },
  },
  setup(props, { emit }) {
    const deleteTODO = (index) => {
      emit("delete", index);
    };
    const doneTODO = (index) => {
      emit("done", index);
    };

    return { deleteTODO, doneTODO };
  },
};
</script>
