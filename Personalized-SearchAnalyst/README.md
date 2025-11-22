## Hyper-Personalized Search Analyst Agent

This project demonstrates a full-stack, real-time AI Agent built using client-side JavaScript, Google's Gemini API (with Search Grounding), and Firestore for persistent state management.

### Project Goal

The agent functions as a Personalized Research Analyst. Instead of simply searching the web, the user defines a professional Persona and Key Topics. The agent then performs a real-time Google search, analyzes the results from the perspective of the given persona, and filters the noise to provide a single, actionable insight and a required action item.

This showcases the ability to turn a Large Language Model (LLM) into a goal-oriented, data-aware tool.

### Technologies Used

- Frontend: HTML, JavaScript (ES6+), and Tailwind CSS (via CDN) for a modern, responsive interface.

- Agent Core (LLM): Google's Gemini 2.5 Flash API with Google Search Grounding enabled for real-time data retrieval and analysis.

- Database: Google Cloud Firestore (via Firebase SDK) for persistent storage and real-time history synchronization.

- Authentication: Firebase Anonymous/Custom Token Authentication (handled by the Canvas environment for secure, user-scoped data access).

### Core Agent Logic

1. Input: Takes a detailed `Persona` (e.g., "Compliance Officer"), `Topics`, and a `Search Query`.

2. Orchestration: Sends a single, complex prompt to the Gemini API, instructing it to use the built-in Search tool (`tools: [{ "google_search": {} }]`) to find real-time information.

3. Filtering & Analysis: The agent's prompt strictly mandates the output format, ensuring it provides:

    - TITLE (The key finding).
  
    - SUMMARY (A concise explanation, grounded in sources).
  
    - ACTION (A specific step the persona must take).

4. Persistence: The resulting insight and source links are saved to a private Firestore collection (`/artifacts/{appId}/users/{userId}/summaries`).

5. Real-Time Display: An `onSnapshot` listener ensures that the "Saved Insights History" updates instantly upon saving new data.

### Repository Structure

[`index.html`](.index.html) : (Main Application File) Contains all HTML, CSS, and JavaScript logic. This is the single runnable application file.

[`firestore.rules`](firestore.rules) : (Database Configuration) Contains the secure rules needed to restrict read/write access to the user's own data path.

#### README.md: This file.

### How to Run Locally (Simulated)

This application requires a running Firebase/Canvas environment to handle the `__firebase_config` and `__initial_auth_token` variables.

**1. Get the Code:** Save the contents of [`index.html`](.index.html) and [`firestore.rules`](firestore.rules) into a local directory.

**2. Set up Firebase:**

    - Create a new project in the Firebase Console.
  
    - Initialize Cloud Firestore in Production Mode.
  
    - Copy and Publish the contents of firestore.rules to the "Rules" tab.

**3. Set up Gemini:** Get a Gemini API Key from Google AI Studio.

**4. Deployment:** Because this is a single HTML file using ES Modules, you can serve it easily using any simple static file server (like Python's `http.server` or VS Code's Live Server extension).

**5. Test:** Open the [`index.html`](.index.html) file in your browser, enter a query (e.g., "latest news on crypto taxation"), and run the agent to see the real-time analysis appear and the history save instantly!
