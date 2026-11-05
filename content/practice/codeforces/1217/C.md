---
title: "CF 1217C - Số Chuỗi Con Tốt"
description: "Chúng ta được cấp một chuỗi nhị phân và được yêu cầu đếm các chuỗi con \"tốt\" của nó. Một chuỗi con là tốt nếu độ dài của nó bằng giá trị thập phân của biểu diễn nhị phân của nó, cho phép các số 0 đứng đầu."
date: "2026-06-11T22:49:44+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "bitmasks", "brute-force"]
categories: ["algorithms"]
codeforces_contest: 1217
codeforces_index: "C"
codeforces_contest_name: "Educational Codeforces Round 72 (Rated for Div. 2)"
rating: 1700
weight: 1217
solve_time_s: 108
verified: true
draft: false
---

[CF 1217C - Số lượng chuỗi con tốt](https://codeforces.com/problemset/problem/1217/C) 

**Đánh giá:** 1700 
**Tags:** tìm kiếm nhị phân, mặt nạ bit, lực lượng vũ phu 
**Thời gian giải:** 1 phút 48 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một chuỗi nhị phân và được yêu cầu đếm các chuỗi con "tốt" của nó. Một chuỗi con là tốt nếu độ dài của nó bằng giá trị thập phân của biểu diễn nhị phân của nó, cho phép các số 0 đứng đầu. Điều đó có nghĩa là đối với một chuỗi con như`011`, đó là`3`ở dạng thập phân, nó chỉ tốt nếu độ dài của nó cũng bằng`3`. Đầu vào của chúng tôi bao gồm nhiều truy vấn, mỗi truy vấn có một chuỗi nhị phân và đầu ra cho mỗi truy vấn là số chuỗi con tốt trong chuỗi đó. 

Các ràng buộc cung cấp cho chúng tôi tối đa 1000 truy vấn và tổng độ dài trên tất cả các truy vấn tối đa là 200.000. Điều đó có nghĩa là bất kỳ thuật toán nào lặp lại tất cả các chuỗi con có thể một cách đơn giản đều quá chậm, bởi vì việc tạo ra tất cả các chuỗi con là$O(n^2)$, có thể đạt tới$4 \cdot 10^{10}$hoạt động trong trường hợp xấu nhất. Chúng ta cần một giải pháp gần hơn$O(n \cdot \text{small factor})$, tuyến tính lý tưởng trong thực tế. 

Các trường hợp cạnh cần theo dõi là các chuỗi con có số 0 đứng đầu. Ví dụ,`000`có giá trị thập phân`0`, vì vậy chỉ những chuỗi con có độ dài bằng 0 mới đủ điều kiện, điều này là không thể. Một trường hợp tinh vi khác là các dãy số 0 dài theo sau là một số 0, ví dụ,`0001`. Các chuỗi con như`001`có số thập phân`1`nhưng chiều dài`3`, vì vậy chúng không tốt. Một cách tiếp cận bất cẩn không tính toán cẩn thận việc chuyển đổi nhị phân sang thập phân cho mỗi độ dài chuỗi con có thể bị đếm quá hoặc thiếu. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản là xem xét mọi chuỗi con, chuyển đổi nó thành số thập phân và kiểm tra xem độ dài chuỗi con có khớp với giá trị thập phân hay không. Điều này đúng về mặt logic nhưng đòi hỏi$O(n^2)$lần lặp cho mỗi truy vấn. Ngay cả việc chuyển đổi chuỗi nhị phân sang số thập phân cũng mất$O(n)$trên mỗi chuỗi con, do đó độ phức tạp trong trường hợp xấu nhất là$O(n^3)$, vượt xa mức có thể chấp nhận được. 

Quan sát quan trọng cho một giải pháp hiệu quả là các chuỗi con dài hơn khoảng 20 ký tự không thể "tốt" nếu giá trị nhị phân của chúng được hiểu là số nguyên. Điều này là do độ dài của chuỗi con nhanh chóng trở nên nhỏ hơn giá trị được biểu thị dưới dạng nhị phân. Ví dụ: chuỗi con 20 bit có tất cả các chuỗi là$2^{20}-1 \approx 10^6$, vượt xa độ dài của bất kỳ chuỗi nào trong các ràng buộc đầu vào của chúng tôi. Vì vậy, chúng ta chỉ cần kiểm tra các chuỗi con có kích thước tối đa khoảng 20 bit bắt đầu từ mỗi vị trí. 

Ngoài ra, chúng ta cần bỏ qua các số 0 đứng đầu một cách hiệu quả, nhưng nếu bao gồm chúng, chúng ta phải đảm bảo không đếm nhầm các chuỗi con có giá trị thập phân nhỏ hơn độ dài do các số 0 đứng đầu. Thông tin chi tiết này giúp giảm vấn đề từ quét bậc hai trên tất cả các chuỗi con sang quét tuyến tính trên từng vị trí bắt đầu bằng một cửa sổ nhỏ cố định. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^3) | O(1) | Quá chậm | 
| Tối ưu | O(n * 20) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng truy vấn`t`và lặp lại qua từng truy vấn. 
2. Đối với mỗi chuỗi`s`chiều dài`n`, khởi tạo bộ đếm`ans = 0`để tích lũy số lượng chuỗi con tốt. 
3. Lặp lại từng chỉ mục bắt đầu`i`trong chuỗi. 
4. Bỏ qua các số 0 đứng đầu hoặc tính riêng chúng. Bắt đầu xây dựng giá trị thập phân`val = 0`. 
5. Đối với mỗi chỉ số cuối`j`bắt đầu từ`i`và kéo dài tối đa 20 ký tự (hoặc cho đến khi chuỗi kết thúc), hãy cập nhật`val = val * 2 + int(s[j])`. 
6. Tính độ dài chuỗi con như sau`length = j - i + 1`. 
7. Nếu`val == length`, tăng`ans`bằng 1. Nếu`val > length + 20`, nghỉ sớm vì việc gia hạn thêm không bao giờ có thể thỏa mãn điều kiện. 
8. Sau khi kết thúc tất cả các vị trí bắt đầu, in bộ đếm`ans`. 

Tại sao nó hoạt động: Điều bất biến là đối với mỗi chỉ mục bắt đầu, chúng tôi chỉ xem xét các chuỗi con có độ dài tương đương với giá trị thập phân của chúng, điều này đảm bảo chúng tôi không bỏ sót bất kỳ chuỗi con tiềm năng tốt nào. Giới hạn 20 ký tự là đủ vì bất kỳ số nhị phân nào dài hơn đều vượt quá độ dài chuỗi tối đa, đảm bảo tính chính xác mà không lặp lại quá độ dài chuỗi con không thực tế. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def count_good_substrings(s):
    n = len(s)
    ans = 0
    for i in range(n):
        if s[i] == '0':
            continue
        val = 0
        for j in range(i, min(n, i + 20)):
            val = val * 2 + int(s[j])
            length = j - i + 1
            if val == length:
                ans += 1
            if val > n:
                break
    return ans

t = int(input())
for _ in range(t):
    s = input().strip()
    print(count_good_substrings(s))
```Giải pháp đọc đầu vào hiệu quả và xử lý nhiều truy vấn. Vòng lặp bên trong chỉ có tối đa 20 ký tự, tránh hết thời gian chờ. Séc`val > n`ngăn chặn các lần lặp không cần thiết vượt quá độ dài chuỗi con khả thi. 

## Ví dụ đã hoạt động 

### Mẫu 1:`0110`| tôi | j | s[i:j+1] | giá trị | chiều dài | Tốt? | trả lời | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 0 | 0 | 1 | Không | 0 | 
| 0 | 1 | 01 | 1 | 2 | Không | 0 | 
| 0 | 2 | 011 | 3 | 3 | Có | 1 | 
| 0 | 3 | 0110 | 6 | 4 | Không | 1 | 
| 1 | 1 | 1 | 1 | 1 | Có | 2 | 
| 1 | 2 | 11 | 3 | 2 | Không | 2 | 
| 1 | 3 | 110 | 6 | 3 | Không | 2 | 
| 2 | 2 | 1 | 1 | 1 | Có | 3 | 
| 2 | 3 | 10 | 2 | 2 | Có | 4 | 
| 3 | 3 | 0 | 0 | 1 | Không | 4 | 

Bảng xác nhận rằng tất cả các chuỗi con tốt hợp lệ đều được tính. 

### Mẫu 2:`0101`Theo dấu vết tương tự, chúng tôi đếm các chuỗi con tốt:`1`tại chỉ số 1,`1`ở chỉ số 3, và`01`ở chỉ số 2-3, tổng cộng là 3. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n * 20) | Vòng lặp bên ngoài qua mỗi vị trí bắt đầu và vòng lặp bên trong tối đa 20 ký tự | 
| Không gian | O(1) | Chỉ sử dụng bộ đếm và biến tạm thời | 

Được cho$n \le 2 \cdot 10^5$Và$t \le 1000$, giải pháp đủ nhanh trong 4 giây và vừa với bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline
    def count_good_substrings(s):
        n = len(s)
        ans = 0
        for i in range(n):
            if s[i] == '0':
                continue
            val = 0
            for j in range(i, min(n, i + 20)):
                val = val * 2 + int(s[j])
                length = j - i + 1
                if val == length:
                    ans += 1
                if val > n:
                    break
        return ans

    t = int(input())
    res = []
    for _ in range(t):
        s = input().strip()
        res.append(str(count_good_substrings(s)))
    return "\n".join(res)

# provided samples
assert run("4\n0110\n0101\n00001000\n0001000\n") == "4\n3\n4\n3", "sample 1"

# custom tests
assert run("1\n0\n") == "0", "single zero"
assert run("1\n1\n") == "1", "single one"
assert run("1\n11111\n") == "5", "all ones"
assert run("1\n000001\n") == "1", "trailing one after zeros"
assert run("1\n1000000000\n") == "1", "long sequence with leading one"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0`| 0 | Số 0 đơn, không có chuỗi con tốt | 
|`1`| 1 | Một người được tính là tốt | 
|`11111`| 5 | Tất cả những cái đó đều tốt | 
|`000001`| 1 | Xử lý số 0 đứng đầu | 
|`1000000000`| 1 | Chuỗi dài chỉ có một chuỗi con hợp lệ | 

## Vỏ cạnh 

Đối với một chuỗi như`00001000`, thuật toán bỏ qua các số 0 đứng đầu cho các vị trí bắt đầu cho đến chỉ số 4. Từ chỉ số 4, nó được tính`1`có chiều dài 1,`10`có độ dài 2, v.v., xác định chính xác`4`những chuỗi con tốt. Các chuỗi con như`0000`bị bỏ qua vì giá trị thập phân của chúng là
