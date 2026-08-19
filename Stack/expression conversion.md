# Infix ↔ Prefix ↔ Postfix Conversion Notes

---

# 1. Infix → Postfix

## Steps
- Initialize an empty stack.
- Traverse expression from left to right.
- If operand → add to answer.
- If '(' → push to stack.
- If ')' → pop until '('.
- If operator:
  - Pop while stack has higher precedence.
  - If same precedence, pop only if operator is Left Associative.
  - Push current operator.
- Pop remaining operators.

## Operator Precedence

| Operator | Priority |
|----------|----------|
| ^ | 3 |
| * / | 2 |
| + - | 1 |

Associativity:
- ^ → Right Associative
- + - * / → Left Associative

---

## Code (C++)

```cpp
#include<bits/stdc++.h>
using namespace std;

int prec(char c){
    if(c=='^') return 3;
    if(c=='*' || c=='/') return 2;
    if(c=='+' || c=='-') return 1;
    return -1;
}

string infixToPostfix(string s){

    stack<char> st;
    string ans="";

    for(char ch:s){

        if(isalnum(ch))
            ans+=ch;

        else if(ch=='(')
            st.push(ch);

        else if(ch==')'){

            while(!st.empty() && st.top()!='('){
                ans+=st.top();
                st.pop();
            }
            st.pop();
        }

        else{

            while(!st.empty() &&
                 ((prec(st.top())>prec(ch)) ||
                 (prec(st.top())==prec(ch) && ch!='^'))){

                ans+=st.top();
                st.pop();
            }

            st.push(ch);
        }
    }

    while(!st.empty()){
        ans+=st.top();
        st.pop();
    }

    return ans;
}
```

### Time Complexity

```
O(n)
```

### Space Complexity

```
O(n)
```

---

# 2. Infix → Prefix

## Steps

- Reverse infix.
- Swap '(' and ')'.
- Convert to postfix.
- Reverse postfix.

---

## Code

```cpp
#include<bits/stdc++.h>
using namespace std;

int prec(char c){
    if(c=='^') return 3;
    if(c=='*'||c=='/') return 2;
    if(c=='+'||c=='-') return 1;
    return -1;
}

string infixToPrefix(string s){

    reverse(s.begin(),s.end());

    for(char &c:s){
        if(c=='(') c=')';
        else if(c==')') c='(';
    }

    stack<char> st;
    string ans="";

    for(char ch:s){

        if(isalnum(ch))
            ans+=ch;

        else if(ch=='(')
            st.push(ch);

        else if(ch==')'){

            while(st.top()!='('){
                ans+=st.top();
                st.pop();
            }
            st.pop();
        }

        else{

            while(!st.empty() &&
            ((prec(st.top())>prec(ch)) ||
            (prec(st.top())==prec(ch) && ch=='^'))){

                ans+=st.top();
                st.pop();
            }

            st.push(ch);
        }
    }

    while(!st.empty()){
        ans+=st.top();
        st.pop();
    }

    reverse(ans.begin(),ans.end());

    return ans;
}
```

### Time Complexity

```
O(n)
```

### Space Complexity

```
O(n)
```

---

# 3. Postfix → Infix

## Steps

- Traverse left to right.
- Operand → push as string.
- Operator:
  - Pop op2.
  - Pop op1.
  - Push

```
(op1 operator op2)
```

---

## Code

```cpp
#include<bits/stdc++.h>
using namespace std;

string postfixToInfix(string s){

    stack<string> st;

    for(char ch:s){

        if(isalnum(ch)){
            st.push(string(1,ch));
        }

        else{

            string op2=st.top(); st.pop();
            string op1=st.top(); st.pop();

            string temp="("+op1+ch+op2+")";

            st.push(temp);
        }
    }

    return st.top();
}
```

### Time Complexity

```
O(n)
```

### Space Complexity

```
O(n)
```

---

# 4. Prefix → Infix

## Steps

- Traverse Right → Left.
- Operand → push.
- Operator:
  - Pop op1.
  - Pop op2.
  - Push

```
(op1 operator op2)
```

---

## Code

```cpp
#include<bits/stdc++.h>
using namespace std;

string prefixToInfix(string s){

    stack<string> st;

    for(int i=s.size()-1;i>=0;i--){

        char ch=s[i];

        if(isalnum(ch)){
            st.push(string(1,ch));
        }

        else{

            string op1=st.top(); st.pop();
            string op2=st.top(); st.pop();

            string temp="("+op1+ch+op2+")";

            st.push(temp);
        }
    }

    return st.top();
}
```

### Time Complexity

```
O(n)
```

### Space Complexity

```
O(n)
```

---

# 5. Prefix → Postfix

## Steps

- Traverse Right → Left.
- Operand → push.
- Operator:
  - Pop op1.
  - Pop op2.
  - Push

```
op1 op2 operator
```

---

## Code

```cpp
#include<bits/stdc++.h>
using namespace std;

string prefixToPostfix(string s){

    stack<string> st;

    for(int i=s.size()-1;i>=0;i--){

        char ch=s[i];

        if(isalnum(ch))
            st.push(string(1,ch));

        else{

            string op1=st.top(); st.pop();
            string op2=st.top(); st.pop();

            st.push(op1+op2+ch);
        }
    }

    return st.top();
}
```

### Time Complexity

```
O(n)
```

### Space Complexity

```
O(n)
```

---

# 6. Postfix → Prefix

## Steps

- Traverse Left → Right.
- Operand → push.
- Operator:
  - Pop op2.
  - Pop op1.
  - Push

```
operator op1 op2
```

---

## Code

```cpp
#include<bits/stdc++.h>
using namespace std;

string postfixToPrefix(string s){

    stack<string> st;

    for(char ch:s){

        if(isalnum(ch))
            st.push(string(1,ch));

        else{

            string op2=st.top(); st.pop();
            string op1=st.top(); st.pop();

            st.push(ch+op1+op2);
        }
    }

    return st.top();
}
```

### Time Complexity

```
O(n)
```

### Space Complexity

```
O(n)
```

---

# Interview Cheat Sheet

| Conversion | Traversal | Stack Stores |
|------------|-----------|--------------|
| Infix → Postfix | Left → Right | Operators |
| Infix → Prefix | Reverse → Postfix → Reverse | Operators |
| Postfix → Infix | Left → Right | Strings |
| Prefix → Infix | Right → Left | Strings |
| Prefix → Postfix | Right → Left | Strings |
| Postfix → Prefix | Left → Right | Strings |

---

# Things to Remember

- `isalnum(ch)` → Operand
- Infix conversions use an **operator stack**.
- Prefix/Postfix conversions use a **string stack**.
- `^` is **Right Associative** (special case while comparing precedence).