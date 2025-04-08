// Questions par défaut
const questions = [
    {
        question: "Tu préfères avoir des ailes ou des bras supplémentaires ?",
        choices: ["Des ailes", "Des bras supplémentaires"],
        votes: [0, 0] // Tableau pour stocker les votes pour chaque choix
    },
    {
        question: "Tu préfères vivre sans internet ou sans téléphone ?",
        choices: ["Sans internet", "Sans téléphone"],
        votes: [0, 0]
    },
    {
        question: "Tu préfères être un génie mais incompris ou un idiot populaire ?",
        choices: ["Un génie incompris", "Un idiot populaire"],
        votes: [0, 0]
    }
];

let userQuestions = [];  // Pour stocker les questions créées par les utilisateurs.
let currentQuestionIndex = 0;
let answers = [];

// Fonction pour afficher une question
function displayQuestion() {
    const questionElement = document.getElementById("question");
    const choiceButtons = document.querySelectorAll(".choice-btn");

    const currentQuestion = getNextQuestion();
    
    questionElement.textContent = currentQuestion.question;
    choiceButtons[0].textContent = currentQuestion.choices[0];
    choiceButtons[1].textContent = currentQuestion.choices[1];
}

// Fonction pour obtenir la question suivante (en incluant les questions utilisateur)
function getNextQuestion() {
    if (currentQuestionIndex < questions.length) {
        return questions[currentQuestionIndex];
    } else if (currentQuestionIndex - questions.length < userQuestions.length) {
        return userQuestions[currentQuestionIndex - questions.length];
    }
    return null; // Fin des questions
}

// Fonction pour avancer à la question suivante
function nextQuestion(choiceIndex) {
    answers.push(getNextQuestion().choices[choiceIndex]);
    questions[currentQuestionIndex].votes[choiceIndex]++;  // Enregistrer le vote
    
    currentQuestionIndex++;

    if (currentQuestionIndex < questions.length + userQuestions.length) {
        // Transition animation
        document.getElementById("question-container").style.opacity = 0;
        setTimeout(() => {
            displayQuestion();
            document.getElementById("question-container").style.opacity = 1;
        }, 300);
    } else {
        showResults();
        displayStatistics();
    }
}

// Affichage des résultats
function showResults() {
    const resultContainer = document.getElementById("result-container");
    const summary = document.getElementById("summary");

    resultContainer.classList.remove("hidden");

    let resultText = "Voici tes choix :<br><ul>";
    answers.forEach((answer, index) => {
        resultText += `<li>Question ${index + 1}: ${answer}</li>`;
    });
    resultText += "</ul>";

    summary.innerHTML = resultText;

    // Masquer la question
    document.getElementById("question-container").classList.add("hidden");
}

// Fonction pour recommencer
function restartGame() {
    currentQuestionIndex = 0;
    answers = [];
    document.getElementById("result-container").classList.add("hidden");
    document.getElementById("question-container").classList.remove("hidden");
    displayQuestion();
}

// Ajouter une nouvelle question
document.getElementById("create-form").addEventListener("submit", function(event) {
    event.preventDefault();

    const newQuestionText = document.getElementById("new-question").value;
    const newChoice1 = document.getElementById("choice-1").value;
    const newChoice2 = document.getElementById("choice-2").value;

    if (newQuestionText && newChoice1 && newChoice2) {
        const newQuestion = {
            question: newQuestionText,
            choices: [newChoice1, newChoice2],
            votes: [0, 0]  // Initialisation des votes
        };
        userQuestions.push(newQuestion); // Ajouter la question à la liste des questions utilisateur
        alert("Votre question a été ajoutée !");
        document.getElementById("create-form").reset();
    }
});

// Fonction pour afficher les statistiques
function displayStatistics() {
    const statsContainer = document.getElementById("stats");
    statsContainer.innerHTML = "";  // Réinitialiser les statistiques

    questions.concat(userQuestions).for
