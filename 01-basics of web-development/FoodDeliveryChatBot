const express = require("express");
const bodyParser = require("body-parser");
const { SessionsClient } = require("@google-cloud/dialogflow");

const app = express();
const port = 5000;

// Middleware to parse JSON requests
app.use(bodyParser.json());

// Load Dialogflow credentials
const projectId = "YOUR_DIALOGFLOW_PROJECT_ID"; // Replace with your actual Dialogflow Project ID
const sessionClient = new SessionsClient({
    keyFilename: "your-service-account.json", // Path to your service account JSON key
});

// Function to send user message to Dialogflow
async function processMessage(message, sessionId) {
    const sessionPath = sessionClient.projectAgentSessionPath(projectId, sessionId);

    const request = {
        session: sessionPath,
        queryInput: {
            text: {
                text: message,
                languageCode: "en",
            },
        },
    };

    const responses = await sessionClient.detectIntent(request);
    const result = responses[0].queryResult;
    return result.fulfillmentText; // Return Dialogflow's response
}

// Chatbot API Route
app.post("/chat", async (req, res) => {
    const userMessage = req.body.message;
    const sessionId = req.body.sessionId || "12345";

    if (!userMessage) {
        return res.status(400).json({ error: "Message is required" });
    }

    try {
        const botResponse = await processMessage(userMessage, sessionId);
        res.json({ reply: botResponse });
    } catch (error) {
        console.error("Error processing message:", error);
        res.status(500).json({ error: "Internal Server Error" });
    }
});

// Start the Server
app.listen(port, () => {
    console.log(`Food Delivery Chatbot is running on http://localhost:${port}`);
});
