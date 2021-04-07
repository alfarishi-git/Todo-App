<script>
  import { createEventDispatcher } from "svelte";
  import TodoCard from "./TodoCard.svelte";

  export let cards, listName;

  const dispatch = createEventDispatcher();

  let todo = "";

  function handleAddCard() {
    dispatch("addCard", { todo, listName });
    todo = "";
  }

  function handleDeleteCard(event) {
    let data = event.detail;
    dispatch("deleteCard", { index: data.index, listName });
  }

  function handleMoveRight(event) {
    let data = event.detail;
    dispatch("moveRight", { index: data.index, listName });
  }

  function handleMoveLeft(event) {
    let data = event.detail;
    dispatch("moveLeft", { index: data.index, listName });
  }
</script>

<div class="column is-4">
  <div class="card has-background-light">
    <div class="card-header">
      <p class="card-header-title">{listName}</p>
    </div>
    <div class="card-content">
      {#each cards as card, index}
        <TodoCard
          content={card.todo}
          {listName}
          {index}
          on:deleteCard={handleDeleteCard}
          on:moveRight={handleMoveRight}
          on:moveLeft={handleMoveLeft}
        />
      {/each}
      <input type="text" class="input is-primary mb-2" bind:value={todo} />
      <button on:click={handleAddCard} class="button is-primary"
        >Add Card</button
      >
    </div>
  </div>
</div>

<style></style>
