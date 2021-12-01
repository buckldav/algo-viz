<script>
  import nodesort from "../scripts/sort";
  import books from "../scripts/books.js";
  const colors = [
    "#ff5500",
    "#ff4518",
    "#ff3227",
    "#ff1732",
    "#fc003d",
    "#f90047",
    "#f40051",
    "#ee005b",
    "#e70064",
    "#de006d",
    "#d40077",
    "#c9007f",
    "#bc0088",
    "#ae0090",
    "#9e0097",
    "#8c009d",
    "#7800a3",
    "#6106a8",
    "#4318ab",
    "#0022ae",
  ];

  let sorting = true;

  let bookMax = 20;
  let bookMin = 3;
  let numBooks = bookMax;

  let index = Math.random() * 500 - numBooks;
  let currentArr = books.splice(index, numBooks);
  let colorMap = generateColorMap(currentArr);
  let arrays = [currentArr];

  function changeNumBooks(e) {
    numBooks = parseInt(e.target.value);
    update();
  }

  function changeSortSearch(e) {
    document.getElementById("arrs").innerHTML = "";
    sorting = !sorting;
    numBooks = bookMax;
    update();
  }

  function update() {
    currentArr = [];
    if (sorting) {
      index = Math.random() * 500 - numBooks;
      currentArr = books.splice(index, numBooks);
      colorMap = generateColorMap(currentArr);
      arrays = [currentArr];
    } else {
      index = Math.random() * 500 - numBooks;
      currentArr = books.splice(index, numBooks).sort((a, b) => {
        if (a > b) return 1;
        else if (a < b) return -1;
        else return 0;
      });
      colorMap = generateColorMap(currentArr);
      printArr(currentArr);
      printArr(currentArr);
      //   printArr(currentArr);
    }
  }

  function getColor() {
    function decimalToHex(d, padding) {
      var hex = Number(d).toString(16);
      padding =
        typeof padding === "undefined" || padding === null
          ? (padding = 2)
          : padding;

      while (hex.length < padding) {
        hex = "0" + hex;
      }

      return hex;
    }
    let R = 255;
    let G = 255;
    let B = 255;
    while (R + G + B > 450) {
      R = parseInt(Math.random() * 255);
      G = parseInt(Math.random() * 255);
      B = parseInt(Math.random() * 255);
      console.log(R, G, B);
    }
    return "#" + decimalToHex(R, 2) + decimalToHex(G, 2) + decimalToHex(B, 2);
  }

  function generateColorMap(arr) {
    let obj = {};
    let result = [...arr].sort((a, b) => {
      if (a > b) return 1;
      else if (a < b) return -1;
      else return 0;
    });
    result.forEach((val, i) => (obj[val] = colors[i]));

    return obj;
  }

  function arrayToHTML(arr) {
    let htmlString = "";
    arr.forEach((val, i) => {
      htmlString +=
        `<span style="text-overflow: ellipsis; white-space: nowrap; transform: rotate(180deg); display: inline-block; height: 150px; width: 22px; line-height: 1.5; overflow: hidden; padding: 0.5em; padding-top: 20px; writing-mode: vertical-rl; color: white; background-color: ${colorMap[val]}">` +
        val +
        "</span>";
      if (i < arr.length - 1) htmlString += "<span>&nbsp;</span>";
    });

    return htmlString;
  }

  function printArr(arr) {
    // console.log(array);
    document.getElementById("arrs").innerHTML +=
      `<li>` + arrayToHTML(arr) + "</li>";
    arrays = [...arrays, arr];
  }

  function sorter(algo) {
    let inputArray = currentArr;
    nodesort(inputArray, printArr, function (err, sortRef) {
      if (err) {
        console.error(err);
      } else {
        document.getElementById("arrs").innerHTML = "";
        let result = [];
        switch (algo) {
          case "bubble":
            result = sortRef.bubbleSort(inputArray);
            break;
          case "insert":
            result = sortRef.insertionSort(inputArray);
            printArr(result);
            break;
          case "merge":
            result = sortRef.mergeSort(inputArray);
            printArr(result);
            break;
          case "qs":
            result = sortRef.quickSort(inputArray);
            break;
          case "select":
            result = sortRef.selectionSort(inputArray);
            break;
        }
        // console.log(result);
        console.log(document.body.scrollHeight);
        window.scrollTo(0, document.body.scrollHeight);
      }
    });
  }

  function bubbleSort() {
    sorter("bubble");
  }

  function insertionSort() {
    sorter("insert");
  }

  function mergeSort() {
    sorter("merge");
  }

  function quickSort() {
    sorter("qs");
  }

  function selectionSort() {
    sorter("select");
  }
</script>

<header>
  <select
    value={sorting ? "Sorting" : "Searching"}
    on:change={changeSortSearch}
  >
    <option value="Sorting">Sorting</option>
    <option value="Searching">Searching</option>
  </select>

  <div class="book-range">
    <label for="range">Number of Books</label>
    <div>
      <span>{bookMin}</span>
      <input
        type="range"
        id="range"
        max={bookMax}
        min={bookMin}
        value={numBooks}
        on:change={changeNumBooks}
      />
      <span>{bookMax}</span>
    </div>
  </div>

  {#if sorting}
    <nav>
      <button on:click={bubbleSort}>Bubble Sort</button>
      <button on:click={insertionSort}>Insertion Sort</button>
      <button on:click={selectionSort}>Selection Sort</button>
      <span>&nbsp;&nbsp;&nbsp;</span>
      <button on:click={mergeSort}>Merge Sort</button>
      <button on:click={quickSort}>Quick Sort</button>
    </nav>
  {:else}
    <div>Searching doesn't work yet because Buckley had to go to bed.</div>
    <!-- <nav>
      <button on:click={bubbleSort}>Linear Search</button>
      <span>&nbsp;&nbsp;&nbsp;</span>
      <button on:click={mergeSort}>Binary Search</button>
    </nav> -->
  {/if}
</header>

<main>
  <ol id="arrs" class={!sorting && "searching"} />
</main>

<style>
  main {
    z-index: -1;
    padding: 1em;
    max-width: 240px;
  }

  ol {
    z-index: -1;
    margin: calc(150px - 1em) auto 0;
    height: calc(100vh - 150px - 1em);
    width: fit-content;
  }

  @media (min-width: 640px) {
    main {
      max-width: none;
    }
  }

  header {
    z-index: 1;
    background: rgb(255, 255, 255, 0.8);
    background: linear-gradient(
      180deg,
      rgba(255, 255, 255, 1) 0%,
      rgba(255, 255, 255, 1) 20%,
      rgba(255, 255, 255, 0.95) 50%,
      rgba(255, 255, 255, 0.7) 100%
    );
    position: fixed;
    top: 0;
    left: 1em;
    right: 1em;
    height: 150px;
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    align-items: center;
  }

  nav {
    display: flex;
    justify-content: center;
  }

  .book-range label {
    text-align: center;
  }

  .book-range div {
    display: flex;
    justify-content: center;
    align-items: center;
    padding: 0.25em 1em 1em;
  }

  .book-range span {
    display: inline-block;
    padding: 0.5em;
  }

  .book-range input {
    margin: 0;
  }

  .searching {
    list-style-type: none;
  }
</style>
