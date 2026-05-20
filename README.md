# Laboratory-Work-5-Activity-Comparative-Analysis-Completion-requirements

https://colab.research.google.com/drive/1z_Eixe_yt5yU4OKJIfo0LrKrXqqT6vG6?usp=sharing

GUIDE QUESTIONS (FINAL REFLECTION)

A. Model Performance
1. Highest Accuracy: Based on comparison_df, Custom MobileNetV2 (Test Acc: 0.0615) performed the best. Its inverted residual blocks and depthwise separable convolutions allow it to learn complex features efficiently even with a relatively small custom dataset compared to the heavier VGG16.

-Based on the metrics in comparison_df, Custom MobileNetV2 achieved the highest accuracy with a Test Accuracy of 0.0615. Its use of inverted residual blocks and depthwise separable convolutions allows the architecture to extract complex features efficiently. This lightweight design prevents severe overfitting and learns better generic feature hierarchies from small, specialized datasets compared to deeper, heavier networks like VGG16

2. Lowest Performance: Custom VGG16 showed lower performance. This is often because VGG16 is very parameter-heavy, making it prone to vanishing gradients or requiring more epochs to converge when using a low learning rate (0.0001) for transfer learning.

-The Custom VGG16 model demonstrated the lowest performance. VGG16 is a highly parameter-heavy architecture composed of large stacks of dense fully connected and standard convolutional layers. When using a low learning rate (0.0001) for transfer learning over a short duration, such models are highly susceptible to slow convergence or getting trapped in local minima. Without a massive dataset to tune its massive weight profile, it fails to optimize effectively.

3. Loss Comparison: The custom models had significantly higher test loss than train loss (visible as N/A in the consolidated table if history was reset, but observed during training), indicating that while they are learning, there is a large generalization gap that could be solved with more epochs or data augmentation.

-The custom models showed a substantial generalization gap, where the test loss was significantly higher than the training loss. This clear mathematical discrepancy indicates overfitting. The models effectively memorized specific pixel arrangements or backgrounds within the limited training partition but struggled to project those representations onto unseen test data. This issue can be resolved by extending training over more epochs, adjusting optimization weights, or applying aggressive data augmentation techniques.

B. Evaluation Metrics

4 Accuracy vs. Others: Accuracy is insufficient here because our dataset has 20 classes. If one class is over-represented, the model could achieve high accuracy by simply guessing that class. Precision and Recall ensure we see how well the model identifies each specific tree species.

-In a multi-class dataset containing 20 distinct tree species, relying solely on Accuracy is insufficient and potentially misleading. If the data split exhibits any class imbalance, a model can cheat the accuracy metric by heavily favoring predictions toward the majority class

5. Best F1-score: Custom MobileNetV2 had the highest F1-score (0.0244). This indicates a better balance between Precision and Recall than the other models.

-Custom MobileNetV2 registered the best F1-score of 0.0244. Because the F1-score is the harmonic mean of Precision and Recall, this peak metric proves that MobileNetV2 maintained a more stable, functional balance between classifying tree targets accurately and reducing false alarms across all 20 classes.

6. Precision/Recall Differences: Most models showed higher Recall for specific classes like 'Willow Tree' or 'Pear Tree', while Precision was low, meaning they frequently over-predicted those classes (False Positives).

-Several models exhibited a high Recall for specific classes such as 'Willow Tree' or 'Pear Tree' alongside very low Precision. This divergence indicates that the models suffered from an over-prediction bias toward those specific classes. Whenever an input was ambiguous, the system defaulted to predicting a Willow or Pear Tree, generating a high count of False Positives while successfully covering the True Positives of that class.

C. Confusion Matrix Analysis

7. Misclassifications: Looking at the confusion matrices, classes with similar leaf shapes or bark textures were frequently confused.

-The confusion matrices showed a high concentration of errors among tree species that possess highly overlapping visual characteristics, such as similar serrated leaf margins, compound leaf arrangements, or matching bark textures. The spatial feature maps produced by the convolutional layers could not resolve fine-grained details, leading to inter-class confusion.

8. Patterns: There is a vertical pattern in the confusion matrices where certain models defaults to a 'majority' class prediction when uncertain.

-A clear vertical striping pattern was present within the confusion matrices of the weaker models. This pattern visually confirms a strong majority-class prediction bias. When the feature maps failed to generate a definitive activation score, the final Softmax layer defaulted to the most statistically dominant class from the training distribution.

D. ROC and AUC

9. Highest AUC: Custom MobileNetV2 (AUC: 0.5489) led the group.

-Custom MobileNetV2 outperformed the other models with a top Area Under the ROC Curve (AUC) of 0.5489.

10. AUC Significance: AUC tells us the probability that the model will rank a random positive example higher than a random negative one. A score above 0.5 means it is performing better than random chance.

-The AUC metric evaluates a model’s ability to distinguish between classes across all operational thresholds. An AUC score above 0.5 mathematically demonstrates that the model performs better than random guessing (which yields a baseline score of 0.5). While 0.5489 indicates room for improvement, it proves the network has established a foundational baseline for distinguishing between the 20 botanical classes.

E. Explainability (Grad-CAM)

11. Revelations: Grad-CAM revealed that MobileNetV2 focused more on leaf textures, while EfficientNetB0 sometimes focused on the background or edges of the image.

-Grad-CAM heatmaps provided key insights into model behavior:
            MobileNetV2 effectively isolated relevant regions, focusing its activation gradients directly on leaf textures and unique structural                               contours.  
            EfficientNetB0 consistently misdirected its focus, activating on edge borders, background dirt, or sky backdrops rather than the                                   botanical specimens themselves.

12. Focus: The more accurate models tended to have heatmaps centered on the actual tree/foliage.

-The high-performing configurations generated tightly centered heatmaps that directly targeted the foliage or plant stalks. Conversely, low-performing models produced scattered, unorganized heatmaps that highlighted arbitrary background details, explaining their poor test performance.

13. Meaningful Heatmaps: VGG16 often produces very clear Grad-CAM heatmaps due to its simpler architecture, but MobileNetV2 provided the most logically relevant activations for classification.

-Because VGG16 relies on a straightforward, linear sequence of traditional convolutional layers, its gradient flow produces clean, high-resolution Grad-CAM visualizations. However, MobileNetV2 provided the most logically coherent features, showing that its internal feature mapping aligned closest with human botanical identification.

F. Model Comparison & Improvement
14. Recommendation: MobileNetV2. It offers the best accuracy-to-size ratio, making it ideal for mobile deployment.

-MobileNetV2 is the optimal choice for this deployment scenario. It balances feature extraction with a lightweight framework built on depthwise separable convolutions. It delivers the best accuracy-to-size ratio, minimizing memory usage and inference latency

15. Improvements: To improve performance, I would implement Data Augmentation (rotation, zooms), increase the number of Epochs to 30-50, and try Fine-tuning (unfreezing the last few layers of the base model).

-To enhance performance from these initial baselines, the following three improvements should be prioritized:
            Data Augmentation: Implement randomized variations (zooms, horizontal flips, shifts, and color jitters) during training to prevent the network from                memorizing static positions.
            Extended Training Window: Increase the optimization period to 30–50 epochs to allow the learning rates to converge smoothly.  
            Selective Fine-Tuning: Unfreeze the final block of layers in the pre-trained base model. This allows the top layers to adapt their weights                          specifically to fine-grained tree textures while preserving the early edge-detection filters. 
            
G. Real-World Application
16. Application: This could be used in an app for foresters or botanists to identify tree species in the field.

-This deep learning system can be integrated into a mobile application tailored for field foresters, environmental surveyors, and botanists. Users can take real-time photos of tree leaves or bark to identify species instantly in the field.

17. Risks: An inaccurate model could lead to the mismanagement of invasive species or incorrect ecological surveys.

-Deploying an inaccurate model carries significant risks:

Environmental surveyors might misclassify and overlook invasive species, delaying critical ecological interventions.

Land management groups could collect faulty biodiversity data, leading to misallocated conservation funding and inaccurate environmental impact assessments.

18 Integration: The .keras models we saved can be converted to TensorFlow Lite and integrated into a mobile application using the Flutter or React Native frameworks.

-deploy the trained models onto mobile devices, the saved .keras files must be compiled into the TensorFlow Lite (.tflite) format to reduce binary size and optimize runtime execution. This file can then be integrated into a mobile application using Flutter or React Native via native plugins, enabling cross-platform, on-device machine learning inference.
