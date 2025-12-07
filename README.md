#include <stdio.h>
#include <string.h>
#include <ctype.h>
#include <time.h>
#include <stdbool.h>

#define MAX_INPUT 256
#define MAX_RESPONSE 512

// Structure to store keyword-response pairs
typedef struct {
    const char *keyword;
    const char *response;
} KnowledgePair;

// Simple knowledge base
KnowledgePair knowledgeBase[] = {
    { "hello",    "Hello! Nice to meet you. How can I help you today?" },
    { "hi",       "Hi there! Ask me anything about college, exams or C programming." },
    { "hey",      "Hey! I am a simple C chatbot created by a B.Tech student." },
    { "name",     "I am a console chatbot written in C. You can call me C-Bot." },
    { "college",  "You mentioned college. Are you asking about life in college, exams, or courses?" },
    { "course",   "B.Tech in CSE is a great choice! Do you have any questions about subjects or labs?" },
    { "exam",     "Exams can be stressful. Try making a study plan and practice previous year questions." },
    { "github",   "GitHub is a platform to host code. You can upload this chatbot project there easily." },
    { "c language", "C is a powerful language. Practice pointers, arrays, and functions regularly." },
    { "thank",    "You're welcome! Happy to help." },
    { "thanks",   "You're welcome! Do you want to ask something else?" },
    { "sorry",    "No problem, we all make mistakes while learning. :)" }
};

int knowledgeBaseSize = sizeof(knowledgeBase) / sizeof(knowledgeBase[0]);

// Remove trailing newline from fgets
void trimNewline(char *str) {
    size_t len = strlen(str);
    if (len > 0 && str[len - 1] == '\n') {
        str[len - 1] = '\0';
    }
}

// Convert string to lower case
void toLowerCase(const char *src, char *dst) {
    while (*src) {
        *dst = (char)tolower((unsigned char)*src);
        src++;
        dst++;
    }
    *dst = '\0';
}

// Check if 'keyword' occurs in 'input' (case-insensitive)
bool containsKeyword(const char *input, const char *keyword) {
    char inputLower[MAX_INPUT];
    char keywordLower[MAX_INPUT];

    toLowerCase(input, inputLower);
    toLowerCase(keyword, keywordLower);

    return strstr(inputLower, keywordLower) != NULL;
}

// Add current time to response
void getTimeString(char *buffer, size_t size) {
    time_t now = time(NULL);
    struct tm *t = localtime(&now);
    if (t != NULL) {
        strftime(buffer, size, "%H:%M:%S", t);
    } else {
        snprintf(buffer, size, "unknown time");
    }
}

// Generate chatbot response
// Returns false if the user wants to exit (bye/exit/quit)
bool generateResponse(const char *userInput, char *response, size_t responseSize) {
    char lowerInput[MAX_INPUT];
    toLowerCase(userInput, lowerInput);

    // If user wants to quit
    if (strstr(lowerInput, "bye") != NULL ||
        strstr(lowerInput, "exit") != NULL ||
        strstr(lowerInput, "quit") != NULL) {

        snprintf(response, responseSize,
                 "It was nice talking to you. Goodbye! 👋");
        return false; // stop chatting
    }

    // If user asks for time
    if (strstr(lowerInput, "time") != NULL) {
        char timeStr[64];
        getTimeString(timeStr, sizeof(timeStr));
        snprintf(response, responseSize,
                 "Current local time is: %s\n(Using C library <time.h>)", timeStr);
        return true;
    }

    // Try to match keywords from knowledge base
    for (int i = 0; i < knowledgeBaseSize; i++) {
        if (containsKeyword(userInput, knowledgeBase[i].keyword)) {
            snprintf(response, responseSize, "%s", knowledgeBase[i].response);
            return true;
        }
    }

    // Default / fallback responses if no keyword matched
    snprintf(response, responseSize,
             "I'm not sure I understand. I'm just a simple C chatbot.\n"
             "Try asking about: college, exam, course, C language, or time.");
    return true;
}

int main(void) {
    char userInput[MAX_INPUT];
    char botResponse[MAX_RESPONSE];

    printf("=============================================\n");
    printf("         Simple C Chatbot - C-Bot\n");
    printf("           Created by a B.Tech student\n");
    printf("=============================================\n");
    printf("Type 'bye', 'exit' or 'quit' to end the chat.\n\n");

    while (1) {
        printf("You: ");
        if (fgets(userInput, sizeof(userInput), stdin) == NULL) {
            // End if input fails (e.g., Ctrl+D)
            printf("\nInput ended.\n");
            break;
        }

        trimNewline(userInput);

        // Ignore empty input
        if (strlen(userInput) == 0) {
            printf("C-Bot: Please type something. :)\n");
            continue;
        }

        bool keepChatting = generateResponse(userInput, botResponse, sizeof(botResponse));

        printf("C-Bot: %s\n\n", botResponse);

        if (!keepChatting) {
            break;
        }
    }

    printf("Chat session ended.\n");
    return 0;
}# BOBBARI-VINAY-KUMAR-
I Have written a Code using C language for chatbot . It's a small type of conversation ann it tells about wheather condition ...
