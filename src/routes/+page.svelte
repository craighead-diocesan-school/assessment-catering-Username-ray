<script>
  import Header from "$lib/Header.svelte"
  import { onMount } from "svelte"

  // Constants and Variables
  const GST_RATE = 0.15
  let eventName = ""
  let availableItems = []
  let selectedItems = []

  // Obtaining data from external sources
  onMount(async () => {
    try {
      const response = await fetch("https://digitech.craighead.school.nz/api/restaurant")
      availableItems = await response.json()
      console.log("Available Items:", availableItems) // For debugging
    } catch (error) {
      console.error("Failed to fetch data:", error)
    }
  })

  // Adding items to a menu
  function addItem(item) {
    selectedItems = [...selectedItems, item]
  }

  // Calculate total cost including GST
  function calculateTotalCost() {
    return selectedItems.reduce((total, item) => total + item.price, 0) * (1 + GST_RATE)
  }
</script>

<Header />

<main>
  <!-- Enter the event name -->
  <div>
    <label for="event-name">Event Name:</label>
    <input id="event-name" bind:value={eventName} placeholder="Enter event name" />
  </div>

  <!-- Listing available items -->
  <div class="menu">
    <h2>Available Items</h2>
    {#each availableItems as item}
      <div class="card">
        <h3>{item.name}</h3>
        <p>Price: ${item.price.toFixed(2)}</p>
        <button on:click={() => addItem(item)}>Add to Custom Menu</button>
      </div>
    {/each}
  </div>

  <!-- Listing selected items -->
  <div class="menu">
    <h2>Selected Items for {eventName || "your event"}</h2>
    {#if selectedItems.length > 0}
      {#each selectedItems as item}
        <div class="card selected">
          <h3>{item.name}</h3>
          <p>Price: ${item.price.toFixed(2)}</p>
        </div>
      {/each}
      <h3>Total Cost (including GST): ${calculateTotalCost().toFixed(2)}</h3>
    {:else}
      <p>No items selected for your menu.</p>
    {/if}
  </div>
</main>

<footer>
  <p>&copy; Craighead Diocesan School 2024</p>
</footer>
