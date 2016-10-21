Preparatory Notes ahead of Code Test on Algorithms for Software Engineer job

Prospective employer is assessing our "problem solving" methodology. Follow these steps when solving the problem. to be used during the code test:


OBJECT-ORIENTED
===============
- Use object-oriented techniques (i.e. classes, helpers, etc)
- Use more than one language in code tests when full stack role


PROBLEM DEFINITION
==================

- Rewrite the problem definition as generic mathematical equations


UNIT TESTING (during development)
=================================

1) Write unit tests for the algorithm first before coding implementation
	- Reason 1: define expectations when implementing algorithm from literature
	- Reason 2: consolidate thoughts when developing or prototyping a new idea
	- Reason 3: improve trust and quality of the code

	i) Write Deterministic tests before Probabilistic tests:
		- Deterministic tests check that operations work correctly
		- Probabilistic tests should be converted to a Deterministic Mock if possible
			-	Check behaviour is as intended for large number of test cases
			- Check idempotent (safe operation with no harm performing two or more times)
	ii) Unit test to verify small/simple behaviour of single aspect of a unit of code
	iii) Unit test the inputs and outputs (I/O) of unit of code, specifically pre-conditions and post-conditions
	iv) Unit test within scope of own codinge black box, but not verifying behaviour of other separate libraries or frameworks
	v) Unit test independence between objects to reduce side-effects of a unit of code
	vi) Ensure independence between unit tests that may be performed in any order
	vii) Unit test multiple test cases for solutions to verify correct:
		- Try to find situations where it could fail
		- Handling for all possible input
		- Handling of corner cases (i.e. empty inputs, very small inputs, very large inputs, worst possible data allowed)
			- Handling without failure large input data sets if time complexity is O(n^2)
				i.e.
				- Handles extreme empty
				- Handles extreme first
				- Handles extreme large input numbers (testing arithmetic overflow) (i.e. if input array contains values like MIN/MAX_INT)
				- Handles extreme last
				- Handles extreme single zero
				- Handles extreme sequence with sum of 0
				- Handles combinations of 2 and 3 (i.e. {a, b, c}^2 and {a, b, c}^3 for integer values of a, b, c)
				- Handles large long sequence of 1 
				- Handles large long sequence of -1
				- Handles large pyramid (performance test to check if O(n^2) solutions fail)

		- Scalability (code remains practical as data size grows)

		> REMEDIES
			- Use different Algorithm to solve problem: "large input data sets, since the time complexity is O(n^2)"

				- e.g. Rotate Array -> Split array and evaluate each to avoid nested loop using O(n*k) time and O(1) space
				http://www.programcreek.com/2015/03/rotate-array-in-java/

				- e.g. Evaluate Input -> Use stack, loop over array, push and pop to stack depending on input element value
				http://www.programcreek.com/2012/12/leetcode-evaluate-reverse-polish-notation/

				- e.g. Isomorphic Inputs -> Populate and check against Hashmap with unique mapping 
				http://www.programcreek.com/2014/05/leetcode-isomorphic-strings-java/ 

				- e.g. Transformation sequence word ladder using Inputs -> 

			- Use different Data Type (i.e. Long instead of Int) to solve problem: "large input numbers (testing arithmetic overflow)"

2) Write code in small functions and refactor to ease testability
3) Refactor and use Helper functions

INTEGRATION TESTING (after modules and systems completed)
===================

USER ACCEPTANCE TESTING (stakeholders determine if needs met)
===================

References: 
- http://www.cleveralgorithms.com/nature-inspired/advanced/testing_algorithms.html

TODO: 
- Algorithms - http://www.programcreek.com/2012/11/top-10-algorithms-for-coding-interview/
- Algorithms in JS - http://www.thatjsdude.com/interview/linkedList.html
- Language challenges - https://www.codewars.com/
- Mina instead of Capistrano (faster)?