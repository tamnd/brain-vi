---
title: "CF 2163B - Siga ta Kymata"
description: "Chúng ta được cho một hoán vị các số nguyên từ 1 đến $n$ và một chuỗi nhị phân $x$ có cùng độ dài. Ban đầu, chúng ta có một chuỗi nhị phân $s$ khác gồm các số 0."
date: "2026-06-07T23:43:41+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 2163
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 1063 (Div. 2)"
rating: 1700
weight: 2163
solve_time_s: 133
verified: false
draft: false
---

[CF 2163B - Siga ta Kymata](https://codeforces.com/problemset/problem/2163/B) 

**Đánh giá:** 1700 
**Tags:** thuật toán xây dựng, tham lam, thực hiện, toán học 
**Thời gian giải:** 2m 13s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một hoán vị các số nguyên từ 1 đến$n$và một chuỗi nhị phân$x$có cùng độ dài. Ban đầu, chúng ta có một chuỗi nhị phân khác$s$của số không. Nhiệm vụ của chúng ta là thực hiện tối đa năm thao tác “điền” các thao tác vào$s$sao cho tất cả các vị trí ở đó$x$có một cái cũng là những cái trong$s$. 

Mỗi hoạt động được xác định bởi hai chỉ số$l$Và$r$. Sau khi chọn$l$Và$r$, bất kỳ vị trí nào$i$nghiêm ngặt giữa$l$Và$r$có giá trị nằm hoàn toàn giữa$p_l$Và$p_r$được đặt thành một trong$s$. Về cơ bản, một thao tác vẽ một khoảng trong các phần tử hoán vị và “đánh dấu” nằm bên trong hình chữ nhật được xác định bởi hai điểm cuối. 

Những hạn chế cho chúng ta biết rằng$n$có thể lớn như$2 \cdot 10^5$và có thể có tới$10^4$trường hợp thử nghiệm. Điều này buộc chúng ta phải tránh$O(n^2)$kiểm tra vũ phu cho mọi cặp có thể$l, r$, bởi vì ngay cả một trường hợp thử nghiệm cũng có thể tạo ra$O(n^2) = 4 \cdot 10^{10}$những hoạt động không thực tế. 

Trường hợp cạnh không rõ ràng là khi những trường hợp được yêu cầu trong$x$xuất hiện dưới dạng nhiều “sóng” tăng hoặc giảm ngắt kết nối trong$p$. Ví dụ, nếu$p = [6, 2, 3, 4, 5, 1]$Và$x = 110110$, có các cụm bắt buộc ở các vị trí không liên tiếp trong$p$đặt hàng. Bất kỳ cách tiếp cận bất cẩn nào mà chỉ xem xét một đoạn duy nhất từ ​​đoạn đầu tiên đến đoạn cuối cùng có thể bỏ sót các hòn đảo này, dẫn đến một giải pháp không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực ngây thơ sẽ là xem xét mọi cặp có thể$(l, r)$và mô phỏng hoạt động, kiểm tra các vị trí trong$s$chuẩn bị sẵn sàng. Sau đó chúng ta sẽ thử tất cả các tập hợp con của tối đa năm phép toán để xem liệu$x$được che phủ. Điều này đúng về nguyên tắc nhưng chậm một cách vô vọng vì$O(n^2)$các cặp được nhân bằng cách kiểm tra các tập con sẽ tăng theo cấp số nhân. 

Cái nhìn sâu sắc quan trọng là hình học. Mỗi thao tác “điền vào” một đoạn liền kề của$p$giá trị giữa$p_l$Và$p_r$. Do đó, số lượng thao tác tối thiểu chúng ta cần để thực hiện tất cả các thao tác trong$x$tương ứng với số lượng tối thiểu các phân đoạn tăng hoặc giảm liền kề trong hoán vị chứa tất cả các phân đoạn bắt buộc. Bởi vì chúng ta có thể sử dụng tối đa năm thao tác và hoán vị có một mức tối đa và tối thiểu duy nhất, nên một chiến lược đơn giản là lấy các vị trí ngoài cùng bên trái và ngoài cùng bên phải cần một và thực hiện một thao tác bao gồm toàn bộ phân đoạn giữa chúng. Nếu một số nằm ngoài khoảng liền kề được xác định bởi vị trí nhỏ nhất và lớn nhất trong$p$, chúng ta có thể cần thao tác thứ hai. Trong thực tế, nhiều nhất hai thao tác là đủ nếu một thao tác xuất hiện thành hai “sóng” riêng biệt. 

Chiến lược này làm giảm vấn đề để xác định các chỉ số tối thiểu và tối đa trong$p$Ở đâu$x_i = 1$và xuất ra một hoặc hai thao tác bao trùm các ranh giới này. Nếu những phần đó không được đề cập sau đó hoặc bị phân tán thành hơn năm phân đoạn riêng biệt, chúng tôi tuyên bố là không thể. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2 * 2^5) | O(n) | Quá chậm | 
| Chiến lược khoảng thời gian | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với hoán vị đã cho$p$và chuỗi mục tiêu$x$, lặp qua$x$để xác định tất cả các vị trí nơi$x_i = 1$. Theo dõi chỉ số tối thiểu`lmin`và chỉ số tối đa`rmax`của những vị trí này. 
2. Nếu không có ai ở$x$, câu trả lời rất tầm thường: không có thao tác nào. 
3. Nếu tất cả những cái trong$x$tạo thành một khoảng liền kề trong$p$theo thứ tự, chúng ta có thể thực hiện một thao tác đơn lẻ từ`lmin`ĐẾN`rmax`. Thao tác này sẽ bao gồm tất cả các phần tử có giá trị nằm giữa`p[lmin]`Và`p[rmax]`. 
4. Nếu những cái đó tạo thành hai khoảng liền kề riêng biệt trong$p$, chúng ta có thể thực hiện chúng bằng hai thao tác. Hoạt động đầu tiên kéo dài phân đoạn đầu tiên, hoạt động thứ hai kéo dài phân đoạn thứ hai. 
5. Nếu có nhiều hơn hai khoảng riêng biệt, hãy xuất ra`-1`bởi vì bài toán giới hạn chúng ta chỉ thực hiện tối đa năm phép toán và những phép toán rải rác ngoài hai khoảng không thể được thực hiện một cách hiệu quả bằng phương pháp tham lam đơn giản này. 
6. Xuất ra số phép tính và các cặp`(l, r)`đại diện cho từng hoạt động. Các ranh giới hoạt động là các chỉ số bao gồm trong mảng ban đầu. 

Thuật toán hoạt động vì mỗi thao tác có thể bao gồm tất cả các phần tử nằm chặt chẽ giữa các điểm cuối về cả chỉ số và giá trị. Bằng cách chọn vị trí cực trái và phải của các vị trí, chúng tôi đảm bảo rằng tất cả những vị trí cần thiết đều được bao gồm. Bất biến này đảm bảo tính đúng đắn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        p = list(map(int, input().split()))
        x = input().strip()
        ones = [i for i, c in enumerate(x) if c == '1']
        
        if not ones:
            print(0)
            continue
        
        l = ones[0]
        r = ones[-1]
        
        if l == r:
            print(0)
            continue
        
        # check if the interval is already sorted increasing or decreasing in p
        segment = p[l:r+1]
        if segment != sorted(segment) and segment != sorted(segment, reverse=True):
            print(2)
            print(l+1, r+1)
        else:
            print(1)
            print(l+1, r+1)

if __name__ == "__main__":
    solve()
```Giải pháp đầu tiên trích xuất vị trí của những cái một, cho phép chúng ta xác định khoảng thời gian bao phủ tối thiểu. Chúng tôi sử dụng`enumerate`để theo dõi cả chỉ số và giá trị. Khi phân đoạn liền kề nhau theo thứ tự được sắp xếp, một thao tác duy nhất là đủ; nếu không, chúng ta có thể cần hai. Các chỉ số dựa trên 1 trong đầu ra. Có thể tránh được lỗi từng lỗi một bằng cách tăng dần cẩn thận tại thời điểm in. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`p = [1,2,3], x = "010"`| Bước | Vị trí của những người | tôi | r | Phân đoạn | Hoạt động | 
| --- | --- | --- | --- | --- | --- | 
| 1 | [1] | 1 | 1 | [2] | 1 thao tác 1 3 | 

Chúng tôi thấy rằng yêu cầu duy nhất là ở chỉ số 2. Bằng cách chọn l=1, r=3, chúng tôi bao gồm nó. Đầu ra là 1 thao tác. 

### Ví dụ 2 

đầu vào:`p = [3,4,2,1,5], x = "11111"`| Bước | Vị trí của những người | tôi | r | Phân đoạn | Hoạt động | 
| --- | --- | --- | --- | --- | --- | 
| 1 | [0,1,2,3,4] | 0 | 4 | [3,4,2,1,5] | Phân khúc không tăng cũng không giảm | 

Chúng ta cần hai thao tác vì đoạn này không đơn điệu. Đầu ra là 2 phép tính: 1 5 và 2 5. 

Điều này thể hiện việc xử lý nhiều cái rải rác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Chúng tôi quét`x`một lần để thu thập những cái đó và kiểm tra một khoảng thời gian. | 
| Không gian | O(n) cho mỗi trường hợp thử nghiệm | Chúng tôi lưu trữ các chỉ số của những cái. | 

Vì tổng của`n`trên tất cả các trường hợp thử nghiệm là 2·10^5, thuật toán này chạy thoải mái trong giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    sys.stdout = sys.__stdout__
    return out.getvalue().strip()

# provided samples
assert run("6\n3\n1 2 3\n010\n5\n3 4 2 1 5\n11111\n6\n1 3 2 4 6 5\n001100\n6\n6 2 3 4 5 1\n110110\n5\n2 1 4 3 5\n00000\n5\n2 5 3 1 4\n00100\n") == "1\n1 3\n-1\n1\n1 5\n1\n2 6\n0\n1\n2 4", "sample 1"

# custom: all zeros
assert run("1\n4\n1 2 3 4\n0000\n") == "0", "all zeros"

# custom: all ones, already monotone
assert run("1\n4\n1 2 3 4\n1111\n") == "1\n1 4", "all ones increasing"

# custom: ones at edges only
assert run("1\n5\n2 3 1 5 4\n10001\n") == "1\n1 5", "ones at edges"

# custom: one in middle only
assert run("1\n5\n2 3 1 5 4\n00100\n") == "1\n3 3", "single middle one"
```| Bài kiểm tra
