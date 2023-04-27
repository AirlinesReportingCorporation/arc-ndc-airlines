<script>
  // use Sheetjs library to read xlsx file
  import { read, utils } from "xlsx";
  // Handles on mount through svelte
  import { onMount } from "svelte";

  //   Components
  import Airline from "./Components/Airline.svelte";

  let airline = [];
  let airlineCombined = [];
  let date = "";

  onMount(async () => {
    //   fetch the xlsx sheet
    const file = await (
      await fetch(
        "https://www2.arccorp.com/globalassets/products--participation/direct-connect/data.xlsx"
      )
    ).arrayBuffer();
    // parse the array by reading the file
    const wb = read(file);
    date =
      wb.Props.ModifiedDate.toString().substring(4, 10) +
      ", " +
      wb.Props.ModifiedDate.toString().substring(11, 15);
    // get the first worksheet
    const ws = wb.Sheets[wb.SheetNames[0]];
    // create objects from worksheet and update svelte state
    airline = utils
      .sheet_to_json(ws)
      .sort((a, b) => a["Airline Name"].localeCompare(b["Airline Name"]));

    var current = "";
    var next = "";

    for (let i = 0; i < airline.length; i++) {
      const element = airline[i];
      var current = element;
      if (i < airline.length - 1) {
        next = airline[i + 1];
        if (current["Numeric Code"] === next["Numeric Code"]) {
          airlineCombined.push([current, next]);
        } else if (
          i > 0 &&
          airline[i - 1]["Numeric Code"] !== current["Numeric Code"]
        ) {
          airlineCombined.push([current]);
        }
      } else {
        airlineCombined.push([current]);
      }
    }

    console.log(airlineCombined);
  });
</script>

<div class="row align-items-center">
  <div class="offset-lg-2 col-lg-8 ml-auto mr-auto" style="text-align: center;">
    <div
      class="product-callout-copy text-align-center"
      style="font-size: 18px; line-height: 24px; margin-bottom: 30px; b ncolor: #868b8c;"
    >
      Below are the details on the airlines currently participating in ARC
      Direct Connect.
    </div>
  </div>
</div>
{#each airlineCombined as a}
  <Airline airline={a} {date} />
{/each}
