<script>
    import Search from "./datatable/Search.svelte";
    import Length from "./datatable/Length.svelte";
    import Table from "./datatable/Table.svelte";
    import Pagination from "./datatable/Pagination.svelte";

    export let config;

    let data = config.data;
    let dataFull = config.dataFull;
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
        updateLocalStorage();
    };
    const updatePageIndex = (e) => {
        pageIndex = e.detail;
        updateLocalStorage();
    };
    const updatePageSize = (e) => {
        pageIndex = 1;
        pageSize = e.detail;
        totalPage = Math.ceil(totalLength / pageSize);
        updateLocalStorage();
    };
    const updatePagination = (e) => {
        pageIndex = 1;
        totalLength = e.detail;
        updateLocalStorage();
    };
    const updateOrderData = (e) => {
        const o = e.detail;
        sortColumn = o.sortColumn;
        orderBy = o.orderBy;
        updateLocalStorage();
    };

    const updateLocalStorage = () => {
        let datatableConfig = {
            data: dataFull,
            dataFull,
            pageIndex,
            pageSize,
            sortColumn,
            orderBy,
            searchText,
            thead,
        };

        console.log(datatableConfig);

        // Set new localStorage
        localStorage.setItem(
            "datatableConfig",
            JSON.stringify(datatableConfig)
        );
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
            on:updateOrderData={updateOrderData}
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
