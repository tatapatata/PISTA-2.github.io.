<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Puzzle Deslizante 4x4</title>
<style>
    body {
        font-family: Arial, sans-serif;
        background: #111;
        color: white;
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
        height: 100vh;
        margin: 0;
    }
    h1 {
        margin-bottom: 20px;
    }
    #puzzle {
        display: grid;
        grid-template-columns: repeat(4, 80px);
        grid-template-rows: repeat(4, 80px);
        gap: 5px;
    }
    .tile {
        width: 80px;
        height: 80px;
        background: #444;
        display: flex;
        align-items: center;
        justify-content: center;
        font-size: 24px;
        cursor: pointer;
        border-radius: 8px;
        user-select: none;
        transition: background 0.2s;
    }
    .tile:hover {
        background: #666;
    }
    .empty {
        background: #111;
        cursor: default;
    }
</style>
</head>
<body>
<h1>Resuelve el Puzzle</h1>
<div id="puzzle"></div>
<script>
const size = 4;
let tiles = [];

function createTiles() {
    tiles = [];
    for (let i = 1; i < size * size; i++) {
        tiles.push(i);
    }
    tiles.push(null);
    shuffle(tiles);
}

function shuffle(array) {
    for (let i = array.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));
        [array[i], array[j]] = [array[j], array[i]];
    }
}

function drawPuzzle() {
    const puzzle = document.getElementById('puzzle');
    puzzle.innerHTML = '';
    tiles.forEach((tile, index) => {
        const tileDiv = document.createElement('div');
        tileDiv.classList.add('tile');
        if (tile === null) {
            tileDiv.classList.add('empty');
        } else {
            tileDiv.textContent = tile;
            tileDiv.addEventListener('click', () => moveTile(index));
        }
        puzzle.appendChild(tileDiv);
    });
}

function moveTile(index) {
    const emptyIndex = tiles.indexOf(null);
    const validMoves = [
        emptyIndex - 1, emptyIndex + 1,
        emptyIndex - size, emptyIndex + size
    ];
    if (validMoves.includes(index) && isAdjacent(index, emptyIndex)) {
        [tiles[index], tiles[emptyIndex]] = [tiles[emptyIndex], tiles[index]];
        drawPuzzle();
        checkWin();
    }
}

function isAdjacent(index1, index2) {
    const row1 = Math.floor(index1 / size);
    const col1 = index1 % size;
    const row2 = Math.floor(index2 / size);
    const col2 = index2 % size;
    return (Math.abs(row1 - row2) + Math.abs(col1 - col2) === 1);
}

function checkWin() {
    const correct = [...Array(size * size - 1).keys()].map(i => i + 1);
    correct.push(null);
    if (JSON.stringify(tiles) === JSON.stringify(correct)) {
        setTimeout(() => {
            alert("¡Felicidades! La contraseña es: AMOR MIOOOOOOOO");
        }, 100);
    }
}

createTiles();
drawPuzzle();
</script>
</body>
</html>
