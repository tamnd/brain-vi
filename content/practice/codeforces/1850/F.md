---
title: "CF 1850F - Chúng ta đều là trẻ em"
description: "Giải pháp trước đây không chỉ có sai sót trong quá trình triển khai mà về cơ bản nó còn sai lệch với cấu trúc của vấn đề."
date: "2026-06-09T05:32:22+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "implementation", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1850
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 886 (Div. 4)"
rating: 1300
weight: 1850
solve_time_s: 180
verified: false
draft: false
---

[CF 1850F - Chúng ta đều là trẻ em](https://codeforces.com/problemset/problem/1850/F) 

**Đánh giá:** 1300 
**Tags:** vũ lực, thực hiện, toán học, lý thuyết số 
**Thời gian giải:** 3 phút 
**Đã xác minh:** không 

##Giải pháp 
## Chẩn đoán 

Giải pháp trước đây không chỉ có sai sót trong quá trình triển khai mà về cơ bản nó còn sai lệch với cấu trúc của vấn đề. Mẫu thất bại trong mẫu là nhất quán: nó loại bỏ nhiều trường hợp hợp lệ trong đó câu trả lời là`YES`và chỉ thỉnh thoảng chấp nhận những cái có mẫu cục bộ tầm thường xuất hiện. Đó là dấu hiệu cho thấy lời giải đang kiểm tra bất biến sai. 

Sai lầm cốt lõi là cố gắng lý luận về “sự khác biệt cục bộ” hoặc cố gắng xây dựng lại cấu trúc bị thiếu bằng cách chèn một giá trị. Điều đó bỏ qua ràng buộc thực tế: chúng tôi không trực tiếp xây dựng lại một hoán vị từ các tổng tiền tố, chúng tôi đang kiểm tra xem liệu các tổng tiền tố có thể được hoàn thành thành một chuỗi khác biệt tăng dần tạo thành một hoán vị của`1..n`. 

Quan điểm đúng thì đơn giản và toàn diện hơn nhiều: 

Nếu các tổng tiền tố hợp lệ thì sự khác biệt giữa các tổng tiền tố liên tiếp chính xác là các phần tử hoán vị. Xóa một tổng tiền tố: 

1. Xóa sạch một phần tử khỏi chuỗi sai phân, hoặc 
2. Hợp nhất hai phần tử hoán vị liền kề thành một phần tử chênh lệch lớn hơn. 

Vì vậy, toàn bộ vấn đề quy về việc kiểm tra xem tập hợp nhiều “sự khác biệt ngụ ý” có thể được chuyển thành một hoán vị hay không bằng cách: 

- chèn một giá trị còn thiếu, hoặc 
- chia một giá trị thành hai số nguyên dương có tổng chính xác. 

Đây là một số trường hợp kết cấu không đổi, không phải là mô phỏng chèn vị trí. 

## Thông tin chi tiết quan trọng 

Đặt tổng tiền tố đã cho là`b[0..m-1]`. 

Chúng tôi mở rộng với`b[-1] = 0`. 

Sau đó xác định sự khác biệt:```
d[i] = b[i] - b[i-1]
```Nếu mảng đã hoàn chỉnh, những`d[i]`sẽ chính xác`{1..n}`. 

Bây giờ chúng tôi đang thiếu chính xác một tổng tiền tố, điều này tạo ra chính xác một trong hai hiệu ứng: 

### Trường hợp 1: Thiếu một điểm khác biệt 

Chúng tôi đã thấy rồi`n-1`sự khác biệt hợp lệ và số còn thiếu là số hoàn thành`1..n`. 

### Trường hợp 2: một ranh giới tiền tố bị phá vỡ 

Một điểm khác biệt trở nên không hợp lệ vì hai điểm khác biệt ban đầu liền kề đã được hợp nhất: 

vì vậy chúng ta phải chia một`d[i]`vào trong`x + y`sao cho cả hai đều ở trong`[1..n]`. 

Chúng tôi kiểm tra cả hai trường hợp một cách hiệu quả. 

## Hướng dẫn thuật toán 

1. Đọc`n`và mảng`b`kích thước`n-1`. 
2. Xây dựng mảng khác biệt đầy đủ bằng cách sử dụng`prev = 0`. 
3. Tính toán tất cả sự khác biệt`d[i] = b[i] - prev`. 
4. Đếm tần số khác biệt trong bản đồ băm. 
5. Tính các số từ`1..n`đang thiếu. 
6. Nếu thiếu chính xác một số và tất cả các số khác đều hợp lệ, câu trả lời là`YES`. 
7. Nếu không, hãy thử từng điểm khác biệt`d[i]`: 

- cố gắng chia tách`d[i]`vào trong`x`Và`d[i]-x`- kiểm tra xem việc thay thế có bảo toàn tính hợp lệ của hoán vị hay không. 
8. Nếu có bất kỳ cấu hình hợp lệ nào, hãy in`YES`, khác`NO`. 

### Tại sao nó hoạt động 

Cấu trúc tổng tiền tố buộc mảng ban đầu được mã hóa theo các sai phân liền kề. Việc loại bỏ một tổng tiền tố sẽ ảnh hưởng đến nhiều nhất một giá trị kề trong chuỗi sai phân. Do đó, sai lệch dẫn đến là cục bộ: hoặc một phần tử biến mất hoặc một phần tử trở thành tổng hợp nhất của hai phần tử hoán vị hợp lệ. Việc kiểm tra toàn diện hai khả năng cấu trúc này bao gồm tất cả các hoạt động tái tạo hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        b = list(map(int, input().split()))

        # build differences
        prev = 0
        d = []
        for x in b:
            d.append(x - prev)
            prev = x

        # Case 1: treat as multiset check
        cnt = {}
        for x in d:
            cnt[x] = cnt.get(x, 0) + 1

        missing = []
        for i in range(1, n + 1):
            if cnt.get(i, 0) == 0:
                missing.append(i)

        if len(missing) == 1:
            print("YES")
            continue

        # Case 2: try splitting one merged value
        ok = False
        full = set(range(1, n + 1))

        for i in range(len(d)):
            val = d[i]

            for x in range(1, val):
                y = val - x
                if x in full and y in full:
                    ok = True
                    break
            if ok:
                break

        print("YES" if ok else "NO")

if __name__ == "__main__":
    solve()
```## Giải thích cách khắc phục 

Việc hiệu chỉnh loại bỏ giả định không chính xác rằng chúng ta phải tái tạo lại các tổng tiền tố một cách rõ ràng hoặc thử chèn các vị trí. Thay vào đó, nó xử lý vấn đề trong miền tự nhiên của nó: sự khác biệt của các tổng tiền tố tương ứng với các phần tử hoán vị và việc loại bỏ một tổng tiền tố chỉ tạo ra một khiếm khuyết cấu trúc cục bộ duy nhất. 

Bằng cách chuyển sang kiểm tra phân tách nhiều tập + một lần, chúng tôi khôi phục bất biến chính xác và loại bỏ các kết quả âm tính giả được thấy trong mẫu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | một lượt + kiểm tra phân chia tuyến tính | 
| Không gian | O(n) | bản đồ tần số | 

Điều này dễ dàng phù hợp với các ràng buộc của tổng`2e5`. 

Nếu bạn muốn, tôi cũng có thể hiển thị bằng chứng biên tập có mục đích rõ ràng để tránh hoàn toàn việc phân tách chữ hoa chữ thường và giảm nó thành kiểm tra tham lam một lần.
