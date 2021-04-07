<script>
  import CardList from "./CardList.svelte";

  let loadTaskCards = JSON.parse(localStorage.getItem("taskCards"));
  let loadInProgressCards = JSON.parse(localStorage.getItem("inProgressCards"));
  let loadDoneCards = JSON.parse(localStorage.getItem("doneCards"));

  let taskCards = loadTaskCards ? loadTaskCards : [];
  let inProgressCards = loadInProgressCards ? loadInProgressCards : [];
  let doneCards = loadDoneCards ? loadDoneCards : [];

  function handleEventAddCard(event) {
    let data = event.detail;

    if (data.listName == "Tasks") {
      taskCards = [...taskCards, { todo: data.todo }];
      localStorage.setItem("taskCards", JSON.stringify(taskCards));
    } else if (data.listName == "In Progress") {
      inProgressCards = [...inProgressCards, { todo: data.todo }];
      localStorage.setItem("inProgressCards", JSON.stringify(inProgressCards));
    } else {
      doneCards = [...doneCards, { todo: data.todo }];
      localStorage.setItem("doneCards", JSON.stringify(doneCards));
    }
  }

  function handleEventDeleteCard(event) {
    let data = event.detail;

    if (data.listName == "Tasks") {
      taskCards.splice(data.index, 1);
      taskCards = taskCards;
      localStorage.setItem("taskCards", JSON.stringify(taskCards));
    } else if (data.listName == "In Progress") {
      inProgressCards.splice(data.index, 1);
      inProgressCards = inProgressCards;
      localStorage.setItem("inProgressCards", JSON.stringify(inProgressCards));
    } else {
      doneCards.splice(data.index, 1);
      doneCards = doneCards;
      localStorage.setItem("doneCards", JSON.stringify(doneCards));
    }
  }

  function handleEventMoveRight(event) {
    let data = event.detail;

    if (data.listName == "Tasks") {
      let cardToMove = taskCards.splice(data.index, 1);
      inProgressCards = [...inProgressCards, cardToMove[0]];
      taskCards = taskCards;
      localStorage.setItem("taskCards", JSON.stringify(taskCards));
      localStorage.setItem("inProgressCards", JSON.stringify(inProgressCards));
    } else {
      let cardToMove = inProgressCards.splice(data.index, 1);
      doneCards = [...doneCards, cardToMove[0]];
      inProgressCards = inProgressCards;
      localStorage.setItem("inProgressCards", JSON.stringify(inProgressCards));
      localStorage.setItem("doneCards", JSON.stringify(doneCards));
    }
  }

  function handleEventMoveLeft(event) {
    let data = event.detail;

    if (data.listName == "In Progress") {
      let cardToMove = inProgressCards.splice(data.index, 1);
      taskCards = [...taskCards, cardToMove[0]];
      inProgressCards = inProgressCards;
      localStorage.setItem("inProgressCards", JSON.stringify(inProgressCards));
      localStorage.setItem("taskCards", JSON.stringify(taskCards));
    } else {
      let cardToMove = doneCards.splice(data.index, 1);
      inProgressCards = [...inProgressCards, cardToMove[0]];
      doneCards = doneCards;
      localStorage.setItem("inProgressCards", JSON.stringify(inProgressCards));
      localStorage.setItem("doneCards", JSON.stringify(doneCards));
    }
  }
</script>

<div class="container is-fluid">
  <h1 class="is-size-2">Todo App</h1>
  <div class="columns">
    <CardList
      cards={taskCards}
      listName={"Tasks"}
      on:addCard={handleEventAddCard}
      on:deleteCard={handleEventDeleteCard}
      on:moveRight={handleEventMoveRight}
    />
    <CardList
      cards={inProgressCards}
      listName={"In Progress"}
      on:addCard={handleEventAddCard}
      on:deleteCard={handleEventDeleteCard}
      on:moveRight={handleEventMoveRight}
      on:moveLeft={handleEventMoveLeft}
    />
    <CardList
      cards={doneCards}
      listName={"Done"}
      on:addCard={handleEventAddCard}
      on:deleteCard={handleEventDeleteCard}
      on:moveLeft={handleEventMoveLeft}
    />
  </div>
</div>

<svelte:head>
  <link rel="stylesheet" href="/bulma.min.css" />
  <script
    src="https://kit.fontawesome.com/9bf91a429b.js"
    crossorigin="anonymous"></script>
</svelte:head>
