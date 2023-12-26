<script>
  import { onMount } from "svelte";

  // Card data
  const cards = [
    { id: 1, value: "A" },
    { id: 2, value: "B" },
    { id: 3, value: "C" },
    { id: 4, value: "D" },
    { id: 5, value: "E" },
    { id: 6, value: "F" },
    { id: 7, value: "G" },
    { id: 8, value: "H" },
    { id: 9, value: "A" },
    { id: 10, value: "B" },
    { id: 11, value: "C" },
    { id: 12, value: "D" },
    { id: 13, value: "E" },
    { id: 14, value: "F" },
    { id: 15, value: "G" },
    { id: 16, value: "H" },
  ];

  // Shuffling the cards
  let shuffledCards = [];
  onMount(() => {
    shuffledCards = shuffle([...cards, ...cards]);
  });

  // Game state
  let flippedCards = [];
  let matchedPairs = [];

  // Shuffle function
  function shuffle(array) {
    let currentIndex = array.length;
    let randomIndex;

    while (currentIndex !== 0) {
      randomIndex = Math.floor(Math.random() * currentIndex);
      currentIndex--;

      [array[currentIndex], array[randomIndex]] = [
        array[randomIndex],
        array[currentIndex],
      ];
    }

    return array;
  }

  // Card click handler
  function handleCardClick(id) {
    if (flippedCards.length < 2 && !flippedCards.includes(id)) {
      flippedCards = [...flippedCards, id];

      if (flippedCards.length === 2) {
        setTimeout(checkMatch, 1000);
      }
    }
  }

  // Check if flipped cards match
  function checkMatch() {
    const [card1, card2] = flippedCards;
    const isMatch = shuffledCards[card1 - 1].value === shuffledCards[card2 - 1].value;

    if (isMatch) {
      matchedPairs = [...matchedPairs, card1, card2];
    }

    flippedCards = [];
  }
</script>

<style>
  .grid {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 10px;
    max-width: 400px;
    margin: 20px auto;
  }

  .card {
    width: 100%;
    padding: 20px;
    text-align: center;
    background-color: #f0f0f0;
    cursor: pointer;
    user-select: none;
    font-size: 18px;
    font-weight: bold;
  }

  .flipped {
    background-color: #fff;
  }

  .matched {
    background-color: #8eff8e;
  }
</style>

<div class="grid">
  {#each shuffledCards as { id, value }, i}
    <div
      class={`card ${flippedCards.includes(i + 1) ? 'flipped' : ''} ${matchedPairs.includes(i + 1) ? 'matched' : ''}`}
      on:click={() => handleCardClick(i + 1)}
    >
      {flippedCards.includes(i + 1) || matchedPairs.includes(i + 1) ? value : '?'}
    </div>
  {/each}
</div>
