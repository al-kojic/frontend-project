<script>
	let loanAmount = null;
	let interestRate = null;
	let years = null;
	let mortgageType = null;
	let monthlyPayment = null;

	let errors = {};

	function validateForm() {
		errors = {};

		if (!loanAmount || loanAmount <= 0) {
			errors.loanAmount = 'Please enter a valid loan amount.';
		}
		if (!interestRate || interestRate <= 0) {
			errors.interestRate = 'Please enter a valid interest rate.';
		}
		if (!years || years <= 0) {
			errors.years = 'Please enter a valid mortgage term.';
		}
		if (!mortgageType) {
			errors.mortgageType = 'Please select a mortgage type.';
		}

		return Object.keys(errors).length === 0;
	}

	function calculateMortgage(event) {
		event.preventDefault();
		if (!validateForm()) {
			return;
		}

		const months = years * 12;
		const monthlyRate = interestRate / 100 / 12;

		if (mortgageType === 'repayment') {
			monthlyPayment = (loanAmount * monthlyRate) / (1 - Math.pow(1 + monthlyRate, -months));
		} else if (mortgageType === 'interest-only') {
			monthlyPayment = loanAmount * monthlyRate;
		}
	}

	function resetForm() {
		loanAmount = null;
		interestRate = null;
		years = null;
		mortgageType = null;
		monthlyPayment = null;
		errors = {};
	}
</script>

<svelte:head>
	<link
		href="https://fonts.googleapis.com/css2?family=Jakarta+Sans:wght@400;500;600&display=swap"
		rel="stylesheet"
	/>

	<style>
		body {
			font-family: 'Jakarta Sans', sans-serif;
		}
		.error {
			color: red;
			font-size: 0.875rem;
			margin-top: 0.25rem;
		}
	</style>
</svelte:head>

<div class="bg-blue-50 min-h-screen flex justify-center items-center">
	<div
		class="bg-white shadow-xl rounded-lg max-w-4xl w-full grid grid-cols-1 md:grid-cols-2 overflow-hidden"
	>
		<div class="p-8">
			<div class="flex justify-between items-center">
				<h1 class="text-2xl font-bold text-gray-700">Mortgage Calculator</h1>
				<button class="underline" onclick={resetForm}>Clear all</button>
			</div>

			<form class="mt-6 space-y-4" onsubmit={calculateMortgage}>
				<div class="form-control">
					<p class="text-gray-600">Mortgage Amount</p>
					<div class="flex items-center">
						<div
							class="bg-blue-100 border border-blue-300 text-blue-900 px-4 py-2 rounded-l-lg flex items-center"
							style="width: 50px; text-align: center;"
						>
							€
						</div>
						<input
							type="number"
							class="w-full border-gray-300 bg-white-50 rounded-r-lg p-2"
							bind:value={loanAmount}
						/>
					</div>
					{#if errors.loanAmount}
						<p class="error">{errors.loanAmount}</p>
					{/if}
				</div>

				<div class="flex gap-4">
					<div class="flex-1 form-control">
						<p class="text-gray-600">Mortgage Term</p>
						<div class="flex items-center">
							<input
								type="number"
								class="w-full border-gray-300 rounded-l-lg bg-white p-2"
								bind:value={years}
							/>
							<div
								class="bg-blue-100 border border-blue-300 text-blue-900 px-4 py-2 rounded-r-lg flex items-center"
							>
								years
							</div>
						</div>
						{#if errors.years}
							<p class="error">{errors.years}</p>
						{/if}
					</div>

					<div class="flex-1 form-control">
						<p class="text-gray-600">Interest Rate</p>
						<div class="flex items-center">
							<input
								type="number"
								step="0.01"
								class="w-full border-gray-300 rounded-l-lg bg-white p-2"
								bind:value={interestRate}
							/>
							<div
								class="bg-blue-100 border border-blue-300 text-blue-900 px-4 py-2 rounded-r-lg flex items-center"
							>
								%
							</div>
						</div>
						{#if errors.interestRate}
							<p class="error">{errors.interestRate}</p>
						{/if}
					</div>
				</div>

				<div class="form-control">
					<div class="flex gap-4 mt-2">
						<span class="cursor-pointer flex items-center gap-2">
							<input
								type="radio"
								value="repayment"
								class="radio radio-primary"
								bind:group={mortgageType}
							/>
							<span>Repayment</span>
						</span>
						<span class="cursor-pointer flex items-center gap-2">
							<input
								type="radio"
								value="interest-only"
								class="radio radio-primary"
								bind:group={mortgageType}
							/>
							<span>Interest Only</span>
						</span>
					</div>
					{#if errors.mortgageType}
						<p class="error">{errors.mortgageType}</p>
					{/if}
				</div>

				<button type="submit" class="btn w-full mt-4" style="background-color: #D9DB30;">
					<img
						src="/assets/images/icon-calculator.svg"
						alt="Calculator"
						class="w-4 h-4 mr-2"
						style="width: 24px; height: 24px; margin-right: 8px"
					/>Calculate Repayments
				</button>
			</form>
		</div>

		<div
			class="bg p-8 flex flex-col justify-center items-center text-white"
			style="background-color: #133040;"
		>
			{#if monthlyPayment === null}
				<img src="/assets/images/illustration-empty.svg" alt="Illustration" class="w-40 mb-6" />
				<h2 class="text-lg font-semibold">Results shown here</h2>
				<p class="text-gray-300 mt-2 text-center">
					Complete the form and click "calculate repayments" to see what your monthly repayments
					should be.
				</p>
			{:else}
				<div class="w-full">
					<p class="text-sm uppercase text-gray-400 font-medium mb-4">Your results</p>
					<div class="p-6 bg-gray-800 rounded-lg">
						<p class="text-gray-300 text-sm">Your monthly repayments</p>
						<p class="text-5xl font-bold text-yellow-400 mt-2">
							€{monthlyPayment.toLocaleString('de-DE')}
						</p>
					</div>
					<div class="p-6 mt-4 bg-gray-700 rounded-lg">
						<p class="text-sm text-gray-300">Total you'll repay over the term</p>
						<p class="text-xl font-semibold text-gray-50 mt-2">
							€{(monthlyPayment * years * 12).toLocaleString('de-DE')}
						</p>
					</div>
				</div>
			{/if}
		</div>
	</div>
</div>
