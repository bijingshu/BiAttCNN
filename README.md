# An Attention based Bidirectional LSTM Method to Predict the Binding of TCR and Epitope
BiLSTM-Attention-CNN is a deep learing based model for predicting TCR-peptide binding.
### Python Dependencies and Requirements:
```text
pytorch 1.0.0
numpy 1.15.4
scikit-learn 0.19.2
```
- The code runs faster with GPU usage, but it should work also on CPU. It was developed on GPU
(Nvidia Tesla V100 with CUDA 10.0).

### Model Training
Training a model should take about an hour.
All runs use the main ERGO.py file.

For training the ERGO model, run:
```commandline
python ERGO.py train model_type dataset sampling device 
```
When: `model_type` is `lstm` for the Bilstm-Attention-CNN based model.
`dataset` is `mcpas` for McPAS-TCR dataset, or `vdjdb` for VDJdb dataset.
`sampling` can be `specific` for distinguishing different binders, `naive` for separating non-binders and binders,
or `memory` for distinguishing binders and memory TCRs.
`device` is a CUDA GPU device (e.g. 'cuda:0') or 'cpu' for CPU device.

Add the flag `--protein` suit the model for protein binding instead of specific peptide binding.
Add the argument `--train_auc_file=file` to write down the model train AUC during training.
Add the argument `--test_auc_file=file` to write down the model validation AUC during training.
Add the argument `--model_file=file.pt` in order to save the trained model.
Add the argument `--test_data_file=file.pickle` in order to save the test data.

### Prediction
You can use the models you have trained to predict new data,
or you can use our already trained models in the models directory.

Data for prediction must be in a .csv format,
where the first column is the TCRs and the second column is the peptides.
See [example](pairs_example.csv).

For binding prediction, run:
```commandline
python ERGO.py predict model_type dataset sampling device --model_file=file.pt --test_data_file=file.csv
```
The argument `--model_file=file.pt` is the trained model.
If you want to use our trained models, use `--model_file=auto`, and the code will choose the right trained
model according to the arguments `model_type`, `dataset` and `sampling`.
If you are using your model file, make sure that those arguments match the model.

The argument `--test_data_file=file.csv` is the test data to predict in .csv format.
If you want to run our example use `--test_data_file=auto`.

The code should print the input TCRs and the peptides, with the predicted binding probabilities.
Notice that in the autoencoder model, TCR max length is 28, so longer sequences will be ignored.
Prediction should be take a few seconds.

## References
1. Springer I , Besser H , Tickotsky-Moskovitz N , et al. Prediction of Specific TCR-Peptide Binding From Large Dictionaries of TCR-Peptide Pairs[J]. Frontiers in Immunology, 2020, 11:1803.
