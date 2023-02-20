<script>
    import Datatable from "./Datatable.svelte";
    import { data } from "./data.js";

    let datatableConfig = {
        data,
        dataFull: data,
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
            { type: "number", title: "Extn.", sortable: true },
            { type: "date", title: "Date", sortable: true },
            { type: "number", title: "Salary", sortable: true },
        ],
    };

    localStorage.clear();
    const cacheConfig = localStorage.getItem("datatableConfig");
    if (cacheConfig) {
        datatableConfig = JSON.parse(cacheConfig);
    } else {
        // Set localStorage
        localStorage.setItem(
            "datatableConfig",
            JSON.stringify(datatableConfig)
        );
    }
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
