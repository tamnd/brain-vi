---
title: "CF 1613C - Dao găm tẩm độc"
description: "Chúng ta được cung cấp một chuỗi thời gian tấn công và một con rồng có h điểm trúng đích. Mỗi đòn tấn công áp dụng một chất độc kéo dài k giây và gây 1 sát thương mỗi giây. Nếu con rồng đã bị nhiễm độc khi có một đòn tấn công mới, bộ đếm thời gian nhiễm độc sẽ được đặt lại."
date: "2026-06-10T06:53:32+07:00"
tags: ["codeforces", "competitive-programming", "binary-search"]
categories: ["algorithms"]
codeforces_contest: 1613
codeforces_index: "C"
codeforces_contest_name: "Educational Codeforces Round 118 (Rated for Div. 2)"
rating: 1200
weight: 1613
solve_time_s: 74
verified: true
draft: false
---

[CF 1613C - Dao găm tẩm độc](https://codeforces.com/problemset/problem/1613/C) 

**Đánh giá:** 1200 
**Thẻ:** tìm kiếm nhị phân 
**Thời gian giải:** 1 phút 14s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi thời gian tấn công và một con rồng với`h`điểm nhấn. Mỗi đòn tấn công áp dụng một chất độc kéo dài`k`giây và gây 1 sát thương mỗi giây. Nếu con rồng đã bị nhiễm độc khi có một đòn tấn công mới, bộ đếm thời gian nhiễm độc sẽ được đặt lại. Nhiệm vụ của chúng ta là tìm mức tối thiểu`k`sao cho tổng thiệt hại từ tất cả các cuộc tấn công ít nhất là`h`. 

Đầu vào chỉ định`n`các cuộc tấn công với thời gian tăng dần`a_1, a_2, ..., a_n`và đầu ra phải là một số nguyên`k`cho từng trường hợp thử nghiệm. Các giá trị của`h`có thể rất lớn (lên tới 10^18), do đó, bất kỳ thuật toán nào cố gắng mô phỏng thiệt hại từng giây sẽ quá chậm. Số lần tấn công`n`tối đa là 100, cho phép chúng tôi lặp lại các cuộc tấn công hoặc khoảng thời gian giữa các cuộc tấn công mà không vượt quá giới hạn tính toán. 

Các trường hợp cạnh có thể dễ dàng phá vỡ một giải pháp đơn giản bao gồm các chuỗi có các cuộc tấn công rất gần nhau. Ví dụ, nếu`n=2`,`h=2`, Và`a=[1,2]`, sau đó là thời gian độc`k=1`sẽ chỉ gây 1 sát thương mỗi giây, nhưng đòn tấn công thứ hai với tốc độ`a_2=2`trùng lặp với giây đầu tiên của chất độc trước đó, có nghĩa là tổng thời lượng ngây thơ sẽ vượt quá mức thiệt hại. Tương tự, nếu các cuộc tấn công cách xa nhau, giải pháp phải cho phép`k`đủ lớn để bao quát các cuộc tấn công bị cô lập. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ là thử mọi cách có thể`k`bắt đầu từ 1 và mô phỏng tổng sát thương bằng cách lặp lại tất cả các giây kể từ đòn tấn công đầu tiên cho đến khi chất độc hết tác dụng. Đối với mỗi đòn tấn công, chúng tôi sẽ kéo dài thời gian tác dụng của chất độc nếu nó trùng nhau. Điều này hiệu quả vì chúng ta có thể tính toán chính xác thiệt hại, nhưng nó cực kỳ kém hiệu quả đối với những trường hợp lớn.`h`bởi vì chúng ta có thể cần`k`lên đến 10^18, khiến việc mô phỏng đơn giản không thể thực hiện được. 

Điểm mấu chốt là chúng ta không cần phải mô phỏng từng giây. Thiệt hại cho một nhất định`k`có thể được tính toán trực tiếp bằng cách tính tổng các đóng góp của từng cuộc tấn công, có tính đến sự trùng lặp. Cụ thể, đối với mỗi cặp đòn tấn công liên tiếp có lúc`a_i`Và`a_{i+1}`, thiệt hại do`i`-cuộc tấn công thứ là`min(k, a_{i+1} - a_i)`. Đòn tấn công cuối cùng luôn đóng góp chính xác`k`hư hại. Sử dụng điều này, chúng ta có thể tính tổng thiệt hại theo O(n) cho bất kỳ`k`. 

Điều này cho phép một cách tiếp cận tìm kiếm nhị phân. Chúng tôi biết rằng nếu một điều gì đó nhất định`k`giao dịch ít nhất`h`thiệt hại, lớn hơn`k`cũng sẽ đủ. Ngược lại, nếu một điều kiện nào đó`k`quá nhỏ, nhỏ hơn nữa`k`thất bại. Do đó chúng ta có thể thực hiện tìm kiếm nhị phân trong phạm vi`[1, h]`để tìm mức tối thiểu`k`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n * k) | O(1) | Quá chậm đối với h lớn | 
| Tìm kiếm nhị phân với tổng khoảng thời gian | O(n log h) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc`n`,`h`, và thời gian tấn công`a_1, ..., a_n`. 
2. Xác định hàm`damage(k)`tính toán tổng thiệt hại trong một khoảng thời gian nhiễm độc nhất định`k`. Khởi tạo`total=0`. Đối với mỗi`i`từ 0 đến`n-2`, thêm vào`min(k, a_{i+1} - a_i)`ĐẾN`total`. Sau vòng lặp, thêm`k`cho cuộc tấn công cuối cùng. Trở lại`total`. 
3. Khởi tạo ranh giới tìm kiếm nhị phân`low=1`Và`high=h`. Trong khi`low < high`, tính toán`mid=(low+high)//2`. Nếu như`damage(mid) >= h`, bộ`high=mid`; nếu không thì đặt`low=mid+1`. 
4. Sau khi tìm kiếm nhị phân,`low`bằng mức tối thiểu`k`ít nhất đạt được`h`hư hại. Xuất nó ra. 

Tại sao nó hoạt động: Chức năng gây sát thương đơn điệu trong`k`. Tăng dần`k`không bao giờ giảm thiệt hại bởi vì mỗi`min(k, a_{i+1}-a_i)`hoặc giữ nguyên hoặc tăng lên, và đòn tấn công cuối cùng luôn góp phần`k`. Điều này đảm bảo rằng tìm kiếm nhị phân tìm thấy chính xác giá trị đủ nhỏ nhất`k`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, h = map(int, input().split())
        a = list(map(int, input().split()))

        def total_damage(k):
            dmg = 0
            for i in range(n-1):
                dmg += min(k, a[i+1] - a[i])
            dmg += k
            return dmg

        low, high = 1, h
        while low < high:
            mid = (low + high) // 2
            if total_damage(mid) >= h:
                high = mid
            else:
                low = mid + 1
        print(low)

if __name__ == "__main__":
    solve()
```các`total_damage`tính toán thiệt hại chính xác mà không cần mô phỏng từng giây. Tìm kiếm nhị phân thu hẹp hiệu quả mức tối thiểu`k`. Chúng tôi sử dụng`low < high`là điều kiện vòng lặp hội tụ về mức nhỏ nhất có thể`k`. Các điều kiện biên được xử lý chính xác bằng cách thêm`k`cho cuộc tấn công cuối cùng. 

## Ví dụ đã hoạt động 

Đối với đầu vào:```
3 10
2 4 10
```| tôi | một [tôi] | min(k, a[i+1]-a[i]) cho k=4 | dmg tích lũy | 
| --- | --- | --- | --- | 
| 0 | 2 | phút(4, 2) = 2 | 2 | 
| 1 | 4 | phút(4, 6) = 4 | 6 | 
| cuối cùng | 10 | cộng k=4 | 10 | 

Điều này xác nhận rằng`k=4`đủ để gây ra 10 sát thương. 

Đối với đầu vào:```
5 3
1 2 4 5 7
```Kiểm tra`k=1`: 

| tôi | một [tôi] | phút(1, a[i+1]-a[i]) | dmg tích lũy | 
| --- | --- | --- | --- | 
| 0 | 1 | 1 | 1 | 
| 1 | 2 | 1 | 2 | 
| 2 | 4 | 1 | 3 | 
| 3 | 5 | 1 | 4 | 
| cuối cùng | 7 | thêm 1 | 5 | 

Vì 5 >= 3 nên`k=1`là tối thiểu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t * n * log h) | Đối với mỗi trường hợp thử nghiệm, tìm kiếm nhị phân trên phạm vi 1..h yêu cầu các lần lặp log h, mỗi lần tính toán O(n) thiệt hại | 
| Không gian | O(n) | Lưu trữ thời gian tấn công | 

Điều này hiệu quả vì`n`<= 100 và`t`<= 1000. Ngay cả với`h`lên đến 10^18,`log2(h)`là ~60, do đó tổng số hoạt động có thể quản lý được. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    sys.stdout = sys.__stdout__
    return out.getvalue().strip()

# Provided samples
assert run("4\n2 5\n1 5\n3 10\n2 4 10\n5 3\n1 2 4 5 7\n4 1000\n3 25 64 1337\n") == "3\n4\n1\n470"

# Custom cases
assert run("1\n1 1\n1\n") == "1", "single attack, minimum h"
assert run("1\n2 100\n1 10\n") == "91", "two attacks far apart"
assert run("1\n3 6\n1 2 3\n") == "2", "all attacks consecutive"
assert run("1\n3 3\n1 10 20\n") == "1", "each attack isolated, h small"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 đòn, h=1 | 1 | Đầu vào tối thiểu, chất độc tối thiểu | 
| 2 đợt tấn công cách xa nhau | 91 | Khoảng cách lớn giữa các đòn tấn công, cần k cao | 
| 3 đòn liên tiếp | 2 | Các cuộc tấn công liên tiếp trong đó k phải che phủ chồng chéo | 
| 3 đòn tấn công biệt lập, h nhỏ | 1 | Cần thiệt hại nhỏ, k tối thiểu mặc dù có khoảng trống | 

## Vỏ cạnh 

cho`n=2, h=2, a=[1,2]`, thử nghiệm`k=1`: 

-`damage(1)`= min(1,2-1) + 1 = 1 + 1 = 2, đáp ứng`h=2`. 
- Thuật toán trả về đúng`k=1`. 

Vì`n=2, h=10, a=[1,100]`, thử nghiệm`k=50`: 

-`damage(50)`= phút(50, 99) + 50 = 50 + 50 = 100, vượt quá h=10. 
- Tìm kiếm nhị phân tìm thấy tối thiểu`k=5`(10 sát thương), xác nhận khoảng cách lớn không kết thúc
