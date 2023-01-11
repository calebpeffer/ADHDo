<script lang="ts">
    import { onMount } from 'svelte';
    import { writable } from 'svelte/store';
    import { flip } from 'svelte/animate';

    interface Todo {
        text: string;
        completed: boolean;
    }

    const todos = writable<Todo[]>([]);
    const completedTodos = writable<Todo[]>([]);
    const dragDuration = 300;
    let draggingCard:Todo | undefined;
    let animatingCards = new Set();

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
            
            // Move completed todo to the completedTodos list
            completedTodos.update((completedItems) => [...completedItems, newItems[index]]);

            // Remove completed todo from the todos list
            newItems.splice(index, 1);



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

function swapWith(index:Todo) {
    if(draggingCard === undefined) return;
    if (draggingCard === index || animatingCards.has(index)) return;
    animatingCards.add(index);
    setTimeout(() => animatingCards.delete(index), dragDuration);
    const cardAIndex = $todos.indexOf(draggingCard);
    const cardBIndex = $todos.indexOf(index);
    $todos[cardAIndex] = index;
    $todos[cardBIndex] = draggingCard;
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
            animate:flip={{ duration: dragDuration }}
            class:completed={todo.completed}
            class="todo-card"
            draggable="true"
            on:dragstart={() => draggingCard = todo}
            on:dragend={() => draggingCard = undefined}
            on:dragenter={() => swapWith(todo)}
            on:dragover|preventDefault
        >
            {todo.text}
            <div>
            <button
            class="ml-4 bg-green-500 text-white rounded-lg p-2"

             on:click={() => completeTodo(index)}>Complete</button>
            <button 
            class="ml-4 bg-red-500 text-white rounded-lg p-2"

            on:click={() => deleteTodo(index)}>Delete</button>
        </div>
        </div>
    {/each}

    <br/>

    <h1>Completed Todos</h1>
    <br/>


    {#each $completedTodos as todo, index (todo)}
        <div
            animate:flip={{ duration: dragDuration }}
            class:completed={todo.completed}
            class="todo-card"
            draggable="true"
            on:dragstart={() => draggingCard = todo}
            on:dragend={() => draggingCard = undefined}
            on:dragenter={() => swapWith(todo)}
            on:dragover|preventDefault
        >
            {todo.text}
            <div>
            <button
            class="ml-4 bg-green-500 text-white rounded-lg p-2"

             on:click={() => completeTodo(index)}>Complete</button>
            <button 
            class="ml-4 bg-red-500 text-white rounded-lg p-2"

            on:click={() => deleteTodo(index)}>Delete</button>
        </div>
        </div>
    {/each}
</div>

<style>
	.todo-list {
		@apply p-4;
	}

	.todo-card {
		@apply bg-gray-200 rounded-lg p-4 mb-4 cursor-pointer justify-between flex flex-row;
	}

	.todo-card.completed {
		@apply text-gray-600 line-through;
	}

	.todo-input {
		@apply border rounded-lg p-2 w-full;
	}
</style>
