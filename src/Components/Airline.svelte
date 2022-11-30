<script>
  import { slide } from "svelte/transition";
  import Fa from "svelte-fa";
  import { MDBRow, MDBCol } from "mdbsvelte";
  import { faMinus, faPlus } from "@fortawesome/free-solid-svg-icons";

  export let airline;
  export let date;
  let isOpen = false;
  const toggle = () => (isOpen = !isOpen);
</script>

<div class="airline-accordion">
  <div class="accordion-top">
    <div class="accordion-start">
      <MDBRow
        center
        style="align-items: center; margin-left: -1.125rem; margin-right: -1.125rem;"
      >
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
                <span>Latest Updated:</span>
                {date}
              </div>
            </div>
          </div>
        </MDBCol>
        <MDBCol size="1" style="max-width: 30px;">
          <button class="expand" on:click={toggle} aria-expanded={isOpen}>
            <Fa icon={isOpen ? faMinus : faPlus} />
          </button>
        </MDBCol>
      </MDBRow>
      {#if isOpen}
        <div transition:slide={{ duration: 300 }}>I'm open!</div>
      {/if}
    </div>
  </div>
</div>

<style>
  .accordion-top {
    font-family: SourceSansPro-Regular, Arial, Helvetica, sans-serif;
    overflow: hidden;
  }
  .airline-accordion {
    background: #f1f2f2;
    color: #212122;
    margin: 0 0 20px;
    border-radius: 10px;
    overflow: hidden;
  }

  .accordion-start {
    position: relative;
    padding: 0;
    z-index: 5;
    padding: 35px;
  }

  .expand {
    max-width: 30px;
    font-size: 25px;
    cursor: pointer;
    text-align: right;
    margin-right: 5px;
    background: #f1f2f2;
    border: none;
    stroke: black;
    stroke-width: 25px;
  }

  button:focus {
    outline: none;
}

  .air-designator {
    font-family: SourceSansPro-SemiBold, Arial, Helvetica, sans-serif;
    background: #1d9cb1;
    width: 90px;
    font-size: 30px;
    color: #fff;
    text-align: center;
    border-radius: 8px;
    margin-right: 20px;
  }

  .air-code {
    font-family: SourceSansPro-SemiBold, Arial, Helvetica, sans-serif;
    width: 90px;
    font-size: 30px;
    color: #fff;
    text-align: center;
    border-radius: 8px;
    background: #c5b593;
    margin-right: 30px;
  }

  .air-name {
    font-family: SourceSansPro-SemiBold, Arial, Helvetica, sans-serif;
    font-size: 30px;
  }

  .air-updated {
    color: #888;
    font-size: 14px;
    line-height: 24px;
    font-family: SourceSansPro-Bold, Arial, Helvetica, sans-serif;
    margin-right: 0;
  }

  .air-updated span {
    font-family: SourceSansPro-Regular, Arial, Helvetica, sans-serif;
  }
</style>
