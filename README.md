# NYC Taxi demand prediction by GluonTS and Amazon Forecast

Let's predict Taxi demand in New York City. Here we use GluonTS and Amazon Forecast, a time-series forecasting service for prediction then compare results.

## 0. Preare data set

We download raw data set from the publically availalbe TLC site. We store one or two files onto the instance local storage for exploratory analysis and to finalize pre-processing script. Then we store whole data set that we want to process for model training, onto a S3 bucket while dividing them into multiple folders. Later we trigger an Amazon Sagemaker processing jobs per each foler for multi-processing.

## 1. Feature engineering

First, we validate and clean raw data set. The raw data contains individual occurence so we aggregate the occurences into an hour slot, which is the target for model training and prediction.
The purpose this step is to finalize a script for massive processing over the whole data set.

## 2. Amazon Sagemaker processing jobs

Once we have finalized a processing script, the next step is to trigger Amazon Sagemaker prossing jobs. Especially, to save our precious time, we trigger one processing job per each folder, 4 in total by taking benefits of Amazon Sagemaker scalability.

## 3. Training, prediction with GluonTS

GluonTS is a python package for probabilistic time series modeling based on PyTorch and MXNet. Here, first we define training data set and test data set from the pre-processed data, followed by storing them onto a S3 bucket for Amazon Forecast models. Then we use PandasDataset(gluonts.dataset.pandas.PandasDataset) to feed the data into a GlueonTS model with 1 week(24x7) of prediction length. Once training is done, we briefly sanity check with a prediction result against test data. We save the prediction onto a S3 bucket for comparision.

## 4. Train and prediction with Amazon Forecast

We start with creating a Forecast data set followed by an import job, a data set group. Then we trigger a Forecast auto predictor with 1 week of Forecast horizon. Also after a sanity check over a prediction, we store the prediction onto a S3 bucket for comparision.

## 5. Train and prediction with Amazon Forecast with additional data i.e. weather and holiday

It is reasonable to imagine New York citizens more or less demand taxi under bad weather or depending on holiday. Let's see if the hypothesis is true. Here we train an Amazon Forecast model with additional data, i.e. weather index and holiday information. We take a look at the explanability a.k.a impact score of the two addiontal data.

## 6. Result comparisons

It's time to see how well the three models performs against the ground truth or test data. As a preview, I can say that all of them showed decent performance in prediction of New York City taxi demand.

