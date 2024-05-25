<script>
	// defining an array (can be empty)
	let todos = ["Add Todo"];
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
</script>


<main class="container-fluid">
	<!-- Bind to addTodoText -->
	<input bind:value={addTodoText}>
	<!-- Clicking calls the function -->
	<button on:click={ addTodo } class="container-fluid">Add Todo</button>
	<br>
	<br>

	<!-- loop over the todos array -->
{#each todos as todo, index}

	<div class="overflow-auto" role="group">
		
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
		
		<br>
	
	</div>

{/each}

</main>
