<template>
	<h1>{{ title }}</h1>
	<form @submit.prevent="addTodo()">
		<label>{{ newItemLabel }}</label>
		<input
			v-model="newTodo"
			name="newTodo"
			autocomplete="off"
		>
		<button>{{ addBtn }}</button>
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
			<button @click="removeTodo(index)">{{ removeBtn }}</button>
		</li>
	</ul>
	<h4 v-if="todos.length === 0">Empty list.</h4>
	<h6>{{ footer }}</h6>
</template>

<script>
	import { ref } from 'vue';
	export default {
		name: 'App',
    data() {
      return {
        title: "Simple ToDo App",
        newItemLabel: "New ToDo Item",
        addBtn: "Add New Item",
        listTitle: "Current ToDo List:",
        removeBtn: "Remove Item",
		footer: "Lauren Elliott-Manheim • 2015-2022 • Ex astris, scientia"
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
$border: 2px solid
	rgba(
		$color: white,
		$alpha: 0.35,
	);
$size1: 6px;
$size2: 12px;
$size3: 18px;
$size4: 24px;
$size5: 48px;
$backgroundColor:darken(#7D6B7D, 10%);
$textColor:#FFF;
$primaryColor:#A3A1AB;
$secondTextColor: #1f2023;
$accentColor:#FF665A;
body {
	margin: 0;
	padding: 0;
	font-family: Avenir, Helvetica, Arial, sans-serif;
	-webkit-font-smoothing: antialiased;
	-moz-osx-font-smoothing: grayscale;
	background-color: $backgroundColor;
	color: $textColor;
	#app {
		max-width: 600px;
		margin-top:60px;
		margin-left: auto;
		margin-right: auto;
		padding: 20px;
		h1 {
			font-weight: bold;
			font-size: 28px;
			text-align: center;
			margin-bottom:30px;
		}
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
				height: $size5;
				box-shadow: none;
				outline: none;
				padding-left: $size2;
				padding-right: $size2;
				border-radius: $size1;
				font-size: 18px;
				margin-top: $size1;
				margin-bottom: $size2;
			}
			input {
				background-color: transparent;
				border: $border;
				color: inherit;

        &:focus {
          background-color:darken($backgroundColor, 8%);
        }
			}
		}
		button {
			background-color: $primaryColor;
			border: 1px solid $primaryColor;
			color: $secondTextColor;
			font-weight: bold;
			outline: none;
			border-radius: $size1;

      &:hover {
        cursor:pointer;
        background-color: lighten($primaryColor, 22%);
        box-shadow: 0 0 11px $secondTextColor;
        transition: all 0.65s ease;
      }
		}
		h2 {
			font-size: 22px;
			border-bottom: $border;
			padding-bottom: $size1;
		}
		ul {
			padding: 10px;
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
					text-decoration:$accentColor line-through 2.5px;
				}
				button {
					font-size: $size2;
					padding: $size1;
				}
			}
		}
		h4 {
			text-align: center;
			opacity: 0.5;
			margin: 0;
		}
		h6 {
  			font-weight:400;
			letter-spacing:2.25px;
			text-transform:uppercase;
			padding:16px;
			text-align:center;
		}
	}
}
</style>
