This project implements a multi-stage CAPTCHA generation and evaluation pipeline that dynamically increases difficulty based on the ability of humans, AI (TrOCR), and large vision-language models (Gemini) to solve distorted CAPTCHAs. The system escalates distortion using CycleGAN, FGSM adversarial attacks, and salt-and-pepper noise, adjusting difficulty based on solver performance.

🧠 Key Features
Dynamic CAPTCHA distortion using:

CycleGAN (image-to-image stylized noise)

FGSM (adversarial perturbations)

Salt-and-pepper noise (random noise corruption)

Multi-agent CAPTCHA evaluation:

AI solver using a fine-tuned TrOCR model

Google Gemini vision-language model

Human input

Escalation policy:

Escalates distortion only if all solvers correctly predict the CAPTCHA text

PPO-based reinforcement learning agent (optional) to recommend optimal difficulty levels

🔧 Requirements
Install dependencies:
pip install torch torchvision transformers google-generativeai matplotlib numpy Pillow stable-baselines3

 How It Works
A random CAPTCHA image is selected from the dataset.

Stage 1: Apply CycleGAN distortion.

All solvers (TrOCR, Gemini, and Human) attempt to decode the CAPTCHA.

If all succeed → escalate to Stage 2 (FGSM adversarial noise).

If all still succeed → escalate to Stage 3 (salt & pepper noise).

If any solver fails at any stage, stop and record the distortion level.

(Optional) PPO agent suggests distortion policy using observed outcomes.

 Noise Functions
CycleGAN: Stylized visual distortion using pretrained generator.

FGSM: Gradient-based perturbation to fool neural networks.

Salt & Pepper: Random pixel noise simulating degraded image quality.

📈 Metrics & Evaluation
Sequence similarity is used to compare predictions to ground truth.

A success threshold (default: ≥ 0.5 similarity) determines prediction correctness.

Aggregate distortion levels are logged to track how difficult CAPTCHAs can get before solvers fail.

🤖 PPO Agent (Optional)
A reinforcement learning agent (Stable Baselines3 PPO) observes (AI_success, human_success, current_distortion) and chooses an action: [decrease, maintain, increase distortion]. This allows adaptive tuning based on real-time performance feedback.
