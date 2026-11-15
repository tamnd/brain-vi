---
title: "CF 1868C - Kế hoạch du lịch"
description: "Sự không phù hợp không còn đến từ tính toán MEX nữa. Logic phân đoạn MEX trong phiên bản trước thực sự phù hợp với thang đo vấn đề này."
date: "2026-06-08T23:35:40+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "dp", "implementation", "math", "trees"]
categories: ["algorithms"]
codeforces_contest: 1868
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 896 (Div. 1)"
rating: 2400
weight: 1868
solve_time_s: 206
verified: false
draft: false
---

[CF 1868C - Kế hoạch du lịch](https://codeforces.com/problemset/problem/1868/C) 

**Đánh giá:** 2400 
**Tags:** tổ hợp, dp, triển khai, toán học, cây 
**Thời gian giải:** 3 phút 26s 
**Đã xác minh:** không 

##Giải pháp 
###Chẩn đoán 

Sự không phù hợp không còn đến từ tính toán MEX nữa. Logic phân đoạn MEX trong phiên bản trước thực sự phù hợp với thang đo vấn đề này. Vấn đề thực sự mang tính cấu trúc hơn: trạng thái DP đang được sử dụng không chính xác như một “sự tích lũy toàn cầu của các XOR có thể truy cập trên mỗi phân đoạn”, thay vì mô hình hóa cấu trúc tổ hợp thực tế của việc chọn các mảng con không chồng chéo. 

Lỗi xuất hiện theo hai cách: 

Đầu tiên,`dp`đang được cập nhật tại chỗ trong khi lặp qua các phân đoạn. Điều đó âm thầm cho phép sử dụng lại một phân đoạn nhiều lần trong cùng một lớp logic, điều này không hợp lệ. Một khi bạn làm`dp |= new_dp`bên trong giống nhau`(l, r)`cấu trúc lặp lại, bạn cho phép các quyết định xâu chuỗi tương ứng với các lựa chọn phân đoạn chồng chéo một cách hiệu quả. 

Thứ hai, bản thân mô hình đang tính quá nhiều lần chuyển đổi: mỗi khi bạn nhìn thấy một phân đoạn`[l, r]`, bạn đang XOR nó với _tất cả các trạng thái XOR có thể truy cập trước đó ở cùng cấp độ toàn cầu_. Nhưng trên thực tế, việc chọn một đoạn sẽ khắc phục được ranh giới và vấn đề còn lại nằm ở hậu tố. Không có DP nhận biết hậu tố (tức là phụ thuộc vào`r+1`), điều này trở thành một kết thúc bắc cầu trên các phân đoạn, làm tăng giá trị XOR có thể truy cập và tạo ra các câu trả lời như`3`Và`7`trong trường hợp cấu trúc nên hạn chế sự kết hợp. 

Vì vậy, cách khắc phục chính xác là khôi phục cấu trúc bị thiếu: DP phải dựa trên hậu tố, không dựa trên tập hợp chung. 

Chúng tôi xác định:`dp[i]`= tập hợp các XOR có thể đạt được bằng cách sử dụng các phân đoạn hợp lệ không chồng chéo hoàn toàn bên trong hậu tố bắt đầu từ chỉ mục`i`. 

Chuyển tiếp: 

1. Bỏ qua`i`: lấy`dp[i+1]`2. Đi đoạn`[i, j]`: XOR mex(i,j) với`dp[j+1]`Điều này khôi phục tính chính xác vì nó thực thi tính không chồng chéo. 

### Cách tiếp cận đúng 

Chúng tôi liệt kê tất cả các mảng con, tính toán MEX tăng dần và chuyển từ`i`ĐẾN`j+1`. DP được thực hiện từ phải sang trái. 

Đây là hạn chế còn thiếu để khắc phục lỗi đếm quá mức. 

### Giải pháp Python đúng```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        # dp[i] = set of XORs achievable from suffix i
        dp = [set() for _ in range(n + 1)]
        dp[n].add(0)

        for i in range(n - 1, -1, -1):
            # option 1: skip index i
            dp[i] = set(dp[i + 1])

            freq = [0] * (n + 2)
            mex = 0

            for j in range(i, n):
                v = a[j]
                if v <= n:
                    freq[v] += 1

                while freq[mex]:
                    mex += 1

                # take segment [i, j]
                for x in dp[j + 1]:
                    dp[i].add(x ^ mex)

        print(max(dp[0]))

if __name__ == "__main__":
    solve()
```### Tại sao điều này lại khắc phục được lỗi 

Trước đây, DP đã hợp nhất các khoản đóng góp phân khúc trên toàn cầu, cho phép "sắp xếp lại" các phân khúc không hợp lệ và tính toán hiệu quả các lựa chọn chồng chéo hoặc không nhất quán hậu tố. 

Bây giờ mỗi lần chuyển đổi sử dụng một tiền tố một cách rõ ràng`[i, j]`và tiếp tục từ`j+1`, thực thi ràng buộc không chồng lấp chính xác như bài toán yêu cầu. 

Điều này khôi phục cấu trúc tổ hợp chính xác: mọi trạng thái XOR tương ứng với một phân vùng hợp lệ thành các phân đoạn rời rạc theo thứ tự từ trái sang phải.
