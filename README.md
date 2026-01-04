AI Automatic Meme Generator
Automatically Turn Google Photos into Shareable Memes with AI-Generated Captions

Project Overview
This project develops a fully automated system that utilizes artificial intelligence technology to transform personal photos stored in Google Drive (originally designed for Google Photos) into humorous meme images, automatically generating relevant captions, and ultimately publishing them to the Instagram social platform.
The core concept is to convert everyday embarrassing, failed, or amusing moments (such as tripping while hiking or a pet staring at an empty bowl) into relatable social media content with Gen-Z humor style, achieving the entertaining effect of “Turn embarrassing moments into comedy gold.”

System Architecture
* Containerized Deployment: Using Docker to run n8n and Ollama simultaneously
* Workflow Automation: n8n (core engine responsible for scheduling, API integration, and process control)
* Local Artificial Intelligence Models: Ollama
    * Vision Model: llava:7b or similar lightweight model, used for image scene analysis and meme potential scoring
    * Language Model (LLM): mistral:7b, used for generating and selecting humorous captions
* Image Processing: Python + Pillow (PIL) library, responsible for text overlay and format optimization
* External Services:
    * Google Drive API (photo retrieval)
    * Instagram Graph API (automatic publishing)
    * Google Sheets (logging publication records, optional)
Main Features
1. Daily Scheduled Trigger (9:00 AM)
2. Automatic Selection of Recent Photos: Randomly select several photos from the last 7 days
3. Image Visual Analysis: Identify scenes, expressions, actions, and potential humorous elements; calculate “meme potential score”
4. Humorous Caption Generation: Produce multiple caption options and select the best one (Gen-Z style)
5. Meme Image Creation:
    * Center crop to square
    * Resize to 1080×1080 pixels
    * Bottom-centered large text with white letters and black outline (classic meme style)
    * High-quality JPEG output compliant with Instagram requirements
6. Automatic Publishing to Instagram and logging relevant data to Google Sheets

Technical Highlights and Solutions
* n8n Loop Does Not Pass Binary Data: Use the Set Fields node before the Loop to convert image binary to a base64 string and place it in a JSON field for transmission, ensuring stable access to the original image within the Loop
* Ollama Vision Model Crashes Due to Insufficient Resources: Switch to the lighter llava:7b model and limit batch size to 1
* Instagram Publishing Shows Pure White Images: Enforce center cropping, fixed 1080×1080 size, and optimized JPEG parameters (quality=95, optimize, progressive)
* High-Resolution Original Image Processing: Add center cropping logic to prevent stretching distortion

Advanced AI Enhancement: Model Fine-tuning (Step 5)
Why Fine-tuning?
While the base Mistral-7B model can generate generic captions, it often lacks the specific humor style and cultural relevance needed for viral memes. Our fine-tuning process transforms it into a specialized meme caption expert.

Training Process
(https://colab.research.google.com/drive/1HjSDF11PEHuvUY_CFtQN6QRuzX9NDqxK?usp=sharing)

Technical Implementation:
Method: LoRA (Low-Rank Adaptation) - trains only 0.1% of parameters
Base Model: Mistral-7B-Instruct-v0.1
Training Data: 50 curated meme samples (image descriptions + humorous captions)
Platform: Google Colab with free T4 GPU
Training Time: 2 hours
Model Size: ~4GB (4-bit quantized)


Environment Requirements
* Docker and Docker Compose
* Host machine recommended to have at least 16 GB RAM (required for running Ollama Vision models)
* Ollama Models:
    * ollama pull llava:7b
    * ollama pull mistral:7b
* Google Drive API credentials
* Instagram Graph API (requires Business or Creator account and long-lived Access Token)

Installation and Running Steps
1. Clone this repository
2. Start containers:Bash  docker compose up -d 
3. Access the n8n interface
4. Import the provided workflow.json
5. Configure the following credentials:
    * Google Drive OAuth
    * Instagram Graph API Access Token
    * (Optional) Google Sheets
6. Execute manually for testing or wait for automatic execution at 9:00 AM daily

Demo video
https://youtu.be/BnLEU5WHFsA

Major Challenges Encountered and Solutions


Challenge	Solution
n8n Loop does not pass binary data	Use Set Fields to convert binary to base64 and pass via JSON
Frequent 500 errors in Ollama Vision model	Switch to lighter model and limit concurrent image processing
Pure white or failed images on Instagram	Enforce 1080×1080, square cropping, and optimized JPEG parameters
Distortion or oversized files from high-resolution originals	Implement center cropping and resize logic

Future Improvements
* Introduce Impact font for more classic meme style
* Support simultaneous publishing to multiple platforms (Twitter, TikTok)
* Fine-tune model using LoRA on Reddit r/memes dataset to enhance caption humor
* Add manual review step to prevent inappropriate content
* Implement engagement tracking and optimal posting time analysis

