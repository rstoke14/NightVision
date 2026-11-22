# Forest Neural Reconstruction — Android proof-of-concept

This is a separate Android test app for a **single still-image crop**. It runs the QuickSRNetSmall TensorFlow Lite model locally on the phone CPU, then displays and saves the 3× result. It does not connect to Colab and does not touch the live Godot render loop.

## Why this first test
- The model is listed as BSD-3-Clause on its Hugging Face model page.
- The published model file is about 136 kB and the model card describes a 128×128 RGB input and 3× super-resolution.
- Your Dimensity 6100+ has no published benchmark here, so the app measures your actual phone rather than promising a guessed millisecond figure.

## Build without a PC (GitHub Actions)
1. Create a new GitHub repository on your phone.
2. Upload the contents of this folder to the repository root (not the folder itself).
3. Open the repository's **Actions** tab and enable Actions if asked.
4. Run **Build Android test APK** (or push to `main`).
5. Open the completed workflow run → Artifacts → `Forest-Neural-Test-APK` and download the ZIP. Extract it and install `app-debug.apk`.
6. Open the app, choose a screenshot taken from Andhera Van, then tap **RUN LOCAL NEURAL MODEL**.
7. Save the result to Photos → `ForestNeuralTest`.

## Test protocol
- Use a screenshot with detailed foliage, tree bark, grass, and fog.
- The first build processes the center 128×128 crop only. This is deliberately a model sanity/latency test, not whole-frame enhancement.
- Record the displayed inference time. Run it 3 times and note the second/third runs (first run can include warm-up effects).
- Compare the input crop with the 384×384 output at the same displayed size. Check whether leaves, bark, and fog look more convincing or merely over-sharpened.

## Important limitations
- CPU-only first pass for broad Android compatibility; no GPU/NPU delegate is enabled yet.
- This app measures one inference, not full game FPS. It does not prove real-time viability.
- Model input/output tensor shape is checked at startup; if the upstream model file changes shape, the app disables inference instead of silently producing a wrong result.
- Not yet tested on the user's Poco M6 5G or built in this environment. The GitHub Actions workflow is supplied to produce the APK.

## Model and license
QuickSRNetSmall model card and file: https://huggingface.co/qualcomm/QuickSRNetSmall
Model file pinned to revision `977fb3092a065d512cd587c210cc1341b28b7161`.
Model license shown by the repository: BSD 3-Clause. The TensorFlow Lite runtime is a separate dependency; see its license at https://github.com/tensorflow/tensorflow.
