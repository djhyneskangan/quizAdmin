<script>
	import { supabase } from '$lib/supabaseClient';

	let data = [];

	async function getQuizQuestions() {
		const { data: quizData, error } = await supabase
			.from('questionsTable')
			.select('question_id, Question, optionsTable(answer_text, is_correct)')
			.order('question_id'); //Order the questions by ID

		if (quizData) {
			data = quizData;
			console.log(data);
		}

		if (error) {
			console.error('Error:', error);
		}
	}

	getQuizQuestions();

	function getCorrectAnswer(optionsTable) {
		const correctAnswer = optionsTable.find((option) => option.is_correct);
		return correctAnswer ? correctAnswer.answer_text : 'No correct answer specified';
	}
</script>

<div class="heading">
	<h1>Code Masons Admin Page</h1>
</div>

<div class="quiz-container">
	{#each data as question}
		<div class="question">{question.question_id}: {question.Question}:</div>
		<div class="answer">
			<ol>
				<li>{question.optionsTable[0].answer_text}</li>
				<li>{question.optionsTable[1].answer_text}</li>
				<li>{question.optionsTable[2].answer_text}</li>
				<li>{question.optionsTable[3].answer_text}</li>
			</ol>
		</div>
		<div class="correct-answer">
			Correct Answer: {getCorrectAnswer(question.optionsTable)}
		</div>
	{/each}
</div>

<style>
	.quiz-container {
		max-width: 700px;
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

	.answer {
		display: flex;
		align-items: center;
		margin-bottom: 8px;
	}

	.heading {
		display: flex;
		justify-content: center;
		align-items: center;
	}

	.correct-answer {
		display: flex;
	}
</style>
