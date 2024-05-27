<script>
	import '@picocss/pico'

	// defining an array (can be empty)
	let todos = [];

	//  defining the next input
	let addTodoText;


	// Function to add values to 'todos' variable
	function addTodo(){
		// if addTodoText is empty or contains onlywhite space
		if (addTodoText == undefined || addTodoText.trim() == '') {
			alert("Todo text can't be empty")
		} else {	
			// main logic: if addTodoText is neither empty nor contains onlywhite space
			// then add addTodoText to the last of todos array
			// since last element is always at length-1, append to index = length of array
			todos[todos.length] = addTodoText; 
			// change addTodoText to undefined as that user can't populate todos with
			// same data
			addTodoText = undefined; 
		}
	}


	// dynamic placeholder for input
	let placeholder;

	// dynamically checks if todos is empty
	$: if (todos.length == 0) {
		placeholder = "Add something to do";
	} else {
		// Just some fun! LOL
		placeholder = `What after '${todos[todos.length-1]}' ?`
	}


	// Saving user's todos into a variable 'data' using localStorage
	function saveTodo() {
		if (todos.length != 0) {
			// since localStorage stores only strings, JSON.stringify converts our array to string by append single quotes around the array
			localStorage.setItem("data", JSON.stringify(todos)) 
			alert("Your todos were saved to your device!")
		} else {
			alert("Empty todos can't be saved!")
		}
	}


	// remove/deleteing the data
	function deleteTodos(){
		localStorage.removeItem("data")
		alert("Your saved todos were deleted from device!")
	}

	// remove/deleteing all the data
	function deleteAllTodos(){
		localStorage.removeItem("data")
		todos = []
		alert("Your saved todos were deleted from device!")
	}


	// importing the onMount function to load todos of user
	import { onMount } from 'svelte';

	// explained above
	onMount(() => {
		// checks if 'data' i.e. todos exists, then assign 
		if (localStorage.getItem("data") != null){
			// JSON.parse converts our todos array (which was stored as an string) back into array
			todos = JSON.parse(localStorage.getItem("data"))
		}
	})

</script>

<br>
<main class="container-fluid">
	<!-- Bind to addTodoText -->
	<input bind:value={addTodoText} placeholder={placeholder}>
	
	<div class="grid">

		<!-- on click events -->

		<!-- calls addTodo function -->
		<button on:click={ addTodo }>Add Todo</button>

		<!-- calls saveTodo function -->
		<button class="contrast " on:click={saveTodo}>
			Save Todos
		</button>
	</div>
	
	<br>

	<!-- on click event -->
	<!-- calls deleteTodo function -->
	<button class="container-fluid" on:click={deleteTodos}>
		Delete stored todos
	</button>

	<br><br>
	<!-- calls deleteAllTodo function -->
	<button class="container-fluid secondary" on:click={deleteAllTodos}>
		Delete all todos
	</button>

	<br>
	<br>

	<!-- loop over the todos array -->
	{#each todos as todo, index}

		<div class="grid" style="text-align: center;">
		
			<div role="button">

				<!-- Show todo number (position in array + 1) and todo -->
				<b>{index+1}. {todo}</b>
			
			</div>

			<!-- Bind the on click function to call the arrow function 
			 todos = [...todos.slice(0,index), ...todos.slice(index+1)]; here index
			 is the index of todo in array todos.
			 slice the array, include elements from index = 0 to index-1
			 and from index+1 till last
			 hence remove the element in todos array
			 the index of values (data in todos array) after removal of element at particular
			 index get updated and reflects as well
			-->
			<button 
				class="secondary"
				on:click={ ()=> {todos = [...todos.slice(0,index), ...todos.slice(index+1)];} }>
				Remove
			</button>
	
		</div>
		<hr>

	{/each}

</main>
