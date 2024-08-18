<script>
  export let item
  export let isSelected = false
  export let onAdd
  export let onRemove
</script>

<div class="card">
  <img src={item.img} alt={item.item} width="100" height="100" />
  <h3>{item.item}</h3>
  <p>{item.description}</p>
  <p>Price: ${item.price.toFixed(2)}</p>
  {#if isSelected}
    <button class="remove-button" on:click={() => onRemove(item)}>Remove from Menu</button>
  {:else}
    <button class="add-button" on:click={() => onAdd(item)}>Add to Menu</button>
  {/if}
</div>

<style>
  .card {
    font-family: "DM Serif Text", serif;
    letter-spacing: 0.2cap;
    margin: 2%;
    background-color: #fff;
    border-radius: 8px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
    overflow: hidden;
    transition:
      transform 0.3s ease,
      box-shadow 0.3s ease;
    text-align: center;
    padding: 15px;
    margin: 10px 0;
  }

  .card:hover {
    background-color: azure;
    transform: translateY(-5px);
    box-shadow: 0 8px 16px rgba(0, 0, 0, 0.2);
  }

  img {
    width: 70%;
    height: auto;
    border-bottom: 1px solid #ddd;
    margin: 15px 0px;
  }

  h3 {
    font-size: 1.5em;
    margin: 10px 0;
  }

  p {
    font-size: 1em;
    color: #555;
    margin: 5px 0;
  }

  button {
    color: #fff;
    border: none;
    border-radius: 5px;
    padding: 10px 20px;
    margin: 1%;
    cursor: pointer;
    font-size: 1em;
    transition: background-color 0.3s ease;
  }

  .remove-button {
    background-color: rgb(235, 43, 43);
  }

  .add-button {
    background-color: #007bff;
  }

  .remove-button:hover {
    background-color: #b01207;
  }

  .add-button:hover {
    background-color: #0056b3;
  }

  button:focus {
    outline: none;
  }
</style>
