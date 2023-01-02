# Reverse Polish Notation

## https://leetcode.com/problems/evaluate-reverse-polish-notation/

You are given an array of strings tokens that represents an arithmetic expression in a Reverse Polish Notation.
Evaluate the expression. Return an integer that represents the value of the expression.
Note that:
The valid operators are '+', '-', '\*', and '/'.
Each operand may be an integer or another expression.
The division between two integers always truncates toward zero.
There will not be any division by zero.
The input represents a valid arithmetic expression in a reverse polish notation.
The answer and all the intermediate calculations can be represented in a 32-bit integer.
`

- Example 1:
  Input: tokens = ["2","1","+","3","*"]
  Output: 9
  Explanation: ((2 + 1) \* 3) = 9
- Example 2:
  Input: tokens = ["4","13","5","/","+"]
  Output: 6
  Explanation: (4 + (13 / 5)) = 6
- Example 3:
  Input: tokens = ["10","6","9","3","+","-11","*","/","*","17","+","5","+"]
  Output: 22
  Explanation: ((10 _ (6 / ((9 + 3) _ -11))) + 17) + 5
  = ((10 _ (6 / (12 _ -11))) + 17) + 5
  = ((10 _ (6 / -132)) + 17) + 5
  = ((10 _ 0) + 17) + 5
  = (0 + 17) + 5
  = 17 + 5
  = 22
  `

```cpp
class Solution {
public:
    bool isOperator(string token){
        if(token == "+" || token == "-" || token == "*" || token == "/")return true;
        return false;
    }

    int evalRPN(vector<string>& tokens) {
    /*
        The reverse pollish notation show a LIFO behaviour so we will use a Stack to evaluate the expression <operand><operator><operator>
        Algorithm :
         1. Push the elements to the stack
         2. Whenever a operand is seen do the operation on the last two operands that are operand first and operand second (oprd1 , oprd2)
         3. Push the result back into the stack
         4. Goto step 1
    */
        int evaluated_result = 0;
        stack<int>myStack;
        for(int i = 0 ; i < tokens.size() ; i++){
            string token = tokens[i];
            if(isOperator(token)){
                // If the current token is an operator
                int operand_first = myStack.top();
                myStack.pop();
                int operand_second = myStack.top();
                myStack.pop();
                int temp_result;
                if(token == "/"){
                    temp_result = operand_second / operand_first;
                }else if(token == "*"){
                    temp_result = operand_second * operand_first;
                }else if(token == "-"){
                    temp_result = operand_second - operand_first;
                }else if(token == "+"){
                    temp_result = operand_second + operand_first;
                }
                myStack.push(temp_result);
            }else{
                // Current token is an operand
                myStack.push(stoi(token));
            }
        }
        evaluated_result = myStack.top();
        return evaluated_result;
    }
};

```
