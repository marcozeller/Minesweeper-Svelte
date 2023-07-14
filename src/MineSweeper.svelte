<script>
  import MineSweeperCell from "./MineSweeperCell.svelte";

  export let field_size = 8;
  export let n_mines = 10;
  let field = Array(field_size + 2)
    .fill(0)
    .map((x) => Array(field_size + 2).fill(0));

  for (let x = 0; x < field_size + 2; x++) {
    for (let y = 0; y < field_size + 2; y++) {
      field[x][y] = { content: 0 };
    }
  }

  console.log(field);

  for (let i = 0; i < n_mines; i++) {
    let x = Math.floor(Math.random() * field_size + 1);
    let y = Math.floor(Math.random() * field_size + 1);
    while (field[x][y].content == "mine") {
      x = Math.floor(Math.random() * field_size + 1);
      y = Math.floor(Math.random() * field_size + 1);
    }
    field[x][y].content = "mine";
  }

  for (let x = 1; x < field_size + 1; x++) {
    for (let y = 1; y < field_size + 1; y++) {
      field[x][y].x = x - 1;
      field[x][y].y = y - 1;
      field[x][y].state = "hidden";
      if (field[x][y].content == "mine") {
        continue;
      }
      let count = 0;
      if (field[x - 1][y - 1].content == "mine") {
        count++;
      }
      if (field[x - 1][y].content == "mine") {
        count++;
      }
      if (field[x - 1][y + 1].content == "mine") {
        count++;
      }
      if (field[x][y - 1].content == "mine") {
        count++;
      }
      if (field[x][y + 1].content == "mine") {
        count++;
      }
      if (field[x + 1][y - 1].content == "mine") {
        count++;
      }
      if (field[x + 1][y].content == "mine") {
        count++;
      }
      if (field[x + 1][y + 1].content == "mine") {
        count++;
      }
      field[x][y].content = count;
    }
  }
  field = field.slice(1, -1);
  field = field.map((row) => row.slice(1, -1));

  const loose_game = (event) => {
    alert("You loose!");
  };

  const uncover_neighbours = (x, y) => {
    field[x][y].state = "open";
    if (x > 0) {
      if (y > 0) {
        if (
          field[x - 1][y - 1].content == 0 &&
          field[x - 1][y - 1].state != "open"
        ) {
          uncover_neighbours(x - 1, y - 1);
        }
        field[x - 1][y - 1].state = "open";
      }
      if (field[x - 1][y].content == 0 && field[x - 1][y].state != "open") {
        uncover_neighbours(x - 1, y);
      }
      field[x - 1][y].state = "open";

      if (y < field_size - 1) {
        if (
          field[x - 1][y + 1].content == 0 &&
          field[x - 1][y + 1].state != "open"
        ) {
          uncover_neighbours(x - 1, y + 1);
        }
        field[x - 1][y + 1].state = "open";
      }
    }

    if (y > 0) {
      if (field[x][y - 1].content == 0 && field[x][y - 1].state != "open") {
        uncover_neighbours(x, y - 1);
      }
      field[x][y - 1].state = "open";
    }
    if (y + 1 < field_size) {
      if (field[x][y + 1].content == 0 && field[x][y + 1].state != "open") {
        uncover_neighbours(x, y + 1);
      }
      field[x][y + 1].state = "open";
    }

    if (x + 1 < field_size) {
      if (y > 0) {
        if (
          field[x + 1][y - 1].content == 0 &&
          field[x + 1][y - 1].state != "open"
        ) {
          uncover_neighbours(x + 1, y - 1);
        }
        field[x + 1][y - 1].state = "open";
      }
      if (field[x + 1][y].content == 0 && field[x + 1][y].state != "open") {
        uncover_neighbours(x + 1, y);
      }
      field[x + 1][y].state = "open";
      if (y + 1 < field_size) {
        if (
          field[x + 1][y + 1].content == 0 &&
          field[x + 1][y + 1].state != "open"
        ) {
          uncover_neighbours(x + 1, y + 1);
        }
        field[x + 1][y + 1].state = "open";
      }
    }
  };

  const zero_uncovered = (event) => {
    uncover_neighbours(event.detail.x, event.detail.y);
  };
</script>

<div class="field">
  {#each field as row}
    <div class="row">
      {#each row as cell}
        <MineSweeperCell
          content={cell.content}
          x={cell.x}
          y={cell.y}
          bind:state={cell.state}
          on:loose_game={loose_game}
          on:zero_uncovered={zero_uncovered}
        />
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
