<script>
    import { createEventDispatcher } from "svelte";

    export let pageSize, thead;

    let dispatch = createEventDispatcher();

    const changeHandle = () => {
        dispatch("updatePageSize", pageSize);
    };
    const clickHandleCheck = (thKey) => {
        dispatch("updateTheadVisible", thKey);
    };
</script>

<div class="length">
    <div class="select">
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

    <div class="dropdown">
        <button class="toggle">
            <span>COLUMNS</span>
        </button>

        <!-- svelte-ignore a11y-no-noninteractive-tabindex -->
        <div class="columns" tabindex={1}>
            {#each thead as row, key}
                <button
                    class:uncheck={row.visible == false}
                    on:click={() => clickHandleCheck(key)}>{row.title}</button
                >
            {/each}
        </div>
    </div>
</div>

<style lang="scss">
    .length {
        width: auto;
        display: flex;
        align-items: center;
        justify-content: flex-start;
        position: relative;
        .select {
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
        .dropdown {
            &:focus-within {
                .columns {
                    opacity: 1;
                    pointer-events: auto;
                }
            }
        }
        button {
            width: 120px;
            height: 32px;
            margin-left: 25px;
            border: 0;
            padding: 0 5px;
            border-radius: 5px;
            font-size: 13px;
            font-weight: 500;
            text-align: left;
            padding-left: 35px;
            background: url(/images/columns.png) #f0f0f0 no-repeat 10px center;
            background-size: 15px;
            cursor: pointer;
            span {
                font-size: 13px;
            }
        }
        .columns {
            margin: 0;
            padding: 10px 0;
            width: 120px;
            position: absolute;
            right: 0;
            top: 36px;
            border-radius: 5px;
            background: #fafafa;
            box-shadow: 0 3px 6px rgba(#000, 0.1);
            pointer-events: none;
            opacity: 0;
            transition: all 0.3s;
            button {
                margin: 0;
                border-radius: 0;
                height: 25px;
                background: none;
                position: relative;
                padding-left: 30px;
                &:before {
                    content: "";
                    width: 30px;
                    height: 100%;
                    background: url(/images/check.png) no-repeat center;
                    background-size: 13px;
                    position: absolute;
                    left: 0;
                    top: 0;
                }
                &:hover {
                    background-color: #efefef;
                }
                &.uncheck {
                    color: #aaa;
                    &:before {
                        opacity: 0.15;
                    }
                }
            }
        }
    }
    @media (max-width: 767px) {
        .length {
            span {
                display: none;
            }
            select {
                margin: 0;
                width: 65px;
                padding: 0 6px;
            }
            .toggle {
                width: 30px;
                margin-left: 10px;
                background-position: center;
            }
        }
    }
</style>
