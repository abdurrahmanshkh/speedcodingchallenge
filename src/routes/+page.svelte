<script>
    // Problem statements array
	let questions = [
		'(4 points) Write a function to count the number of vowels in a given string.',
		'(6 points) Implement a method to sort an array of integers using the quicksort algorithm.',
		'(5 points) Generate the first n Fibonacci numbers.',
		'(4 points) Create a function to check if a given string is a palindrome.',
		'(5 points) Write a recursive method to calculate the factorial of a number.',
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

</script>

<main>
    <div class="container">
        <h1 class="text-2xl font-bold mb-4">Coding Challenge</h1>

        <!-- Display current problem statement -->
        <p class="mb-4">{currentQuestion}</p>

        <!-- Input box for user's answer -->
        <input
            id="answer-input"
            type="text"
            placeholder="Type your solution here"
            class="border rounded p-2 w-full mb-4"
            bind:value={answers[currentIndex]}
        />

        <!-- Button to go to the next problem statement -->
        <button
            class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded"
            on:click={nextQuestion}>
            Next
        </button>
    </div>
</main>

<style>
    .container {
        max-width: 600px;
        margin: 0 auto;
        padding: 20px;
    }
</style>
