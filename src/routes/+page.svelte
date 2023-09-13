<script>
	import { supabase } from '$lib/supabaseClient';

	let data = [];

	async function getQuizQuestions() {
		const { data: quizData, error } = await supabase
			.from('questionsTable')
			.select('question_id, Question, optionsTable(answer_text, is_correct)')
			.order('question_id');

		if (quizData) {
			data = quizData;
			console.log(data);
		}

		if (error) {
			console.error('Error:', error);
		}
	}

	getQuizQuestions();
</script>

<div class="heading">
	<h1>Code Masons Admin Page</h1>
</div>

<div class="quiz-container">
	{#each data as question}
		<div class="question">{question.question_id}: {question.Question}:</div>
		<div class="answer-container">
			{#each question.optionsTable as option}
				<div class="answer" class:correct={option.is_correct}>
					{option.answer_text}
				</div>
			{/each}
		</div>
	{/each}
</div>

<style>
	.quiz-container {
		max-width: 600px;
		margin: 0 auto;
		background-color: #ffffff;
		box-shadow: 0px 2px 4px rgba(0, 0, 0, 0.1);
		border-radius: 8px;
		padding: 16px;
	}

	.question {
		font-weight: bold;
		font-size: 18px;
		margin-bottom: 10px;
		padding-top: 20px;
	}

	.answer-container {
		display: flex;
		flex-wrap: wrap;
		gap: 10px; /* Adjust the gap between answers as needed */
	}

	.answer {
		flex: 1;
		padding: 10px;
		border: 2px solid #ccc;
		border-radius: 8px;
		text-align: center;
		font-weight: bold;
		background-color: rgb(247, 74, 39); /* Default background color for incorrect answers */
		color: white; /* Default text color for incorrect answers */
	}

	.correct {
		background-color: green;
		color: white;
		border-color: green;
	}

	.heading {
		display: flex;
		justify-content: center;
		align-items: center;
	}
</style>
