<script>
	let loanAmount = null;
	let interestRate = null;
	let years = null;
	let mortgageType = null;
	let monthlyPayment = null;

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
		loanAmount = null;
		interestRate = null;
		years = null;
		mortgageType = null;
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
					<div class="flex items-center">
						<div
							class="bg-blue-100 border border-blue-300 text-blue-900 px-4 py-2 rounded-l-lg flex items-center"
							style="width: 50px; text-align: center;"
						>
							£
						</div>
						<input
							type="number"
							class="w-full border-gray-300 bg-white-50 rounded-r-lg p-2"
							bind:value={loanAmount}
						/>
					</div>
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
			class="bg p-8 flex flex-col justify-center items-center"
			style="background-color: #133040;"
		>
			<img src="/assets/images/illustration-empty.svg" alt="Illustration" class="w-40 mb-6" />
			<h2 class="text-lg font-semibold text-white">Results shown here</h2>

			{#if monthlyPayment === null}
				<p class="text-gray-300 mt-2 text-center">
					Complete the form and click "calculate repayments" to see what your monthly repayments
					should be.
				</p>
			{:else}
				<div class="mt-4">
					<p class="text-white">Your monthly repayments</p>
					<p class="text-4xl font-bold mt-2 text-white">€{monthlyPayment.toFixed(2)}</p>
				</div>
			{/if}
		</div>
	</div>
</div>
