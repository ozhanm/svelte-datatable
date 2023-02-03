<script>
    import { createEventDispatcher } from "svelte";

    export let pageIndex, totalPage;

    let dispatch = createEventDispatcher();

    const clickHandle = (index) => {
        pageIndex = index;
        dispatch("updatePageIndex", pageIndex);
    };
</script>

<div class="pagination" class:hide={totalPage == 1}>
    <ul>
        <li class:inactive={pageIndex == 1}>
            <button on:click={() => clickHandle(pageIndex - 1)}>◀</button>
        </li>
        {#each Array(totalPage) as row, key}
            <li class:active={key + 1 == pageIndex}>
                <button on:click={() => clickHandle(key + 1)}>{key + 1}</button>
            </li>
        {/each}
        <li class:inactive={pageIndex == totalPage}>
            <button on:click={() => clickHandle(pageIndex + 1)}>▶</button>
        </li>
    </ul>
</div>

<style lang="scss">
    .pagination {
        width: 100%;
        margin: 25px 0;
        display: flex;
        justify-content: center;
        &.hide {
            display: none;
        }
        ul {
            display: flex;
            flex-wrap: nowrap;
            align-items: center;
            li {
                list-style: none;
                margin: 0 1px 0 0;
                &.active {
                    button {
                        pointer-events: none;
                        background: #dfdfdf;
                        cursor: default;
                    }
                }
                &.inactive {
                    button {
                        pointer-events: none;
                        background: #dfdfdf;
                        cursor: default;
                        opacity: 0.35;
                    }
                }
                button {
                    width: 38px;
                    height: 38px;
                    display: flex;
                    align-items: center;
                    justify-content: center;
                    border: 0;
                    font-size: 14px;
                    font-weight: 600;
                    color: #222;
                    cursor: pointer;
                    &:not(:last-child) {
                        margin-right: 5px;
                    }
                    &:hover {
                        background: #f3f3f3;
                    }
                }
            }
        }
    }
</style>
