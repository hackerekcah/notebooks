## ROC (Receiver operating characteristic) & AUC (Area Under Curve)
* TPR (y-axis) vs FPR
```
# Alis for recall or sensitivity
TPR = TP/P = TP / (TP + FN)

# Alis for fall-out
FPR = FP/N = FP / (FP + TN)
```

## [Precision-Recall & AP (average_precision_score)](https://scikit-learn.org/stable/auto_examples/model_selection/plot_precision_recall.html#sphx-glr-auto-examples-model-selection-plot-precision-recall-py)
* AP is a summary of Precision-Recall curve
* precision (y-axis) vs recall
* Useful when classes are unbalanced.
```
Precision = TP / (TP + FP)

Recall = TP / (TP + FN)

# area under Precision-Recall curve
AP = sum_n (R_n - R_{n-1}) P_n
```

## multi-label average
```
micro: ignore classes, directly calculate metrics.
macro: calculate metric per-classes, then average the metrics.
mAP: mean Average Precision, claculate AP for each class, then average over classes.
mAUC: claculate AUC for each class, then average over classes.
```



