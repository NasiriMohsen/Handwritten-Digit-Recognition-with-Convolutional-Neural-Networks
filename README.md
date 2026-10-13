# Handwritten Digit Recognition with Convolutional Neural Networks

**IU International University of Applied Sciences — Project: Computer Vision (DLBAIPCV01), Task 1**

This project implements and compares three convolutional neural networks (CNNs) for handwritten digit recognition using the MNIST dataset. The main objective is to approximate the historical CNN described by LeCun et al. (1989), compare it with Chollet's Keras MNIST example, and investigate whether the historical approximation can be improved through tuning while keeping the model compact.

The project also includes a small practical demonstration: detecting individual handwritten digits in photographs with OpenCV and classifying them with the trained networks.

## Models

| Model | Description | Test accuracy | Test loss | Trainable parameters |
| --- | --- | ---: | ---: | ---: |
| **Chollet** | Modern Keras baseline with convolution, max pooling and dropout | **99.17%** | 0.02596 | 34,826 |
| **LeCun** | Historical approximation using 5 × 5 kernels, tanh activations, average pooling and a compact feature-map layout | **96.50%** | 0.10544 | 3,246 |
| **LeCun Tuned** | Adaptation selected through ten manually specified tuning trials | **99.09%** | 0.02842 | 8,882 |

The tuned model approaches the Chollet baseline's test accuracy with approximately **74.5% fewer trainable parameters**. The results are based on one fixed dataset split and training seed; they should not be interpreted as performance guarantees on other handwriting datasets.

## Dataset and experimental setup

- **Dataset:** MNIST, ten classes (digits 0–9), 28 × 28 grayscale images.
- **Training:** 54,000 images from the official MNIST training portion.
- **Validation:** Remaining 6,000 images from the same portion.
- **Testing:** Official 10,000-image MNIST test set.
- **Random seed:** 42 in the recorded experiments.
- **Preprocessing:** The Chollet baseline uses pixel values scaled to 0–1. The historical approximation resizes the active digit to 16 × 16, pads it to 28 × 28 and scales to −1–1. The tuned model uses the original 28 × 28 resolution and the −1–1 scale.

The tuned configuration uses 8 and 24 convolutional feature maps, tanh activations, average pooling, spatial dropout and small random translations during training. Tuning configurations were ranked by validation loss, not by test-set performance.

## Repository contents

The main files and output folders are organised as follows (paths may differ if files are moved within the repository):

```text
.
├── Digit.ipynb                 # Main notebook: models, tuning, evaluation and image recognition
├── requirements.txt            # Python package version ranges
├── results/                    # Saved model outputs (if included in the repository)
│   ├── comparison.csv
│   ├── chollet/
│   ├── lecun/
│   ├── lecun_tune/
│   ├── mnist_examples.png
│   └── recognition/
└── README.md
```

The supplied experiment archive places these saved outputs inside a directory named `full/`. If you use that archive without renaming its directory, update the notebook's `RUN_DIR` or arrange the folder as the notebook expects. Model folders contain `.keras` weights, training histories, confusion matrices and misclassification examples. The `lecun_tune/tuning/` subfolders retain the tuning trials and recorded configurations.

**Note:** `Digit.ipynb` is the intended notebook filename used in this project's workflow; include your actual notebook in the repository. The saved-results archive alone does not contain the notebook.

## Installation

The project was developed in Python using TensorFlow/Keras, NumPy, pandas, Matplotlib and OpenCV. **Python 3.12** is a suitable starting point for the project environment; the supplied dependency file specifies TensorFlow `>=2.16,<2.22` rather than an exact tested version.

Create and activate a virtual environment:

```bash
python -m venv .venv
```

**Windows (PowerShell):**

```powershell
.\.venv\Scripts\Activate.ps1
```

**macOS / Linux:**

```bash
source .venv/bin/activate
```

Install the listed requirements and the OpenCV package used by the digit-detection code:

```bash
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
python -m pip install opencv-python jupyter
```

> The provided `requirements.txt` does not explicitly include `opencv-python` or the `jupyter` command-line package. They are installed separately above. NumPy is normally installed as a dependency of TensorFlow and pandas.

## Running the project

1. Open the notebook (`Digit.ipynb`) in Jupyter or VS Code using the project's Python environment.
2. Check the paths under the notebook's **Variables** section, especially `ROOT`, `RUN_NAME`, `RUN_DIR` and the input photograph paths.
3. To **reuse saved models and results**, set `TRAIN_NEW = False` and ensure the corresponding `.keras` files and result CSV files are in the configured output location.
4. Execute the notebook cells in order to display comparisons, learning curves, confusion matrices and image-recognition results.
5. To train the baseline models again, set `TRAIN_NEW = True`. Use a different `RUN_NAME` to avoid overwriting saved experiments. For a new tuning search, also select a new `TUNE_NAME`.

The notebook stores evaluation and plot outputs for later inspection. Retraining times depend on the machine and workload and may differ from the archived measurements.

## Handwritten photograph recognition

The optional image-recognition section uses OpenCV to identify digit regions in two sample photographs, centre them on 28 × 28 canvases, and apply each trained classifier using its own preprocessing method.

In the recorded demonstration, all three models produced the same digit sequences for the two test photographs: **13540** and **05221859**. These two examples demonstrate the workflow but are not a broad evaluation of real-world handwriting recognition.

## Reports and results

The project report presents the architectural decisions, experimental configuration, comparisons and limitations. Detailed results are retained in the notebook and saved outputs, including:

- `comparison.csv` — model test losses, accuracies, parameters and archived timing records;
- `lecun_tune/trials.csv` — tuning trial results and validation ranking;
- `*_learning.png` — training/validation learning curves;
- `*_confusion.png` and `*_confusion.csv` — confusion matrices;
- `*_errors.png` — examples of incorrect predictions;
- `recognition/predictions.csv` — photograph recognition outputs and confidence values.

The archived model-fitting times and times shown in an independently executed notebook export may differ. Use the accompanying recorded outputs when interpreting timings from a particular run.

## References

- Chollet, F. (2020). *Simple MNIST convnet*. Keras. https://keras.io/examples/vision/mnist_convnet/
- LeCun, Y., Boser, B., Denker, J. S., Henderson, D., Howard, R. E., Hubbard, W., & Jackel, L. D. (1989). *Handwritten digit recognition with a back-propagation network*. *Advances in Neural Information Processing Systems*, 2, 396–404. https://proceedings.neurips.cc/paper_files/paper/1989/file/53c3bce66e43be4f209556518c2fcb54-Paper.pdf
- LeCun, Y., Cortes, C., & Burges, C. J. C. (1998). *The MNIST database of handwritten digits*. http://yann.lecun.com/exdb/mnist/

## Author

**Mohsen Nasiri**  
B.Sc. Applied Artificial Intelligence  
IU International University of Applied Sciences
