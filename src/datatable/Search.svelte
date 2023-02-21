<script>
    import { createEventDispatcher } from "svelte";

    export let config, searchText, pageIndex, pageSize, sortColumn, orderBy;

    let dispatch = createEventDispatcher();

    const inputHandle = () => {
        dispatch("updateSearchText", searchText);
    };

    const clickHandle = () => {
        dispatch("resetDefaultConfig", true);
    };
</script>

<div class="search">
    <span>Search:</span>
    <input
        type="search"
        name="search"
        placeholder="Search"
        bind:value={searchText}
        on:input={inputHandle}
    />
    {#if config.searchText != searchText || config.pageIndex != pageIndex || config.pageSize != pageSize || config.sortColumn != sortColumn || config.orderBy != orderBy}
        <button on:click={clickHandle}>Reset to Default</button>
    {/if}
</div>

<style lang="scss">
    .search {
        display: flex;
        align-items: center;
        justify-content: flex-start;
        span {
            font-size: 15px;
            line-height: 20px;
        }
        input {
            width: 240px;
            padding: 0 10px;
            border: 1px solid #ddd;
            height: 32px;
            margin-left: 10px;
            border-radius: 5px;
            &::placeholder {
                color: #fff;
            }
        }
        button {
            background: transparent;
            font-size: 13px;
            height: 30px;
            padding: 0 5px;
            border: 0;
            cursor: pointer;
            margin-left: 10px;
            white-space: nowrap;
            color: #7a80dd;
        }
    }
    @media (max-width: 767px) {
        .search {
            span {
                display: none;
            }
            input {
                margin: 0;
                width: 220px;
                &::placeholder {
                    color: #666;
                }
            }
            button {
                display: none;
            }
        }
    }
</style>
