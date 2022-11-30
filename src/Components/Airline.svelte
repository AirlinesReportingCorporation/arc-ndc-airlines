<script>
  import { slide } from "svelte/transition";
  import Fa from 'svelte-fa'
  import { faMinus, faPlus } from '@fortawesome/free-solid-svg-icons'
  export let airline;
  export let date;
  let isOpen = false;
  const toggle = () => (isOpen = !isOpen);
</script>

<div class="airline-row">
  <div class="col-lg-11">
    <div class="d-flex flex-column flex-lg-row">
      <div class="d-flex align-items-center">
        <div class="air-deignator">{airline.Designator}</div>
        <div class="air-code">
          {airline["Numeric Code"].toString().length == 2
            ? "0" + airline["Numeric Code"]
            : airline["Numeric Code"]}
        </div>
      </div>
      <div><div class="air-name">{airline["Airline Name"]}</div></div>
      <div class="ml-auto d-flex align-items-center">
        <div class="air-updated" style="margin-right: 0px;">
          <span>Latest Updates:</span>
          {date}
        </div>
      </div>
    </div>
  </div>
  <div class="col-lg-1" on:click={toggle} aria-expanded={isOpen}><Fa icon={ isOpen ? faMinus : faPlus}></Fa></div>
</div>
{#if isOpen}
  <div transition:slide={{ duration: 300 }}>
    <div class="col-lg-11">
      <div class="d-flex flex-column flex-lg-row">
        <div class="d-flex align-items-center">
          <div class="apDesignator">{airline.Designator}</div>
          <div class="apCode">
            {airline["Numeric Code"].toString().length == 2
              ? "0" + airline["Numeric Code"]
              : airline["Numeric Code"]}
          </div>
        </div>
        <div class="apName">{airline["Airline Name"]}</div>
        <div class="ml-auto d-flex align-items-center">
          <div class="apUpdated" style="margin-right: 0px;">
            <span>Latest Updates:</span>
            {date}
          </div>
        </div>
      </div>
    </div>
  </div>
{/if}

<style>
    .airline-row {
    display: flex;
    flex-direction: row;
    align-items: center;
    }
</style>
