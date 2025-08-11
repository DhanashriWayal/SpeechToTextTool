import speech_recognition as sr

# Initialize the recognizer
recognizer = sr.Recognizer()

# Ask the user to choose a language
print("Select a language to speak in:")
print("1. English")
print("2. Hindi")
print("3. Marathi")
choice = input("Enter your choice (1/2/3): ")

# Map the choice to language codes
if choice == "1":
    lang_code = "en-US"
elif choice == "2":
    lang_code = "hi-IN"
elif choice == "3":
    lang_code = "mr-IN"
else:
    print("Invalid choice! Defaulting to English.")
    lang_code = "en-US"

# Capture audio from microphone
with sr.Microphone() as source:
    print(f"\nListening... Speak in the selected language ({lang_code})")
    recognizer.adjust_for_ambient_noise(source)  # optional: reduce background noise
    audio_data = recognizer.listen(source)

    print("Recognizing...")

    try:
        # Recognize speech using Google with selected language
        text = recognizer.recognize_google(audio_data, language=lang_code)
        print(f"\nTranscribed Text:\n{text}")

        # Save the output
        with open("transcription.txt", "w", encoding="utf-8") as f:
            f.write(text)

    except sr.UnknownValueError:
        print("❌ Sorry, could not understand your speech.")
    except sr.RequestError as e:
        print(f"❌ Could not connect to Google Speech Recognition service; {e}")

