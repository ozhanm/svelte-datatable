<script>
    import { createEventDispatcher } from "svelte";

    export let pageSize;
    let dispatch = createEventDispatcher();

    const changeHandle = () => {
        dispatch("updatePageSize", pageSize);
    };
</script>

<div class="length">
    <span>Show</span>
    <select name="length" bind:value={pageSize} on:change={changeHandle}>
        <option value={10}>10</option>
        <option value={15}>15</option>
        <option value={25}>25</option>
        <option value={50}>50</option>
        <option value={100}>100</option>
    </select>
    <span>entries</span>
</div>

<style lang="scss">
    .length {
        width: auto;
        display: flex;
        align-items: center;
        justify-content: flex-start;
        span {
            font-size: 15px;
            line-height: 20px;
        }
        select {
            width: 70px;
            padding: 0 10px;
            border: 1px solid #ddd;
            height: 32px;
            margin: 0 10px;
            border-radius: 5px;
            cursor: pointer;
        }
    }
    @media (max-width: 767px) {
        .length {
            span {
                display: none;
            }
            select {
                margin: 0;
            }
        }
    }
</style>
