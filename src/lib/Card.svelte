<script>
  export let item
  export let isSelected = false
  export let onAdd
  export let onRemove
</script>

<div class="card {isSelected ? 'selected' : ''}">
  <img src={item.img} alt={item.item} width="100" height="100" />
  <h3>{item.item}</h3>
  <p>{item.description}</p>
  <p>Price: ${item.price.toFixed(2)}</p>
  {#if isSelected}
    <button on:click={() => onRemove(item)}>Remove from Menu</button>
  {:else}
    <button on:click={() => onAdd(item)}>Add to Menu</button>
  {/if}
</div>
