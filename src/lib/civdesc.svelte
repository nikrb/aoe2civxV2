<script>
  import TechSummary from './TechSummary.svelte';
  import Popup from './Popup.svelte';
  export let changeCiv, civnames, toggleClick;
  let selected = "Armenians";

  let civdesc;
  let display_units;
  let display_techs;
  let display_unique;
  let show_unique = false;
  let y_offset = 0;
  let x_offset = 0;

  function getOffsets(event) {
	if( event.touches) {
		y_offset = event.touches[0].pageY;
		x_offset = event.touches[0].pageX;
	} else {
		y_offset = event.pageY;
		x_offset = event.pageX;
	}
	return {y_offset, x_offset};
  }
  const toggle = (event) => {
    if( toggleClick){
		const {y_offset, x_offset} = getOffsets(event);
		show_unique = !show_unique;
	}
  };
  const show = (event) => {
    if( !toggleClick || event.touches){
		const {y_offset, x_offset} = getOffsets(event);
		show_unique = true;	
	}
  };
  const hide = () => {
    if( !toggleClick) show_unique = false;
  };

  const changeMyCiv = (() => {
    const ret  = changeCiv(selected);
    civdesc = ret.civdesc;
    display_units = ret.display_units;
    display_techs = ret.display_techs;
    display_unique = ret.display_unique;
  });
  changeMyCiv();
</script>

<div class="civcount">
  <select bind:value={selected} on:change="{changeMyCiv}" >
    {#each civnames as civname}
      <option value={civname}>
        {civname}
      </option>
    {/each}
  </select>
</div>
<!-- svelte-a11y-ignore a11y-click-events-have-key-events -->
<div on:click={toggle} 
	on:mouseenter={show}
	on:mousemove={show}
	on:mouseleave={hide}
	on:touchstart={show}
	on:touchmove={show}
	on:touchend={hide}
	on:keydown={()=>{}}>
  {@html civdesc}
</div>
<div class="popup-outer">
  {#if show_unique}
    <div class="popup" style="top: {y_offset}px; left: calc({x_offset}px + 1rem);">
      <div class="row">
        <img src="{'https://aoe2techtree.net/img/Units/'+
                   display_unique.castle.info.info_list[0].ID+'.png'}"
                   alt="castle UU">
        <img src="{'https://aoe2techtree.net/img/Units/'+
                   display_unique.imperial.info.info_list[0].ID+'.png'}"
                   alt="imperial UU">
       </div>
       <div class="row">
        <Popup info="{display_unique.castle.info}" info_index="{0}"/>
        <Popup info="{display_unique.imperial.info}" info_index="{0}"/>
      </div>
    </div>
  {/if}
</div>
<div class="summary">
  <TechSummary display_units="{display_units}" display_techs="{display_techs}"/>
</div>

<style>
  .civcount {
    margin: 1em;
  }
  .popup-outer {
    display: relative;
  }
  .popup {
    position: absolute;
    z-index: 100;
    background-color: white;
    border: 1px solid black;
    border-radius: 5px;
    padding: 0.5em;
    font-size: 0.8em;
  }
  .row {
    display: flex;
    justify-content: space-around;
  }
  .summary {
    margin-top: 1em;
  }
</style>