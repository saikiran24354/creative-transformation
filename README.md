# Function to recognize speech and convert it to text
def recognize_speech():
    recognizer = sr.Recognizer()
    with sr.Microphone() as source:
        st.write("Please speak...")
        audio = recognizer.listen(source)
    try:
        text = recognizer.recognize_google(audio)
        st.write(f"Text recognized: {text}")
        return text
    except sr.UnknownValueError:
        st.write("Sorry, I could not understand the audio.")
        return None
    except sr.RequestError:
        st.write("Sorry, there was a problem with the speech recognition service.")
        return None

# Load Hugging Face text-to-image pipeline (DALL-E or Stable Diffusion)
@st.cache_resource
def load_model():
    generator = pipeline("text-to-image", model="stabilityai/stable-diffusion-2-1")
    return generator

# Streamlit app layout
def main():
    st.title("Voice to Image Generator")
    st.write("This app converts your voice prompts into visual creations.")

    # Button to start voice recognition
    if st.button("Start Voice Input"):
        # Capture the voice input and convert it to text
        prompt = recognize_speech()

        if prompt:
# Load the model (you can also use a different model from Hugging Face)
            generator = load_model()

            # Generate the image from the prompt
            st.write(f"Generating image for prompt: {prompt}")
            image = generator(prompt)[0]['image']
            
            # Display the generated image
            st.image(image, caption="Generated Image", use_column_width=True)

if _name_ == "_main_":
    main()
