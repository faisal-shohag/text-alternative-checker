<script>
	import FormUrl from './FormUrl.svelte'
    let isLoading = false;
	let isResult = false;
	let alt;
	let no_alt;
	let garbage;

const getResults =(e) => {
	isLoading = e.detail.isLoading;

	if(!isLoading){
	alt = e.detail.altTextImg;
	no_alt = e.detail.noAltTextImg;
	garbage = e.detail.garbage;
	isResult = true;
	}
	
	
}



</script>

<main>
	<div class="head"><img src="images/alt.png" alt="logo">Text Alternative Checker</div>
	<FormUrl on:results={getResults}/>
	{#if isLoading}
	<div class="loading" {isLoading}>
		<div class="lds-dual-ring"></div>
	Looking up site...</div>
	{/if}
    
	{#if isResult}

	<div class="count">
		<div class="alt">
			<div class="count-alt">{alt.length}</div>
			<div class="count-alt-text">Text Alternative</div>
		</div>
		<div class="alt">
			<div class="count-noalt">{no_alt.length}</div>
			<div class="count-noalt-text">No Text Alternative</div>

		</div>
		<div class="alt">
			<div class="count-noalt">{garbage.length}</div>
			<div class="count-noalt-text">Without img tags</div>

		</div>
	</div>
  
	{#if no_alt.length > 0}
		<div class="status"><img src="images/warning.png" alt="warning"> A11y: Your website is not fully accessible! <div class="info"><a href="https://www.w3.org/WAI/WCAG21/Understanding/text-alternatives"><img src="images/info.png" alt="info"/></a></div></div>
	{:else}
	<div class="status success"><img src="images/success.png" alt="success"> A11y: Website has Text Alternatives! <div class="info"><a href="https://www.w3.org/WAI/WCAG21/Understanding/text-alternatives"><img src="images/info.png" alt="info"></a></div></div>
	{/if}
	


	<div class="result">

		<div class="res">
			<div class="alt-head"><img src="images/success.png" alt="success"/>With</div>
			{#each alt as img_item}
				<div class="image"><img alt="image_with_alt" src="{img_item}"/></div>
			{:else}
				<div class="empty">No Image</div>
			{/each}
		</div>
		
		<div class="res">
			<div class="alt-head"><img src="images/warning.png" alt="success"/>Without</div>
			{#each no_alt as img_item}
				<div class="image"><img alt="image_without_alt" src="{img_item}"/></div>
			{:else}
				<div class="empty">No Image</div>
			{/each}
		</div>

		<div class="res">
			<div class="alt-head"><img src="images/garbage.png" alt="success"/>Without img tags</div>
			{#each garbage as gar_item}
				<div class="image garbage">{gar_item}/></div>
			{:else}
				<div class="empty">No Garbage</div>
			{/each}
		</div>

	</div>

	{/if}

	<div class="a11y">
		<img alt="a11y" src="images/a11y.png">
		<h2>WCAG 3.0</h2>
		<div><b>Text alternatives</b> were chosen as the first guideline to show that relevant and important guidelines from WCAG 2 can be ported into WCAG 3. It currently only applies to HTML images.</div>
			
	</div>


	<div class="footer">
		By Faisal Shohag | CSE'11 | BRUR<br>
		<small>Built with worlds most loved framework!</small>
	</div>

	
	
</main>



<style>
	main {
		text-align: center;
		max-width: 340px;
		margin: 0 auto;
	}



	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>