# Sign Language Recognition

A real-time American Sign Language (ASL) recognition system that uses **MediaPipe hand tracking** and an **LSTM sequence model** to recognize hand signs from a webcam feed. The system continuously processes hand movements and predicts one of six supported signs, achieving **84.72% test accuracy** in the reported evaluation.

## Overview

Communication barriers can make everyday interactions challenging for people who use sign language. This project explores a computer vision and deep learning approach to recognizing hand signs from live video.

The system uses MediaPipe to extract hand landmarks from webcam frames and an LSTM model to analyze landmark sequences for sign classification. Unlike a purely static image classifier, the sequence-based approach is designed to capture temporal information in hand movements.

The application displays predictions through an OpenCV window and updates them continuously as new frames are processed.

## Key Features

- **Real-time webcam recognition:** Processes live video input.
- **Hand landmark tracking:** Uses MediaPipe to track hand movements and extract landmark information.
- **Sequence-based classification:** Uses an LSTM model to learn temporal patterns in hand movements.
- **Six supported signs:** Recognizes the six classes used during training.
- **Continuous predictions:** Updates the predicted sign as new frames and sequence information become available.
- **Model training and inference:** Includes code for training the model and saved model artifacts for inference.
- **Multiclass evaluation:** Evaluates classification performance using test accuracy and a multiclass confusion matrix.

## Supported Signs

The model is trained to recognize the following six classes:

| Class | Sign label |
|---|---|
| 1 | `GoodBye` |
| 2 | `Hello` |
| 3 | `ILoveYou` |
| 4 | `No` |
| 5 | `ThankYou` |
| 6 | `Yes` |

**Scope:** This is a limited-vocabulary recognition system, not a complete ASL translator. It does not claim to interpret unrestricted conversations, grammar, facial expressions, or every component of American Sign Language.

## System Architecture

The recognition pipeline consists of the following stages:

1. **Video capture:** OpenCV captures frames from the webcam.
2. **Hand tracking:** MediaPipe detects and tracks the hand in each frame.
3. **Landmark extraction:** Hand landmark information is extracted from the detected hand.
4. **Sequence preparation:** Landmark information is organized into a temporal sequence for the LSTM model.
5. **Sign classification:** The trained LSTM processes the sequence and predicts a sign class.
6. **Prediction display:** The predicted class is displayed through the OpenCV application.

```text
             Webcam Input
                   |
                   v
          OpenCV Frame Capture
                   |
                   v
        MediaPipe Hand Tracking
                   |
                   v
          Landmark Extraction
                   |
                   v
          Temporal Sequences
                   |
                   v
             LSTM Model
                   |
                   v
       Six-Class Sign Prediction
                   |
                   v
        OpenCV Prediction Display
```

The exact landmark feature representation and sequence configuration depend on the implementation.

## Dataset

The dataset was collected using a webcam and contains recordings for six sign classes.

| Dataset characteristic | Value |
|---|---:|
| Number of classes | 6 |
| Recordings per class | 30 |
| Frames per recording | 30 |
| Total recordings | 180 |
| Nominal frame instances | 5,400 |

Each recording contains 30 frames of a sign being performed.

The nominal total of 5,400 frame instances is calculated as:

`6 classes × 30 recordings × 30 frames = 5,400 frame instances`

These frames are grouped into recordings and are not necessarily independent training examples.

### Dataset Considerations

- The data was collected using a custom recording process.
- The model's generalization depends on the variety of hand movements, recording conditions, and people represented in the data.
- A separate test set is needed to assess performance on data not used for model training.
- The exact train-validation-test split and the number of sequences in each split have not been documented here.

## Model and Technologies

### LSTM

Long Short-Term Memory (LSTM) networks are a type of recurrent neural network designed to learn temporal dependencies in sequential data.

In this project, the LSTM is used to classify sequences of hand landmark information. This makes it suitable for modeling changes in hand position over successive video frames.

### MediaPipe

MediaPipe provides hand tracking and landmark information from video frames. These landmarks provide a structured representation of hand movement for downstream processing.

### OpenCV

OpenCV handles webcam input and the application's visual prediction interface.

### Technology Stack

| Technology | Purpose |
|---|---|
| Python | Main programming language |
| MediaPipe | Hand tracking and landmark extraction |
| OpenCV | Webcam capture and prediction display |
| TensorFlow/Keras | Model training and inference |
| NumPy | Numerical and array operations |
| LSTM | Temporal sequence classification |

## Results

The model was evaluated using test accuracy and a multiclass confusion matrix.

| Metric | Result |
|---|---:|
| Test accuracy | **84.72%** |
| Evaluation method | Multiclass confusion matrix |
| Number of classes | 6 |

The reported test accuracy is approximately 84.72%, calculated from the supplied accuracy value of `0.8472222222`.

### Interpreting the Results

Test accuracy measures the proportion of test predictions that match their ground-truth labels.

A multiclass confusion matrix provides a more detailed view of classification performance by showing which signs are correctly recognized and which signs are confused with one another.

The numerical confusion matrix and per-class precision, recall, and F1-scores are not included in this documentation. They can be added when the evaluation output is available.

**Early prediction:** The system is designed to update predictions continuously as hand movement sequences are processed. A separate quantitative evaluation of prediction accuracy at different stages of gesture completion has not been reported, so early-recognition performance should not be inferred from the overall test accuracy.

## Installation

### Prerequisites

Before running the project, make sure you have:

- Python installed.
- A working webcam.
- A compatible environment for MediaPipe, OpenCV, and TensorFlow/Keras.
- The repository's source code and saved model artifacts.

The exact Python version and tested dependency versions have not been specified. Use versions compatible with the project's implementation.

### 1. Clone the Repository

```bash
git clone https://github.com/Punyam22/Sign-Language-Recognition.git
cd Sign-Language-Recognition
```

### 2. Create a Virtual Environment

```bash
python -m venv .venv
```

Activate the environment before installing dependencies.

**Windows — Command Prompt:**

```bat
.venv\Scripts\activate.bat
```

**Windows — PowerShell:**

```powershell
.\.venv\Scripts\Activate.ps1
```

**macOS/Linux:**

```bash
source .venv/bin/activate
```

### 3. Install Dependencies

If the repository contains a `requirements.txt` file, install the pinned dependencies with:

```bash
python -m pip install -r requirements.txt
```

If no dependency file is present, the expected core packages can be installed with:

```bash
python -m pip install mediapipe opencv-python tensorflow numpy
```

Package compatibility may require specific versions. If installation fails, check the Python version, the model's framework requirements, and the dependency versions used during development.

## Running the Application

The project includes model training code and saved model artifacts. For normal recognition, the intended workflow is to load the trained model and run the webcam inference script.

The exact script filenames and launch command have not been confirmed. To identify the correct entry point:

1. Open the repository and inspect its Python files.
2. Identify the script that initializes the webcam and MediaPipe hand tracking.
3. Confirm which saved model file the inference script loads.
4. Run the inference script using the Python environment configured above.

For example, once you have identified the correct entry-point filename:

```bash
python <inference_script>.py
```

Replace `<inference_script>.py` with the actual filename. This is a placeholder, not a verified command for the repository.

If you want to retrain the model, locate the training script and follow the dataset paths and preprocessing requirements defined in that implementation.

## Usage

Once the correct inference script is running:

1. Allow the application to access your webcam.
2. Position your hand within the camera's field of view.
3. Perform one of the six supported signs.
4. Observe the predicted class in the OpenCV window.
5. Continue performing signs to observe how the predictions change over time.

For more consistent results, use adequate lighting, keep your hand visible, and avoid abrupt changes in camera position.

The exact interaction controls, prediction thresholds, and application shutdown behavior depend on the implementation.

## Limitations

This project is an experimental sign-recognition system with a restricted vocabulary. Its results should be interpreted within that scope.

- **Limited vocabulary:** The model recognizes only the six trained sign classes.
- **Lighting sensitivity:** Changes in lighting can affect hand detection and landmark quality.
- **Background and camera conditions:** Background clutter, camera angle, and hand visibility may affect tracking.
- **Prediction delay:** Sequence processing may introduce latency before a stable prediction is displayed.
- **Misclassification:** Similar hand movements or imperfect landmark tracking can cause incorrect predictions.
- **Dataset size:** The custom dataset contains 30 recordings per class, which limits the diversity of examples available for learning.
- **Early-recognition evaluation:** The overall test accuracy does not independently measure how early a sign can be identified.
- **Generalization:** Performance with unfamiliar users, different recording environments, or variations in sign execution requires further evaluation.

The system should not be treated as a replacement for a fluent sign-language interpreter or as a complete accessibility solution.

## Future Improvements

Potential next steps include:

- Expand the dataset with more signers, recording conditions, and examples per class.
- Add per-class precision, recall, F1-scores, and a visualized confusion matrix.
- Measure inference latency and frames per second.
- Evaluate prediction accuracy at different stages of sign completion.
- Improve temporal smoothing and prediction stability.
- Investigate confidence thresholds to reduce unstable predictions.
- Extend the vocabulary and evaluate generalization to previously unseen recording conditions.
- Add screenshots or a short demonstration GIF to document the running application.

## Repository

Source code and project files:

[Sign Language Recognition — GitHub](https://github.com/Punyam22/Sign-Language-Recognition)

## License

No license information has been specified in this README. Add a `LICENSE` file if you intend to define how others may use, modify, and distribute the project.

## Acknowledgements

- [MediaPipe](https://ai.google.dev/edge/mediapipe/solutions/guide) for hand tracking and landmark extraction.
- [OpenCV](https://opencv.org/) for computer vision and webcam processing.
- [TensorFlow](https://www.tensorflow.org/) for deep learning and model development.

---

Developed as a computer vision and deep learning portfolio project exploring real-time, sequence-based sign recognition.
