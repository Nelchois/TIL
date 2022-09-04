## Using Stack Question.2
Make calculator for arithmetic operation.
```
import re
import sys

def tokenize(expStr):
    pat = re.compile(r'(?:(?<=[^\d\.])(?=\d)|(?=[^\d\.]))', re.MULTILINE)
    return [x for x in re.sub(pat, ' ', expStr).split(' ') if x]

def Postfix():
    equation = input('수식입력 :')
    token = tokenize(equation)
    out_stack = []
    op_stack = []
    for i in token:
        if i not in "+-*/()":
            out_stack.append(i)
        elif i in "+-":
            if len(op_stack) != 0:
                if (op_stack[-1] == "*") or (op_stack[-1] == "/"):
                    operator = op_stack.pop()
                    out_stack.append(operator)
                    op_stack.append(i)
                elif (op_stack[-1] != "*") and (op_stack[-1] != "/"):
                    op_stack.append(i)
            elif len(op_stack) == 0:
                op_stack.append(i)
        elif i in "*/":
            op_stack.append(i)
        elif i in "()":
            if i == "(":
                op_stack.append(i)
            else:
                for _ in range(len(op_stack)):
                    if op_stack[-1] != "(":
                        operator = op_stack.pop()
                        out_stack.append(operator)
                    else:
                        dummy_parentheses = op_stack.pop()
                        break
    if len(op_stack) != 0:
        for _ in range(len(op_stack)):
            left_operator = op_stack.pop()
            out_stack.append(left_operator)
    return out_stack[::-1]

def calculator():
    oper_list = []
    token_list = Postfix()
    while len(token_list) != 0:
        i = token_list.pop()
        if i not in "+-*/":
            oper_list.append(i)
        elif i in "+-*/" and len(oper_list) != 0:
            B = int(oper_list.pop())
            A = int(oper_list.pop())
            if i == "+":
                app_ans = A + B
                oper_list.append(app_ans)
            elif i == "-":
                app_ans = A - B
                oper_list.append(app_ans)
            elif i == "*":
                app_ans = A * B
                oper_list.append(app_ans)
            elif i == "/":
                app_ans = A / B
                oper_list.append(app_ans)
        elif len(oper_list) == 1:
            break
    return oper_list[0]
```