<script>
    import { createEventDispatcher } from "svelte";

    export let searchText;

    let dispatch = createEventDispatcher();

    const inputHandle = () => {
        dispatch("updateSearchText", searchText);
    }
</script>

<div class="search">
    <span>Search:</span>
    <input type="search" name="search" bind:value={searchText} on:input={inputHandle} />
</div>

<style lang="scss">
    .search {
        width: 300px;
        display: flex;
        align-items: center;
        justify-content: flex-start;
        span {
            font-size: 15px;
            line-height: 20px;
        }
        input {
            width: 100%;
            padding: 0 10px;
            border: 1px solid #ddd;
            height: 32px;
            margin-left: 10px;
            border-radius: 5px;
        }
    }
</style>
