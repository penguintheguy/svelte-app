<script>
	let todos = ["Add Todo"];
	let addTodoText;

	function addTodo(){
		if (addTodoText == undefined || addTodoText.trim() == "") {
			alert("Todo text can't be empty")
		} else {	
			todos[todos.length] = addTodoText;
			addTodoText = undefined;
		}
	}

</script>

<main class="container-fluid">
	<br>
	<input bind:value={addTodoText}>
	<button on:click={ addTodo } class="container-fluid">Add Todo</button>
	<br>
	<br>
{#each todos as todo, index}

	<div class="overflow-auto" role="group">
		
		<div role="button">
			
			<b>{index+1}. {todo}</b>
			
		</div>
		
		<button 
			class="secondary"
			on:click={ ()=> {todos = [...todos.slice(0,index), ...todos.slice(index+1)];} }>
			Remove
		</button>
		
		<br>
	
	</div>

{/each}

</main>
