<script>
	// Add any page-specific logic here (if needed)
</script>

<link rel="preconnect" href="https://fonts.googleapis.com" />
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
<link
	href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:ital,wght@0,200..800;1,200..800&display=swap"
	rel="stylesheet"
/>

<!-- <h1 class="text-red-500">Welcome to SvelteKit</h1>
<p>Visit <a href="https://svelte.dev/docs/kit">svelte.dev/docs/kit</a> to read the documentation</p>
<a class="btn" href="https://www.johak.at/" target="_blank">Button Daisy UI</a> -->

<!-- <style>
	:global(body) {
		background-color: #e3f4fc;
		height: 100vh;
	}
</style> -->

<body>
	<div class="box">
		<h2><b>Mortage Calculator</b></h2>
		<p class="p1"><u>Clear All</u></p>
		<p class="p2">Mortage Amount</p>
		<!-- <p class="p3">Mortage Term</p>
		<p class="p4">Interest Rate</p>
		<p class="p5">Mortage Type</p> -->
	</div>
	<div class="amount"></div>
</body>

<style>
	body {
		display: flex;
		justify-content: center;
		align-items: center;
		height: 100vh;
		background-color: #e3f4fc;
	}

	.box {
		background-color: white;
		padding: 30px;
		border-radius: 30px;
		box-shadow: 0 10px 70px rgba(0, 0, 0, 0.1);
		text-align: center;
		width: 800px;
		height: 500px;
	}

	.box h2 {
		color: #000000;
		text-align: left;
		font-size: 20px;
		margin: 5px 10px;
		font-family: 'Plus Jakarta Sans', sans-serif;
	}

	.p1 {
		color: hsl(200, 24%, 40%);
		text-align: top;
		padding-right: 100px;
		margin-top: -30px;
		font-size: 16px;
		font-weight: 500;
	}

	.p2 {
		color: hsl(200, 24%, 40%);
		font-size: 16px;
		text-align: left;
		margin-top: 20px;
		padding: 10px;
		font-weight: 500;
	}
</style>
