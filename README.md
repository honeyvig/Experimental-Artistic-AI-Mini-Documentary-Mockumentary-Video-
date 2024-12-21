# Experimental-Artistic-AI-Mini-Documentary-Mockumentary-Video
I would like to make a 2-3 minute mini-’mockumentary’ to come across like a real documentary, which is meant to be an artistic introduction (and possibly viral video clip) to a digital fine art project (a large mural divided into many hundred pieces, to be explained another time), but with a touch of the uncanny through AI generation of the main character who is introducing the work. The video involves an artist introducing their work.



(If you would delight in gently poking fun at a video like this ( https://vimeo.com/923765084 ) by reproducing it with a weird AI-driven person in a school mascot/costume talking in sweeping tones about their important work (maybe looking like this— https://teachtap.learnwith.ai/ —but from their fancy art studio, this is a job for you!)



The ‘assets’ we would have to work with are (1) digital images of the artwork we’re making a documentary for, (2) some limited real footage of the actual making of that art in real life, (3) AI driven content of a character introducing the art, ideally with all of it blended to feel consistently ‘weird’ as the best/worst AI videos tend to look and possibly (4) other ‘staged’ videos produced to meet the needs of the project.



This does not need to be a 4K-8K looking project. There’s a bit of joy for the mixed mess and lo fi nature of what a possible result would be. Clarity-Uncanniness-Artisticness should all be there (with “is this for real” dry humor infusing it all) and not sacrifice one for the other, but Sexiness-Marketability-Coolness is NOT important.



Presumably working with a tool like Runway.

-------------------
To create a mini-mockumentary with AI-driven weirdness and a touch of humor for your digital fine art project, we can break the process down into distinct steps, using Python tools for video editing and AI generation, while incorporating a mix of real footage and digital art. Here's how we could approach the project:
Steps and Python Code Structure:

    Generate the AI Character (Main Subject):
        We will use an AI-generated character in a school mascot costume (with some weird, uncanny features). This can be done using image and video synthesis models like RunwayML, DeepArt, or DALL·E for creating the character visuals.
        Use OpenAI's GPT or similar models to generate a script that the AI character will read, making it sound over-the-top but dryly humorous.

    AI Narration and Speech Synthesis:
        Text-to-Speech (TTS) can be used to voice the AI character's monologue. We can use Google Cloud Text-to-Speech or pyttsx3 for generating speech.
        The script can involve the AI character talking about the artwork and its "importance," in a dramatic and pretentious tone, similar to the tone used in the video you shared.

    Combine the Art and Footage:
        Mix the real footage (of the actual mural creation process) with digital images of the artwork and AI-generated character video.
        Use OpenCV or a similar video processing library to stitch everything together into one cohesive (yet strange) video.

    Post-Processing with Visual Effects:
        Apply some distorted visuals and filters to make the video look lo-fi and surreal.
        Using moviepy and PIL (Pillow), we can overlay text, adjust colors, add special effects, and synchronize visuals to the narrative.

    Final Touches (Uncanny Effects):
        To enhance the uncanniness, slight visual glitches or facial distortions could be added using video editing techniques.

Python Code Example for a Mockumentary Video

We'll focus on parts like creating the script, generating speech, stitching video, and applying effects. Here's a conceptual example of how to structure your code.
Step 1: AI Character Script Generation (with GPT)

Using OpenAI's GPT-3, generate an exaggerated introduction by the AI character.

import openai

openai.api_key = 'your-openai-api-key'

def generate_script():
    prompt = """
    Imagine you are an AI character in a school mascot costume. You are introducing a highly sophisticated and deeply important digital fine art project to a curious public. Your tone should be pretentious, dramatic, and over-the-top. Here is your script:

    'Greetings, artistic souls. I, AI, the supreme guide of creativity, humbly present to you my finest creation...'

    Finish the script and make it sound like a serious art documentary introduction, with plenty of grandiose and artistic expressions.

    """
    
    response = openai.Completion.create(
      model="text-davinci-003",
      prompt=prompt,
      temperature=0.7,
      max_tokens=300
    )
    
    return response.choices[0].text.strip()

# Generate and print the script
script = generate_script()
print(script)

Step 2: Text-to-Speech (TTS) for the AI Character

Once we have the script, we can convert it to speech using pyttsx3 (or Google Cloud Text-to-Speech if you prefer a more natural-sounding voice).

import pyttsx3

def generate_speech(text, filename="ai_narration.mp3"):
    engine = pyttsx3.init()
    engine.save_to_file(text, filename)
    engine.runAndWait()

# Generate speech for the AI character's script
generate_speech(script)

Step 3: Video Stitching with Real Footage and AI-Generated Content

Now, we combine the real footage (of the mural creation) and AI-generated video content. For this, we will use moviepy and OpenCV.

from moviepy.editor import *
import cv2

# Load video clips: 1) AI character footage, 2) Real footage of mural creation
ai_video = VideoFileClip("ai_character_video.mp4")
real_footage = VideoFileClip("mural_creation_footage.mp4")

# Load the AI narration audio (generated from the script)
narration_audio = AudioFileClip("ai_narration.mp3")

# Adjust the duration of the footage to match the narration
ai_video = ai_video.set_duration(narration_audio.duration)
real_footage = real_footage.set_duration(narration_audio.duration)

# Combine the footage: Overlay AI video on top of the real footage
final_video = CompositeVideoClip([real_footage, ai_video.set_position(("center", "bottom"))])

# Set audio to be the AI narration
final_video = final_video.set_audio(narration_audio)

# Apply a vintage filter to give it a lo-fi look
final_video = final_video.fx(vfx.colorx, 0.8)  # Adjust the colors slightly to make it feel old and surreal

# Export the final video
final_video.write_videofile("mockumentary.mp4", codec="libx264", audio_codec="aac")

Step 4: Add Distorted Visuals or Uncanny Effects (Optional)

To make the AI character feel more unsettling, you can apply some basic video distortions.

from moviepy.editor import vfx

# Apply a subtle glitch effect to make the AI character video look uncanny
def apply_glitch_effect(clip):
    return clip.fx(vfx.time_mirror)  # Mirror the video to create a glitch effect

# Apply the glitch effect to the AI character video
ai_video_with_glitch = apply_glitch_effect(ai_video)

# Combine again with the rest of the footage
final_video_with_glitch = CompositeVideoClip([real_footage, ai_video_with_glitch.set_position(("center", "bottom"))])
final_video_with_glitch = final_video_with_glitch.set_audio(narration_audio)

# Export the final distorted version
final_video_with_glitch.write_videofile("mockumentary_with_glitch.mp4", codec="libx264", audio_codec="aac")

Final Notes:

    Script Generation: The script should reflect the comedic, over-the-top tone of the AI character, mixing self-importance with awkwardness.
    AI Character Generation: You could use RunwayML or other platforms to generate AI-driven characters that look slightly off, adding to the uncanny feeling.
    Voice Generation: TTS engines can make the AI character’s voice sound either robotic or natural, depending on the aesthetic you want.
    Video Effects: The video’s surreal, low-fi look can be enhanced with color filters, glitch effects, and frame distortions.

The final mockumentary will blend real footage, AI generation, and artistic visuals to create a weird and engaging video.
