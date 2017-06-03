---
layout: post
title: Machine Learning DataQuest (in progress!)
---

# Table of Contents
  * [Chapter 1 - Machine Learning](#chapter-1)

* Link
    * https://www.dataquest.io/subject/lessons
    * Solutions https://github.com/dataquestio/solutions
    * New Content Roadmap https://trello.com/b/uPWIcFqF/dataquest-content-roadmap

## Chapter 1 - K-Nearest Neighbors using Regression <a id="chapter-1"></a>

* Project - AirBnB
    * STEP 1: Problem definition
        * Given **Core** product (i.e. AirBnB) is marketplace
        for renting from others where renters search and select
        from listings and may filter by price, qty bedrooms, etc.
        Host price is linked to dynamics of marketplace
        * Goal:
            * Use data (on local listings)
            to predict optimal value (price) to use
            using **Machine Learning**
            * Determining optimal rental price without being:
                * Above market price (renters select cheaper alternatives)
                * Below market price (miss potential revenue)
        * Strategy
            * Find similar listings (1-3)
            * Average list price calculate
            * Set our list price to the average
        * Process and Technique
            * **Machine Learning - K-Nearest Neighbors**
                * Dfn: used to find patterns in
                existing data to make a prediction
            * **Machine Learning Model** is a function that outputs
            prediction based on the input to the model

    * STEP 2: Evaluate Raw Data
        * Find relevant data
            * Primary: Check if data (i.e. on listings in marketplace)
            from **Core** product (i.e. AirBnB) is available
            * Secondary: Check elsewhere for datasets that extract
            data on samples of listings
            (i.e. of a major cities on AirBnB website)
                * Example Data provider http://insideairbnb.com/get-the-data.html
                * Sample Dataset (in .csv.gz format) http://data.insideairbnb.com/united-states/dc/washington-dc/2015-10-03/data/listings.csv.gz
        * Filter only relevant data columns (so manageable)
            * Cleanse
            * Aggregate
            * Remove unnecessary columns
    * STEP 3: Import Data Set into Python using Pandas
        * Unzip .gz gzip data `gunzip ___.csv.gz` OR `gzip -d ___.csv.gz`
        * Switch to Anaconda dev env with data science libraries installed
        `source activate aind` or `pip3 install pandas`
        * Open Python interpreter `python3`
        * Import into Pandas to familiarise
            * Ref:
                * Pandas Dataframe.iloc http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.iloc.html
                * Pandas read_csv http://pandas.pydata.org/pandas-docs/stable/generated/pandas.read_csv.html
            ```
            import pandas as pd

            # Read dataset (.csv format) into a Dataframe of Pandas
            # Reference: http://pandas.pydata.org/pandas-docs/version/0.17.0/generated/pandas.DataFrame.iloc.html

            df1 = pd.read_csv('listings.csv', header=0, nrows=1)

            # OR df1 = pd.read_csv('http://www.____/listings.csv', header=0, nrows=1)

            # Show dataset (first row)
            df1.head()   # OR print(df)

            # ALTERNATIVE (using `from_csv`)
            # Note `from_csv` uses first column as the index
            df2 = pd.DataFrame.from_csv('listings.csv', header=0)

            # Show first two rows (i.e. `0:2`) with first column (i.e. `0`)
            # using slicing
            df2.iloc[0:2, 0]

            # Show first two rows with "name" column
            df2.iloc[0:2]["name"]
            ```
        * Answer:
            ```
            import pandas as pd

            dc_listings = pd.read_csv('dc_airbnb.csv', header=0)
            print(dc_listings.iloc[0:1])
            ```
    * STEP 4: Build **Machine Learning Model** by Apply Strategy of K-Nearest Neighbors Algorithm and
    Rank the existing living spaces by similarity (i.e. by ascending distance values)
        * Apply strategy of K-Nearest Neighbor to:
            * Select the number (i.e. **k**) of
            similar listings to compare with
            (i.e. scatter plot for k = 3 the Price vs Qty Bedrooms)
            * Calculate similarity of each listing in
            dataset (i.e. table with Price vs Qty Bedrooms)
            with our proposed unpriced listing
            (i.e. table with Price ?, Bedrooms 1)
            * **Rank** (order) each listing in dataset by a
            **Metric (Similarity)**
            (i.e. 3 Bedrooms lower rank than 1 Bedroom? OR ...).
            * Select the first **k** listings from ranked/ordered list
            * Calculate mean/average rental list price
            (of the **k** similar listings).
            * Set our listing price to be the average price found
        * TODO - Define:
            * Optimal **k** value
                * How to choose **Optimal** value of **k**?
        * Assumptions:
            * Use `k = 5` value for this example
        * **Metric (Similarity)** Setup
            * Compares fixed set of numerical **features**
            (aka attributes/columns in dataset)
            between 2x **observations** (i.e. rental living spaces)
            * Predict **continuous** value (i.e. price) using the
            **Metric (Similarity)** of **Euclidean Distance** with
            general formula (to break down the Euclidean Distance between
            two observations in a dataset using only specific columns)
                ```
                d = sqrt( (q1-p1)^2 + (q2-p2)^2 + ... + (qn-pn)^2 )

                where q1 to qn - represents a feature(table column) values for Observation P
                where p1 to pn - represents a feature(table column) values for Observation Q
                ```
            * **Optimal Value of Euclidean Distance**
                * Lowest value possible is 0 (since takes absolute value),
                which occurs when value of "feature" is exactly the same between
                both data sets (i.e. when q1 == p1), so the
                closer the output value is the closer the living spaces
        * Simple **Machine Learning Workflow** may use just one **feature**
        (**Univariate case**)
            ```
            d = sqrt( (q1-p1)^2 )
            ```
        * Create **Metric (Similarity)** by calculating the
        **Euclidean Distance** for the **Univariate case**
        of just the `accommodates` **feature**, of just the first
        living space in data set (i.e. first row) and our own
        living space (i.e. accommodates 1-3 bedrooms, unpriced listing)
            * Answer:
                ```
                import numpy as np
                import math

                # Fetch dataset
                dc_listings = pd.read_csv('dc_airbnb.csv', header=0)

                # Access first row of DataFrame as Series from given data set
                # and access specific "feature" (column) value
                first_dc_listing_feature_accommodates = dc_listings.iloc[0:1]["accommodates"][0]
                print(first_dc_listing_feature_accommodates)
                my_listing_feature_accommodates = 3
                first_distance = math.sqrt(abs(first_dc_listing_feature_accommodates - my_listing_feature_accommodates)**2)
                print(first_distance)
                ```

        * Create **Metric (Similarity)** by calculating the
        **Euclidean Distance** for the **Univariate case**
        of just the `accommodates` **feature**, of ALL the
        living space in data set (i.e. ALL rows) and our own
        living space (i.e. accommodates up to 3 bedrooms, unpriced listing)
            * Answer (note that below only takes Absolute value):
                ```
                import numpy as np
                import math

                # Fetch dataset
                dc_listings = pd.read_csv('dc_airbnb.csv', header=0)
                my_accom = 3
                dc_listings["distance"] = dc_listings["accommodates"].apply(lambda x: abs(x - my_accom))
                print(dc_listings["distance"].value_counts())
                ```

    * STEP 5 - Randomising and Sorting
        * Answer:
            ```
            import numpy as np
            import math
            np.random.seed(1)

            def calc_euclidean_dist(val1, val2):
                return int(math.sqrt(abs(val1 - val2)**2))

            # Fetch dataset
            dc_listings = pd.read_csv('dc_airbnb.csv', header=0)
            my_accom = 3
            dc_listings["distance"] = dc_listings["accommodates"].apply(lambda x: calc_euclidean_dist(x, my_accom))
            dc_listings = dc_listings.loc[np.random.permutation(len(dc_listings))]
            dc_listings = dc_listings.sort_values("distance")
            print(dc_listings.iloc[0:10]["price"])
            ```
    * STEP 6 - Average Price
        * Answer:
            ```
            import numpy as np
            import math

            np.random.seed(1)

            def calc_euclidean_dist(val1, val2):
                return int(math.sqrt(abs(val1 - val2)**2))

            # Fetch dataset
            dc_listings = pd.read_csv('dc_airbnb.csv', header=0)
            my_accom = 3
            dc_listings["distance"] = dc_listings["accommodates"].apply(lambda x: calc_euclidean_dist(x, my_accom))
            dc_listings = dc_listings.loc[np.random.permutation(len(dc_listings))]
            dc_listings = dc_listings.sort_values("distance")
            #print(dc_listings.iloc[0:10]["price"])

            def clean_price(dataframe):

                def replace_bad_chars(row):
                    row = row.replace(",", "")
                    row = row.replace("$", "")
                    row = float(row)
                    return row

                dataframe["price"] = dataframe["price"].apply(lambda row: replace_bad_chars(row))

                return dataframe

            dc_listings = clean_price(dc_listings)
            mean_price = dc_listings.iloc[0:5]["price"].mean()
            print(mean_price)
            ```
    * STEP 7 - Function to Predict
        * Answer:
            ```
            import numpy as np
            import math

            # Brought along the changes we made to the `dc_listings` Dataframe.
            dc_listings = pd.read_csv('dc_airbnb.csv')
            stripped_commas = dc_listings['price'].str.replace(',', '')
            stripped_dollars = stripped_commas.str.replace('$', '')
            dc_listings['price'] = stripped_dollars.astype('float')
            dc_listings = dc_listings.loc[np.random.permutation(len(dc_listings))]

            def calc_euclidean_dist(val1, val2):
                return int(math.sqrt(abs(val1 - val2)**2))

            def compare_observations(obs1, obs2):
                return obs2.apply(lambda x: calc_euclidean_dist(x, obs1))

            def sort_dataframe_by_feature(dataframe, feature):
                return dataframe.sort_values(feature)

            def predict_price(new_listing):
                temp_df = dc_listings
                temp_df["distance"] = compare_observations(new_listing, dc_listings["accommodates"])
                # Sort temp_df by distance column
                temp_df = sort_dataframe_by_feature(temp_df, "distance")

                # Select first 5 values in price column.
                # Calculate the mean of these 5 values and use that as the return value for the entire predict_price function.
                mean_price = temp_df.iloc[0:5]["price"].mean()

                ## Complete the function.
                return mean_price

            # Note: Assume "price" column already cleaned
            acc_four = predict_price(4)
            acc_five = predict_price(5)
            acc_six = predict_price(6)
            ```
    * STEP 8: Evaluate a **Machine Learning Model's** Performance (test quality of model)
        * Steps
            * Split dataset into 2x partitions
                * Training Set - contains majority of rows (75%)
                * Test Set - contains remaining minority of rows (25%)
            * **Train/Test Validation** Process (used to "understand" the validation process,
            but it's not the most robust process available
                * Dfn:
                    * Training Set used to make predictions
                    * Test Set used to predict values for
                * Purpose of Validation:
                    * Ensure Machine Learning Model makes good predictions on new data
                    by selecting an **Error Metric**
                * Use Training Set rows to predict "price" value
                for rows in Test Set
                    * Add "predicted_price" column to Test Set
                * Compare "predicted_price values in Test Set with Actual Test Set values
                to check accuracy of predicted values

        * Answer:
            ```
            import pandas as pd
            import numpy as np
            dc_listings = pd.read_csv("dc_airbnb.csv")
            stripped_commas = dc_listings['price'].str.replace(',', '')
            stripped_dollars = stripped_commas.str.replace('$', '')
            dc_listings['price'] = stripped_dollars.astype('float')
            train_df = dc_listings.iloc[0:2792]
            test_df = dc_listings.iloc[2792:]

            def predict_price(new_listing):
                temp_df = train_df
                temp_df['distance'] = temp_df['accommodates'].apply(lambda x: np.abs(x - new_listing))
                temp_df = temp_df.sort_values('distance')
                nearest_neighbor_prices = temp_df.iloc[0:5]['price']
                predicted_price = nearest_neighbor_prices.mean()
                return(predicted_price)

            test_df["predicted_price"] = test_df['accommodates'].apply(lambda x: predict_price(x))
            ```

    * STEP 9: Error Metrics
        * Dfn: **Error Metric** quantifies level of inaccuracy between
        predicted values and actual values (i.e. price)
        for rental listing living spaces in test dataset
        * **Mean Error** (NOT EFFECTIVE) involves calc difference between
        each predicted and actual value and then averaging differences
        (NOT EFFECTIVE, since treats positive difference differently from
        negative difference, whereas we want in both directions)
        * **Mean Absolute Error (MAE)** sums the absolute value of each difference
        between predicted and actual value divided by total count of values
        to get the mean
            ```
            MAE = |(actual1 - predicted1)| + ... + |(actualn - predictedn)| / n
            ```

            * Answer:
                ```
                import pandas as pd
                import numpy as np
                dc_listings = pd.read_csv("dc_airbnb.csv")
                stripped_commas = dc_listings['price'].str.replace(',', '')
                stripped_dollars = stripped_commas.str.replace('$', '')
                dc_listings['price'] = stripped_dollars.astype('float')
                train_df = dc_listings.iloc[0:2792]
                test_df = dc_listings.iloc[2792:]

                def predict_price(new_listing):
                    temp_df = train_df
                    temp_df['distance'] = temp_df['accommodates'].apply(lambda x: np.abs(x - new_listing))
                    temp_df = temp_df.sort_values('distance')
                    nearest_neighbor_prices = temp_df.iloc[0:5]['price']
                    predicted_price = nearest_neighbor_prices.mean()
                    return(predicted_price)

                test_df["predicted_price"] = test_df['accommodates'].apply(lambda x: predict_price(x))
                print(test_df["predicted_price"])

                test_df["diff"] = test_df.apply(lambda x: np.absolute(x['price'] - x['predicted_price']), axis=1)
                mae = test_df["diff"].mean()
                print(mae)
                ```

        * **Mean Square Error (MSE)** sums the squared value of each difference
        between predicted and actual value divided by total count of values
        to get the mean
            * Benefits:
                * Penalises predicted values further from the actual value more
                that those closer to actual value
                * Better than MAE
            ```
            MSE = (actual1 - predicted1)^2 + ... + (actualn - predictedn)^2 / n
            ```
            * Answer:
                ```
                import pandas as pd
                import numpy as np
                dc_listings = pd.read_csv("dc_airbnb.csv")
                stripped_commas = dc_listings['price'].str.replace(',', '')
                stripped_dollars = stripped_commas.str.replace('$', '')
                dc_listings['price'] = stripped_dollars.astype('float')
                train_df = dc_listings.iloc[0:2792]
                test_df = dc_listings.iloc[2792:]

                def predict_price(new_listing):
                    temp_df = train_df
                    temp_df['distance'] = temp_df['accommodates'].apply(lambda x: np.abs(x - new_listing))
                    temp_df = temp_df.sort_values('distance')
                    nearest_neighbor_prices = temp_df.iloc[0:5]['price']
                    predicted_price = nearest_neighbor_prices.mean()
                    return(predicted_price)

                test_df["predicted_price"] = test_df['accommodates'].apply(lambda x: predict_price(x))
                print(test_df["predicted_price"])

                test_df["diff"] = test_df.apply(lambda x: (x['price'] - x['predicted_price'])**2, axis=1)
                mse = test_df["diff"].mean()
                print(mse)
                ```

    * STEP 10: Train Another Model
        * Dfn: So far we only trained Model #1 "accommodates" column
        with MAE and MSE results.
        Training another Model #2 "bathrooms" column and then comparing its
        MSE with Model #1 to see better performance on relative basis.
        Lower the MSE (better) means gap between actual and predicted "price"
        is lower, so:
            * **Model with lower MSE is better predictor of price on a relative basis**
            (i.e. "predicted price" relative to "bathrooms" or
            "predicted price" relative to "accommodation"
            (**but does not determine if model performance good enough in general,
            since MSE metric units are squared** so use RMSE instead,
            i.e. $ squared, so does not
            give intuitive sense of how far off the model predicted prices are
            from the actual price)
        * Answer:
            ```
            train_df = dc_listings.iloc[0:2792]
            test_df = dc_listings.iloc[2792:]

            def predict_price(new_listing):
                temp_df = dc_listings
                temp_df['distance'] = temp_df['bathrooms'].apply(lambda x: np.abs(x - new_listing))
                temp_df = temp_df.sort_values('distance')
                nearest_neighbors_prices = temp_df.iloc[0:5]['price']
                predicted_price = nearest_neighbors_prices.mean()
                return(predicted_price)

            test_df['predicted_price'] = test_df['bathrooms'].apply(lambda x: predict_price(x))
            test_df['squared_error'] = (test_df['predicted_price'] - test_df['price'])**(2)
            mse = test_df['squared_error'].mean()
            print(mse)
            ```

    * STEP 11: Root Mean Squared Error (RMSE)
        * Dfn:
            * Error metric with units matching base unit
            of the target feature/column (i.e. "price" dollars)  since
            is calculated as sqrt of MSE:
                ```
                RMSE = sqrt(MSE)
                ```
        * Benefit:
            * RMSE value for a model is a reflection of the
            predicted value
            (i.e. if model RMSE is >100 then expect predicted price value
            to be off by $100)

    * STEP 10: Compare MAE, MSE, RMSE
        https://medium.com/human-in-a-machine-world/mae-and-rmse-which-metric-is-better-e60ac3bde13d
        * MSE - linear growth in individual errors (i.e. prediction off by $10 has
        10x higher error than a prediction that's off by $1)
        * RMSE - quadratic growth in individual errors (so different
        effect on final RMSE value than individual errors in MSE do)
        * RMSE - penalises large errors more than MAE
        * MAE is better than RMSE from interpretation standpoint since RMSE does not describe
        average error alone
        * RMSE has advantage over MAE of not undesirably avoiding taking absolute value
        * RMSE increases with variance of the frequency distribution of error magnitudes
        * RMSE gives high weight to large errors so more useful when large errors undesireable
        * Answer:
            ```
            errors_one = pd.Series([5, 10, 5, 10, 5, 10, 5, 10, 5, 10, 5, 10, 5, 10, 5, 10, 5, 10])
            errors_two = pd.Series([5, 10, 5, 10, 5, 10, 5, 10, 5, 10, 5, 10, 5, 10, 5, 10, 5, 1000])
            mae_one = errors_one.sum()/len(errors_one)
            rmse_one = np.sqrt((errors_one**2).sum())/(len(errors_one))
            print(mae_one)
            print(rmse_one)

            MAE_ONE is 7.5
            RMSE_ONE is 1.86338998125

            mae_two = errors_two.sum()/len(errors_two)
            rmse_two = np.sqrt((errors_two**2).sum())/(len(errors_two))
            print(mae_two)
            print(rmse_two)

            MAE_TWO is 62.5
            RMSE_TWO is 55.5840204855

             Note: TWO
                 * MAE to RMSE ratio of 1:1
            ```
            * Note:
                * Errors One
                    * MAE to RMSE ratio of 4:1
                * Errors Two
                    * MAE to RMSE ratio of 1:1
                * **Outliers**
                    * Note that only difference is one extreme value of 1000 in errors_two instead of 10
                * **Expected Results**
                    * Expected the RMSE value to be much less than MAE in general since
                    RMSE takes sqrt of the squared errors.
        * **Comparing the MAE to RMSE Ratio helps understand if "outliers" exist that
        cause large but infrequent errors. Always expect the ratio to be ~4:1**
            * https://medium.com/human-in-a-machine-world/mae-and-rmse-which-metric-is-better-e60ac3bde13d

    * STEP 11:
        * Issue comparing just two columns:
            * **Comparing just two columns (i.e. "accommodates" and
            "bathrooms") indicates that the "bathrooms" column is more
            accurate), but is still not sufficient enough to predict
            the best model features to obtain the most accurate output, since
            there are other important factors such as "crime" in area, etc**
        * Improve accuracy of model options (i.e. decrease RMSE during validation)
            * Increase qty attributes/features (i.e. columns) the
            model uses to calculate "Similarity"
            when ranking **nearest-neighbors**
                * **Warning** (ensure column data type normalised
                for input to Euclidean Distance equation)
                    * Convert **non-numeric** values to numeric
                    (i.e. city/state)
                    * Substitute value for **missing values**
                    * Convert **non-ordinal** values to ordinal
                    (i.e. latitude/longtitude to low or high)
                * STEP 1: Drop columns temporarily if they contain
                    **non-numeric**, or **non-ordinal**, or data not entirely
                     relevant (i.e. host ratings that aren't specific to
                     describing the living space or listing itself)
                    and retain the Remaining Columns
                * STEP 2: Show any columns that have any rows that contain NaN values
                    ```
                    space                            20.04
                    description                       0.03
                    neighborhood_overview            33.68
                    notes                            54.02
                    transit                          30.49
                    thumbnail_url                     0.89
                    medium_url                        0.89
                    xl_picture_url                    0.89
                    host_location                     0.16
                    host_about                       25.79
                    host_response_time               11.66
                    host_response_rate               11.66
                    host_acceptance_rate             16.49
                    host_neighbourhood                7.04
                    neighbourhood                     9.48
                    neighbourhood_group_cleansed    100.00
                    zipcode                           0.24
                    property_type                     0.03
                    bathrooms                         0.73
                    bedrooms                          0.56
                    beds                              0.30
                    square_feet                      97.80
                    weekly_price                     42.95
                    monthly_price                    51.46
                    security_deposit                 61.70
                    cleaning_fee                     37.28
                    first_review                     22.29
                    last_review                      22.29
                    review_scores_rating             23.31
                    review_scores_accuracy           23.50
                    review_scores_cleanliness        23.53
                    review_scores_checkin            23.53
                    review_scores_communication      23.42
                    review_scores_location           23.42
                    review_scores_value              23.42
                    license                          99.97
                    jurisdiction_names                0.70
                    reviews_per_month                22.29
                    ```
                * STEP 3: fix any **missing values**
                    * Cols with large >= 30% missing rows
                        * Remove whole column (since removing missing rows
                        loses too many observations from
                        dataset)
                    * Cols with small < 1% missing rows
                        * Retain column, but remove missing rows/observation
            * Increase `k` (qty of **nearest-neighbors** the model uses
            to compute the prediction)

        * Answers:
            * Part 2
                ```
                remove_non_numeric_columns = ["room_type", "city", "state"]
                remove_non_ordinal_columns = ["latitude", "longitude", "zipcode"]
                remove_out_of_scope_columns = ["host_response_rate", "host_acceptance_rate", "host_listings_count"]
                remove_columns = remove_non_numeric_columns + remove_non_ordinal_columns + remove_out_of_scope_columns
                # Return new object with labels in requested axis removed (i.e. axis=1 asks Pandas to drop across DataFrame columns)
                dc_listings.drop(remove_columns, axis=1, inplace=True)
                print(dc_listings)
                ```
            * Part 3
                ```
                remove_high_missing_columns = ["cleaning_fee", "security_deposit"]
                dc_listings.drop(remove_high_missing_columns, axis=1, inplace=True)

                retain_column_remove_missing_rows = ["bedrooms", "bathrooms", "beds"]
                dc_listings.dropna(axis=0, how="any", subset=retain_column_remove_missing_rows, inplace=True)
                print(dc_listings)
                ```

    * STEP 12: Normalise Columns
        * **Issue** Since in the first few Rows
            some Columns the values vary between say 0 and 12,
            whereas in other Columns they vary say 4 and 1825 .
            If these Columns are used in
            **K-Nearest-Neighbors Model** the attributes could
            have **outsized effect** on the
            **Euclidean Distance** calculations due to the size
            of the values
                * i.e. if observations of two living spaces
                 had same across all attributes except one had
                 `maximum_nights` of 1825, whilst others had 4,
                 then the overall Euclidean distance would be
                 impacted by the **outsized effect**.
         * **Solution**
            https://en.wikipedia.org/wiki/Normal_distribution#Standard_normal_distribution
            * **Normalise** all the columns to have
            a mean of 0 and standard deviation of 1, in order
            to prevent any single column
            having too much impact on the distance calculation
            (preserve distribution of values whilst
            aligning scales).
            Normalise values in a column to the
            **Standard Normal Distribution** by:
                * Subtracting mean of the Col from each value
                * Divide each value by Col's std deviation
                    ```
                    i.e. col_val = (col_val - col_mean) / col_std_dev
                    ```
            * Answer: Part 4
                ```
                normalized_listings = (dc_listings - dc_listings.mean())/(dc_listings.std())
                normalized_listings['price'] = dc_listings['p
                ```

    * STEP 13: Train a **Multivariate K-Nearest Neighbors Model**
        * Use TWO attributes **both** "accommodates" and "bathrooms"
        (**Multivariate Case**) to determining similarity between
        2 living spaces
        * Euclidean Distance Equation between 2 living spaces
         using 2x attributes:
            ```
            d = sqrt( (accom1-accom2)^2 - (bath1-bath2)^2 )
            ```

        * **SciPy** https://docs.scipy.org/doc/scipy-0.14.0/reference/generated/scipy.spatial.distance.euclidean.html
        has a function **scipy.spatial.distance.euclidean**
        to calculate the **Euclidean distance (between two rows/observations in dataset)**
        between two 1-dimensional
        (array-like object) vectors U and V represented using
        **list-like** object (i.e. Python list, NumPy array, or Pandas Series)
        where each Vector has same qty elements
            * Answer Part 5
                ```
                from scipy.spatial import distance

                # Create Series object by selecting Row 0 of Cols "accommodates" and "bathrooms"
                # Series object required to be passed to SciPy euclidean() function
                first_listing = normalized_listings.iloc[0][['accommodates', 'bathrooms']]
                fifth_listing = normalized_listings.iloc[4][['accommodates', 'bathrooms']]

                # Calculate Euclidean distance between Rows 0 and 5 using
                # the SciPy distance.euclidean() function
                first_fifth_distance = distance.euclidean(first_listing, fifth_listing)
                print(first_fifth_distance)
                ```
    * STEP 14: **Scikit-Learn**
        * Dfn: Replace manual calculation of **K-Nearest-Neighbors** Models
        with more productive **Scikit-Learn** Python Machine Learning library
        http://scikit-learn.org/ to handle implementation of algorithms for
        Data Scientists for Training and Testing different models on new datasets.
        * **Scikit-Learn** Workflow** Steps:
            * 1 Instantiate specific Machine Learning Model to use
                * Identify Scikit-Learn Model Class to create Instance of
                (i.e. KNeighborsRegressor class of type
                sklearn.neighbors **Nearest Neighbors**)
                http://scikit-learn.org/dev/modules/classes.html#module-sklearn.neighbors
                http://scikit-learn.org/stable/modules/generated/sklearn.neighbors.KNeighborsRegressor.html#sklearn.neighbors.KNeighborsRegressor
                * Implement Scikit-Learn Model as separate Class http://scikit-learn.org/dev/modules/classes.html
                * Model types used include:
                    * **Regression Model Class** (i.e. listing "price") help predict numerical values `KNeighborsRegressor`
                    * **Classification Model Class** help predict a label from fixed set of labels
                * Instantiate an empty Model calling the constructor where according to the documentation its defaults are
                    * where `algorithm` is used to compute **Nearest Neighbors**
                        * If `auto` is used, Scikit-Learn tries **Tree-based Optimisations** instead
                    * where `p` corresponds to Euclidean distance
                            `(n_neighbors=5,
                              weights='uniform',
                              algorithm='auto',     # OR ball_tree, kd_tree, brute
                              leaf_size=30,         #
                              p=2,
                              ...)`
                    * Implementation:
                            ```
                            from sklearn.neighbors import KNeighborsRegressor
                            knn = KNeighborsRegressor(n_neighbors=5, algorithm="brute", p=2)
                            ```
            * 2 Fit the Model to Training data
                * Fit the Model to the data using the `fit` method http://scikit-learn.org/stable/modules/generated/sklearn.neighbors.KNeighborsRegressor.html#sklearn.neighbors.KNeighborsRegressor.fit
                    ```
                    >>> X = [[0], [1], [2], [3]]
                    >>> y = [0, 0, 1, 1]
                    >>> from sklearn.neighbors import KNeighborsRegressor
                    >>> neigh = KNeighborsRegressor(n_neighbors=2)
                    >>> # Fit the model using X as training data and y as target values
                    >>> #   where X is matrix-like object containing DataFrame "feature" columns to use from Training dataset
                    >>> #     (i.e. "matrix-like" means:
                    >>> #           flexible method input accepting either a DataFrame or NumPy 2D array of values)
                    >>> #   where y is list-like object containing Target values (array of Float values)
                    >>> #     so for this we select the Target Column from the DataFrame
                    >>> #     (i.e. "list-like" means:
                    >>> #           flexible method input accepts NumPy array, Python list, or Panda Series object (e.g. when selecting a column))
                    >>> neigh.fit(X, y)
                    KNeighborsRegressor(...)
                    >>> print(neigh.predict([[1.5]]))
                    [ 0.5]
                    ```
                * E.g.
                    ```
                    # Split full dataset into Train and Test sets.
                    train_df = normalized_listings.iloc[0:2792]
                    test_df = normalized_listings.iloc[2792:]

                    # Matrix-like object, containing just 2 columns of interest from Training set.
                    train_features = train_df['accommodates', 'bathrooms']

                    # List-like object, containing just Target Column, `price`.
                    train_target = normalized_listings['price']

                    # Pass everything into fit method.
                    # Scikit-Learn stores the Train data (to use to make Predictions) within the KNearestNeighbors instance `knn`
                    # Warning: DO NOT pass in data containing the following else Error occurs:
                    #   - Missing values
                    #   - Non-numerical values
                    knn.fit(train_features, train_target)
                    ```

            * 3 Use Model to make Predictions
                * Scikit-Learn's `predict` method makes Predictions of the Target based on the provided
                Training data stored in the KNearestNeighbors instance
                    http://scikit-learn.org/stable/modules/generated/sklearn.neighbors.KNeighborsRegressor.html#sklearn.neighbors.KNeighborsRegressor.predict
                    * `predict` has one required parameter X, which is
                        * Parameters - Matrix-like object, containing "feature" Columns (Test samples)
                        from dataset to make Predictions on
                        * Returns - NumPy Array of int, representing Target values
                        (i.e. Predicted "price" values for the Test set)
                        ```
                        predictions = knn.predict(test_df['accommodates', 'bathrooms'])
                        ```
                    * **Important** Ensure qty "feature" Columns used during Training and Testing match else
                    Scikit-Learn returns Error
                * **Calculate MSE using Scikit-Learn** instead
                    * **Previously** done manually with Pandas arithmetic operators
                    to compare each predicted value with actual value from `price` column of Test set
                    * **Instead** use Scikit-Learn's MSE Regression Loss
                    `sklearn.metrics.mean_squared_error(y_true, y_pred, sample_weight=None, multioutput='uniform_average')`
                    http://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_squared_error.html#sklearn.metrics.mean_squared_error
                        * `mean_squared_error()` function accepts 2 inputs:
                            * List-like object representing True (actual) values
                            * List-like object representing Predicted values using the Model
            * 4 Evaluate accuracy of Predictions

        * Answer Part 7
            ```
            from sklearn.neighbors import KNeighborsRegressor

            # Split full dataset into Training and Testing sets
            train_df = normalized_listings.iloc[0:2792]
            test_df = normalized_listings.iloc[2792:]

            """
            Scikit-Learn Workflow
            """
            # Step 1: Create instance of K-Nearest-Neighbors Machine Learning Model class
            knn = KNeighborsRegressor(n_neighbors=5, algorithm='brute', p=2)

            # Step 2: Fit the Model using by specifying data for K-Nearest-Neighbor Model to use:
            #   - X as Training data (i.e. DataFrame "feature" Columns from Training data)
            #   - y as Target values (i.e. DataFrame's Target Column)

            # X for `fit` function is matrix-like object, containing
            # just 2 columns of interest from Training set (to use to make Predictions).
            train_columns = ['accommodates', 'bathrooms']
            X = train_df[train_columns]

            # y for `fit` function is list-like object, containing
            # just Target Column, `price`.
            train_target = 'price'
            y = train_df[train_target]

            # X and y are passed into `fit` method of Scikit-Learn.
            # Warning: DO NOT pass in data containing the following else Error occurs:
            #   - Missing values
            #   - Non-numerical values
            knn.fit(X, y)

            # Scikit-Learn's `predict` function called to make Predictions on
            # the 2 Columns of test_df that returns a NumPy array of Predicted "price" values
            predictions = knn.predict(test_df[train_columns])

            print(predictions)

            ```
        * Answer Part 8
            ```
            from sklearn.metrics import mean_squared_error
            import math

            train_columns = ['accommodates', 'bathrooms']
            knn = KNeighborsRegressor(n_neighbors=5, algorithm='brute', metric='euclidean')
            knn.fit(train_df[train_columns], train_df['price'])
            predictions = knn.predict(test_df[train_columns])

            # Calculate MSE float values for each individual Target, where least loss "best" values are 0
            two_features_mse = mean_squared_error(test_df['price'], predictions, multioutput='raw_values')

            # Calculate RMSE
            two_features_rmse = math.sqrt(two_features_mse)
            print(two_features_mse)
            print(two_features_rmse)
            ```
        * Answer Part 9
            ```
            import math
            train_columns = ['accommodates', 'bedrooms', 'bathrooms', 'number_of_reviews']
            from sklearn.neighbors import KNeighborsRegressor
            knn = KNeighborsRegressor(n_neighbors=5, algorithm='brute')

            knn.fit(train_df[train_columns], train_df['price'])
            four_predictions = knn.predict(test_df[train_columns])

            # Calculate MSE float values for each individual Target, where least loss "best" values are 0
            four_mse = mean_squared_error(test_df['price'], four_predictions, multioutput='raw_values')

            # Calculate RMSE
            four_rmse = math.sqrt(four_mse)
            print(four_mse)
            print(four_rmse)
            ```
        * Answer Part 10
            ```
            knn = KNeighborsRegressor(n_neighbors=5, algorithm='brute')

            features = train_df.columns.tolist()
            features.remove('price')

            knn.fit(train_df[features], train_df['price'])
            all_features_predictions = knn.predict(test_df[features])
            all_features_mse = mean_squared_error(test_df['price'], all_features_predictions)
            all_features_rmse = all_features_mse ** (1/2)
            print(all_features_mse)
            print(all_features_rmse)
            ```

    * STEP 15: **FEATURE SELECTION - Exclude Columns from Training that increase RMSE**
        * Using ALL features/columns as Training Columns may actually
        increase the RMSE value (bad as more outlier errors causing
        prediction accuracy of K-Nearest Neighbors Model to decrease
        so **FEATURE SELECTION** should involve:
            * select the relevant attributes the model uses to calculate similarity
            when ranking the closest neighbors

    * STEP 16: **Hyperparameter 'k' Optimisation**
        * i.e. Increase the qty of nearby neighbors `k` that the model uses
        to make predictions.
        * **Hyperparameters** such as `k` are values that may be varied
        and affect the behaviour and performance
        of the model independently of the actual data being used to make
        predictions (impacting how the model performs without changing
        the data that is used).
        * Note: Whereas varying the features used in the model affects the data the model uses
        * **Hyperparameter Optimisation Techniques** is the process of finding the
        optimal Hyperparameter value for the best model
        https://en.wikipedia.org/wiki/Hyperparameter_optimization
            * **Grid Search** https://en.wikipedia.org/wiki/Hyperparameter_optimization#Grid_search
                * Steps
                    * Repeat for different models (i.e. different qty of relevant features)
                        * Select relevant features to use for predicting target column
                        * Use Grid Search to find optimal hyperparameter value for selected features
                            * Select subset of possible **hyperparameter** values
                            * Train model using each **hyperparameter** value
                        * Evaluate each model's performance/accuracy at different **hyperparameter** values
                        (i.e. at different `k` values)
                        * Select **hyperparameter** value resulting in lowest MSE error value
                        * **Note:** For large datasets **Grid Search** can take a long time
                        * **Note:** Increasing `k` value from 1 to 5 caused MSE to fall from 26364 o 14090
                    * Compare the resulting lowest MSE value for each model
                    * Use the model (i.e. combination of features) with the lowest MSE
                * Expanding
                    * Visualise different MSE value when increase `k` value from 1 to 20.
                        * Between `k` 1 to 7 decreases from 26364 to 13657
                        * Between `k` 8 to 20 no further decreases, just rebounds and hovers
                            * Show behaviour in Scatter plot with `k` on x-axis and MSE values on y-axis.
                            http://matplotlib.org/api/pyplot_api.html#matplotlib.pyplot.scatter

                        * Conclude that **Optimal** value of `k` is 6 (since results in lowest MSE value)

        * Observe difference in model performance with different hyperparameter values
        (i.e. try increasing from k=1 to k=5, and use features that resulted in
        best model accuracy "accommodates", "bedrooms", "bathrooms", "number_of_reviews"
            ```
            from sklearn.neighbors import KNeighborsRegressor
            from sklearn.metrics import mean_squared_error

            hyper_params = [1, 2, 3, 4, 5]
            mse_values = []

            training_columns = ["accommodates", "bedrooms", "bathrooms", "number_of_reviews"]
            target_column = "price"

            for index, qty_neighbors in enumerate(hyper_params):
                knn = KNeighborsRegressor(n_neighbors=qty_neighbors, algorithm="brute", p=2)
                X = train_df[training_columns]
                y = train_df[target_column]
                knn.fit(X, y)
                predictions = knn.predict(test_df[training_columns])

                mse = mean_squared_error(test_df[target_column], predictions, multioutput='raw_values')
                mse_values.append(mse)

            print(mse_values)

            # [array([ 26364.92832765]), array([ 15100.52246871]), array([ 14579.59790166]), array([ 16212.30076792]), array([ 14090.0116496])]
            ```
        * Part 3 Change `k` from 1 to 20
            ```
            from sklearn.neighbors import KNeighborsRegressor
            from sklearn.metrics import mean_squared_error
            import numpy

            hyper_params = numpy.arange(1, 21, 1) # 1 to 20
            mse_values = []

            training_columns = ["accommodates", "bedrooms", "bathrooms", "number_of_reviews"]
            target_column = "price"

            for index, qty_neighbors in enumerate(hyper_params):
                knn = KNeighborsRegressor(n_neighbors=qty_neighbors, algorithm="brute", p=2)
                X = train_df[training_columns]
                y = train_df[target_column]
                knn.fit(X, y)
                predictions = knn.predict(test_df[training_columns])

                mse = mean_squared_error(test_df[target_column], predictions, multioutput='raw_values')
                mse_values.append(mse)

            print(mse_values)

            # [array([ 26364.92832765]), array([ 15100.52246871]), array([ 14579.59790166]), array([ 16212.30076792]), array([ 14090.0116496]), array([ 13657.29067122]), array([ 14288.27389659]), array([ 14853.4481833]), array([ 14670.83190775]), array([ 14642.45147895]), array([ 14734.07138089]), array([ 14854.55666951]), array([ 14733.16190399]), array([ 14777.97589445]), array([ 14771.12464669]), array([ 14870.17850985]), array([ 14832.59850963]), array([ 14783.5929683]), array([ 14775.59471699]), array([ 14676.94798635])]
            ```
        * Part 4 plot different MSE for `k` values 1 to 20 (with only 4 features)
            ```
            import matplotlib.pyplot as plt
            %matplotlib inline

            features = ['accommodates', 'bedrooms', 'bathrooms', 'number_of_reviews']
            hyper_params = [x for x in range(1, 21)]
            mse_values = list()

            for hp in hyper_params:
                knn = KNeighborsRegressor(n_neighbors=hp, algorithm='brute')
                knn.fit(train_df[features], train_df['price'])
                predictions = knn.predict(test_df[features])
                mse = mean_squared_error(test_df['price'], predictions)
                mse_values.append(mse)

            x = hyper_params
            y = mse_values
            plt.scatter(x=x, y=y, c='b', marker='o')
            plt.show()
            ```

        * Part 5 plot different MSE for `k` values 1 to 20 (with only ALL features)
           ```
           hyper_params = [x for x in range(1,21)]
           mse_values = list()

           # All features except price
           features = train_df.columns.tolist()
           features.remove('price')

           for hp in hyper_params:
               knn = KNeighborsRegressor(n_neighbors=hp, algorithm='brute')
               knn.fit(train_df[features], train_df['price'])
               predictions = knn.predict(test_df[features])
               mse = mean_squared_error(test_df['price'], predictions)
               mse_values.append(mse)

           x = hyper_params
           y = mse_values
           plt.scatter(x=x, y=y, c='b', marker='o')
           plt.show()
           ```
       * Part 6 find best MSE out of all the `k` values 1 to 20 when using two features, three features, and more
            ```
            two_features = ['accommodates', 'bathrooms']
            three_features = ['accommodates', 'bathrooms', 'bedrooms']
            hyper_params = [x for x in range(1,21)]
            # Append the first model's MSE values to this list.
            two_mse_values = list()
            # Append the second model's MSE values to this list.
            three_mse_values = list()
            two_hyp_mse = dict()
            three_hyp_mse = dict()
            for hp in hyper_params:
                knn = KNeighborsRegressor(n_neighbors=hp, algorithm='brute')
                knn.fit(train_df[two_features], train_df['price'])
                predictions = knn.predict(test_df[two_features])
                mse = mean_squared_error(test_df['price'], predictions)
                two_mse_values.append(mse)

            two_lowest_mse = two_mse_values[0]
            two_lowest_k = 1

            for k,mse in enumerate(two_mse_values):
                if mse < two_lowest_mse:
                    two_lowest_mse = mse
                    two_lowest_k = k + 1

            for hp in hyper_params:
                knn = KNeighborsRegressor(n_neighbors=hp, algorithm='brute')
                knn.fit(train_df[three_features], train_df['price'])
                predictions = knn.predict(test_df[three_features])
                mse = mean_squared_error(test_df['price'], predictions)
                three_mse_values.append(mse)

            three_lowest_mse = three_mse_values[0]
            three_lowest_k = 1

            for k,mse in enumerate(three_mse_values):
                if mse < three_lowest_mse:
                    three_lowest_mse = mse
                    three_lowest_k = k + 1

            two_hyp_mse[two_lowest_k] = two_lowest_mse
            three_hyp_mse[three_lowest_k] = three_lowest_mse

            print(two_hyp_mse)
            print(three_hyp_mse)
            ```

    * STEP 17: **Cross Validation**
        * Previously Train/Test Validation technique tested the
        ML model's accuracy on new data that model was not trained on
        * Other techniques are more robust:
            * **Holdout Validation** technique
                * About
                    * Holdout Validation is a K-fold Cross-Validation Technique
                    * Holdout Validation holdout validation is better than
                    Train/Test Validation because the model isn't repeatedly
                    biased towards a specific subset of the data, both models
                    that are trained only use half the available data
                    * Holdout validation is essentially a version of K-Fold
                    Cross Validation when k (qty of partitions) is equal to 2.
                    Generally, 5 or 10 folds is used for k-fold cross-validation
                * Process
                    * Split full dataset into 2x parts, usually 50%/50%
                    (instead of 75%/25% of the Train/Test Validation technique)
                    to remove some observations as potential sources of variation
                    in the model performance:
                        * Training set
                        * Test set
                    * Repeat section
                        * Train a Model #1 (k-nearest neighbors) on Training set
                        * Test the Trained Model #1 on the Test set
                        to predict labels on the Test set
                        * Error Metric computed to understand model's effectiveness
                    * Switch Training and Test sets and repeat the Repeat section
                    * Calc Average of the two errors
            * **K-Fold Cross Validation**
                * Dfn:
                    * K-fold cross validation, takes advantage of a larger
                    proportion of the data during training while still rotating
                    through different subsets of the data to avoid the issues of
                    Train/Test Validation
                    * Use 5 to 10 K-Folds
                    * Since you're training k models, the more number of folds you use
                    the longer it takes. When working with large datasets,
                    often only a few number of folds are used because of the time
                    and cost it takes, with the tradeoff that having more training
                    examples helps improve the accuracy even with less folds
                * Process:
                    * Assign a new column to the dataset called "fold" that contains the
                    Fold number each row belongs to (where number of folds is k)
                        ```
                        dc_listings.set_value(dc_listings.index[0:744], "fold", 1)
                        dc_listings.set_value(dc_listings.index[744:1488], "fold", 2)
                        dc_listings.set_value(dc_listings.index[1488:2232], "fold", 3)
                        dc_listings.set_value(dc_listings.index[2232:2976], "fold", 4)
                        dc_listings.set_value(dc_listings.index[2976:3722], "fold", 5)
                        ```
                    * splitting the full dataset into k equal length partitions,
                    * selecting k-1 partitions as the training set and
                    selecting the remaining partition as the test set
                        ```
                        # Iteration #1 of the K-Fold Cross Validation

                        from sklearn.neighbors import KNeighborsRegressor
                        from sklearn.metrics import mean_squared_error

                        # Training
                        model = KNeighborsRegressor()

                        # Test set assigned to Fold 1, Train set assigned to other Folds
                        train_iteration_one = dc_listings[dc_listings["fold"] != 1]
                        test_iteration_one = dc_listings[dc_listings["fold"] == 1]
                        model.fit(train_iteration_one[["accommodates"]], train_iteration_one["price"])

                        # Predicting
                        labels = model.predict(test_iteration_one[["accommodates"]])
                        test_iteration_one["predicted_price"] = labels
                        iteration_one_mse = mean_squared_error(test_iteration_one["price"], test_iteration_one["predicted_price"])
                        iteration_one_rmse = iteration_one_mse ** (1/2)
                        ```
                    * training and validating the model on the training set,
                        * using the trained model to predict labels on the test fold,
                        * computing the test fold's error metric,
                    * repeating all of the above steps k-1 times, until each partition has been used as the test set for an iteration,
                    * calculating the mean of the k error values.
                        ```
                        # Use np.mean to calculate the mean.
                        import numpy as np
                        fold_ids = [1,2,3,4,5]
                        def train_and_validate(df, folds):
                            fold_rmses = []
                            for fold in folds:
                                # Train
                                model = KNeighborsRegressor()
                                train = dc_listings[dc_listings["fold"] != fold]
                                test = dc_listings[dc_listings["fold"] == fold]
                                model.fit(train[["accommodates"]], train["price"])
                                # Predict
                                labels = model.predict(test[["accommodates"]])
                                test["predicted_price"] = labels
                                mse = mean_squared_error(test["price"], test["predicted_price"])
                                rmse = mse**(1/2)
                                fold_rmses.append(rmse)
                            return(fold_rmses)

                        rmses = train_and_validate(dc_listings, fold_ids)
                        print(rmses)
                        avg_rmse = np.mean(rmses)
                        print(avg_rmse)
                        ```
                * Note:
                    * Increasing the number the folds causes number of observations
                    in each fold to decrease and the variance of the fold-by-fold
                    errors to increase. Let's start by manually partitioning the
                    data set into 5 folds. Instead of splitting into 5 dataframes,
                    let's add a column that specifies which fold the row belongs to.
                    This way, we can easily select

            * Solution Part 2
                ```
                from sklearn.neighbors import KNeighborsRegressor
                from sklearn.metrics import mean_squared_error
                import numpy as np
                import math

                train_one = split_one
                test_one = split_two
                train_two = split_two
                test_two = split_one

                training_columns = ["accommodates"]
                target_column = "price"

                def get_rmse(X, y, test_training_cols, test_target_col):
                    knn = KNeighborsRegressor(n_neighbors=5, algorithm='auto', p=2)
                    knn.fit(X, y)
                    predictions = knn.predict(test_training_cols)
                    mse = mean_squared_error(test_target_col, predictions, multioutput='raw_values')
                    return math.sqrt(mse)

                iteration_one_rmse = get_rmse(train_one[training_columns], train_one[target_column], test_one[training_columns], test_one[target_column])
                print(iteration_one_rmse)

                iteration_two_rmse = get_rmse(train_two[training_columns], train_two[target_column], test_two[training_columns], test_two[target_column])

                print(iteration_two_rmse)

                avg_rmse = np.mean([iteration_one_rmse, iteration_two_rmse])
                print(avg_rmse)
                ```

            * Solution Part 5
                ```
                # Use np.mean to calculate the mean.
                import numpy as np
                fold_ids = [1,2,3,4,5]
                def train_and_validate(df, folds):
                    fold_rmses = []
                    for fold in folds:
                        # Train
                        model = KNeighborsRegressor()
                        train = dc_listings[dc_listings["fold"] != fold]
                        test = dc_listings[dc_listings["fold"] == fold]
                        model.fit(train[["accommodates"]], train["price"])
                        # Predict
                        labels = model.predict(test[["accommodates"]])
                        test["predicted_price"] = labels
                        mse = mean_squared_error(test["price"], test["predicted_price"])
                        rmse = mse**(1/2)
                        fold_rmses.append(rmse)
                    return(fold_rmses)

                rmses = train_and_validate(dc_listings, fold_ids)
                print(rmses)
                avg_rmse = np.mean(rmses)
                print(avg_rmse)
                ```


            * **K-Fold Cross Validation with Scikit-Learn**
                * So far we have:
                    * Improved the K-Nearest Neighors model by changing
                    features/columns used to train, or tweaking the `k` neighbors
                    hyperparameter for optimisation
                    * Accurately understood the model's performance using
                    K-Fold Cross Validation with proper quantity of folds.
                * **Scikit-Learn helps quickly experiment with K-Fold Cross Validation**
                    * Instantiate instance of `KFold` class (simply returns iterator object ready
                    for training and testing of models)
                        `kf = KFold(n, n_folds, shuffle=False, random_state=None)`
                        http://scikit-learn.org/stable/modules/generated/sklearn.model_selection.KFold.html
                        http://scikit-learn.org/stable/modules/generated/sklearn.model_selection.cross_val_score.html
                        * where
                            ```
                            n - qty observations in dataset
                            n_folds - qty folds
                            shuffle - shuffle order of observations in dataset
                            random_state - specify seed value if shuffle set to True
                            (value of 8 allows answer check using same seed)
                            ```
                    * Use `cross_val_score` function with `KFold` of Scikit-Learn to
                    perform training, testing, and obtain Error Metrics for each KFold
                        `cross_val_score(estimator, X, Y, scoring=None, cv=None)`
                        http://scikit-learn.org/stable/modules/generated/sklearn.model_selection.cross_val_score.html
                        http://scikit-learn.org/stable/modules/model_evaluation.html#common-cases-predefined-values
                        * where
                            ```
                            estimator - SKLearn model that implements `fit` method (instance of KNeighborsRegressor)
                            X - list or 2D array with features to Train on
                            y - list containing values to Predict (Target column)
                            scoring - string describing Scoring Criteria (i.e. MAE, MSE, etc see predefined
                            values link above) where single total value returned for each fold
                            cv - qty of Folds (provided by instance of KFold class, or integer
                            representing qty of folds in KFolds)
                            ```
                        * Solution
                            ```
                             from sklearn.cross_validation import KFold
                             from sklearn.cross_validation import cross_val_score
                             import numpy as np

                             kf = KFold(n=len(dc_listings), n_folds=5, shuffle=True, random_state=1)
                             model = KNeighborsRegressor(n_neighbors=5, algorithm="auto", p=2)

                             # MSEs for each Fold
                             mses = cross_val_score(model, dc_listings[["accommodates"]], dc_listings["price"], scoring="mean_squared_error", cv=kf, verbose=1)
                             rmses = [np.sqrt(np.absolute(mse)) for mse in mses]
                             avg_rmse = np.mean(rmses)

                             print(rmses)
                             print(avg_rmse)
                            ```

                    * Explore different K Values for `KFold`
                    where if:
                        * `k = 2` means Holdout Validation
                        * `k = n` means Leave-One-Out Cross Validation LOOCV
                        (where n is qty observations in dataset),
                        where a Standard value of 10 is used
                        * Note: when vary value of `k` from 3 to 23
                        and calculate RMSE value across all Folds and
                        Standard Deviation of RMSE values we find:
                            * Average RMSE value is found
                            * Standard Deviation of RMSE value increases as qty Folds increases
                        ```
                        from sklearn.cross_validation import KFold
                        from sklearn.cross_validation import cross_val_score
                        num_folds = [3, 5, 7, 9, 10, 11, 13, 15, 17, 19, 21, 23]

                        for fold in num_folds:
                            kf = KFold(len(dc_listings), fold, shuffle=True, random_state=1)
                            model = KNeighborsRegressor()
                            mses = cross_val_score(model, dc_listings[["accommodates"]], dc_listings["price"], scoring="mean_squared_error", cv=kf)
                            rmses = [np.sqrt(np.absolute(mse)) for mse in mses]
                            avg_rmse = np.mean(rmses)
                            std_rmse = np.std(rmses)
                            print(str(fold), "folds: ", "avg RMSE: ", str(avg_rmse), "std RMSE: ", str(std_rmse))
                        ```
            * **Bias-Variance Tradeoff**
                * Previously we assumed lowest RMSE always more accurate,
                 however model Error causes are **Bias** and **Variance**.
                 Finding **Optimum Model Complexity** with
                 Lowest Error by finding combination with
                 Lowest **Average RMSE value** (Bias) and
                 Lowest **Standard Deviation of RMSE values** (Variance)
                 defined as:
                    * **Bias**
                        * Dfn: Error that results in bad assumptions about the
                        learning algorithm
                            * e.g. assuming on feature #1 (i.e. "accommodations")
                            relates to rental listings "price" leads to a simple
                            univariate regression model **High in Bias**
                            where **Error Rate is High** since "price" is
                            affected by many other factors
                        * Use the **Average RMSE value** as a proxy for a
                        models **Bias**

                    * **Variance**
                        * Dfn: Error that occurs due to variability of a model's
                        predicted values
                            * e.g. given dataset with 1000 features on each
                            rental listing, where all features used to train a
                            complicated multivariate regression model with **Low Bias**
                            and **High Variance** (ideally want Low Variance, but
                            there is a trade-off)
                        * Use the **Standard Deviation of RMSE values** as a proxy for a
                        models **Variance**
                * Note: KNN makes Predictions but is not a Mathematical Model
                since it cannot exist without original data.
                **Linear Regression** is however (an equation that can exist without original data).
                **Bias-Variance Tradeoff** is best applied to Mathematical Models

* Tricks:
    ```

    Numpy
    ======
    np.sqrt vs math.sqrt:
        math.sqrt only works in single values, not DataFrames

    DataFrame stop bounds for slicing subsets of rows and columns
        http://www.datacarpentry.org/python-ecology-lesson/02-index-slice-subset/
        i.e. iloc[row slicing, column slicing]
    ======================
    [:5]            -> first 5 rows
    [-1:]           -> last element in list
    [:]             -> all elements

    loc - works on labels in the index
    iloc - works on the positions in index (only takes integers)

    DataFrame Copy vs Reference
    ======================
    a = b           -> a and b DataFrame's refer to same object
    a = b.copy()    -> use DataFrame's copy() method

    Convert from Dict to DataFrame
    ======================
    http://pbpython.com/pandas-list-dict.html

    Plotting
    ======================
    Overlay - http://stackoverflow.com/questions/36132749/pandas-plotting-from-pivot-table
    Simple - http://matplotlib.org/examples/pylab_examples/simple_plot.html
    Misc - http://machinelearningmastery.com/machine-learning-in-python-step-by-step/
    Misc - http://www.peterbeerli.com/classes/images/2/26/Isc4304matplotlib6.pdf
    Misc - Markers http://stackoverflow.com/questions/22172565/matplotlib-make-plus-sign-thicker
    ```

* Other guides
    * ML step by step http://machinelearningmastery.com/machine-learning-in-python-step-by-step/

* TODO
    * Testing and training
        * http://stackoverflow.com/questions/24147278/how-do-i-create-test-and-train-samples-from-one-dataframe-with-pandas
        * http://stackoverflow.com/questions/27953980/how-to-train-large-dataset-for-classification

* Bugs
    * in Introduction to the Data https://www.dataquest.io/m/139/introduction-to-k-nearest-neighbors/3/k-nearest-neighbors,
     it says I've got the right answer
    even if I just do `print(dc_listings)` instead of only displaying
    the first row as we are asked in the instructions by using
    `print(dc_listings.iloc[0:1])`. It should also suggest using
    `read_csv` since `from_csv` no longer has support
    * In the hint for "4: Euclidean Distance", it says to use the
    value of 8, when it should say to use the value of 3
    (i.e. accommodates up to 3)
    * In section "5: Calculate Distance For All Observations"
    What does `np.random.seed(1)` do? Without adding that line of code
     it appears to still randomise the ordering of the rows,
     and if I change the value of the argument it doesn't
     seem to do anything different either (i.e.
     `np.random.seed(0)` or `np.random.seed(None)`)
    * In "8: Function To Make Predictions" it says to "Sort temp_df
    by the distance column and select the first 5 values in the
    bedrooms column". I think it should say "price" column (not
    "bedrooms" column), as we are taking the mean of the price
    column values
    * In “introduction-to-k-nearest-neighbors” I think it would be
    beneficial to incorporate a plot (i.e. using matplotlib library)
    after the student has cleansed the "price" column, since I think
    data science is better understood when at least some of the data
    may be visualised.
    i.e.
      ```
      import matplotlib.pyplot as plt
      dc_listings_cleaned.pivot_table(index='accommodates', values='price').plot()
      plt.show()
      ```
    * In "https://www.dataquest.io/m/156/evaluating-model-performance",
    within section "1: Testing Quality Of Predictions" the following needs fixing:
          * The Solution code is wrong, the `predict_price` function is duplicated (appears twice), the first one is wrong, as the first line in the function should be `temp_df = train_df`
          * In the "Instructions" it says "...temp_df is assigned to from dc_listings to train_df...". But I think it should instead state "...temp_df is assigned WITH from dc_listings to train_df..." so it's clear what it means.
    * In the "4: Normalize Columns" of https://www.dataquest.io/m/140/multivariate-k-nearest-neighbors/3/handling-missing-values,
    the last paragraph before the code snippet is incomplete:
    "Let's now normalize all of the feature columns in dc_listings.
    We'll avoid normalizing the price column since we want the"
    * In "2: Hyperparameter Optimization" of https://www.dataquest.io/m/141/hyperparameter-optimization/2/hyperparameter-optimization
    typo "Let's confirm that grid search will work quickly for the dataset we're owrking..."
    * In https://www.dataquest.io/m/154/cross-validation/2/holdout-validation, "2: Holdout Validation", it says to use 5 neighbors, but it doesn't specify what algorithm to use, so I decided to use the "brute" algorithm as we'd used that before i.e. `KNeighborsRegressor(n_neighbors=5, algorithm='brute', p=2)`.  I kept getting the error "The variable avg_rmse is greater than it should be". It took me a while to realise that this was because the solution uses `KNeighborsRegressor()` with default arguments for the algorithm of 'auto'.
      I think it would help to give students a hint to students to be careful when using the KNN algorithm argument (as I just thought we were to adopt values we'd use previously since it wasn't specified)


## Chapter 2 - Regression Process <a id="chapter-2"></a>

* Machine Learning techniques when trying to understand a process or
system given **observational data** are easier to understand when the questions
are not abstract and we break-down the question.
    * Example:
        * i.e. instead of
            * NO "how do the attributes of a house affect its market value"
            * YES "how does house size, qty rooms, neighborhood crime index affect its market value"
* Processes
    * **Regression** Process is where trying to predict a specific real valued number
    * **Classification** Process of trying to predict a binary value (True/False)\

* Example:
    * Narrow question
        * Problem (abstract) - How do the properties of a car impact it's fuel efficiency?
        * Problem (narrow) - How does the number of cylinders, displacement, horsepower, weight, acceleration, and model year affect a car's fuel efficiency?
    * STEP 1
        * Exploratory data analysis of some columns to find
        which one best correlates with fuel efficiency
            * Create a grid of Scatter Subplots
            (2 rows, 1 column)
                ```
                import matplotlib.pyplot as plt

                fig = plt.figure()

                # Top Sub-Plot
                ax1 = fig.add_subplot(2,1,1)

                # Bottom Sub-Plot
                ax2 = fig.add_subplot(2,1,2)
                x = "weight"
                y = "mpg"
                cars.plot(x, y, kind='scatter', ax=ax1)

                x = "acceleration"
                y = "mpg"
                cars.plot(x, y, kind='scatter', ax=ax2)
                plt.show()
                ```


        * Interpret the Model with Exploratory Data Analysis by plotting each Training column against the Target column
            ```
            - Purpose:
                - Machine Learning Model is "equation" representing input to output mapping by determining relationship
                  between Independent Variables and the Dependent Variable
                - Linear Machine Learning Models are of the form: y = mx + b
                    - where input x is transformed using m slope and b intercept parameters
                    - where output is y
                    - where m expected to be negative since negative linear relationship
                - Determine relationship between Target column (Dependent Variable) and Training columns (Independent Variables)
                - Determine how Training columns affect the Target column
                - Determine which Training column best correlates to Target column
            - Process of Finding "equation" (Machine Learning Model) that best "Fits" the data
                - Interpret Linear Relationship
                    - Strong / Weak and Positive or Negative Linear Relationship / Gradient
                - Fit the Model to the Data
                    - Scikit-Learn
            ```

    * STEP 2:
        * Scikit-Learn Linear Regression
            http://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LinearRegression.html
            * Usage
                * Ideal for small to medium sized datasets
                * Used to Prototype and explore Machine Learning Models on subsets of larger datasets)
            * Fit the Model to the Data
            * Instantiate Machine Learning Model
                * Implementation
                    * Instantiate each Machine Learning Model then Fit the model to a dataset
                    * Train the Linear Regression Model
                        ```
                        # LinearRegression class from Scikit-Learn
                        lr = LinearRegression()

                        # Fit a Machine Learning Model to the data
                        #   - where `input` is matrix with:
                        #     - rows - `n_samples`
                        #     - columns - `n_features`
                        #   - where `output` is:
                        #     - array of `n_samples` when predicting one output
                        #     - matrix of `n_samples` rows and `n_outputs` columns when predicting multiple outputs simultaneously
                        #
                        #   - Important Note:
                        #     - Given say a dataset with 400 rows and 10 columns, must pass in matrix of 400 rows and 1 column to predict 1 column
                        #     - Prior to passing `input` to the Fit function, convert the Series/Dataframe objects to a Numpy matrix
                        #       first so Scikit-Learn can convert the input to a Numpy Object
                        #       - WRONG Obtain Numpy array (400 elements) returned from Series using `values` attribute `df["mpg"].values.shape`
                        #       - CORRECT Obtain Numpy matrix object (400 rows, 1 col) returned from Series using `values` attribute `df[["mpg"]].values.shape`
                        #  -
                        lr.fit(inputs, output)
                        ```
            * Make Predictions using the LinearRegression `predict` method
            that takes single matrix as the required parameter (n_samples x n_features) and
            returns predicted values matrix/list (n_samples x 1)
                http://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LinearRegression.html#sklearn.linear_model.LinearRegression.predict
            * Plot Evaluation of Predicted Target value from training on known features vs Actual Target value
            * Calculate the MSE Error Metric for Regression to gain quantitative understanding of Models error
            (mismatch between the Model's Predictions and Actual values)

## Chapter 3 - Classification <a id="chapter-3"></a>
    * **Machine Learning Goal**
        * Understand relationship b/w Dependent Variable (Target Feature)
        and Independent Variables (Other Features), including the
        Maths function that uses Features to generate Labels
    * **Supervised Machine Learning**
        * Types include **Linear Regression**
            * USEFUL when Target column we're trying to
            predict (Dependent Variable) is **ordered and continuous**
            * NOT USEFUL when Target column contains **discrete** values
        * Uses Training Data that contains a Label for each
        row to approximate the Maths function
    * **Classification Problem Types** are solved using **Predictive Models**
    where the Target column has a **finite** set of possible values that
    represent different Categories a row may belong to,
    where Categories represented by integers (so Math functions can describe
    how Independent Variables map to Dependent Variables
        * **Binary Classification Algorithm** where Boolean values for options:
            * 0 for False
            * 1 for True
            ```
            Problem                    | Features                |   Type    |   Categories    |   Numerical Categories
            ================================================================================================
            should we pass exam test?  | Score, Quality Level    | Binary    | Don't, Accept   | 0, 1
            ---------------------------|-------------------------|-----------|-----------------|------------
            what's likely blood type   | Parent 1's blood type,  | Multi-    | A, B, AB, O     | 1, 2, 3, 4
            of two people?             | Parent 2's blood type.  | class     |                 |
            ```

    * STEP 1: Identify Problem
        * Example
            * Context
                * University admissions must decide whether
                to accept or reject student applicants, each having unique
                set of test scores, grades, backgrounds.
            * Goal
                * Predict using **Binary Classification Algorithm**
                if student applicant will be admitted to University.
                Predict by estimating the relationship
                using **Categorical Values** of
                the `admit` (Dependent Variable) Column based on
                `gpa` and `gre` (Independent Variable) Columns
            * Approach
                * **Logistic Regression** Model as
                the **Classification Technique**
                that outputs an **Output Probability value**.
                    * Set the **Threshold Probability**
                    * If **Output Probability Value** exceeds
                    the **Threshold Probability**
                    probability then assign Label of 1 to the row,
                    otherwise assign Label of 0
                * **Logit Function** is used to represent the relationship between Dependent and
                Independent Variables (an adaption of the Linear Function applied to Classification)
                where the **Output Probability Value** has to be
                a positive real value between **0 and 1**
                * **Logit Function** parts must combine the following two parts:
                    http://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LogisticRegression.html
                    * **Exponential Transformation** (transforms values to be Positive)
                    i.e. y = e^x (not restricted to a range)
                    * **Normalisation Transformation** transforms values to range between 0 and 1
                    (does not constrain negative values by itself when inspect plot y = x / (x + 1)
            * Data set
                * Contains 644 "Applicants" with Columns:
                    * `gre` - graduate exam score (range 200 to 800)
                    * `gpa` - average college grade (range 0 to 4)
                    * `admit` - Binary (admitted or rejected) (0 or 1)
                        * The numbers do not carry any weight!!
            * Visualise the Relationship using Pandas between the
            Dependent Variable and Independent Variables to
            see which have clear Linear relationship
            * Plot Probabilities
                * **Output Probability value** for each row of a trained Logistic Regression Model
                is the predicted probability that each row should be labelled as 1 (i.e. True)
                * Predicted Probability is calculated using the `predicted_proba`
                http://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LogisticRegression.html#sklearn.linear_model.LogisticRegression.predict_proba
                by passing it the parameter of Training Features (observations) that we
                want the Model to return predicted probabilities for.
                Scikit-Learn returns for each input row a Numpy array with two probabilities:
                (probability that row should be labelled 0,
                and probability that row should be labelled 1, both results together add to 1)
                    ```
                    probabilities = logistic_model.predict_proba(admissions[["gpa"]])
                    # Probability that the row belongs to label `0`.
                    probabilities[:,0]
                    # Probabililty that the row belongs to label `1`. Returns Numpy Array of values at index 1
                    probabilities[:,1]
                    ```
            * Predict the Target Columns value for each row of the dataset using `predict` method
            so we can see how well the model can predict the correct Target column value based on training on the data
                http://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LogisticRegression.html#sklearn.linear_model.LogisticRegression.predict
                ```
                fitted_labels = logistic_model.predict(admissions[["gpa"]])
                plt.scatter(admissions["gpa"], fitted_labels)
                ```
            * **Accuracy** equation to determine the fraction of the predictions that were
            correct (i.e. where the Actual Target Column value `admit` matched the `predictioned_target_values`)
            to understand how effective the classification model was on the training data.
                * Equation
                    ```
                    Accuracy = # of correctly Predicted Target observation values / # of observations
                    ```
                * **Binary Classification Outcomes**
                    * **Accuracy** does not help discriminate b/w different types of outcomes
                    like a **Binary Classification Model** can
                    * Accuracy > 50% validates that the model used on the dataset used for training
                    is better than just randomly guessing the Target value for each observation.
                    * Accuracy == 100% when evaluated on its training set does not tell how model
                    performs on data it has not see before (i.e. **Unseen data**)
                    * Note: Since Logistic Regression model's output is probability between 0 and 1,
                    we set a **Discrimination Threshold** to determine what observations to `admit` if the
                    computed probability exceeds the threshold.
                    Scikit-Learn sets the **Discrimination Threshold** to 0.5 by Default when
                    predicting Target observation values (so if predicted probability is greater than 0.5
                    it sets the respective row in `predicted_target_values` to 1, otherwise 0)
                    * An **Accuracy** value of 0.2 means the model predicted 20% of the `admit` column rows
                    correctly for the given Discrimination Threshold

                    * **Evaluate Binary Classification Models**
                        * Options:
                            * Testing Model's effectiveness on Unseen data
                                * TODO
                            * **Testing Model's effectiveness on Training data**
                                * **Segment Binary Classification Model Predictions into
                                4x different Binary Classification Outcome Categories**

                                    * Increase **Granularity** above just **Accuracy**
                                    ```
                                    Prediction   |    Observation
                                                 |-------------------------------------------
                                                 |    Admitted(1)          Rejected(0)
                                    -------------|-------------------------------------------
                                    Admitted(1)  |    True Positive(TP)    False Positive(FP)
                                                 |
                                    Rejected(0)  |    False Negative(FN)   True Negative(TN)

                                    ```
                                    * where
                                        * **True Positive** - model correctly predicted student would be admitted
                                        (i.e. model Predicted the Target column row Positive, and was Actually True)
                                        (i.e. when Predicted Target column row is 1, whilst Actual Target column row is 1)
                                        * **True Negative** - model correctly predicted student would be rejected
                                        (i.e. opposite of True Positive)
                                        * **False Positive** - model Incorrectly predicted student would be admitted, even
                                        though the student was Actually rejected
                                        (i.e. when Predicted Target column row is 1, whilst Actual Target column row is 0)
                                        * **False Negative** - model Incorrectly predicted student would be rejected, even
                                        though the student was Actually admitted

                                * **Sensitivity**
                                    * **Dfn** is the **True Positive Rate (TPR)** is the
                                    proportion of applicants that were correctly admitted
                                    and is used to find out **how effective model is at identifying
                                    positive outcomes**
                                        ```
                                        TPR = (Fraction of students the model correctly admitted)
                                              / (All students that should have been admitted)
                                            = True Positives / (True Positives + False Negatives)
                                        ```
                                        * **Trade-offs**
                                            * **TPR Low** means model not effective at catching positive cases
                                            * **Highly Sensitive** model should be able to catch all
                                            positive cases (apply to detecting important things like
                                            cancer, etc). i.e. in healthcare low sensitivity could mean
                                            loss of life (i.e. if classification model only catches
                                            10% of positive cases for an illness then
                                            9 out of 10 people are going undiagnosed
                                            (classified as False Negatives)
                                            * **Low Sensitivity** (i.e. graduate schools can only
                                            admit a select qty of students into their programs and
                                            it is ok to reject many qualified students that would have
                                            succeeded)
                                * **Specificity**
                                    *  **Dfn** is the **True Negative Rate (TNR)** is the
                                    proportion of observations that were correctly rejected
                                    and is used to find out **how effective model is at identifying
                                    Negative outcomes**
                                        ```
                                        TPR = (Fraction of students the model correctly rejected)
                                              / (All students that should have been rejected)
                                            = True Negatives / (False Positives + True Negatives)
                                        ```
                                        * **Trade-offs**
                                            * **TNR High** means model is good at predicting which
                                            observations should be rejected
                                            * **High TNR** means model is really good at predicting
                                            which observations/applicants to reject
                                            (if TNR is 90% and only 5% of observations/applicants
                                            were Actually accepted that applied, then it's important
                                            for the model to reject people correctly who would not
                                            otherwise have been accepted)
                                * **Fall-Out**
                                    * **Dfn** is the **False Positive Rate (TPR)** is the
                                    proportion of applicants that were incorrectly admitted
                                    (i.e. were admitted but should have been rejected, where
                                    `actual_label` 0 and `predicted_label` 1
                                    and is used to find out **how ineffective model is at identifying
                                    positive outcomes**
                                        ```
                                        TPR = (Fraction of students the model correctly admitted)
                                              / (All students that should have been admitted)
                                            = True Positives / (True Positives + False Negatives)
                                        ```
                                        * **Trade-offs**
                                            * **TPR Low** means model not effective at catching positive cases
                                            * **Highly Sensitive** model should be able to catch all
                                            positive cases (apply to detecting important things like
                                            cancer, etc). i.e. in healthcare low sensitivity could mean
                                            loss of life (i.e. if classification model only catches
                                            10% of positive cases for an illness then
                                            9 out of 10 people are going undiagnosed
                                            (classified as False Negatives)
                                            * **Low Sensitivity** (i.e. graduate schools can only
                                            admit a select qty of students into their programs and
                                            it is ok to reject many qualified students that would have
                                            succeeded)
            * **Cross-Validation** - after training any kind of machine learning model,
            evaluate the model's accuracy on new data it was not trained on
            by testing a Classifiers Generalisability using Cross-Validation Techniques
            of splitting historical data into Training set (used to train the classifier) and
            Test set (used to evaluate classifier's effectiveness),
            since we want to use Training data (labelled historical outcomes for each output for
            the Target column row observation) to use to build a **Classifier** that returns
            predicted labels for new **unlabelled data** (since if we only evaluate the
            Classifier's effectiveness on the data it was trained on we can run into
            **Overfitting** where Classifier only performs well on Training but does not
            generalise to future data.
                * **Overfitting** may be apparent if Cross-Validation (i.e. before splitting
                into Training/Testing) with LARGE K-Folds causes the Prediction **Accuracy**
                to decrease when compared to say No Folds, which would mean the Model performs
                significantly worse on New data **Unseen**. If **Accuracy** was lower than say 40%
                then we'd reconsider even using Logistic Regression at all for predicting.


            * **Receiver Operator Characteristic (ROC) Curve**
                * **DFN** used to vary the **Discrimination Threshold** and calculate the
                TPR (Sensitivity) and FPR (Fall-Out) for each value by using the
                Scikit-Learn `roc_curve` function http://scikit-learn.org/stable/modules/generated/sklearn.metrics.roc_curve.html.
                which calculates the TPR and FPR for varying Discrimination Thresholds until both reach 0%
                and takes 2x parameters:
                    * `y_true` - list of true labels (i.e. Actual Target column values) for the observations
                    * `y_score` - list of Models probability scores for the observations
                `roc_curve` returns 3x values that can be assigned all at once:
                `fpr, tpr, thresholds = metrics.roc_curve(labels, probabilities)`
                * **ROC Curve** allows understand a Binary Classification Model's Performance
                as the Discrimination Threshold is varied
            * **Area Under Curve (AUC)**
                * **DFN** Inspect ROC Curve to see how the FPR and TPR measures trade-off
                and then select an appropriate **Discrimination Threshold** based on your priorities
                * **AUC** describes the probability that the Classifier will rank a random positive
                observation higher than a random negative observation
                (noting that just randomly guessing converges to a probability of 0.5), such that
                the higher the AUC the Higher Accuracy of the model
                * **AUC** calculated using `roc_auc_score` function of Scikit-Learn
                * Custom Plot with text and arrows https://matplotlib.org/users/text_intro.html

NEXT - unsupervised machine learning and we'll focus on a technique called clustering

    * **Clustering**
        * Previously we learnt **Supervised Machine Learning** types (where we
        train an algorithm to predict an unknown variable from known variables) including:
            * Regression
            * Classification
        * Now we learn **Unsupervised Machine Learning** type (where instead of trying to predict anything
        we instead try to find Patterns in data)

            * **Clustering**
                * Usage: Machine learning technique to explore unknown data and understand
                connections b/w rows and columns.
                Typically **unsupervised learning** like K-Means Clustering Algorithm is used to explore a dataset
                when it isn't obvious where to start before trying to use
                **supervised learning** machine learning models
                (i.e. generate new columns that are easier to reason with to use later in **supervised learning**
                * Inspect the data for patterns that may help develop a machine learning model.
                One way to look for patterns is to use a clustering algorithm to create clusters, then plot them out
                To visualize how board games are clustered, we can calculate the row means and row standard deviations
                and then generate a scatter plot that compares the means against the standard deviations
                    ```
                    plt.hist(board_games["average_rating"])
                    print(board_games["average_rating"].std())
                    print(board_games["average_rating"].mean())
                    ```
                * Creating Clusters: https://flowingdata.com/2012/03/21/redefining-nba-basketball-positions/
                    https://www.ayasdi.com/
                    * Group similar rows together.
                    * Groups form the Clusters
                    * Cluster inspection to better understand the structure of the data
                        * Reindex Labels
                        http://stackoverflow.com/questions/40280552/pandas-crosstab-how-to-print-rows-columns-for-values-that-dont-exist-in-the-d
                * Example 1: Cluster US Senators based on how they voted
                    https://github.com/unitedstates/congress

                    * Dataset comprising results of roll call votes from 114th Senate
                    where each row represents a single Senator, and each column represents a vote.
                    Rows with 0 means No vote to the bill, whereas 1 means they voted Yes, and 0.5 means
                    they abstained.
                    Columns include:
                        * `name` Last name of Senator
                        * `party` party of Senator (i.e. D democrat, R republican, I independent)
                        * `0001`, `0004` are results of each bill's single roll call vote
                    Senator perform a public roll call vote on proposed legislation
                    to pass a bill to get provisions
                    enacted. A majority vote is required to pass a bill.
                    Typically Senators vote along party lines (i.e. in accordance with how
                    their political party votes) for either:
                        * Democrats (liberal)
                        * Republicans (conservative)
                        * Independent (unaffiliated)
                    * Goal is to Cluster the voting data to expose Patterns deeper than just party
                    affiliation (i.e. some republicans less mainstream and more liberal than rest of party)
        * STEPS
            * Step 1: Load data from CSV file
            * Step 2: Explore data using
                * Values breakdown - `value_counts()` on columns of dataframe
                * Average value of column - `mean()` on dataframe column
            * Step 3: Find out how close rows are to each other
             by calculating similarity distance (Euclidean Distance) b/w each row (Senator votes)
             where the closer together the voting records are then the more ideologically similar
             they are (i.e. voting same way indicates they share same views)
                * i.e. calculate distance between Senator vote in 1st and 2nd rows
                `euclidean_distances(votes.iloc[0,3:], votes.iloc[1,3:])`
            * Step 4: Group together rows (Senator votes) closest to each other
            * Step 5: **K-Means Clustering (aka Lloyd's)** Algorithm
                https://en.wikipedia.org/wiki/K-means_clustering
                * **Definition**: Partitioning the data space into Voronoi cells by partitioning n observations (rows)
                into k clusters where each observation belongs to the cluster with the nearest mean
                and where the means are called the cluster Centroids. K-Means algorithm
                aims to choose Centroids that minimise the inertia (meaure of how internally coherent
                clusters are)
                * Create Clusters by specifying quantity of clusters upfront for the K-Means Algorithm
                    * i.e. since we expect their votes to cluster along party lines, we suspect majority of
                    Senators to be either Republicans or Democrats (so we'd pick Clusters quantity of 2)
                * Assign each Cluster a center. Calculate Euclidean distance from each row (Senator data) to the cluster center
                * Assign rows (Senator data) to Cluster they are closest to (lowest Euclidean Distance)
                * Train the model using Scikit-Learn `KMeans`
                    http://scikit-learn.org/stable/modules/clustering.html#clustering
                    http://scikit-learn.org/stable/modules/generated/sklearn.cluster.KMeans.html
                    * Note: Since we are not predicting there is no risk of **Overfitting**, so
                    we will train the model on the whole dataset
                    * i.e. initialise K-Means Model with 2 clusters and random state of 1 to allow same results to be
                    reproduced whenever the algorithm is run
                    ```
                    kmeans_model = KMeans(n_clusters=2, random_state=1)
                    ```
                * Extract Cluster Labels after Training that indicate what cluster each Senator belongs to
                * Fit using `fit_transform()` the model to the the dataframe and get Euclidean Distance of each row (Senator vote) to each Cluster
                http://scikit-learn.org/stable/modules/generated/sklearn.cluster.KMeans.html#sklearn.cluster.KMeans.fit_transform
                where the response is Numpy array with 2x columns indicating how far the row (Senator vote) is
                from each Cluster (the farther away the less the Senator's voting history aligns with the Cluster's
                voting history):
                    * 1st column - Euclidean distance from each row (Senator vote) to the 1st Cluster
                    * 2nd column - Euclidean distance from each row (Senator vote) to the 2nd Cluster
                * Initial Clustering
                    * Example where only selects all the rows but only from columns after the first 3 from the dataframe when fitting
                    ```
                    import pandas as pd
                    from sklearn.cluster import KMeans

                    kmeans_model = KMeans(n_clusters=2, random_state=1)
                    senator_distances = kmeans_model.fit_transform(votes.iloc[:, 3:])
                    ```
                * Explore the Clusters
                    * Use `crosstab()` function from Pandas http://pandas.pydata.org/pandas-docs/version/0.17.1/generated/pandas.crosstab.html
                    (takes two parameters as vectors or Pandas Series to compute how many times each unique value in the
                    second vector occurs for each unique value in first vector.
                    to compute and display how many rows (Senator votes) from each party ended up in each Cluster
                        ```
                        * Given:

                        is_smoker =       [0,1,1,0,0,1]
                        has_lung_cancer = [1,0,1,0,1,0]

                        * Crosstab output

                        has_lung_cancer    0     1
                        smoker
                        0                  1     2
                        1                  2     1

                        where 0 is False, 1 is True
                        ```
                * Extract the Cluster Labels for each row (Senator vote) from the `kmeans_model` using
                `kmeans_models.labels_` and then make a table comparing the labels to `votes["party"]`
                using `crosstab()` so we can compare and try to see if the clusters tend to break down along
                the party lines or not
                    ```
                    labels = kmeans_model.labels_
                    print(pd.crosstab(labels, votes["party"]))
                    ```
                * Analyse Output
                    * Output:
                        * Indicates that both clusters mostly broke down along party lines since
                        1st cluster contains 41 Democrats, and both Independents, whilst
                        2nd cluster contains 3 Democrats, and 54 Republicans
                        None of the Republicans appear to have broken party ranks to vote instead with the Democrats, however
                        3 Democrats are more similar to Republicans in their voting behaviour than with their own party
                        ```
                        party   D  I   R
                        row_0
                        0      41  2   0
                        1       3  0  54
                        ```
                        * Find out why the 3 Democrats switched:
                            * Subset the dataframe using Pandas to only select rows where `party` column is `D`, and
                            where the `labels` variable is `1` (indicating the Senator votes are in the 2nd Cluster)
                                * Select all Independents in 1st Cluster (subset dataframe):
                                    `votes[(labels == 0) & (votes["party"] == "I")]`
                                * Select all that were assigned to 2nd Cluster that were Democrats
                                assign to `democrat_outliers`
                                    ```
                                    democratic_outliers = votes[(labels == 1) & (votes["party"] == "D")]
                                    print(democratic_outliers)
                                    ```
                                    * Output:
                                        ```
                                                name party state  00001  00004  00005  00006  00007  00008  00009  \
                                        42  Heitkamp     D    ND    0.0    1.0    0.0    1.0    0.0    0.0    1.0
                                        56   Manchin     D    WV    0.0    1.0    0.0    1.0    0.0    0.0    1.0
                                        74      Reid     D    NV    0.5    0.5    0.5    0.5    0.5    0.5    0.5

                                            00010  00020  00026  00032  00038  00039  00044  00047
                                        42    1.0    0.0    0.0    0.0    1.0    0.0    0.0    0.0
                                        56    1.0    1.0    0.0    0.0    1.0    1.0    0.0    0.0
                                        74    0.5    0.5    0.5    0.5    0.5    0.5    0.5    0.5
                                        ```
                        * Plot the Clusters
                            * Create a scatter plot that shows the position of each Senator
                            as `x` (1st column of `senator_distances` Numpy array `senator_distances[:,0]`) and
                            `y` (2nd column of `senator_distances`)
                            and `c` as the `labels` (to shade points according to label)
                            coordinates (since distances are relative to the Cluster Centers),
                            based on computed `senator_distances` array that showed the distance from each Senator
                            the the center of each Cluster.
                            Shade each point according to party affiliation (so can quickly inspect
                            the layout of the Senators and see who crosses party lines)
                                ```
                                plt.scatter(x=senator_distances[:,0], y=senator_distances[:,1], c=labels)
                                plt.show()
                                ```
                        * Find Radical/Extreme Clusters
                            * Find the rows (Senator votes) that are furthest from one Cluster
                                * i.e. Radical Republican would be farthest from Democratic Cluster as possible)
                                * i.e. Moderate Senators would be between the views of the two parties
                            * Inspect the first few rows of `senator_distances` to find the extremes
                                ```
                                [
                                   [ 3.12141628,  1.3134775 ], # Slightly moderate, far from cluster 1, close to cluster 2.
                                   [ 2.6146248 ,  2.05339992], # Moderate, far from cluster 1, far from cluster 2.
                                   [ 0.33960656,  3.41651746], # Somewhat extreme, very close to cluster 1, very far from cluster 2.
                                   [ 3.42004795,  0.24198446], # Fairly extreme, very far from cluster 1, very close to cluster 2.
                                   ...
                               ]
                                ```
                            * Create formula to find Extremeists (i.e. cube distance in both columns of `senator_distances`
                            with `senator_distances ** 3`
                            then add them together with `sum()` and `axis=1`, whereby the higher the exponent we raise a set of numbers to will mean
                            the more separation we'll see between small values and low values
                            (i.e. given [1,2,3], squaring it gives [1,4,9], and cubing gives [1,8,27]

                                    * i.e. We cube the distances so that we can get a good amount of separation
                                    between the extremists who are farther away from a party, who have distances
                                    that look like extremist = [3.4, .24], and moderates, whose distances look like
                                     moderate = [2.6, 2]. If we left the distances as is, we'd end up with 3.4 + .24 = 3.64,
                                     and 2.6 + 2 = 4.6, which would make the moderate, who is between both parties, seem
                                     extreme. If we cube, we instead end up with 3.4 ** 3 + .24 ** 3 = 39.3, and
                                     2.6 ** 3 + 2 ** 3 = 25.5, which correctly identifies the extremist.
                                 * Note: to cube every value in Numpy array `senator_distances` with `senator_distances ** 3`
                                 * Note: to sum across every row we use Numpy `sum()` method with `axis=1`
                            * Store and sort Extremists
                                * Assign result of computing extremism rating to `extremism`
                                (cubing all values in `senator_distances` and then finding sum across each row)
                                * Assign `extremism` variable to `extremism` column of `votes`
                                * Sort `votes` on by its `extremism` column in descending order using `sort_values()` method on DataFrames

                                    ```
                                    extremism = (senator_distances ** 3).sum(axis=1)
                                    votes["extremism"] = extremism
                                    votes.sort_values("extremism", inplace=True, ascending=False)
                                    print(votes.head(10))
                                    ```
                                    * Output
                                        ```
                                                 name party state  00001  00004  00005  00006  00007  00008  00009  extremism
                                        98     Wicker     R    MS    0.0    1.0    1.0    1.0    1.0    0.0    1.0  46.24
                                        53   Lankford     R    OK    0.0    1.0    1.0    0.0    1.0    0.0    1.0  46.04
                                        69       Paul     R    KY    0.0    1.0    1.0    0.0    1.0    0.0    1.0  46.04
                                        80      Sasse     R    NE    0.0    1.0    1.0    0.0    1.0    0.0    1.0  46.04
                                        26       Cruz     R    TX    0.0    1.0    1.0    0.0    1.0    0.0    1.0  46.04
                                        48    Johnson     R    WI    0.0    1.0    1.0    1.0    1.0    0.0    1.0  46.01
                                        47    Isakson     R    GA    0.0    1.0    1.0    1.0    1.0    0.0    1.0  46.01
                                        65  Murkowski     R    AK    0.0    1.0    1.0    1.0    1.0    0.0    1.0  46.01
                                        64      Moran     R    KS    0.0    1.0    1.0    1.0    1.0    0.0    1.0  46.01
                                        30       Enzi     R    WY    0.0    1.0    1.0    1.0    1.0    0.0    1.0  46.01

                                        ```

        * Part 2 Answer:
            ```
            import pandas as pd
            votes = pd.read_csv("114_congress.csv")
            ```
        * Part 3 Answer:
            ```
            print(votes["party"].value_counts())
            print(votes.mean())
            ```
        * Part 4 Answer:
            * Find Euclidean distance between Senator in 1st row and Senator in 2nd row
            ```
            from sklearn.metrics.pairwise import euclidean_distances

            print(euclidean_distances(votes.iloc[0,3:].reshape(1, -1), votes.iloc[1,3:].reshape(1, -1)))
            distance = euclidean_distances(votes.iloc[0,3:].reshape(1, -1), votes.iloc[2,3:].reshape(1, -1))
            ```


* Part 1 Answer:
    ```
    names = "mpg,cylinders,displacement,horsepower,weight,acceleration,model year,origin,car name"
    names = names.split(",")
    cars = pd.read_table("auto-mpg.data", delim_whitespace=True, names=names)
    print(cars)
    ```
* Part 6 Answer:
    ```
    pred_probs = logistic_model.predict_proba(admissions[["gpa"]])
    plt.scatter(admissions["gpa"], pred_probs[:,1])
    ```
* Part 7 Answer:
    ```
    fitted_labels = logistic_model.predict(admissions[["gpa"]])
    plt.scatter(admissions["gpa"], fitted_labels)
    ```
* Part 4 Binary Classification Answer:
    ```

    true_positive_filter = (admissions["predicted_label"] == 1) & (admissions["actual_label"] == 1)
    true_positives = len(admissions[true_positive_filter])
    true_negative_filter = (admissions["predicted_label"] == 0) & (admissions["actual_label"] == 0)
    ```

    * **Multi-Classification** Datasets
        * **Definition**: When 3 or more Categories in a column
        * **Multi-Classification** Methods:
            * **One-Versus-All** Method
                * Converting an n-class (in our case n is 3 since there are 3 Categories for the `origin` column)
                classification problem into n binary classification problems
                * technique where we split the problem into Multiple Binary Classification problems,
                and for each observation, the model will then output the probability of belonging to each category:
                    * Positive case - choose single Category
                    * False case - group the rest of the Categories
        * Example
            * Given a Target column `origin` in a dataset that has different Categories (i.e. `1` Nth America, `2` Europe, `3` Asia)
            in its rows (each representing a different car), we can use **Multi-Classification** to predict the `origin`
            of the car
        * STEP 1
            * Load DataFrame
            * Assign unique elements in column `origin` to a variable called `unique_origins`
                ```
                import pandas as pd
                cars = pd.read_csv("auto.csv")
                print(cars.head())
                unique_origins = cars["origin"].unique()
                print(unique_origins)
                ```
            * Identify **Categorical Columns** that contain **Categorical Values** (i.e. `cylinders`, `year`, `origin`)
                * Note: The plain numerical values of `year` are unlikely to relate the same as they would if considered Categories of the Year
                so best to treat Discrete Values as **Categorical Values** in these instances
            * Convert **Categorical Columns** to Numeric values so they may be used to Predict the `origin` label
                * **Dummy Variables** used for columns containing **Categorical Values**
                    ```
                    i.e. Given the `cylinders` Column, where we have More Than TWO Categories of cylinders (i.e. 3,4,5,6,8)
                    we need to create More binary Columns to represent the Categories

                    i.e.
                        Create columns:
                            `cyl3` with values either TRUE or FALSE
                            `cyl4`   "
                            `cyl5`   "
                            `cyl6`   "
                            `cyl8`   "
                    ```
                    * Use the `pandas.get_dummies()` function to return DataFrame containing Dummy Binary Columns from the
                     values in the `cylinders` Column, and pass the `prefix` parameter a value of `"cyl"` so Pandas prepends the
                      column names to match the style we want.
                    * Append the columns from the Dummy DataFrame to our Original DataFrame using `pandas.concat()` function
                        ```
                        dummy_cylinders = pd.get_dummies(cars["cylinders"], prefix="cyl")
                        cars = pd.concat([cars, dummy_cylinders], axis=1)
                        dummy_years = pd.get_dummies(cars["year"], prefix="year")
                        cars = pd.concat([cars, dummy_years], axis=1)
                        ```
                    * Drop the previous columns from the Original DataFrame
                        ```
                        cars = cars.drop("year", axis=1)
                        cars = cars.drop("cylinders", axis=1)
                        print(cars.head())
                        ```
            * Shuffle rows
            * Split into two DataFrames:
                * Train 70%, and
                * Test, 30%
                ```
                shuffled_rows = np.random.permutation(cars.index)
                shuffled_cars = cars.iloc[shuffled_rows]
                highest_train_row = int(cars.shape[0] * .70)
                train = shuffled_cars.iloc[0:highest_train_row]
                test = shuffled_cars.iloc[highest_train_row:]
                ```
            * Using **One-Versus-All** we Train 3 OFF Models for each value in `unique_origins` using Logistic Regression
            (based on **Dummy Variables** created from `cylinder` and `year` columns)
            http://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LogisticRegression.html#sklearn.linear_model.LogisticRegression.fit
            for `origin` Categories that are each **Binary Classification Models**
            that return probability between 0 and 1, such that when apply the OVERARCHING MODEL on unseen data
            it returns a probability value from each of the 3 OFF models.
            Choose the Label (output Prediction column) corresponding to the Binary Classification Model that
            predicted the **Highest probability**
                * Model 1 - where cars in the following places are considered as follows:
                    * `origin` of 1 (Nth America) - Positive 1
                    * `origin` of 2 (Europe) - Negative 0
                    * `origin` of 3 (Asia) - Negative 0
                * Model 2 - where cars in the following places are considered as follows:
                    * `origin` of 1 (Nth America) - Negative 0
                    * `origin` of 2 (Europe) - Positive 1
                    * `origin` of 3 (Asia) - Negative 0
                * Model 3 - where cars in the following places are considered as follows:
                    * `origin` of 1 (Nth America) - Negative 0
                    * `origin` of 2 (Europe) - Negative 0
                    * `origin` of 3 (Asia) - Positive 1
                ```
                from sklearn.linear_model import LogisticRegression

                unique_origins = cars["origin"].unique()
                unique_origins.sort()

                models = {}
                features = [c for c in train.columns if c.startswith("cyl") or c.startswith("year")]

                for origin in unique_origins:

                    # Train 3 OFF Models for each unique origin
                    model = LogisticRegression()

                    # X_train - DataFrame containing just columns starting with prefixes `cyl` and `year` Binary columns
                    X_train = train[features]

                    # y_train - List or Series of Boolean values
                    #           - True if observation/row value for `origin` matches current iterator variable
                    #           - False if observation/row value for `origin` DOES NOT match current iterator variable
                    y_train = train["origin"] == origin

                    model.fit(X_train, y_train)

                    # Add each Model to the models dict with structure:
                    #  Key - origin value (i.e. 1,2, or 3)
                    #  Value - relevant LogisticRegression Model instance
                    models[origin] = model
                ```
            * Test the Models we have for each Category by running the Test dataset against them to check their performance
            by running `predict_proba` Logistic Regression algorithm against each of the 3 OFF `unique_origins` to return
            3 OFF lists of predicted probabilities as `testing_probs`, such that testing_probs[2] returns the probability
            returned from Model 2 for each observation
                ```
                testing_probs = pd.DataFrame(columns=unique_origins)

                for origin in unique_origins:
                    # Select testing features.
                    X_test = test[features]
                    # Compute probability of observation being in the origin.
                    testing_probs[origin] = models[origin].predict_proba(X_test)[:,1]
                ```
            * Classify each observation in the Test set by choosing the origin (column) in DataFrame `testing_probs`
            with highest probability of classification for that observation. Use the DataFrame method
            `.idxmax()` to return a Series of `predicted_origins` where each value corresponds to the
            Column where max value occurs for each observation (row).
                ```
                predicted_origins = testing_probs.idxmax(axis=1)
                print(predicted_origins)
                ```


    * Linear Regression to estimate rate

        * About:
            Compute and interpret statistics often used to measure linear models

        * Example: Pisa


        * Step 1:
            * Plot with scatter plot, and if appears to be Linear,
            then Linear Regression may be good fit for the data
                ```
                import pandas
                import matplotlib.pyplot as plt

                pisa = pandas.DataFrame({"year": range(1975, 1988),
                                         "lean": [2.9642, 2.9644, 2.9656, 2.9667, 2.9673, 2.9688, 2.9696,
                                                  2.9698, 2.9713, 2.9717, 2.9725, 2.9742, 2.9757]})

                print(pisa)
                plt.scatter(pisa["year"], pisa["lean"])
                ```
        * Step 2:
            * Identify key statistical concepts from Linear Regression Model
            using **Statsmodels** Library (Statistical Analysis) offering
            statistical measures for evaluation.
            * Use `sm.OLS` (Ordinary Least Squares) to Initialise Model and
            * Fit the Linear Models with data using `.fit()` method to
            estimate Coefficients of Linear Model
            * Since `OLS()` doesn't automatically add an Intercept to
            Linear Model we can manually add a column of 1's to add
            another Coefficient to Linear Model (so Coefficient multiplied
            by 1 and we are given an Intercept)
                ```
                import statsmodels.api as sm

                y = pisa.lean # target
                X = pisa.year  # features
                X = sm.add_constant(X)  # add a column of 1's as the constant term

                # OLS -- Ordinary Least Squares Fit
                linear = sm.OLS(y, X)
                # fit model
                linearfit = linear.fit()
                print(linearfit.summary())
                ```

                * Output:
                    ```
                                               OLS Regression Results
                    ==============================================================================
                    Dep. Variable:                   lean   R-squared:                       0.988
                    Model:                            OLS   Adj. R-squared:                  0.987
                    Method:                 Least Squares   F-statistic:                     904.1
                    Date:                Thu, 04 May 2017   Prob (F-statistic):           6.50e-12
                    Time:                        19:56:19   Log-Likelihood:                 83.777
                    No. Observations:                  13   AIC:                            -163.6
                    Df Residuals:                      11   BIC:                            -162.4
                    Df Model:                           1
                    Covariance Type:            nonrobust
                    ==============================================================================
                                     coef    std err          t      P>|t|      [95.0% Conf. Int.]
                    ------------------------------------------------------------------------------
                    const          1.1233      0.061     18.297      0.000         0.988     1.258
                    year           0.0009    3.1e-05     30.069      0.000         0.001     0.001
                    ==============================================================================
                    Omnibus:                        0.310   Durbin-Watson:                   1.642
                    Prob(Omnibus):                  0.856   Jarque-Bera (JB):                0.450
                    Skew:                           0.094   Prob(JB):                        0.799
                    Kurtosis:                       2.108   Cond. No.                     1.05e+06
                    ==============================================================================
                    ```
            * Predict Residuals (errors) so they may be removed from the Linear
            Regression prior to esimatation, using `linearfit` with data `X` and `y`
            by subtracting Observed values from Predicted values
                * Printed `summary` contains info about our model. Understand these
                statistical measures with formal definition of a basic linear regression
                 model that is defined mathematically as:
                    ```
                        yi=β0+β1xi+ei

                        where ei∼N(0,σ2)    - is the error term for each observation i
                        where β0            - is the intercept
                        where β1            - is the slope

                    Residual for the prediction of observation i is:

                        ei=yi^−yi

                        where yi^           - is the prediction.
                        where N(0,σ2)       - is normal distribution with mean 0 and variance σ2

                     Model assumes that the errors, known as residuals,
                     between prediction and observed values are normally distributed
                     and that the average error is 0.

                     Estimated coefficients, those which are modeled, will
                     be referred to as βi^ while βi is the true coefficient.

                     In the end, yi^=β0^+β1^xi is the model we will estimate
                    ```
                * Answer:
                    ```
                    # Our predicted values of y
                    yhat = linearfit.predict(X)
                    print(yhat)
                    residuals = yhat - y
                    ```
            * Plot Residuals (errors) on Histogram
                * Histogram using Matplotlib's `hist()` with `bins` parameter for the qty
                of bins of the residual to plot the residuals so can visually accept or reject the
                assumption of normality of errors.
                Basic approach of if histogram of residuals looks similar to bell curve
                then accept assumption of normality.
                    ```
                    # The variable residuals is in memory
                    plt.hist(residuals, bins=5)
                    ```
            * Interpret Histogram
                * Our dataset only has 13 observations in it making it difficult to
                interpret plot. Plot looks somewhat normal, the largest bin only has
                4 observations. In this case we cannot reject the assumption that the
                residuals are normal. Let's move forward with this linear model and
                look at measures of statistical fit.
            * Compute Sum of Squares (SS) for Model
                * Many statistical measures used to evaluate linear regression models
                rely on three sum of squares measures including:
                    * Sum of Square Error (SSE)
                        * `SSE=∑i=1n(yi−yi^)2=∑i=1nei2`.
                        * SSE is sum of all residuals giving us a measure between the
                        model's prediction and the observed values.

                    * Regression Sum of Squares (RSS)
                        * `RSS=∑i=1n(yi¯−yi^)2 where yi¯=1n∑i=1nyi`
                        * RSS measures the amount of explained variance which our model
                        accounts for. For instance, if we were to predict every observation
                        as the mean of the observed values then our model would be useless
                        and RSS would be very low.
                        A large RSS and small SSE can be an indicator of a strong model.
                    * Total Sum of Squares (TSS)
                        * `TSS=∑i=1n(yi−yi¯)2`
                        * TSS measures the total amount of variation within the data
                * In aggregate each of these measures explain the variance of the entire model.
                * With some algebra we can show that `TSS = RSS + SSE`. Intuitively this makes
                sense, the total amount of variance in the data is captured by the variance
                explained by the model plus the variance missed by the model
                ```
                import numpy as np

                # sum the (predicted - observed) squared
                SSE = np.sum((y.values-yhat)**2)
                # Average y
                ybar = np.mean(y.values)

                # sum the (mean - predicted) squared
                RSS = np.sum((ybar-yhat)**2)

                # sum the (mean - observed) squared
                TSS = np.sum((ybar-y.values)**2)
                ```
            * Compute R-Squared
                * R-Squared is Coefficient of Determination is a measure of linear
                dependence. It is a single number which tells us what the percentage of
                Variation in the target variable is explained by our model.
                `R^2 = 1−SSE/TSS = RSS/TSS`
                Intuitively we know a low SSE and high RSS indicates a good fit.
                This single measure tells us what percentage of the total variation
                of the data our model is accounting for. Correspondingly, the R2 exists
                between 0 and 1.
                    ```
                    SSE = np.sum((y.values-yhat)**2)
                    ybar = np.mean(y.values)
                    RSS = np.sum((ybar-yhat)**2)
                    TSS = np.sum((y.values-ybar)**2)
                    R2 = RSS/TSS
                    print(R2)
                    ```
            * Interpret R-Squared
                * We see that the R-Squared value is very high for our linear model,
                0.988, accounting for 98.8% of the variation within the data.
            * Coefficients of the Linear Model
                * Advantage of linear models over some more complex ones is
                its ability to understand and interpret Coefficients
                Each βi in a linear model f(x)=β0+β1x is a coefficient.
                Methods may be used to find the confidence of the estimated coefficients.
                In the "OLS Regression Results" we see summary of our model. There are many statistics here
                including R-squared, the number of observations, and others.
                In the second section there are coefficients with corresponding statistics.
                The row `year` corresponds to the independent variable x while
                `lean` is the target variable.
                The variable `const` represents the model's intercept.
                First look at the coefficient itself.
                The Coefficient measures how much the dependent variable will change
                with a unit increase in the independent variable.
                For instance, we see that the coefficient for year is 0.0009.
                This means that on average the tower of Pisa will lean 0.0009 meters
                per year.
            * Assuming no external forces on the tower, how many meters will the
            tower of Pisa lean in 15 years
                ```
                # Print the models summary
                #print(linearfit.summary())

                #The models parameters
                print("\n",linearfit.params)
                delta = linearfit.params["year"] * 15
                ```

            * Variance of Coefficients
                * The variance of each of the coefficients is an important and powerful measure.
                Coefficient of `year` represents number of meters the tower will lean each year.
                Variance of this coefficient would then give us an interval of the expected movement
                for each year.
                In the OLS Regression Results summary output, next to each coefficient,
                there is a column with standard errors.
                Standard error is the square root of the Estimated Variance.
                Estimated Variance for a single variable linear model is defined as:
                    ```
                    s2(β1^) = ∑i=1n (yi−yi^)^2 / (n−2) * ∑i=1n(xi−x¯)^2 = SSE / (n−2) * ∑i=1n(xi−x¯)^2
                    ```
                This formulation can be shown by taking the variance of our estimated `β1^` but
                we will not cover that here. Analyzing the formula term by term we see that the
                numerator, SSE, represents the error within the model. A small error in the model
                will then decrease the coefficient's variance. The denominator term, `∑i=1n(xi−x¯)^2`,
                measures the amount of variance within x. A large variance within the independent
                variable decreases the coefficient's variance. The entire value is divided by (n-2)
                to normalize over the SSE terms while accounting for 2 degrees of freedom in the model.

                Using this variance we will be able to compute t-statistics and confidence intervals
                regarding this `β1`. We will get to these in the following screens.
                * Compute s2(β1^) (i.e. `s2b1`) for `linearfit`
                    ```
                    # Enter your code here.
                    # Compute SSE
                    SSE = np.sum((y.values - yhat)**2)
                    # Compute variance in X
                    xvar = np.sum((pisa.year - pisa.year.mean())**2)
                    # Compute variance in b1
                    s2b1 = SSE / ((y.shape[0] - 2) * xvar)
                    ```
            * T-Distribution
                * Statistical tests can be done to show that the lean of the tower is dependent on
                the year. A common test of statistical significance is the student t-test.
                The student **t-test** relies on the **t-distribution**, which is very similar to the
                **normal distribution**, following the bell curve but with a lower peak.
                where **T-Distribution** accounts for the number of observations in our sample set
                while **Normal Distribution** assumes we have the entire population.
                In general, the smaller the sample we have the less confidence we have in our estimates.
                The **t-distribution** takes this into account by increasing the variance relative to the number
                of observations. You will see that as the number of observations increases the
                t-distribution approaches the normal distribution.
                The density functions of the t-distributions are used in significance testing.
                The **probability density function (pdf)** models the relative likelihood of a
                continuous random variable.
                The **cumulative density function (cdf)** models the probability of a random variable
                being less than or equal to a point.
                The **Degrees of Freedom (df)** accounts for the number of observations in the sample.
                In general the degrees of freedom will be equal to the number of observations minus 2.
                Say we had a sample size with just 2 observations, we could fit a line through them
                perfectly and no error in the model.
                To account for this we subtract 2 from the number of observations to compute the
                degrees of freedom.

                Scipy has functions in the library `scipy.stats.t` which can be used to compute
                the pdf and cdf of the t-distribution for any number of degrees of freedom.
                `scipy.stats.t.pdf(x,df)` is used to estimate the pdf at variable x with df degrees
                of freedom.

                * Make a plot comparing 2 pdfs of the t-distribution with different degrees of freedom.
                  With the `x` variable given, compute the pdf with 3 degrees of freedom and assign it to variable `tdist3`.
                  Then compute a similar pdf with 30 degrees of freedom and assign it to variable `tdist30`.
                  On a single plot, plot x on the x-axis for both `tdist3` and `tdist30` on the y-axis.
                    ```
                    from scipy.stats import t

                    # 100 values between -3 and 3
                    x = np.linspace(-3,3,100)

                    # Compute the pdf with 3 degrees of freedom
                    print(t.pdf(x=x, df=3))
                    # Pdf with 3 degrees of freedom
                    tdist3 = t.pdf(x=x, df=3)
                    # Pdf with 30 degrees of freedom
                    tdist30 = t.pdf(x=x, df=30)

                    # Plot pdfs
                    plt.plot(x, tdist3)
                    plt.plot(x, tdist30)
                    ```

            * Statistical Significance of Coefficients

                * Use **T-Distribution** for **Signficance Testing**

                * **Signficance Testing**

                    * STEPS:
                        * State Hypothesis:
                            * Test if the tower's lean depends on a year (i.e. each year the tower
                            leans a certain amount)

                                * **Null Hypothesis** -
                                Lean of the tower of Pisa does not depend on the year
                                i.e. Coefficient of 0
                                `H0:β1 = 0`

                                    * Test Null Hypothesis using T-Distribution,
                                    where the **T-Statistic** is defined as `t=|^β1−0| / sqrt( s^2 * (^β1) )`
                                    where `^β1` means `hat symbol on top of β1`
                                    which measures how many Standard Deviations the
                                    Expected Coefficient is from 0.

                                        * If `β1` is far from 0, with Low Variance, then `t` is Very High
                                        * In a Probability Density Function (PDF) a T-Statistic far from 0
                                        has a Very Low **probability**

                                    * Computing the T-Statistic of `β1`:

                                        ```
                                        # Assume the variable s2b1 (varience of B1 computed earlier)
                                        is in memory.  The variance of beta_1
                                        tstat = linearfit.params["year"] / np.sqrt(s2b1)
                                        ```

                                * **Alternative Hypothesis** -
                                Lean of tower depends on the year
                                i.e. Coefficient is NOT 0
                                `H1:β1 ≠ 0`

                * **P-Value**
                    * Given the computed **T-Statistic**, we now Test the Coefficient
                    which is the probability of `β1` being different than 0 at some
                    significance level. If we choose to use say the
                    "95% Significance Level" (i.e. 95% certain that `β1` differs from 0)
                    we simply compute the Cumulative Density at a given P-Value and
                    Degrees of Freedom on the **T-Distribution**
                        * **Two-sided Test** is used that tests for **Linear Regression Coefficients**:
                            * If Coefficient is < 0
                            * If Coefficient is > 0

                            * e.g. If amount the tower leans X metres per year could be
                             either Positive or Negative then we much check both possibilities
                             at a Significance Level.
                             So to Test if `β1` is either Positive or Negative at "95% Confidence Interval"
                             then inspect the 2.5 and 97.5 percentiles in the PDF, since doing so
                             leaves 95% between the two, then just take the absolute value and test
                             only at the 97.5 percentile (i.e. Positive side) since the **T-Distribution**
                             is symmetrical.
                             If probability is > 0.975 then we can Reject the **Null Hypothesis** (`H0`)
                             and say the year significantly affects the lean of the tower
                                * i.e.
                                    ```
                                    If Tcdf(|t|,df) < 0.975
                                        Accept H0: β1 = 0
                                    Else:
                                        Accept H1: β1 ≠ 0

                                    ```
                    * Example
                        * Do we accept `β1` > 0 (i.e. `beta1_test` True or False)
                        (i.e. by checking if `pval` < `p`)
                            ```
                            # At the 95% confidence interval for a two-sided t-test we must use a p-value of 0.975
                            pval = 0.975

                            # The degrees of freedom
                            df = pisa.shape[0] - 2

                            # The probability to test against
                            p = t.cdf(tstat, df=df)
                            beta1_test = p > pval
                            ```

    * **Overfitting** Identification

        * Overfitting is exploered here at a deeper level and introduced related terminology that you'll see in other
        literature as well. So far, we've mostly dealt with supvervised machine learning models to solve
        regression and classification problems

        * Given the car dataset (i.e. where target column was say `mpg`)
            ```
            import pandas as pd
            columns = ["mpg", "cylinders", "displacement", "horsepower", "weight", "acceleration", "model year", "origin", "car name"]
            cars = pd.read_table("auto-mpg.data", delim_whitespace=True, names=columns)
            filtered_cars = cars[cars['horsepower'] != '?']
            filtered_cars['horsepower'] = filtered_cars['horsepower'].astype('float')
            ```

        * **Bias and Variance**
            * **Bias** and **Variance** are at the heart of understanding **Overfitting**.
            * They comprise 2 observable sources of error in a model that we can indirectly control.
            * **Bias**
                * describes error that results in bad assumptions about the learning algorithm.
                i.e. assuming only one feature `weight` relates to a car's fuel efficiency
                will lead us to fitting a simple, Univariate regression model that will result in **High Bias**
                and **High Error rate** since a car's fuel efficiency is actually affected by many other factors
                besides just its weight.

            * **Variance**
                * describes **error** that occurs because of the variability of a model's predicted values.
                If we were given a dataset with 1000 features on each car and used every single feature
                to train an incredibly complicated Multivariate regression model, we will have **Low Bias**
                but **High Variance**

        * Ideally we want **Low Bias** and **Low Variance** but in reality, there's always a **Trade-Off**.

        * **Bias-Variance Trade-Off (BVTO)**
            * Dfn:
                * Previously we learnt that Overfitting happens when a model performs well on a Training set
                but doesn't generalize well to new data. A key nuance here is that you should think of overfitting
                as a relative term. Between any 2 models, one will overfit more than the other one.
                * Understanding the **Bias-Variance Trade-Off** is critical to understanding **Overfitting**.
                Every process has some amount of inherent noise that's unobservable.
                **Overfit** models tend to capture the **noise** as well as the **signal** in a dataset.
                Scott Fortman Roe's blog post on BVTO has a wonderful image that describes this tradeoff:
                    https://en.wikipedia.org/wiki/Bias%E2%80%93variance_tradeoff
                    http://scott.fortmann-roe.com/docs/BiasVariance.html

                * Optimum Model Complexity is where total error is lowest, Variance is lowest, and Bias is lowest
                * Bias error decreases with increasing model complexity
                * Variance error increases with increasing model complexity

                * **Bias** of a model is approximated by training a few different models from the same class
                (linear regression in this case) using different features on the same dataset and
                calculating their error scores. For regression, we can use MAE, MSE, or R-squared.
                * **Variance** of predicted values for each model we train may be calculated and we'll observe
                an increase in variance as we build more complex, multivariate models.

                * Univariate simple linear regression model will **Underfit**, conversely an
                extremely complicated, Multivariate linear regression model will **Overfit**.
                The middle ground helps (shown in diagram of Scott Fortmann's post) helps
                you construct reliable and useful predictive models.

                * Example:
                    * Create a function for training the model and computing the Bias and Variance values
                    and use it to train some simple, Univariate models.
                    * STEP 1:
                        ```
                            - Train a Linear Regrsesion Model using:
                                - Target Variable of "mpg"
                                - Training Column Features
                            - Use the Trained Model to make predictions using the sampe input it was trained on
                            - Variance computed of the Predicted values and the MSE between the
                            Predicted Values and the Actual Label "mpg" Column
                            - Return MSE and Variance (i.e. train_and_test = return(mse, variance) )
                            - Use train_and_test function to train a modle using only a Univariate
                            `cylinders` column, and assign resulting MSE and Variance to `cyl_mse` and `cyl_var`
                            - Use train_and_test function to train a modle using only a Univariate
                            `weight` column, and assign resulting MSE and Variance to `weight_mse` and `weight_var`

                        ```
                        * Answer:
                            ```
                            from sklearn.linear_model import LinearRegression
                            from sklearn.metrics import mean_squared_error
                            import numpy as np
                            import matplotlib.pyplot as plt
                            def train_and_test(cols):
                                # Split into features & target.
                                features = filtered_cars[cols]
                                target = filtered_cars["mpg"]
                                # Fit model.
                                lr = LinearRegression()
                                lr.fit(features, target)
                                # Make predictions on training set.
                                predictions = lr.predict(features)
                                # Compute MSE and Variance.
                                mse = mean_squared_error(filtered_cars["mpg"], predictions)
                                variance = np.var(predictions)
                                return(mse, variance)

                            cyl_mse, cyl_var = train_and_test(["cylinders"])
                            weight_mse, weight_var = train_and_test(["weight"])
                            ```
                    * STEP 2:
                        * Use the same function `train_and_test` for training a regression model
                        and calculating the MSE and Variance to
                        train and understand more Complex models
                            ```
                            columns: cylinders, displacement.
                            MSE: two_mse, variance: two_var.
                            columns: cylinders, displacement, horsepower.
                            MSE: three_mse, variance: three_var.
                            columns: cylinders, displacement, horsepower, weight.
                            MSE: four_mse, variance: four_var.
                            columns: cylinders, displacement, horsepower, weight, acceleration.
                            MSE: five_mse, variance: five_var.
                            columns: cylinders, displacement, horsepower, weight, acceleration, model year
                            MSE: six_mse, variance: six_var.
                            columns: cylinders, displacement, horsepower, weight, acceleration, model year, origin
                            MSE: seven_mse, variance: seven_var.
                            ```
                        * Note: When using whole dataset to train, it's not necessary to Shuffle or Randomise the data order

                        * Answer:
                            ```
                            # Our implementation for train_and_test, takes in a list of strings.

                            def train_and_test(cols):
                                # Split into features & target.
                                features = filtered_cars[cols]
                                target = filtered_cars["mpg"]
                                # Fit model.
                                lr = LinearRegression()
                                lr.fit(features, target)
                                # Make predictions on training set.
                                predictions = lr.predict(features)
                                # Compute MSE and Variance.
                                mse = mean_squared_error(filtered_cars["mpg"], predictions)
                                variance = np.var(predictions)
                                return(mse, variance)

                            one_mse, one_var = train_and_test(["cylinders"])
                            two_mse, two_var = train_and_test(["cylinders", "displacement"])
                            three_mse, three_var = train_and_test(["cylinders", "displacement", "horsepower"])
                            four_mse, four_var = train_and_test(["cylinders", "displacement", "horsepower", "weight"])
                            five_mse, five_var = train_and_test(["cylinders", "displacement", "horsepower", "weight", "acceleration"])
                            six_mse, six_var = train_and_test(["cylinders", "displacement", "horsepower", "weight", "acceleration", "model year"])
                            seven_mse, seven_var = train_and_test(["cylinders", "displacement", "horsepower", "weight", "acceleration","model year", "origin"])
                            ```

                    * STEP 3: **Cross Validation & Overfitting detection**

                        * Multivariate regression models you trained got progressively better at reducing the amount of error.
                        Detect if model is Overfitting by comparing the **In-sample error** and the **Out-of-sample error**,
                        or the **Training error** with the **Test error**.
                        So far, we calculated the **in-sample error** by testing the model over the same data it was trained on.
                        To calculate the **Out-of-sample error**, we need to test the data on a test set of data.
                        We unfortunately don't have a separate test dataset and we'll instead use **Cross validation**.
                        If a model's **cross validation error (out-of-sample error)** is much higher than the **in-sample error**,
                        then your data science senses should start to tingle.
                        This is the first line of defense against **Overfitting** and is a clear indicator that the trained model
                        doesn't generalize well outside of the training set.
                        Let's create a new function to handle performing the cross validation and computing the
                        cross validation error.
                            ```
                            Create a function named train_and_cross_val that:
                            - takes in a single parameter (list of column names),
                            - trains a linear regression model using the features specified in the parameter,
                            - uses the KFold class to perform 10-fold validation using a random seed of 3 (we use this seed to answer check your code),
                            - calculates the mean squared error across all folds and the mean variance across all folds.
                            - returns the mean squared error value then the variance using a multiple return statement (e.g. return(avg_mse, avg_var)).

                            Use the train_and_cross_val function to train linear regression models using the following columns as the features:
                            - the cylinders and displacement columns. Assign the resulting mean squared error value to two_mse and the resulting variance value to two_var.
                            - the cylinders, displacement, and horsepower columns. Assign the resulting mean squared error value to three_mse and the resulting variance value to three_var.
                            - the cylinders, displacement, horsepower, and weight columns. Assign the resulting mean squared error value to four_mse and the resulting variance value to four_var.
                            - the cylinders, displacement, horsepower, weight, acceleration columns. Assign the resulting mean squared error value to five_mse and the resulting variance value to five_var.
                            - the cylinders, displacement, horsepower, weight, acceleration, and model year columns. Assign the resulting mean squared error value to six_mse and the resulting variance value to six_var.
                            - the cylinders, displacement, horsepower, weight, acceleration, model year, and origin columns. Assign the resulting mean squared error value to seven_mse and the resulting variance value to seven_var.
                            ```
                            * Answer:
                                ```
                                from sklearn.cross_validation import KFold
                                from sklearn.metrics import mean_squared_error
                                import numpy as np
                                def train_and_cross_val(cols):
                                    features = filtered_cars[cols]
                                    target = filtered_cars["mpg"]

                                    variance_values = []
                                    mse_values = []

                                    # KFold instance.
                                    kf = KFold(n=len(filtered_cars), n_folds=10, shuffle=True, random_state=3)

                                    # Iterate through over each fold.
                                    for train_index, test_index in kf:
                                        # Training and test sets.
                                        X_train, X_test = features.iloc[train_index], features.iloc[test_index]
                                        y_train, y_test = target.iloc[train_index], target.iloc[test_index]

                                        # Fit the model and make predictions.
                                        lr = LinearRegression()
                                        lr.fit(X_train, y_train)
                                        predictions = lr.predict(X_test)

                                        # Calculate mse and variance values for this fold.
                                        mse = mean_squared_error(y_test, predictions)
                                        var = np.var(predictions)

                                        # Append to arrays to do calculate overall average mse and variance values.
                                        variance_values.append(var)
                                        mse_values.append(mse)

                                    # Compute average mse and variance values.
                                    avg_mse = np.mean(mse_values)
                                    avg_var = np.mean(variance_values)
                                    return(avg_mse, avg_var)

                                two_mse, two_var = train_and_cross_val(["cylinders", "displacement"])
                                three_mse, three_var = train_and_cross_val(["cylinders", "displacement", "horsepower"])
                                four_mse, four_var = train_and_cross_val(["cylinders", "displacement", "horsepower", "weight"])
                                five_mse, five_var = train_and_cross_val(["cylinders", "displacement", "horsepower", "weight", "acceleration"])
                                six_mse, six_var = train_and_cross_val(["cylinders", "displacement", "horsepower", "weight", "acceleration", "model year"])
                                seven_mse, seven_var = train_and_cross_val(["cylinders", "displacement", "horsepower", "weight", "acceleration","model year", "origin"])
                                ```

                    * STEP 4: Plot **Cross-Validation Error** vs **Cross-Validation Variance**

                        * Cross Validation lowers the MSE with the more Features that are added
                        which is good since it means the model generlises well to new data it was not trained on.
                        However as MSE decreases, the Variance of predictions increases (as expected since
                        models with lower MSE had higher complexity and are more sensitive to Variations
                        in input values i.e. High Variance)

                        * Example:
                            * Plot the MSE and Variance for model to understand the Trade-Off
                            as the number of Features increases
                            http://matplotlib.org/api/pyplot_api.html#matplotlib.pyplot.scatter
                            https://www.dataquest.io/course/exploratory-data-visualization
                                * Generate a scatter plot with the model's number of features on the x-axis
                                and the model's overall, cross-validation MSE on the y-axis.
                                Use Red for the scatter dot color.
                                * Generate a scatter plot with the model's number of features on the x-axis
                                and the model's overall, cross-validation Variance on the y-axis.
                                Use Blue for the scatter dot color.

                        * Conclusion:
                            * Higher order Multivariate models **Overfit** in relation to the
                            Lower order Multivariate models, the **in-sample error** and **out-of-sample**
                            didn't deviate by much. The best model was around 50% more accurate than the simplest model.
                            On the other hand, the overall Variance increased around 25% as we increased the model
                            complexity. This is a really good starting point, but your work is not done!
                            The increased variance with the increased model complexity means that your model
                            will have more unpredictable performance on truly new, unseen data.
                            If you were working on this problem on a data science team,
                            you'd need to confirm the predictive accuracy of the model using completely new,
                            unobserved data (e.g. maybe from cars from later years). Since often you can't wait
                            until a model is deployed in the wild to know how well it works, the exploration we
                            did in this mission helps you approximate a model's real world performance.

