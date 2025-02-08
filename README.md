# Modeling Typing Performance: Insights from a Novel Dataset and Minimal Attributes
Typing is a vital skill in the present age that has a major impact on various activities of daily life. Hence, it is quite important to analyse the various factors that impact typing performance. A lot of research has been accomplished in areas of user authentication, behavioral patterns, keyboard ergonomics, and the influence of cognitive factors like memory, fatigue, and attention. Much of the existing research has been around keystroke dynamics. This paper aims to identify the various patterns in typing performance particularly for people taking part in online typing tests by providing a new dataset of 15,003 typing tests given by 22 users on monkeytype platform. The dataset provides a unique opportunity to analyze typing performance patterns, particularly in the context of online typing tests. The research investigates the feature importance of the key features that influence typing speed and examines their predictive power using various machine learning algorithms — including Linear Regression, Random Forest, Decision Tree, XGBoost, ExtraTrees, and Catboost — as well as neural network models. Additionally, we conduct a comparative evaluation of these models, identifying the most effective approaches for WPM prediction, followed by statistical testing of hypothesis indicating performance variations in typing sessions. This research also discusses the correlation between accuracy and consistency during typing tests and how the tradeoff contributes to a better typing speed.
## Dataset Description
Monkeytype is a typing test platform with many types of typing tests.To gather diverse and authentic datasets, we circulated a Google Form among typing communities and college circuits. Participants were requested to download their performance data from the Monkeytype dashboard and submit the CSV files. Data of 15003 tests from 22 unique subjects was collected.
- Dimension – 15003 x 24
- Memory Usage – Approximately 2.2MB

| Attribute               | Description |
|-------------------------|-------------|
| `_id`                  | Unique identifier for the entire test. |
| `isPb`                 | Indicates if the session resulted in a personal best (true/false). |
| `wpm`                  | Total number of characters in the correctly typed words (including spaces), divided by 5 and normalized to 60 seconds. |
| `acc`                  | Percentage of correctly pressed keys. |
| `rawWpm`               | Calculated just like `wpm`, but also includes incorrect words. |
| `consistency`          | Based on the variance of your raw WPM. Closer to 100% is better. Calculated using the coefficient of variation of raw WPM and mapped onto a scale from 0 to 100. |
| `charStats`            | Number of correct, incorrect, extra, and missed characters (separated by semicolon). |
| `mode`                 | Typing mode used (`time`, `words`, `quote`, `zen`, and `custom`). |
| `mode2`                | Sub-mode or additional typing configuration. |
| `quoteLength`          | Length of the quote or text used for the session. |
| `restartCount`         | Number of times the test was restarted before successful completion. |
| `testDuration`         | Total duration of the typing test. |
| `afkDuration`          | Time spent idle (away from the keyboard) during the test. |
| `incompleteTestSeconds` | Time spent in incomplete tests before finishing the test. |
| `punctuation`          | Indicates if punctuation was included in the typing session (true/false). |
| `numbers`              | Indicates if numbers were included in the typing session (true/false). |
| `language`             | Language of the text used in the session (English by default). |
| `funbox`               | Indicates if funbox mode was used (e.g., custom word lists). |
| `difficulty`           | Difficulty level of the typing session (`normal` by default). |
| `lazyMode`             | Indicates if lazy mode (relaxed typing rules) was enabled (true/false). |
| `blindMode`            | Indicates if blind mode (hidden typed text) was enabled (true/false). |
| `bailedOut`            | Indicates if the user exited the test prematurely (true/false). |
| `tags`                 | Tags or labels associated with the typing session. |
| `timestamp`            | Date and time when the session occurred. |

## Predictive modeling for wpm prediction 
Linear Regression and Tree-Based Models - Random Forest, Decision Tree, Extra Trees, and Gradient Boosting were explored. Further performance of models - AdaBoost, ExtraTrees, catboost and Support Vector Regressor was also analysed. Futher, the best set of hyperparameters for these models was identified using Grid Search.

To achieve higher accuracy and robustness, advanced models and techniques were implemented. An advanced neural network model was designed (fig) with :
- Input Layer: Accepts preprocessed feature vectors as input. Second-degree polynomial features were generated with interaction terms only, enhancing the model's ability to capture non-linear relationships.
- Hidden Layers: Includes layers with 128, 256, and 128 units, each employing ReLU activation, L2 regularization, batch normalization, and dropout (rates of 0.3 to 0.4) for improved generalization.
- Residual Connection: Combines intermediate features to enhance representational capability.
- Output Layer: A single neuron with a linear activation function outputs regression predictions.
- The model was compiled using the Adam optimizer (learning rate: 0.001) with mean absolute error (MAE) as the loss metric. Training was performed over 50 epochs with a batch size of 32, employing early stopping and learning rate reduction callbacks for optimized convergence.

This was follwed by a Stacking regressor which combined predictions from the neural network and traditional ensemble models (CatBoost, Extra Trees, Gradient Boosting) along with the original features, utilizing Ridge regression as the meta-model. Passthrough of original features to the meta-model was enabled for enriched prediction inputs. The stacked model was evaluated on processed test data.

![image](https://github.com/user-attachments/assets/84db53fe-e715-4b62-a206-96ff308066f5)

## Performance evaluation of various models
