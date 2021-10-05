<template>
	<Header />
	<form @submit.prevent="addTodo()">
		<label>{{ newItemLabel }}</label>
		<input
			v-model="newTodo"
			name="newTodo"
			autocomplete="off"
		>
		<button class="submit">{{ addBtn }}</button>
	</form>
	<h2>{{ listTitle }}</h2>
	<ul>
		<li
			v-for="(todo, index) in todos"
			:key="index"
		>
			<span
				:class="{ done: todo.done }"
				@click="doneTodo(todo)"
			>{{ todo.content }}</span>
			<button @click="removeTodo(index)" class="remove">{{ removeBtn }}</button>
		</li>
	</ul>
	<h4 v-if="todos.length === 0">Empty list.</h4>
	<Footer />
</template>

<script>
	import { ref } from 'vue';
  import Header from "./components/Header.vue";
  import Footer from './components/Footer.vue';
	export default {
		name: 'App',
    components: {
      Header,
      Footer
    },
    data() {
      return {
        newItemLabel: "New ToDo Item",
        addBtn: "Add New Item",
        listTitle: "Current ToDo List:",
        removeBtn: "Remove Item",
      }
    },
		setup () {
			const newTodo = ref('');
			const defaultData = [{
				done: false,
				content: 'Write a blog post'
			}]
			const todosData = JSON.parse(localStorage.getItem('todos')) || defaultData;
			const todos = ref(todosData);
			function addTodo () {
				if (newTodo.value) {
					todos.value.push({
						done: false,
						content: newTodo.value
					});
					newTodo.value = '';
				}
				saveData();
			}

			function doneTodo (todo) {
				todo.done = !todo.done
				saveData();
			}

			function removeTodo (index) {
				todos.value.splice(index, 1);
				saveData();
			}

			function saveData () {
				const storageData = JSON.stringify(todos.value);
				localStorage.setItem('todos', storageData);
			}

			return {
				todos,
				newTodo,
				addTodo,
				doneTodo,
				removeTodo,
				saveData
			}
		}
	}
</script>

<style lang="scss">
  @import "./scss/variables.scss";
  @import "./scss/mixins.scss";
  body {
    margin: 0;
    padding: 0;
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    background-color: $background;
    color: $text;
    #app {
      max-width: 600px;
      margin-top:30px;
      margin-left: auto;
      margin-right: auto;
      padding: 20px;
      form {
        display: flex;
        flex-direction: column;
        width: 100%;
        label {
          font-size: 14px;
          font-weight: bold;
        }
        input,
        button {
          @include styleRemoval();
          height: $size5;
          padding-left: $size2;
          padding-right: $size2;
          border-radius: $size1;
          font-size: 18px;
          margin-top: $size1;
          margin-bottom: $size2;
        }
        input {
          border: $border;
          color: inherit;

          &:focus {
            background-color:darken($background, 8%);
          }
        }
        button {
          @include btnStyle();
        }
      }
      h2 {
        font-size: 22px;
        border-bottom: $border;
        display: inline-block;
        margin:10px auto
      }
      ul {
        padding: 0px 0 5px;
        li {
          display: flex;
          justify-content: space-between;
          align-items: center;
          border: $border;
          padding: $size2 $size4;
          border-radius: $size1;
          margin-bottom: $size2;
          span {
            cursor: pointer;
          }
          .done {
            text-decoration:$accent line-through 2.5px;
          }
          button {
            @include styleRemoval();
            @include btnStyle();
            font-size: $size2;
          }
        }
      }
      h4 {
        text-align: center;
        opacity: 0.5;
        margin: 0;
      }
    }
  }
   // X-Small devices (portrait phones, less than 576px)
  @media (max-width: 575.98px) {
    body #app {
      max-width:400px;
    }
  }
</style>
