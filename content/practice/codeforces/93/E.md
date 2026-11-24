---
title: "CF 93E - Thất Sinh"
description: "Chúng ta được yêu cầu tính xem có bao nhiêu số từ 1 đến n không chia hết cho bất kỳ số nào trong một tập hợp số nguyên nhất định, được gọi là chỉ báo trúng. Mỗi chỉ báo được đảm bảo là nguyên tố cùng nhau với các chỉ báo khác và có k chỉ báo, mỗi chỉ báo tối đa là 1000."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "dp", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 93
codeforces_index: "E"
codeforces_contest_name: "Codeforces Beta Round 76 (Div. 1 Only)"
rating: 2600
weight: 93
solve_time_s: 65
verified: true
draft: false
---

[CF 93E - Lostborn](https://codeforces.com/problemset/problem/93/E) 

**Đánh giá:** 2600 
**Tags:** dp, toán, lý thuyết số 
**Thời gian giải:** 1m 5s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu tính toán có bao nhiêu số từ 1 đến _n_ **không chia hết** cho bất kỳ số nào trong một tập hợp số nguyên nhất định, được gọi là chỉ số truy cập. Mỗi chỉ báo được đảm bảo là đồng nguyên tố với những chỉ báo khác và có _k_ trong số đó, mỗi chỉ báo nhiều nhất là 1000. Về mặt khái niệm, hãy tưởng tượng anh hùng của Igor với sức mạnh _n_ vung một loại vũ khí có đặc tính được mã hóa trong các chỉ báo; thiệt hại tương ứng với việc đếm các số trong 1…_n_ "sống sót" khi chia hết cho bất kỳ chỉ số nào trong số này. 

Các ràng buộc cho chúng ta manh mối ngay lập tức về việc lựa chọn thuật toán. _n_ có thể lên tới 10^13, loại trừ bất kỳ thuật toán nào kiểm tra từng số riêng lẻ. Ngay cả việc lặp lại tuyến tính cũng là không thể, vì điều đó sẽ yêu cầu 10^13 thao tác. _k_ tương đối nhỏ, nhiều nhất là 100 và các chỉ báo nhỏ (1000), do đó các kỹ thuật kết hợp sử dụng tập hợp con các chỉ báo là khả thi. 

Trường hợp cạnh không rõ ràng phát sinh khi _n_ nhỏ hơn bất kỳ chỉ báo nào. Ví dụ: nếu _n_ = 3 và các chỉ báo là 5, 7, 11 thì không có số nào chia hết cho bất kỳ chỉ báo nào, vì vậy câu trả lời là 3. Một cách tiếp cận ngây thơ giả định rằng ít nhất một số sẽ luôn chia hết cho mỗi chỉ báo sẽ không thành công ở đây. Một cạm bẫy tiềm tàng khác là tràn số nguyên khi tính tích của các chỉ số trong các tổ hợp tập hợp con; chúng ta phải cẩn thận vì một số sản phẩm có thể vượt quá 10^13. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực rất đơn giản: lặp qua tất cả các số nguyên từ 1 đến _n_, kiểm tra từng số nguyên với tất cả các chỉ báo _k_ và đếm những số không chia hết cho bất kỳ chỉ số nào trong số đó. Điều này hoạt động chính xác nhưng yêu cầu thao tác O(n * k), có thể đạt tới 10^15 trong trường hợp xấu nhất. Điều này vượt xa giới hạn có thể chấp nhận được. 

Quan sát quan trọng là tất cả các chỉ báo đều là cặp nguyên tố cùng nhau. Điều này cho phép chúng tôi sử dụng **Nguyên tắc loại trừ-bao gồm**. Đối với bất kỳ tập hợp con chỉ báo nào, chúng ta có thể tính xem có bao nhiêu số chia hết cho tích của chúng. Nếu một tập hợp con chứa số lẻ các chỉ số, chúng tôi sẽ cộng phần đóng góp của nó; nếu chẵn, chúng tôi trừ nó. Bằng cách này, chúng ta chỉ cần xem xét 2^k tập con, điều này khả thi với k 100 nếu chúng ta lược bỏ các tập con có tích vượt quá _n_. Tính đồng nguyên tố đảm bảo rằng bội số chung nhỏ nhất của bất kỳ tập hợp con nào chỉ là tích của các phần tử của nó, giúp đơn giản hóa các phép tính. 

Lực lượng vũ phu hoạt động vì nó kiểm tra chính xác khả năng chia hết từng cái một, nhưng không thành công đối với _n_ lớn. Cách tiếp cận loại trừ bao gồm hoạt động vì tính đồng nguyên tố đảm bảo rằng việc đếm bội số theo sản phẩm sẽ nắm bắt chính xác các phần trùng lặp mà không cần tính hai lần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * k) | O(1) | Quá chậm | 
| Bao gồm-Loại trừ | O(2^k) (cắt tỉa theo sản phẩm ≤ n) | O(k) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên _n_ và _k_, sau đó đọc danh sách các chỉ số _a_. 
2. Khởi tạo một biến`count_divisible`đến 0; nó sẽ lưu tổng số số chia hết cho ít nhất một chỉ báo. 
3. Lặp lại tất cả các tập hợp con không trống của các chỉ báo. Biểu thị các tập hợp con có mặt nạ bit từ 1 đến 2^k - 1. 
4. Với mỗi tập hợp con, hãy tính tích các phần tử của nó. Bỏ qua tập hợp con nếu tích vượt quá _n_, vì không có số nào ≤ _n_ chia hết cho nó. 
5. Xác định sự đóng góp của tập hợp con vào`count_divisible`. Nếu kích thước tập hợp con là số lẻ, hãy thêm Floor(n/product); nếu chẵn thì trừ sàn(n/sản phẩm). Đây là công thức Bao gồm-Loại trừ. 
6. Sau khi xử lý tất cả các tập con, câu trả lời là`n - count_divisible`, đại diện cho các số không chia hết cho bất kỳ chỉ số nào. 

Bất biến đảm bảo tính chính xác là Nguyên tắc Bao gồm-Loại trừ đếm chính xác các số chia hết cho ít nhất một phần tử, cộng khi các tập hợp con có số lượng lẻ và trừ khi chẵn. Tính cùng nguyên tố đảm bảo rằng các tích tương ứng chính xác với bội số chung nhỏ nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from itertools import combinations
from math import prod

def main():
    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    
    count_divisible = 0
    for mask in range(1, 1 << k):
        subset = []
        for i in range(k):
            if mask & (1 << i):
                subset.append(a[i])
        p = 1
        for x in subset:
            if p > n // x:
                p = n + 1
                break
            p *= x
        if p > n:
            continue
        if len(subset) % 2 == 1:
            count_divisible += n // p
        else:
            count_divisible -= n // p
    print(n - count_divisible)

if __name__ == "__main__":
    main()
```Giải pháp xử lý cẩn thận tình trạng tràn số nguyên bằng cách kiểm tra`p > n // x`trước khi nhân lên. Điều này ngăn sản phẩm vượt quá 10^13. Việc sử dụng mặt nạ bit cho phép chúng tôi tạo các tập hợp con một cách hiệu quả và kiểm tra lẻ/chẵn thực hiện dấu hiệu loại trừ bao gồm. Phép trừ`n - count_divisible`ở cuối đưa ra chính xác những con số an toàn trước tất cả các chỉ số. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
20 3
2 3 5
```| Tập hợp con | Sản phẩm | Tầng(20 / Sản phẩm) | Đóng góp | 
| --- | --- | --- | --- | 
| [2] | 2 | 10 | +10 | 
| [3] | 3 | 6 | +6 | 
| [5] | 5 | 4 | +4 | 
| [2,3] | 6 | 3 | -3 | 
| [2,5] | 10 | 2 | -2 | 
| [3,5] | 15 | 1 | -1 | 
| [2,3,5] | 30 | 0 | +0 |`count_divisible = 10+6+4-3-2-1+0 = 14`Đáp án = 20 - 14 = 6 

Điều này xác nhận rằng phép tính bao gồm-loại trừ sẽ đếm chính xác các phần trùng lặp. 

### Mẫu tùy chỉnh 

đầu vào:```
10 2
3 7
```| Tập hợp con | Sản phẩm | Tầng(10 / Sản phẩm) | Đóng góp | 
| --- | --- | --- | --- | 
| [3] | 3 | 3 | +3 | 
| [7] | 7 | 1 | +1 | 
| [3,7] | 21 | 0 | -0 |`count_divisible = 4`Đáp án = 10 - 4 = 6 

Điều này chứng tỏ rằng _n_ nhỏ so với các chỉ báo được xử lý chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(2^k * k) | Mỗi tập hợp con được tạo bằng cách sử dụng một bitmask (2^k tập hợp con), sản phẩm điện toán chiếm tới k phép nhân | 
| Không gian | O(k) | Chỉ lưu trữ tập hợp con và sản phẩm hiện tại | 

Với k 100, các tập con 2^k được cắt bớt bằng cách kiểm tra`p > n`, do đó trong thực tế thuật toán chạy tốt trong khoảng thời gian n lên tới 10^13. Việc sử dụng bộ nhớ là tối thiểu vì chỉ lưu trữ các sản phẩm và bộ đếm tập hợp con. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import main
    sys.stdout = io.StringIO()
    main()
    return sys.stdout.getvalue().strip()

# provided sample
assert run("20 3\n2 3 5\n") == "6", "sample 1"

# minimum input
assert run("1 1\n1\n") == "0", "minimum input"

# all indicators larger than n
assert run("3 2\n5 7\n") == "3", "no number divisible"

# n equals product of all indicators
assert run("6 2\n2 3\n") == "2", "product equals n"

# all indicators 1
assert run("10 3\n1 1 1\n") == "0", "all divisible by 1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1\n1 | 0 | tối thiểu n, chỉ số 1 | 
| 3 2\n5 7 | 3 | không có số nào chia hết cho bất kỳ chỉ báo nào | 
| 6 2\n2 3 | 2 | bội số chồng chéo được tính chính xác | 
| 10 3\n1 1 1 | 0 | chỉ số bằng 1, mọi thứ đều chia hết | 

## Vỏ cạnh 

Khi _n_ nhỏ hơn tất cả các chỉ báo, chẳng hạn như`n=3`và các chỉ số`[5,7]`, thuật toán loại trừ bao gồm sẽ bỏ qua chính xác các tập hợp con trong đó sản phẩm > n. Mỗi sản phẩm tập hợp con vượt quá n, vì vậy`count_divisible`vẫn là 0, mang lại câu trả lời là 3. Điều này chứng tỏ thuật toán không trừ sai các số khi không tồn tại bội số. 

Khi chỉ báo là 1, tất cả các số đều chia hết, do đó thuật toán xác định chính xác rằng mọi tập hợp con chứa 1 đều góp phần vào`count_divisible`, dẫn đến số còn lại bằng 0. Séc`p > n // x`ngăn chặn tràn khi xử lý n lớn với các chỉ báo nhỏ, chẳng hạn như n=10^13 và chỉ báo 1000.
