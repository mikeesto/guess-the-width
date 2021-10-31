<script>
  let width = 100;
  let calcWidth = 0;
  let startX = 0;
  const minWidth = 10;
  let maxWidth = Math.floor(window.innerWidth * 0.9);
  let randomWidth = generateRandomWidth();
  let gameOver = false;

  function handleMouseDown(event) {
    if (!gameOver) {
      startX = event.clientX;

      const styles = window.getComputedStyle(
        document.getElementById("rectangle")
      );
      calcWidth = parseInt(styles.width, 10);

      window.addEventListener("mousemove", handleMouseMove);
      window.addEventListener("mouseup", handleMouseUp);
    }
  }

  function handleMouseUp() {
    window.removeEventListener("handlemouseup", handleMouseDown);
    window.removeEventListener("mousemove", handleMouseMove);
    gameOver = true;
  }

  function handleMouseMove(event) {
    const dx = event.clientX - startX;

    const newWidth = calcWidth + dx;
    maxWidth = Math.floor(window.innerWidth * 0.9);

    // Stop the rectangle from becoming too small or wide
    if (newWidth < minWidth) {
      width = minWidth;
    } else if (newWidth > maxWidth) {
      width = maxWidth;
    } else {
      width = newWidth;
    }
  }

  function restart() {
    width = 100;
    randomWidth = generateRandomWidth();
    gameOver = false;
  }

  function generateRandomWidth() {
    let w = Math.floor(Math.random() * (maxWidth - minWidth + 1) + minWidth);
    return Math.ceil(w / 10) * 10; // Round number to greatest multiple of 10
  }
</script>

<main>
  <h1>Guess the width</h1>
  <p>Make the rectangle {randomWidth}px wide</p>
  {#if gameOver}
    <p>
      {randomWidth === width ? "Amazing!" : "Almost!"} Your rectangle was {width}px
      wide
    </p>
    <button on:click={restart}>Try again</button>
  {/if}
  <div id="rectangle" style="width: {width}px">
    <div id="resizer" on:mousedown={handleMouseDown} />
    {#if gameOver}
      <div id="overlay" style="width: {randomWidth}px" />
    {/if}
  </div>
</main>

<style>
  #rectangle {
    border: 2px solid blue;
    height: 100px;
    position: relative;
  }

  #resizer {
    cursor: col-resize;
    height: 100%;
    right: 0;
    top: 0;
    width: 5px;
    position: absolute;
  }

  #overlay {
    box-shadow: 0 0 0 3px red;
    opacity: 0.4;
    height: 100px;
    position: absolute;
    top: 0;
    left: 0;
  }

  button {
    cursor: pointer;
  }
</style>
