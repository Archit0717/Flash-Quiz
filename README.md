# Flash-Quiz
Create a flashcard quiz app for studying
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Flashcard Quiz App</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background-color: #f9f9f9;
      text-align: center;
      padding: 20px;
    }
    .flashcard {
      background-color: white;
      border: 1px solid #ccc;
      border-radius: 10px;
      padding: 20px;
      max-width: 500px;
      margin: 20px auto;
      box-shadow: 0 4px 8px rgba(0,0,0,0.1);
    }
    .controls, .form {
      margin: 10px 0;
    }
    button, input {
      margin: 5px;
      padding: 10px;
      font-size: 16px;
    }
    input {
      width: 80%;
    }
  </style>
</head>
<body>

<h1>Flashcard Quiz App</h1>
<div class="flashcard">
  <div id="card-content">No flashcards available.</div>
  <div class="controls">
    <button onclick="toggleAnswer()" id="toggle-btn" disabled>Show Answer</button>
    <br>
    <button onclick="prevCard()" id="prev-btn" disabled>Previous</button>
    <button onclick="nextCard()" id="next-btn" disabled>Next</button>
  </div>
</div>

<div class="form">
  <input type="text" id="question" placeholder="Enter question">
  <input type="text" id="answer" placeholder="Enter answer">
  <br>
  <button onclick="addFlashcard()">Add Flashcard</button>
  <button onclick="editFlashcard()" id="edit-btn" disabled>Edit Flashcard</button>
  <button onclick="deleteFlashcard()" id="delete-btn" disabled>Delete Flashcard</button>
</div>

<script>
  let flashcards = [];
  let currentIndex = 0;
  let showingAnswer = false;

  function displayCard() {
    const cardContent = document.getElementById('card-content');
    const toggleBtn = document.getElementById('toggle-btn');
    const prevBtn = document.getElementById('prev-btn');
    const nextBtn = document.getElementById('next-btn');
    const editBtn = document.getElementById('edit-btn');
    const deleteBtn = document.getElementById('delete-btn');

    if (flashcards.length === 0) {
      cardContent.textContent = 'No flashcards available.';
      toggleBtn.disabled = true;
      prevBtn.disabled = true;
      nextBtn.disabled = true;
      editBtn.disabled = true;
      deleteBtn.disabled = true;
    } else {
      const card = flashcards[currentIndex];
      cardContent.textContent = showingAnswer ? card.answer : card.question;
      toggleBtn.textContent = showingAnswer ? 'Show Question' : 'Show Answer';
      toggleBtn.disabled = false;
      prevBtn.disabled = currentIndex === 0;
      nextBtn.disabled = currentIndex === flashcards.length - 1;
      editBtn.disabled = false;
      deleteBtn.disabled = false;
    }
  }

  function toggleAnswer() {
    showingAnswer = !showingAnswer;
    displayCard();
  }

  function prevCard() {
    if (currentIndex > 0) {
      currentIndex--;
      showingAnswer = false;
      displayCard();
    }
  }

  function nextCard() {
    if (currentIndex < flashcards.length - 1) {
      currentIndex++;
      showingAnswer = false;
      displayCard();
    }
  }

  function addFlashcard() {
    const question = document.getElementById('question').value.trim();
    const answer = document.getElementById('answer').value.trim();
    if (question && answer) {
      flashcards.push({ question, answer });
      currentIndex = flashcards.length - 1;
      showingAnswer = false;
      displayCard();
      document.getElementById('question').value = '';
      document.getElementById('answer').value = '';
    }
  }

  function editFlashcard() {
    const question = document.getElementById('question').value.trim();
    const answer = document.getElementById('answer').value.trim();
    if (question && answer && flashcards.length > 0) {
      flashcards[currentIndex] = { question, answer };
      showingAnswer = false;
      displayCard();
    }
  }

  function deleteFlashcard() {
    if (flashcards.length > 0) {
      flashcards.splice(currentIndex, 1);
      if (currentIndex >= flashcards.length) currentIndex = flashcards.length - 1;
      showingAnswer = false;
      displayCard();
    }
  }

  displayCard();
</script>

</body>
</html>
