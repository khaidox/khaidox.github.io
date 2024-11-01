---
created: 2023-11-01
title:  Determine the current location on a 2D map from Wi-Fi access points
layout: post
tags: [msp430,esp8266]
category: [MCU]
author: khaido
comments: true
---

> This is a note on my past study progress


Trong không gian 2D xác định vị trí hiện tại của thiết bị thông qua thông số RSSI bằng MSP430G2553 và Wifi ESP8266 biết tọa độ của 3 vị trí wifi.
Sơ đồ hệ thống:
![dl1](https://khaidox.github.io/assets/img/msp430/1.png)

# 1. Tính khoảng cách từ vị trí đến ba wifi.

![dl1](https://khaidox.github.io/assets/img/msp430/4.png)

Thư viện MSP430 có hỗ trợ hàm pow() để tính lũy thừa, nhưng nếu ta sử dụng hàm có sẵn này, kích thước file chương trình nạp vào bộ nhớ MSP430 sẽ tăng lên đáng kể, dễ gây ra tình trạng gần đầy bộ nhớ flash. Giải pháp là tự xây dựng hàm pow() dựa trên chuỗi Taylor để khắc phục vấn đề tràn bộ nhớ.

Ta có:
![dl1](https://khaidox.github.io/assets/img/msp430/5.png)

```
#define MAX_DELTA_DOUBLE 1.0E-15
#define EULERS_NUMBER 2.718281828459045

double MathAbs_Double(double x)
{
    return ((x >= 0) ? x : -x);
}

int MathAbs_Int(int x)
{
    return ((x >= 0) ? x : -x);
}

double MathPow_Double_Int(double x, int n)
{
    double ret;
    if ((x == 1.0) || (n == 1))
    {
        ret = x;
    }
    else if (n < 0) {
        ret = 1.0 / MathPow_Double_Int(x, -n);
    }
    else
    {
        ret = 1.0;
        while (n--)
        {
            ret *= x;
        }
    }
    return (ret);
}

double MathLn_Double(double x)
{
    double ret = 0.0, d;
    if (x > 0)
    {
        int n = 0;
        do
        {
            int a = 2 * n + 1;
            d = (1.0 / a) * MathPow_Double_Int((x - 1) / (x + 1), a);
            ret += d;
            n++;
        } while (MathAbs_Double(d) > MAX_DELTA_DOUBLE);
    }
    else
    {
    }
    return (ret * 2);
}

double MathExp_Double(double x)
{
    int i;
    double ret;
    if (x == 1.0)
    {
        ret = EULERS_NUMBER;
    }
    else if (x < 0)
    {
        ret = 1.0 / MathExp_Double(-x);
    }
    else
    {
        int n = 2;
        double d;
        ret = 1.0 + x;
        do {
            d = x;
            for ( i = 2; i <= n; i++)
            {
                d *= x / i;
            }
            ret += d;
            n++;
        } while (d > MAX_DELTA_DOUBLE);
    }
    return (ret);
}

double MathPow_Double_Double(double x, double a)
{
    double ret;
    if ((x == 1.0) || (a == 1.0))
    {
        ret = x;
    }
    else if (a < 0)
    {
        ret = 1.0 / MathPow_Double_Double(x, -a);
    }
    else
    {
        ret = MathExp_Double(a * MathLn_Double(x));
    }
    return (ret);
}


float cal_val(float arr[])
{
    int n =1;
    int i = 0;
    int sum = 0;
    for (; i<n; i++)
        sum += arr[i];
    return (float)sum / n;
}
```
# 2. Công thức xác định vị trí

Đề bài cho xi,yi là tọa độ của 3 vị trí 1, 2, 3. Gọi xU,yU là tọa độ vị trí cần xác định

![dl1](https://khaidox.github.io/assets/img/msp430/2.png)
![dl1](https://khaidox.github.io/assets/img/msp430/3.png)