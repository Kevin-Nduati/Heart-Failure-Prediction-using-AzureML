# Heart Failure Prediction using AzureML
Cardiovascular diseases are the number 1 cause of death globally. My aim is to create a machine learning model that can predict mortality caused by heart failure.

## Dataset
The dataset contains the following columns:
* **Age-** Age of patient
* **Anaemia-** Decrease of red blood cells or haemoglobin (boolean)
* **creatinine_phosphate-** Level of the CPK enzyme in the blood
* **Diabetes-** If the patient has diabetes (boolean)
* **ejection_fraction-** Percentage of blood leaving the heart at each contraction
* **high_blood_pressure-** If the patient has hypertension
* **platelets-** Plateletes in the blood
* **serum_creatinine-** Level of serum creatinine in the blood
* **serum_sodium-** Level of serum sodium in the blood
* **sex-** woman or man
* **smoking-** If the patient smokes or not
* **time-** Follow up period(days)
* **death_event-** If the patient deceased during the follow-up period(boolean)
### Overview
* The data was downloaded from kaggle and you can download it <a href="https://github.com/Kevin-Nduati/Heart-Failure-Prediction-using-AzureML/blob/71c29d71430511ab531bf353f1c1287f00c022f3/heart_failure_clinical_records_dataset.csv">here</a>

### Task
We will create a machine learning model that predicts whether a person died from heart failure given the other features.

### Access
The data was registered to the Azure Machine Learning Studio by uploading it from local files. 

## Automated ML
These are the settings I set for the AutoML. I set the experiment to timeout after 15 minutes, and to focus on accuracy. The task is a binary classification problem, and I enabled early stopping, in the event accuracy did not improve over 20 iterations.
<img src="https://github.com/Kevin-Nduati/Heart-Failure-Prediction-using-AzureML/blob/4c9641370ee8178d5e047bcfada7acc414868496/images/01-Automl_settings.png">


### Results
The AutoML model managed to get 85.9% accuracy. The best model was a voting ensemble
<img src="https://github.com/Kevin-Nduati/Heart-Failure-Prediction-using-AzureML/blob/e51ea77355105392e2fde37f3c099bb8e92246fe/images/01-Automl_best_acc.png">
The parameters of the model are:
<img src="https://github.com/Kevin-Nduati/Heart-Failure-Prediction-using-AzureML/blob/e6bf27248989f0361af1e26363f8f69607c1e9a0/images/01-Automl_params.png">


## Hyperparameter Tuning
I used the Logistic Regression model in my python train script. I chose C and max_iter as the hyperparameters to tune. The parameter search space for C is [0.01, 0.1, 1.0, 10, 100] and for max_iter was [20,50,100,120,150].
I also used a random arameter sampling to sample over a discrete set of values, even though this would mean it was going to take longer to execute.
The bandit policy is based on slack factor/ slack amount and evaluation interval. Bandit terminates where the primary metric is not within the specified slack factor/ slack amount compared to the best performing run.

<img src="https://github.com/Kevin-Nduati/Heart-Failure-Prediction-using-AzureML/blob/4b3fb82372a86a54bd68d0fab8874b30bb4abecb/images/02-Hyperdrive_params.png">



### Results
<img src="https://github.com/Kevin-Nduati/Heart-Failure-Prediction-using-AzureML/blob/b5a48edb5ed50c3152c88d4154c1af3b971888db/images/02-Hyperdrive_acc.png">
In order to improve the model, I could have increased max_total_runs and reduced the experiment_timeout_minutes so as to allow it to optimize parameters more. I could also have used Bayesian sampling as it picks the next sample of hyperparameters based on how the previous samples performed.

## Model Deployment
Since the Automl model performed better, that is tthe model that I deployed.
This screenshot shows that the model was deployed successfully
<img src="https://github.com/Kevin-Nduati/Heart-Failure-Prediction-using-AzureML/blob/80fabf1d761e0a783c199815b8946be0085544fc/images/05-Deployed_model1.png">

Here is how I consumed the model:
<img src="https://github.com/Kevin-Nduati/Heart-Failure-Prediction-using-AzureML/blob/80fabf1d761e0a783c199815b8946be0085544fc/images/05-Deployed_model2.png">

Here are the app insights of the deployed model:
<img src="https://github.com/Kevin-Nduati/Heart-Failure-Prediction-using-AzureML/blob/e51ea77355105392e2fde37f3c099bb8e92246fe/images/03-Model_deployed_03.png">

## Screen Recording
This is the link to the youtube video detailing the workings of my model: <a href="https://youtu.be/TtizUykbwXQ">LINK</a>

