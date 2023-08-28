<script>
    import { onMount } from 'svelte';

	let fontsize = 14;
    let fontweight = 400;
    let fontstyle = 'none';

    const fontConversions = [
        { sizeRange: [10, 11], style: "label1", condition: function() {return fontweight >= 700}},
        { sizeRange: [9, 13], style: "caption", condition: function() {return fontweight >= 400} },
        { sizeRange: [14, 15], style: "body1", condition: function() {return fontweight < 600} },
        { sizeRange: [14, 15], style: "body1-emphasized", condition: function() {return fontweight >= 600 }},
        { sizeRange: [16, 18], style: "body2", condition: function() {return fontweight < 600 }},
        { sizeRange: [16, 18], style: "body2-emphasized", condition: function() {return fontweight >= 600 }},
        { sizeRange: [19, 24], style: "headline-h5" },
        { sizeRange: [25, 34], style: "headline-h4" },
        { sizeRange: [35, 42], style: "headline-h3" },
        { sizeRange: [43, 52], style: "headline-h2" },
        { sizeRange: [53, Infinity], style: "headline-h1" }
    ];

    function recalculate() {
        // let's do some if/else like a PRO AI would
        for (let i = 0; i < fontConversions.length; i++) {
            const { sizeRange, style, condition } = fontConversions[i];

            if (fontsize >= sizeRange[0] && fontsize <= sizeRange[1] && (!condition || condition(fontweight))) {
                fontstyle = style;
                return;
            }
        };

        fontstyle = "none"
    }

    onMount(() => {
        recalculate();
    });
</script>

<section>
	<p>
		Input your font styles below:
	</p>
    <form>
        <label for="fontsize">Font size</label>
        <input type="number" id="fontsize" bind:value={fontsize} on:change={recalculate} />

        <br />
        <br />

        <label for="fontweight">Font weight</label>
        <select type="number" id="fontsize" bind:value={fontweight} on:change={recalculate}>
            <option value={300}>300</option>
            <option value={400}>400</option>
            <option value={500}>500</option>
            <option value={600}>600</option>
            <option value={700}>700</option>
            <option value={800}>800</option>
            <option value={900}>900</option>
        </select>

    </form>

    <h2>
    New font style:<br /> 
    <strong>
        {fontstyle !== undefined && fontstyle !== 'none' ? `@extend %${fontstyle};` : fontstyle }
    </strong>
    <br />
    <strong>
        {fontstyle !== undefined && fontstyle !== 'none' ? `@include ${fontstyle};` : fontstyle }
    </strong>
</section>

<style>
form {
    width: 500px;
    margin: 0 auto;
}

label {
    display: block;
}
</style>