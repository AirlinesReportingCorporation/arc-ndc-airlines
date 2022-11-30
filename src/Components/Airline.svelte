<script>
  import { slide } from "svelte/transition";
  import Fa from "svelte-fa";
  import {MDBRow, MDBCol} from 'mdbsvelte';
  import { faMinus, faPlus } from "@fortawesome/free-solid-svg-icons";


  export let airline;
  export let date;
  let isOpen = false;
  const toggle = () => (isOpen = !isOpen);
</script>

<MDBRow style="align-items: center;
">
  <MDBCol size="11">
    <div class="d-flex flex-column flex-lg-row">
      <div style="display:flex; align-items:center">
        <div class="air-designator">{airline.Designator}</div>
        <div class="air-code">
          {airline["Numeric Code"].toString().length == 2
            ? "0" + airline["Numeric Code"]
            : airline["Numeric Code"]}
        </div>
      </div>
      <div class="air-name">{airline["Airline Name"]}</div>
      <div class="ml-auto d-flex align-items-center">
        <div class="air-updated" style="margin-right: 0px;">
          <span>Latest Updates:</span>
          {date}
        </div>
      </div>
    </div>
  </MDBCol>
  <MDBCol size="1" class="ml-auto">
    <button class="expand" on:click={toggle} aria-expanded={isOpen}>
      <Fa icon={isOpen ? faMinus : faPlus} />
    </button>
  </MDBCol>
</MDBRow>
{#if isOpen}
  <div transition:slide={{ duration: 300 }}>I'm open!</div>
{/if}

<style>
  .expand {
    font-size: 25px;
    cursor: pointer;
    text-align: right;
    margin-right: 5px;
    background-color: #fff;
    border: none;
    stroke: black;
    stroke-width: 25px;
  }


  .air-designator {
    font-family: SourceSansPro-SemiBold,Arial,Helvetica,sans-serif;
    background: #1d9cb1;
    width: 90px;
    font-size: 30px;
    color: #fff;
    text-align: center;
    border-radius: 8px;
    margin-right: 20px;
  }

  .air-code {
    font-family: SourceSansPro-SemiBold,Arial,Helvetica,sans-serif;
    width: 90px;
    font-size: 30px;
    color: #fff;
    text-align: center;
    border-radius: 8px;
    background: #c5b593;
    margin-right: 30px;
  }

  .air-name {
    font-family: SourceSansPro-SemiBold,Arial,Helvetica,sans-serif;
    font-size: 30px;
  }

  .air-updated{
    color: #888;
    font-size: 14px;
    line-height: 24px;
    font-family: SourceSansPro-Bold,Arial,Helvetica,sans-serif;
    margin-right: 0;
  }

  .air-updated span {
    font-family: SourceSansPro-Regular,Arial,Helvetica,sans-serif;
  }
</style>
