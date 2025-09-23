<script>
  import Popup from './Popup.svelte';
  export let info;
  let show_info = false;
  let info_index = 0;
  let image_bounds;
  let bottom_pos = 0;

  const show = (i) => { show_info = true; info_index = i; };
  const hide = () => show_info = false;
  // units Attack, HP, LineOfSight, MeleeArmor, PierceArmor, Range, MinRange, TrainTime
  // techs ResearchTime, 
  const icon_url = 'https://aoe2techtree.net/img/'+
             info.info_type.replace(/\S/, (s)=>s.toUpperCase())+'s/'+
             info.info_list[info.info_list.length-1].id+
             '.png';

	function handleStart(event) {
		image_bounds = event.target.getBoundingClientRect();
		bottom_pos = image_bounds.top;
		console.log("bottom_pos", bottom_pos);
		handleMove(event);
	}
	function getXOffset(event) {
		let offsetX;
		if( event.touches) {
			offsetX = event.touches[0].clientX - image_bounds.left;
		} else {
			offsetX = event.clientX - image_bounds.left;
		}
		return offsetX;
	}
	function handleMove(event) {
		if(!image_bounds) return;
		const offsetX = getXOffset(event);
		const index = Math.min(
			info.info_list.length - 1,
			Math.max(
				0, 
				Math.floor((offsetX / image_bounds.width) * info.info_list.length)
			)
		);
		show(index);
	}
	function handleEnd(event) {
		hide();
		image_bounds = null;
	}
</script>

<div class="col">
	<div class="popup-container">
		<div class="icons">
			<img src="{'https://aoe2techtree.net/img/'+
				info.info_type.replace(/\S/, (s)=>s.toUpperCase())+'s/'+
				info.info_list[info.info_list.length-1].id+
				'.png' }"
				on:touchstart={handleStart}
				on:touchmove={handleMove}
				on:touchend={handleEnd}
				on:mouseenter={handleStart}
				on:mousemove={handleMove}
				on:mouseleave={handleEnd}
				alt="unit pic"
			>
		</div>
		<div class="row">
			{#each Array(info.available) as _, i}
				<div class="wbox"
					on:mouseenter={() => show(i)}
					on:mouseleave={hide}
				>
				</div>
			{/each}
			{#if show_info}
				<div class="popup">
					<Popup {info} {info_index}/>
				</div>
			{/if}
		</div>
	</div>
</div>


<style>
  img {
    width: 32px;
  }
  .icons {
    /* there's an extra 4px height coming from somewhere! */
    height: 32px;
  }
  
  .wbox {
    flex: 1;
    height: 5px;
    margin-left: 1px;
    background-color: #bbb;
  }
  .row {
    display: flex;
    justify-content: center;
    height: 10px;
    align-items: center;
  }
  .col {
    display: flex;
    flex-direction: column;
    align-items: center;
    margin: 0 2px;
  }
  .popup-container {
    position: relative;
  }
  .popup {
    bottom: 3rem;
    position: absolute;
    z-index: 100;
    background-color: white;
    border: 1px solid black;
    border-radius: 5px;
    padding: 0.5em;
    font-size: 0.8em;
  }
</style>