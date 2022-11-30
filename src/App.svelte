<script>
  // get API call from XLSX sheet
  // use Sheetjs library to read xlsx file
  import { read, utils } from "xlsx";
  // Handles on mount through svelte
  import { onMount } from "svelte";
  import Airline from "./Components/Airline.svelte";

  let airline = [];
  let date = '';

  onMount(async () => {
    //   fetch the xlsx sheet
    const file = await (
      await fetch(
        "https://www2.arccorp.com/globalassets/homepage/redesign/ndc/airlines/ndc-airlines.xlsx"
      )
    ).arrayBuffer();
    // parse the array by reading the file
    const wb = read(file);
	date = wb.Props.ModifiedDate.toString().substring(4, 10) + ", " + wb.Props.ModifiedDate.toString().substring(11, 15);
    // get the first worksheet
    const ws = wb.Sheets[wb.SheetNames[0]];
    // create objects from worksheet and update svelte state
    airline = utils.sheet_to_json(ws);
	console.log(date)
  });
</script>

<div>
	<div class="row">
		<div class="offset-lg-2 col-lg-8">
		  <div
			class="product-callout-copy text-center"
			style="font-size: 18px; line-height: 24px; margin-bottom: 30px"
		  >
			Below are the details on the airlines currently participating in ARC
			Direct Connect.
		  </div>
		</div>
	  </div>
	  <div class="row">
		  <div class="col-lg-12">
			{#each airline as a}
			<Airline airline={a} {date}/>
			{/each}
		  </div>
	  </div>
 
</div>
