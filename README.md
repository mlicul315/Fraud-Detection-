# Fruad Detection for Wells Fargo

[wells fargo image]

## Buisness Understanding

Near the end of 2022, the Consumer Financial Protection Bureau (CFPB) ordered Wells Fargo to pay 2 billion in consumer redress and a 1.7 billion civil penalty. This resulted from customers having their vehicle wrongly possessed, illegally charging customers large fees and interest on auto and home loans, and charging 'unlawful' overdraft fees (https://www.nerdwallet.com/article/banking/well-fargo-fines). 
Additionally, JD Power released their 2022 U.S. National Banking Satisfaction Study. According to the study, the industry average score is 648. Wells Fargo scored well below the average; recording a score of 625. Dropping 3 points from 2021. In comparison to other competitors, Chase recorded a score of 678, and PNC recording a score of 658 (https://www.jdpower.com/business/press-releases/2022-us-national-banking-satisfaction-study). Although this does not pertain to fraud, this publicity is detrimental to the bank’s reputation. 

Nonetheless, in 2022, a survey conducted by Verint, asked respondents what is a major factor when deciding what bank to choose. Many of the answers reported cited “security of personal information,” “fraud protection,” and “fraud alerts” as the most important factors surrounding their banking selection. This beats out ‘low or no fees’ which was the the previous top concern of customers (https://www.scmagazine.com/analysis/identity-and-access/bank-customers-now-rank-security-and-fraud-protection-ahead-of-low-fees). 

Based on the survey, by detecting fraudulent activity on a customers account and aiming to prevent it as soon as possible, Wells Fargo can establish a trusting relationship with their customers in hopes to maintain consumer retention and potentially increase the numbers of customers by generating a positive reputation. 

## Data Understanding

The data used to create the model includes historical information of over 6 million transactions. It contains various features such as:
`step`
`transaction type` 
`amount`
`nameOrig`
`oldbalanceOrg`
`newbalanceOrig`
`nameDest`
`oldbalanceDest`
`newbalanceDest`
`isFraud`
`isFlaggedFraud`
The original dataset was a large file (almost 500mb). Therefore, I split it into 6 different chunks and then concated them together to make `fraud_df`.

[insert data exloring images]

## Model 
After checking the correlation of features using a heatmap, I created a Linear Regression model. The target variable is the isFraud column and the remaing columns as features. Based on the heatmap and .corr(), I can see that amount, isFlaggedFraud, and step have the highest correlation.

The Linear Regression model performed extremely well. 

Linear Regression:
- Accuracy: 0.9991190735891818
- Precision: 0.7819209039548023
- Recall: 0.4271604938271605
- F1 score: 0.5524950099800399

I decided to try a Random Forrest model despite the Linear Regression to see if the results would vary. The Random Forrest model resulted in a similar accuracy score, but had a better precision, recall, and F1 score. 

Random Forrest:
- Accuracy: 0.9997312427899199
- Precision: 0.9855623100303952
- Recall: 0.8006172839506173
- F1 score: 0.8835149863760218

## Deployment 

I used Gradio to create an interface for users to input information about a transaction and determine if it is fraudulent. The interface asks the user to input the type of transaction (e.g. CASH_IN, CASH_OUT, DEBIT, PAYMENT, TRANSFER), the amount of the transaction, and the old balance before the transaction. Once the user inputs this information, the model makes a prediction using a pre-trained Random Forest Classifier that was built using transaction data. The model was trained on a dataset with features such as transaction type, transaction amount, and the old balance before the transaction, with the target variable being whether the transaction was fraudulent or not. The model predicts whether the transaction is fraudulent or not, and the interface displays the prediction as output. If the prediction is "Fraud", the user is notified that the transaction is potentially fraudulent and should be investigated further. If the prediction is "No Fraud", the user is notified that the transaction is likely not fraudulent.

[insert gradio picture]

## Conclusion

Through Gradio, users can input the type of transaction, the amount, and the old balance to determine whether the transaction is fraudulent. While our model has shown good performance, there are steps that can be taken to improve it further. One major step is collecting more data directly from Wells Fargo to increase the size and quality of our dataset. Additionally, collecting new features such as the date or time of the transactions and even information on the customer's spending habits can all help in providing an even more accurate model.
