## **Predicting Income Levels from Census Data**

Income affects nearly every aspect of a person's living condition, such as the place they live, the opportunities they can have, and even how much free time they have. I started to wonder whether it is possible to predict someone's income using only demographic features. And if it is possible, which type of model would be the best to make predictions? To explore this question, I used a U.S. census dataset that contains roughly 48,000 individuals. The dataset includes demographic and employment-related features such as age, education level, occupation, marital status, and weekly working hours. My goal was not to build the most complicated model, but simply to examine the performance of classic models.

## Exploring the Data

Before building any models, it is important to understand what kinds of patterns may exist in the data. After visualizing the data, some relationships were revealed. For instance, Figure 1 shows that most people who earn more than 50K work more hours per week. It can be observed from the figure that people who earn more than 50K rarely have low weekly work hours. However, the trend is not linear. Most high-income people work around 30 to 40 hours per week, but extreme work hours tend to reduce the number of high-income individuals.

![figure1](figure1.png) 

Figure 1: Working Hours per Week vs. Income

Education showed an even stronger separation. The dataset lists 16 different education categories, ranging from preschool to doctorate degrees. When mapping these categories in their natural order, the distribution of people with high income shows a clear pattern: people with advanced degrees were far more likely to earn more than 50K. From Figure 2, it can be clearly observed that high-income individuals mostly have an education level above 9, which corresponds to high school graduates.

![figure2](figure2.png) 

Figure 2: Education Number vs. Income

In addition, age also played a noticeable role. Younger individuals are more likely to fall into the less than 50K group. It can be observed from the figure 3 that under 20s, there are few people earning more than 50K. The number of high income people started to increase from 30s, and reach its max at 40s.

![figure3](figure3.png) 

Figure 3: Age vs. Income

These visual patterns provided a sense of which features would likely be helpful for predictions.

## Preprocessing: Cleaning and Selecting What Matters
The raw dataset had a few issues that had to be addressed. Some entries used “?” to represent missing values, and several columns were redundant. For instance, fnlwgt is a census weighting field that doesn't describe anything about an individual. Relationship also overlapped heavily with marital-status. Therefore, I removed these features entirely to reduce noise.

For numeric variables like age and hours worked, I filled missing values with medians and standardized them. For categorical variables, any missing data was filled with “missing.” To better build the model, the education feature was treated as an ordered variable. All of these steps were embedded into a unified preprocessing pipeline, which allowed all of the transformations to be completed in one step.

## Models Developing
The model building started with a simple baseline. A dummy classifier that always predicts the majority class was built. It reached an accuracy of around 76%, which was surprisingly high but also indicated that the data was imbalanced.

Following the dummy classifier, a decision tree was built, which is a model that asks a series of yes/no questions about the data and breaks it into smaller and smaller groups. The performance improved significantly, reaching around 81% accuracy. Another classic model was also built, which is k-nearest neighbors. It is a model that makes predictions by looking at the k closest examples in the dataset and choosing the most common outcome. The performance improved slightly again, reaching approximately 84% accuracy.

The model that had the most promising performance appeared after k-nearest neighbors. The model built using a Support Vector Machine (SVM) with an RBF kernel brought the accuracy to another peak, reaching 0.85. Compared to the other models, which rely on straight lines to separate the data, the SVM draws flexible boundaries around groups of data. Instead of a rigid boundary, it bends and wraps around the data. Since the curved boundaries fit the data more naturally, it can make better predictions. However, the performance of this model can be further improved. The SVM relies heavily on one regularization parameter, C. To explore its best value, I ran a small loop over several values and found that the model performed best when C = 100. With this value, the SVM’s accuracy increased further, ultimately reaching 0.86. 

## How Well Can We Predict Income?
After training the SVM with C = 100 and evaluating it on the test set, the model showed around 0.8498 accuracy. It was surprising that the test accuracy closely matched the cross-validation performance on the training set. This implied that the model was well generalized and that the reliability of this model was quite good.

## Caveats and Limitations
Despite the model's good numerical performance, there are still several limitations.

1. The dataset is outdated. It reflects the U.S. in the 1990s, not today. After 30 years of change, the relationships between income and factors such as age, gender, and occupation have shifted significantly. With the boom of the internet and technology, the distribution of the high-income group is likely very different now. A model trained on this dataset may not fit modern contexts.

2. The dataset does not consider other important factors influence the income. Although the model shows a promising accuracy, predicting income from demographic information has inherent limits. There are many more factors that influence income but do not appear in the dataset, such as region, population, year in workforce, household structure, disability status and immigration status. 
   
3. Some features may encode social biases. For example, the “sex” feature reflects historical gender income disparities. Thirty years ago, income differences between men and women were more obvious, and these inequalities can be subtly captured in the dataset. Since then, society has placed more emphasis on pay equity, so the patterns learned from this older data may no longer reflect current realities.

4. The dataset is imbalanced. When predicting the most frequent class, the accuracy already reaches 76%. This imbalance can limit how well the model learns meaningful patterns and can influence the performance of the model.

5. SVM is powerful but hard to explain. Compared to linear models, which provide coefficients for each feature, SVM does not provide such coefficients and makes the results harder to interpret. It would be better to use a linear model or another more interpretable model if we need to explicitly explain how the predictions are made.

## Conclusion
In the beginning, a simple question was asked: can income be predicted from basic census features? In the end, after building several models, we have confidence to say that we can, at least to a meaningful degree. Despite the limitations of demographic attributes, they still possess enough information for classic models to detect patterns and make reliable predictions.

Besides the satisfying 0.86 accuracy of the final model, working through data cleaning, feature selection, model building and comparison, and interpretation made me appreciate the importance of data preprocessing and made me understand that better data leads to a better model.

