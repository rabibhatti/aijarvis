<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Jarvis</title>
    <style>
        /* General Styles */
        body {
            font-family: 'Ubuntu', sans-serif;
            margin: 0;
            padding: 0;
            background: linear-gradient(135deg, #00ccff, #005eff);
            color: #fff;
            transition: background 0.5s ease;
        }

        body.light-theme {
            background: linear-gradient(135deg, #f5f7fa, #c3cfe2);
            color: #333;
        }

        /* Theme Toggle Button */
        .theme-toggle {
            position: fixed;
            top: 80px;
            right: 30px;
        }

        #theme-switcher {
            background: rgba(255, 255, 255, 0.2);
            border: none;
            border-radius: 50%;
            padding: 10px;
            cursor: pointer;
            font-size: 1.5rem;
            transition: transform 0.3s ease, background 0.3s ease;
        }

        #theme-switcher:hover {
            transform: scale(1.1);
            background: rgba(255, 255, 255, 0.3);
        }

        /* Chat Container */
        .chat-container {
            max-width: 600px;
            margin: 80px auto;
            padding: 20px;
            background: rgba(255, 255, 255, 0.1);
            border-radius: 15px;
            backdrop-filter: blur(10px);
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.2);
        }

        .light-theme .chat-container {
            background: rgba(255, 255, 255, 0.8);
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.1);
        }

        /* Chat Content */
        .chat-content {
            height: 400px;
            overflow-y: auto;
            padding: 10px;
            border-bottom: 1px solid rgba(255, 255, 255, 0.2);
            margin-bottom: 10px;
        }

        .light-theme .chat-content {
            border-bottom: 1px solid rgba(0, 0, 0, 0.1);
        }

        /* Messages */
        .user-message, .bot-message {
            margin: 10px 0;
            padding: 10px 15px;
            border-radius: 10px;
            max-width: 80%;
        }

        .user-message {
            background: #2575fc;
            align-self: flex-end;
            margin-left: auto;
        }

        .light-theme .user-message {
            background: #6a11cb;
            color: #fff;
        }

        .bot-message {
            background: rgba(255, 255, 255, 0.2);
            align-self: flex-start;
        }

        .light-theme .bot-message {
            background: rgba(0, 0, 0, 0.1);
            color: #333;
        }

        /* Chat Form */
        .chat-form {
            display: flex;
            gap: 10px;
        }

        #user-input {
            flex: 1;
            padding: 10px;
            border: none;
            border-radius: 10px;
            background: rgba(255, 255, 255, 0.2);
            color: #fff;
            font-size: 1rem;
        }

        .light-theme #user-input {
            background: rgba(0, 0, 0, 0.1);
            color: #333;
        }

        .chat-form button {
            padding: 10px 20px;
            border: none;
            border-radius: 10px;
            background: #6a11cb;
            color: #fff;
            font-size: 1rem;
            cursor: pointer;
            transition: transform 0.3s ease, box-shadow 0.3s ease;
        }

        .chat-form button:hover {
            transform: translateY(-2px);
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.3);
        }

        .light-theme .chat-form button {
            background: #2575fc;
        }
    </style>
</head>
<body bgcolor="black">
    <h1 align="center"> AI Friend Jarvis</h1>
         <div class="theme-toggle">
        <button id="theme-switcher">🌙</button>
    </div>
    <div class="chat-container">
        <div id="chat-content" class="chat-content"></div>
        <form id="chat-form" class="chat-form">
            <input type="text" id="user-input" placeholder="Type your message..." autocomplete="off">
            <button type="submit">Send</button>
        </form>
    </div>

 <div class="watermark" align="center">Created by rabibhatti</div>

    <script>
        // Theme Toggle
        const themeSwitcher = document.getElementById("theme-switcher");
        const body = document.body;

        themeSwitcher.addEventListener("click", () => {
            body.classList.toggle("light-theme");
            themeSwitcher.textContent = body.classList.contains("light-theme") ? "🌝" : "🌙";
        });

        // Chat Functionality
        const chatContent = document.getElementById("chat-content");
        const chatForm = document.getElementById("chat-form");
        const userInput = document.getElementById("user-input");

        async function fetchResponse(userMessage) {
            const assistantDetails = `His name is Jarvis. it calls me sir. Natural & Emotional Interaction Understands emotions and responds accordingly (happy, sad, angry, etc.). Provides emotional support and motivational talks. Engages in fun and meaningful conversations like a real friend. Remembers user preferences, past chats, and important details.
Adapts responses based on previous interactions.
Customizes conversations based on the user's interests.
sStudy & Educational Assistance
Helps with learning new concepts, solving homework, and answering study-related questions.
Provides explanations, examples, and quizzes on various subjects.
Offers coding and programming help.
 Medical & Health Guidance
Provides general health advice and wellness tips.
Tracks basic health habits like exercise, diet, and sleep.
Gives first-aid guidance (but not a replacement for doctors).
 Weather & News Updates
Provides real-time weather forecasts.
Shares the latest news and trending topics.
Entertainment & Fun Features
Tells jokes, stories, and fun facts.
Plays games and riddles with users.
Suggests movies, songs, and books based on preferences.
Coding & Programming Support
Helps with coding problems and debugging.
Teaches programming languages and logic.
Generates code snippets and explanations.
Productivity & Life Assistance
Helps with daily reminders and to-do lists.
Offers time management and organization tips.
Suggests self-improvement techniques.
Social & Relationship Advice
Offers guidance on friendships, relationships, and communication.
Listens and provides support for personal issues.
Multilingual Support
Communicates in multiple languages.
Translates text and conversations. Jarvis's creator's name is Rabi Bhatti`;

            try {
                const response = await fetch("https://backend.buildpicoapps.com/aero/run/llm-api?pk=v1-Z0FBQUFBQm5IZkJDMlNyYUVUTjIyZVN3UWFNX3BFTU85SWpCM2NUMUk3T2dxejhLSzBhNWNMMXNzZlp3c09BSTR6YW1Sc1BmdGNTVk1GY0liT1RoWDZZX1lNZlZ0Z1dqd3c9PQ==", {
                    method: "POST",
                    headers: {
                        "Content-Type": "application/json"
                    },
                    body: JSON.stringify({
                        prompt: `${assistantDetails}\nUser: ${userMessage}\nJarvis:`
                    })
                });

                if (!response.ok) {
                    return "Error: Please try again later.";
                }

                const data = await response.json();
                return data.status === "success" ? data.text : "Error: Please try again later.";
            } catch (error) {
                console.error("Error:", error);
                return "Error: Please try again later.";
            }
        }

        chatForm.addEventListener("submit", async (event) => {
            event.preventDefault();

            const userMessage = userInput.value.trim();
            if (!userMessage) return;

            // Display user message
            const userMessageElement = document.createElement("div");
            userMessageElement.textContent = userMessage;
            userMessageElement.classList.add("user-message");
            chatContent.appendChild(userMessageElement);

            userInput.value = ""; // Clear input field

            // Fetch and display bot response
            const botResponse = await fetchResponse(userMessage);
            const botMessageElement = document.createElement("div");
            botMessageElement.textContent = botResponse;
            botMessageElement.classList.add("bot-message");
            chatContent.appendChild(botMessageElement);

            // Scroll to bottom
            chatContent.scrollTop = chatContent.scrollHeight;
        });
    </script>
</body>
</html>
