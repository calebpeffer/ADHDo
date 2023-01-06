<!-- TodoList.svelte -->
<script lang="ts">
	import { onMount } from 'svelte';
	import { writable } from 'svelte/store';

	interface Todo {
		text: string;
		completed: boolean;
	}

	const todos = writable<Todo[]>([]);

    onMount(() => {
        restoreTodosFromLocalStorage();
        saveTodosInLocalStorage();
    });


    function saveTodosInLocalStorage() {
        todos.subscribe((items) => {
            localStorage.setItem('todos', JSON.stringify(items));
        });
    }

    function restoreTodosFromLocalStorage() {
        const items = localStorage.getItem('todos');
        if (items !== null) {
            todos.set(JSON.parse(items));
        }
    }

	function addTodo(todo: string) {
		todos.update((items) => [...items, { text: todo, completed: false }]);
	}
    
    function addTodoToEnd(todo: string){
        todos.update((items) => [{ text: todo, completed: false }, ...items]);
    }

    function deleteTodo(index: number) {
        todos.update((items) => {
            const newItems = [...items];
            newItems.splice(index, 1);
            return newItems;
        });
    }
	function completeTodo(index: number) {
		todos.update((items) => {
			const newItems = [...items];
			newItems[index].completed = true;
			return newItems;
		});
	}

	function reorderTodos(oldIndex: number, newIndex: number) {
		todos.update((items) => {
			const newItems = [...items];
			const [removed] = newItems.splice(oldIndex, 1);
			newItems.splice(newIndex, 0, removed);
			return newItems;
		});
	}

	let newTodo = '';

	onMount(() => {
		todos.subscribe((items) => {
			console.log(items);
		});
	});
</script>

<div class="todo-list">
    <div class='flex flex-row gap-2'>
        <button class="bg-blue-500 p-2 rounded-md text-white"
            on:click={() => {
                addTodo(newTodo);
                newTodo = '';
            }}
        >
            queue
        </button>
        <button
            class="bg-blue-500 p-2 rounded-md text-white "
            on:click={() => {
                addTodoToEnd(newTodo);
                newTodo = '';
            }}
        >
            stacks
        </button>
        <input
            type="text"
            bind:value={newTodo}
            class="todo-input"
            placeholder="Add a new to-do"
            on:keydown={(event) => {
                if (event.key === 'Enter') {
                    addTodo(newTodo);
                    newTodo = '';
                }
            }}
        />
    </div>
	<br />
    <br />

	{#each $todos as todo, index (todo)}
		<div
			class:completed={todo.completed}
			class="todo-card"
			draggable
			on:dragstart={(event) => {
				if (event.dataTransfer !== null) {
					event.dataTransfer.setData('text/plain', index.toString());
				}
			}}
			on:dragover={(event) => {
				event.preventDefault();
			}}
			on:drop={(event) => {
				if (event.dataTransfer !== null) {
					const oldIndex = event.dataTransfer.getData('text/plain');
					reorderTodos(+oldIndex, index);
				}
			}}
		>
			<div>
                <button 
                    class="ml-4 bg-red-500 text-white rounded-lg p-2"
                    on:click={() => {
                        deleteTodo(index);
                    }}
                >
                clear
                </button>  
                <button 
                    class="ml-4 bg-green-500 text-white rounded-lg p-2"
                    on:click={() => {
                        completeTodo(index);
                    }}
                >
                complete
                </button>  
				{todo.text}
                
			</div>
		</div>
	{/each}
</div>

<style>
	.todo-list {
		@apply p-4;
	}

	.todo-card {
		@apply bg-gray-200 rounded-lg p-4 mb-4 cursor-pointer;
	}

	.todo-card.completed {
		@apply text-gray-600 line-through;
	}

	.todo-input {
		@apply border rounded-lg p-2 w-full;
	}
</style>
