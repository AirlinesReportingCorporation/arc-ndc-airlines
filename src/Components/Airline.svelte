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

<div
  class={isOpen
    ? "active airline-accordion product-accordion-row"
    : "airline-accordion product-accordion-row"}
>
  <div class="product-accordion-top">
    <div class="product-accordion-start">
      <div
        class="row align-items-center"
        center
        style="margin-left: -1.125rem; margin-right: -1.125rem;"
      >
        <div class="col-11">
          <div class="d-flex flex-column flex-lg-row">
            <div class="d-flex align-items-center">
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
        </div>
        <div class="col-1">
          <!-- svelte-ignore a11y-click-events-have-key-events -->
          <div
            class={isOpen ? "active open expand" : "closed expand"}
            on:click={toggle}
            aria-expanded={isOpen}
          >
            <Fa icon={isOpen ? faMinus : faPlus} />
          </div>
        </div>
      </div>
      {#if isOpen}
        <div transition:slide={{ duration: 300 }}>I'm open!</div>
      {/if}
    </div>
  </div>
</div>

<style>
  .active {
    background: #212122;
    color: #fff;
  }

  .open {
    stroke: white;
  }

  .closed {
    background: #f1f2f2;
    color: #212122;
    stroke: black;
  }
  .product-accordion-top {
    font-family: SourceSansPro-Regular, Arial, Helvetica, sans-serif;
    overflow: hidden;
  }
  .airline-accordion {
    margin: 0 0 20px;
    border-radius: 10px;
    overflow: hidden;
  }

  .product-accordion-start {
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
    border: none;
    stroke-width: 25px;
  }

  #ndc-app .product-accordion-row .col-1 {
    flex: 0 0 5%;
    max-width: 5%;
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
