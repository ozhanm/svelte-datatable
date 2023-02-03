<script>
    import { createEventDispatcher } from "svelte";

    export let data, thead, pageIndex, pageSize, searchText, sortColumn, orderBy;

    let dispatch = createEventDispatcher();

    $: showStart = (pageIndex - 1) * pageSize;

    // Sırala
    $: sortRows = data.sort((a, b) => {
        let x1 = a[sortColumn].replace(/[^0-9a-zA-Z]/g, "");
        let x2 = b[sortColumn].replace(/[^0-9a-zA-Z]/g, "");

        if (orderBy == "desc") {
            let x3 = x2;
            x2 = x1;
            x1 = x3;
        }

        return x1.localeCompare(x2, undefined, { numeric: true });
    });
    // Ara
    $: searchRows = searchText
        ? sortRows.filter((item) => {
              return (
                  item[0].toLocaleLowerCase().includes(searchText) ||
                  item[1].toLocaleLowerCase().includes(searchText) ||
                  item[2].toLocaleLowerCase().includes(searchText) ||
                  item[3].toLocaleLowerCase().includes(searchText) ||
                  item[4].toLocaleLowerCase().includes(searchText) ||
                  item[5].toLocaleLowerCase().includes(searchText) ||
                  item[5]
                      .replace(/[^0-9a-zA-Z]/g, "")
                      .toLocaleLowerCase()
                      .includes(searchText)
              );
          })
        : sortRows;

    $: tableRows = searchRows.slice(showStart, pageIndex * pageSize);

    $: {
        dispatch("updatePagination", searchRows.length);
    }

    // TH click (sort)
    const clickHandle = (index) => {
        if (sortColumn == index) {
            orderBy = orderBy == "asc" ? "desc" : "asc";
        } else {
            orderBy = "asc";
        }
        sortColumn = index;
    };
</script>

<div class="table">
    <table>
        <thead>
            <tr>
                {#each thead as row, key}
                    <th>
                        <button
                            class:asc={sortColumn == key && orderBy == "asc"}
                            class:desc={sortColumn == key && orderBy == "desc"}
                            on:click={() => clickHandle(key)}>{row.title}</button
                        >
                    </th>
                {/each}
            </tr>
        </thead>
        <tbody>
            {#if tableRows.length > 0}
                {#each tableRows as row, key}
                    <tr>
                        <td>{row[0]}</td>
                        <td>{row[1]}</td>
                        <td>{row[2]}</td>
                        <td>{row[3]}</td>
                        <td>{row[4]}</td>
                        <td>{row[5]}</td>
                    </tr>
                {/each}
            {:else}
                <tr>
                    <td colspan="6" class="notfound"
                        >No matching records found!</td
                    >
                </tr>
            {/if}
        </tbody>
    </table>
</div>

<style lang="scss">
    table {
        width: 100%;
        text-align: left;
        margin-top: 30px;
        border-collapse: collapse;
        border-spacing: 0;
        font-size: 14px;
        border: 1px solid #ddd;
        thead {
            tr {
                th {
                    padding: 0;
                    font-size: 15px;
                    border-bottom: 1px solid #aaa;
                    button {
                        width: 100%;
                        padding: 15px;
                        text-align: left;
                        border: 0;
                        font-weight: 700;
                        position: relative;
                        background: url(/images/sort_both.png) no-repeat;
                        background-position: 95% center;
                        cursor: pointer;
                        &.asc {
                            background-image: url(/images/sort_asc.png);
                        }
                        &.desc {
                            background-image: url(/images/sort_desc.png);
                        }
                    }
                }
            }
        }
        tbody {
            tr {
                &:nth-child(even) {
                    background: #f6f6f6;
                }
                td {
                    padding: 12px 15px;
                    &.notfound {
                        padding: 15px;
                        font-size: 16px;
                        text-align: center;
                    }
                }
            }
        }
    }
</style>
