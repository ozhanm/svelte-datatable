<script>
    import Search from "./datatable/Search.svelte";
    import Length from "./datatable/Length.svelte";
    import Table from "./datatable/Table.svelte";
    import Pagination from "./datatable/Pagination.svelte";
    import { data } from "./data.js";

    export let config;

    let pageIndex = config.pageIndex;
    let pageSize = config.pageSize;
    let searchText = config.searchText;
    let thead = config.thead;
    let sortColumn = config.sortColumn;
    let orderBy = config.orderBy;

    $: totalLength = data.length;
    $: totalPage = Math.ceil(totalLength / pageSize);

    const updateSearchText = (e) => {
        pageIndex = 1;
        searchText = e.detail;
        totalLength = !searchText ? data.length : totalLength;
    };
    const updatePageIndex = (e) => {
        pageIndex = e.detail;
    };
    const updatePageSize = (e) => {
        pageIndex = 1;
        pageSize = e.detail;
        totalPage = Math.ceil(totalLength / pageSize);
    };
    const updatePagination = (e) => {
        pageIndex = 1;
        totalLength = e.detail;
    };
</script>

<div class="datatable">
    <div class="top">
        <Search {searchText} on:updateSearchText={updateSearchText} />
        <Length {pageSize} {totalLength} on:updatePageSize={updatePageSize} />
    </div>
    <div class="container">
        <Table
            {data}
            {thead}
            {pageIndex}
            {pageSize}
            {searchText}
            {sortColumn}
            {orderBy}
            on:updatePagination={updatePagination}
        />
    </div>
    {#if totalPage > 0}
        <div class="bottom">
            <Pagination
                {pageIndex}
                {totalPage}
                on:updatePageIndex={updatePageIndex}
            />
        </div>
    {/if}
</div>

<style lang="scss">
    .datatable {
        width: 100%;
        .top {
            display: flex;
            align-items: flex-start;
            justify-content: space-between;
        }
    }
</style>
