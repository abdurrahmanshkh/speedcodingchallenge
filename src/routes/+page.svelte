<script>
	import { Card, Button, Label, Input, P } from 'flowbite-svelte';
	import { List, Li, Modal } from 'flowbite-svelte';

	let name = '';
	let phone = '';
	let email = '';
	let password = '';
	let login = false;
	let round1 = false;
	let round2 = false;
	let round3 = false;
	let showSummary = false;
	let key = 'ietcre2024';

	let loginModal = false;
	let round1Modal = false;
	let round2Modal = false;
	let round3Modal = false;

	// Timer logic
	let timeLeft = 0;
	let timerId;
	let startTime;
	let elapsedTimeRound1 = 0;
	let elapsedTimeRound2 = 0;
	let elapsedTimeRound3 = 0;
	let totalTime = 0;

	function startTimer(minutes, onTimeUp) {
		timeLeft = minutes * 60;
		startTime = Date.now(); // Record the start time
		timerId = setInterval(() => {
			timeLeft--;
			if (timeLeft <= 0) {
				clearInterval(timerId);
				onTimeUp();
			}
		}, 1000);
	}

	function stopTimer() {
		clearInterval(timerId);
	}

	// Capture elapsed time for each round
	function getElapsedTime() {
		const currentTime = Date.now();
		return Math.floor((currentTime - startTime) / 1000); // Calculate elapsed time in seconds
	}

	// Function to submit the form
	function submitForm() {
		if (password == key) {
			loginModal = true;
		}
	}

	let pythonCode1 = '';
	let javaCode1 = '';
	let round1CodeNo = 0;
	let round1ans = '';

	let pythonCode2 = '';
	let javaCode2 = '';
	let round2CodeNo = 0;
	let round2ans = '';

	let pythonCode3 = '';
	let javaCode3 = '';
	let round3CodeNo = 0;
	let round3ans = '';

	// Function to load random Python and Java codes for round 1
	async function loadRound1Codes() {
		login = true;
		round1 = true;
		const totalCodes = 13; // Total codes in round 1
		const srno = Math.floor(Math.random() * totalCodes) + 1; // Random number between 1 and 13
		round1CodeNo = srno;

		// File paths for Python and Java codes
		const pythonFilePath = `/codes/1_${srno}.py`;
		const javaFilePath = `/codes/J1_${srno}.java`;

		try {
			// Fetch and display Python code
			pythonCode1 = await fetchFileContent(pythonFilePath);

			// Fetch and display Java code
			javaCode1 = await fetchFileContent(javaFilePath);
			startTimer(10, handleRound1Timeout); // 10-minute timer for round 1
		} catch (error) {
			console.error('Error loading code files:', error);
		}
	}

	// Function to load random Python and Java codes for round 2
	async function loadRound2Codes() {
		stopTimer();
		elapsedTimeRound1 = getElapsedTime(); // Save time taken in round 1
		round1 = false;
		round2 = true;
		const totalCodes = 13; // Total codes in round 2
		const srno = Math.floor(Math.random() * totalCodes) + 1; // Random number between 1 and 13
		round2CodeNo = srno;

		// File paths for Python and Java codes
		const pythonFilePath = `/codes/2_${srno}.py`;
		const javaFilePath = `/codes/J2_${srno}.java`;

		try {
			// Fetch and display Python code
			pythonCode2 = await fetchFileContent(pythonFilePath);

			// Fetch and display Java code
			javaCode2 = await fetchFileContent(javaFilePath);
			startTimer(15, handleRound2Timeout); // 15-minute timer for round 2
		} catch (error) {
			console.error('Error loading code files:', error);
		}
	}

	// Function to load random Python and Java codes for round 3
	async function loadRound3Codes() {
		stopTimer();
		elapsedTimeRound2 = getElapsedTime(); // Save time taken in round 2
		round2 = false;
		round3 = true;
		const totalCodes = 10; // Total codes in round 3
		const srno = Math.floor(Math.random() * totalCodes) + 1; // Random number between 1 and 13
		round3CodeNo = srno;

		// File paths for Python and Java codes
		const pythonFilePath = `/codes/3_${srno}.py`;
		const javaFilePath = `/codes/J3_${srno}.java`;

		try {
			// Fetch and display Python code
			pythonCode3 = await fetchFileContent(pythonFilePath);

			// Fetch and display Java code
			javaCode3 = await fetchFileContent(javaFilePath);
			startTimer(20, handleRound3Timeout); // 20-minute timer for round 3
		} catch (error) {
			console.error('Error loading code files:', error);
		}
	}

	// Handle timeouts for each round
	function handleRound1Timeout() {
		stopTimer();
		elapsedTimeRound1 = getElapsedTime(); // Save time taken in round 1 when timeout occurs
		if (round1ans.length === 0) {
			terminateEvent();
		} else {
			loadRound2Codes();
		}
	}

	function handleRound2Timeout() {
		stopTimer();
		elapsedTimeRound2 = getElapsedTime(); // Save time taken in round 2 when timeout occurs
		if (round2ans.length === 0) {
			terminateEvent();
		} else {
			loadRound3Codes();
		}
	}

	function handleRound3Timeout() {
		stopTimer();
		elapsedTimeRound3 = getElapsedTime(); // Save time taken in round 3 when timeout occurs
		endEvent();
	}

	// Function to end the event
	function endEvent() {
		stopTimer();
		elapsedTimeRound3 = getElapsedTime(); // Save time taken in round 3 if event is manually ended
		totalTime = elapsedTimeRound1 + elapsedTimeRound2 + elapsedTimeRound3;
		round3 = false;
		showSummary = true;
	}

	function terminateEvent() {
		totalTime = elapsedTimeRound1 + elapsedTimeRound2 + elapsedTimeRound3;
		round1 = false;
		round2 = false;
		round3 = false;
		showSummary = true;
	}

	// Helper function to fetch file content
	async function fetchFileContent(filePath) {
		const response = await fetch(filePath);
		if (!response.ok) {
			throw new Error(`Failed to load file: ${filePath}`);
		}
		return response.text();
	}

	function downloadSummaryAsTxt() {
		const summaryText = `Summary

Participant Details:
Name: ${name}
Phone: ${phone}
Email: ${email}

Round 1 - Question No: ${round1CodeNo}
Answer: ${round1ans}
Time Taken: ${elapsedTimeRound1} seconds

Round 2 - Question No: ${round2CodeNo}
Answer: ${round2ans}
Time Taken: ${elapsedTimeRound2} seconds

Round 3 - Question No: ${round3CodeNo}
Answer: ${round3ans}
Time Taken: ${elapsedTimeRound3} seconds

Total Time Taken: ${totalTime} seconds
    `;

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
	{#if !login}
		<div class="grid grid-cols-3 gap-8">
			<Card class="align-center h-full min-w-full justify-center">
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
			<Card class="col-span-2 min-w-full">
				<P class="mb-4 text-2xl font-bold">Code Reverse Engineering - Event Rules</P>
				<P class="text-xl font-bold">Participation</P>
				<List class="mb-4">
					<Li>Login with your registered details before the event starts.</Li>
					<Li>Ensure accurate login information to avoid disqualification.</Li>
				</List>

				<P class="text-xl font-bold">Event Structure</P>
				<List class="mb-4">
					<Li>3 rounds with increasing difficulty.</Li>
					<Li>Code snippets available in Python and Java.</Li>
					<Li>Identify the algorithm or function within the time limit.</Li>
				</List>

				<P class="text-xl font-bold">Time Limits</P>
				<List class="mb-4">
					<Li><strong>Round 1:</strong> 10 minutes</Li>
					<Li><strong>Round 2:</strong> 15 minutes</Li>
					<Li><strong>Round 3:</strong> 20 minutes</Li>
				</List>

				<P class="text-xl font-bold">Submission</P>
				<List class="mb-4">
					<Li>Enter your answer in the input box before time runs out.</Li>
					<Li>Proceed to the next round only if you submit a correct answer.</Li>
					<Li>No skipping rounds; incomplete submissions result in elimination.</Li>
				</List>

				<P class="text-xl font-bold">Winner Announcement</P>
				<List class="">
					<Li>Winners will be announced at the end of the event.</Li>
					<Li>Organizer decisions are final.</Li>
				</List>
			</Card>
		</div>
	{/if}

	{#if login && round1}
		<Card class="min-w-full">
			<P class="mb-4 text-2xl font-bold">Round 1</P>
			<P class="mb-2 text-xl font-bold">Python Code</P>
			<pre class="rounded-lg bg-gray-100 p-4 dark:bg-gray-800">{pythonCode1}</pre>
			<P class="mb-2 mt-4 text-xl font-bold">Java Code</P>
			<pre class="rounded-lg bg-gray-100 p-4 dark:bg-gray-800">{javaCode1}</pre>
			<P class="mb-2 mt-4 text-xl font-bold">
				Time Remaining: {Math.floor(timeLeft / 60)}:{timeLeft % 60}
			</P>
			<!-- Answer input form -->
			<form class="mt-4" on:submit|preventDefault={() => (round1Modal = true)}>
				<Label class="space-y-2">
					<span>Your answer</span>
					<Input type="text" placeholder="Enter your answer" required bind:value={round1ans} />
				</Label>
				<Button class="mt-4" type="submit">Submit and move to next Round</Button>
			</form>
		</Card>
	{/if}

	{#if login && round2}
		<Card class="min-w-full">
			<P class="mb-4 text-2xl font-bold">Round 2</P>
			<P class="mb-2 text-xl font-bold">Python Code</P>
			<pre class="rounded-lg bg-gray-100 p-4 dark:bg-gray-800">{pythonCode2}</pre>
			<P class="mb-2 mt-4 text-xl font-bold">Java Code</P>
			<pre class="rounded-lg bg-gray-100 p-4 dark:bg-gray-800">{javaCode2}</pre>
			<P class="mb-2 mt-4 text-xl font-bold">
				Time Remaining: {Math.floor(timeLeft / 60)}:{timeLeft % 60}
			</P>
			<!-- Answer input form -->
			<form class="mt-4" on:submit|preventDefault={() => (round2Modal = true)}>
				<Label class="space-y-2">
					<span>Your answer</span>
					<Input type="text" placeholder="Enter your answer" required bind:value={round2ans} />
				</Label>
				<Button class="mt-4" type="submit">Submit and move to next Round</Button>
			</form>
		</Card>
	{/if}

	{#if login && round3}
		<Card class="min-w-full">
			<P class="mb-4 text-2xl font-bold">Round 3</P>
			<P class="mb-2 text-xl font-bold">Python Code</P>
			<pre class="rounded-lg bg-gray-100 p-4 dark:bg-gray-800">{pythonCode3}</pre>
			<P class="mb-2 mt-4 text-xl font-bold">Java Code</P>
			<pre class="rounded-lg bg-gray-100 p-4 dark:bg-gray-800">{javaCode3}</pre>
			<P class="mb-2 mt-4 text-xl font-bold">
				Time Remaining: {Math.floor(timeLeft / 60)}:{timeLeft % 60}
			</P>
			<!-- Answer input form -->
			<form class="mt-4" on:submit|preventDefault={() => (round3Modal = true)}>
				<Label class="space-y-2">
					<span>Your answer</span>
					<Input type="text" placeholder="Enter your answer" required bind:value={round3ans} />
				</Label>
				<Button class="mt-4" type="submit">Submit and end the event</Button>
			</form>
		</Card>
	{/if}
	<!-- show all question no and ans along with participant details in summary -->
	{#if showSummary}
		<Card class="min-w-full">
			<P class="mb-4 text-2xl font-bold">Summary</P>
			<P class="mb-2 text-xl font-bold">Participant Details</P>
			<P class="mb-2"><strong>Name:</strong> {name}</P>
			<P class="mb-2"><strong>Phone:</strong> {phone}</P>
			<P class="mb-4"><strong>Email:</strong> {email}</P>
			<P class="mb-2 text-xl font-bold">Round 1 - Question No: {round1CodeNo}</P>
			<P class="mb-2"><strong>Answer:</strong> {round1ans}</P>
			<P class="mb-4"><strong>Time Taken:</strong> {elapsedTimeRound1} seconds</P>
			<P class="mb-2 text-xl font-bold">Round 2 - Question No: {round2CodeNo}</P>
			<P class="mb-2"><strong>Answer:</strong> {round2ans}</P>
			<P class="mb-4"><strong>Time Taken:</strong> {elapsedTimeRound2} seconds</P>
			<P class="mb-2 text-xl font-bold">Round 3 - Question No: {round3CodeNo}</P>
			<P class="mb-2"><strong>Answer:</strong> {round3ans}</P>
			<P class="mb-4"><strong>Time Taken:</strong> {elapsedTimeRound3} seconds</P>
			<P class="text-xl font-bold">Total Time Taken: {totalTime} seconds</P>
			<Button class="mt-4 w-64" on:click={downloadSummaryAsTxt}>Download Summary</Button>
		</Card>
	{/if}
</main>

<Modal title="Terms of Service" bind:open={loginModal} autoclose>
	<p class="text-base leading-relaxed text-gray-500 dark:text-gray-400">
		Are you sure you want to start now?
	</p>
	<svelte:fragment slot="footer">
		<Button on:click={() => loadRound1Codes()}>Start Now</Button>
		<Button color="alternative">Decline</Button>
	</svelte:fragment>
</Modal>

<Modal title="Submit Round 1" bind:open={round1Modal} autoclose>
	<p class="text-base leading-relaxed text-gray-500 dark:text-gray-400">
		Are you sure you want to submit your answer and move to next round?
	</p>
	<svelte:fragment slot="footer">
		<Button on:click={() => loadRound2Codes()}>Submit</Button>
		<Button color="alternative">Decline</Button>
	</svelte:fragment>
</Modal>

<Modal title="Submit Round 2" bind:open={round2Modal} autoclose>
	<p class="text-base leading-relaxed text-gray-500 dark:text-gray-400">
		Are you sure you want to submit your answer and move to next round?
	</p>
	<svelte:fragment slot="footer">
		<Button on:click={() => loadRound3Codes()}>Submit</Button>
		<Button color="alternative">Decline</Button>
	</svelte:fragment>
</Modal>

<Modal title="Submit Round 3" bind:open={round3Modal} autoclose>
	<p class="text-base leading-relaxed text-gray-500 dark:text-gray-400">
		Are you sure you want to submit your answer and end the competition?
	</p>
	<svelte:fragment slot="footer">
		<Button on:click={endEvent}>Submit</Button>
		<Button color="alternative">Decline</Button>
	</svelte:fragment>
</Modal>
