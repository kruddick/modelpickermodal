<script>
  	import models from "./models";
	import { onMount } from "svelte";

	let callback = null;

    let show = false;

    function eventCallback(messageName, messageBody) {
		console.log("eventCallback: " + messageName);
		if(messageName === "model-picker-show") {
			show = true;
			callback = messageBody;
		} else if(messageName === "model-picker-hide") {
			show = false;			
		}		
    }

	function modelSelected(model) {
		console.log(model);
		if (callback) {
			callback(model);
		}
	}

    onMount(() => {
        console.log("onMount");
        if(window.messageBus.register) {
            window.messageBus.register(eventCallback);
            console.log("onMount: registered")
        }
    });


</script>

{#if show}
  <div class="overlay">
   <div class="model-list-container">
	   {#each models as model}
	   <div class="model-list-row" on:click={() => modelSelected(model)}>
         <div class="model-cell">{model.APLID}</div>
          <div class="model-cell-wide">{model.ModelName}</div>
          <div class="model-cell">{model.StrategistName}</div>
          <div class="model-cell">{model.StrategyName}</div>
		</div>
      {/each}
   </div>
  </div>
{/if}

<style>
  .overlay {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(255, 255, 255, 0.7);
    z-index: 9999;
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .model-list-row {
    display: flex;
  }
  
  .model-list-container {
    height: 300px;
  width: 800px;
  overflow: auto;
  font-size: 12px;
  }

  .model-cell {
    padding-top: 5px;
  padding-bottom: 5px;
  border: 1px solid black;
  width: 100px;
  overflow: hidden;
  }

  .model-cell-wide {
    padding-top: 5px;
  padding-bottom: 5px;
  border: 1px solid black;
  width: 400px;
  overflow: hidden;
  }
</style>