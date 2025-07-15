# matematic-juego
juego casual
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Juego Matemático Interactivo</title>
  <link rel="stylesheet" href="style.css" />
  <style>
    body {
      font-family: 'Segoe UI', sans-serif;
      background: linear-gradient(to right, #f0f8ff, #d4e0f5);
      margin: 0;
      padding: 0;
      display: flex;
      align-items: center;
      justify-content: center;
      height: 100vh;
    }

    #game-container {
      background: white;
      padding: 30px;
      border-radius: 20px;
      box-shadow: 0 8px 20px rgba(0, 0, 0, 0.2);
      width: 90%;
      max-width: 600px;
      text-align: center;
    }

    h1 {
      margin-bottom: 20px;
      color: #333;
    }

    button {
      background-color: #4CAF50;
      color: white;
      border: none;
      padding: 10px 20px;
      margin: 10px 5px;
      border-radius: 8px;
      font-size: 16px;
      cursor: pointer;
      transition: background-color 0.3s ease;
    }

    button:hover {
      background-color: #45a049;
    }

    button:disabled {
      background-color: #ccc;
      cursor: not-allowed;
    }

    #answers button {
      display: block;
      width: 100%;
      margin: 8px 0;
    }

    .hidden {
      display: none;
    }

    #summary h2 {
      color: #2a2a2a;
      margin-top: 20px;
    }
  </style>
</head>
<body>
  <div id="game-container">
    <h1>Juego Matemático</h1>
    <div id="level-container"></div>
    <div id="question-container" class="hidden">
      <p id="question"></p>
      <div id="answers"></div>
      <button id="next-button" class="hidden">Siguiente</button>
    </div>
    <div id="summary" class="hidden"></div>
  </div>

  <script>
    const questions = [
      // Nivel 1
      [
        {
          question: "¿Cuánto es 5 + 3?",
          options: ["6", "7", "8", "9"],
          answer: "8"
        },
        {
          question: "¿Cuánto es 10 - 4?",
          options: ["5", "6", "7", "8"],
          answer: "6"
        },
        {
          question: "¿Cuánto es 2 * 3?",
          options: ["5", "6", "7", "8"],
          answer: "6"
        }
      ],
      // Nivel 2
      [
        {
          question: "¿Cuál es el número primo?",
          options: ["4", "6", "7", "9"],
          answer: "7"
        },
        {
          question: "¿Cuánto es 15 / 3?",
          options: ["4", "5", "6", "7"],
          answer: "5"
        },
        {
          question: "¿Qué número es par?",
          options: ["3", "5", "7", "8"],
          answer: "8"
        }
      ]
      // Podés agregar hasta 20 niveles aquí
    ];

    let currentLevel = 0;
    let currentQuestion = 0;
    let correctAnswers = 0;
    let totalQuestions = 0;

    const levelContainer = document.getElementById("level-container");
    const questionContainer = document.getElementById("question-container");
    const questionText = document.getElementById("question");
    const answersContainer = document.getElementById("answers");
    const nextButton = document.getElementById("next-button");
    const summary = document.getElementById("summary");

    function loadLevel(level) {
      currentQuestion = 0;
      levelContainer.classList.add("hidden");
      questionContainer.classList.remove("hidden");
      showQuestion();
    }

    function showQuestion() {
      const q = questions[currentLevel][currentQuestion];
      questionText.textContent = q.question;
      answersContainer.innerHTML = "";
      nextButton.classList.add("hidden");

      q.options.forEach(option => {
        const btn = document.createElement("button");
        btn.textContent = option;
        btn.onclick = () => selectAnswer(option);
        answersContainer.appendChild(btn);
      });
    }

    function selectAnswer(option) {
      const q = questions[currentLevel][currentQuestion];
      const buttons = answersContainer.querySelectorAll("button");
      buttons.forEach(btn => {
        btn.disabled = true;
        if (btn.textContent === q.answer) btn.style.background = "lightgreen";
        else btn.style.background = "#f88";
      });
      if (option === q.answer) correctAnswers++;
      totalQuestions++;
      nextButton.classList.remove("hidden");
    }

    nextButton.onclick = () => {
      currentQuestion++;
      if (currentQuestion < questions[currentLevel].length) {
        showQuestion();
      } else {
        currentLevel++;
        if (currentLevel < questions.length) {
          showLevelScreen();
        } else {
          showSummary();
        }
      }
    };

    function showLevelScreen() {
      questionContainer.classList.add("hidden");
      levelContainer.classList.remove("hidden");
      levelContainer.innerHTML = `<button onclick="loadLevel(${currentLevel})">Iniciar Nivel ${currentLevel + 1}</button>`;
    }

    function showSummary() {
      questionContainer.classList.add("hidden");
      summary.classList.remove("hidden");
      const percent = Math.round((correctAnswers / totalQuestions) * 100);
      summary.innerHTML = `
        <h2>¡Juego Finalizado!</h2>
        <p>Respuestas correctas: ${correctAnswers}</p>
        <p>Preguntas totales: ${totalQuestions}</p>
        <p>Efectividad: ${percent}%</p>
      `;
    }

    // Comienza el juego
    levelContainer.innerHTML = `<button onclick="loadLevel(0)">Iniciar Nivel 1</button>`;
  </script>
</body>
</html>
