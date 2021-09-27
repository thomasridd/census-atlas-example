<script>
    export let selected;
    export let nearestLookup;
    export let data;
</script>

<style>
    .last-cell {
        text-align: right;
    }
</style>
<table id="basic-table" class="table ">
    <caption class="table__caption">Nearby</caption>

    <tbody class="table__body">

    {#each nearestLookup[selected].slice(1, 6) as lad}
        <tr class="table__row">
            <td class="table__cell " name="cell-name">
                {lad.name}
            </td>
            <td class="table__cell last-cell ">
                {#if data}
                    { data.lad.index[lad.code].perc.toFixed(1) } %
                    {/if}
            </td>
        </tr>
    {/each}


    </tbody>
</table>