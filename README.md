# url_path_classifier

🧠 Project Overview
This project implements a multi-label machine learning model that classifies URLs based solely on their path components (like /about, /team, etc.). The goal is to identify what category a webpage belongs to — such as team, about, product, or service — without accessing or analyzing the actual webpage content.

It is particularly useful in large-scale data crawling or link classification pipelines where webpage content may not be available, but the URL structure can provide valuable clues.

🔍 Description of the Steps
1. Dataset Loading
The project begins by loading a dataset containing URLs and their corresponding label indicators (multi-label format). Each row consists of a URL (or path) and binary values indicating whether it belongs to one or more of the four target categories.

2. Data Sampling
To ensure faster training and reduce computation during experimentation, a random sample of 20,000 rows is taken from the original dataset. This helps with initial testing without compromising diversity in the data.

3. URL Path Extraction
The dataset may contain full URLs or partial fragments. Therefore, a URL parsing mechanism is applied to extract only the path part of each URL. This includes special handling for edge cases:

If the URL contains hash fragments (like #!/aboutus), these are converted into a standard path format (/aboutus)

Query strings and domain names are discarded

Only the meaningful routing structure of the URL is retained

4. Data Cleaning
After path extraction, rows with empty or missing path values are discarded. Additionally, missing values in the label columns are filled with zeros to standardize the label format across the dataset.

5. Manual Label Augmentation
To improve the model's accuracy on commonly used paths (like /aboutus, /contactus, /team), a few manually labeled examples are added to the dataset. These are paths that are known to clearly belong to one or more target categories but may be underrepresented in the sampled data.

6. Tokenization and Feature Engineering
The URL paths are then transformed into token sequences using a custom tokenizer. This tokenizer splits paths on slashes (/), hyphens (-), and underscores (_). For example, a path like /about-us_team is broken down into the individual words: about, us, team. These tokens are used to construct a sparse feature matrix using TF-IDF (Term Frequency–Inverse Document Frequency) vectorization. This allows the model to recognize meaningful patterns in the structure of the URL.

7. Model Training
A Random Forest classifier is trained in a multi-label setting using One-vs-Rest classification. This setup enables the model to predict multiple categories for a single URL path (e.g., both "about" and "service"). The dataset is split into training and validation sets to evaluate the performance and generalizability of the model.

8. Model Evaluation
The trained model is evaluated using standard classification metrics such as precision, recall, and F1-score for each category. This provides insight into how well the model performs across each label and identifies potential areas for improvement.

9. Model Saving
Once training and evaluation are complete, the trained model and TF-IDF vectorizer are serialized and saved using joblib. These files can be reused in a production or testing environment without needing to retrain the model.

10. Interactive URL Testing
A simple user interface is built using interactive widgets that allows users to input a URL and immediately receive predictions. When a user enters a URL, the system extracts the path, vectorizes it using the saved TF-IDF model, and predicts which categories it likely belongs to. This makes the model easy to test and demonstrates real-time inference capabilities.

