<script>
	let loanAmount = $state(0);
	let interestRate = $state(0);
	let years = $state(0);
	let mortgageType = $state('repayment');
	let monthlyPayment = $state(null);

	function calculateMortgage(event) {
		event.preventDefault();

		const months = years * 12;
		const monthlyRate = interestRate / 100 / 12;

		if (mortgageType === 'repayment') {
			monthlyPayment = (loanAmount * monthlyRate) / (1 - Math.pow(1 + monthlyRate, -months));
		} else if (mortgageType === 'interest-only') {
			monthlyPayment = loanAmount * monthlyRate;
		}
	}

	function resetForm() {
		loanAmount = 0;
		interestRate = 0;
		years = 0;
		mortgageType = 'repayment';
		monthlyPayment = null;
	}
</script>

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
					<p class="text-gray-600">Mortgage Amount (€)</p>
					<input type="number" class="w-full border-gray-300 bg-white-50" bind:value={loanAmount} />
				</div>

				<div class="flex gap-4">
					<div class="flex-1 form-control">
						<p class="text-gray-600">Mortgage Term</p>
						<div class="flex items-center gap-2">
							<input type="number" class="w-full border-gray-300" bind:value={years} />
							<span class="text-gray-500">years</span>
						</div>
					</div>

					<div class="flex-1 form-control">
						<p class="text-gray-600">Interest Rate</p>
						<div class="flex items-center gap-2">
							<input
								type="number"
								step="0.01"
								class="w-full border-gray-300"
								placeholder="Interest Rate (%)"
								bind:value={interestRate}
							/>
							<span class="text-gray-500">%</span>
						</div>
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
				</div>

				<button type="submit" class="btn btn-primary w-full mt-4">Calculate Repayments</button>
			</form>
		</div>

		<div class="bg-secondary text-white p-8 flex flex-col justify-center items-center">
			<img src="/assets/images/illustration-empty.svg" alt="Illustration" class="w-40 mb-6" />
			<h2 class="text-lg font-semibold">Results shown here</h2>
			{#if monthlyPayment === null}
				<p class="text-gray-300 mt-2 text-center">
					Complete the from and click "Calculate Repayments" to see your results.
				</p>
			{:else}
				<div class="mt-4">
					<p>Your monthly repayments</p>
					<p class="text-4xl font-bold mt-2">€{monthlyPayment.toFixed(2)}</p>
				</div>
			{/if}
		</div>
	</div>
</div>
