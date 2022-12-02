<script>
  import { slide } from "svelte/transition";
  import Fa from "svelte-fa";
  import { faMinus, faPlus } from "@fortawesome/free-solid-svg-icons";
  import Card from "./Card.svelte";
  export let airline;
  export let date;
  let isOpen = false;
  const toggle = () => (isOpen = !isOpen);

  //   Transaction array
  let transaction = airline["Airline Transaction Types"].split(", ");
  let types = ["Sales", "Voids", "Refunds", "Exchanges"];
  //   Sales File Type
  let sfType = ["SPRF", "RET"];
  //   IAR Error Management
  let error = ["Corrected By Agent", "Return to Airline"];
  //   IAR Modifications
  let modifications = ["All", "None", "Commission Only"];
//   Payment via ARC array
  let arcPayment = airline["Payment via ARC"].split(', ');
  let payments = ["Cash", "Credit"];

  console.log(airline["IAR Manual Entries"])

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
        <!-- First Row -->
        <div
          transition:slide={{ duration: 300 }}
          class="row no-gutters"
          style="padding-top: 35px;"
        >
          <!-- SASI -->
          <Card
            col="3"
            tooltip="Four-digit code identifying the airline reservation system that
            generated the transaction. For ARC’s Direct Connect transactions,
            this is usually the carrier code + the check digit."
            bottomClass="apProcessing m-auto"
            title="System Provider (SASI)"
            buttons={[{ item: airline["System Provider"], itemClass: "" }]}
            bottomStyle="max-width: 80px; width: auto; margin-top: 14px !important;"
          />
          <!-- Sales File Type -->
          <Card
            col="3"
            tooltip="RET is the standard IATA file format; SPRF is the standard ARC file format."
            bottomClass="d-flex justify-content-center"
            title="Sales File Type"
            buttons={[
              {
                item: sfType[0],
                itemClass:
                  sfType[0] == airline["Sales File Type"]
                    ? "on apButton"
                    : "apButton",
              },
              {
                item: sfType[1],
                itemClass:
                  sfType[1] == airline["Sales File Type"]
                    ? "on apButton"
                    : "apButton",
              },
            ]}
            bottomStyle=""
          />
          <!-- IAR Error Management -->
          <Card
            col="6"
            tooltip="How the airline chooses to manage transactions that trigger an IAR business edit."
            bottomClass="d-flex justify-content-center"
            title="IAR Error Management"
            buttons={[
              {
                item: error[0],
                itemClass:
                  error[0] == airline["IAR Error Management"]
                    ? "on apButton"
                    : "apButton",
              },
              {
                item: error[1],
                itemClass:
                  error[1] == airline["IAR Error Management"]
                    ? "on apButton"
                    : "apButton",
              },
            ]}
            bottomStyle=""
          />
        </div>
        <!-- Second Row -->
        <div
          transition:slide={{ duration: 300 }}
          class="row no-gutters"
          style="padding-top: 35px;"
        >
          <!-- Airline Transaction Types -->
          <Card
            col="6"
            tooltip="The transaction types an airline chooses to transmit to ARC’s Direct Connect Program."
            bottomClass="d-flex justify-content-center"
            title="System Provider (SASI)"
            buttons={[
              {
                item: types[0],
                itemClass: transaction.includes(types[0])
                  ? "on apButton"
                  : "apButton",
              },
              {
                item: types[1],
                itemClass: transaction.includes(types[1])
                  ? "on apButton"
                  : "apButton",
              },
              {
                item: types[2],
                itemClass: transaction.includes(types[2])
                  ? "on apButton"
                  : "apButton",
              },
              {
                item: types[3],
                itemClass: transaction.includes(types[3])
                  ? "on apButton"
                  : "apButton",
              },
            ]}
            bottomStyle=""
          />
          <Card
            col="6"
            tooltip="The airline’s choice of which standard IAR fields remain open for agency modification (not just those in error). The standard modification fields are Commission, Waiver Code, Tour Code, Certificate, and Ticket Designator."
            bottomClass="d-flex justify-content-center"
            title="IAR Modifications"
            buttons={[
              {
                item: modifications[0],
                itemClass:
                  modifications[0] == airline["IAR Modifications"]
                    ? "on apButton"
                    : "apButton",
              },
              {
                item: modifications[1],
                itemClass:
                  modifications[1] == airline["IAR Modifications"]
                    ? "on apButton"
                    : "apButton",
              },
              {
                item: modifications[2],
                itemClass:
                  modifications[2] == airline["IAR Modifications"]
                    ? "on apButton"
                    : "apButton",
              },
            ]}
            bottomStyle=""
          />
        </div>
        <!-- Third  Row -->
        <div
          transition:slide={{ duration: 300 }}
          class="row no-gutters"
          style="padding-top: 35px;"
        >
          <!-- Payment via ARC -->
          <Card
            col="3"
            tooltip="Four-digit code identifying the airline reservation system that
       generated the transaction. For ARC’s Direct Connect transactions,
       this is usually the carrier code + the check digit."
            bottomClass="d-flex justify-content-center"
            title="Payment via ARC"
            buttons={[{
                item: payments[0],
                itemClass: payments.includes(arcPayment[0])
                  ? "ongreen apButton"
                  : "apButton",
              },
              {
                item: payments[1],
                itemClass: payments.includes(arcPayment[1])
                  ? "ongreen apButton"
                  : "apButton",
              },]}
            bottomStyle=""
          />
          <!-- Exch & Rfnd Verification -->
          <Card
            col="3"
            tooltip="RET is the standard IATA file format; SPRF is the standard ARC file format."
            bottomClass="d-flex justify-content-center"
            title="Exch & Rfnd Verification"
            buttons={[
              {
                item: "Yes",
                itemClass:
                  "Yes" == airline["Exchange & Refund Verification"]
                    ? "ongreen apButton"
                    : "apButton",
              },
              {
                item: "No",
                itemClass:
                  "No" == airline["Exchange & Refund Verification"]
                    ? "onred apButton"
                    : "apButton",
              },
            ]}
            bottomStyle=""
          />
          <!-- IAR Manual Entries -->
          <Card
            col="6"
            tooltip="How the airline chooses to manage transactions that trigger an IAR business edit."
            bottomClass="d-flex justify-content-center"
            title="IAR Manual Entries"
            buttons={[
              {
                item: "All",
                itemClass:
                "All" == airline["IAR Manual Entries"]
                    ? "on apButton"
                    : "apButton",
              },
              {
                item: "None",
                itemClass:
                "None" == airline["IAR Manual Entries"]
                    ? "on apButton"
                    : "apButton",
              },
              {
                item: "Void",
                itemClass:
                "Void" == airline["IAR Manual Entries"]
                    ? "on apButton"
                    : "apButton",
              },
            ]}
            bottomStyle=""
          />
        </div>
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
