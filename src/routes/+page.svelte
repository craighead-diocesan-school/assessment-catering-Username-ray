<script>
  import Header from "$lib/Header.svelte"
  import Card from "$lib/Card.svelte"
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

  // Obtaining data from external sources
  onMount(async () => {
    try {
      const response = await fetch("https://digitech.craighead.school.nz/api/restaurant")
      const data = await response.json()

      availableItems = {
        breakfast: data.breakfast,
        dinner: data.dinner,
        dessert: data.dessert,
      }
      console.log("Available Items:", availableItems) // For debugging
    } catch (error) {
      console.error("Failed to fetch data:", error)
    }
  })

  // Adding items to a menu
  function addItem(item) {
    selectedItems = [...selectedItems, item]
  }

  // Removing item from menu
  function removeItem(item) {
    selectedItems = selectedItems.filter((selectedItem) => selectedItem.item !== item.item)
  }

  // Calculate total cost including GST
  $: totalCost = selectedItems.reduce((total, item) => total + item.price, 0) * (1 + GST_RATE)
</script>

<Header />

<main>
  <!-- Enter the event name -->
  <div>
    <label for="event-name">Event Name:</label>
    <input id="event-name" bind:value={eventName} placeholder="Enter event name" />
  </div>

  <!-- Listing available items by category-->
  <div class="menu">
    <h2>Available Items</h2>

    {#if availableItems.breakfast.length > 0}
      <h3>Breakfast</h3>
      {#each availableItems.breakfast as item}
        <Card {item} onAdd={addItem} />
      {/each}
    {/if}

    {#if availableItems.dinner.length > 0}
      <h3>Dinner</h3>
      {#each availableItems.dinner as item}
        <Card {item} onAdd={addItem} />
      {/each}
    {/if}

    {#if availableItems.dessert.length > 0}
      <h3>Dessert</h3>
      {#each availableItems.dessert as item}
        <Card {item} onAdd={addItem} />
      {/each}
    {/if}
  </div>

  <!-- Listing selected items -->
  <div class="menu">
    <h2>Selected Items for {eventName || "your event"}</h2>
    {#if selectedItems.length > 0}
      {#each selectedItems as item}
        <Card {item} isSelected={true} onRemove={removeItem} />
      {/each}
      <h3>Total Cost (including GST): ${totalCost().toFixed(2)}</h3>
    {:else}
      <p>No items selected for your menu.</p>
    {/if}
  </div>
</main>

<footer>
  <p>&copy; Craighead Diocesan School 2024</p>
</footer>
