Single HTML file to interface with google's gemini models including the latest gemini pro 2.5. 
SImply open the page in a web browser, enter your key, select a model, and begin chatting.

This should work on any operating system with a modenn web browser.
Contributions are welcome, the initial version was written by gemini pro 2.5 03-35.


GeminiHTML Documentation
1. Overview
This web application provides a user-friendly interface to interact directly with Google's Gemini AI models via their Generative Language API. It allows users to enter an API key, select a compatible model, input a prompt, configure generation parameters, send the request to the API, and view the formatted response. It includes features like theme switching, API key storage, and detailed status reporting.
2. Core Features
Here's a breakdown of the components and their functions:
Header:
Title: Displays "Gemini API Interface".
Theme Switcher (🌙/☀️): Allows toggling between light and dark visual themes. The preference is saved in the browser's Local Storage.
API Key Input:
Field: A password-masked input field where you enter your Google AI Studio API Key. This key is required to authenticate with the Google API.
Remember Key Checkbox: If checked, the application will store your API key in the browser's Local Storage. This avoids needing to re-enter it every time you open the app in the same browser.
Warning: A notice reminds users that storing API keys in browser storage is insecure, especially on shared computers. Use this feature with caution.
Functionality: Entering a valid key and moving focus away from the input (blurring) will trigger the application to fetch the list of available Gemini models associated with that key.
Model Selection:
Dropdown: A select menu to choose which specific Gemini model to use for the request.
Functionality: This dropdown is initially disabled and shows "-- Enter API Key to Load Models --". Once a valid API key is entered and models are successfully fetched, it populates with compatible models (those supporting the generateContent method). The display name of the model is shown. If the gemini-1.5-pro-latest model is available, it will be pre-selected. Selecting a model updates the footer text. If no compatible models are found or an error occurs during fetching, appropriate messages are displayed.
Prompt Input:
Textarea: A multi-line text area where you type your prompt or question for the Gemini model.
Shortcut: Pressing Ctrl+Enter (or Cmd+Enter on Mac) while focused on this textarea will trigger the "Send Prompt" action.
Configuration Options (Collapsible Section):
Summary (<summary>): Click "Configuration Options" to expand or collapse the advanced settings.
Temperature (0-1): Controls the randomness of the output. Lower values (e.g., 0.2) make the output more deterministic and focused, while higher values (e.g., 0.9) increase randomness and creativity. Default: 0.7.
Max Output Tokens: Sets the maximum number of tokens (roughly words or parts of words) the model should generate in its response. Note: The model might stop before reaching this limit. Default: 8192 (This default was increased from typical API defaults).
Top P (0-1): Nucleus sampling parameter. The model considers only the tokens whose cumulative probability mass exceeds this threshold. Lower values narrow the choices, higher values allow more diversity. Default: 0.95.
Top K (integer): The model considers only the top 'k' most likely tokens at each step of generation. Default: 40.
Button Group:
Send Prompt: Clicking this button sends the prompt, selected model, API key, and configuration settings to the Gemini API. It's disabled while a request is in progress.
Clear: Clears the Prompt input and the Response area, resets the Status message. It does not clear the API key, model selection, or configuration settings.
Status Area:
Display: Shows the current status of the application.
States:
Loading: Displays a spinning loader animation and "Processing request..." while waiting for the API response.
Success: Shows "Response received." along with the Finish Reason provided by the API (e.g., STOP, MAX_TOKENS) and Token Usage information (Total, Prompt, Candidates) if available.
Warning: Displays status messages in a warning color (yellow/orange). This is used specifically when the Finish Reason is MAX_TOKENS (indicating potential truncation) or any other reason besides STOP (indicating potential incompleteness).
Error: Displays error messages in red, such as "API Key is missing," "Please select a model," API errors (with status code and message from Google), or network fetch errors.
Response Area:
Display: Shows the content generated by the Gemini model.
Formatting: The raw text response from the API is parsed as Markdown and rendered as HTML. This includes formatting for headings, lists, bold/italic text, links, etc.
Code Highlighting: Code blocks within the response are automatically detected and syntax-highlighted using Highlight.js (adapting to the selected light/dark theme).
Footer:
Link: Provides a link to the official Google Gemini API documentation page.
Current Model: Displays the display name of the currently selected model in the dropdown (or "N/A" if none is selected).
3. How to Use
Obtain API Key: Get an API key from Google AI Studio (https://ai.google.dev/).
Enter API Key: Paste your key into the "API Key" field.
(Optional) Remember Key: Check the "Remember Key" box if you want the browser to store the key for future sessions (understand the security implications).
Wait for Models: The application will automatically try to fetch compatible models. Wait for the "Select Model" dropdown to become active and populated.
Select Model: Choose the desired Gemini model from the dropdown. The footer will update to show your selection.
(Optional) Configure: Expand "Configuration Options" and adjust Temperature, Max Output Tokens, Top P, or Top K if needed.
Enter Prompt: Type your question or instruction into the "Your Prompt" text area.
Send: Click the "Send Prompt" button or press Ctrl+Enter / Cmd+Enter.
Monitor Status: Observe the Status area for "Processing request...", followed by success, warning, or error messages. Check the status message for the Finish Reason and token counts upon completion.
View Response: The model's response will appear in the "Response" area, formatted with Markdown and code highlighting.
Clear (Optional): Click "Clear" to reset the prompt and response areas for a new query.
4. Understanding the Response & Status
Markdown Rendering: The output isn't plain text; it uses Markdown for better readability (headings, lists, bold, etc.).
Code Blocks: Code snippets are automatically highlighted for clarity.
Status - Finish Reason: This is important:
STOP: The model finished generating naturally.
MAX_TOKENS: The model stopped because it reached the Max Output Tokens limit you set. The response might be incomplete. The status message will show as a warning.
SAFETY: The model stopped because the prompt or response potentially violated safety guidelines. The response might be empty or incomplete. The status message will show as a warning/error.
Other reasons (RECITATION, OTHER): Indicate other stopping conditions. The status message will show as a warning.
Status - Token Usage: Provides insight into how many tokens were used for your prompt and the generated response (candidates). This is useful for understanding potential costs or limits.
Status - Warnings/Errors: Pay attention to red (error) or yellow/orange (warning) messages in the status area. Errors often indicate problems with the API key, model selection, or the request itself. Warnings often relate to why the model stopped generating (e.g., token limits, safety). Check the browser's developer console (usually F12) for more detailed error information if needed, as the application logs full API responses and errors there.
5. API Key Security
Remember the warning provided in the interface. Storing API keys in Local Storage is convenient but less secure than server-side solutions or environment variables, especially on shared or potentially compromised machines. Use the "Remember Key" feature responsibly.
6. Troubleshooting
Models Not Loading:
Ensure the API Key is correct and valid.
Check your internet connection.
Check the browser's developer console (F12) for error messages related to fetching models (e.g., 401 Unauthorized, 403 Forbidden, network errors).
Error Sending Prompt:
Verify an API Key is entered and a Model is selected.
Check the Status area and the developer console for specific API error messages (e.g., invalid arguments, quota exceeded, key issues).
Incomplete Response:
Check the Finish Reason in the Status area. If it's MAX_TOKENS, increase the "Max Output Tokens" value in the Configuration Options and try again.
If the reason is SAFETY, your prompt or the expected response might be triggering safety filters. Try rephrasing the prompt.
