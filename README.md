# Pink-Panther-Comment-Generator
This is a comment generator used by the Pink Panther teammates to ease the creation of comments.
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Click-to-Comment Builder</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Inter', sans-serif;
            background-color: #f0f4f8; /* Light gray background */
        }
        .container {
            max-width: 900px;
            margin: 2rem auto;
            padding: 1.5rem;
            background-color: #ffffff;
            border-radius: 1rem;
            box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.1), 0 4px 6px -2px rgba(0, 0, 0, 0.05);
        }
        .word-button {
            @apply bg-blue-500 hover:bg-blue-600 text-white font-semibold py-2 px-4 rounded-lg shadow-md transition-all duration-200 ease-in-out transform hover:scale-105 focus:outline-none focus:ring-2 focus:ring-blue-400 focus:ring-opacity-75;
            min-width: 100px; /* Ensure buttons have a minimum width */
            margin: 0.5rem; /* Add margin around buttons */
        }
        .control-button {
            @apply bg-gray-600 hover:bg-gray-700 text-white font-semibold py-2 px-4 rounded-lg shadow-md transition-all duration-200 ease-in-out transform hover:scale-105 focus:outline-none focus:ring-2 focus:ring-gray-500 focus:ring-opacity-75;
        }
        textarea {
            @apply resize-y; /* Allow vertical resizing */
            min-height: 120px; /* Minimum height for the textarea */
        }
        /* Message box for copy confirmation */
        .message-box {
            position: fixed;
            bottom: 20px;
            left: 50%;
            transform: translateX(-50%);
            background-color: #4CAF50; /* Green */
            color: white;
            padding: 10px 20px;
            border-radius: 8px;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
            z-index: 1000;
            opacity: 0;
            transition: opacity 0.5s ease-in-out;
        }
        .message-box.show {
            opacity: 1;
        }
    </style>
</head>
<body class="p-4">
    <div class="container">
        <h1 class="text-3xl font-bold text-center text-gray-800 mb-6">Comment Builder</h1>

        <!-- Text Area for Compiled Comment -->
        <textarea id="commentOutput"
                  class="w-full p-4 mb-4 border border-gray-300 rounded-lg focus:ring-blue-500 focus:border-blue-500 text-gray-700 text-lg"
                  placeholder="Click words below to build your comment..."
                  readonly></textarea>

        <!-- Control Buttons -->
        <div class="flex justify-center space-x-4 mb-8">
            <button id="clearButton" class="control-button">Clear Comment</button>
            <button id="copyButton" class="control-button">Copy to Clipboard</button>
        </div>

        <!-- Word/Phrase Buttons Container -->
        <div id="wordButtonsContainer" class="flex flex-wrap justify-center gap-3">
            <!-- Buttons will be dynamically loaded here by JavaScript -->
        </div>
    </div>

    <!-- Message Box for Copy Confirmation -->
    <div id="messageBox" class="message-box"></div>

    <script>
        // Array of predefined words and phrases
        // YOU CAN EDIT THIS ARRAY WITH YOUR OWN WORDS AND PHRASES!
        const predefinedWords = [
            "Pink Panther - scam -",
            "New account.",
            "Suspicious listings.",
            "Listing taken from the web.",
            "Malicious IP.",
            "Carrier suspected fraud.",
            "Suspicious drop off point.",
            "Non responsive member.",
            "1st High value TX.",
            "Suspicious fingerprints.",
            "Signs of label manipulation.",
            "Known scam pattern.",
            "Listing/item commonly used with scam cases.",
            "Suspicious deleted items."
            // Add or remove phrases here, each enclosed in double quotes and separated by a comma.
        ];

        const commentOutput = document.getElementById('commentOutput');
        const wordButtonsContainer = document.getElementById('wordButtonsContainer');
        const clearButton = document.getElementById('clearButton');
        const copyButton = document.getElementById('copyButton');
        const messageBox = document.getElementById('messageBox');

        // Function to display a temporary message
        function showMessage(message, type = 'success') {
            messageBox.textContent = message;
            messageBox.className = 'message-box show'; // Reset classes and show
            if (type === 'error') {
                messageBox.style.backgroundColor = '#f44336'; // Red for error
            } else {
                messageBox.style.backgroundColor = '#4CAF50'; // Green for success
            }

            setTimeout(() => {
                messageBox.classList.remove('show');
            }, 2000); // Message disappears after 2 seconds
        }

        // Function to create and append word buttons
        function createWordButtons() {
            // Clear existing buttons before creating new ones (useful if array changes)
            wordButtonsContainer.innerHTML = ''; 
            predefinedWords.forEach(word => {
                const button = document.createElement('button');
                button.textContent = word;
                button.className = 'word-button'; // Apply Tailwind classes
                button.addEventListener('click', () => {
                    // Add a space before appending if the textarea isn't empty
                    // and the new word doesn't start with punctuation that typically follows text
                    if (commentOutput.value.length > 0 && !commentOutput.value.endsWith(' ') && !word.startsWith(',') && !word.startsWith('.')) {
                        commentOutput.value += ' ';
                    }
                    commentOutput.value += word;
                    commentOutput.scrollTop = commentOutput.scrollHeight; // Scroll to bottom
                });
                wordButtonsContainer.appendChild(button);
            });
        }

        // Event listener for Clear button
        clearButton.addEventListener('click', () => {
            commentOutput.value = '';
            showMessage('Comment cleared!', 'success');
        });

        // Event listener for Copy button
        copyButton.addEventListener('click', () => {
            if (commentOutput.value) {
                // Use document.execCommand('copy') for better iframe compatibility
                const tempInput = document.createElement('textarea');
                tempInput.value = commentOutput.value;
                document.body.appendChild(tempInput);
                tempInput.select();
                try {
                    document.execCommand('copy');
                    showMessage('Comment copied to clipboard!', 'success');
                } catch (err) {
                    console.error('Failed to copy text: ', err);
                    showMessage('Failed to copy comment.', 'error');
                } finally {
                    document.body.removeChild(tempInput);
                }
            } else {
                showMessage('Nothing to copy!', 'error');
            }
        });

        // Initialize the app when the DOM is fully loaded
        document.addEventListener('DOMContentLoaded', createWordButtons);
    </script>
</body>
</html>
