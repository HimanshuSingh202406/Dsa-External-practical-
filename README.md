#include<iostream>
#include<string>
#include<stack>
using namespace std;
bool isOperand(char t) {
    if (t >= 'a' && t <= 'z')    return true;
    else                return false;
}
bool isOperator(char t) {
    switch (t) {
    case '+':
    case '-':
    case '/':
    case '*':
    case '^':
        return true;
    }
    return false;
}
// returns operator prece
int prece(char t) {
    if (t == '+' || t == '-')    return 1;
    if (t == '*' || t == '/')    return 2;
    if (t == '^')    return 3;
    return 0;
}
//    returns 1 for left associativity and 0 for right associativity
int assoc(char t) {
    if (t == '+' || t == '-' || t == '*' || t == '*')    return 1;
    if (t == '^')    return 0;
}
string convertInfixToPostfix(string infix) {
    // operator stack
    stack<char> opStack;
    // it stores final postfix expression
    string postfix;
    // it stores each token of infix expression
    char token;
    postfix = "";
    for (int i = 0; i < infix.length(); ++i) {
        token = infix[i];
        if (token == '(') {
            opStack.push(token);
        }
        else if (isOperand(token)) {
            postfix.push_back(token);
        }
        else if (isOperator(token)) {
            while (isOperator(opStack.top()) && (
                    (prece(opStack.top()) > prece(token)) ||
                    (prece(opStack.top()) == prece(token) && assoc(opStack.top()) && assoc(token))
                )
            ){
                char temp = opStack.top();
                opStack.pop();
                postfix.push_back(temp);
            }
            opStack.push(token);
        }
        else if (token == ')') {
            while (opStack.top() != '(') {
                char temp = opStack.top();
                opStack.pop();
                postfix.push_back(temp);
            }
            // pop again to remove extra '(' from the stack
            // as it is already paired with ')' 
            opStack.pop();
        }
    }
    return postfix;
}
int main() {
    // it stores infix expression
    string infix;
    cout << "Enter Infix Expression: ";
    cin >> infix;
    infix = "(" + infix + ")";
    string postfix = convertInfixToPostfix(infix);
    cout << "postfix Expression is: " << postfix << endl;
}
