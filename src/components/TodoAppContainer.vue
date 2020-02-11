<template>
    <div class="app-container">
        <h1>Vue js Todo app</h1>
        <input class="todo-input" type="text" v-model="currentTodo" @keyup.enter="addTodo" />

        <div class="todo-list">
            <div class="todo-item" v-for="(todo, index) in filteredTodos" v-bind:key="index">
                <div class="label-container">
                    <input type="checkbox" v-model="todo.completed">
                    <div v-bind:class="{ completed: todo.completed }"  v-if="!todo.editing" @dblclick="editTodo(todo)" class="todo-item-label" >{{ todo.title }}</div>
                    <input v-else class="todo-item-edit" type="text" v-model="todo.title" @blur="doneEdit(todo)" @keyup.enter="doneEdit(todo)" v-focus>
                </div>
            <div class="remove-todo" @click="removeTodo(index)">&times;</div>
            </div>
        </div>

        <div v-if="!todosRemaining" class="all-done-container">All done</div>

        <div class="bottom-bar">
            <div class="btns-container">
                <button v-bind:class="{ active: currentFilter === 'all'}" @click="toggleFilter('all')">All tasks</button>
                <button v-bind:class="{ active: currentFilter === 'completed'}" @click="toggleFilter('completed')">Completed</button>
                <button v-bind:class="{ active: currentFilter === 'active'}" @click="toggleFilter('active')">Active</button>
            </div>
            <div>todos remaining: {{todosRemaining}}</div>
        </div>

    </div>
</template>

<script>
  export default {
    name: 'to do app',
    props: {
      message: String,
    },
    data() {
      return {
        currentTodo: ' ',
        todoLists: [
          { id: 1, title: 'learn vue', completed: false, editing: false },
          { id: 2, title: 'sleep', completed: false, editing: false },
          { id: 3, title: 'repeat', completed: false, editing: false },
        ],
        currentFilter: 'all',
      };
    },
    methods: {
      addTodo() {
        if (this.currentTodo.trim() === 0) return;
        this.todoLists.push({
          id: this.todoLists.length + 1,
          title: this.currentTodo,
          completed: false,
          editing: false,
        });
        this.currentTodo = ' ';
      },
      removeTodo(index) {
        this.todoLists.splice(index, 1);
      },
      editTodo(todo) {
        todo.editing = true;
      },
      doneEdit(todo) {
        todo.editing = false;
      },
      toggleFilter(filter) {
        this.currentFilter = filter;
      }
    },
    directives: {
      focus: {
        inserted: function (el) {
          el.focus()
        }
      }
    },
    computed: {
      filteredTodos: function () {
        return this.currentFilter === 'all'
            ? this.todoLists
            : this.currentFilter === 'completed'
                ? this.todoLists.filter(todo => todo.completed)
                : this.todoLists.filter(todo => !todo.completed);
      },
      todosRemaining: function () {
        return this.todoLists.filter(todo => !todo.completed).length
      }
  }

  };
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
    .app-container {
        display: flex;
        flex-flow: column;
        align-items: center;
        justify-content: center;

        .todo-list {
            display: flex;
            flex-flow: column;
            width: 50%;

            .todo-item-edit {
                font-size: 24px;
                color: #2c3e50;
                margin-left: 12px;
                width: 100%;
                padding: 10px;
                border: 1px solid #ccc; //override defaults
                font-family: 'Avenir', Helvetica, Arial, sans-serif;

                &:focus {
                    outline: none;
                }
            }


            .todo-item {
                font-size: 24px;
                margin-bottom: 12px;
                display: flex;
                align-items: center;
                justify-content: space-between;
                animation-duration: 0.3s;
                padding: 5px 10px;
                .label-container {
                    display: flex;
                    align-items: center;
                }
                .todo-item-label {
                    padding: 10px;
                    border: 1px solid white;
                    margin-left: 12px;
                }
            }

            .remove-todo {
                cursor: pointer;
                font-size: 20px;

                &:hover {
                    color: #000;
                }
            }
        }

        .todo-input {
            width: 50%;
            padding: 10px 18px;
            font-size: 18px;
            margin-bottom: 16px;

            &:focus {
                outline: 0;
            }
        }

        .bottom-bar {
            display: flex;
            align-items: center;
            width: 50%;
            border-top: 1px solid #ccc; //override defaults
            padding: 20px 0;
            justify-content: space-between;
            .btns-container {
                display: flex;
                button {
                    margin-right: 10px;
                    border-radius: 0;
                    padding: 10px;
                }
                .active {
                    background-color: indianred;
                    color: white;
                }
            }
        }

        .completed {
            text-decoration: line-through;
            color: grey;
        }

        .all-done-container {
            margin: 20px 0;
        }
    }

    input, button {
        cursor: pointer;
    }


    h3 {
        margin: 40px 0 0;
    }

    ul {
        list-style-type: none;
        padding: 0;
    }

    li {
        display: inline-block;
        margin: 0 10px;
    }

    a {
        color: #42b983;
    }
</style>