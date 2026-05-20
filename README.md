# Laboratory-Work-5-Activity-Comparative-Analysis-Completion-requirements

https://colab.research.google.com/drive/1z_Eixe_yt5yU4OKJIfo0LrKrXqqT6vG6?usp=sharing

GUIDE QUESTIONS (FINAL REFLECTION)
A. Model Performance
1. Highest Accuracy: Based on comparison_df, Custom MobileNetV2 (Test Acc: 0.0615) performed the best. Its inverted residual blocks and depthwise separable convolutions allow it to learn complex features efficiently even with a relatively small custom dataset compared to the heavier VGG16.
2. Lowest Performance: Custom VGG16 showed lower performance. This is often because VGG16 is very parameter-heavy, making it prone to vanishing gradients or requiring more epochs to converge when using a low learning rate (0.0001) for transfer learning.
3. Loss Comparison: The custom models had significantly higher test loss than train loss (visible as N/A in the consolidated table if history was reset, but observed during training), indicating that while they are learning, there is a large generalization gap that could be solved with more epochs or data augmentation.
B. Evaluation Metrics
4 Accuracy vs. Others: Accuracy is insufficient here because our dataset has 20 classes. If one class is over-represented, the model could achieve high accuracy by simply guessing that class. Precision and Recall ensure we see how well the model identifies each specific tree species.
5. Best F1-score: Custom MobileNetV2 had the highest F1-score (0.0244). This indicates a better balance between Precision and Recall than the other models.
6. Precision/Recall Differences: Most models showed higher Recall for specific classes like 'Willow Tree' or 'Pear Tree', while Precision was low, meaning they frequently over-predicted those classes (False Positives).
C. Confusion Matrix Analysis
7. Misclassifications: Looking at the confusion matrices, classes with similar leaf shapes or bark textures were frequently confused.
8. Patterns: There is a vertical pattern in the confusion matrices where certain models defaults to a 'majority' class prediction when uncertain.
D. ROC and AUC
9. Highest AUC: Custom MobileNetV2 (AUC: 0.5489) led the group.
10. AUC Significance: AUC tells us the probability that the model will rank a random positive example higher than a random negative one. A score above 0.5 means it is performing better than random chance.
E. Explainability (Grad-CAM)
11. Revelations: Grad-CAM revealed that MobileNetV2 focused more on leaf textures, while EfficientNetB0 sometimes focused on the background or edges of the image.
12. Focus: The more accurate models tended to have heatmaps centered on the actual tree/foliage.
13. Meaningful Heatmaps: VGG16 often produces very clear Grad-CAM heatmaps due to its simpler architecture, but MobileNetV2 provided the most logically relevant activations for classification.
F. Model Comparison & Improvement
14. Recommendation: MobileNetV2. It offers the best accuracy-to-size ratio, making it ideal for mobile deployment.
15. Improvements: To improve performance, I would implement Data Augmentation (rotation, zooms), increase the number of Epochs to 30-50, and try Fine-tuning (unfreezing the last few layers of the base model).
G. Real-World Application
16. Application: This could be used in an app for foresters or botanists to identify tree species in the field.
17. Risks: An inaccurate model could lead to the mismanagement of invasive species or incorrect ecological surveys.
18 Integration: The .keras models we saved can be converted to TensorFlow Lite and integrated into a mobile application using the Flutter or React Native frameworks.
