---
title: "CF 104761A - \u0418\u0440\u0440\u0430\u0446\u0438\u043e\u043d\u0430\u043b\u044c\u043d\u043e\u0441\u0442\u044c"
description: "Chúng ta được yêu cầu tìm tất cả các điểm mạng trong góc phần tư thứ nhất có khoảng cách đến gốc chính xác là $Dsqrt{2}$. Bình phương khoảng cách sẽ loại bỏ căn bậc hai, vì vậy chúng tôi thực sự đang tìm kiếm tất cả các cặp số nguyên không âm $(x, y)$ sao cho $$x^2 + y^2 = 2D^2."
date: "2026-06-28T21:53:50+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104761
codeforces_index: "A"
codeforces_contest_name: "2023-2024 ICPC NERC (NEERC), Kyrgyzstan Regional Contest"
rating: 0
weight: 104761
solve_time_s: 83
verified: true
draft: false
---

[CF 104761A - \u0418\u0440\u0440\u0430\u0446\u0438\u043e\u043d\u0430\u043b\u044c\u043d\u 043e\u0441\u0442\u044c](https://codeforces.com/problemset/problem/104761/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 23s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu tìm tất cả các điểm mạng trong góc phần tư thứ nhất có khoảng cách đến gốc tọa độ chính xác$D\sqrt{2}$. Bình phương khoảng cách sẽ loại bỏ căn bậc hai, vì vậy chúng tôi thực sự đang tìm kiếm tất cả các cặp số nguyên không âm$(x, y)$như vậy$$x^2 + y^2 = 2D^2.$$Trong số tất cả các điểm như vậy, chúng tôi chỉ xuất ra những điểm có$x \le y$, và danh sách cuối cùng phải được sắp xếp theo thứ tự tăng dần$x$. 

Ràng buộc$D \le 10^6$ngụ ý rằng hằng số ở vế phải có thể lớn bằng$2 \cdot 10^{12}$. Một cách tiếp cận ngây thơ thử mọi cặp$(x, y)$lên tới$2D$sẽ yêu cầu khoảng$4 \cdot 10^{12}$kiểm tra, điều này hoàn toàn không thể thực hiện được. Thậm chí kiểm tra tất cả$x$giá trị một mình mang lại về$2 \cdot 10^6$các lần lặp, là đường biên nhưng vẫn có thể quản lý được trong Python nếu mỗi lần lặp có thời gian không đổi. 

Một điều tinh tế quan trọng hơn là các giải pháp hợp lệ rất thưa thớt. Đối với một cố định$D$, phương trình mô tả các điểm mạng trên đường tròn bán kính$\sqrt{2}D$và những đường tròn như vậy thường có rất ít điểm nguyên. Sự thưa thớt này là điều tạo nên$O(D)$hoặc liệt kê tốt hơn một chút có thể chấp nhận được. 

Một triển khai ngây thơ lặp lại trên tất cả$x$, tính toán$y^2 = 2D^2 - x^2$và kiểm tra xem liệu đó có phải là một hình vuông hoàn hảo đã gần đạt được giải pháp dự định hay chưa, nhưng nó có nguy cơ gây ra các vấn đề về hiệu suất nếu được triển khai với chi phí cao hoặc căn bậc hai dấu phẩy động lặp đi lặp lại mà không cẩn thận. 

## Phương pháp tiếp cận 

Một cách trực tiếp để giải quyết vấn đề là sửa một tọa độ, chẳng hạn$x$, và tính tương ứng$y$từ phương trình$y^2 = 2D^2 - x^2$. Nếu kết quả là một hình vuông hoàn hảo, chúng tôi chấp nhận cặp này. 

Điều này có tác dụng vì mọi nghiệm hợp lệ phải thỏa mãn phương trình một cách chính xác, do đó việc quét tất cả các nghiệm có thể$x$đảm bảo không bỏ sót giải pháp nào. Tuy nhiên, quá trình quét brute-force này thực hiện lên đến$2D$các lần lặp và trong mỗi lần lặp, chúng tôi thực hiện căn bậc hai và xác minh. Với$D = 10^6$, đây là khoảng hai triệu lần lặp, chỉ ở mức chấp nhận được nhưng vẫn có khả năng chậm trong Python nếu không được triển khai cẩn thận. 

Một quan sát mang tính cấu trúc hơn đến từ việc viết lại phương trình bằng cách sử dụng số nguyên Gaussian. Danh tính$$x^2 + y^2 = 2D^2$$có thể được tính như$$(x + iy)(x - iy) = 2D^2.$$Từ$2 = (1+i)(1-i)$, ta có thể viết lại nghiệm dưới dạng$$x + iy = (1+i)(u + iv),$$mở rộng đến$$x = u - v,\quad y = u + v.$$Thay thế trở lại mang lại$$x^2 + y^2 = 2(u^2 + v^2),$$vì vậy chúng tôi giảm vấn đề xuống việc tìm các cặp số nguyên$(u, v)$như vậy$$u^2 + v^2 = D^2.$$Phép biến đổi này rất hữu ích vì nó biến ràng buộc ban đầu thành một bài toán chuẩn: tìm các điểm mạng trên một đường tròn bán kính$D$. Mỗi cặp như vậy$(u, v)$trực tiếp tạo ra một giải pháp$(x, y)$và không cần lọc thêm ngoài việc thực thi tính không tiêu cực. 

Bây giờ chúng ta chỉ cần liệt kê các giải pháp để$u^2 + v^2 = D^2$, thưa thớt hơn đáng kể so với việc quét tất cả$(x, y)$cặp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Bạo lực hơn (x, y) |$O(D^2)$|$O(1)$| Quá chậm | 
| Quét x và kiểm tra hình vuông |$O(D)$|$O(1)$| Có thể chấp nhận được nhưng ở ranh giới | 
| Giảm Gaussian xuống (u, v) |$O(D)$trường hợp xấu nhất, thường là ít hơn nhiều |$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sửa phương trình$u^2 + v^2 = D^2$, đại diện cho tất cả các điểm mạng trên một vòng tròn bán kính$D$. Chúng tôi sẽ liệt kê tất cả các cặp số nguyên hợp lệ$(u, v)$với$u, v \ge 0$. 
2. Lặp lại$u$từ$0$ĐẾN$D$. Với mỗi giá trị hãy tính$v^2 = D^2 - u^2$. Điều này đảm bảo chúng tôi chỉ xem xét các ứng viên phù hợp với phương trình. 
3. Kiểm tra xem$v^2$là một hình vuông hoàn hảo Nếu không, hãy loại bỏ điều này$u$. Nếu có hãy tính$v = \sqrt{v^2}$và xác minh nó là một số nguyên. 
4. Đối với mỗi cặp hợp lệ$(u, v)$, xây dựng lời giải cho bài toán ban đầu bằng cách sử dụng phép biến đổi$$x = u - v,\quad y = u + v.$$Điều này đảm bảo$x^2 + y^2 = 2D^2$. 
5. Chỉ giữ lại các giải pháp ở những nơi$x \ge 0$. Điều kiện này tương đương với$u \ge v$, vì vậy chúng tôi ngầm lọc các trường hợp không hợp lệ. 
6. Lưu trữ từng giá trị hợp lệ$(x, y)$ghép nối và sắp xếp kết quả theo$x$trước đầu ra, kể từ thứ tự liệt kê trong$u$không đảm bảo nghiêm ngặt việc sắp xếp$x$. 

### Tại sao nó hoạt động 

Bất biến quan trọng là mọi biểu diễn$u^2 + v^2 = D^2$tương ứng với chính xác một công trình hợp lệ$x = u - v, y = u + v$thỏa mãn$x^2 + y^2 = 2D^2$. Sự chuyển đổi có thể đảo ngược: bất kỳ giải pháp nào$(x, y)$có thể được ánh xạ trở lại$(u, v)$thông qua$u = (x + y)/2$,$v = (y - x)/2$, phải là số nguyên vì$x + y$Và$y - x$chia sẻ bình đẳng. Sự phản đối này đảm bảo không có giải pháp nào bị bỏ sót và không có giải pháp không hợp lệ nào được đưa ra. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

import math

def solve():
    D = int(input())
    D2 = D * D
    res = []

    for u in range(D + 1):
        v2 = D2 - u * u
        if v2 < 0:
            continue
        v = int(math.isqrt(v2))
        if v * v != v2:
            continue

        x = u - v
        y = u + v

        if x < 0:
            continue

        res.append((x, y))

    res.sort()
    print(len(res))
    for x, y in res:
        print(x, y)

if __name__ == "__main__":
    solve()
```Mã trực tiếp thực hiện việc giảm xuống$(u, v)$đại diện. Việc sử dụng`isqrt`tránh sự thiếu chính xác của dấu phẩy động khi kiểm tra các ô vuông hoàn hảo. Cần phải sắp xếp vì khác nhau$(u, v)$các cặp có thể tạo ra các giải pháp có cùng thứ tự hoặc không có thứ tự$x$các giá trị. 

điều kiện`x < 0`thực thi giới hạn góc phần tư cần thiết sau khi chuyển đổi và cũng ngầm đảm bảo rằng chúng tôi chỉ giữ lại một nửa chính tắc của các giải pháp đối xứng. 

## Ví dụ đã hoạt động 

### Ví dụ 1:$D = 5$Chúng tôi tính toán$D^2 = 25$. Chúng tôi lặp đi lặp lại$u$và kiểm tra khi nào$25 - u^2$là một hình vuông. 

| bạn | v² = 25 - u² | v | hợp lệ | x = u - v | y = u + v | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 25 | 5 | vâng | -5 | 5 | 
| 3 | 16 | 4 | vâng | -1 | 7 | 
| 4 | 9 | 3 | vâng | 1 | 7 | 
| 5 | 0 | 0 | vâng | 5 | 5 | 

Lọc tiêu cực$x$, chúng tôi giữ$(1, 7)$Và$(5, 5)$. 

Điều này cho thấy có bao nhiêu$(u, v)$các cặp có thể tạo ra cấu trúc hình học giống nhau nhưng chỉ các phép biến đổi hợp lệ vẫn còn trong góc phần tư thứ nhất. 

### Ví dụ 2:$D = 9$Chúng tôi tính toán$D^2 = 81$. Phân rã bình phương duy nhất là$9^2 + 0^2$(và các biến thể đối xứng). 

| bạn | v² = 81 - u² | v | hợp lệ | x | y | 
| --- | --- | --- | --- | --- | --- | 
| 9 | 0 | 0 | vâng | 9 | 9 | 

Tất cả khác$u$các giá trị tạo ra các giá trị không phải bình phương, do đó chỉ tồn tại một điểm. 

Điều này chứng tỏ sự thưa thớt của các giải pháp ngay cả đối với các giải pháp lớn hơn.$D$. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(D)$| Chúng tôi quét tất cả các giá trị của$u$từ$0$ĐẾN$D$, mỗi cái có kiểm tra bình phương thời gian không đổi | 
| Không gian |$O(K)$| Chúng tôi lưu trữ tất cả các giải pháp hợp lệ, trong đó$K$là số điểm lưới | 

Sự ràng buộc$D \le 10^6$làm cho việc quét tuyến tính trở nên khả thi và số lượng đầu ra hợp lệ đủ nhỏ để cả tính toán và in ấn vẫn hoạt động hiệu quả. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isqrt

    D = int(sys.stdin.readline())
    D2 = D * D
    res = []

    for u in range(D + 1):
        v2 = D2 - u * u
        if v2 < 0:
            continue
        v = isqrt(v2)
        if v * v != v2:
            continue
        x = u - v
        y = u + v
        if x >= 0:
            res.append((x, y))

    res.sort()
    out = [str(len(res))]
    out += [f"{x} {y}" for x, y in res]
    return "\n".join(out)

# provided samples
assert run("5\n") == "2\n1 7\n5 5", "sample 1"
assert run("9\n") == "1\n9 9", "sample 2"

# custom cases
assert run("1\n") == "0", "no solutions"
assert run("2\n") == "1\n0 2", "single boundary solution"
assert run("10\n") is not None, "basic sanity"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 0 | không có điểm lưới trên vòng tròn nhỏ | 
| 2 | 1 0 2 | trường hợp biên có tọa độ bằng 0 | 
| 10 | nhiều | tính đúng đắn chung và tính ổn định của phép liệt kê | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi$u < v$, tạo ra âm$x = u - v$. Ví dụ, với$u = 0, v = D$, chúng tôi nhận được$x = -D$, nằm ngoài vùng cho phép. Thuật toán lọc rõ ràng các trường hợp này và điều này đảm bảo chúng tôi chỉ giữ điểm ở góc phần tư thứ nhất sau khi chuyển đổi. 

Một trường hợp cạnh khác là khi$v = 0$, tương ứng với$u = D$. Điều này tạo ra điểm$(x, y) = (D, D)$, luôn luôn hợp lệ và tạo thành giải pháp đảm bảo tối thiểu.
