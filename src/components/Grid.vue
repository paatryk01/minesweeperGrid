<template>
  <div class="minefield">
    <form class="options" @submit.prevent="createGrid">
      <p>Size: </p>
      <input
      type="number"
      min="5"
      max="15"
      v-model.number="width" />
      <p>Bombs: </p>
      <input
      min="5"
      max="20"
      type="number"
      v-model.number="bombs" />
    </form>
    <button class="createButton" @click="createGrid()">Create grid</button>
    <table cellspacing="0">
    </table>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';

@Component
export default class Grid extends Vue {
  width = 10;

  bombs = 10;

  createGrid() {
    const table = document.getElementsByTagName('table')[0];
    while (table.rows.length > 0) {
      table.deleteRow(0);
    }
    for (let i = 1; i <= this.width; i += 1) {
      const tr = document.createElement('tr');
      table.appendChild(tr);
      for (let j = 1; j <= this.width; j += 1) {
        const currTr = document.getElementsByTagName('tr')[i - 1];
        const td = document.createElement('td');
        currTr.appendChild(td);
      }
    }
    let assignedBombs = 0;
    while (assignedBombs < this.bombs) {
      const rowIndex = this.randNum();
      const colIndex = this.randNum();
      if (colIndex !== 0 && rowIndex !== 0) {
        const currentTr = document.getElementsByTagName('tr')[rowIndex - 1].children[colIndex - 1];
        if (currentTr.className !== 'bomb') {
          currentTr.className = 'bomb';
          assignedBombs += 1;
        }
      }
    }
  }

  randNum() {
    const randomNumber = Math.floor(Math.random() * this.width + 1);
    return randomNumber;
  }
}

</script>
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss">

  table {
    position: absolute;
    left: 50%;
    transform: translateX(-50%);
    top: 300px;
    margin: 30px 0px 150px;
  }

  .createButton {
    margin-top: 20px;
    background-color: transparent;
    border: solid 1px black;
    padding: 5px 12px;

    &:hover {
      transition: 1s;
      background-color: cyan;
      cursor: pointer;
    }

    &:focus {
      outline:none;
    }
  }

  .bomb {
    background-color: red;
  }

  .options {
    display: flex;
    justify-content: center;
    font-weight: 600;

    input {
      margin: 0 15px;
      background: transparent;
      width: 50px;
      border: 0px;
      border-bottom: 2px solid #000;
      font-family: "Righteous";
      font-size: 16px;
      text-align: center;

      &:focus {
        outline: none;
      }
    }
  }

  td {
    height:20px;
    width:20px;
    background:#808080;
    border:2px outset white;
    color:rgba(0,0,0,0);

    &:active {
      border: 2px inset white;
    }
  }
</style>
