# Bearing Remaining Useful Life Prediction

My master's thesis at TU Clausthal, in collaboration with Piller Blowers & Compressors GmbH, focuses on bearing diagnostics and remaining useful life prediction for high-speed rotating equipment.

Unplanned bearing failures shut down machines at the worst possible time. The goal of this work is to estimate how much useful life a bearing has left from its vibration signal, early enough to plan maintenance instead of reacting to a breakdown.

The catch: a model trained on one bearing dataset usually performs much worse on another. Different machines, sensors, sampling rates, and loads all shift the data. Most published results train and test on the same rig and never fully expose this problem. So a major part of this project is cross-dataset evaluation — training on one public dataset and testing on a different one.

The benchmark uses the NASA IMS and XJTU-SY bearing datasets in both transfer directions. The pipeline combines physics-based vibration features such as envelope RMS, kurtosis, and crest factor with learned features from a convolutional autoencoder. These feed a health indicator, which a BiLSTM uses for RUL regression.

Cross-dataset results, leave-one-bearing-out cross-validation:

* NASA IMS → XJTU-SY: RMSE 0.0859 ± 0.0218
* XJTU-SY → NASA IMS: RMSE 0.1331 ± 0.0169

Detection was much more reliable for outer-race faults than inner-race faults. That limitation is analyzed in the thesis, not hidden.

I also report what did not work. FFT-based data augmentation made results worse on several folds, and the sliced Wasserstein distance gave no improvement over a plain Mahalanobis distance in this setup. Negative results are part of the contribution — they save the next person from running the same dead ends.

The same methodology was applied to confidential vibration data from a high-speed industrial fan: speed-dependent autoencoder training, anomaly detection, statistical state-change detection, and false-alarm-rate calibration for a production-oriented monitoring workflow. The industrial dataset cannot be published, but the method structure is documented here.

## Status

The thesis is being finalized in summer 2026. Cleaned notebooks and source code will be added after the defense, once confidential company material has been removed.

Built with Python, PyTorch, and scikit-learn.

Supervised by Mazen Bouchur and Prof. Dr.-Ing. Andreas Reinhardt (first examiner); Prof. Dr. Armin Lohrengel (second examiner).
