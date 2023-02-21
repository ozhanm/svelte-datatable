<script>
    import Search from "./datatable/Search.svelte";
    import Length from "./datatable/Length.svelte";
    import Table from "./datatable/Table.svelte";
    import Pagination from "./datatable/Pagination.svelte";
    import Showing from "./datatable/Showing.svelte";

    export let config;

    let data = config.data;
    let pageIndex = config.pageIndex;
    let pageSize = config.pageSize;
    let searchText = config.searchText;
    let thead = config.thead;
    let sortColumn = config.sortColumn;
    let orderBy = config.orderBy;
    let dateFormat = config.dateFormat;

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
    const updateOrderData = (e) => {
        const o = e.detail;
        sortColumn = o.sortColumn;
        orderBy = o.orderBy;
    };
    const resetDefaultConfig = (e) => {
        searchText = config.searchText;
        pageIndex = config.pageIndex;
        pageSize = config.pageSize;
        sortColumn = config.sortColumn;
        orderBy = config.orderBy;
    };
</script>

<div class="datatable">
    <div class="top">
        <Search
            {config}
            {searchText}
            {pageIndex}
            {pageSize}
            {sortColumn}
            {orderBy}
            on:updateSearchText={updateSearchText}
            on:resetDefaultConfig={resetDefaultConfig}
        />
        <Length {pageSize} on:updatePageSize={updatePageSize} />
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
            {dateFormat}
            on:updatePagination={updatePagination}
            on:updateOrderData={updateOrderData}
        />
    </div>
    {#if totalPage > 0}
        <div class="bottom">
            <Showing {pageIndex} {pageSize} {totalLength} />
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
        .top,
        .bottom {
            display: flex;
            align-items: center;
            justify-content: space-between;
            margin: 30px 0;
        }
        @media (max-width: 767px) {
            .top {
                margin-bottom: 15px;
            }
            .bottom {
                flex-wrap: wrap;
                justify-content: center;
                margin-top: 15px;
            }
        }
    }
</style>
