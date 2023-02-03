<script>
    import Datatable from "./Datatable.svelte";

    const datatableConfig = {
        pageIndex: 1,
        pageSize: 15,
        sortColumn: 0,
        orderBy: "asc",
        searchText: "",
        thead: [
            { type: "text", title: "Name" },
            { type: "text", title: "Position" },
            { type: "text", title: "Office" },
            { type: "number", title: "Extn." },
            { type: "date", title: "Date" },
            { type: "number", title: "Salary" },
        ],
    };
</script>

<main>
    <Datatable config={datatableConfig} />
</main>

<style lang="scss">
    main {
        padding: 30px;
        height: 100%;
    }
</style>
