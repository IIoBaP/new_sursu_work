<script lang="ts">
	import ToDoControls from './ToDoControls.svelte';
	import ToDoItem from './ToDoitem.svelte';
    import { todoApiModule } from "../lib/api";

    let items = loadData(); 

    function loadData() {
        return todoApiModule.getToDos();
    }

    async function createToDo(event) {
        await todoApiModule.createToDo({
            text: event.detail.text,
            is_done: false
        });
        items = loadData();
    }

    async function removeToDo(event) {
        await todoApiModule.removeToDo(event.detail.id)
        items = loadData();
    }
</script>

<div class="todo-app">
    {#await items}
      Loading...
    {:then value}
        <ToDoControls on:add={createToDo} />
        <div class="todo-app__field">
            {#each value as item} 
                <ToDoItem id ={item.id} text={item.text} bind:isDone={item.is_done} on:remove={removeToDo} />
            {/each}
        </div>
    {/await}
</div>
    


<style>
    .todo-app{
        width: 700px;
        height: 80%;
        background: grey;
        border-radius: 15px;
        padding: 40px;
        display: flex;
        flex-direction: column;
        gap: 30px;
    }
    .todo-app__field{
        background: white;
        flex-grow: 1;
        border-radius: 15px;
        overflow-y: auto;
        padding: 20px;
        display: flex;
        flex-direction: column;
        gap: 20px;
    }
</style>

<!-- 
    onMount(()=>{
        if(localStorage.key('items')){
            items = JSON.parse(localStorage.getItem('items'));
        }
        if(items.length) {
            items.forEach((i)=>{
                if(id < i.id){
                    id = i.id;
                }
            });
            id++;
        }
    }); -->



<!-- function onChangeStatus(event){
    const items = items.find((i) => i.id === event.detail.id);
    item.isDone = !item.isDone;
    items = items;
    localStorage.setItem('items', JSON.stringify(items));
}
function onAddItem(event){
    const item = {
        id: id++,
        text:event.detail.text,
        isDone: false
    };
items.push(item);
items = items;
localStorage.setItem('items', JSON.stringify(items));
}

function onDeleteItem(event){
    const idx = items.findIndex((i)=>i.id === event.detail.id);
    items.splice(idx, 1)
    items = items;
    localStorage.setItem('items', JSON.stringify(items));
} -->
