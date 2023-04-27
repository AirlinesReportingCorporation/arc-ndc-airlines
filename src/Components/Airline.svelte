<script>
  import { slide } from "svelte/transition";
  import Fa from "svelte-fa";
  import Card from "./Card.svelte";
  export let airline;
  export let date;
  let isOpen = false;
  const toggle = () => {
    isOpen = !isOpen;
  };

  //   Transaction array
  let transaction = airline[0]["Airline Transaction Types"].split(", ");
  let types = ["Sales", "Voids", "Refunds", "Exchanges"];
  //   Sales File Type
  let sfType = ["SPRF", "RET"];
  //   IAR Error Management
  let error = ["Corrected by Agent", "Return to Airline"];
  //   IAR Modifications
  let modifications = ["All", "None", "Commission Only"];
  //   Payment via ARC array
  let arcPayment = airline[0]["Payment via ARC"].split(", ");
  let payments = ["Cash", "Credit"];

  let airName = airline[0]["Airline Name"];
  let airName2 = "";

  let airURL = airline[0]["URL"];

  if (airName.indexOf(" via ") > -1) {
    airName = airline[0]["Airline Name"].split(" via ")[0];
    airName2 = airline[0]["Airline Name"].split(" via ")[1];
  }
</script>

<a
  class="accordion container"
  href={isOpen
    ? "#" +
      airline[0]["Airline Name"].replace(" ", "") +
      "open?utm_source=ndc_airline"
    : "#" +
      airline[0]["Airline Name"].replace(" ", "") +
      "closed?utm_source=ndc_airline"}
  on:click={toggle}
>
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
                <div class="air-designator">{airline[0].Designator}</div>
                <div class="air-code">
                  {airline[0]["Numeric Code"].toString().length == 2
                    ? "0" + airline[0]["Numeric Code"]
                    : airline[0]["Numeric Code"].toString().length == 1
                    ? "00" + airline[0]["Numeric Code"]
                    : airline[0]["Numeric Code"]}
                </div>
              </div>
              <div class="air-name">
                {airName}
              </div>
              <div class="ml-auto d-flex align-items-center">
                <a
                  href={airURL}
                  target="_blank"
                  class="apProfile d-flex align-items-center"
                  >Airline Website&nbsp;<i class="fas fa-chevron-right" /></a
                >
              </div>
            </div>
          </div>
          <div class="col-1">
            <!-- svelte-ignore a11y-click-events-have-key-events -->
            <div
              class={isOpen ? "active open apExpand" : "closed apExpand"}
              aria-expanded={isOpen}
            >
              {#if isOpen}
                <div class=" ap-close" />
              {:else}
                <div class=" ap-open" />
              {/if}
            </div>
          </div>
        </div>
        {#if isOpen}
          {#each airline as a}
            <!-- First Row -->

            {#if a["Airline Name"].indexOf("Accelya") > -1}
              <div class="via-container">
                <div class="via-tag accelya">via Accelya</div>
              </div>
            {:else}
              <div class="via-container">
                <div class="via-tag">via Airline Direct</div>
              </div>
            {/if}
            <div
              transition:slide={{ duration: 300 }}
              class="row no-gutters"
              style="padding-top: 35px; margin-left: -20px;"
            >
              <!-- SASI -->

              <Card
                col="3"
                tooltip="Four-digit code identifying the airline reservation system that
          generated the transaction. For ARC’s Direct Connect transactions,
          this is usually the carrier code + the check digit."
                bottomClass="apProcessing m-auto"
                title="System Provider (SASI)"
                buttons={[
                  {
                    item:
                      a["System Provider"].toString().length < 4
                        ? "0" + a["System Provider"].toString()
                        : a["System Provider"],
                  },
                ]}
                bottomStyle="max-width: 80px; width: auto; margin-top: 14px !important;"
              />
              <!-- Sales File Type -->
              <Card
                col="3"
                tooltip="SPRF is the standard ARC file format; RET is the standard IATA file format."
                bottomClass="d-flex justify-content-center"
                title="Sales File Type"
                buttons={[
                  {
                    item: sfType[0],
                    itemClass:
                      sfType[0] == a["Sales File Type"]
                        ? "on apButton"
                        : "apButton",
                  },
                  {
                    item: sfType[1],
                    itemClass:
                      sfType[1] == a["Sales File Type"]
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
                      error[0].indexOf(a["IAR Error Management"]) > -1
                        ? "on apButton"
                        : "apButton",
                  },
                  {
                    item: error[1],
                    itemClass:
                      error[1] === a["IAR Error Management"]
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
              style="padding-top: 35px; margin-left: -20px;"
            >
              <!-- Airline Transaction Types -->
              <Card
                col="6"
                tooltip="The transaction types an airline chooses to transmit to ARC’s Direct Connect Program."
                bottomClass="d-flex justify-content-center"
                title="Transaction Types via ARC"
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
                      modifications[0] == a["IAR Modifications"]
                        ? "on apButton"
                        : "apButton",
                  },
                  {
                    item: modifications[1],
                    itemClass:
                      modifications[1] == a["IAR Modifications"]
                        ? "on apButton"
                        : "apButton",
                  },
                  {
                    item: modifications[2],
                    itemClass:
                      modifications[2] == a["IAR Modifications"]
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
              style="padding-top: 35px; margin-left: -20px;"
            >
              <!-- Payment via ARC -->
              <Card
                col="3"
                tooltip="Forms of payment transmitted to ARC by the
          carrier for reporting through ARC’s Direct
          Connect Program."
                bottomClass="d-flex justify-content-center"
                title="Payment via ARC"
                buttons={[
                  {
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
                  },
                ]}
                bottomStyle=""
              />
              <!-- Exch & Rfnd Verification -->
              <Card
                col="3"
                tooltip="Indicates whether ARC will validate numerous data elements from the original sale."
                bottomClass="d-flex justify-content-center"
                title="Exch & Rfnd Verification"
                buttons={[
                  {
                    item: "Yes",
                    itemClass:
                      "Yes" == a["Exchange & Refund Verification"]
                        ? "ongreen apButton"
                        : "apButton",
                  },
                  {
                    item: "No",
                    itemClass:
                      "No" == a["Exchange & Refund Verification"]
                        ? "onred apButton"
                        : "apButton",
                  },
                ]}
                bottomStyle=""
              />
              <!-- IAR Manual Entries -->
              <Card
                col="6"
                tooltip="Which Direct Connect Program transactions the airline allows an agency to add in IAR."
                bottomClass="d-flex justify-content-center"
                title="IAR Manual Entries"
                buttons={[
                  {
                    item: "All",
                    itemClass:
                      "All" == a["IAR Manual Entries"]
                        ? "on apButton"
                        : "apButton",
                  },
                  {
                    item: "None",
                    itemClass:
                      "None" == a["IAR Manual Entries"]
                        ? "on apButton"
                        : "apButton",
                  },
                  {
                    item: "Void",
                    itemClass:
                      "Void" == a["IAR Manual Entries"]
                        ? "on apButton"
                        : "apButton",
                  },
                ]}
                bottomStyle=""
              />
            </div>
          {/each}
        {/if}
      </div>
    </div>
  </div>
</a>

<style>
  .active {
    background: #212122;
    color: #fff;
  }

  .open {
    stroke: white;
  }

  .closed {
    background: inherit;
    color: #212122;
    stroke: black;
  }

  .apProfile {
    color: #129bb2;
    font-family: SourceSansPro-Bold, Arial, Helvetica, sans-serif;
    font-size: 14px;
    text-transform: uppercase;
    cursor: pointer;
    pointer-events: all;
    position: relative;
    position: absolute;
    z-index: 99999999;
    right: 15px;
  }

  .apProfile i {
    font-size: 11px;
    margin-left: 3px;
  }
  .accordion.container {
    cursor: pointer;
  }

  .product-accordion-top:hover,
  .closed:hover {
    background: #dddddd;
  }

  .product-accordion-top {
    font-family: SourceSansPro-Regular, Arial, Helvetica, sans-serif;
    overflow: hidden;
  }
  .airline-accordion {
    margin: 0;
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

  #ndc-app .product-accordion-row {
    margin-bottom: 0 !important;
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

  #ndc-app .product-accordion-row {
    margin-bottom: 0 !important;
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

  .air-name-sub {
    font-family: SourceSansPro-SemiBold, Arial, Helvetica, sans-serif;
    font-size: 14px;
    color: #888888;
  }

  .via-container {
    overflow: hidden;
  }
  .via-tag {
    font-family: SourceSansPro-SemiBold, Arial, Helvetica, sans-serif;
    margin-top: 30px;
    color: #f77f00;
    font-size: 14px;
    position: relative;
  }

  .apExpand {
    font-size: 25px;
    cursor: pointer;
    text-align: right;
    margin-right: 5px;
  }

  .ap-open {
    background: url(https://www2.arccorp.com/globalassets/airline-participation/open.png)
      center center no-repeat;
    width: 22px;
    height: 14px;
  }

  .ap-close {
    background: url(https://www2.arccorp.com/globalassets/airline-participation/close.png)
      center center no-repeat;
    width: 22px;
    height: 14px;
  }

  .via-tag:after {
    content: "";
    border-top: 1px solid #f77f00;
    position: absolute;
    top: 50%;
    width: 100%;
    height: 100px;
    left: 110px;
  }

  .via-tag.accelya:after {
    left: 75px;
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

  @media screen and (max-width: 1250px) {
    .apSectionTop .apButton {
      font-size: 15px !important;
    }
  }
</style>
