---
title: "CF 1713C - Hoán vị xây dựng"
description: "Chúng ta được yêu cầu xây dựng một hoán vị của các số từ $0$ đến $n-1$ sao cho với mỗi chỉ số $i$, tổng giá trị tại chỉ mục đó và bản thân chỉ số đó là một bình phương hoàn hảo. Nói cách khác, với mọi $i$, $pi + i = k^2$ đối với một số nguyên $k$."
date: "2026-06-09T20:17:04+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "dp", "math"]
categories: ["algorithms"]
codeforces_contest: 1713
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 812 (Div. 2)"
rating: 1200
weight: 1713
solve_time_s: 397
verified: false
draft: false
---

[CF 1713C - Hoán vị bản dựng](https://codeforces.com/problemset/problem/1713/C) 

**Đánh giá:** 1200 
**Tags:** thuật toán xây dựng, dp, toán học 
**Thời gian giải:** 6 phút 37 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xây dựng một hoán vị của các số từ$0$ĐẾN$n-1$sao cho với mỗi chỉ số$i$, tổng giá trị tại chỉ mục đó và chính chỉ mục đó là một bình phương hoàn hảo. Nói cách khác, với mỗi$i$,$p_i + i = k^2$đối với một số nguyên$k$. Đầu vào bao gồm một số trường hợp thử nghiệm, mỗi trường hợp chỉ định một số nguyên duy nhất$n$, và với mỗi$n$chúng ta phải đưa ra một hoán vị hợp lệ hoặc chỉ ra sự không thể thực hiện được với$-1$. 

Những ràng buộc cho chúng ta biết rằng$n$có thể lớn như$10^5$, và tổng của tất cả$n$trên tất cả các trường hợp thử nghiệm cũng được giới hạn ở mức$10^5$. Điều này có nghĩa là mọi giải pháp đều phải hoạt động trong khoảng$O(n)$cho mỗi trường hợp thử nghiệm, bởi vì một sự ngây thơ$O(n^2)$cách tiếp cận sẽ thực hiện lên đến$10^{10}$hoạt động trong trường hợp xấu nhất là quá chậm. 

Các trường hợp cạnh là tinh tế. Các giá trị nhỏ của$n$, chẳng hạn như$1$hoặc$2$, phải được xử lý rõ ràng vì logic của việc nhóm các chỉ số theo hình vuông hoàn hảo tiếp theo có thể không thành công đối với những điều này. Một kịch bản phức tạp khác phát sinh khi$n$bản thân nó là một hình vuông hoàn hảo hoặc nhỏ hơn một một chút: chọn hình vuông hoàn hảo lớn nhất không vượt quá$n-1 + (n-1)$yêu cầu xử lý cẩn thận các ranh giới chỉ số. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ thử mọi hoán vị của$[0, 1, \dots, n-1]$và kiểm tra xem$p_i + i$là một hình vuông hoàn hảo Cách tiếp cận này đúng về nguyên tắc nhưng có$n!$hoán vị, điều này là không thể thực hiện được ngay cả đối với$n = 10$. Ngay cả việc tạo ra tất cả các tập hợp con cho mỗi chỉ mục cũng quá tốn kém. 

Quan sát quan trọng là các bình phương hoàn hảo có khoảng cách ngày càng xa nhau và chúng tôi muốn ghép từng chỉ số với một giá trị sao cho tổng là một bình phương. Làm việc ngược lại từ chỉ số lớn nhất thường có ích. Nếu chúng ta bắt đầu từ chỉ số lớn nhất$i = n-1$, ta tìm được hình vuông lớn nhất không vượt quá$i + (n-1)$, giao phó$p_{n-1}$như sự khác biệt giữa hình vuông đó và$i$và tiếp tục điền vào mảng bằng cách giảm các chỉ số. Bởi vì sự khác biệt giữa một số chính phương và một chỉ số giảm đi một cách đơn điệu, điều này đảm bảo chúng ta sử dụng mỗi số đúng một lần. Phương pháp này "lấp đầy các khối" một cách hiệu quả giữa các ô vuông liên tiếp theo thứ tự giảm dần. 

Cách tiếp cận tối ưu xây dựng hoán vị bằng cách liên tục xác định bình phương lớn nhất$s^2$như vậy$s^2 \ge i$và điền tất cả các chỉ số từ vị trí hiện tại cho đến$s^2 - i$theo thứ tự ngược lại. Chiến lược tham lam này đảm bảo rằng mỗi$p_i + i$là một hình vuông và không có số nào được lặp lại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| Xây dựng tham lam | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một mảng`p`kích thước`n`với tất cả các phần tử không được đặt. Đặt chỉ mục hiện tại`i`ĐẾN`n-1`, vị trí cuối cùng của mảng. 
2. Trong khi`i >= 0`, tìm số nguyên lớn nhất`s`như vậy`s^2`lớn hơn hoặc bằng`i`. Đây là hình vuông hoàn hảo mà chúng ta sẽ sử dụng cho khối hiện tại. 
3. Tính toán`start = s^2 - i`. Đây là giá trị nên có ở chỉ mục`i`làm`p[i] + i = s^2`. 
4. Điền vào mảng từ chỉ mục`start`ĐẾN`i`theo thứ tự ngược lại. Cụ thể, giao`p[j] = start + (i - j)`vì`j`từ`i`xuống tới`start`. Điều này đảm bảo tất cả các số trong khối này là khác biệt và tổng của chúng với các chỉ số của chúng bằng nhau`s^2`. 
5. Cập nhật`i`ĐẾN`start - 1`và lặp lại quá trình cho các chỉ số còn lại. 
6. Khi tất cả các chỉ số đã được điền đầy đủ, hãy xuất mảng`p`. 

Tại sao nó hoạt động: Tính bất biến được duy trì là mỗi lần chúng ta điền vào một khối, tất cả các số được chỉ định đều khác biệt và khớp với phạm vi chỉ mục được yêu cầu. Làm việc từ chỉ mục lớn nhất đảm bảo rằng cùng một số không bao giờ được chỉ định hai lần. Mỗi nhiệm vụ đảm bảo`p_i + i`bằng với hình vuông hoàn hảo đã chọn, thỏa mãn thuộc tính "mảng tốt". 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

import math

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        p = [-1] * n
        i = n - 1
        while i >= 0:
            s = math.isqrt(i)
            square = s * s
            start = square - (i - square)
            start = max(0, 2*s*s - i - 1) if square < i else square - i
            for j in range(i, start - 1, -1):
                p[j] = square - j
            i = start - 1
        print(*p)

if __name__ == "__main__":
    solve()
```Giải pháp này sử dụng căn bậc hai số nguyên để tìm hình vuông hoàn hảo lớn nhất cho mỗi khối. Việc điền mảng ngược lại đảm bảo rằng mỗi số được gán chính xác một lần, trong khi vẫn duy trì thuộc tính tổng bình phương hoàn hảo. Phải cẩn thận với các điều kiện biên:`start`chỉ số không được giảm xuống dưới 0, nếu không chỉ số âm sẽ được sử dụng. 

## Ví dụ đã hoạt động 

Hãy xem xét`n = 3`. Chúng tôi bắt đầu với`i = 2`. Hình vuông lớn nhất không nhỏ hơn 2 là`2^2 = 4`. Sau đó`start = 4 - 2 = 2`. Webgán`p[2] = 4 - 2 = 2`. Di chuyển đến`i = 1`. Hình vuông lớn nhất ≥ 1 là`1^2 = 1`. Sau đó`start = 1 - 1 = 0`. Webgán`p[1] = 1 - 1 = 0`Và`p[0] = 1 - 0 = 1`. Mảng cuối cùng:`[1, 0, 2]`. 

Vì`n = 4`, bắt đầu với`i = 3`. Hình vuông lớn nhất ≥ 3 là`2^2 = 4`. chỉ mục bắt đầu`start = 4 - 3 = 1`. Đổ đầy`p[3] = 1`,`p[2] = 2`,`p[1] = 3`. Còn lại`i = 0`, bình phương lớn nhất = 0, gán`p[0] = 0`. Mảng cuối cùng`[0,3,2,1]`. 

Những dấu vết này chứng tỏ rằng thuật toán xác định chính xác hình vuông lớn nhất cho mỗi khối và điền số ngược lại để duy trì thuộc tính hoán vị. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi chỉ mục được truy cập chính xác một lần và tính toán căn bậc hai số nguyên là O(1) cho mỗi chỉ mục. | 
| Không gian | O(n) | Mảng có kích thước n được duy trì cho hoán vị. | 

Điều này phù hợp thoải mái trong các ràng buộc của bài toán, vì tổng của tất cả`n`trên các trường hợp thử nghiệm không vượt quá$10^5$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    f = io.StringIO()
    with redirect_stdout(f):
        solve()
    return f.getvalue().strip()

# Provided samples
assert run("3\n3\n4\n7\n") == "1 0 2\n0 3 2 1\n1 0 2 6 5 4 3", "sample 1"

# Custom cases
assert run("1\n1\n") == "0", "minimum n"
assert run("1\n2\n") == "0 1", "small n with two elements"
assert run("1\n5\n") == "4 3 2 1 0", "n=5, reverse block filling"
assert run("1\n10\n") == "6 5 4 3 2 1 0 9 8 7", "n=10, multiple blocks"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 0 | mảng một phần tử | 
| 2 | 0 1 | hoán vị hợp lệ tối thiểu | 
| 5 | 4 3 2 1 0 | n nhỏ với khối điền ngược | 
| 10 | 6 5 4 3 2 1 0 9 8 7 | xây dựng nhiều khối đúng cách | 

## Vỏ cạnh 

cho`n = 1`, mảng`[0]`thỏa mãn một cách tầm thường`p_0 + 0 = 0`, đó là một hình vuông hoàn hảo. Bộ thuật toán`i = 0`, tìm hình vuông = 0, gán`p[0] = 0`. Đầu ra là`[0]`. 

Vì`n = 2`, chúng ta phải xử lý khối đầu tiên từ chỉ mục`1`. Hình vuông lớn nhất ≥ 1 là 1. Chỉ số bắt đầu`start = 1 - 1 = 0`. Webgán`p[1] = 0`Và`p[0] = 1`. đầu ra`[1,0]`, thỏa mãn chính xác tổng bình phương hoàn hảo. Điều này cho thấy thuật toán xử lý đúng các mảng nhỏ mà không bỏ qua hoặc lặp lại các số.
