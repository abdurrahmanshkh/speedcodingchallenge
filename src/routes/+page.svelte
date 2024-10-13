<script>
	import { Button, Card, Input, Label, Li, List, Modal, P } from 'flowbite-svelte';
	import { Textarea, Toolbar, ToolbarGroup, ToolbarButton } from 'flowbite-svelte';

	let name = '';
	let phone = '';
	let email = '';
	let password = '';
	let login = false;
	let key = 'ietscc2024';
	let loginModal = false;
	let terminateModal = false;

	// Function to submit the form
	function submitForm() {
		if (password == key) {
			loginModal = true;
		}
	}

	// Problem statements array
	let questions = [
		'Write a function to count the number of vowels in a given string. (4 points)',
		'Implement a method to sort an array of integers using the quicksort algorithm. (6 points)',
		'Generate the first n Fibonacci numbers. (5 points)',
		'Create a function to check if a given string is a palindrome. (4 points)',
		'Write a recursive method to calculate the factorial of a number. (5 points)',
		'Develop a function to find all prime numbers up to a given limit. (6 points)',
		'Write a method to check if two strings are anagrams of each other. (5 points)',
		'Implement binary search on a sorted array of integers. (6 points)',
		'Write a method to find the sum of all elements in an array. (4 points)',
		'Create a function to transpose a given matrix. (5 points)',
		'Write a Python program to create a list of squares of numbers from 1 to 20 using list comprehension. (3 points)',
		'Write a script to read a text file and count the frequency of each word. (5 points)',
		'Create a function that reads a JSON file and outputs the data in a human-readable format. (6 points)',
		'Write a function that counts how many times each character appears in a string. (4 points)',
		'Implement a method to sort a dictionary by its values. (5 points)',
		'Write a program to find the intersection of two sets. (6 points)',
		'Generate the first n Fibonacci numbers using a generator. (5 points)',
		'Create a simple web scraper that extracts titles from a given webpage. (6 points)',
		'Write a function to reverse a given string without using built-in functions. (4 points)',
		'Implement a simple class structure using inheritance (e.g., a base class Animal and a derived class Dog). (6 points)',
		'Write a program that demonstrates exception handling for invalid input. (7 points)',
		'Create a simple command-line calculator that supports addition, subtraction, multiplication, and division. (8 points)',
		'Implement a method to find the prime factorization of a number. (7 points)',
		'Write a function to find the intersection of two lists. (6 points)',
		'Create a function that reverses the order of words in a given sentence. (5 points)',
		'Implement a basic Caesar cipher for encrypting and decrypting messages. (6 points)',
		'Write a program to generate a list of unique random numbers within a specified range. (7 points)',
		'Write a function to count the occurrences of each character in a given string, returning the result as a dictionary. (5 points)',
		'Create a function that solves quadratic equations using the quadratic formula. (6 points)',
		'Write a program that uses multithreading to print numbers from 1 to 10. (8 points)',
		'Create a custom exception class and demonstrate its usage. (7 points)',
		'Write a recursive function to calculate the nth triangular number. (8 points)',
		'Demonstrate the use of ArrayList and HashMap in a Java program. (5 points)',
		'Write a simple decorator that logs function calls. (7 points)',
		'Create a script that accepts command-line arguments and prints them. (6 points)',
		'Use a lambda function to sort a list of tuples based on the second element. (7 points)',
		'Write a function that calculates the average of a list of numbers. (5 points)',
		'Use the requests library to fetch data from an API and display it. (8 points)',
		'Create a simple text-based game loop with basic commands (like quit). (7 points)',
		'Solve the coin change problem using dynamic programming. (9 points)',
		'Write a function to read and write CSV files. (8 points)',
		'Analyze and compare the time complexity of different sorting algorithms. (9 points)',
		'Implement a simple command-line version of Tic-Tac-Toe. (8 points)',
		'Write a program that validates email addresses using regular expressions. (7 points)',
		'Implement depth-first search (DFS) on a graph. (8 points)',
		'Create a method to sort an array using the shell sort algorithm. (7 points)',
		'Write a program that converts currencies based on current exchange rates (mock data). (6 points)',
		'Implement in-order traversal for a binary tree. (8 points)',
		'Write a program that calculates the median of a list of numbers. (7 points)',
		'Create a function that compresses a string using run-length encoding. (9 points)'
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

	let showQuestions = false;
	let showSummary = false;
	let timeRemaining = 0; // time remaining in seconds
	let interval; // interval for tracking remaining time
	let timer; // timeout for 30-minute timer

	// Function to trigger showing questions and start the timer
	function triggerShowQuestions() {
		login = true;
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
				terminateProgram(); // Automatically call terminate when time is up
			}
		}, 1000);
	}

	// Function to terminate the program and show the summary before timeout
	function terminateProgram() {
		showQuestions = false;
		showSummary = true;
		clearInterval(interval); // Stop the timer
		clearTimeout(timer); // Clear the timeout
		downloadSummaryAsTxt();
	}

	// Function to generate the summary
	function generateSummary() {
		let summary = [];
		for (let i = 0; i < questions.length; i++) {
			if (answers[i].length > 0) {
				summary.push({
					question: questions[i],
					answer: answers[i]
				});
			}
		}
		return summary;
	}

	function downloadSummaryAsTxt() {
		let summaryText = `Summary

Participant Details:
Name: ${name}
Phone: ${phone}
Email: ${email}
    `;

		//add questions and answers
		generateSummary().forEach((item, index) => {
			summaryText += `
Question ${index + 1}: ${item.question}
Answer:
${item.answer}\n`;
		});

		// Create a blob and download the text file
		const blob = new Blob([summaryText], { type: 'text/plain' });
		const link = document.createElement('a');
		link.href = URL.createObjectURL(blob);
		link.download = `${name} - ${phone}.txt`;

		document.body.appendChild(link);
		link.click();
		document.body.removeChild(link);
	}
</script>

<main>
	<!-- Button to trigger the function -->
	{#if !login}
		<div class="grid grid-cols-3 gap-8">
			<Card class="align-center h-full min-w-full justify-center bg-blue-100 bg-opacity-90">
				<form class="flex flex-col space-y-6" on:submit|preventDefault={submitForm}>
					<h3 class="text-2xl font-bold text-gray-900 dark:text-white">
						Sign in to start the competition
					</h3>
					<Label class="space-y-2">
						<span>Full Name</span>
						<Input type="text" placeholder="Name" required bind:value={name} />
					</Label>
					<Label class="space-y-2">
						<span>Phone Number</span>
						<Input type="text" placeholder="9876543210" required bind:value={phone} />
					</Label>
					<Label class="space-y-2">
						<span>Email</span>
						<Input type="email" placeholder="name@company.com" required bind:value={email} />
					</Label>
					<Label class="space-y-2">
						<span>Your password</span>
						<Input type="password" placeholder="•••••" required bind:value={password} />
					</Label>
					<Button type="submit" class="w-full">Login and Start</Button>
				</form>
			</Card>
			<Card class="col-span-2 min-w-full bg-gray-100 bg-opacity-90">
				<P class="mb-4 text-2xl font-bold">Speed Coding Challenge - Event Rules</P>

				<P class="text-xl font-bold">Participation</P>
				<List class="mb-4">
					<Li>Log in with your registered details before the challenge begins.</Li>
					<Li>Ensure correct login credentials to avoid disqualification.</Li>
				</List>

				<P class="text-xl font-bold">Event Structure</P>
				<List class="mb-4">
					<Li>A single coding round with multiple random coding problems.</Li>
					<Li>Questions range from beginner to advanced levels.</Li>
					<Li>You must complete as many problems as possible within the time limit.</Li>
				</List>

				<P class="text-xl font-bold">Time Limit</P>
				<List class="mb-4">
					<Li><strong>Total Time:</strong> 30 minutes</Li>
					<Li>Participants can track remaining time during the challenge.</Li>
				</List>

				<P class="text-xl font-bold">Submission</P>
				<List class="mb-4">
					<Li>Submit your solutions for each problem before the timer ends.</Li>
					<Li>Only correctly submitted answers will be evaluated.</Li>
					<Li>No second attempts once time is up or the challenge is terminated early.</Li>
				</List>

				<P class="text-xl font-bold">Winner Announcement</P>
				<List>
					<Li>Winners will be announced based on the total number of points scored.</Li>
					<Li>Organizer decisions are final and binding.</Li>
				</List>
			</Card>
		</div>
	{/if}

	<!-- Conditional display based on showQuestions and showSummary -->
	{#if showQuestions}
		<Card class="min-w-full">
			<Textarea
				id="answer-input"
				placeholder="Your answer"
				rows="16"
				bind:value={answers[currentIndex]}
			>
				<Toolbar slot="header" embedded>
					<ToolbarGroup>
						<ToolbarButton name="Attach file" class="font-bold">Question</ToolbarButton>
					</ToolbarGroup>
					<ToolbarGroup>
						<ToolbarButton name="Attach file" class="font-bold" color="blue"
							>{currentQuestion}</ToolbarButton
						>
					</ToolbarGroup>
					<ToolbarButton name="send" slot="end" class="min-w-fit font-bold" color="red">
						Time Remaining: {Math.floor(timeRemaining / 60)} minutes {timeRemaining % 60} seconds
					</ToolbarButton>
				</Toolbar>
				<div slot="footer" class="flex items-center justify-between p-2">
					<Button class="w-64" on:click={nextQuestion}>Next Question</Button>
					<Button
						class="w-64"
						color="red"
						on:click={() => {
							terminateModal = true;
						}}>End Competition</Button
					>
				</div>
			</Textarea>
		</Card>
	{/if}

	{#if showSummary}
		<Card class="min-w-full">
			<P class="mb-4 text-2xl font-bold">Summary</P>
			<P class="mb-2 text-xl font-bold">Participant Details</P>
			<P class="mb-2"><strong>Name:</strong> {name}</P>
			<P class="mb-2"><strong>Phone:</strong> {phone}</P>
			<P class="mb-4"><strong>Email:</strong> {email}</P>
			<P class="text-xl font-bold">Challenge Details</P>
			<List>
				{#each generateSummary() as { question, answer }}
					<P class="mt-2"><strong>Question:</strong> {question}</P>
					<P><strong>Answer:</strong> {answer}</P>
				{/each}
			</List>
		</Card>
	{/if}
</main>

<Modal title="Terms of Service" bind:open={loginModal} autoclose>
	<p class="text-base leading-relaxed text-gray-500 dark:text-gray-400">
		Are you sure you want to start now?
	</p>
	<svelte:fragment slot="footer">
		<Button on:click={triggerShowQuestions}>Start Now</Button>
		<Button color="alternative">Decline</Button>
	</svelte:fragment>
</Modal>

<!-- Modal to terminate program -->
<Modal title="End Competition" bind:open={terminateModal} autoclose>
	<p class="text-base leading-relaxed text-gray-500 dark:text-gray-400">
		Are you sure you want to end the competition?
	</p>
	<svelte:fragment slot="footer">
		<Button on:click={terminateProgram}>End Competition</Button>
		<Button color="alternative">Cancel</Button>
	</svelte:fragment>
</Modal>
