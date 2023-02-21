<script>
    export let pageIndex, pageSize, totalLength;

    $: pageKey = pageIndex - 1;
    $: start = pageKey * pageSize + 1;
    $: end = pageKey * pageSize + pageSize;
    $: show = totalLength > end ? end : totalLength;
</script>

<div class="showing">
    Showing {start} to {show} of {totalLength} entries
</div>

<style lang="scss">
    .showing {
        display: flex;
        font-size: 14px;
    }
</style>
