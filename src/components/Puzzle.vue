<template>
  <div id="app">
    <div v-if="!isGameStarted">
      <button @click="startGame">Start Game</button>
    </div>

    <div v-else>
      <h2>Historical Puzzle Game</h2>
      <div class="puzzle">
        <div
          v-for="(piece, index) in shuffledPuzzle"
          :key="index"
          class="puzzle-piece"
          :style="getPieceStyle(piece)"
          @click="movePiece(index)"
        >
          <span>{{ piece.number }}</span>
        </div>
      </div>

      <div v-if="isGameCompleted">
        <h3>Congratulations! You've solved the puzzle!</h3>
        <p>Score: {{ score }}</p>
        <p>{{ funFact }}</p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      puzzle: [
        { number: 1, position: 0 },
        { number: 2, position: 1 },
        { number: 3, position: 2 },
        { number: 4, position: 3 },
        { number: 5, position: 4 },
        { number: 6, position: 5 },
        { number: 7, position: 6 },
        { number: 8, position: 7 },
        { number: "", position: 8 },
      ],
      shuffledPuzzle: [],
      isGameStarted: false,
      isGameCompleted: false,
      score: 0,
      funFact: "",
    };
  },
  methods: {
    startGame() {
      this.isGameStarted = true;
      this.shufflePuzzle();
    },
    shufflePuzzle() {
      this.shuffledPuzzle = [...this.puzzle].sort(() => Math.random() - 0.5);
      this.isGameCompleted = false;
      this.score = 0;
    },
    movePiece(index) {
      const emptyIndex = this.shuffledPuzzle.findIndex(
        (piece) => piece.number === ""
      );
      const validMoves = [
        emptyIndex - 1,
        emptyIndex + 1,
        emptyIndex - 3,
        emptyIndex + 3,
      ];

      if (validMoves.includes(index)) {
        // Swap the pieces
        [this.shuffledPuzzle[emptyIndex], this.shuffledPuzzle[index]] = [
          this.shuffledPuzzle[index],
          this.shuffledPuzzle[emptyIndex],
        ];
        this.checkGameCompletion();
      }
    },
    checkGameCompletion() {
      const isCompleted = this.shuffledPuzzle.every(
        (piece, index) => piece.position === index
      );
      if (isCompleted) {
        this.isGameCompleted = true;
        this.funFact =
          "Rwanda's historical figure X contributed greatly to education!";
        this.score += 10; // Reward score
      }
    },
    getPieceStyle(piece) {
      const size = 100; // Define size of each piece
      return {
        width: `${size}px`,
        height: `${size}px`,
        top: `${Math.floor(piece.position / 3) * size}px`,
        left: `${(piece.position % 3) * size}px`,
        background:
          piece.number === "" ? "white" : `url('/path-to-image.jpg') no-repeat`,
        backgroundPosition:
          piece.number === ""
            ? ""
            : `-${(piece.position % 3) * size}px -${
                Math.floor(piece.position / 3) * size
              }px`,
      };
    },
  },
  created() {
    this.shufflePuzzle();
  },
};
</script>

<style scoped>
.puzzle {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-template-rows: repeat(3, 1fr);
  gap: 5px;
  margin: 20px auto;
  width: 320px;
  height: 320px;
}
.puzzle-piece {
  display: flex;
  align-items: center;
  justify-content: center;
  background-color: lightgray;
  cursor: pointer;
  font-size: 20px;
}
</style>
