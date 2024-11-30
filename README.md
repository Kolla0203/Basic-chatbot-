# Basic-chatbot-
import random
import nltk
import spacy
from nltk.chat.util import Chat, reflections

# Download necessary NLTK data files
nltk.download('punkt')
nltk.download('wordnet')

# Load spaCy language model
nlp = spacy.load("en_core_web_sm")

# Sample corpus for NLTK-based chatbot
pairs = [
    ['hi|hello', ['Hello!', 'Hi there!']],
    ['how are you?', ['I am fine, thank you!', 'Doing well, and you?']],
    ['(.*) your name?', ['I am a chatbot created for conversations.']],
    ['(.*) help (.*)', ['I can assist you with general queries. What do you need help with?']],
    ['quit', ['Goodbye! Have a great day!']]
]

# Initialize NLTK Chat
nltk_chatbot = Chat(pairs, reflections)

def spacy_response(user_input):
    """Generate a basic response using spaCy for more contextual understanding."""
    doc = nlp(user_input.lower())
    if "name" in user_input:
        return "I'm a chatbot, but I don't have a personal name!"
    elif "help" in user_input:
        return "Sure, I'd love to help. What do you need assistance with?"
    elif any(token.text in ["bye", "goodbye"] for token in doc):
        return "Goodbye! Take care."
    else:
        return random.choice(["I'm not sure I understand.", "Could you rephrase that?", "Interesting, tell me more!"])

# Main function to handle conversations
def chatbot():
    print("Chatbot: Hi! I'm your chatbot. Type 'quit' to exit.")
    while True:
        user_input = input("You: ")
        if user_input.lower() == 'quit':
            print("Chatbot: Goodbye!")
            break
        elif any(kw in user_input.lower() for kw in ['help', 'name']):
            print(f"Chatbot: {spacy_response(user_input)}")
        else:
            # Use NLTK chatbot for predefined responses, fallback to spaCy for contextual ones.
            nltk_resp = nltk_chatbot.respond(user_input)
            if nltk_resp:
                print(f"Chatbot: {nltk_resp}")
            else:
                print(f"Chatbot: {spacy_response(user_input)}")

if __name__ == "__main__":
    chatbot()
