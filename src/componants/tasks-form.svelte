<script lang="ts">
    let newTask = $state("");
    let {addTask} : {addTask: (newTask: string)=> void} = $props();

    function formSubmitted(e: SubmitEvent){
        e.preventDefault();
        addTask(newTask);    
        newTask = "";    
    }
</script>

<form onsubmit={formSubmitted}>
    <label for="">
        <input name="newTask" bind:value={newTask}>
    </label>
    <div class="btn-container">
        <button>Add</button>
    </div>
</form>

<style>
    .btn-container{
        display: flex;
        justify-content: end;
    }
    input{
        width: 100%;
    }
    button{
        margin: 1em;
        font-size: 20px;
        padding: 0.5em;
    }
</style>