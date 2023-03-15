<script>
    import { createEventDispatcher } from "svelte";

    export let data,
        thead,
        pageIndex,
        pageSize,
        searchText,
        sortColumn,
        orderBy,
        dateFormat;

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
              let find = false;
              let result = false;

              thead.map((row, key) => {
                  let text = item[key].toLocaleLowerCase();
                  let number = item[key]
                      .toLocaleLowerCase()
                      .replace(/[^0-9a-zA-Z]/g, "");

                  switch (row.type) {
                      case "number":
                          find =
                              text.includes(searchText) ||
                              number.includes(searchText);
                          break;

                      case "date":
                          let date = writeTable(text, "date");
                          find =
                              text.includes(searchText) ||
                              date.includes(searchText);
                          break;

                      default:
                          find = text.includes(searchText);
                          break;
                  }
                  if (find) result = true;
              });

              return result;
          })
        : sortRows;

    $: tableRows = searchRows.slice(showStart, pageIndex * pageSize);

    $: {
        dispatch("updatePagination", searchRows.length);
    }

    // Write TD text
    const writeTable = (text, type) => {
        let result = text;

        if (type == "date" && text) {
            let date = new Date(text);
            let day = ("0" + date.getDate()).slice(-2);
            let month = ("0" + (date.getMonth() + 1)).slice(-2);
            let year = date.getFullYear();
            let hour = ("0" + date.getHours()).slice(-2);
            let minute = ("0" + date.getMinutes()).slice(-2);
            let second = ("0" + date.getSeconds()).slice(-2);
            result = dateFormat
                .toLocaleLowerCase()
                .replace("d", day)
                .replace("m", month)
                .replace("y", year)
                .replace("h", hour)
                .replace("i", minute)
                .replace("s", second);
        }

        return result;
    };

    // TH click (sort)
    const clickHandle = (index) => {
        if (sortColumn == index) {
            orderBy = orderBy == "asc" ? "desc" : "asc";
        } else {
            orderBy = "asc";
        }
        sortColumn = index;

        const orderData = {
            orderBy,
            sortColumn,
        };

        dispatch("updateOrderData", orderData);
    };
</script>

<table>
    <thead>
        <tr>
            {#each thead as th, key}
                {#if th.visible == true}
                    <th>
                        {#if th.sortable == true}
                            <button
                                class:asc={sortColumn == key &&
                                    orderBy == "asc"}
                                class:desc={sortColumn == key &&
                                    orderBy == "desc"}
                                on:click={() => clickHandle(key)}
                                >{th.title}</button
                            >
                        {:else}
                            <span>{th.title}</span>
                        {/if}
                    </th>
                {/if}
            {/each}
        </tr>
    </thead>
    <tbody>
        {#if tableRows.length > 0}
            {#each tableRows as row, key}
                <tr>
                    {#each thead as th, i}
                        {#if th.visible == true}
                            <td data-label={th.title}
                                >{writeTable(row[i], thead[i].type)}</td
                            >
                        {/if}
                    {/each}
                </tr>
            {/each}
        {:else}
            <tr>
                <td colspan="6" class="notfound">
                    No matching records found!
                </td>
            </tr>
        {/if}
    </tbody>
</table>

<style lang="scss">
    table {
        width: 100%;
        text-align: left;
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
                    span {
                        padding: 15px;
                    }
                    button {
                        width: 100%;
                        padding: 15px;
                        border: 0;
                        font-weight: 700;
                        text-align: left;
                        position: relative;
                        font-size: 15px;
                        background: url(../images/sort_both.png) no-repeat;
                        background-position: 95% center;
                        cursor: pointer;
                        &.asc {
                            background-image: url(../images/sort_asc.png);
                        }
                        &.desc {
                            background-image: url(../images/sort_desc.png);
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
    @media (max-width: 767px) {
        table {
            thead {
                display: none;
            }
            tbody {
                tr {
                    width: 100%;
                    display: block;
                    padding: 15px 0;
                    td {
                        font-weight: 400;
                        position: relative;
                        display: flex;
                        align-items: flex-start;
                        justify-content: space-between;
                        padding: 6px 15px;
                        &:before {
                            content: attr(data-label);
                            font-weight: 600;
                        }
                    }
                }
            }
        }
    }
</style>
