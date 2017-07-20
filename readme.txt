package com.example.a69430.calculate3;

import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import java.text.DecimalFormat;
import java.util.StringTokenizer;
import android.util.Log;
import android.view.Menu;
import android.view.View.OnClickListener;
import android.widget.EditText;;

public class MainActivity extends AppCompatActivity {

    private Button btn0, btn1, btn2, btn3,btn4,btn5,btn6,btn7,btn8,btn9;
    private EditText input;
    private Button div, mul, sub, add, equal, sqrt, square, back, left, right, dot, dec;
    public String str_old;
    public String str_new;
    public boolean vbegin = true;
    public boolean tip_lock = true;
    public boolean equals_flag = true;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        InitWigdet();
        AllWigdetListener();
    }
    @Override
    public boolean onCreateOptionsMenu(Menu menu) {
        getMenuInflater().inflate(R.menu.menu_main, menu);
        return true;
    }

    private void InitWigdet() {
        input = (EditText) findViewById(R.id.input);
        btn0 = (Button) findViewById(R.id.zero);
        btn1 = (Button) findViewById(R.id.one);
        btn2 = (Button) findViewById(R.id.two);
        btn3 = (Button) findViewById(R.id.three);
        btn4 = (Button) findViewById(R.id.four);
        btn5 = (Button) findViewById(R.id.five);
        btn6 = (Button) findViewById(R.id.six);
        btn7 = (Button) findViewById(R.id.seven);
        btn8 = (Button) findViewById(R.id.eight);
        btn9 = (Button) findViewById(R.id.nine);
        div = (Button) findViewById(R.id.divide);
        mul = (Button) findViewById(R.id.mul);
        sub = (Button) findViewById(R.id.sub);
        add = (Button) findViewById(R.id.add);
        equal = (Button) findViewById(R.id.equal);
        sqrt = (Button) findViewById(R.id.sqrt);
        square = (Button) findViewById(R.id.square);
        left = (Button) findViewById(R.id.left);
        right = (Button) findViewById(R.id.right);
        dot = (Button) findViewById(R.id.dot);
        dec = (Button) findViewById(R.id.c);
        back = (Button) findViewById(R.id.bksp);
    }

    private void AllWigdetListener() {
        btn0.setOnClickListener(listener);
        btn1.setOnClickListener(listener);
        btn2.setOnClickListener(listener);
        btn3.setOnClickListener(listener);
        btn4.setOnClickListener(listener);
        btn5.setOnClickListener(listener);
        btn6.setOnClickListener(listener);
        btn7.setOnClickListener(listener);
        btn8.setOnClickListener(listener);
        btn9.setOnClickListener(listener);
        div.setOnClickListener(listener);
        mul.setOnClickListener(listener);
        sub.setOnClickListener(listener);
        add.setOnClickListener(listener);
        equal.setOnClickListener(listener);
        sqrt.setOnClickListener(listener);
        square.setOnClickListener(listener);
        left.setOnClickListener(listener);
        right.setOnClickListener(listener);
        dot.setOnClickListener(listener);
        dec.setOnClickListener(listener);
        back.setOnClickListener(listener);
    }
    String[] TipCommand = new String[500];
    int tip_i = 0;

    private OnClickListener listener = new OnClickListener() {
        public void onClick(View v) {
            String command = ((Button) v).getText().toString();
            String str = input.getText().toString();
            if (equals_flag == false
                    && "0123456789.()+-×÷√^".indexOf(command) != -1) {

                if (right(str)) {
                    if ("+-×÷√^)".indexOf(command) != -1) {
                        for (int i = 0; i < str.length(); i++) {
                            TipCommand[tip_i] = String.valueOf(str.charAt(i));
                            tip_i++;
                        }
                        vbegin = false;
                    }
                } else {
                    input.setText("0");
                    vbegin = true;
                    tip_i = 0;
                    tip_lock = true;
                }
                equals_flag = true;
            }
            if ("0123456789.()sincostann!+-×÷√^".indexOf(command) != -1
                    && tip_lock) {
                TipCommand[tip_i] = command;
                tip_i++;
            }
            if ("0123456789.()sincostann!+-×÷√^".indexOf(command) != -1
                    && tip_lock) {
                print(command);

            } else if (command.compareTo("BACK") == 0 && equals_flag) {

                try {
                    input.setText(str.substring(0, str.length() - 1));
                } catch (Exception e) {
                    input.setText("");

                        input.setText("0");
                        vbegin = true;
                        tip_i = 0;
                }
                if (input.getText().toString().compareTo("-") == 0
                        || equals_flag == false) {
                    input.setText("0");
                    vbegin = true;
                    tip_i = 0;
                }
                tip_lock = true;
                if (tip_i > 0)
                    tip_i--;
            } else if (command.compareTo("BACK") == 0 && equals_flag == false) {

                input.setText("0");
                vbegin = true;
                tip_i = 0;
                tip_lock = true;

            } else if (command.compareTo("CLEAR") == 0) {

                input.setText("0");
                vbegin = true;
                tip_i = 0;
                tip_lock = true;
                equals_flag = true;
            } else if (command.compareTo("=") == 0 && tip_lock && right(str)
                    && equals_flag) {
                tip_i = 0;
                tip_lock = false;
                equals_flag = false;
                str_old = str;
                vbegin = true;
                str_new = str.replaceAll("-", "-1×");
                new calc().process(str_new);
            }
            tip_lock = true;
        }
    };

    private void print(String str) {
        if (vbegin) {
            input.setText(str);
        } else {
            input.append(str);
        }
        vbegin = false;
    }

    private boolean right(String str) {
        int i;
        for (i = 0; i < str.length(); i++) {
            if (str.charAt(i) != '0' && str.charAt(i) != '1'
                    && str.charAt(i) != '2' && str.charAt(i) != '3'
                    && str.charAt(i) != '4' && str.charAt(i) != '5'
                    && str.charAt(i) != '6' && str.charAt(i) != '7'
                    && str.charAt(i) != '8' && str.charAt(i) != '9'
                    && str.charAt(i) != '.' && str.charAt(i) != '-'
                    && str.charAt(i) != '+' && str.charAt(i) != '×'
                    && str.charAt(i) != '÷'&& str.charAt(i) != '√'
                    && str.charAt(i) != '^' && str.charAt(i) != '('
                    && str.charAt(i) != ')')
                break;
        }
        if (i == str.length()) {
            return true;
        } else {
            return false;
        }
    }

    public class calc {
        public calc() {
        }
        final int LEN = 500;
        public void process(String str) {
            int weightPlus = 0, topOp = 0, topNum = 0, flag = 1, weightTemp = 0;
            int weight[];
            double number[];
            char ch, ch_gai, operator[];
            String num;
            weight = new int[LEN];
            number = new double[LEN];
            operator = new char[LEN];
            String exp = str;
            StringTokenizer expToken = new StringTokenizer(exp,
                    "+-×÷()√^");
            int i = 0;
            while (i < exp.length()) {
                ch = exp.charAt(i);
                if (i == 0) {
                    if (ch == '-')
                        flag = -1;
                } else if (exp.charAt(i - 1) == '(' && ch == '-')
                    flag = -1;
                if (ch <= '9' && ch >= '0' || ch == '.' || ch == 'E') {
                    num = expToken.nextToken();
                    ch_gai = ch;
                    Log.e("guojs", ch + "--->" + i);
                    while (i < exp.length()
                            && (ch_gai <= '9' && ch_gai >= '0' || ch_gai == '.' || ch_gai == 'E')) {
                        ch_gai = exp.charAt(i++);
                        Log.e("guojs", "i的值为：" + i);
                    }
                    if (i >= exp.length())
                        i -= 1;
                    else {
                        i -= 2;
                    }
                    if (num.compareTo(".") == 0)
                        number[topNum++] = 0;
                    else {
                        number[topNum++] = Double.parseDouble(num) * flag;
                        flag = 1;
                    }
                }
                if (ch == '(')
                    weightPlus += 4;
                if (ch == ')')
                    weightPlus -= 4;
                if (ch == '-' && flag == 1 || ch == '+' || ch == '×'
                        || ch == '÷' || ch == '√'
                        || ch == '^') {
                    switch (ch) {
                        case '+':
                        case '-':
                            weightTemp = 1 + weightPlus;
                            break;
                        case '×':
                        case '÷':
                            weightTemp = 2 + weightPlus;
                            break;
                        case 's':
                            weightTemp = 3 + weightPlus;
                            break;
                        default:
                            weightTemp = 4 + weightPlus;
                            break;
                    }
                    if (topOp == 0 || weight[topOp - 1] < weightTemp) {
                        weight[topOp] = weightTemp;
                        operator[topOp] = ch;
                        topOp++;
                    } else {
                        while (topOp > 0 && weight[topOp - 1] >= weightTemp) {
                            switch (operator[topOp - 1]) {
                                case '+':
                                    number[topNum - 2] += number[topNum - 1];
                                    break;
                                case '-':
                                    number[topNum - 2] -= number[topNum - 1];
                                    break;
                                case '×':
                                    number[topNum - 2] *= number[topNum - 1];
                                    break;
                                case '÷':
                                    if (number[topNum - 1] == 0) {
                                        showError(1, str_old);
                                        return;
                                    }
                                    number[topNum - 2] /= number[topNum - 1];
                                    break;

                                case '√':
                                    if (number[topNum - 1] == 0
                                            || (number[topNum - 2] < 0 && number[topNum - 1] % 2 == 0)) {
                                        showError(2, str_old);
                                        return;
                                    }
                                    number[topNum - 2] = Math.pow(number[topNum - 1], 1 / number[topNum - 2]);
                                    break;

                                case '^':
                                    number[topNum - 2] = Math.pow(
                                            number[topNum - 2], number[topNum - 1]);
                                    break;

                            }
                            topNum--;
                            topOp--;
                        }
                        weight[topOp] = weightTemp;
                        operator[topOp] = ch;
                        topOp++;
                    }
                }
                i++;
            }

            while (topOp > 0) {
                switch (operator[topOp - 1]) {
                    case '+':
                        number[topNum - 2] += number[topNum - 1];
                        break;
                    case '-':
                        number[topNum - 2] -= number[topNum - 1];
                        break;
                    case '×':
                        number[topNum - 2] *= number[topNum - 1];
                        break;
                    case '÷':
                        if (number[topNum - 1] == 0) {
                            showError(1, str_old);
                            return;
                        }
                        number[topNum - 2] /= number[topNum - 1];
                        break;

                    case '√':
                        if (number[topNum - 1] == 0
                                || (number[topNum - 2] < 0 && number[topNum - 1] % 2 == 0)) {
                            showError(2, str_old);
                            return;
                        }
                        number[topNum - 2] = Math.pow(number[topNum - 1], 1 / number[topNum - 2]);
                        break;

                    case '^':
                        number[topNum - 2] = Math.pow(number[topNum - 2], number[topNum - 1]);
                        break;
                }
                topNum--;
                topOp--;
            }
            if (number[0] > 7.3E306) {
                showError(3, str_old);
                return;
            }
            input.setText(String.valueOf(FP(number[0])));
        }

        public double FP(double n) {

            DecimalFormat format = new DecimalFormat("0.#############");
            return Double.parseDouble(format.format(n));
        }

        public double N(double n) {
            double sum = 1;
            for (double i = 1; i <= n; i++) {
                sum = sum * i;
            }
            return sum;
        }

        public void showError(int code, String str) {
            String message = "";
            switch (code) {
                case 1:
                    message = "零不能作除数";
                    break;
                case 2:
                    message = "函数格式错误";
                    break;
                case 3:
                    message = "值太大了，超出范围";
            }
            input.setText("\"" + str + "\"" + ": " + message);
        }
    }
}