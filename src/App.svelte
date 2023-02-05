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
        thead: [
            { type: "text", title: "Name" },
            { type: "text", title: "Position" },
            { type: "text", title: "Office" },
            { type: "number", title: "Extn." },
            { type: "date", title: "Date" },
            { type: "number", title: "Salary" },
        ],
    };

    //localStorage.clear();
    const cacheConfig = localStorage.getItem("datatableConfig");
    if (cacheConfig) {
        datatableConfig = JSON.parse(cacheConfig);
    } else {
        // Set localStorage
        localStorage.setItem("datatableConfig", JSON.stringify(datatableConfig));
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
