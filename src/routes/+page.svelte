<script lang="ts">
    import TasksForm from "../componants/tasks-form.svelte";
	import TasksList from "../componants/tasks-list.svelte";
    import type { Filter, Task } from "../types";

    let message = "Tasks App";
    let currentFilter = $state<Filter>("All");
    let tasks = $state<Task[]>([]);
    let totalDone = $derived(
        tasks.reduce(
            (total, task) => total + Number(task.done),
            0
        )
    );

    let filteredTasks = $derived.by(() => {
        switch (currentFilter) {
            case 'All': {
                return tasks;        
            }
            case 'Done': {
                return tasks.filter((task) => task.done);
            }
            case "Todo": {
                return tasks.filter((task) => !task.done);
            }
        }
        return tasks;
    });

    function addTask(newTask: string){
        tasks.push({
            id: crypto.randomUUID(),
            title: newTask,
            done: false
        });
    }
    function toggleDone(task: Task){
        task.done = !task.done;
    }
    function removeTask(id: string){
        const index = tasks.findIndex(
            (task) => task.id === id
        );
        tasks.splice(index, 1);
    }
</script>

{#snippet filterButton (filter: Filter)}
    <button 
        onclick={() => currentFilter = filter} 
        class:contrast={currentFilter === filter}
    >
        {filter}
    </button>

{/snippet}

<main>
    <h1>{message}</h1>
    <TasksForm {addTask}/>
    <p>
        {#if tasks.length}
            {totalDone} / {tasks.length} tasks completed
        {:else}
            Add a task to get started.
        {/if}
    </p>
    {#if tasks.length} 
        <div class="filter-buttons">
            {@render filterButton("All")}
            {@render filterButton("Todo")}
            {@render filterButton("Done")}
        </div>
    {/if}
    <TasksList 
        tasks = {filteredTasks}
        {toggleDone} 
        {removeTask}
    />
</main>

<style>
    main{
        display: flex;
        justify-content: center;
        flex-direction: column;
        margin: auto;
        max-width: 800px;
    }
    h1{
        text-align: center;
    }
    .filter-buttons{
        margin: 0.5em;
        display: flex;
        justify-content: end;
        gap: 0.5em;
    }
    .contrast{
        border-color: aqua;
    }
</style>