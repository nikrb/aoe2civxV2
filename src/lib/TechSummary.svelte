<script>
  import UnitCard from "./UnitCard.svelte";
  export let display_units, display_techs;
  // not bothering with dock atm
  const buildings = ["archery", "barracks", "stable", "workshop", "monastery", "eco", "defense"];
  console.log("TechSummary - display units:", display_units);
</script>

{#each buildings as building}
  <div><b>{building}</b></div>
  <div class="row">
    {#each display_techs as t}
      <!-- blacksmith tech first -->
      {#if t.building.includes(building) && t.type !== "unique"}
        <UnitCard
          info="{t}">
        </UnitCard>
      {/if}
    {/each}
    {#each display_units as d}
      {#if d.building === building}
        <UnitCard
          info="{d}">
        </UnitCard>
      {/if}
    {/each}
    {#each display_techs as t}
      {#if t.building.includes(building) && t.type === "unique"}
        <UnitCard
          info="{t}">
        </UnitCard>
      {/if}
    {/each}
  </div>
{/each}

<style>
  .row {
    display: flex;
    justify-content: center;
  }
</style>