<script lang="ts">
    import type { Task } from "../types";
    import { fade } from "svelte/transition";
    let {
        tasks, 
        toggleDone,
        removeTask
    }: { 
        tasks: Task[];
        toggleDone: (task: Task) => void;
        removeTask: (id: string) => void;
    } = $props();
</script>
{#if tasks.length}
    <section>
        {#each tasks as task}
            <article transition:fade>
                <label for="">
                    <input 
                        type="checkbox" 
                        checked={task.done}
                        onchange={() => toggleDone(task)}                
                    >
                    <span class:done={task.done}>{task.title}</span>
                </label>
                <button onclick={()=> removeTask(task.id)}>Remove</button>
            </article>
        {/each}
    </section> 
{/if}


<style>
    section{
        padding: 0.5em;
        border-radius: 10px;
    }
    article{
        background-color: #949393;
        color: #ffff;
        font-size: 20px;
        padding: 0.5em;
        margin: 0.5em;
        border-radius: 5px;
        display: flex;
        justify-content: space-between;
    }
    .done{
        text-decoration: line-through;
    }
    button{
        background-color: rgb(250, 43, 43);
        color: #ffff;
    }
</style>