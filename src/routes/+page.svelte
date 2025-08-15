<script>
import { Button, Card, Input, Label, Li, List, Modal, P } from 'flowbite-svelte';
import { Textarea, Toolbar, ToolbarGroup, ToolbarButton } from 'flowbite-svelte';

let name = '';
let phone = '';
let email = '';
let password = '';
let login = false;
let key = 'ietkjsit';
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

<main class="space-y-8">
	<!-- Login Section -->
	{#if !login}
		<div class="animate-fade-in">
			<!-- Hero Section -->
			<div class="text-center mb-12">
				<h1 class="text-5xl font-bold bg-gradient-to-r from-blue-400 via-purple-400 to-pink-400 bg-clip-text text-transparent mb-4 animate-float pb-2 mt-4">
					⚡ Speed Coding Challenge ⚡
				</h1>
				<p class="text-xl text-white/80 max-w-2xl mx-auto leading-relaxed">
					Test your coding skills against the clock. Solve challenging problems and compete with the best!
				</p>
			</div>

			<div class="grid lg:grid-cols-5 gap-8 max-w-7xl mx-auto">
				<!-- Login Form -->
				<div class="lg:col-span-2">
					<Card class="bg-white/10 backdrop-blur-md border border-white/20 shadow-2xl hover:shadow-3xl transition-all duration-300 min-w-full">
						<div class="p-8">
							<div class="text-center mb-8">
								<div class="w-16 h-16 bg-gradient-to-r from-blue-500 to-purple-600 rounded-2xl flex items-center justify-center mx-auto mb-4">
									<svg class="w-8 h-8 text-white" fill="none" stroke="currentColor" viewBox="0 0 24 24">
										<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 15v2m-6 4h12a2 2 0 002-2v-6a2 2 0 00-2-2H6a2 2 0 00-2 2v6a2 2 0 002 2zm10-10V7a4 4 0 00-8 0v4h8z"/>
									</svg>
								</div>
								<h3 class="text-2xl font-bold text-white mb-2">
									Ready to Code?
								</h3>
								<p class="text-white/70">Enter your details to begin the challenge</p>
							</div>

							<form class="space-y-6" on:submit|preventDefault={submitForm}>
								<div class="space-y-4">
									<Label class="space-y-2">
										<span class="text-white font-medium">Full Name</span>
										<Input 
											type="text" 
											placeholder="Enter your full name" 
											required 
											bind:value={name}
											class="bg-white/10 border-white/20 text-white placeholder-white/50 focus:border-blue-400 focus:ring-blue-400/50"
										/>
									</Label>

									<Label class="space-y-2">
										<span class="text-white font-medium">Phone Number</span>
										<Input 
											type="text" 
											placeholder="9876543210" 
											required 
											bind:value={phone}
											class="bg-white/10 border-white/20 text-white placeholder-white/50 focus:border-blue-400 focus:ring-blue-400/50"
										/>
									</Label>

									<Label class="space-y-2">
										<span class="text-white font-medium">Email Address</span>
										<Input 
											type="email" 
											placeholder="name@company.com" 
											required 
											bind:value={email}
											class="bg-white/10 border-white/20 text-white placeholder-white/50 focus:border-blue-400 focus:ring-blue-400/50"
										/>
									</Label>

									<Label class="space-y-2">
										<span class="text-white font-medium">Access Password</span>
										<Input 
											type="password" 
											placeholder="Enter the event password" 
											required 
											bind:value={password}
											class="bg-white/10 border-white/20 text-white placeholder-white/50 focus:border-blue-400 focus:ring-blue-400/50"
										/>
									</Label>
								</div>

								<Button 
									type="submit" 
									class="w-full bg-gradient-to-r from-blue-500 to-purple-600 hover:from-blue-600 hover:to-purple-700 text-white font-semibold py-3 px-6 rounded-xl transition-all duration-300 transform hover:scale-105 shadow-lg"
								>
									<svg class="w-5 h-5 mr-2" fill="none" stroke="currentColor" viewBox="0 0 24 24">
										<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 10V3L4 14h7v7l9-11h-7z"/>
									</svg>
									Start Challenge
								</Button>
							</form>
						</div>
					</Card>
				</div>

				<!-- Rules Section -->
				<div class="lg:col-span-3">
					<Card class="bg-white/5 backdrop-blur-md border border-white/20 shadow-2xl h-full min-w-full">
						<div class="p-8">
							<div class="flex items-center mb-6">
								<div class="w-12 h-12 bg-gradient-to-r from-green-500 to-blue-500 rounded-xl flex items-center justify-center mr-4">
									<svg class="w-6 h-6 text-white" fill="none" stroke="currentColor" viewBox="0 0 24 24">
										<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 12h6m-6 4h6m2 5H7a2 2 0 01-2-2V5a2 2 0 012-2h5.586a1 1 0 01.707.293l5.414 5.414a1 1 0 01.293.707V19a2 2 0 01-2 2z"/>
									</svg>
								</div>
								<P class="text-3xl font-bold text-white">Challenge Rules & Guidelines</P>
							</div>

							<div class="grid gap-6">
								<div class="bg-white/5 rounded-xl p-6 border border-white/10">
									<P class="text-xl font-bold text-blue-400 mb-3 flex items-center">
										<span class="w-6 h-6 bg-blue-500 rounded-full flex items-center justify-center text-white text-sm font-bold mr-2">1</span>
										Participation
									</P>
									<List class="text-white/80 space-y-2">
										<Li class="flex items-start">
											<span class="text-green-400 mr-2">✓</span>
											Log in with your registered details before the challenge begins
										</Li>
										<Li class="flex items-start">
											<span class="text-green-400 mr-2">✓</span>
											Ensure correct login credentials to avoid disqualification
										</Li>
									</List>
								</div>

								<div class="bg-white/5 rounded-xl p-6 border border-white/10">
									<P class="text-xl font-bold text-purple-400 mb-3 flex items-center">
										<span class="w-6 h-6 bg-purple-500 rounded-full flex items-center justify-center text-white text-sm font-bold mr-2">2</span>
										Event Structure
									</P>
									<List class="text-white/80 space-y-2">
										<Li class="flex items-start">
											<span class="text-green-400 mr-2">✓</span>
											Single coding round with multiple random coding problems
										</Li>
										<Li class="flex items-start">
											<span class="text-green-400 mr-2">✓</span>
											Questions range from beginner to advanced levels
										</Li>
										<Li class="flex items-start">
											<span class="text-green-400 mr-2">✓</span>
											Complete as many problems as possible within time limit
										</Li>
									</List>
								</div>

								<div class="bg-white/5 rounded-xl p-6 border border-white/10">
									<P class="text-xl font-bold text-orange-400 mb-3 flex items-center">
										<span class="w-6 h-6 bg-orange-500 rounded-full flex items-center justify-center text-white text-sm font-bold mr-2">3</span>
										Time Limit & Scoring
									</P>
									<List class="text-white/80 space-y-2">
										<Li class="flex items-start">
											<span class="text-green-400 mr-2">⏰</span>
											<strong class="text-white">Total Time: 30 minutes</strong>
										</Li>
										<Li class="flex items-start">
											<span class="text-green-400 mr-2">✓</span>
											Track remaining time during the challenge
										</Li>
										<Li class="flex items-start">
											<span class="text-green-400 mr-2">✓</span>
											Winners announced based on total points scored
										</Li>
									</List>
								</div>
							</div>
						</div>
					</Card>
				</div>
			</div>
		</div>
	{/if}

	<!-- Coding Challenge Interface -->
	{#if showQuestions}
		<div class="animate-slide-up">
			<Card class="bg-white/10 backdrop-blur-md border border-white/20 shadow-2xl min-w-full">
				<div class="bg-gradient-to-r from-blue-500/20 to-purple-500/20 p-6 rounded-t-lg border-b border-white/20">
					<div class="flex items-center justify-between">
						<div class="flex items-center space-x-4">
							<div class="w-12 h-12 bg-gradient-to-r from-blue-500 to-purple-600 rounded-xl flex items-center justify-center">
								<svg class="w-6 h-6 text-white" fill="none" stroke="currentColor" viewBox="0 0 24 24">
									<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M10 20l4-16m4 4l4 4-4 4M6 16l-4-4 4-4"/>
								</svg>
							</div>
							<div>
								<h2 class="text-2xl font-bold text-white">Coding Challenge</h2>
								<p class="text-white/70">Question {currentIndex + 1} of {questions.length}</p>
							</div>
						</div>
						<div class="text-right">
							<div class="bg-red-500/20 border border-red-500/30 rounded-xl px-4 py-2">
								<p class="text-red-400 font-bold text-lg">
									{Math.floor(timeRemaining / 60)}:{(timeRemaining % 60).toString().padStart(2, '0')}
								</p>
								<p class="text-red-300 text-sm">Time Remaining</p>
							</div>
						</div>
					</div>
				</div>

				<div class="p-6">
					<div class="bg-blue-500/10 border border-blue-500/30 rounded-xl p-6 mb-6">
						<h3 class="text-lg font-semibold text-blue-400 mb-2">Problem Statement</h3>
						<p class="text-white text-lg leading-relaxed">{currentQuestion}</p>
					</div>

					<div class="mb-6">
						<Label class="text-white font-medium mb-3 block">Your Solution</Label>
						<Textarea
							id="answer-input"
							placeholder="Write your code solution here..."
							rows="16"
							bind:value={answers[currentIndex]}
							class="bg-slate-900/50 border-white/20 text-white placeholder-white/50 focus:border-blue-400 focus:ring-blue-400/50 font-mono text-sm"
						/>
					</div>

					<div class="flex items-center justify-between gap-4">
						<Button 
							class="flex-1 bg-gradient-to-r from-green-500 to-blue-500 hover:from-green-600 hover:to-blue-600 text-white font-semibold py-3 px-6 rounded-xl transition-all duration-300"
							on:click={nextQuestion}
						>
							<svg class="w-5 h-5 mr-2" fill="none" stroke="currentColor" viewBox="0 0 24 24">
								<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 7l5 5m0 0l-5 5m5-5H6"/>
							</svg>
							Next Question
						</Button>
						
						<Button
							class="flex-1 bg-gradient-to-r from-red-500 to-pink-500 hover:from-red-600 hover:to-pink-600 text-white font-semibold py-3 px-6 rounded-xl transition-all duration-300"
							on:click={() => { terminateModal = true; }}
						>
							<svg class="w-5 h-5 mr-2" fill="none" stroke="currentColor" viewBox="0 0 24 24">
								<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 12a9 9 0 11-18 0 9 9 0 0118 0z"/>
								<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 10a1 1 0 011-1h4a1 1 0 011 1v4a1 1 0 01-1 1h-4a1 1 0 01-1-1v-4z"/>
							</svg>
							End Challenge
						</Button>
					</div>
				</div>
			</Card>
		</div>
	{/if}

	<!-- Summary Section -->
	{#if showSummary}
		<div class="animate-fade-in">
			<Card class="bg-white/10 backdrop-blur-md border border-white/20 shadow-2xl min-w-full">
				<div class="bg-gradient-to-r from-green-500/20 to-blue-500/20 p-6 rounded-t-lg border-b border-white/20">
					<div class="flex items-center space-x-4">
						<div class="w-12 h-12 bg-gradient-to-r from-green-500 to-blue-500 rounded-xl flex items-center justify-center">
							<svg class="w-6 h-6 text-white" fill="none" stroke="currentColor" viewBox="0 0 24 24">
								<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 12l2 2 4-4m6 2a9 9 0 11-18 0 9 9 0 0118 0z"/>
							</svg>
						</div>
						<div>
							<h2 class="text-3xl font-bold text-white">Challenge Complete! 🎉</h2>
							<p class="text-white/70">Your solutions have been saved automatically</p>
						</div>
					</div>
				</div>

				<div class="p-8 space-y-8">
					<!-- Participant Details -->
					<div class="bg-white/5 rounded-xl p-6 border border-white/10">
						<h3 class="text-xl font-bold text-blue-400 mb-4 flex items-center">
							<svg class="w-5 h-5 mr-2" fill="none" stroke="currentColor" viewBox="0 0 24 24">
								<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M16 7a4 4 0 11-8 0 4 4 0 018 0zM12 14a7 7 0 00-7 7h14a7 7 0 00-7-7z"/>
							</svg>
							Participant Details
						</h3>
						<div class="grid md:grid-cols-3 gap-4">
							<div class="bg-white/5 rounded-lg p-4">
								<p class="text-white/60 text-sm">Name</p>
								<p class="text-white font-semibold">{name}</p>
							</div>
							<div class="bg-white/5 rounded-lg p-4">
								<p class="text-white/60 text-sm">Phone</p>
								<p class="text-white font-semibold">{phone}</p>
							</div>
							<div class="bg-white/5 rounded-lg p-4">
								<p class="text-white/60 text-sm">Email</p>
								<p class="text-white font-semibold">{email}</p>
							</div>
						</div>
					</div>

					<!-- Solutions Summary -->
					<div class="bg-white/5 rounded-xl p-6 border border-white/10">
						<h3 class="text-xl font-bold text-purple-400 mb-4 flex items-center">
							<svg class="w-5 h-5 mr-2" fill="none" stroke="currentColor" viewBox="0 0 24 24">
								<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 12h6m-6 4h6m2 5H7a2 2 0 01-2-2V5a2 2 0 012-2h5.586a1 1 0 01.707.293l5.414 5.414a1 1 0 01.293.707V19a2 2 0 01-2 2z"/>
							</svg>
							Your Solutions ({generateSummary().length} completed)
						</h3>
						
						{#if generateSummary().length === 0}
							<div class="text-center py-8">
								<p class="text-white/60">No solutions submitted</p>
							</div>
						{:else}
							<div class="space-y-6">
								{#each generateSummary() as { question, answer }, index}
									<div class="bg-white/5 rounded-lg p-6 border border-white/10">
										<div class="flex items-start space-x-4">
											<div class="w-8 h-8 bg-gradient-to-r from-blue-500 to-purple-600 rounded-lg flex items-center justify-center flex-shrink-0 mt-1">
												<span class="text-white font-bold text-sm">{index + 1}</span>
											</div>
											<div class="flex-1">
												<h4 class="text-white font-semibold mb-3">{question}</h4>
												<div class="bg-slate-900/50 rounded-lg p-4 border border-white/10">
													<pre class="text-green-400 text-sm font-mono whitespace-pre-wrap">{answer}</pre>
												</div>
											</div>
										</div>
									</div>
								{/each}
							</div>
						{/if}
					</div>
				</div>
			</Card>
		</div>
	{/if}
</main>

<!-- Enhanced Modals -->
<Modal title="🚀 Ready to Start?" bind:open={loginModal} autoclose size="sm" class="bg-gray-900">
	<div class="text-center p-6">
		<div class="w-16 h-16 bg-gradient-to-r from-green-500 to-blue-500 rounded-full flex items-center justify-center mx-auto mb-4">
			<svg class="w-8 h-8 text-white" fill="none" stroke="currentColor" viewBox="0 0 24 24">
				<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 10V3L4 14h7v7l9-11h-7z"/>
			</svg>
		</div>
		<p class="text-lg text-white mb-2">
			You're about to begin the 30-minute speed coding challenge!
		</p>
		<p class="text-white/70">
			Make sure you're ready before starting the timer.
		</p>
	</div>
	<svelte:fragment slot="footer">
		<div class="flex space-x-3 w-full">
			<Button 
				on:click={triggerShowQuestions}
				class="flex-1 bg-gradient-to-r from-green-500 to-blue-500 hover:from-green-600 hover:to-blue-600 text-white font-semibold"
			>
				Start Challenge
			</Button>
			<Button color="alternative" class="flex-1">Cancel</Button>
		</div>
	</svelte:fragment>
</Modal>

<Modal title="⏹️ End Challenge?" bind:open={terminateModal} autoclose size="sm" class="bg-gray-900">
	<div class="text-center p-6">
		<div class="w-16 h-16 bg-gradient-to-r from-red-500 to-pink-500 rounded-full flex items-center justify-center mx-auto mb-4">
			<svg class="w-8 h-8 text-white" fill="none" stroke="currentColor" viewBox="0 0 24 24">
				<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 12a9 9 0 11-18 0 9 9 0 0118 0z"/>
				<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 10a1 1 0 011-1h4a1 1 0 011 1v4a1 1 0 01-1 1h-4a1 1 0 01-1-1v-4z"/>
			</svg>
		</div>
		<p class="text-lg text-white mb-2">
			Are you sure you want to end the challenge early?
		</p>
		<p class="text-white/70">
			Your current progress will be saved and submitted.
		</p>
	</div>
	<svelte:fragment slot="footer">
		<div class="flex space-x-3 w-full">
			<Button 
				on:click={terminateProgram}
				class="flex-1 bg-gradient-to-r from-red-500 to-pink-500 hover:from-red-600 hover:to-pink-600 text-white font-semibold"
			>
				End Challenge
			</Button>
			<Button color="alternative" class="flex-1">Continue</Button>
		</div>
	</svelte:fragment>
</Modal>

<style>
	@keyframes fade-in {
		from { opacity: 0; transform: translateY(20px); }
		to { opacity: 1; transform: translateY(0); }
	}
	
	@keyframes slide-up {
		from { opacity: 0; transform: translateY(50px); }
		to { opacity: 1; transform: translateY(0); }
	}
	
	:global(.animate-fade-in) {
		animation: fade-in 0.6s ease-out;
	}
	
	:global(.animate-slide-up) {
		animation: slide-up 0.4s ease-out;
	}
</style>