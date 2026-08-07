# System Design Interactive Flashcards

Practice one question at a time. Submit your answer to see whether it is correct, read a short explanation, and then go to the next question.

<div class="flashcard-quiz" id="flashcard-quiz">
  <div class="quiz-header">
    <p id="quiz-progress">Question 1</p>
    <p id="quiz-score">Score: 0</p>
  </div>

  <h2 id="quiz-question">Loading question...</h2>

  <form id="quiz-form">
    <div id="quiz-options"></div>

    <div class="quiz-actions">
      <button type="submit" id="submit-btn">Submit answer</button>
      <button type="button" id="next-btn" hidden>Next question</button>
    </div>
  </form>

  <div id="quiz-feedback" class="quiz-feedback" aria-live="polite"></div>
</div>

<style>
  .flashcard-quiz {
    max-width: 780px;
    margin: 1rem auto;
    padding: 1rem;
    border: 1px solid #d5d7da;
    border-radius: 10px;
    background: #ffffff;
  }

  .quiz-header {
    display: flex;
    justify-content: space-between;
    gap: 1rem;
    margin-bottom: 0.75rem;
    font-weight: 600;
  }

  #quiz-question {
    margin-top: 0;
    margin-bottom: 1rem;
  }

  .option-item {
    display: block;
    margin: 0.5rem 0;
    padding: 0.6rem 0.75rem;
    border: 1px solid #e1e4e8;
    border-radius: 8px;
    cursor: pointer;
  }

  .option-item:hover {
    background: #f8f9fb;
  }

  .quiz-actions {
    display: flex;
    gap: 0.5rem;
    margin-top: 1rem;
  }

  .quiz-actions button {
    border: none;
    padding: 0.6rem 1rem;
    border-radius: 8px;
    cursor: pointer;
    font-weight: 600;
  }

  #submit-btn {
    background: #0b57d0;
    color: #ffffff;
  }

  #next-btn {
    background: #2e7d32;
    color: #ffffff;
  }

  .quiz-feedback {
    margin-top: 1rem;
    padding: 0.75rem;
    border-radius: 8px;
    display: none;
  }

  .quiz-feedback.correct {
    display: block;
    background: #e8f5e9;
    border: 1px solid #a5d6a7;
  }

  .quiz-feedback.wrong {
    display: block;
    background: #ffebee;
    border: 1px solid #ef9a9a;
  }

  @media (prefers-color-scheme: dark) {
    .flashcard-quiz {
      background: #1f1f1f;
      border-color: #3d3d3d;
    }

    .option-item {
      border-color: #4a4a4a;
    }

    .option-item:hover {
      background: #2a2a2a;
    }
  }
</style>

<script>
(function () {
  function initQuiz() {
    var root = document.getElementById("flashcard-quiz");
    if (!root || root.dataset.ready === "true") {
      return;
    }
    root.dataset.ready = "true";

    var questions = [
      {
        question: "Which CAP property is always present in distributed systems?",
        options: ["Consistency", "Availability", "Partition Tolerance", "Latency"],
        answer: 2,
        description: "Network partitions are unavoidable, so the real trade-off is consistency vs availability."
      },
      {
        question: "For a one-way server-to-client live update stream, which protocol is usually simpler?",
        options: ["WebSockets", "SSE", "gRPC", "FTP"],
        answer: 1,
        description: "SSE is simpler for unidirectional real-time updates such as scores and notifications."
      },
      {
        question: "When should denormalization usually be introduced?",
        options: ["At the start of schema design", "When reads are heavy and joins become expensive", "Only for write-heavy systems", "Never"],
        answer: 1,
        description: "Start normalized first, then denormalize only when read performance needs it."
      },
      {
        question: "A good non-functional requirement should be:",
        options: ["Generic", "Measurable and specific", "As short as possible", "Optional"],
        answer: 1,
        description: "Specific targets like 'search latency < 500 ms' are testable and actionable."
      },
      {
        question: "What is a common threshold before database sharding is seriously considered?",
        options: ["Around 500 GB", "Around 1 TB", "Around 10-100 TB", "Around 50 GB"],
        answer: 2,
        description: "Sharding adds complexity, so teams usually wait until very large data or throughput pressure."
      },
      {
        question: "In cache-aside pattern, what happens on a cache miss?",
        options: ["Return empty response", "Read from DB and populate cache", "Delete the key", "Scale the cache cluster"],
        answer: 1,
        description: "On miss, fetch from the database, put result in cache, and return to caller."
      },
      {
        question: "For external public APIs, which protocol is usually preferred?",
        options: ["gRPC", "REST", "Raw TCP", "Kafka"],
        answer: 1,
        description: "REST is widely supported by browsers and clients; gRPC is often used internally."
      },
      {
        question: "Which change most improves search over billions of documents?",
        options: ["Add Redis cache", "Move to inverted index engine", "Add 10% CPU", "Increase app threads only"],
        answer: 1,
        description: "Index-based search architecture usually provides the largest gain over brute-force scanning."
      }
    ];

    var current = 0;
    var score = 0;
    var locked = false;

    var progressEl = document.getElementById("quiz-progress");
    var scoreEl = document.getElementById("quiz-score");
    var questionEl = document.getElementById("quiz-question");
    var optionsEl = document.getElementById("quiz-options");
    var feedbackEl = document.getElementById("quiz-feedback");
    var formEl = document.getElementById("quiz-form");
    var submitBtn = document.getElementById("submit-btn");
    var nextBtn = document.getElementById("next-btn");

    function renderQuestion() {
      locked = false;
      feedbackEl.className = "quiz-feedback";
      feedbackEl.style.display = "none";
      feedbackEl.textContent = "";
      nextBtn.hidden = true;
      submitBtn.hidden = false;
      submitBtn.disabled = false;

      var q = questions[current];
      progressEl.textContent = "Question " + (current + 1) + " of " + questions.length;
      scoreEl.textContent = "Score: " + score;
      questionEl.textContent = q.question;

      optionsEl.innerHTML = q.options
        .map(function (option, index) {
          return (
            '<label class="option-item">' +
            '<input type="radio" name="quiz-option" value="' + index + '"> ' +
            option +
            "</label>"
          );
        })
        .join("");
    }

    function showResults() {
      progressEl.textContent = "Completed";
      questionEl.textContent = "You finished the flashcards.";
      optionsEl.innerHTML = "";
      submitBtn.hidden = true;
      nextBtn.hidden = true;
      feedbackEl.className = "quiz-feedback correct";
      feedbackEl.style.display = "block";
      feedbackEl.innerHTML =
        "Final score: <strong>" +
        score +
        " / " +
        questions.length +
        "</strong><br>Refresh the page to practice again.";
    }

    formEl.addEventListener("submit", function (event) {
      event.preventDefault();
      if (locked) {
        return;
      }

      var selected = formEl.querySelector('input[name="quiz-option"]:checked');
      if (!selected) {
        feedbackEl.className = "quiz-feedback wrong";
        feedbackEl.style.display = "block";
        feedbackEl.textContent = "Please choose an answer before submitting.";
        return;
      }

      locked = true;
      submitBtn.disabled = true;
      submitBtn.hidden = true;
      nextBtn.hidden = false;

      var selectedIndex = Number(selected.value);
      var q = questions[current];
      var isCorrect = selectedIndex === q.answer;

      if (isCorrect) {
        score += 1;
        feedbackEl.className = "quiz-feedback correct";
        feedbackEl.innerHTML = "Correct. " + q.description;
      } else {
        feedbackEl.className = "quiz-feedback wrong";
        feedbackEl.innerHTML =
          "Wrong. Correct answer: <strong>" +
          q.options[q.answer] +
          "</strong>. " +
          q.description;
      }
      feedbackEl.style.display = "block";
      scoreEl.textContent = "Score: " + score;
    });

    nextBtn.addEventListener("click", function () {
      current += 1;
      if (current >= questions.length) {
        showResults();
        return;
      }

      renderQuestion();
      root.scrollIntoView({ behavior: "smooth", block: "start" });
    });

    renderQuestion();
  }

  if (typeof document$ !== "undefined" && document$.subscribe) {
    document$.subscribe(function () {
      initQuiz();
    });
  } else {
    document.addEventListener("DOMContentLoaded", initQuiz);
  }
})();
</script>

<div class="quiz-box">
<b>How does Spring Boot auto configuration work internally like the @ConditionalOnClass and @ConditionalOnMissingBean?</b>
<details class="quiz-toggle">
<summary>Reveal Answer</summary>
Spring Boot scan auto configuration classes listed on the auto configurations.import file. Each class activates when the condition matches.
Example ConsitionalOnClass in case a dependency is on the class path or @ConditionalOnMissingBean in case the bean is not defined.
</details>
</div>