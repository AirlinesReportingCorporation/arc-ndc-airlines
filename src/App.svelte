<script>
  // get API call from XLSX sheet
  // use Sheetjs library to read xlsx file
  import { read, utils } from "xlsx";
  // Handles on mount through svelte
  import { onMount } from "svelte";

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

<div class="ndc-airlines" style="display: inline-block;">
  {#each airline as a}
    <div>
      <p>
        {a.Designator}
        {a["Numeric Code"].toString().length == 2
          ? "0" + a["Numeric Code"]
          : a["Numeric Code"]}
        {a["Airline Name"]}
		Latest Updates: {date}
      </p>
    </div>
  {/each}
</div>
