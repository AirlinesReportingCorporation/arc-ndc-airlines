<script>
  export let col;
  export let title;
  export let bottomClass;
  export let buttons = [];
  export let bottomStyle;
  export let tooltip;
</script>

<div class="col-lg-{col}">
  <div class="apSection apSectionMarginLeft product-accordion-pb">
    <div class="apSectionTop">
      <div class="d-flex align-items-center">
        <h3>{title}</h3>
        <div class="ml-auto">
          <div class="airlineTooltip">
            <img
              src="https://www2.arccorp.com/globalassets/about-us/our-data/redesign/tooltip.png"
              alt="Help"
            />
            <div class="airlineTooltipText">
              {tooltip}
            </div>
          </div>
        </div>
      </div>
    </div>
    <div class="apSectionBottom">
      <div
        class={bottomClass}
        style={bottomStyle}
      >
        {#each buttons as button}
          <div class={button.itemClass}>{button.item}</div>
        {/each}
      </div>
    </div>
  </div>
</div>
