Ecco il codice completo per una pagina HTML autonoma e moderna (single-file: HTML + CSS + JavaScript).

Le caratteristiche incluse:

* **Interattivo con feedback immediato**: evidenzia in verde la risposta corretta e in rosso quella errata appena l'utente clicca.
* **Punteggio finale**: calcola il punteggio finale e permette di riavviare il quiz.
* **Design pulito e responsive**: visualizzazione ottimizzata sia per desktop sia per smartphone/tablet.

### Come salvarlo ed eseguirlo

1. Salva il codice sottostante in un file di testo chiamato ad esempio `quiz_m5s_genova.html`.
2. Aprilo con qualsiasi browser (Firefox, Chrome, Edge, Safari).

```html
<!DOCTYPE html>
<html lang="it">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Quiz: Pilastri Valoriali M5S Genova</title>
  <style>
    :root {
      --bg-color: #f4f6f9;
      --card-bg: #ffffff;
      --primary: #c99700;
      --primary-hover: #a37a00;
      --text-main: #2b2d42;
      --text-muted: #6c757d;
      --border-color: #e0e0e0;
      --correct-bg: #d4edda;
      --correct-border: #28a745;
      --correct-text: #155724;
      --wrong-bg: #f8d7da;
      --wrong-border: #dc3545;
      --wrong-text: #721c24;
      --radius: 8px;
    }

    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
      font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
    }

    body {
      background-color: var(--bg-color);
      color: var(--text-main);
      padding: 24px 16px;
      line-height: 1.5;
    }

    .container {
      max-width: 760px;
      margin: 0 auto;
    }

    header {
      background: var(--card-bg);
      padding: 24px;
      border-radius: var(--radius);
      box-shadow: 0 2px 8px rgba(0, 0, 0, 0.06);
      margin-bottom: 24px;
      text-align: center;
    }

    header h1 {
      font-size: 1.5rem;
      margin-bottom: 8px;
      color: #1a1a1a;
    }

    header p {
      color: var(--text-muted);
      font-size: 0.95rem;
    }

    .card {
      background: var(--card-bg);
      border-radius: var(--radius);
      padding: 20px;
      margin-bottom: 20px;
      box-shadow: 0 2px 6px rgba(0, 0, 0, 0.05);
      border: 1px solid var(--border-color);
    }

    .question-category {
      font-size: 0.8rem;
      text-transform: uppercase;
      font-weight: 700;
      letter-spacing: 0.05em;
      color: var(--primary);
      margin-bottom: 8px;
    }

    .question-title {
      font-size: 1.05rem;
      font-weight: 600;
      margin-bottom: 16px;
    }

    .options {
      display: flex;
      flex-direction: column;
      gap: 10px;
    }

    .option-btn {
      display: flex;
      align-items: flex-start;
      gap: 12px;
      width: 100%;
      text-align: left;
      padding: 12px 14px;
      background: #fafafa;
      border: 1px solid var(--border-color);
      border-radius: var(--radius);
      cursor: pointer;
      font-size: 0.92rem;
      color: var(--text-main);
      transition: background 0.2s, border-color 0.2s;
    }

    .option-btn:hover:not(:disabled) {
      background: #f0f0f0;
      border-color: #bbb;
    }

    .option-btn:disabled {
      cursor: default;
    }

    .option-letter {
      font-weight: 700;
      min-width: 20px;
    }

    /* Feedback visuali */
    .option-btn.correct {
      background: var(--correct-bg) !important;
      border-color: var(--correct-border) !important;
      color: var(--correct-text) !important;
      font-weight: 500;
    }

    .option-btn.wrong {
      background: var(--wrong-bg) !important;
      border-color: var(--wrong-border) !important;
      color: var(--wrong-text) !important;
    }

    .explanation {
      margin-top: 14px;
      padding: 10px 14px;
      font-size: 0.88rem;
      border-radius: 6px;
      display: none;
    }

    .explanation.show {
      display: block;
    }

    .explanation.correct {
      background: #eafaf1;
      color: var(--correct-text);
      border-left: 4px solid var(--correct-border);
    }

    .explanation.wrong {
      background: #fdf2f2;
      color: var(--wrong-text);
      border-left: 4px solid var(--wrong-border);
    }

    /* Sezione Risultato */
    #score-section {
      display: none;
      background: var(--card-bg);
      padding: 24px;
      border-radius: var(--radius);
      text-align: center;
      box-shadow: 0 2px 10px rgba(0, 0, 0, 0.08);
      margin-top: 24px;
    }

    #score-section.show {
      display: block;
    }

    .score-title {
      font-size: 1.25rem;
      margin-bottom: 10px;
    }

    .score-value {
      font-size: 2.2rem;
      font-weight: 700;
      color: var(--text-main);
      margin-bottom: 16px;
    }

    .restart-btn {
      background: #2b2d42;
      color: #fff;
      border: none;
      padding: 10px 22px;
      border-radius: 6px;
      font-size: 0.95rem;
      font-weight: 600;
      cursor: pointer;
      transition: opacity 0.2s;
    }

    .restart-btn:hover {
      opacity: 0.9;
    }
  </style>
</head>
<body>

  <div class="container">
    <header>
      <h1>Pilastri Valoriali Movimento 5 Stelle Genova</h1>
      <p>Verifica le priorità e le linee guida dell'azione amministrativa nel Comune di Genova.</p>
    </header>

    <div id="quiz-container"></div>

    <section id="score-section">
      <h2 class="score-title">Quiz Completato!</h2>
      <div class="score-value" id="score-display">0 / 5</div>
      <p id="score-feedback" style="margin-bottom: 20px; color: var(--text-muted);"></p>
      <button class="restart-btn" onclick="initQuiz()">Ricomincia il quiz</button>
    </section>
  </div>

  <script>
    const questions = [
      {
        id: 1,
        category: "Domanda 1: Giustizia Sociale, Lavoro e Servizi Pubblici Locali",
        question: "Qual è l'obiettivo prioritario del Movimento 5 Stelle a Genova nell'ambito della giustizia sociale e dei servizi pubblici locali?",
        options: [
          { text: "A) Privatizzare i servizi sociali per ridurne i costi di gestione.", correct: false },
          { text: "B) Difendere e rafforzare i servizi pubblici essenziali (welfare, asili nido, politiche per la casa) e promuovere il lavoro dignitoso negli appalti.", correct: true },
          { text: "C) Concentrare tutti gli investimenti esclusivamente nel centro cittadino.", correct: false },
          { text: "D) Sostituire le clausole sociali negli appalti con criteri di incentivo aziendale.", correct: false }
        ]
      },
      {
        id: 2,
        category: "Domanda 2: Legalità, Trasparenza e Monitoraggio della Spesa",
        question: "In materia di legalità e trasparenza amministrativa, quale misura caratterizza l'azione del Movimento 5 Stelle nei contratti e nelle aziende partecipate?",
        options: [
          { text: "A) L'assegnazione diretta dei contratti per velocizzare le opere pubbliche.", correct: false },
          { text: "B) La riduzione dei controlli antimafia per incentivare gli investimenti esteri.", correct: false },
          { text: "C) La massima trasparenza nelle procedure amministrative, negli appalti e nelle nomine, con un presidio rigoroso contro la corruzione e la gestione opaca dei fondi.", correct: true },
          { text: "D) L'eliminazione delle rendicontazioni pubbliche sui fondi del PNRR.", correct: false }
        ]
      },
      {
        id: 3,
        category: "Domanda 3: Transizione Ecologica, Mobilità Sostenibile e Tutela del Territorio",
        question: "Qual è l'approccio del Movimento 5 Stelle di Genova in tema di sviluppo urbano e mobilità sostenibile?",
        options: [
          { text: "A) Incentivare il trasporto privato a combustibile fossile e costruire nuovi parcheggi in centro.", correct: false },
          { text: "B) Dare priorità al trasporto pubblico locale a basse emissioni (elettrificazione flotta AMT), alla manutenzione del territorio contro il dissesto e allo stop al consumo di suolo.", correct: true },
          { text: "C) Favorire nuove colate di cemento nelle aree verdi e nelle zone collinari.", correct: false },
          { text: "D) Sostituire la raccolta differenziata puntuale con la gestione indistinta dei rifiuti.", correct: false }
        ]
      },
      {
        id: 4,
        category: "Domanda 4: Etica Pubblica e Sobrietà Istituzionale",
        question: "Cosa si intende per \"sobrietà istituzionale\" nell'azione di governo del Movimento 5 Stelle a Genova?",
        options: [
          { text: "A) L'azzeramento di qualsiasi investimento pubblico nei servizi essenziali.", correct: false },
          { text: "B) Un utilizzo rigoroso delle risorse affinché ogni euro speso generi benefici tangibili per la collettività.", correct: true },
          { text: "C) La rinuncia totale a presentare bilanci preventivi e consuntivi.", correct: false },
          { text: "D) L'assegnazione delle risorse comunali in base a logiche di appartenenza politica o clientelare.", correct: false }
        ]
      },
      {
        id: 5,
        category: "Domanda 5: Partecipazione, Ascolto e Centralità dei Municipi",
        question: "Come intende promuovere la partecipazione cittadina il Movimento 5 Stelle nell'amministrazione comunale?",
        options: [
          { text: "A) Sopprimendo i Municipi per centralizzare ogni decisione a Palazzo Tursi.", correct: false },
          { text: "B) Valorizzando i Municipi come organi di prossimità reale e coinvolgendo sistematicamente comitati e associazioni nei processi decisionali.", correct: true },
          { text: "C) Limitando le consultazioni pubbliche alle sole scadenze elettorali.", correct: false },
          { text: "D) Sostituire il bilancio partecipativo con decisioni unilaterali della Giunta.", correct: false }
        ]
      }
    ];

    let userAnswers = {};

    function initQuiz() {
      userAnswers = {};
      const container = document.getElementById("quiz-container");
      const scoreSection = document.getElementById("score-section");
      scoreSection.classList.remove("show");
      container.innerHTML = "";

      questions.forEach((q, qIndex) => {
        const card = document.createElement("div");
        card.className = "card";
        card.id = `card-${qIndex}`;

        const cat = document.createElement("div");
        cat.className = "question-category";
        cat.textContent = q.category;

        const title = document.createElement("div");
        title.className = "question-title";
        title.textContent = q.question;

        const optionsDiv = document.createElement("div");
        optionsDiv.className = "options";

        q.options.forEach((opt, optIndex) => {
          const btn = document.createElement("button");
          btn.className = "option-btn";
          btn.innerHTML = opt.text;
          btn.onclick = () => selectOption(qIndex, optIndex);
          optionsDiv.appendChild(btn);
        });

        const feedback = document.createElement("div");
        feedback.className = "explanation";
        feedback.id = `feedback-${qIndex}`;

        card.appendChild(cat);
        card.appendChild(title);
        card.appendChild(optionsDiv);
        card.appendChild(feedback);
        container.appendChild(card);
      });
    }

    function selectOption(qIndex, selectedOptIndex) {
      if (userAnswers[qIndex] !== undefined) return; // Già risposta

      userAnswers[qIndex] = selectedOptIndex;
      const q = questions[qIndex];
      const card = document.getElementById(`card-${qIndex}`);
      const buttons = card.querySelectorAll(".option-btn");
      const feedback = document.getElementById(`feedback-${qIndex}`);
      const isCorrect = q.options[selectedOptIndex].correct;

      // Disabilita tutti i pulsanti della domanda ed evidenzia
      buttons.forEach((btn, idx) => {
        btn.disabled = true;
        if (q.options[idx].correct) {
          btn.classList.add("correct");
        } else if (idx === selectedOptIndex) {
          btn.classList.add("wrong");
        }
      });

      // Messaggio di spiegazione/conferma
      feedback.classList.add("show", isCorrect ? "correct" : "wrong");
      feedback.textContent = isCorrect 
        ? "✓ Risposta corretta!" 
        : "✗ Risposta errata. L'opzione corretta è stata evidenziata in verde.";

      // Controllo se il quiz è completato
      if (Object.keys(userAnswers).length === questions.length) {
        showFinalScore();
      }
    }

    function showFinalScore() {
      let score = 0;
      questions.forEach((q, idx) => {
        if (q.options[userAnswers[idx]].correct) {
          score++;
        }
      });

      const scoreSection = document.getElementById("score-section");
      const scoreDisplay = document.getElementById("score-display");
      const scoreFeedback = document.getElementById("score-feedback");

      scoreDisplay.textContent = `${score} / ${questions.length}`;

      if (score === questions.length) {
        scoreFeedback.textContent = "Ottimo risultato! Conosci perfettamente i pilastri programmatici del M5S a Genova.";
      } else if (score >= 3) {
        scoreFeedback.textContent = "Buon risultato! Hai una buona padronanza delle linee guida e dei valori di governo.";
      } else {
        scoreFeedback.textContent = "Puoi fare meglio: ripassa le direttive e riprova il quiz.";
      }

      scoreSection.classList.add("show");
      scoreSection.scrollIntoView({ behavior: "smooth" });
    }

    // Avvio al caricamento della pagina
    window.onload = initQuiz;
  </script>
</body>
</html>

```
