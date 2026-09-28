# Applied Machine Learning: NTU Singapore, Summer 2026

Three reports on machine learning for gastro-oesophageal reflux pH monitoring and medical AI, written for BG4104 (Machine Learning and Optimisation in Bioengineering) during the GEM Trailblazer Summer Programme at Nanyang Technological University, Singapore (June to July 2026). The module was taught on-site by NTU faculty.

## Projects at a glance

| # | Project | Type | Headline result |
|---|---------|------|-----------------|
| 1 | Supervised classification of acid vs non-acid reflux | Individual | k-NN reached ROC-AUC 0.903 against 0.908 for a neural network, while training about 300x faster (34 ms vs 10.6 s) |
| 2 | Supervised and unsupervised learning on reflux pH time series | Team of six | Full-covariance GMM was the best raw clustering (ARI 0.123); PCA kept 95.8% of variance in 4 of 9 dimensions |
| 3 | Critical appraisal of a MICCAI 2025 paper | Individual | 4 limitations identified, 2 testable hypotheses proposed |

## 1. Supervised classification of acid vs non-acid reflux

An IEEE-format study comparing k-nearest neighbours (k-NN), support vector machines (SVM) and a feedforward neural network on ambulatory oesophageal pH data.

- **Data:** two 24-hour recordings, 172,597 valid readings after cleaning (86,197 acid, 86,400 non-acid). Each recording was cut into non-overlapping 60-second windows (2,876 in total) and described with ten features, such as standard deviation, range, and the proportion of readings below pH 4.
- **Method:** stratified 80/20 split (2,300 training and 576 test windows), scaling fitted on training data only, 5-fold cross-validation for all model selection, and the test set used once at the end. Seeds were fixed for reproducibility.
- **Results:** all three models reached test accuracy of 0.81 to 0.83 and ROC-AUC of 0.87 to 0.91. k-NN (k=15) had ROC-AUC 0.903 and trained in 34 ms. The neural network (two-layer ReLU with dropout) had the best recall (0.788) and average precision (0.925) but took 10.6 s to train. The RBF SVM had the highest precision (0.931) and the lowest recall (0.698).
- **Conclusion:** variability and acid-exposure features, not raw pH, carry the signal, and the simplest model stays close to the strongest. k-NN is recommended as the default for real-time, resource-aware monitoring.
- **Extra experiment:** a window-size study (30, 60 and 120 seconds) showed 30-second windows are clearly weaker.

## 2. Supervised and unsupervised learning on reflux pH time series

A six-person IEEE report asking whether the acid and non-acid labels appear as natural structure without supervision. It reuses the supervised benchmark from a teammate's earlier assignment and adds a three-stage unsupervised study on 1,438 windows of 120 seconds.

- **Stage 1, clustering:** K-Means and Gaussian mixture models (GMM) on nine standardised features. The best raw clustering was a two-component full-covariance GMM (ARI 0.1233), but label-free criteria preferred six clusters.
- **Stage 2, dimensionality reduction:** PCA needed four components to keep 95.81% of the variance; ICA gave only one strongly non-Gaussian component; Gaussian random projection was variable and did not preserve distances well.
- **Stage 3, six combinations:** PCA (4D) with K-Means ranked first by ARI (0.0685), only 0.0015 above raw K-Means, and no reduction helped both clusterers.
- **Method:** six hypotheses were recorded before any labels were interpreted, and labels were used only afterwards for ARI, NMI and matched accuracy. Only one hypothesis (GMM outperforming K-Means) was fully supported, and the negative results are reported as they are.
- **Conclusion:** natural structure exists but does not reproduce the two record-level labels, so supervision is better for the stated target while unsupervised methods suit discovery, monitoring and annotation support.

## 3. Critical appraisal of "Learning Segmentation from Radiology Reports"

An appraisal of the MICCAI 2025 paper by Bassi et al., which trains tumour-segmentation models from radiology reports using two new losses (volume loss and ball loss) in a method called R-Super.

- **Four limitations:** inconsistent radiology reports weaken the supervision; the method still needs some segmentation masks; the ball loss matches tumours one at a time, so an early error carries through; and the model offers no explanation for its predictions.
- **Two hypotheses, each with a proposed test:** (1) R-Super's gains keep rising as training reports grow far beyond a few thousand, tested at 10K, 50K and 100K reports; (2) adding an interpretability layer makes predictions verifiable to clinicians with little or no drop in accuracy.

## Repository contents

```
01-supervised-classification/supervised-classification-report.pdf
02-unsupervised-learning/reflux-ph-group-report.pdf
03-paper-appraisal/paper-appraisal-report.pdf
```

## Tools used in the reports

Python, NumPy, pandas, SciPy, scikit-learn, Keras/TensorFlow and Matplotlib.

## Data and attribution

The pH recordings were supplied by the course instructor and are not included in this repository.

Project 2 is a six-person report by Yilin Hu, Edward Pan, Mahir Sharif Askari, Muskan Zarak Gul, Reuben Apor Morton and MingZhao Zhu. Its supervised benchmark comes from Yilin Hu's earlier assignment.

## Use of AI tools

Each report declares its AI use in its Acknowledgment section: Claude for drafting, phrasing and grammar in my two individual reports, and OpenAI Codex for code drafting, result verification and language polishing in the group report.

## Author

Mahir Sharif Askari, BSc Computer Science and Artificial Intelligence, Queen Mary University of London.
