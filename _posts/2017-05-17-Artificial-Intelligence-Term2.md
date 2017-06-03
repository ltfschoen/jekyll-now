---
layout: post
title: Artificial Intelligence Nanodegree Term 2
---

# Table of Contents
  * [Chapter 1 - Artificial Intelligence Nanodegree](#chapter-1)

## Chapter 1 - Artificial Intelligence Nanodegree Term 2 <a id="chapter-1"></a>

### TODO

* AWS Training Free
    * https://www.aws.training
    * https://www.awseducate.com/microsite/Training

### Instructors

* Luis Serrano - Course Developer / Instructor (did ML for Google Youtube recommendations)
* Alexis Cook - Applied maths / biologist, uses Deep Learning
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
                    P(duck) == 0.23,
                    P(beaver) == 0.54, and
                    P(walruss) == 0.21
            ```

            * **Softmax Function**
                * i.e. equivalent to Sigmoid Function but for when problem has 3+ classes

