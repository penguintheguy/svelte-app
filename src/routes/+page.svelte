<script>
	let todos = []; // defining an array (can be empty)
	let addTodoText; //  defining the next input
	let placeholder; // dynamic placeholder for input
	let isSaveModalOpen = false; // if saved info dialog is open
	let isSaveModalWarning = false; // if empty todo tried to save
	let isTodoTextEmpty = false; // if todo text is empty
	let isDeleteModalOpen = false; // if todo is deleted

	// Components
	import {
		Button,
		Modal,
		TextInput,
		ProgressBar,
	} from "carbon-components-svelte";
	// Icons
	import { TaskRemove, RowDelete, Add } from "carbon-icons-svelte";

	// importing the onMount function to load todos of user
	import { onMount } from "svelte";
	onMount(() => {
		// checks if 'data' i.e. todos exists, then assign
		if (localStorage.getItem("data") != null) {
			// JSON.parse converts our todos array (which was stored as an string) back into array
			todos = JSON.parse(localStorage.getItem("data"));
		}
	});

	// Function to add values to 'todos' variable
	function addTodo() {
		// if addTodoText is empty or contains onlywhite space
		if (addTodoText == undefined || addTodoText.trim() == "") {
			isTodoTextEmpty = true; // show the popup is addTodoText is empty
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

	// dynamically checks if todos is empty
	$: if (todos.length == 0) {
		placeholder = "Add something to do";
	} else {
		// Just some fun! LOL
		placeholder = `What after '${todos[todos.length - 1]}' ?`;
	}

	// Saving user's todos into a variable 'data' using localStorage
	function saveTodo() {
		if (todos.length != 0) {
			// since localStorage stores only strings, JSON.stringify converts our array to string by append single quotes around the array
			localStorage.setItem("data", JSON.stringify(todos));
			isSaveModalOpen = true; // show popup if todos is saved
		} else {
			isSaveModalWarning = true; // show popup if empty todos is tried to be saved
		}
	}

	// remove/deleteing all the data
	function deleteAllTodos() {
		localStorage.removeItem("data");
		todos = [];
		isDeleteModalOpen = true; // show popup after deleting popup
	}
</script>

<br />
<main>

	<br />

	<!-- Add atleast 10 todos! -->
	<ProgressBar
		value={todos.length}
		max={10}
		labelText="Have atleast 10 todos!"
		kind="inline"
	/>

	<br />
	<!-- Bind to addTodoText -->
	<TextInput bind:value={addTodoText} {placeholder} />

	<br />
	<br />

	<!-- on click events -->

	<div class="btns">
		<!-- calls addTodo function -->
		<Button on:click={addTodo} icon={Add}>Add Todo</Button><br />
		<!-- calls saveTodo function -->
		<Button on:click={saveTodo} icon={RowDelete}>Save Todos</Button><br />
		<!-- calls deleteAllTodo function -->
		<Button on:click={deleteAllTodos} icon={TaskRemove}
			>Delete all todos</Button
		>
	</div>


	<!-- The popups! -->
	<Modal
		passiveModal
		preventCloseOnClickOutside
		size="lg"
		bind:open={isSaveModalOpen}
		modalHeading="Todos Saved"
	>
		<h3>Your todos were saved.</h3>
	</Modal>

	<Modal
		passiveModal
		preventCloseOnClickOutside
		size="lg"
		bind:open={isDeleteModalOpen}
		modalHeading="Todos Deleted"
	>
		<h3>Your todos were deleted completely!</h3>
	</Modal>

	<Modal
		passiveModal
		preventCloseOnClickOutside
		size="lg"
		bind:open={isSaveModalWarning}
		modalHeading="Can't save todos!"
	>
		<h3>Empty todos will not be saved!</h3>
	</Modal>

	<Modal
		passiveModal
		preventCloseOnClickOutside
		size="lg"
		bind:open={isTodoTextEmpty}
		modalHeading="Todos can't be empty!"
	>
		<h3>Add some todos! Empty ones aren't added.</h3>
	</Modal>

	<br />
	<br />

	<!-- loop over the todos array -->
	{#each todos as todo, index}
		<div class="loop">
			<h3>
				<!-- Show todo number (position in array + 1) and todo -->
				<b>{index + 1}. {todo}</b>
			</h3><br />

			<!-- Bind the on click function to call the arrow function 
			 todos = [...todos.slice(0,index), ...todos.slice(index+1)]; here index
			 is the index of todo in array todos.
			 slice the array, include elements from index = 0 to index-1
			 and from index+1 till last
			 hence remove the element in todos array
			 the index of values (data in todos array) after removal of element at particular
			 index get updated and reflects as well
			-->
			<Button
				icon={RowDelete}
				on:click={() => {
					todos = [
						...todos.slice(0, index),
						...todos.slice(index + 1),
					];
				}}
			>
				Remove
			</Button>
		</div>
	{/each}
</main>

<style>
	.btns {
		text-align: center;
	}
	.loop{
                max-width: 97dvw;
                overflow-y: auto;
		text-align: center;
		margin-bottom: 15px;
	}
</style>
