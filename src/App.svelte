<script>
    import Datatable from "./Datatable.svelte";
    import { data } from "./data.js";

    let datatableConfig = {
        data,
        pageIndex: 1,
        pageSize: 15,
        sortColumn: 0,
        orderBy: "asc",
        searchText: "",
        dateFormat: "d.m.y",
        thead: [
            { type: "text", title: "Name", sortable: true },
            { type: "text", title: "Position", sortable: false },
            { type: "text", title: "Office", sortable: true },
            { type: "number", title: "Total", sortable: true },
            { type: "date", title: "Date", sortable: true },
            { type: "number", title: "Salary", sortable: true },
        ],
    };
</script>

<main>
    <Datatable config={datatableConfig} />
</main>

<style lang="scss">
    main {
        padding: 0 20px;
        height: 100%;
    }
</style>
