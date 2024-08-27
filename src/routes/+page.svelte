<script>
  import Header from "$lib/Header.svelte"
  import Card from "$lib/Card.svelte"
  import Footer from "$lib/Footer.svelte"
  import { onMount } from "svelte"

  // Constants and Variables
  const GST_RATE = 0.15
  let eventName = ""
  let availableItems = {
    breakfast: [],
    dinner: [],
    dessert: [],
  }
  let selectedItems = []
  let loading = true

  // Obtaining data from external sources
  onMount(async () => {
    try {
      const response = await fetch("https://digitech.craighead.school.nz/api/restaurant")
      const data = await response.json()

      availableItems = { ...availableItems, ...data }
    } catch (error) {
      console.error("Failed to fetch data:", error)
    } finally {
      loading = false
    }
  })

  // Adding items to a menu
  function addItem(item) {
    if (!item.selected) {
      item.selected = true
      availableItems = availableItems
      selectedItems = [...selectedItems, item]
    } else {
      alert("You've already selected the item. You can only select once.")
    }
  }

  // Removing item from menu
  function removeItem(item) {
    selectedItems = selectedItems.filter((selectedItem) => selectedItem.item !== item.item)
    item.selected = false
    availableItems = availableItems
  }

  // Calculate total cost including GST
  $: totalCost = selectedItems.reduce((total, item) => total + item.price, 0) * (1 + GST_RATE)
</script>

<Header />

<main>
  <!-- Enter the event name -->
  <div class="event-name">
    <label for="event-name">Event Name:</label>
    <input id="event-name" bind:value={eventName} placeholder="Enter event name" />
  </div>
  {#if loading}
    <div class="wrapper">
      <div class="circle"></div>
      <div class="circle"></div>
      <div class="circle"></div>
      <div class="shadow"></div>
      <div class="shadow"></div>
      <div class="shadow"></div>
      <span>Loading...</span>
    </div>
  {:else}
    <div class="menu-container">
      <!-- Listing available items by category-->
      <div class="menu">
        <h2>Available Items</h2>

        {#if availableItems.breakfast.length > 0}
          <h3>Breakfast</h3>
          {#each availableItems.breakfast as item}
            <Card {item} onAdd={addItem} isAvailableSection={true} />
          {/each}
        {/if}

        {#if availableItems.dinner.length > 0}
          <h3>Dinner</h3>
          {#each availableItems.dinner as item}
            <Card {item} onAdd={addItem} isAvailableSection={true} />
          {/each}
        {/if}

        {#if availableItems.dessert.length > 0}
          <h3>Dessert</h3>
          {#each availableItems.dessert as item}
            <Card {item} onAdd={addItem} isAvailableSection={true} />
          {/each}
        {/if}
      </div>

      <!-- Listing selected items -->
      <div class="menu">
        <h2>Selected Items for {eventName || "your event"}</h2>
        {#if selectedItems.length > 0}
          <h3>Total Cost (including GST): <span>${totalCost.toFixed(2)}</span></h3>
          {#each selectedItems as item}
            <Card {item} onRemove={removeItem} />
          {/each}
        {:else}
          <p>No items selected for your menu.</p>
        {/if}
      </div>
    </div>
  {/if}
</main>

<Footer />

<style>
  main {
    font-family: "DM Serif Text", serif;
    margin: 0;
    background-color: lightblue;
    flex-direction: row;
    gap: 20px;
  }

  .event-name {
    font-size: 21px;
    letter-spacing: 0.2cap;
    margin: 20px;
  }

  .wrapper {
    width: 200px;
    height: 60px;
    position: absolute;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);
  }
  .circle {
    width: 20px;
    height: 20px;
    position: absolute;
    border-radius: 50%;
    background-color: #fff;
    left: 15%;
    transform-origin: 50%;
    animation: circle 0.5s alternate infinite ease;
  }

  @keyframes circle {
    0% {
      top: 60px;
      height: 5px;
      border-radius: 50px 50px 25px 25px;
      transform: scaleX(1.7);
    }
    40% {
      height: 20px;
      border-radius: 50%;
      transform: scaleX(1);
    }
    100% {
      top: 0%;
    }
  }
  .circle:nth-child(2) {
    left: 45%;
    animation-delay: 0.2s;
  }
  .circle:nth-child(3) {
    left: auto;
    right: 15%;
    animation-delay: 0.3s;
  }
  .shadow {
    width: 20px;
    height: 4px;
    border-radius: 50%;
    background-color: rgba(0, 0, 0, 0.5);
    position: absolute;
    top: 62px;
    transform-origin: 50%;
    z-index: -1;
    left: 15%;
    filter: blur(1px);
    animation: shadow 0.5s alternate infinite ease;
  }

  @keyframes shadow {
    0% {
      transform: scaleX(1.5);
    }
    40% {
      transform: scaleX(1);
      opacity: 0.7;
    }
    100% {
      transform: scaleX(0.2);
      opacity: 0.4;
    }
  }
  .shadow:nth-child(4) {
    left: 45%;
    animation-delay: 0.2s;
  }
  .shadow:nth-child(5) {
    left: auto;
    right: 15%;
    animation-delay: 0.3s;
  }
  .wrapper span {
    position: absolute;
    top: 75px;
    font-family: "Lato";
    font-size: 20px;
    letter-spacing: 12px;
    color: #fff;
    left: 15%;
  }

  .menu-container {
    display: flex;
    justify-content: space-between;
  }

  .menu {
    margin: 1%;
    flex: 1;
    padding: 10px;
    border: 1px solid #ccc;
    border-radius: 5px;
    background-color: #f9f9f9;
  }

  .menu h2 {
    font-size: 24px;
    text-align: center;
    font-weight: bold;
  }

  h2 {
    letter-spacing: 0.2cap;
  }

  h3 {
    font-size: 18px;
    letter-spacing: 0.2cap;
    text-align: center;
    margin: 2%;
  }

  p {
    font-size: 18px;
    letter-spacing: 0.2cap;
    text-align: center;
    margin: 2%;
  }

  span {
    font-weight: 800;
    text-decoration: underline;
  }
</style>
