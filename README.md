# ASM-quiz-website
Student test quiz
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ASM Quiz - Quiz</title>
    <style>
        body {
            background-color: #34495e;
            color: white;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            flex-direction: column;
            font-family: Arial, sans-serif;
            text-align: center;
        }
        .quiz-container {
            width: 50%;
            margin: auto;
        }
        .btn {
            padding: 10px 20px;
            margin: 10px;
            background-color: #1abc9c;
            border: none;
            color: white;
            cursor: pointer;
            font-size: 18px;
        }
        .btn:hover {
            background-color: #16a085;
        }
        .timer {
            font-size: 24px;
            margin-bottom: 20px;
        }
    </style>
</head>
<body>
    <div class="quiz-container">
        <div class="timer" id="timer">15:00</div>
        <div id="question"></div>
        <div id="options"></div>
        <button class="btn" id="prevBtn" onclick="prevQuestion()">Previous</button>
        <button class="btn" id="nextBtn" onclick="nextQuestion()">Next</button>
    </div>

    <script>
        const questions = [
            // Add 50 questions here in the same format
            { question: "What will be the output of the following code?\n\nint x = 5;\nprintf(\"%d\", ++x);", options: ["4", "5", "6", "None of the above"], answer: "6" },
            { question: "What is the size of an `int` in C?", options: ["2 bytes", "4 bytes", "8 bytes", "Depends on the compiler"], answer: "Depends on the compiler" },
            // ... (48 more questions)
        ];

        let shuffledQuestions = questions.sort(() => 0.5 - Math.random()).slice(0, 15);
        let currentQuestionIndex = 0;
        let score = 0;
        let timeLeft = 900;

        function displayQuestion() {
            const currentQuestion = shuffledQuestions[currentQuestionIndex];
            document.getElementById('question').innerText = `Question ${currentQuestionIndex + 1}: ${currentQuestion.question}`;
            const optionsHtml = currentQuestion.options.map((option, index) => 
                `<input type="radio" name="option" id="option${index}" value="${option}"> <label for="option${index}">${option}</label><br>`
            ).join('');
            document.getElementById('options').innerHTML = optionsHtml;
        }

        function nextQuestion() {
            const selectedOption = document.querySelector('input[name="option"]:checked');
            if (selectedOption) {
                if (selectedOption.value === shuffledQuestions[currentQuestionIndex].answer) {
                    score += 5;
                }
            }

            if (currentQuestionIndex < shuffledQuestions.length - 1) {
                currentQuestionIndex++;
                displayQuestion();
            } else {
                submitQuiz();
            }
        }

        function prevQuestion() {
            if (currentQuestionIndex > 0) {
                currentQuestionIndex--;
                displayQuestion();
            }
        }

        function submitQuiz() {
            clearInterval(timer);
            const userName = localStorage.getItem('username');
            const message = `Dear ${userName}, you scored ${score} out of 75.`;
            alert(message);
            window.close();
        }

        function startTimer() {
            timer = setInterval(function() {
                timeLeft--;
                const minutes = Math.floor(timeLeft / 60);
                const seconds = timeLeft % 60;
                document.getElementById('timer').innerText = `${minutes}:${seconds < 10 ? '0' : ''}${seconds}`;

                if (timeLeft <= 0) {
                    submitQuiz();
                }
            }, 1000);
        }

        displayQuestion();
        startTimer();
    </script>
</body>
</html>
