<script>
    import MineSweeperCell from "./MineSweeperCell.svelte";

    export let field_size = 8;
    export let n_mines = 10;
    let field = Array(field_size + 2)
        .fill(0)
        .map((x) => Array(field_size + 2).fill(0));

    console.log(field);

    for (let i = 0; i < n_mines; i++) {
        let x = Math.floor(Math.random() * field_size + 1);
        let y = Math.floor(Math.random() * field_size + 1);
        while (field[x][y] == "mine") {
            x = Math.floor(Math.random() * field_size + 1);
            y = Math.floor(Math.random() * field_size + 1);
        }
        field[x][y] = "mine";
    }

    for (let x = 1; x < field_size + 1; x++) {
        for (let y = 1; y < field_size + 1; y++) {
            if (field[x][y] == "mine") {
                continue;
            }
            let count = 0;
            if (field[x - 1][y - 1] == "mine") {
                count++;
            }
            if (field[x - 1][y] == "mine") {
                count++;
            }
            if (field[x - 1][y + 1] == "mine") {
                count++;
            }
            if (field[x][y - 1] == "mine") {
                count++;
            }
            if (field[x][y + 1] == "mine") {
                count++;
            }
            if (field[x + 1][y - 1] == "mine") {
                count++;
            }
            if (field[x + 1][y] == "mine") {
                count++;
            }
            if (field[x + 1][y + 1] == "mine") {
                count++;
            }
            field[x][y] = count;
        }
    }
    field = field.slice(1, -1);
    field = field.map((row) => row.slice(1, -1));
</script>

<div class="field">
    {#each field as row}
        <div class="row">
            {#each row as cell}
                <MineSweeperCell content={cell} />
            {/each}
        </div>
    {/each}
</div>

<style>
    .field {
        display: flex;
        flex-direction: column;
    }
    .row {
        display: flex;
        flex-direction: row;
    }
</style>
