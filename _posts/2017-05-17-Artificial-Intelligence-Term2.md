---
layout: post
title: Artificial Intelligence Nanodegree Term 2
---

# Table of Contents
  * [Chapter 1 - Deep Learning](#chapter-1)
  * [Chapter 2 - Convolutional Neural Networks (CNN)](#chapter-2)

## Chapter 1 - Deep Learning<a id="chapter-1"></a>

### Links

* Forum https://discussions.udacity.com

* Other Solutions
    * https://github.com/lemuelbarango/dog-breed-classifier
    * https://github.com/binojohnthomas/AIND-RNN
    * https://github.com/morganel?tab=repositories

### Concentrations

* About
    * introductory project that utilizes a commercially-available API to build a complete solution, and,
    * capstone project where you will try to solve one challenging problem in the chosen area

* Concentrations

    * NLP - label words in sentence with Part-of-Speech (POS) tags as named entities
        * Grammars https://classroom.udacity.com/courses/cs101/lessons/48299949/concepts/487192400923
    * VUI
        * Links
            * https://developer.amazon.com/alexa-skills-kit/alexa-skills-developer-training
        * Applications
            * Apple Siri
            * Microsoft Cortana
            * Google Home
            * Amazon Alexa on Echo
        * Device
            * Amazon Echo Dot V2 http://www.smarthome.com.au/z-wave/z-wave-accessories.html
* Previews
    * Voice Preview https://classroom.udacity.com/nanodegrees/nd889/parts/16cf5df5-73f0-4afa-93a9-de5974257236/modules/38e74312-3173-4456-919d-bcb00a82bfb5/lessons/dc1efdfd-e07f-4a5c-ab35-dbb274a25c88/concepts/last-viewed
    * NLP Preview https://classroom.udacity.com/nanodegrees/nd889/parts/16cf5df5-73f0-4afa-93a9-de5974257236/modules/ac7813e7-2907-44e4-a9a5-388bcc4edd38/lessons/5f3de1f2-df97-46c4-a2ba-82418c66f9e5/concepts/last-viewed
    * Computer Vision Preview https://classroom.udacity.com/nanodegrees/nd889/parts/16cf5df5-73f0-4afa-93a9-de5974257236/modules/0917fcad-9a95-401c-9c92-8cec8f6dc09e/lessons/260fb8ce-eb1d-4ea2-864b-d8ed31b7082f/concepts/last-viewed

### TODO

* AWS Training Free
    * https://www.aws.training
    * https://www.awseducate.com/microsite/Training

### Instructors

* Luis Serrano - Course Developer / Instructor (did ML for Google Youtube recommendations)
* Alexis Cook - Applied maths / biologist, uses Deep Learning @alexis_b_cook
* Arpan Chakraborty - Builds Computer Vision and Machine Learning courses
* San Camacho - Expert in Computer Vision (applied in medical tech to self-driving car navigation)
* Dana Sheehan - Elec Engineer, MSc with interest in AI
* Jeremy Watt - ML engineer educator and uni textbook author, likes NLP and Computer Vision, and maths optimisation, wrote "Machine Learning Refined"
* Raisa Honey - Deep Learning researcher with teaching experience in ML, wrote "Machine Learning Refined"

### Deep Learning

* Applications
    * Defeating humans in games
    * Detecting spam emails
    * Forecasting stock prices
    * Recognising images in pictures
    * Diagnosing illnesses
    * Self driving cars

* Components
    * **Neural Networks**
        * Dfn: Mimics way brain operates with neurons that fire off info
        * Way to visualise Neural Networks: Given some data comprising groups of Red and Blue points,
        then Neural Networks find the best line that separates the groups. Or for complex data, the boundary
        to separate the points will be more complex
        * Types:
            * **Deep Neural Networks** - many Nodes, Edges, Layers

* **Classification Problems**
    * Example 1: Predict whether student is admitted into university by analysing
                 n-Columns of known admissions data samples from prior student applicants
                 and Plot data on graph in n-Dimensions. Create an equation/Model
                 that generates a "Line" (if 2 columns, 2D), "Plane" (if 3 Columns, 3D),
                 or "Hyper Plane" (if n Columns, n-D) that is a Linear Demarcation Boundary
                 between all known samples points that were "Accepted" and "Rejected"
        ```
        Problem

            - Given students, results of Tests and Grades results, and an Admissions officer
            - Given known admission samples, predict whether another student admitted
            - Known (previous data):
                Accepted - Student 1 - Test 9/10, Grade 8/10
                Rejected - Student 2 - Test 3/10, Grade 4/10
            - Unknown:
                ?        - Student 3 - Test 7/10, Grade 6/10

        Solution

            - Determine how many Columns we want to plot to determine
              how many Dimensions the plot will be in (including "Higher Dimensions")
                i.e.
                    2x Columns - Plot in 2D with demarcation boundary "Line"
                    3x Columns - Plot in 3D with demarcation bounary "Plane"
                    nx Columns - Plot in nD with demarcation boundary "Hyper Plane"

            - Plot all Known Test results
                i.e. if 2D then  (X1 (X-axis), Grade results on X2 (Y-axis)

            - Create Demarcation Boundary representing our Model
              between where students likely accepted/rejected.

                - Linear Equation represents the demarcation of the Model, and
                  if result of substituting Unknown prediction Grade and Test results
                  into equation are score < 0 then Reject, or if >= 0 then Accept.


                    i.e.
                        W1 * X1 + W2 * X2 + b * 1 = 0  # product of two Vectors
                        W * x             + b = 0      # abbreviated in Vector notation
                            where W = (W1, W2)         # Vector W - Weights
                            where x = (X1, X2)         # Vector x - Inputs
                            where b                    # Bias
                            where Y = 0 or 1           # Label that we're trying to predict
                                                       # for given coordinates (X1, X2)
                                                       #   Y == 0 if Rejected (under demarcation line of Model)
                                                       #   Y == 1 if Accepted (above demarcation line of Model)

                        Note: Points are of form (X1, X2, Y)

                        y^ = 1 if Score of W * x + b >= 0       # y Hat is what algorithm Predicts
                             0 if Score of W * x + b < 0        # the labels will be

                             Note: Points on the Demarcation Boundary Line have Score of 0 when
                             substitute coordinates into equation.

                        Note: Goal of algorithm is to have y^ (prediction) resembling Y (actual)
                        as closely as possible (i.e. finding the demarcation boundary line that
                        keeps the previous Y == 1 above, and Y == 0 below it)


                - If more than >= 3x Data Columns (i.e. Test Result, Grade Result, Class Rank)
                  we Fit the data by using >= 3x Dimensions/axis (i.e. X1, X2, X3)

                    i.e. X1 (X) - Test
                         X2 (Y) - Grades
                         X3 (Z) - Class Rank

                    - Plot each sample as a point on the graph
                    - Demarcation Boundary line is plotted as a 3D "Plane" (possibly on an angle)

                        W1 * X1 + W2 * X2 + W3 * X3 + b * 1 = 0  # product of three Vectors
                        W * x             + b = 0                # abbreviated in Vector notation

                        y^ = 1 if W * x + b >= 0       # y Hat is what algorithm Predicts
                             0 if W * x + b < 0        # the labels will be

                    - Colour each sample depending on the Region of the sample (from 2x Regions available)
                    (i.e. whether above or below the "Plane")

                - If have 'n' Data Columns we Fit the data in 'n'-dimensional space,
                  where each sample Point contains coordinates (i.e. (X1, X2, ..., Xn) ),
                  and where Label is Y, then:

                    - Demarcation Boundary Line:

                        (n - 1) Dimensional "Hyper Plane"

                        (i.e. a High Dimensional Equivalent of a "Line" in 2D or a "Plane" in 3D)

                    - Equation

                        W1 * X1 + W2 * X2 + Wn * Xn + b * 1 = 0  # product of 'n' Vectors
                                                                 # (each Vector has 'n' entries, one for each Column from Data set

                        W * x             + b = 0                # abbreviated in Vector notation

                        y^ = 1 if W * x + b >= 0       # y Hat is what algorithm Predicts
                             0 if W * x + b < 0        # the labels will be


            - Solve Unknown prediction by plotting them on most likely demarcaton side.
              Whilst the Model will make some mistakes we can assume this
              prediction is correct with some confidence

        ```

* **Perceptrons**
    * Dfn:
        * **Perceptrons** are called **Neural Networks** since they look like
        the Neural Networks in the **Brain**.
        Perceptrons calculate an equation in Node 1 based on Input Node values.
        Similarly, the Brain Neuron takes values from "Dendrites" Input Nodes
        (nervous impulses), processes them, and decides whether to output a
        nervous impulse via the Axon.
        Create **Neural Networks** by concatenating Multi-Layered (multiple) Perceptrons to mimic
        the way the Brain connects Neurons by making successive outputs the input to the next.
        * Visualise

            ```

            INPUT NODES             NODE 1 - SUMMATION -                    NODE 2 - STEP -
                                             CALC LINEAR EQN ON INPUTS               APPLIES STEP
                                             ON THE WEIGHTS                          FN TO RESULT
            ===========             ==================================      =====================

            | X1 |  --- W1 --->

            | X2 |  --- W2 --->     LINEAR FUNCTION / PLOT                  STEP FUNCTION

            ...                     | Wx + b = (n ∑ i=1) WiXi + b |  ---->  | Wx + b >= 0 ???     |  --->  YES: 1
                                    |                             |         | Wx + b < 0 ???      |        NO:  0
            | Xn |  --- Wn --->

            | 1  |  --- b  --->
            ```

        * NOTE: In the future we'll use different **Step Functions**

        * Building block of Neural Networks where we encode the Equation
        (that defines our Model) into a small graph

        * Build the Perceptron by:
            * Create Model Plot Node containing our Plot inside (showing our Boundary Line and Data Points)
            * Create Input Nodes (i.e. for each Sample value for all Columns) to the Model Node
            * Perceptron will plot the Sample values at a point on the Model Node Plot
            and checks if point is in Positive Region (returns Yes) or Negative Region (returns No)

                ```
                i.e.
                    Given equation:

                    0 = W1 * X1 + W2 * X2 + b * 1          # Linear Equation Boundary

                    Substitute:

                    Score = 2 * Test + 1 * Grade - 18 * 1  # Linear Equation with Weights
                                                           # and Input Types substituted


                    i.e. if we have the following:

                        Weight 1 (Test) : 2
                        Test result     : 7
                        Weight 2 (Grade): 1
                        Grade result    : 6
                        Bias unit       : -18

                        we then plot the following on the Perceptron:

                        - Point (7,6)   on the plot
                        - Edge  2       between Input Node (Test) and Model Plot Node
                        - Edge  1       between Input Note (Grade) and Model Plot Node
                        - Bias  -18     label this value over the Model Plot Node

                    Outcome:
                        Now when we see a Perceptron having Nodes with these labels,
                        we can think of the Linear Equation the nodes generate

                        i.e.

                            | TEST = 7   |   ---- 2 ---->
                                                           | -18 |
                            | GRADES = 6 |   ---- 1 ---->



                    Alternative:
                        Alternatively can include the Bias as an Input Node
                        (i.e. think of it in the equation as b * 1)
                        and have b labelling an Edge coming from a 1

                        Then the Model Plot Note multiplies the values from
                        the incoming Nodes by the values on the corresponding
                        Edges

                        i.e.

                            | TEST = 7   |   ---- 2 ---->
                                                           | SCORE = 1 * 7 + 2 * 6 - 18 * 1 = 1 |
                            | GRADES = 6 |   ---- 1 ---->

                            | BIAS = 1   |   ---- -18 -->

                        Finally it checks if SCORE is >= 0 or < 0
                        and returns:

                            1 (i.e. YES) for SCORE >= 0  signalling student accepted
                            0 (i.e. NO) for SCORE < 0


                        i.e. in general

                            | X1 |  --- W1 --->

                            | X2 |  --- W2 --->

                            ...                     | Wx + b = (n ∑ i=1) WiXi + b |  ---->   YES: 1
                                                    | Wx + b >= 0 ???             |          NO:  0
                            | Xn |  --- Wn --->

                            | 1  |  --- b  --->

                        Note: we are implicitly using the "Step Function"
                        (i.e. returns a 1 if Input Positive, and returns 0 if Input Negative)


                ```
                * Use the "Weights" as Labels in the Plot, since they define
                the Linear Equation itself

        * Example: Perceptrons as Logic Operators
            * Create Perceptrons for logic operators including AND, OR, NOT, and XOR.
                https://www.youtube.com/watch?v=45K5N0P9wJk

                * AND Perceptron
                    ```
                    i.e.
                                                    Plots Bounary line from
                                                    substituting inputs
                                                    Weights + Bias' into
                                                    equation. Then plot each point
                                                    and returns
                                                    1 if point in Positive region
                                                    and returns 0 if point in
                                                    Negative region (i.e. below
                                                    Boundary diagonal line that
                                                    represents the equation
                                                    and crosses just below (1, 1))

                        |  INPUT1   |   -------->
                                                    | AND     |   -------> OUTPUT
                        |  INPUT2   |   -------->


                        Truth Table
                        ========================
                        INPUT1   INPUT2   OUTPUT
                        ------------------------
                        T        T        T
                        T        F        F
                        F        T        F
                        F        F        F

                        Perceptron Table
                        ========================
                        INPUT1   INPUT2   OUTPUT
                        ------------------------
                        1        1        1
                        1        0        0
                        0        1        0
                        0        0        0
                    ```
                    * Code Implementation
                        ```
                        import pandas as pd

                        # TODO: Set weight1, weight2, and bias
                        weight1 = 0.0
                        weight2 = 0.0
                        bias = 0.0


                        # DON'T CHANGE ANYTHING BELOW
                        # Inputs and outputs
                        test_inputs = [(0, 0), (0, 1), (1, 0), (1, 1)]
                        correct_outputs = [False, False, False, True]
                        outputs = []

                        # Generate and check output
                        for test_input, correct_output in zip(test_inputs, correct_outputs):
                            linear_combination = weight1 * test_input[0] + weight2 * test_input[1] + bias
                            output = int(linear_combination >= 0)
                            is_correct_string = 'Yes' if output == correct_output else 'No'
                            outputs.append([test_input[0], test_input[1], linear_combination, output, is_correct_string])

                        # Print output
                        num_wrong = len([output[4] for output in outputs if output[4] == 'No'])
                        output_frame = pd.DataFrame(outputs, columns=['Input 1', '  Input 2', '  Linear Combination', '  Activation Output', '  Is Correct'])
                        if not num_wrong:
                            print('Nice!  You got it all correct.\n')
                        else:
                            print('You got {} wrong.  Keep trying!\n'.format(num_wrong))
                        print(output_frame.to_string(index=False))
                        ```

                * OR Perceptron
                    * Similar to AND Perceptron
                    * The gradient of the Demarcation Boundary is same as AND Perceptron
                    but is shifted down by using Different Weights and Bias


                * XOR Perceptron

                   ```
                   Perceptron Table
                   ========================
                   INPUT1   INPUT2   OUTPUT
                   ------------------------
                   1        1        0
                   1        0        1
                   0        1        1
                   0        0        0
                   ```

    * Tricks
        * Trick for Perceptron Algorithm to Split Data Points and Adjust Linear Equation
            * STEP 1:
                * Choose random linear equation that defines a line
                with a Positive area and Negative area (each side of line)
            * STEP 2:
                * Points indicate if they are Correctly or Incorrectly
                Classified (i.e. whether on correct side or not)
                so we may improve the line
                    * Misclassified point indicates to line to come closer to it
            * STEP 3:
                * Adjust the linear equation (and associated line)
                based on feedback from points instructing line how to move
                ```
                i.e.
                    given Linear Equation and relative regions:

                        Positive Region             3 * x1 + 4 * x2 - 10 > 0
                        LINE                        3 * x1 + 4 * x2 - 10 = 0
                        Negative Region             3 * x1 + 4 * x2 - 10 < 0


                    STEP 1: Misclassified Point in POSITIVE Region

                        given a Misclassified Point incorrectly located
                        in the POSITIVE Region:

                            POINT                       (4, 5)

                        move the Point closer to the Line by using the
                        4 and 5 and using them to modify the Linear Equation
                        to make the Line move closer to the Point

                        given the following: Parameters of the Line are:

                            1) Parameters of Line:      3, 4, -10

                            2) Points and Bias Unit:    (4, 5) 1

                        Avoid moving toward this NEW LINE drastically as may
                        result in accidently Misclassifying all other Points,
                        instead we want to make SMALL steps toward the point,
                        by using the LEARNING RATE small number.

                            LEARNING RATE:              0.1

                        now, first Multiple 2) by LEARNING RATE to get 3)

                                                        (4, 5) 1
                                                        4 * 0.1  5 * 0.1  1 * 0.1
                                                        0.4      0.5      0.1

                        and then the Subtract 3) from 1) to get NEW LINE Parameters:

                                                     -  3    4    -10
                                                        0.4  0.5  0.1
                                                      -----------
                                                        2.6  3.5  10.1

                        this gives us a NEW LINE equation of, which will move closer to Point:

                                                        2.6 * x1 + 3.5 * x2 - 10.1 = 0

                    STEP 2: Misclassified Point in NEGATIVE Region

                        Repeat similar to STEP 1, but instead of Subtracting,
                        we instead use Addition


                            1) Parameters of Line:      3, 4, -10

                            2) Points and Bias Unit:    (1, 1) 1

                            LEARNING RATE:              0.1

                                                        (1, 1) 1
                                                        1 * 0.1  1 * 0.1  1 * 0.1
                                                        0.1      0.1      0.1

                                                     +  3    4    -10
                                                        0.1  0.1  0.1
                                                      -----------
                                                        3.1  4.1  -9.9

                                                       3.1 * x1 + 4.1 * x2 - 9.9 = 0
                ```

    * Perceptron STEP Algorithm - **Linear Data**

        * PSEUDOCODE

            ```
            1.  Random Weights

                Start with random weights:              w1, ..., wn, b

                Apply to Line Equestion:                Wx + b = 0      (separates Positive and Negative areas)

            2.  For every misclassified points  (i.e. x1, ..., xn)
                repeat 2.1 and 2.2 until no more erroneously misclassified points

                2.1
                    If `prediction = 0` (i.e. Positive Point Label misclassified in Negative area)

                    Then:
                        Update weights as follows (adding):

                            for i = 1 ... n

                                change wi + α * xi

                                    where α is the Learning Rate (i.e. 0.1)

                            change Bias Unit of b to (b + α)
                                (to move Line closer to Misclassified Point)

                                where b is the Bias Unit

                2.2
                    If `prediction = 1` (i.e. Negative Point Label by missclassified in Positive area):

                    Then:
                        Update the weights as follows (subtracting):

                            for i = 1 ... n

                                change wi - α * xi

                                    where α is the Learning Rate (i.e. 0.1)

                            change Bias Unit of b to (b - α)
                                (to move Line closer to Misclassified Point)

                                where b is the Bias Unit
            ```


        * CODE

            * Implement the Perceptron STEP Algorithm to separate data in a CSV file

                ```
                Perceptron steps:

                    Given:

                        Point Coords            (p, q)
                        Label                   y
                        Prediction Equation     ^y = step(w1 * x1 + w2 * x2 + b)

                    Then:

                        If Point is:
                            Correctly classified
                        Then
                            do nothing

                        If Point is:
                            Incorrectly classified (i.e. Positive classification but has Negative Label)
                        Then
                            SUBTRACT (α * p, α * q, α) from (w1, w2, b)

                        If Point is:
                            Incorrectly classified (i.e. Positive classification but has Negative Label)
                        Then
                            ADD (α * p, α * q, α) to (w1, w2, b)

                ```

            * Graph the output of the Perceptron Algorithm by clicking on `test run`
                * Draws Dotted Lines, showing how algorithm approaches Best solution (Black Solid Line)

            * Note:
                * Modifying Perceptron Algorithm Parameters including:
                    * epochs
                    * Learning Rate
                    * Randomising initial parameters

    * Perceptron STEP Algorithm - **Non-Linear Data**

        * Example - **Error Function** and **Gradient Descent**
            ```
            LINEAR
            ========================
            Given the following data point:
                * Test: 9/10
                * Grades: 1/10

            Using Boundary Line, the student will be
            accepted, since on Positive side of line.


            NON-LINEAR (ALTERNATIVE):
            ========================


            If only want to accept candidates based on
            CUSTOM criteria
            (i.e. must have Test >= 5 and Grades >= 5)
            then we need to Label the data points
            differently, and the Positive and
            Negative region cannot be just a
            Boundary "Line".

            Instead we need to separate with a
            "Circle", "Curve" or "Multiple Lines"

            Redefine the Perceptron Algorithm that
            we created for Boundary "Lines" so that
            it generalises to other types of "Curves"

            "Error Function" is used to
            with an Error Metric (distance) to tell us and
            the computer how badly its doing:
                - show us how far we are from the
                ideal solution so we can repeatedly
                take steps to decrease the error
                to eventually solve the problem:
                    - check in what directions we
                    can take subsequent steps to
                    get closer to the solution
                    - pick direction that takes us
                    the farthest distance
                    (decreases the error distance)

            "Gradient Descent" Method is used to
            overcome issues:

                Issues:
                    - Local Error Minimum (getting stuck)
                        - which often gives good solution to problem


            ```

        * Example - Goal Split Data
        **Discrete Error Fn** vs **Continuous Error Fn** (Differentiable
        using high Penalty weights for misclassified points so allows
        solving problem with **Gradient Descent**)
            ```
            - Given data points plotted.
            - Given a Boundary Line between Positive and Negative region

            - Goal is to inform computer how far it is from
            perfect solution.
                - Count qty of errors (i.e. data points misclassified on wrong side of line)
                - Decrease the qty errors
                    - Check directions can move/rotate
                    the Boundary Line to
                - PROBLEM:
                    - DISCRETE ERROR FN - This algorithm uses Calculus used to take
                    small steps (by taking derivatives) but with
                    small steps, each step may not reduce the qty errors
                    (similar to being on top of a pyramid of steps
                    that say jump from 2 to 1 and to 0, and taking
                    small steps in a direction toward the steps, but
                    confusing since all the steps are x levels from the ground)
                - ALTERNATIVE
                    - CONTINUOUS ERROR FN - Allows use of small steps
                    to indicate which direction will decrease
                    the error the most
                    (since small variations in position translate to
                    small variations in error)
                    The Error Function should also be
                    Differentiable

            - Build a Continous Error Fn.
            - Given plotted points (with say 2 of 6 missclassified)
            wrt to Boundary Line
            - Error Function will assign a Large Penalty
            to the misclassified data points
            (and Small Penalty to correctly classified data points)
            where on the plot we represent the Size of the
            point as the Penalty
                - Misclassified Penalty - approx. the
                distance from Boundary Line.
                - Correctly classified Penalty - close to 0
            - Total Error obtained by counting all errors from
            corresponding points (both correctly classified and misclassified points).
                - TOTAL ERROR = PT1_ERR + PT2_ERR + ... + PTN_ERR
            - Find out what small changes to Boundary Line Parameters that
            will make small changes to Error Function to make the
            TOTAL ERROR decrease, which we will see since the
            misclassified points will now have smaller Penalties
            (i.e. causing error Penalties to change), and then
            take a small step in that direction (each step will
            correctly classify a misclassified point). Repeat
            until reduce the TOTAL ERROR to its minimum possible value
            with all points correctly classified.

            - IMPORTANT NOTE: Since we can build an
            Error Function (Continous) with this Penalty property
            we can now use Gradient Descent to solve the problem
            ```

        * **Predictions**
            ```
            - Predictions are the Output from the Algorithm.
            - Probability is a Function of the distance from
            the Boundary Line.
                - DISCRETE Answer
                    i.e. Yes or No
                    i.e. Labels the data points:
                        - Positive side of Boundary Line w 1
                        - Negative side of Boundary Line w 0
                - CONTINUOUS Answer (probability between 0 and 1):
                    i.e. 63.8%
                    i.e. Labels the data points:
                        - Positive side of Boundary Line w values >= 50%
                        (further away from Boundary Line the higher the %)
                        - Negative side of Boundary Line w values < 50%
                        (further away from Boundary Line the lower the %)
            ```

        * Switch from **Linear** to **Continuous** by changing
        **Activation Function** from Step to Sigmoid:
            * **Compares Step and Sigmoid Perceptrons** https://youtu.be/D5WNzbr6P78?t=3m20s
            * **Step Function** (returns Yes or No) `Step(Wx + b)`
            (with **Boundary Line**)
                * where y == 1 if x >= 0
                * where y == 0 if x < 0
            * **Sigmoid Function** (returns Probability of Yes)
            (with an entire **Probability Space**)
            (where for each data point in plane we are given probability that the
             Label of the point is 1 for Blue points and 0 for Red points)
                * Function that for large Positive numbers returns
                values close to 1 and for large Negative numbers returns
                value close to 0, and for numbers close to 0 returns
                value of 0.5
                https://www.youtube.com/watch?v=D5WNzbr6P78
                    ```
                    σ(x) = 1 / (1 + e^-x)
                    ```
                * i.e. at say Point (0.5, 0.5) the
                probability P(Blue) == 50%, P(Red) == 50%
                and at say Point (0.6, 0.4) the
                probability P(Blue) == 40%, P(Red) == 60%, etc

            * Generate the **Probability Space**:
                * Combine together the:
                    * **Linear Function** - `Wx + b` applied to different lines
                     that represent points where `Wx + b = -n` ... `Wx + b = -4` ... `Wx + b = -2`,
                     ... `Wx + b = 2`, `Wx + b = n`
                    * **Sigmoid Function** `σ(Wx + b)`
                    where `σ(x) = 1 / (1 + e^-x)` applied to each of the
                    potential lines on the plane, we obtain numbers for each respectively
                    from 0 to 1 for each line, which each represent the **Probability**
                    of each point and associated line being Blue or Red (between 0 and 1), where the
                    (i.e. P(Blue)), and where:
                        ```
                        P(Blue) = prediction of model ^y
                                = σ(Wx + b)
                                = % value between 0 and 1 for each line
                        ```
                        * when on Boundary Line `Wx + b = 0`, then Sigmoid `σ(Wx + b) = 0.5`


        * **Multi-Class Classification (3+ classes)** and **Softmax**
            * i.e. classify as **Blue, Red, Green** (instead of just Blue, Red)

            ```
            e.g.

            Bi-Classification Problem:

                Find probability of getting a gift or not.

                    P(gift) = 0.8

                    P(no gift) = 0.2

                - Model takes Inputs (i.e. been good all year, is it your b'day)
                - Model uses Inputs to calculate Linear Model, which is the Score

                    Score(gift) = Linear Function

                So, probability of getting gift or not is simply the
                Sigmoid Function applied to the Score:

                    P(gift) = σ(Score)

            ```

            ```
            Multi-Classification Problem:

                Find probability of what animal we just saw from
                duck, beaver, and walruss?

                i.e. want model to return the following (where combined add to 1):
                    P(duck) == 0.67,
                    P(beaver) == 0.24, and
                    P(walruss) == 0.09

            Given Linear Model based on Inputs:
                Beak?       Boolean
                Teeth Qty?  Int
                Feathers?   Boolean
                Hair?       Boolean

            Calculate Linear Function based on Inputs that ouputs Scores:
                    Score(duck)     = 2     =   e^2 / (e^2 + e^1 + e^0)     =   P(duck)     = 0.67
                    Score(beaver)   = 1     =   e^1 / (e^2 + e^1 + e^0)     =   P(beaver)   = 0.24
                    Score(walrus)   = 0     =   e^0 / (e^2 + e^1 + e^0)     =   P(walrus)   = 0.09

            Convert Scores into Probabilities by using **Softmax Function** which
            applies Exponential Function (e^x returns positive number for every input)
            to each Score to ensure all Scores (outputs of Linear Function)
            will be positive number, where satisfies:
                - Combined probabilities must add to 1
                - Higher Scores should correspond to higher probability proportion

                Note: Cannot just divide each score by sum of all scores to get
                each percentage since possible to get Negative Scores since we're using
                a Linear Function that may give negative values, or denominator may
                become 0. Instead we need to convert all scores into positive scores
                using a function.

            i.e.

                Given quantity of classes:                      N

                Given Linear Model whose Linear Function that
                outputs Scores for each of the n classes:       Z1, ..., Zn

                Convert to Scores into Probabilities by
                saying the Probability that object is in
                class i will be:                                P(class i) = e^Zi / (e^Z1 + ... + e^Zn)
            ```

            * **Softmax Function**
                * i.e. equivalent to Sigmoid Function but for when problem has 3+ classes

                * 2 classes - apply **Sigmoid** Function to Scores to get Probabilities
                * 3+ classes - apply **Softmax** Function to Scores to get Probabilities

        * **One-Hot Encoding**

            * Convert inputs to numbers
                ```
                Gift?
                -----------------------
                True    ->      1
                False   ->      0
                ```


            * With multiple classes:
                ```

                Animal          Value
                -----------------------
                Duck      ->      ?
                Beaver    ->      ?
                Duck      ->      ?
                Walrus    ->      ?
                Beaver    ->      ?

                - Cannot use 1, 2, 3, etc, since
                  assumes dependencies between classes that we cannot have

                - Instead use "One-Hot Encoding" by creating
                  a Variable for each Class (no unnecessary dependencies)
                i.e
                    IF Input is Duck
                    THEN Duck is 1
                    AND Beaver is 0
                    AND Walrus is 0

                Animal      Duck?       Beaver?     Walrus?
                ---------------------------------------------
                Duck        1           0           0
                Beaver      0           1           0
                Duck        1           0           0
                Walrus      0           0           1
                Beaver      0           1           0
                ```

            * **Maximum Likelihood (Probability)** (and **Cross Entropy**)

                * **Avoid taking the PRODUCT**
                * **Instead take the SUM by using LOGS** since
                Logarithms have identity whereby
                    ```
                    log(ab) = log(a) + log(b)
                    ```
                * Use Probability to evaluate and improve our Models
                * Pick the Model that gives Existing Labels the Highest Probability,
                so by Maximising Probability we pick the Best possible Model

                * **Minimise Cross Entropy == Maximise Probability**

                * **Cross Entropy** (Error Function) informs us if the Model
                is Good or bad since it returns the Errors at each point
                (by taking sum of Negatives of the Logarithm of each Probability)
                    * Good Model == Low Cross Entropy == Likely that events happened with given probabilities
                    * Bad Model  == High Cross Entropy == Unlikely that events happened with given probabilities

                    * Negatives of the Logarithms ==  Errors at each Point
                        * where "Correctly Classified Points" have Small Errors
                        * where "Misclassified Points" have Large Errors, such that
                                            Cross Entropy informs us if Model is Good or Bad

                    ```
                    Given 2x Models with just one point each

                    where Model #1 output Probability is 80% (of "win")
                    where Model #2 output Probability is 55% (of "lose")

                    Best Model has Higher Probability when we Actually "win"
                    Best Model has Lower Probability when we Actually "lose"
                    ```

                    ```
                    Given 2x Models with four points each

                    Find out what Model is Good and Bad by:

                        - Calculate Probability of each point being the colour it is
                        according to the Model
                        - Multiply the Probabilities of all the points to get the
                        Model Arrangement Probability

                        Model #1    -   = P(p1_blue, blue) + P(p2_blue, blue) + P(p3_red, red) + P(p4_red, red)
                                        = 0.6 * 0.2 * 0.1 * 0.7
                                        = 0.0084

                        Model #2    -   = P(p1_blue, blue) + P(p2_blue, blue) + P(p3_red, red) + P(p4_red, red)
                                        = 0.7 * 0.9 * 0.8 * 0.6
                                        = 0.3024

                        - Take Logs and Sum using Logarithmic identity function

                        Model #1    -   = - log(0.6) - log(0.2) - log(0.1) - log(0.7)
                                        = 0.51         1.61       2.3        0.36
                                        = 4.8

                        Model #2    -   = - log(0.7) - log(0.9) - log(0.8) - log(0.6)
                                        = 0.36         0.1        0.22       0.51
                                        = 1.2

                        - Take natural Logarithm of e (instead of base 10) by convention.
                        Note that taking Log of value between 0 and 1 is always a Negative number,
                        since Log(1) = 0, now, if we take the Negative of the Logarithm of each
                        Probability, it returns Positive numbers

                        - Best Model has the Highest Model Arrangement Probability


                        - Good Model - Low Cross Entropy
                        - Bad Model  - High Cross Entropy

                    Note:
                        Given the calculated probabilities for two models, if we then
                        take the sum of the negative logarithm of each, and then
                        pair each logarithm with the point where it came from
                        then we get a value for each point, and if calculate each
                        value then we'll find that the Misclassified Points
                        have Large values, whereas Correctly Classified Points
                        have Small values, since a Correctly Classified Point
                        has a Probability close to 1, which when we take the Logarithm of
                        it returns a small value, so we can think of the
                        "Negatives of the Logarithms as Errors at each Point"
                        where "Correctly Classified Points" have Small Errors
                        and "Misclassified Points" have Large Errors, such that
                        Cross Entropy informs us if Model is Good or Bad

                    Calculate Model for Error Function:

                        https://www.youtube.com/watch?v=nV1W7oQOlkU

                        Case #1 (Point is "blue")

                            If y == 1       (if point is "blue"/Accept to begin with)

                            P(blue) = ^y    (prediction y hat, where "blue" point in "blue" area has higher probability
                                             than point in "red" area)

                            Error = -ln(^y)

                        Case #2 (Point is "red")

                            If y == 0       (if point is "red"/Reject to begin with)

                            P(red) = 1 - P(blue)
                                   = 1 - ^y

                                            (prediction y hat, where "red" point in "red" area has higher probability
                                             than point in "blue" area)

                            Error = -ln(1 - ^y)

                        Summarise both Cases:

                            Error = - (1 - y) * ln(1 - ^y) - y * ln(^y)

                                (i.e. if y == 1 then Error is -ln(^y)     )
                                (i.e. if y == 0 then Error is -ln(1 - ^y) )

                        So, ######## Error Function for Binary Classification Problems ########  is:

                            Error Function  = Sum of all the Error Functions of all the Points

                                            = - (1/m) * (   (m ∑ j=1) (1 - yj) * ln(1 - ^yj) + yj * ln(^yj)  )

                                            = - (1/m) * (   4.8                                              )

                                            = 1 (1/4) * (   4.8                                              )

                                            = 1.2

                            Error Function  = E(W, b)

                                            = - (1/m) * (   (m ∑ j=1) (1 - yj) * ln(1 - σ(Wxi + b)) + yj * ln(σ(Wxi + b))  )

                                            where ^y (prediction of model) is given by Sigmoid of Linear Function (Wx + b)
                                                so formula is in terms of Wx and b

                                            where xi is Label

                                            Note: By convention we're taking the Average not the Sum


                            Goal is to Minimise Error Function

                    ```

                    ```
                    Given 2x Models with n points each

                        - Repeat above

                        PROBLEM:

                            - We will be multiplying multiple
                            n values between 0 and 1 together, which would result
                            in a very small number (BAD).
                            - If change one of the values the product output would
                            change drastically

                        SOLUTION:

                            - AVOID USING A FUNCTION THAT TAKES THE "PRODUCT"

                            - USER A FUNCTION TO TAKE THE "SUM" INSTEAD, BY TAKING "LOGS"
                              WHERE   log(ab) = log(a) + log(b)

                            Model #1    -   log(n1) + log(n2) + ... + log(n) = ?
                            Model #2    -   log(n1) + log(n2) + ... + log(n) = ?

                    ```

                * Best Model has Higher Probability and classifies the Most points Correctly
                    https://www.youtube.com/watch?time_continue=188&v=6nUUeQ9AeUA
                * Minimising Error Function results in Best solution
                * Maximum Probability == Minimum Error Function

                * **Cross Entropy** Example

                    ```
                    Given 3x doors with probability of Gift behind them

                                Green door  Red door    Blue door
                    ---------------------------------------------
                    P(gift)     0.8 @       0.7 @       0.1
                    P(no gift)  0.2         0.3         0.9 @

                    Problem:
                        - Find scenario with highest probability, assuming independent events.

                    Solution:
                        - Take Largest probability from each column (indicated with @ symbol)
                        - Whole arrangement probability is Product of 3x Largest probabilities

                            P = 0.8 * 0.7 * 0.9     =   0.504   =   50 %



                    Now, let's look at all possible scenarios,
                    since there are 3 doors, each with 2 possibilities, we have 2^3 scenarios:

                                Green door  Red door    Blue door   Probability     Cross Entropy (i.e. -ln(Probability))
                    ------------------------------------------------------------------------------
                    P(gift)     0.8         0.7         0.1         0.056           2.88
                    P(gift)     0.8         0.7         0.9 !!      0.504           0.69
                    P(gift)     0.8         0.3 !!      0.1         0.024           3.73
                    P(no gift)  0.2 !!      0.7         0.1         0.014           4.27
                    P(gift)     0.8         0.3 !!      0.9 !!      0.216           1.53
                    P(no gift)  0.2 !!      0.3 !!      0.9 !!      0.126           2.07
                    P(no gift)  0.2 !!      0.3 !!      0.1         0.006           5.12
                    P(no gift)  0.2 !!      0.3 !!      0.9 !!      0.054           2.92

                    where !! indicates negative probability of given door for a scenario

                    Step 1:
                        Obtain Probability of each arrangement by multiplying the
                        3x independent probabilities, where Total sum of all arrangement
                        probabilities add to 1:

                    Step 2:
                        Calculate Cross Entropy, which is negative of the logarithm of the probability,
                        such that Events with High Probability have High Cross Entropy

                    ```

                * Derive **Cross Entropy** formula
                    ```
                                Green door  Red door    Blue door     Cross Entropy (i.e. -ln(Probability))
                    ------------------------------------------------------------------------------
                    P(gift)     0.8         0.7         0.9 !!        -ln(0.8) - ln(0.7) - ln(0.9)

                                (p1)        (p2)        (1 - p3)

                                y1 = 1      y2 = 1      y3 = 0

                    where !! indicates the probability of there NOT being a gift behind the door for a given scenario

                    where p1 == 0.8      (prob. of gift)
                    where p2 == 0.7      (prob. of gift)
                    where p3 == 0.1      (prob. of gift)


                    yj = 1               (variable yj is number of presents behind door i, 1 if a present behind, else 0)

                    Cross-Entropy =   - (m ∑ j=1) yj * ln(pj) + (1 - yj) * ln(1 - pj)

                    i.e. CE[(1,1,0), (0.8,0.7,0.1)] = 0.69      low since vector (1,1,0) similar to (0.8,0.7,0.1) meaning
                                                                that arrangment of gifts (1,1,0) is likely to
                                                                happen based on probabilities given (0.8,0.7,0.1)

                         CE[(0,0,1), (0.8,0.7,0.1)] = 5.12      high since arrangement of gifts given by (0,0,1) is
                                                                very unlikely from the probabilities given by second
                                                                set of numbers (0.8,0.7,0.1)
                    ```

                * **Multi-Class Cross Entropy**

                    ```
                    Given 3x doors with probability of Duck, Beaver, or Walrus behind each

                        Animal      Door 1      Door 2      Door 3
                        ---------------------------------------------
                        P(duck)     0.7         0.3         0.1
                        P(beaver)   0.2         0.4         0.5
                        P(walrus)   0.1         0.3         0.4

                    Note: numbers in each column must add to 1


                    Given Scenario #1:

                            Door 1      Door 2      Door 3
                            ------------------------------
                            Duck        Walrus      Walrus

                    P     = 0.7      *  0.3      *  0.4     =   0.084

                    CE    = -ln(0.7) + -ln(0.3)  + -ln(0.4) =   2.48

                    Probability of Scenario #1 is product of probabilities of each independent event


                    Given all Scenarios:

                        Animal      Door 1      Door 2      Door 3
                        ---------------------------------------------
                        P(duck)     p11         p12         p13
                        P(beaver)   p21         p22         p23
                        P(walrus)   p31         p32         p33

                        y1j = 1     if  Duck    behind      Door i
                        y2j = 1     if  Beaver  behind      Door j
                        y3j = 1     if  Walrus  behind      Door k

                        otherwise y1j, y2j, y3j are 0

                        Cross-Entropy   =   - (n ∑ i=1) (m ∑ j=1) yij * ln(pij)

                        ######## Error Function for Multi-Classification Problems ########


                    ```

            * **Logistic Regression** Algorithm

                * About
                    ```
                    - Obtain Input data
                    - Random Model selected
                    - Error calculated
                    - Error minimised to obtain a better model
                    ```

                * Error Function Calculation

                    https://www.youtube.com/watch?v=nV1W7oQOlkU

                    * Recall using Cross-Entropy to calculate the Best Model (when given 2x models) https://www.youtube.com/watch?v=njq6bYrPqSU


            * **Gradient Descent**

                * Dfn: Looks for Direction we will descent the most
                (reduce the Error the most) and takes a Step in that Direction

                https://www.youtube.com/watch?v=26_dnS0r2jc

                * **Error Function** is `E = W(x + b)` (function of weights)

                    * Graphed as a Plane:
                        ```
                        x-axis  w1      (Input to function)
                        y-axis  Error E (height)
                        z-axis  w2      (Input to function)
                        ```

                    * Imagine you are at a Point on the Curved Plane
                        ```
                        E           given by Vector formed by sum of the Partial
                                    Derivatives of Error Function E, with respect to
                                    Weights w1 and w2 and Bias b
                                    (i.e. dE/dw1, dE/dw2), which gives
                                    us the Gradient (i.e. Direction)
                                    to move ∇E
                                    to increase the Error Function
                                    the Most, so if we take the
                                    Negative of the Gradient -∇E
                                    then we can Decrease the
                                    Error Function the Most,
                                    which is what we'll do!

                                    We'll take a Step in the Direction
                                    given by the Negative of the
                                    Gradient at a Point of the
                                    Curved Plane representing the
                                    Error Function, which will take
                                    us to a Lower Point on the Curved Plane.
                                    Repeat this until we get
                                    to Lowest Point on Curved Plane

                        ```

                    * Calculate the Gradient

                        ```
                        1.
                            Initial "Bad" Prediction
                              (since we are high up on
                               Curved Plane's "Error" axis):

                                ^y = σ(Wx + b)    <--- BAD

                                ^y = σ(w1 * x1 + ... + wn * xn + b)

                            Error Function of Initial "Prediction" is:

                                E = W(x + b)

                            "Gradient" of Error Function is:

                                ∇E = (∂E/∂w1, ... , ∂E/∂wn, ∂E/∂b)


                            "Learning Rate" Alpha set to low value
                                to avoid making dramatic changes by using Small


                                α = 0.1

                            "Take a step" in Negative direction of the Gradient
                              multiplied by Alpha

                              Note: where "Taking a step" is same as
                                    Updating the Weights and Bias as follows

                                    (i.e. ∂E/∂wi means Partial Derivative of the Error
                                     with respect to wi)

                                wi'   <--   wi -  α * ∂E/∂wi

                                b'    <--   b  -  α * ∂E/∂b

                            This will take us to a Point E(W', b')
                            with Better "Prediction" that
                            has a Lower Error Function,
                            with Weights W' and Bias b':

                                ^y = σ(W'x + b')   <--- BETTER


                        ```

                * Gradient Calculation https://classroom.udacity.com/nanodegrees/nd889/parts/16cf5df5-73f0-4afa-93a9-de5974257236/modules/6124bd95-dec2-44f9-bf3b-498ea57699c7/lessons/47f6c25c-7749-4a02-b807-7a5b37f362e8/concepts/0d92455b-2fa0-4eb8-ae5d-07c7834b8a56

                    ```
                    Gradient  ==  Scalar  *  Point Coords

                        where,  Scalar  ==  Label  -  Prediction

                        where Gradient is Small
                        when Point is "Well Classified"
                        when Label is close to Prediction
                        so we'll change our Point Coordinates a Little

                        whereas Gradient is Large
                        when Point is "Poorly Classified"
                        when Label is far from Prediction
                        so we'll change our Point Coordinates a Lot


                    Note: Similar to Perceptron Algorithm


                    ```

                * **Gradient Descent Algorithm Pseudocode**
                    https://www.youtube.com/watch?v=I-l32oR5iMM
                    https://classroom.udacity.com/nanodegrees/nd889/parts/16cf5df5-73f0-4afa-93a9-de5974257236/modules/6124bd95-dec2-44f9-bf3b-498ea57699c7/lessons/47f6c25c-7749-4a02-b807-7a5b37f362e8/concepts/ca6eff40-a3e2-4d53-85f4-d2454b538d87
                    Lesson 2 Part 25 https://classroom.udacity.com/nanodegrees/nd889/parts/16cf5df5-73f0-4afa-93a9-de5974257236/modules/6124bd95-dec2-44f9-bf3b-498ea57699c7/lessons/47f6c25c-7749-4a02-b807-7a5b37f362e8/concepts/5e9bd75b-a419-45d4-8a2b-88ba847cc814

                    ```
                    Step 1:

                        Start with random Weights
                        to give us a Line:

                            w1, ... , wn, b

                            ^y = σ(Wx + b)

                    Step 2:

                        Calculate the Error for every plotted Point,
                        where:

                            Error is Large for Misclassified Points

                            Error is Small for Correctly Classified Points


                    Step 3:

                        For every Point Coordinates:

                            x1, ... , xn

                            For i = 1 ... n

                                Update wi by Adding the
                                Learning Rate α times
                                Partial Derivative of Error Function
                                wrt to wi

                                    wi'  <---   wi - α * ∂E/∂wi
                                                wi - α * ((y - ^y) * xi)

                                Update b similarly

                                    b'  <---   b - α * ∂E/∂b
                                               b - α * (y - ^y)

                            This gives us new Weights and Bias

                    Step 4:

                        Update Weights and Bias to give
                        us a New Line:

                            w1', ... , wn', b'

                            ^y' = σ(W'x + b')

                    Step 5:

                        Repeat process "E-box" qty of times
                        until Error is Small
                    ```

            * **Perceptron Algorithm vs Gradient Descent Algorithm**

                * Gradient Descent

                    * `^y` may take ANY value
                        ```
                        wi'  <---   wi - α * ((y - ^y) * xi)
                        ```

                    * If Point is Incorrectly Classified:
                        * Tells Line to come closer (on each step)
                    * If Point is Correctly Classified:
                        * CHANGE WEIGHTS
                        (i.e. Point tells Line to go farther away,
                        since if already in Correct region,
                        want Prediction to still get closer
                        and closer to 1, reducing Error farther)

                * Perceptron Algorithm

                    * `^y` may ONLY take `0` or `1`
                    for Correctly or Incorrectly classified

                    * If Point is Correctly Classified:
                        * DO NOTHING


                * Common
                    * Both Gradient Descent and Perceptron Algorithm
                        * Misclassified Point tells Line to come closer
                        (trying to get Point on the correct side)

            * **Non-Linear Data** (Neutral Networks)

                * Linear - Data sets separatable by a Line

                * Non-Linear - Complex Data Sets with highly
                               Non-Linear Boundaries
                               (not separable due to Non-Linear
                               Equation of Line)
                               **Neural Networks** used instead

                   * Create Probability Function where:
                       * Points in Blue region more likely to be Blue,
                       * Points in Red region are more likely to be Red
                       * Points on LINE are equally likely to be Blue or Red

            * **Neural Networks**
              (aka Multi-Layer Perceptrons,
               Neural Network Architecture)

                https://www.youtube.com/watch?v=Boy3zHVrWB4


                * Combine Two Perceptrons
                into Third (more complex one)

                    ```
                    Use Probability Function (Sigmoid)
                    for every Point on the Plane
                    (so resulting probab. always between 0 and 1)

                    Step 1:

                        - A = Calc probab. for Point K on Model #1
                        - B = Calc probab. for Point K on Model #2

                    Step 2: (neural network, similar to perceptrons)

                        - C = W1 * A + W2 * B - BIAS

                            where W1 is Weight1
                            where W2 is Weight2
                    Step 3:
                    (i.e. create resulting curved line
                          non-linear model b/w both linear models)

                        - Sigmoid(C)

                    ```

                    * Combine Two "Linear" Models
                    overimposed into a "Non-Linear" Model

                    * i.e.  `Linear Line  +  Linear Line
                            =  Non-Linear Line (i.e. Curve)`

                * Note:
                    * Linear Model is a Probability Space that
                    gives Probability of Point being Blue (in a Region)

                * Combining the Probability of a Point
                across Two Perceptrons (P1, P2)
                Linear Models Probability Spaces,
                i.e. P1 Point probability (of Blue): 0.7
                     P2 Point probability (of Blue): 0.8

                     * Apply the Sigmoid Function to the Sum of the
                      Point Probability across Two Perceptrons

                          `Sigmoid ( 0.7 + 0.8 )
                           = Resulting Probability of Blue`

                * Weight the Sum
                    * i.e. if want Model #1 to have more say
                    in resulting probability

                * Neural Network, different Bias Diagrams

                    * https://www.youtube.com/watch?v=au-Wxkr_skM
                        * Weights on Left describe
                        Equations of the Linear Models
                        (i.e. Cx + Dy + E = 0)
                        * Weights on Right describe
                        Linear Combination of the Two Models
                        to obtain resulting Curved
                        Non-Linear Model
                        (**Non-Linear Boundary defined by
                        Neural Network**)

                * Neural Network Multiple Layers Architecture

                    * https://www.youtube.com/watch?v=pg99FkXYK0M

                        * Layers
                            * **Input Layer**
                                * Inputs (i.e. x1, x2)

                                * Note: when x1, x2, x3 then
                                we're in 3D space, where Hidden Layers
                                are Planes (instead of Lines)
                                and Output Layer is Non-Linear
                                Region in free space

                                * Note: when N Input Layers,
                                we are in N-Dimensional space

                            * **Hidden Layer**
                                * Set of Linear Models created
                                using the first Input Layer
                                (when only 2 Nodes i.e. Red/Blue)

                                *  Note: when Hidden Layer has
                                3+ Nodes, then we have a
                                **Multi-Class Classification Model**
                                and Output Layer is produced for
                                each of the Classes
                                (i.e. Cat, Dog, Bird)

                                * Note: when **Multiple Hidden Layers**
                                we have a **Deep Neural Network**
                                where Linear Models combine to
                                create Non-Linear Models, and these
                                resulting Non-Linear Models combine
                                to create more Non-Linear Models

                                **Deep Neural Network** splits the
                                N-Dimensional space in the Output Layer
                                to have a highly Non-Linear Boundary
                                 (i.e. wiggly line)

                            * **Output Layer**
                                * Creates a Non-Linear Model
                                from Combining N Linear Models
                                from Hidden Layer
                                (i.e. combine 3x Linear Models
                                to create Triangular Boundary
                                in Output Layer)

                * **Multi-Class Classification vs Binary Classification
                (with Deep Neural Networks)**

                    https://www.youtube.com/watch?v=uNTtvxwfox0

                    * If have multiple classes and want Model to
                    predict if we have a Duck, Beaver, or Walrus

                    * APPROACH #1
                        * Create 3x Neural Networks,
                        one for each Class

                        ```
                        Softmax( NN (Duck)  +  NN (Beaver)  +  NN (Walrus) )
                        ```

                    * **APPROACH #2 (BETTER)**
                        * Create 1x Neural Network,
                        with more Nodes in the Output Layer
                        (where each Output Layer Node gives the
                        probab. that the image is each of the animals)

                        * Take the Scores and apply the
                        **Softmax** Function to obtain Well-defined probabilities

                * **Feedforward** (Process of Neural Network)

                    https://www.youtube.com/watch?v=Ioe3bwMgjAM

                    * **Feedforward Process** used by
                    Neural Networks to turn
                    Input into Output:

                        * Neural Network (i.e. Perceptron is simplest NN)

                            ```
                            Inputs

                                Input Data Point

                                    x = (x1, x2)

                                Label

                                    y = 1  (means Point is Blue)

                            Linear Equation of Perceptron
                            Boundary Line in 2D space

                                w1 * x1 + w2 * x2 + b = 0

                                where w1, w2  (Weights and Edges)

                                where b       (Bias on the Node)


                            w1 and w2 as connecting lines
                            between Inputs and Linear Models
                            with greater Thickness of
                            connecting line
                            for the higher
                            Weight values
                            (see 0:56 of video)

                            Perceptron then Plots the
                            Points x1 and x2 and outputs
                            the Probability that the point is Blue
                            (i.e. if Blue Point is in Red Area then
                             Output Probability is Small number,
                             since Point is Not likely to be Blue)


                            ```
                        * Use Matrix Multiplication
                        of Weights for Non-Linear mapping of Complex
                        Neural Networks including
                        Multi-Layer (see 2:42, 3:50)
                        https://www.youtube.com/watch?v=Ioe3bwMgjAM

                            ```
                            i.e. output prediction from input Vector

                            ^y = σ  o  W(3)  o  σ  o  W(2)  o  W(1)(x)

                                where o is Composition of
                                  Weights Matrix with Sigmoid Function
                            ```

                            * Error Function

                                https://www.youtube.com/watch?v=VAX9Di9cjzE



                * **Training Neural Networks**

                    * **Backpropogation** Method

                        * Dfn / Steps:
                            * Feedforward operation https://www.youtube.com/watch?v=1SmY3TZTyUk
                            * Compare Model Output with
                              Desired Model Output
                                * i.e. Bad Neural Network - if predicts Point is Red when it's actually Blue
                            * Calculate Error
                            * Backpropagation
                            (run the Feedforward backwards) to spread
                            Error to each of the Weights
                                * **Backpropagation after Feedforward (3:08)
                                to Train Neural Networks**
                                    https://www.youtube.com/watch?v=1SmY3TZTyUk

                                    * Ask Point what it wants to do to be
                                    "Better" classified
                                    (i.e. if Misclassified Point will ask Boundary Line
                                    of Blue Region to come closer to the Point)

                                    * Listen to the "Better" Model in the
                                    **Hidden Layer** MORE (by INCREASING the
                                    Weight (Thickness) of connecting line between
                                    Hidden Layer and Output Layer)
                                    than we Listen to the
                                    "Bad" Model in the Hidden Layer
                                    (DECREASE Weight of its connecting line),
                                    so that Final Model looks more like the
                                    "Better" Model in the Hidden Layer

                                    * THEN, go Back to the
                                    Linear Models in the **Hidden Layer**
                                    and for each Linear Model
                                    Ask Points  what it wants to do to be
                                    "Better" classified
                                    (i.e. if Misclassified Point will ask Boundary Line
                                    of Blue Region to come closer to the Point,
                                    otherwise if Correctly Classified will ask
                                    Boundary Line to move away from Point)

                                        * This will update the Weights (thickness) of
                                        connecting line between Input Layer and Hidden Layer

                                    * Now we have "Better" Predictions of all the
                                    Models in the Hidden Layer, and for the Model
                                    in the Output Layer

                            * Update Weights with output of
                            Backpropagation for "Better" Model
                                * Point says what wants Model to do:
                                    * Move Boundary Line closer to Point
                                    (if Misclassified),
                                        * Boundary Line moves closer by
                                        updating its Weights
                                        (thereby defining a new line)
                                    * Move Boundary Line away from Point
                                    (if Correctly classified)
                            * Calculate Error Function `E(W)` and
                            step in direction of Negative of Gradient
                            `-∇E`, to new Model `W'` with New Model
                            Error ``E(W')` with "Better" Prediction
                            * Note: Error reduces from `E(W)` to `E(W')`
                            after taking Gradient Descent step down the
                            Negative of the Gradient `-∇E`, such that
                            new Boundary Line is closer to the Point
                            * Repeat until "Good" Model, successively minimising error

                    * **Backpropagation** (DETAILED MATHS covered by Keras)
                        * Multi-Layer Perceptron, Multi-Layered Perceptron, Feedforward
                        https://www.youtube.com/watch?v=tVuZDbUrzzI
                        https://www.youtube.com/watch?v=YAhIBOnbt54
                        https://www.youtube.com/watch?v=0EoRxu3EeGM

                        * Chain Rule
                        https://www.youtube.com/watch?v=0EoRxu3EeGM

                            ```
                            Given x, A = f(x), B = g o f(x)

                            Partial Derivatives given by:

                                ∂B / ∂x  =  ∂B * ∂A / ∂A * ∂x

                                (when composing functions
                                 the derivatives multiple)
                            ```

                             * Feedforwarding - composing various Functions
                             * Backpropagation - reverse of Feed Forward
                                                 calculating Derivative of
                                                 complex Composition
                                                 at each Error Function wrt
                                                 each of the Weights in the
                                                 Labels using Chain Rule

                    * Neural Networks in Keras
                        * Packages for Neural Networks, activation function, gradient descent
                            * Keras             https://keras.io/
                            * TensorFlow        https://www.tensorflow.org/
                            * Caffe             http://caffe.berkeleyvision.org/
                            * Theano            http://deeplearning.net/software/theano/
                            * Scikit-Learn      http://scikit-learn.org/stable/

                        * Keras
                            * Project - Build a Fully Connected
                            (Multi-layer) Feedforward Neural Network
                            to solve the XOR problem
                                * Steps:
                                    * Load the data
                                    * Define the network
                                    * Train the network

                            * Project - Student Admissions
                                * https://github.com/ltfschoen/aind2-dl

                    * Training Model Optimisation
                        * Failure points
                            * Poor architecture chosen
                            * Noisy data
                            * Model takes too long to run


                    * Batch vs Stochastic Gradient Descent
                        * Recap:
                            * **Gradient Descent Algorithm**:
                                * Reduce height (error function) by taking steps (aka Epochs)
                                following the negative of the gradient of the height

                                * **Epoch** https://www.youtube.com/watch?v=2p58rVgqsgo
                                    * Each steps taken to reduce error function in gradient descent algorithm
                                    * At each Epoch, we take all the input data, and run it through the
                                    entire Neural Network, then find predictions, calculate the error
                                    (i.e. how far from actual labels), then back propagate the error
                                    in order to update the weights in the Neural Network, to improve the
                                    boundary for predicting all our data

                                    * **Batch vs Stochastic Gradient Descent**
                                        * Since we perform the above steps at each Epoch for all the data points,
                                        these are huge computational steps using lots of memory.
                                        Since we don't need to plugin all our data at every Epoch,
                                        when data is well distributed, a small subset of the data would
                                        at least give a good idea what the gradient would be, and much quicker!


                                        * Split the data into several Batches
                                        (i.e. given 24 points, split into 4x batches of 6 points each)
                                        * Run Batch #1 points through Neural Network
                                        * Calculate error and gradient, and back propagate to update
                                        with better weights that define a better boundary region
                                        * Repeat for subsequent Batches

                                        * Keras
                                            ```
                                            model.fit(X_train, y_train, epochs=1000, batch_size=100, verbose=0)
                                            ```


                    * Learning Rate Decay
                        * High Learning Rate + Large Steps may result in missing the bottom and coming up again
                        * Low Learning Rate + Small Steps higher chance of arrive local minimum
                            * If Model is not working, then Lower the Learning Rate
                            * Ideally the Learning Rate would Decrease as the Model
                            gets closer to the solution
                        * Note:
                            * If "steep", take long steps
                            * If "plain", take small steps

                    * **Training and Testing sets** to Find "Better" Model
                        * Given blue and red points from data set that are plotted on each
                        two classification models with a boundary
                        that separate the blue points from the red points
                        Instead of choosing the "better" model based on a combination of
                        observations (i.e. boundary curve complexity vs less mistakes
                        in classifying points) we using Training and Testing sets.

                        * STEP 1
                            * Differentiate Training and Testing sets
                                * **Training Set** are Solid Coloured Points
                                * **Testing Set** are Hollow Coloured Points (white inside)
                        * STEP 2
                            * Train each model with the Training Set (without looking at
                            the Testing Set) to obtain updated boundary
                        * STEP 3
                            * Evaluate results by reintroducing the Testing Set
                            to see how we did
                            * Check how many mistakes each model made with the
                            Testing Set now reintroduced

                        * Note: Follow our intuition, when comparing which
                        is better between a simpler model and a complex model,
                        always go for the simpler model

                    * **Overfitting vs Underfitting** (Types of Errors)

                        https://youtu.be/SVqEgaT1lXU?t=4m15s

                        * **Overfitting** (Too Specific) (aka Error due to Variance)
                            * Dfn: Overcomplicating the problem and using a solution that
                            is too excessive (i.e. kill fly with bazooka), may
                            lead to bad solutions and extra complexity
                            (when simpler solution possible)

                            * Example 1
                                * Given some data to be classified (i.e. different objects and animals)
                                * If we groups we choose are too granular
                                (i.e. Dogs that are orange or gray vs
                                Anything that is orange or gray except dogs) it is Too Complex

                                * Identify Issues:
                                    * Introduce a **Testing Set** to see if introduced data
                                    is classified correctly or not
                                    * It may fit the data well, but may not generalise correctly

                            * Example 2
                                * i.e. memorising textbook instead of studying word by
                                word, so may be able to regurgitate, but not be able
                                to generalise properly to questions

                        * Good Model Fit
                            * Dfn: Generalises better than overfitting
                            **Always have preferance for Overly Complex models
                            and apply certain Techniques to prevent Overfitting**
                            (like using a belt to fit into a pants that are too big)
                            Finding the right Architecture for a Neutral Network

                            * Example 2
                                * i.e. like study well and good result in exam

                        * **Underfitting** (Too Simple) (aka Error due to High Bias)

                            * Dfn: Oversimplifying the problem and using a solution that is too
                            simple to do the job (i.e. trying to kill Giant with fly swatter)

                            * Example 1
                                * Given some data to be classified (i.e. different objects and animals)
                                * If we groups we choose are too abstract
                                (i.e. Animals vs No Animals) it is Too Simple

                                * Identify Issues:
                                    * Misclassify objects

                            * Example 2
                                * i.e. not studying enough and failing

                    * **Early Stopping Algorithm**
                        * Given an Overly complex Neutral Network architecture than
                        what we need (according to Model Complexity Graph)

                        * **After Training Evaluate each Model by introducing a Testing Set**
                            * Introduce a set of Training Set points to each model plot
                            * Plot the:
                                * Error vs Epoch for the Training Set
                                * Error vs Epoch for the Testing Set

                        * **Training and Testing results**
                            * Epoch 1
                                * Training
                                    * Use random weights
                                    * Underfitting so makes many mistakes
                                * Testing
                                    * Badly misclassifies both Training and Testing Sets
                                    so both:
                                        * Large Training Error
                                        * Large Testing Error
                            * Epochs 20
                                * Training
                                    * Good model
                                * Testing
                                    * Small Training Error
                                    * Small Testing Error
                            * Epochs 100
                                * Training
                                    * Fits data better
                                    * Overfit data
                                    * Training error decreases with higher Epochs
                                     but Testing errors increases (due to misclassification
                                * Testing
                                    * Tiny Training Error
                                    * Medium Testing Error
                            * Epochs 600
                                * Training
                                    * Fits the "training data" well by Generalises badly
                                    * Heavily Overfits
                                    * **Problem**
                                        If introduce say a blue point in blue region it may be
                                        misclassified as red unless its very close to existing blue point
                                * Testing
                                    * Tiny Training Error
                                    * Large Testing Error

                        * **Goldilocks Spot and Model Complexity Graph ( Error vs Model Complexity(qty epochs) )**
                            * https://youtu.be/NnS0FJyVcDQ?t=2m35s

                            * Use **Model Complexity Graph** to identify ideal Model Complexity
                            of ideal quantity of Epochs to use

                            * Approach:
                                * Use Gradient Descent until the Testing Error stops decreasing and instead
                                starts increasing and then we Stop (**Early Stopping Algorithm**) technique
                                when Training the Neutral Network

                    * **Regularisation**

                        https://youtu.be/aX_m9iyK3Ac?t=2m50s

                        ```
                        Given two points P1 (-1,-1) and P2 (1,1), which of equations gives
                        smaller error?

                            Solution 1:  x1 + x2 = 0

                            Solution 2:  10x1 + 10x2 = 0    (scalar multiple of Solution 1)

                        Both give same line.

                        Prediction is sigmoid of linear function

                            Using Solution 1 and substituting Point Coordinates for Coefficients:

                                substitute P1;

                                ^y = σ(w1 * x1 + w2 * x2 + b)
                                   = σ(1 + 1)
                                   = 0.88

                               substitute P2:

                               ^y = σ(w1 * x1 + w2 * x2 + b)
                               = σ(-1 - 1)
                               = 0.12

                            Using Solution 2 and substituting Point Coordinates for Coefficients:

                                substitute P1;

                                ^y = σ(w1 * x1 + w2 * x2 + b)
                                   = σ(10 + 10)
                                   = 0.9999999979   (great prediction, less error,
                                                     but only subtle Overfitting)

                               substitute P2:

                               ^y = σ(w1 * x1 + w2 * x2 + b)
                               = σ(-10 - 10)
                               = 0.0000000021   (great prediction, less error,
                                                 but only subtle Overfitting)

                                                Error Function of Initial "Prediction" is:

                                                    E = W(x + b)

                            Solution 2 is "Too Certain" and it will be too
                            hard to tune the model to correct any misclassified errors if any
                        ```

                        * Gradient Descent is best used on Models that are NOT "Too Certain"
                        (i.e. lower scalar weights applied to x1, x2, etc, so the function
                        is not too steep, and so the derivatives aren't too close to 0 and
                        very large at middle of curve)

                        ```
                        Since we now know that


                            LARGE COEFFICIENTS     ---->    OVERFITTING


                        We need to tweak the Error Function to Punish any High Coefficients
                        (penalise Large Weights w1, ..., wn)
                        by taking the Old Error Function
                        and add a term that is Big when the Weights are big
                        by either:

                            * Note: where Lambda constant is how much to Punish the Coefficients

                            * "L1 Regularisation" - Adding sums of absolute values of the Weights * Lambda constant

                                Error Function  = Sum of all the Error Functions of all the Points
                                                = - (1/m) * (   (m ∑ j=1) (1 - yj) * ln(1 - ^yj) + yj * ln(^yj)
                                                                + λ (|w1| + ... + |wn|)
                                                            )
                                * Usage:

                                    * Usually results in sparse vectors
                                    where small weights tend toward 0,
                                    so use L1 to reduce weights and
                                    end up with small set.
                                        i.e. Sparsity: (1,0,0,1,0)

                                    * Also good for Feature Selection by
                                    only selecting the most important points and
                                    turns the rest into 0's

                            * "L2 Regularisation" - Add sum of squares of the Weights * Lambda constant

                                Error Function  = Sum of all the Error Functions of all the Points
                                                = - (1/m) * (   (m ∑ j=1) (1 - yj) * ln(1 - ^yj) + yj * ln(^yj)
                                                                + λ (w1^2 + ... + wn^2)
                                                            )
                                * Usage:

                                    * Does not favour sparse vectors since
                                    it tries to maintain all weights homogenously small
                                    i.e. Sparsity: (0.5,0.3,-0.2,0.4,0.1)

                                    * Better for Training Models

                            * Example
                                Given vector (1, 0) then:
                                    λ (|w1| + ... + |wn|)  =  λ (1)
                                    λ (w1^2 + ... + wn^2)  =  λ (1)

                                Given vector (0.5, 0.5) then:
                                    λ (|w1| + ... + |wn|)  =  λ (1)
                                    λ (w1^2 + ... + wn^2)  =  λ (0.25 + 0.25)  = 0.5

                                    So therefore "L2 Regularisation" prefers the
                                    vector (0.5, 0.5) over the vector (1, 0)
                                    since (0.5, 0.5) produces smaller sum of squares
                                    and in turn a "Smaller Error Function"

                        ```

                        * BertrAIND Russell Quote

                            "The whole problem with AI is that bad models are so
                            certain of themselves, and good models so full of doubts"

                    * **Dropout Method** (used when Training Neural Networks)

                        * Dfn: Sometimes one part of the Neutral Network has Large Weights
                        and dominates the Training whilst others doesn't train as much,
                        so as mitigation we need to turn the dominator off sometimes

                        * Approach:
                            * Randomly turn off some Nodes in Neutral Network as we
                            go through Epochs (i.e. feed forward and backpropagation passes)
                            to force the rest of the Nodes to pick up the
                            slack and get involved in the training (so none dropout)

                            * Parameter is passed to the Algorithm in order to
                            drop the Nodes. The **Parameter is the Probability that the
                            Node gets dropped at a particular Epoch**
                            i.e. if P = 0.2 it means for each Epoch, each Node gets
                            turned off with probability of 20%

                    * **Vanishing Gradient Issue**

                        * **Problem**

                            https://www.youtube.com/watch?v=W_JJm_5syFw

                            * Calculating the Derivative `σ(x) = 1 / (1 + e^-x)`
                            of the Sigmoid function at a point far
                            to the left or far to the right results in 0.
                            **But the Derivative is what tells us the Direction to Move**.
                            This issue is worse in Multi-Linear Perceptron
                            since the Derivative of an Error Function with respect
                            to a Weight equals the Product of all the Derivatives
                            calculated at the Nodes in the corresponding path to the output,
                            where all those Derivatives are Derivatives as a Sigmoid Function
                            (so they are small), and so their Product is tiny,
                            which makes Training difficult, since Gradient Descent gives
                            us very tiny changes to make on the Weights, causing us to
                            make very small Steps taking forever to reduce the Error

                        * **Solution 1: Change the Activation Function

                            * **Tanh - Hyperbolic Tangent Function**

                                * https://www.youtube.com/watch?v=VzGOR5SlFSw

                                * `tanh(x) = (e^x - e^-x) / (e^x + e^-x)`
                                * Similar to Sigmoid but range is -1 to 1
                                so Derivatives are Larger, which leads to
                                great advances in Neural Networks

                            * **ReLU Rectified Linear Unit**

                                *
                                  ```
                                  relu(x) = {  x if x >= 0
                                               0 if x < 0
                                  ```

                                * Simple function between -1 and 1:
                                    * if Positive return same value
                                      Derivative is 1 if Positive
                                    * if Negative return zero

                                * Used instead of Sigmoid since improves
                                Training significantly without sacrificing
                                much accuracy since the
                                **Derivatives will not be as small
                                and allows us to perform Gradient Descent**

                                * Note that **Final Activation Function
                                in Multi-Layer Perceptron must be between 0 and 1
                                (i.e. a Sigmoid)**

                                * If the Final Activation Function is a
                                ReLU then we may end up with
                                Regression Models that Predict a value
                                (used in **Recurrent Neural Networks**)

            * **Gradient Descent**

                * **Problem**

                    * Getting stuck in **Local Minima** during Gradient Descent

                * **Solutions**

                    * **Random Restart** (used by Gradient Descent Algorithms)
                        * Prevent
                        * Start from a few Random places and use Gradient Descent from
                        all of them to increase the Probability we'll reach a good
                        Global Minimum

                    * **Momentum** (used by Gradient Descent Algorithms)

                        https://www.youtube.com/watch?v=r-rYz_PEWC8

                        * Move with Momentum so when reach a Local Minimum
                        we power through the Local Minimum and over the next hump.
                        Otherwise when we reach a Local Minima
                        where our Gradient is too small to Step out of the Local
                        Minima and over the next hump.

                        * Make the next Step in the Local Minima be the
                        **Average of the Previous Steps** and where the
                        most recent previous step matters more (and
                        is given a higher Weight) than older steps.

                        https://youtu.be/r-rYz_PEWC8?t=1m36s

                        * Beta β (**Momentum**) is a constant between 0 and 1
                        that attaches to the Steps

                        ```
                        STEP(n) --> STEP(n) +
                                    β * STEP * (n-1) +
                                    β^2 * STEP * (n-2) + ...
                        ```

            * **Optimisers in Keras** (used as Arguments when compiling Keras Models
            to optimise their performance)

                * Links
                    * https://keras.io/optimizers/
                    * http://sebastianruder.com/optimizing-gradient-descent/index.html#rmsprop

                * **SGD**
                    This is Stochastic Gradient Descent. It uses the following parameters:

                    * Learning rate
                    * Momentum (takes weighted average of the previous steps,
                    in order to get a bit of momentum and go over bumps,
                    as a way to not get stuck in local minima).
                    * Nesterov Momentum (This slows down the gradient when
                    it's closed to the solution).

                * **Adam**
                    * Adam (Adaptive Moment Estimation) uses a more complicated
                    exponential decay that consists of not just considering the
                    average (first moment), but also the variance (second moment) of the previous steps.

                * **RMSProp**
                    * RMSProp (RMS stands for Root Mean Squared Error) decreases
                    the learning rate by dividing it by an exponentially decaying average of squared gradients.

            * **Keras Lab**

                * Goal: Build Neural Network to analyse real data that consists of
                thousands of movie reviews from IMDB, and the challenge is to
                Predict the Sentiment Analysis of a review


## Chapter 2 - Convolutional Neural Networks (CNN)<a id="chapter-2"></a>

* About
    * State of the art
    * **Great Summary** https://ujjwalkarn.me/2016/08/11/intuitive-explanation-convnets/
        * CNN 2D visual http://scs.ryerson.ca/~aharley/vis/conv/flat.html
        * CNN 3D visual http://scs.ryerson.ca/~aharley/vis/conv/
        * JS source code http://scs.ryerson.ca/~aharley/vis/

* COURSES
    * http://cs231n.stanford.edu/

* Applications
    * Voice User Interfaces
        * Applications
            * WaveNet model (by Google)
                * Convert text to speech
                * Trained sufficiently it sounds like you
                * Link About https://deepmind.com/blog/wavenet-generative-model-raw-audio/
                * Link Paper https://arxiv.org/pdf/1609.03499.pdf
                * Link Singing http://www.creativeai.net/posts/W2C3baXvf2yJSLbY6/a-neural-parametric-singing-synthesizer

    * Natural Language Processing (NLP)
        * General
            * Recurrent Neural Networks (RNNs) used more than CNNs
        * Applications
            * CNNs used to extract info from sentences for:
                * Sentiment Analysis
            * CNN for NLP with Text Classification using TensorFlow,
            baseline for Sentiment Analysis tasks, etc + Code
                * http://www.wildml.com/2015/12/implementing-a-cnn-for-text-classification-in-tensorflow/
                * http://www.wildml.com/2015/11/understanding-convolutional-neural-networks-for-nlp/
            * CNNs at Facebook for language translation 9x faster than RNN
                * https://code.facebook.com/posts/1978007565818999/a-novel-approach-to-neural-machine-translation/
                * (hierarchical/parallel process words of sentence)
            * CNNs to play Atari Games with DeepMind
                * About https://deepmind.com/research/dqn/
                * Code https://sites.google.com/a/deepmind.com/dqn/
                * **Deep Reinforcement Learning** using
                 **Policy Gradients** code beginner
                    http://karpathy.github.io/2016/05/31/rl/
    * Computer Vision
        * General
            * Given set of Images, CNN assigns corresponding Label
            it believes Summarises the Image Content
        * Applications
            * CNNs used to Teach AI Agents to play Video Games like
            paddle game
                * CNN has no prior knowledge of what a
                ball is or knowing precisely what the controls do.
                * Only provided screen, score, and controls a user given
                * CNN extracts crucial info allowing them to develop a
                useful strategy
            * ClickDraw
                * Pictionary playing, guesses what you're drawing
                based on what you draw
                    * https://quickdraw.withgoogle.com/#
                    * Auto-suggestions for sketching https://www.autodraw.com/
            * DeepMind
                * Go boardgame (Ancient Chinese complex)
                AI agent beat human professional
            * Drones flying unfamiliar territory
                * Deliver medical supplies to remote areas
                * CNNs give drone ability to see or determine what's
                happening in streaming video data
            * Decoding Images of Text
                * Digitise historical books
                * Digitise hand-written notes
                * Improve Algorithm to handle letters, numbers, punctuation
                * Decode road signs for Self-Driving Cars
            * Google Street Maps
                * Trained algorithm to create better street maps of
                the world that reads house number signs from street view
                images
        * Misc
            * AI Experiments https://aiexperiments.withgoogle.com/
            * AlphaGo https://deepmind.com/research/alphago/
                * https://www.technologyreview.com/s/604273/finding-solace-in-defeat-by-artificial-intelligence/?set=604287
            * CNNs powering Drones Indoors
                * https://www.youtube.com/watch?v=AMDiR61f86Y
            * Outdoor navigation with GPS
                * http://www.droneomega.com/gps-drone-navigation-works/
            * CNN powered Drone Outdoors Autonomous
                * https://www.youtube.com/watch?v=wSFYOw4VIYY
            * Classify Traffic sign recognition system
                * https://github.com/udacity/CarND-Traffic-Sign-Classifier-Project
            * Classify Street Signs
                * https://github.com/udacity/machine-learning/tree/master/projects/digit_recognition
            * CNN to produce a self-driving AI to play Grand Theft Auto V
                * https://pythonprogramming.net/game-frames-open-cv-python-plays-gta-v/
            * CNNs to convert famous paintings into 3D for Vision Impaired using
            CNN to predict Depth from a Single Image https://www.cs.nyu.edu/~deigen/depth/
                * http://www.businessinsider.com/3d-printed-works-of-art-for-the-blind-2016-1/?r=AU&IR=T
            * CNN to localise Breast Cancer
                * https://research.googleblog.com/2017/03/assisting-pathologists-in-detecting.html
            * CNN to save endangered species using HotSpotter http://cs.rpi.edu/hotspotter/
                * https://blogs.nvidia.com/blog/2016/11/04/saving-endangered-species/?adbsc=social_20170303_70517416
            * CNN to change gender or make you smile in photo
                * https://www.digitaltrends.com/photography/faceapp-neural-net-image-editing/


* Deep Learning Recognition

    * Interpret hand-written numerical digits
        * Design Image Classification Algorithm
            * Goal
                * Takes pictures of hand-written numbers and
                identifies the numbers shown in the images
            * Tools
                * MNIST Database - contains 70k greyscale hand-written
                images of digits 0 to 9
                    * Figure that shows datasets referenced
                    over time in NIPS papers
                        * https://www.kaggle.com/benhamner/popular-datasets-over-time/code
                        * https://nips.cc/

            * **My Code and Detailed Documentation explaining MLP Steps**
                * https://github.com/ltfschoen/aind2-cnn

            * Links
                * TODO - Drop Out Layer technique for Overfitting https://www.cs.toronto.edu/~hinton/absps/JMLRdropout.pdf
                * TODO - Keras Flatten Layer https://keras.io/layers/core/#flatten
                * TODO - Activation Functions http://cs231n.github.io/neural-networks-1/#actfun
                * TODO - Fully Connected "Dense" Layer https://keras.io/layers/core/
                    * TODO - Initialisers https://keras.io/initializers/
                * TODO - Loss/Error Functions https://keras.io/losses/
                * TODO - Early Stopping and ModelCheckpoint https://keras.io/callbacks/#modelcheckpoint
                * TODO - http://machinelearningmastery.com/check-point-deep-learning-models-keras/
                * TODO - Performance of other classifiers http://yann.lecun.com/exdb/mnist/
                * Black Box of AI Unknowns https://www.technologyreview.com/s/604087/the-dark-secret-at-the-heart-of-ai/

    * Intepret sophisticated images with complex patterns
    by replacing **Fully Connected Layer (aka Dense Layers)** with
     **Locally Connected Layer (aka Convolutional Layers)**

        * https://www.youtube.com/watch?v=z9wiDg0w-Dc
        * https://www.youtube.com/watch?v=h5R_JvdUrUI
        * Colour images https://www.youtube.com/watch?v=RnM1D-XI--8

        * Issues:

            * MLPs use many Parameters
                * Only using **Densely Connected Layer
                (aka Fully Connected Layers)**
                i.e. 28x28px image required 0.5M parameters
                so moderately sized images require high computational
                complexity

                * Only accepting **Vectors** as Input as throwing
                away the 2D info (i.e. spatial info of where
                pixels are located in reference to each other)
                contained in image when we
                Flatten its Matrix into a Vector

        * Solution

            * Use **CNNs** instead of MLPs as they process images
            without losing 2D info since they use
            **"Locally Connected Layer" (aka Sparsely/Convolutional Connected
            Layers)** where Connections
            between Layers are informed  by the 2D structure
            of the image Matrix, and they accept 2D Matrices as
            Input.

            Instead of connecting ever Hidden Node to every
            pixel in the original image, break the original image
            into 4x Regions, such that each Hidden Node is a
            **"Locally/Convolutional Connected Layer"**
            that connects each Hidden Node to only
            one of the pixels in each of the 4x Regions
            (i.e. sees only 1/4 of original image) to find patterns
            Each Hidden Node still reports to the Output Layer,
            which combines the findings for the discovered patterns
            that were learnt separately in each Region.
            It uses far fewer Parameters than Densely Connected Layer,
            is less prone to Overfitting and understands how to tease
            out patterns in image data.
            Expanding quantity of Nodes with multiple Collections
            in the Hidden Layer, where each Collection contains Nodes that are
            responsible for analysing different Regions of the original
            image and allows
            discovering more Complex Patterns in data.
            Each of the Hidden Nodes within a Collection would share
            common group of Weights (parameters), which is the motivation
            behind **Convolutional Layers** (i.e. for images where
            pattern may appear in any region of the image)

    * **Convolutional Layers**

        * Steps
            * Given an image 5x5
            * Select a Width (W) and Height (H) that defines
            a Convolution Window (i.e. 3x3 Window)
            * Slide the image horizontally and vertically over
            Regions of the image pixels. At each position the Window specifies
            a small piece within the image and defines a Collection
            of pixels to which we connect a single Hidden Node,
            and we call this Hidden Layer a
            **Convolutional Layer**

            * Each Regional Collection of Input Nodes influences the
            value of a node in the Convolutional Layer by
            multiplying the Input Nodes by their corresponding Weights and
            summing the result. Assume the Bias is 0
            * Always add a **ReLU Activation Function** to the
            Convolutional Layers (leaves Positive values alone and makes all
            Negative values 0). See https://www.youtube.com/watch?v=h5R_JvdUrUI

            * **Filters** (i.e. 3x3) representing the Weights in a grid, whose
            size matches the size of the **Convolutional Window**

                http://deeplearning.stanford.edu/wiki/index.php/Feature_extraction_using_convolution

                ```

                ReLU ( SUM ( Region Input Nodes * Weights Grid ) ) = 0

                    where Weights Grid == Filter

                ```

                * Note: We try to visualise the Filter so we understand
                what kind of Pattern the Filter is trying to detect.
                Each Filter detects only a single Pattern.

                * Detection of Multiple Patterns requires the use of
                **Multiple Filters**
                    * i.e. if want part of a dog image that contains Teeth,
                    Whiskers, then we'd have Two Filters to detect Patterns of
                    each https://www.youtube.com/watch?v=RnM1D-XI--8

                * **Weights** are not known in advance. Instead the Weights are
                learnt by the Neural Network as being the Weights that
                minimise the Loss Function

                * **Filter Image Kernels for Feature Extraction**. It allows
                 allows you to create your own filter. You can then use your webcam
                 as input to a convolutional layer and visualize the corresponding activation map
                    * http://setosa.io/ev/image-kernels/

    * **Greyscale vs Colour Images**
        * Greyscale images interpreted as 2D array (Width, Height)

        * Colour images interpreted as 3D array (Width, Height, Depth)
            * RGB images have Depth of 3
            (i.e. 3x steps of 2D Matrices, one for each of the Red, Green,
            and Blue Channels of the image)

        * **Convolutions** over Colour image (3D) are performed using the
        3D Filter (stack of 3x 2D matrices),
        that has a value for each Colour Channel for all pixels
        in the image array.

    * **Feature Maps**
        http://iamaaditya.github.io/2016/03/one-by-one-convolution/

        * Given a Coloured image with 3x Filters.
        * Each **Feature Map** in Convolutional Layer produced by performing
        `Region Input Nodes Matrix * Weights Matrix (aka Filter)`
        may be thought of as Image Channels that may be stacked into a
        3D array. This Stack of Image Channels may be provided as Input to
        another Second Convolutional Layer to discover Patterns **within** the
        Patterns that we discovered in the First Convolutional Layer,
        and so forth.

    * **Weights and Biases in CNNs**
        * Both Dense Layers in MLPs and Convolutional Layers in CNNs
        have Weights and Biases
        that are initially randomly generated
        * In CNNs where the Weights take the form of Convolutional Filters
        these filters are randomly generated, and the Patterns they are initially
        designed to detect are also initially randomly generated

    * **Loss Functions and Training CNNs**

        * CNN always have a Loss Function
            * Multi-Class Categorisation uses `categorical_cross_entropy_loss`

        * Train the Models through Backpropagation and update the Filters (aka
        Weights matrices) with values that minimise the Loss Function
        at each Epoch (iteration)

        * The CNN determines the kinds of Patterns it needs to detect based on
        the Loss Function, we do not tell the CNN the values of the Filters or
        the kinds of Patterns to detect, the CNN determines this from the dataset.

        * Visualising the Patterns we will see that if the dataset contains
        say Dog images then the CNN is able to on its own learn Filters
        that appear like Dogs.

    * **Stride and Padding Hyperparameters**

        * https://www.youtube.com/watch?v=Qt5SQNcQfgo

        * Stride
            * amount the Filter slides over the images.
        * Padding
            * padding around the image with 0's to plan ahead for the possibility
            that the Filter may extend part-way over the side of an image if
            there is an odd difference between dimensions of image and dimensions
            of the Filter. Padding gives the Filter more space to move so
            we get contributions from all Regions in the image when
            populating the Convolutional Layer
            * `padding = 'valid'` means we are ok with potentially losing some
            Nodes in the Convolutional Layer and do not want padding
            * `padding = 'same'` means we want padding as do not want to lose
            any Nodes in the Convolutional Layer

    * **Convolutional Layers in Keras**

        * Docs https://keras.io/layers/convolutional/

        * Create a Convolutional Layer in Keras
            ```
            from keras.layers import Conv2D

            Conv2D(filters, kernel_size, strides, padding, activation='relu', input_shape)
            ```
            * Arguments
                * filters - number of filters.
                * kernel_size - Number specifying both height and width of (square) convolution window.
            * Optional arguments
                * strides - stride of convolution. default strides is set to 1.
                * padding - 'valid' or 'same'. default is 'valid' (i.e. no padding)
                * activation - typically 'relu'. default is no activation applied.
                Strongly encouraged to add a ReLU activation function to every Convolutional Layer in your networks.

            * NOTE:
                * It is possible to represent both kernel_size and strides as either a number or a tuple.
                * When using Convolutional Layer as first layer (appearing after the input layer) in a model,
                you must provide an additional `input_shape` argument:
                    * `input_shape` - Tuple specifying the Height, Width, and Depth (in that order) of the input.
                        * NOTE: Do not include the `input_shape` argument if the Convolutional Layer is not the
                        first layer in your network.

            * Example #1
                * constructing a CNN, input layer accepts grayscale images that are 200 by 200 pixels
                (corresponding to a 3D array with height 200, width 200, and depth 1).
                I want next layer to be a convolutional layer with 16 filters, each with a width and height of 2
                (kernel size).
                When performing the convolution, I'd like the filter to jump two pixels at a time (strides).
                I also don't want the filter to extend outside of the image boundaries; in other words,
                I don't want to pad the image with zeros.
                To construct this convolutional layer, I would use the following line of code:
                    ```
                    Conv2D(filters=16, kernel_size=2, strides=2, activation='relu', input_shape=(200, 200, 1))
                    ```
            * Example #2
                * want next layer in my CNN to be a convolutional layer that takes the layer constructed
                in Example 1 as input. Say I'd like my new layer to have 32 filters,
                each with a Height and Width of 3. When performing the convolution,
                I'd like the filter to jump 1 pixel at a time.
                I want the convolutional layer to see all regions of the previous layer,
                and so I don't mind if the filter hangs over the edge of the previous layer when it's
                performing the convolution. Then, to construct this convolutional layer,
                I would use the following line of code:

                    ```
                    Conv2D(filters=32, kernel_size=3, padding='same', activation='relu')
                    ```
            * Example #3
                * If you look up code online, it is also common to see convolutional layers in Keras in this format:

                    ```
                    Conv2D(64, (2,2), activation='relu')
                    ```
                * In this case, there are 64 filters, each with a size of 2x2,
                and the layer has a ReLU activation function.
                The other arguments in the layer use the default values,
                so the convolution uses a stride of 1, and the padding has been set to 'valid'.

        * Dimensionality: see conv-dims.py of https://github.com/ltfschoen/aind2-cnn
            * Formula for Shape of a Convolutional Layer
            * Formula for Number of Parameters in Convolutional Layer

        * Pooling Layers
            * https://www.youtube.com/watch?v=OkkIZNs7Cyc

            * Convolutional Networks Summary - http://cs231n.github.io/convolutional-networks/

            * docs https://keras.io/layers/pooling/

            * Dfn:
                * Pooling Layers take Convolutional Layers as Inputs
                * Convolutional Layers are a stack of Feature Maps (one for each Filter)
                * Complex datasets with many object Categories require many Filters
                each responsible for finding a Pattern in the image
                * Dimensionality of Convolutional Layers may get quite large since more Filters
                means a larger stack.
                * Higher Dimensionality means use of more Parameters
                which may lead to Overfitting, so we need a method to reduce the Dimensionality,
                which is the role of **Pooling Layers** within a Convolutional Neural Network

            * Types of Pooling Layers
                * **Max Pooling Layers**
                    * take from the Convolutional Layer a stack of Feature Maps as Input
                    * Define a
                        * Window Size: i.e. 2x2
                        * Stride: i.e. 2
                    * Construct the Max Pooling Layer by working with each Feature Map separately
                    * Choose the Maximum value within the Window for each Stride.
                    * The outcome will be that each Feature Map will be reduced in Width and Height
                    (lower Dimensionality)

                    * Example
                        * Say I'm constructing a CNN, and I'd like to reduce the dimensionality
                        of a convolutional layer by following it with a max pooling layer.
                        Say the convolutional layer has size (100, 100, 15), and I'd like the
                        max pooling layer to have size (50, 50, 15).
                        I can do this by using a 2x2 window in my max pooling layer,
                        with a stride of 2, which could be constructed in the following line of code:

                            ```
                            MaxPooling2D(pool_size=2, strides=2)
                            ```
                        If you'd instead like to use a stride of 1,
                        but still keep the size of the window at 2x2, then you'd use:
                            ```
                            MaxPooling2D(pool_size=2, strides=1)
                            ```

                * **Global Average Pooling Layer**
                    * Extreme type of Dimensionality Reduction
                    * takes stack of Feature Maps and computes the average value of the Nodes for
                    each Feature Map in the stack (reduces each Feature Map to a single value) such that
                    the **Global Average Pooling Layer converts a 3D array into a Vector**
                    * DO NOT specify Window size or Stride


        * Designing Boilerplate CNN Architecture by arranging layers (for image classification)
            * MY CODE AND NOTES
                * https://github.com/ltfschoen/aind2-cnn/tree/master/cifar10-classification

            * Steps
                * https://www.youtube.com/watch?v=kI_RQoYsgQw
                * Input
                    * CNN must accept image array input
                    * CNNs require a fixed-size input of the provided real-world images
                        * Select image size
                        * Resize all images to that same size (i.e. a square 32x32px) with
                        dimensions divisible by 2
                    * All images are interpreted by computer as 3D array. Width and Height
                    always higher than Depth
                        * Coloured RGB image shape = (10, 10, 3)
                            * where Depth caters for RGB Channels
                        * Greyscale image shape = (10, 10, 1)
                * Architecture (to allow the model to train "better" and classify objects more accurately)
                    * Sequence of Convolutional Layers
                        * Purpose of Sequence of Convolutional Layers is to discover hierarchies of
                        spatial Patterns in the image
                        * Specify various Hyperparameters as Input to each Convolutional Layer
                            * **Config so Convolutional Layer has same Width and Height as Previous Layer**
                                * `kernel_size` - between 2 and 5 (i.e. 2x2 to 5x5)
                                    * controls Depth of Convolutional Layers since the Convolutional Layer has one
                                    Activation Map for each Filter
                                * `stride` - 1 (default)
                                * `padding` - 'same' (gives better results)
                                * `activation` - 'relu' (for all Convolutional Layers)
                            * Increase number of Filters with each higher Convolutional Layer index in the Sequence
                                * i.e. 1st Convolutional Layer - 16 filters
                                * i.e. 2nd " "                 - 32 "
                                * i.e. 3rd " "                 - 64 "
                            * First Convolutional Layer requires the following additional parameter to be provided:
                                * `input_shape` (i.e. (32, 32, 3) means 32x32 pixel colour images in dataset)
                            * Note:
                                * This configuration gradually increases the Depth of the provided
                                Input array (32x32x3) without modifying the Width and Height at each Convolutional Layer
                                i.e. when run the function we'll see the Depth increasing

                                * WITHOUT MAX POOLING LAYERS
                                    ```
                                    Input    - (None, 32, 32, 3)
                                    conv2d_1 - (None, 32, 32, 16)
                                    conv2d_2 - (None, 32, 32, 32)
                                    conv2d_3 - (None, 32, 32, 64)

                                    ```
                                * WITH MAX POOLING LAYERS (also decreases Width and Height)
                                    ```
                                    Input           - (None, 32, 32, 3)
                                    conv2d_1        - (None, 32, 32, 16)
                                    max_pooling2d_1 - (None, 16, 16, 16)
                                    conv2d_2        - (None, 16, 16, 32)
                                    max_pooling2d_2 - (None, 8, 8, 32)
                                    conv2d_3        - (None, 8, 8, 64)
                                    max_pooling2d_3 - (None, 4, 4, 64)
                                    ```
                        * Accepts Input array and gradually making converting its shape
                         until its Depth is much taller than its Width and Height
                            * Eventually it transforms into Vector representation where there is
                            not more spatial info to discover (i.e. whiskers, teeth, etc)
                            in the image and then feed
                            the Vector into Fully Connected Layer(s) (i.e. Dense) to determine
                            what object is contained in the image.
                            i.e. if the Last Max Pooling Layer discovers spatial information that
                            wheels are present in that part of the image, then the
                            Fully Connected Layer transforms that information to predict that a
                            car is likely present in the image with higher probability
                            (i.e. prob(car) = 0.99, prob(dog) = 0.01).
                            This information is NOT pre-specified by us, instead it is learnt
                            by the model during training through backpropagation
                        * Convolutional Layers increase the Depth of the Input array as it passes
                        through the network
                        * Max Pooling Layers decrease the Width and Height of the Input array
                        (spatial dimensions)
                            * Follow every or every second Convolutional Layer in the Sequence
                                * **Config so Spatial Dimensions become Half what they were in the
                                Previous Layer**
                                    * `pool_size` - 2
                                    * `stride` - 2
                                    * `padding` - default


    * TODO -
        * http://cs231n.github.io/convolutional-networks/


* AWS GPU Instances
    * https://classroom.udacity.com/nanodegrees/nd889/parts/16cf5df5-73f0-4afa-93a9-de5974257236/modules/53b2a19e-4e29-4ae7-aaf2-33d195dbdeba/lessons/2df3b94c-4f09-476a-8397-e8841b147f84/concepts/ced1fa22-5723-4212-b73f-08f7f6613cae
    * About:
        * GPU-enabled server instance on AWS to train
        larger NN architectures
        * Alternative to fast training of NNs on local CPU
        when built-in is not fast Nvidia GPU
    * Setup:
        * Login to AWS account https://aws.amazon.com/
            * Using Root https://www.amazon.com/ap/signin?openid.assoc_handle=aws&openid.return_to=https%3A%2F%2Fsignin.aws.amazon.com%2Foauth%3Fresponse_type%3Dcode%26client_id%3Darn%253Aaws%253Aiam%253A%253A015428540659%253Auser%252Fhomepage%26redirect_uri%3Dhttps%253A%252F%252Fconsole.aws.amazon.com%252Fconsole%252Fhome%253Fstate%253DhashArgs%252523%2526isauthcode%253Dtrue%26noAuthCookie%3Dtrue&openid.mode=checkid_setup&openid.ns=http://specs.openid.net/auth/2.0&openid.identity=http://specs.openid.net/auth/2.0/identifier_select&openid.claimed_id=http://specs.openid.net/auth/2.0/identifier_select&openid.pape.preferred_auth_policies=MultifactorPhysical&openid.pape.max_auth_age=0&openid.ns.pape=http://specs.openid.net/extensions/pape/1.0&server=/ap/signin&forceMobileApp=&forceMobileLayout=&pageId=aws.ssop&ie=UTF8
        * View EC2 Service Limits https://console.aws.amazon.com/ec2/v2/home?#Limits
        * Find EC2 Instance Limit for **p2.xlarge** instance type
        (virtual server with GPU)
        * Submit a Limit Increase Request to
        increase **p2.xlarge** to 1 with use case:
        "I would like to use GPU instances for deep learning."
    * Launch Instance
        * Visit EC2 Management Console https://console.aws.amazon.com/ec2/v2/home
            * Click "Launch Instance" Button
            * Select Amazon Machine Image (AMI) with OS for instance and
            config and pre-installed software
                * Open Existing AMI
                    * Go to "Community AMIs"
                    * Search for "udacity-aind2" AMI
                    * Click "Select"
            * Choose Instance Type (i.e. hardware the AMI runs on)
                * Filter to only list "GPU compute"
                * Select "p2.xlarge"
                * Click "Review and Launch"
            * Configure Security Group
            (allow special config to allow running and accessing
            Jupyter Notebook from AWS)
                * Allow access the Jupyter notebook
                to port 8888 by configuring the
                AWS Security Group
                * Click "Edit Security groups"
                * On the "Configure Security Group" page:
                    * Select "Create a new security group"
                    * Set the "Security group name" to "Jupyter"
                    * Set the "Description" to "Jupyter"
                    * Click "Add Rule"
                    * Set a "Custom TCP Rule"
                    * Set the "Port Range" to "8888"
                    * Select "Anywhere" as the "Source"
                    * Click "Review and Launch" (again)
                * Note:
                    * EC2 Pricing https://aws.amazon.com/ec2/pricing/on-demand/
                * **WARNING: AWS CHARGES FEE FROM THIS POINT ONWARDS**
                    * EC2 Instances cost $
                        * Shutdown instances when not using them
                    * Storage of EC2 Instances cost $
                        * Delete EC2 instances when not using them
                    * ACTIONS > INSTANCE STATE > STOP/TERMINATE
                    * Note:
                        * Set AWS Billing Alarms
                * Click "Launch" button to launch the GPU instance
                * Click "Proceed without a key pair"
                * Click "Launch Instances" button
                * Click "View Instances" button
                * Go to ECS Management Console to watch instance booting
                    * When it says "2/2 checks passed" then the
                    instance is ready to be logged into
                    * An "IPv4 Public IP" address
                    (in the format of “X.X.X.X”) appears
                    on the EC2 Dashboard
                * Open Terminal and SSH to that address
                as user "aind2":
                    `ssh aind2@X.X.X.X`
                * Authenticate with the password: aind2
                * Success: Now we have a GPU-enabled server on which to
                train your Neural Networks
    * Launch Jupyter Notebook using EC2 Instance
        * On the EC2 Instance
        * Clone the aind2-cnn repo:
            ```
            git clone https://github.com/udacity/aind2-cnn.git
            source activate aind2
            cd aind2-cnn
            jupyter notebook --ip=0.0.0.0 --no-browser
            ```
        * Find the output window line that looks like:
        * Copy/paste this URL into your browser when you connect
        for the first time to login with a token:
          http://0.0.0.0:8888/?token=3156e...
        * Copy and paste the complete URL into the address bar of a web browser
        (Firefox, Safari, Chrome, etc).
        Before navigating to the URL,
        replace 0.0.0.0 in the URL with the "IPv4 Public IP" address from the EC2 Dashboard.
        Press Enter.
        * Note that the browser should display the folders contained in the aind2-cnn repo
        * Verify that the EC2 Instance can run a
        Jupyter Notebook by using https://github.com/udacity/aind2-cnn/blob/master/cifar10-classification/cifar10_mlp.ipynb
            * Click "cifar10-classification"
            * Click "cifar10_mlp.ipynb"
            * Run All Cells in notebook
    * Shutdown and Delete EC2 instance

* Example CNN in Keras using AWS EC2 with cifar10-classification
    * Video https://www.youtube.com/watch?v=faFvmGDwXX0
    * Link
        * Cheatsheet for CNNs in Keras
            * https://s3.amazonaws.com/assets.datacamp.com/blog_assets/Keras_Cheat_Sheet_Python.pdf
        * CIFAR-10 Winning Architecture
            * http://blog.kaggle.com/2015/01/02/cifar-10-competition-winners-interviews-with-dr-ben-graham-phil-culliton-zygmunt-zajac/
    * Note: Previously we used `validation_split` argument in model.fit to 0.2.
    This removed the final 20% of the training data, which was instead used as validation data.
    Alternatively instead of having Keras split off the validation set for us,
    we may opt to hard-code the split ourselves

* Mini Project: CNNs in Keras
    * Modify architecture of the neural network in cifar10_cnn.ipynb.
    * Specify new CNN architecture in Step 5: Define the Model Architecture in the notebook.
        * Example https://github.com/fchollet/keras/blob/master/examples/cifar10_cnn.py
    * Train the new model. Check the accuracy on the test dataset,
    and report the percentage in the text box below.
    * Try different optimiser

* Example Image Augmentation in Keras

    * Links
        * Video https://www.youtube.com/watch?v=odStujZq3GY
        * Keras `ImageDataGenerator` https://keras.io/preprocessing/image/
        * Visualise Augmentations of MNIST dataset http://machinelearningmastery.com/image-augmentation-deep-learning-keras/
        * Augmentation to boost performance on Kaggle dataset
         using less data https://blog.keras.io/building-powerful-image-classification-models-using-very-little-data.html

    * Image Augmentation
        * Focus algorithm on learning an **Invariant Representation**
        of the image that just checks if object is present in image or not
        without dwelling on irrelevant info. Do not want the model to change
        its prediction based on any of the following different attributes of the
        object, which are not relevant
            * Size **Scale Invariance**
            * Angle **Rotation Invariance**
            * Position **Translation Invariance**
        * Technique to increase Invariance of images that **Expands the dataset**
        by **Data Augmenting** (Data Augmentation also avoids Overfitting and is
         better at generalising since model sees many new images)
            * Add images to dataset at random Rotations to increase **Rotation Invariance**
            * etc

        * MY CODE AND NOTES
            * https://github.com/ltfschoen/aind2-cnn/tree/master/cifar10-augmentation

        * Note: Some CNNs have some built-in Translation Invariance
        (i.e. in Max Pooling where we take max value contained in Window,
        can occur at any pixel in that Window)

    * Jupyter Notebook
        aind2-cnn/cifar10-augmentation/cifar10_augmentation.ipynb

    * Note on `steps_per_epoch`
        * Recall that `fit_generator` takes many parameters, including
        `steps_per_epoch = x_train.shape[0] / batch_size`
        where `x_train.shape[0]` corresponds to number of unique samples
        in the training dataset x_train.
        By setting `steps_per_epoch` to this value, we ensure that the model
        sees `x_train.shape[0]` augmented images in each epoch.


* Groundbreaking CNN Architectures for Object Classification Tasks
    * Top CNN Models in Keras
        * https://keras.io/applications/
    * Benchmarks of Top CNN Models architectures in Keras https://github.com/jcjohnson/cnn-benchmarks
    * TODO - ImageNet Paper http://papers.nips.cc/paper/4824-imagenet-classification-with-deep-convolutional-neural-networks.pdf
        * 10M images drawn from 1000 different image categories
        * In 2012, **AlexNet Architecture** was built from the ImageNet Large Scale Visual Recognition Competition
        using best available GPUs.
            * Pioneered the use of the ReLU Activation Function and Dropout technique
            to avoid Overfitting
        * In 2014, **VGG / VGGNet Architecture** (Visual Geometry Group)
            * VGG16 has 16 total layers, VGG19 has 19 total layers
            * Each have long sequence of 3x3 convolutions broken up by 2x2 pooling layers, and
            finished with 3x fully connected "dense" layers
            * Pioneered use of small 3x3 convolution windows (instead of AlexNets 11x11 windows)
        * In 2015, **ResNet Architecture** (similar to VGG, by Microsoft)
            * One version has CNN with 152 layers
            (previously above a certain number of layers the performance declined due to the
            **Vanishing Gradients Problem** that arises when go to train the CNN through backpropagation
            where the gradient signal must be pushed through the entire network, and in deeper network
            its more likely the signal gets weakened before arriving at destination)
            * ResNet adds Connections to deep CNN at skip layers so gradient has shorter route to travel
            to achieve superhuman performance in image classification
    * TODO - VGGNet Paper https://arxiv.org/pdf/1409.1556.pdf
    * TODO - ResNet Paper https://arxiv.org/pdf/1512.03385v1.pdf
    * Treatment of Vanishing Gradients Problem http://neuralnetworksanddeeplearning.com/chap5.html
    * ImageNet Large Scale Visual Recognition Competition (ILSVRC) http://www.image-net.org/challenges/LSVRC/

* Visualising CNNs to understand them
    * Visualising Activation Maps in Convolutional Layers to dig deeper in how CNN works
        * Example: Pass Webcam through CNN in real-time
    * Take Filters from Convolutional Layers and constructing images that Maximise their Activations
        * Steps:
            * Start with image containing Random Noise
            * Gradually amend the pixels at each step changing them to values that filter more
            highly activated
        * 1st Convolutional Layer
            * Random Noise
            * Filter that detects specific Colours or Edges
            * Output image
        * 2nd Convolutional Layer
            * Filter that detects Circles or Stripes
            * Output image
        * 5th Convolutional Layer
            * Filter detects Complex Patterns
            * Output image
        * Example:
            * Deep Dreams
                * Starting image (i.e. a Tree) is applied a Filter (i.e. a "Statue") that
                transforms it into a Hybrid

    * Links
        * Course on Visualising CNNs http://cs231n.github.io/understanding-cnn/
        * Openframeworks.cc for visualising CNNs in real-time http://openframeworks.cc/
            * Demo https://aiexperiments.withgoogle.com/what-neural-nets-see
        * WaveNets to generate audio
        * DeepVis Toolbox https://github.com/yosinski/deep-visualization-toolbox
            * http://ml4a.github.io/
        * Visualisation Tool https://www.youtube.com/watch?v=AgkfIQ4IGaM&t=78s
        * Creating CNN Visualisations https://www.youtube.com/watch?v=ghEmQSxT6tw&t=5s
        * Clarifai.com https://www.clarifai.com/
        * Picasso Visualiser for CNNs in Keras and TensorFlow https://medium.com/merantix/picasso-a-free-open-source-visualizer-for-cnns-d8ed3a35cfc5
        * Visualizing how CNNs see the world. Introduction to Deep Dreams,
        along with code for writing your own deep dreams in Keras
            * https://blog.keras.io/how-convolutional-neural-networks-see-the-world.html
        * Music Video uses DeepDreams 3:15-3:40
            * https://www.youtube.com/watch?v=XatXy6ZhKZw
        * Create DeepDreams without code https://deepdreamgenerator.com/
        * Interpretability of CNNs
            * Dangers from using deep learning models (that are not yet interpretable)
            in real-world applications https://blog.openai.com/adversarial-example-research/
            * https://arxiv.org/abs/1611.03530


* How a CNN works in Action
    * Given an image trained on ImageNet http://www.matthewzeiler.com/pubs/arxive2013/eccv2014.pdf
    * Each image represents a pattern that causes the neurons in the first layer to activate
    (i.e. they are patterns that the first layer recognizes, such as a -45 degree line).
    * **First Layer** of our CNN clearly picks out very simple shapes and patterns like lines and blobs
    * **Second Layer** picks up more complex ideas like circles and stripes.
    A grayscale grid may be used to represent how the layer of the CNN activates
    (or "what it sees") based on the corresponding images from the grid on the right.
    Second layer of the CNN captures complex ideas like circles, stripes, and rectangles.
    CNN learns to do this on its own. There is no special instruction for the CNN to focus
    on more complex objects in deeper layers. That's just how it normally works out when
    you feed training data into a CNN.
    * **Third Layer** third layer picks out complex combinations of features from the second layer.
    These include things like grids, and honeycombs, wheels, and even faces
    * **Final Layer** last layer picks out the highest order ideas that we care about for
    classification, like dog faces, bird faces, and bicycles

* **Transfer Learning**
    * Links
        * Slides https://classroom.udacity.com/nanodegrees/nd889/parts/16cf5df5-73f0-4afa-93a9-de5974257236/modules/53b2a19e-4e29-4ae7-aaf2-33d195dbdeba/lessons/2df3b94c-4f09-476a-8397-e8841b147f84/concepts/8c202ff3-aab5-46c3-8ed1-0154fa7b566b

        * Systematically analyzes the transferability of features learned in pre-trained CNNs
            * https://arxiv.org/pdf/1411.1792.pdf
        * Cancer detecting CNN https://www.nature.com/articles/nature21056.epdf

    * Steps Summary
        * Deep Learning Professionals Design the Architecture of CNN
            * Setting Hyperparameters (Filter Window size, stride, padding)
            * Choose Loss Function and Optimiser
            * Start Model Training and wait
        * Note: **State-of-art CNNs architectures available in Keras
        are result of experimenting with numerous architectures
        and extensive Hyperparameter tuning and
        are trained on large ImageNet database that
        took weeks to train on latest GPUs**

    * About
        * **Adapt the State-of-the-art CNN Architectures** that have learnt so much
        about how to find Patterns in image datasets toward our own
        Classification tasks (instead of creating CNN from scratch, take the
        learnt understanding and pass it on to a new Deep Learning Model)
        using the **Transfer Learning Technique**

        * Transfer Learning Approach
            * Removing the Final Classification Layers of a State-of-the-art
            CNN Inception Architecture (i.e. Conv, Pool, Dense)
            that are specifically pre-trained on the ImageNet data set
            (containing animals, fruits, etc), but retain the early layers
            (detecting colors, shapes, general features), and replace with
            a new Dense layer and only train that
            * Update new Dense layer by:
                * randomly initialising the Weights
                in the new Dense layer
            * Update all other existing layers by:
                * initialise Weights using pre-trained Weights
            * During re-training of the entire neural network
            the parameters were
            further fine-tuned / optimised to fit to the
            custom database (i.e. of skin lesions)
        * Example
            * Diagnosing skin cancer

    * Different Approach for different Cases
        * Case 1: Small Data Set, Similar Data
        * Case 2: Small Data Set, Different Data
        * Case 3: Large Data Set, Similar Data
        * Case 4: Large Data Set, Different Data

        * **See CNN Part 27. Transfer Learning Slide for details**


    * Notes:
        * Small dataset of images - 1,000
            * Overfitting is concern when using transfer learning
            with a small data set.
        * Large dataset of images - 1,000,000

        * **Global Average Pooling (GAP) layers** were proposed in 2016
        and are not only used for **Object Classification** (what is in the image)
        but also **Object Localisation** (where the object is in the image)

    * Example
        * Video - https://www.youtube.com/watch?v=HsIAznMM1LA

        * Code `transfer-learning/transfer_learning.ipynb`

        Note:
            * After training CNN to identify dog breeds, use the model
            in an end-to-end pipeline for incorporation into an app

    * Links
        * Global Average Pooling (GAP) Layers for Object Localisation using ResNet http://cnnlocalization.csail.mit.edu/Zhou_Learning_Deep_Features_CVPR_2016_paper.pdf
        * Code that uses CNN for object localisation https://github.com/alexisbcook/ResNetCAM-keras
        * Video on CNN object localisation https://www.youtube.com/watch?v=fZvOy0VXWAI
        * Visualization techniques to better understand bottleneck features
        https://github.com/alexisbcook/keras_transfer_cifar10