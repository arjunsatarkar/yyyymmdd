<script lang="ts">
    import { onMount } from "svelte";

    const doFormat = (dateStr: string, format: string) => {
        const dateStrSplit = dateStr.split("-");
        if (dateStrSplit.length !== 3) {
            // Date input value might be eg. empty
            return "";
        }
        const dateParts = {
            Y: dateStrSplit[0],
            M: dateStrSplit[1],
            D: dateStrSplit[2],
        };
        let result = "";
        for (const c of format) {
            if (c === "Y" || c === "M" || c === "D") {
                if (dateParts[c].length == 0) {
                    result += "";
                } else {
                    result += dateParts[c][0];
                    dateParts[c] = dateParts[c].slice(1);
                }
            } else {
                result += c;
            }
        }
        return result;
    };

    const getDateStrFromDate = (date: Date) =>
        `${date.getFullYear().toString().padStart(4, "0")}-${(date.getMonth() + 1).toString().padStart(2, "0")}-${date.getDate().toString().padStart(2, "0")}`;

    let dateStr = $state("1815-12-10"); // Ada Lovelace's birthday
    let format = $state("YYYY-MM-DD");
    let output = $derived(doFormat(dateStr, format));
    onMount(() => {
        format = new URLSearchParams(location.search).get("format") || format;
        dateStr = getDateStrFromDate(new Date());
    });
</script>

<input type="date" bind:value={dateStr} /> <br />
<input bind:value={format} /> <br />
{output}

<div id="sourceLinkContainer">
    <a href="https://github.com/arjunsatarkar/yyyymmdd">source code</a>
</div>

<style>
    #sourceLinkContainer {
        text-align: right;
    }
</style>