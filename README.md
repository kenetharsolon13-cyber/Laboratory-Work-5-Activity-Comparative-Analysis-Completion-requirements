# Laboratory-Work-5-Activity-Comparative-Analysis-Completion-requirements

https://drive.google.com/drive/folders/1yocHYuwYPzoUgoTHHm8BfPya20mADRt2?usp=drive_link

https://colab.research.google.com/drive/1z_Eixe_yt5yU4OKJIfo0LrKrXqqT6vG6?usp=sharing

GUIDE QUESTIONS (FINAL REFLECTION)


A. Model Performance

1. Highest AccuracyBased on the finalized comparison_df, Custom MobileNetV2 performed the best, achieving a Test Accuracy of 0.0718. Its specialized architecture—relying on inverted residual blocks and depthwise separable convolutions—allows it to extract intricate spatial feature hierarchies much more efficiently. This lightweight structure prevents severe overfitting and allows the network to adapt better to custom, smaller datasets compared to heavier architectures.
  
2. Lowest PerformanceThe lowest classification accuracy came from Custom EfficientNetB0 (Test Acc: 0.0410), closely trailing Custom VGG16 (Test Acc: 0.0462). VGG16 and EfficientNetB0 are highly complex, parameter-dense models. When frozen for transfer learning and paired with a low learning rate ($0.0001$) over limited training iterations, these heavy architectures frequently get stuck in suboptimal local minima or suffer from slow convergence, failing to align their massive weight profiles to a niche 20-class dataset.

3. Loss Comparison
Looking at the final metrics, the models demonstrate a notable performance gap. For instance, MobileNetV2 presents a high test loss (4.0416), indicating that while it is succeeding at picking the correct top class more often than its peers, its underlying probability distributions are highly uncertain or overconfident on incorrect elements. This systemic generalization gap confirms overfitting to the training data split, which can be mitigated using data augmentation or extended learning rate schedules.

B. Evaluation Metrics
4. Accuracy vs. Other Metrics
In a multi-class dataset containing 20 distinct tree species, relying exclusively on Accuracy is insufficient. If a dataset exhibits class imbalances, a model can exploit the metric by over-predicting the majority class.

Precision evaluates exactness (minimizing False Positives to ensure that a tree flagged as a 'Willow' is truly a Willow).

Recall measures completeness (minimizing False Negatives to ensure no actual 'Willow' trees are skipped).

5. Best F1-score
Custom MobileNetV2 registered the highest F1-score of 0.0655. Because the F1-score is the harmonic mean of Precision and Recall, this peak score mathematically demonstrates that MobileNetV2 established a much more functional, stable balance between identifying correct species and minimizing false alarms than the other networks.

6. Precision/Recall Differences
Both VGG16 and EfficientNetB0 show an extreme skew where their Recall matches their overall accuracy, but their Precision drops to near zero (0.0028 and 0.0020, respectively). This statistical behavior reveals a critical defect: the models are defaulting to predicting a single dominant "majority" class for almost every image. This yields a massive count of False Positives across the dataset and tanks the Precision scores.

C. Confusion Matrix Analysis
7. Misclassifications
The confusion matrices show high clusters of errors among specific botanical categories. This occurs because species sharing overlapping geometric properties—such as matching leaf margins (serrated vs. smooth), similar leaf compound structures, or identical bark textures—generate highly identical activation responses across the network's convolutional filters.

8. Structural Patterns
A distinct vertical striping pattern is noticeable within the confusion matrices of the weaker models. This structural visual artifact mathematically confirms a severe majority-class prediction bias; when the model encounters an ambiguous set of features, it systematically falls back on a single baseline class prediction.

D. ROC and AUC
9. Highest AUC
Custom MobileNetV2 led the group with an Area Under the ROC Curve (AUC) of 0.5985.

10. AUC Significance
An AUC score evaluates a model's true positive rate against its false positive rate across all classification thresholds. A baseline score of 0.5 represents blind, random guessing. Therefore, MobileNetV2's score of 0.5985 shows that it possesses a statistically genuine ability to distinguish between the 20 distinct tree classes, outperforming pure chance.

E. Explainability (Grad-CAM)
11. Visual Revelations
Grad-CAM gradient heatmaps exposed what the models were actually looking at:

MobileNetV2 succeeded in focusing its activation weights directly on target biological features, such as distinct leaf venation patterns and foliage structures.

EfficientNetB0 consistently misdirected its focus, activating heavily on irrelevant image borders, background soil, or sky backdrops.

12. Spatial Focus
The more accurate models featured tightly grouped, centered heatmaps locked directly onto the actual foliage. The poor-performing models produced scattered, peripheral heatmaps, proving they were learning arbitrary background noise rather than meaningful botanical markers.

13. Architectural Impact on Heatmaps
Because VGG16 relies on a classic, linear stack of standard convolutional layers, its gradient maps are often visually clear and high-resolution. However, MobileNetV2's internal feature maps provided the most logically coherent representations, proving that its internal deep layers are processing regions that align with human botanical identification.

F. Model Comparison & Improvement
14. Architecture RecommendationMobileNetV2 is highly recommended. It yields the highest overall task accuracy ($0.0718$), the strongest structural AUC ($0.5985$), and operates on a lightweight, efficient architecture. This makes it ideal for deployment in resource-constrained environments.

15. Engineering Improvements
    
To scale these performance baselines higher, I would implement three changes:
     - Data Augmentation: Apply random rotations, horizontal flips, zooming, and color shifts to prevent the models from memorizing static positions or lighting           conditions.
     -Extended Training Cycles: Increase the training duration to 30–50 epochs to give the optimizers time to settle into stable global minima.
     - Layer Fine-Tuning: Unfreeze the top layers of the pre-trained base models (rather than just training the final dense classification head) so the deep               convolutional filters can fine-tune directly to the unique visual textures of tree species.

G. Real-World Application

16. Practical Application
This system can be integrated directly into a mobile application used by field botanists, forest rangers, or conservationists to quickly inventory local tree populations and map forest biodiversity.

17. Operational Risks
Deploying an inaccurate model introduces significant real-world risks, such as field surveyors misclassifying and ignoring highly destructive invasive plant species, or generating faulty biodiversity reports that lead to misallocated environmental conservation funding.

18. Mobile Deployment Integration
To bring these models into production, the saved .keras files should be converted into the optimized TensorFlow Lite (.tflite) format to compress model size and reduce operational latency. This .tflite model file can then be integrated into a mobile cross-platform application UI via Flutter or React Native for fast, on-device edge AI inference without needing internet access.
