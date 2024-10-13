<script>
	// Problem statements array
	let questions = [
		'(4 points) Write a function to count the number of vowels in a given string.',
		'(6 points) Implement a method to sort an array of integers using the quicksort algorithm.',
		'(5 points) Generate the first n Fibonacci numbers.',
		'(4 points) Create a function to check if a given string is a palindrome.',
		'(5 points) Write a recursive method to calculate the factorial of a number.'
		// Add more questions here...
	];

	// Store the answers for each problem
	let answers = new Array(questions.length).fill('');

	// Array to keep track of already shown problem indexes
	let shownQuestions = [];

	// Get a random index for the first question
	let randomIndex = Math.floor(Math.random() * questions.length);

	// Current question index (random at first)
	let currentIndex = randomIndex;

	// This will hold the current problem statement being shown
	let currentQuestion = questions[currentIndex];

	// Function to move to the next problem statement
	function nextQuestion() {
		// Save current answer before moving to the next question
		saveAnswer();

		// Add the current question index to the shown questions
		shownQuestions.push(currentIndex);

		// Check if all questions have been shown; if yes, reset
		if (shownQuestions.length >= questions.length) {
			shownQuestions = []; // Reset shown questions
		}

		// Get a new random question index that hasn't been shown yet
		do {
			randomIndex = Math.floor(Math.random() * questions.length);
		} while (shownQuestions.includes(randomIndex));

		// Update the current question index and the current question
		currentIndex = randomIndex;
		currentQuestion = questions[currentIndex];
	}

	// Function to save the answer input for the current question
	function saveAnswer() {
		const input = document.querySelector('#answer-input');
		if (input) {
			answers[currentIndex] = input.value; // Save answer to the respective index
		}
	}

	let showQuestions = false;
	let showSummary = false;
	let timeRemaining = 0; // time remaining in seconds
	let interval; // interval for tracking remaining time
	let timer; // timeout for 30-minute timer

	// Function to trigger showing questions and start the timer
	function triggerShowQuestions() {
		showQuestions = true;
		showSummary = false;
		timeRemaining = 30 * 60; // 30 minutes in seconds

		// Clear any existing interval/timer to avoid multiple instances
		clearInterval(interval);
		clearTimeout(timer);

		// Start interval to count down the remaining time
		interval = setInterval(() => {
			timeRemaining--;
			if (timeRemaining <= 0) {
				clearInterval(interval);
			}
		}, 1000);

		// Set a timeout to hide questions and show summary after 30 minutes
		timer = setTimeout(
			() => {
				showQuestions = false;
				showSummary = true;
			},
			30 * 60 * 1000
		); // 30 minutes in milliseconds
	}

	//function to terminate program and show summary before timeout
	function terminateProgram() {
		showQuestions = false;
		showSummary = true;
	}
</script>

<main>
	<!-- Button to trigger the function -->
	<button on:click={triggerShowQuestions}>Start Quiz</button>

	<!-- Conditional display based on showQuestions and showSummary -->
	{#if showQuestions}
		<p>
			Questions are displayed. Time remaining: {Math.floor(timeRemaining / 60)} minutes and {timeRemaining %
				60} seconds.
		</p>
		<div class="container">
			<h1 class="mb-4 text-2xl font-bold">Coding Challenge</h1>

			<!-- Display current problem statement -->
			<p class="mb-4">{currentQuestion}</p>

			<!-- Input box for user's answer -->
			<input
				id="answer-input"
				type="text"
				placeholder="Type your solution here"
				class="mb-4 w-full rounded border p-2"
				bind:value={answers[currentIndex]}
			/>

			<!-- Button to go to the next problem statement -->
			<button
				class="rounded bg-blue-500 px-4 py-2 font-bold text-white hover:bg-blue-700"
				on:click={nextQuestion}
			>
				Next
			</button>
			<!-- Button to terminate -->
			<button
				class="rounded bg-red-500 px-4 py-2 font-bold text-white hover:bg-red-700"
				on:click={terminateProgram}
			>
				Terminate
			</button>
		</div>
	{/if}

	{#if showSummary}
		<p>The summary is displayed.</p>
	{/if}
</main>

<style>
	.container {
		max-width: 600px;
		margin: 0 auto;
		padding: 20px;
	}
</style>
