<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Voting Machine</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f4f4f4;
            color: #333;
            margin: 0;
            padding: 0;
        }

        .container {
            max-width: 600px;
            margin: 20px auto;
            padding: 30px;
            background-color: #fff;
            border-radius: 10px;
            box-shadow: 0 6px 12px rgba(0, 0, 0, 0.15);
        }

        h1, h3 {
            text-align: center;
            color: #444;
        }

        label {
            font-size: 18px;
            color: #555;
        }

        input[type="text"], input[type="number"] {
            width: 100%;
            padding: 12px;
            margin: 10px 0;
            border: 1px solid #ccc;
            border-radius: 6px;
            font-size: 18px;
        }

        button {
            padding: 14px 22px;
            font-size: 18px;
            color: #ffffff;
            background-color: #000000;
            border: none;
            border-radius: 6px;
            cursor: pointer;
            margin: 10px;
            transition: background-color 0.3s ease;
        }

        button:hover {
            background-color: #a50000;
        }

        .hidden {
            display: none;
        }

        .party-buttons {
            display: flex;
            flex-wrap: wrap;
            justify-content: space-around;
            margin: 20px 0;
        }

        .party-buttons button {
            width: 45%;
            padding: 18px;
            background-color: #000000;
            border-radius: 10px;
            border: none;
            cursor: pointer;
            font-size: 18px;
        }

        .party-buttons button:hover {
            background-color: #ca3131;
        }

        #message {
            text-align: center;
            color: #000000;
            font-size: 20px;
            margin-top: 20px;
        }

        #final-results {
            margin-top: 20px;
            padding-top: 20px;
            border-top: 1px solid #ddd;
        }

        #results p {
            font-size: 20px;
            color: #333;
        }

        #thank-you {
            text-align: center;
            font-size: 20px;
            color: #0051ff;
            margin-top: 20px;
        }
    </style>
</head>
<body>
    <div class="container">
        <div id="login-page">
            <h1>Electrical Voting Machine</h1>
            <h3>Login</h3>
            <label for="name">Enter your name:</label>
            <input type="text" id="name" placeholder="Enter your name" required>
            <br><br>
            <label for="age">Enter your age to verify eligibility:</label>
            <input type="number" id="age" placeholder="Enter your age" required>
            <br><br>
            <button onclick="validateLogin()">Login</button>
        </div>
        
        <div id="voting-page" class="hidden">
            <h3>Available parties:</h3>
            <div class="party-buttons">
                <button onclick="castVote('VTK')">VTK</button>
                <button onclick="castVote('JBP')">JBP</button>
                <button onclick="castVote('IEAS')">IEAS</button>
                <button onclick="castVote('ETC')">ETC</button>
            </div>
            <button onclick="goToConfirmationPage()" id="go-to-confirmation-btn" class="hidden">Go to Confirmation</button>
        </div>

        <div id="confirmation-page" class="hidden">
            <h3>Would you like to end the voting session?</h3>
            <button onclick="endVoting()">Yes, End Voting</button>
            <button onclick="allowAnotherUser()">No, Another User</button>
        </div>

        <div id="final-results" class="hidden">
            <h2>Final Voting Results</h2>
            <div id="results"></div>
            <div id="thank-you" class="hidden">
                <h3>Thank you for using the voting machine!</h3>
            </div>
        </div>

        <div id="message" class="hidden"></div>
    </div>

    <script>
        let votes = { "VTK": 0, "JBP": 0, "IEAS": 0, "ETC": 0 };
        let voters = new Set();
        let currentVoter = "";
        let hasVoted = false;

        function validateLogin() {
            let name = document.getElementById("name").value.trim();
            let age = parseInt(document.getElementById("age").value);
            let messageElement = document.getElementById("message");

            messageElement.classList.add("hidden");

            if (name === "" || isNaN(age) || age < 18) {
                messageElement.textContent = name === "" ? "Name is required!" : "You must be at least 18 years old to vote.";
                messageElement.classList.remove("hidden");
                return;
            }

            if (voters.has(name)) {
                messageElement.textContent = "You have already voted.";
                messageElement.classList.remove("hidden");
                return;
            }

            currentVoter = name;
            document.getElementById("login-page").classList.add("hidden");
            document.getElementById("voting-page").classList.remove("hidden");
        }

        function castVote(party) {
            if (currentVoter && !voters.has(currentVoter) && !hasVoted) {
                votes[party]++;
                voters.add(currentVoter);
                hasVoted = true;
                document.getElementById("go-to-confirmation-btn").classList.remove("hidden");
            }
        }

        function goToConfirmationPage() {
            document.getElementById("voting-page").classList.add("hidden");
            document.getElementById("confirmation-page").classList.remove("hidden");
        }

        function endVoting() {
            document.getElementById("confirmation-page").classList.add("hidden");
            document.getElementById("final-results").classList.remove("hidden");
            document.getElementById("results").innerHTML = Object.entries(votes).map(([party, count]) => `<p>${party}: ${count} vote(s)</p>`).join('');
            document.getElementById("thank-you").classList.remove("hidden");
        }

        function allowAnotherUser() {
            hasVoted = false;
            currentVoter = "";
            document.getElementById("confirmation-page").classList.add("hidden");
            document.getElementById("login-page").classList.remove("hidden");
            document.getElementById("name").value = "";
            document.getElementById("age").value = "";
        }
    </script>
</body>
</html>
