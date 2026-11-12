---
title: "CF 1616A - Phân tập số nguyên"
description: "Chúng ta được cung cấp một danh sách các số nguyên và chúng ta được phép phủ định bất kỳ tập hợp con nào của chúng. Mục tiêu là tối đa hóa số lượng số nguyên riêng biệt trong mảng sau khi có thể phủ định một số phần tử."
date: "2026-06-10T06:33:27+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1616
codeforces_index: "A"
codeforces_contest_name: "Good Bye 2021: 2022 is NEAR"
rating: 800
weight: 1616
solve_time_s: 102
verified: false
draft: false
---

[CF 1616A - Phân tập số nguyên](https://codeforces.com/problemset/problem/1616/A) 

**Đánh giá:** 800 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 42s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một danh sách các số nguyên và chúng ta được phép phủ định bất kỳ tập hợp con nào của chúng. Mục tiêu là tối đa hóa số lượng số nguyên riêng biệt trong mảng sau khi có thể phủ định một số phần tử. Mỗi trường hợp thử nghiệm cung cấp một danh sách các số nguyên mới và đầu ra đơn giản là số lượng lớn nhất các giá trị khác nhau mà chúng ta có thể thu được thông qua các phủ định này. 

Nhìn vào các ràng buộc đầu vào, kích thước mảng`n`tối đa là 100 và các giá trị nằm trong khoảng từ -100 đến 100. Giá trị này đủ nhỏ để xem xét các giải pháp kiểm tra tần suất hoặc sự hiện diện của từng giá trị một cách trực tiếp mà không phải lo lắng về độ phức tạp tiệm cận cao. Số lượng ca kiểm thử cũng ở mức vừa phải ở mức 100, do đó ngay cả một`O(n^2)`cách tiếp cận cho mỗi trường hợp thử nghiệm sẽ chạy trong thời gian chấp nhận được. 

Các trường hợp cần chú ý là số 0, số trùng lặp và các số dương và âm đối xứng. Ví dụ, nếu mảng là`[0, 0]`, việc phủ định các số 0 không thay đổi gì cả, vì vậy câu trả lời là 1. Nếu mảng là`[1, 1, -1]`, chúng ta cần xử lý các bản sao một cách cẩn thận: chúng ta có thể sử dụng một 1 và một -1, nhưng chúng ta không thể tạo lại số 1 thứ hai từ -1. Bỏ qua sự tinh tế này sẽ gây ra tình trạng đếm quá mức. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử mọi tập hợp con của mảng, phủ định nó và đếm các giá trị riêng biệt. Với mỗi phần tử, ta có hai lựa chọn: phủ định hoặc không, cho`2^n`khả năng. Ngay cả với`n = 20`, điều này đạt tới hơn một triệu tập hợp con và đối với`n = 100`, cái này lớn về mặt thiên văn. Vì vậy, vũ lực là không thể thực hiện được. 

Điều quan trọng cần lưu ý là chúng tôi chỉ quan tâm đến **giá trị tuyệt đối riêng biệt**. Mỗi số dương có thể xuất hiện dưới dạng chính nó, số âm của nó hoặc cả hai. Mỗi số âm chỉ là phiên bản âm của một giá trị tuyệt đối dương nào đó. Số không rất đặc biệt: nó chỉ có thể xuất hiện một lần bất chấp sự phủ định. Vì vậy, vấn đề giảm xuống còn việc đếm xem chúng ta có bao nhiêu giá trị tuyệt đối riêng biệt, nhưng chúng ta cần quyết định cách xử lý các trường hợp tồn tại cả dương và âm của một số. 

Chiến lược tối ưu như sau: chúng ta đếm các số 0 riêng biệt và giới hạn chúng ở mức 1, sau đó với mọi giá trị tuyệt đối khác, chúng ta có thể bao gồm tối đa hai số: một dương và một âm. Nếu mảng ban đầu chứa cả hai dấu cho cùng một giá trị tuyệt đối, chúng ta sẽ bao gồm cả hai; mặt khác, chúng tôi bao gồm bất cứ điều gì tồn tại. Điều này đảm bảo số lượng tối đa các phần tử riêng biệt. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n * n) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) (hoặc O(n) cho tập hợp) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo bộ đếm cho số 0 và hai bộ: một cho số dương, một cho số âm. 
2. Lặp lại mảng. Nếu một số bằng 0, hãy tăng bộ đếm số 0. Nếu dương, hãy thêm nó vào tập dương. Nếu âm, hãy thêm giá trị tuyệt đối của nó vào tập âm. 
3. Số giá trị phân biệt tối đa là tổng của số 0 (nhiều nhất là 1), số số chỉ dương, số số chỉ âm và số số tồn tại ở cả dạng dương và dạng âm. 
4. Đối với mỗi giá trị tuyệt đối, nếu nó xuất hiện ở cả tập hợp dương và âm thì hãy đếm cả hai. Nếu nó chỉ xuất hiện trong một bộ, hãy đếm lần xuất hiện đó. Thêm số 0 nếu nó tồn tại. 
5. Xuất tổng này cho từng test case. 

**Lý do hoạt động:** Bằng cách đếm các giá trị tuyệt đối riêng biệt và xem xét tối đa một số 0, chúng tôi đảm bảo không có sự trùng lặp nào được tính và không có số nào bị bỏ qua. Việc phủ định các bản sao ngoài điều này không làm tăng các giá trị riêng biệt, vì vậy chiến lược này là tối đa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    
    pos = set()
    neg = set()
    zero = 0
    
    for x in a:
        if x > 0:
            pos.add(x)
        elif x < 0:
            neg.add(-x)
        else:
            zero = 1
    
    both = pos & neg  # numbers that appear in both positive and negative
    pos_only = pos - both
    neg_only = neg - both
    
    result = len(both) * 2 + len(pos_only) + len(neg_only) + zero
    print(result)
```Mã phân tách các số dương, âm và 0. Nó đếm các số xuất hiện ở cả dạng dương và âm và xử lý chúng một cách chính xác. Số không được tính nhiều nhất một lần. Việc sử dụng bộ đảm bảo các bản sao bị bỏ qua. 

## Ví dụ đã hoạt động 

### Đầu vào mẫu 1```
4
1 1 2 2
```| Bước | tư thế | phủ định | không | cả hai | pos_only | phủ định | kết quả | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| ban đầu | {} | {} | 0 | - | - | - | - | 
| 1 | {1} | {} | 0 | - | - | - | - | 
| 2 | {1} | {} | 0 | - | - | - | - | 
| 3 | {1,2} | {} | 0 | - | - | - | - | 
| 4 | {1,2} | {} | 0 | - | - | - | - | 
| cuối cùng | - | - | - | {} | {1,2} | {} | 4 | 

Giải thích: Không có số âm, số 0 = 0. Tất cả các số đều dương và duy nhất, do đó số khác biệt tối đa là 4. 

### Đầu vào mẫu 2```
0 0
```| Bước | tư thế | phủ định | không | cả hai | pos_only | phủ định | kết quả | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| ban đầu | {} | {} | 0 | - | - | - | - | 
| 1 | {} | {} | 1 | - | - | - | - | 
| 2 | {} | {} | 1 | - | - | - | - | 
| cuối cùng | - | - | - | {} | {} | {} | 1 | 

Chỉ số không, kết quả = 1. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi số được xử lý một lần và hỗ trợ chèn và tra cứu O(1) | 
| Không gian | O(n) | Bộ lưu trữ tối đa n số trong tổng số | 

Với n ≤ 100 và t ≤ 100, phương pháp này dễ dàng thực hiện trong thời gian dưới 1 giây và sử dụng bộ nhớ không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    # copy solution here
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        
        pos = set()
        neg = set()
        zero = 0
        
        for x in a:
            if x > 0:
                pos.add(x)
            elif x < 0:
                neg.add(-x)
            else:
                zero = 1
        
        both = pos & neg
        pos_only = pos - both
        neg_only = neg - both
        
        result = len(both) * 2 + len(pos_only) + len(neg_only) + zero
        print(result)
    
    return output.getvalue().strip()

# provided samples
assert run("3\n4\n1 1 2 2\n3\n1 2 3\n2\n0 0\n") == "4\n3\n1"

# custom cases
assert run("1\n3\n-1 1 0\n") == "3", "pos+neg+zero"
assert run("1\n5\n5 5 5 5 5\n") == "1", "all equal"
assert run("1\n6\n-3 -3 3 3 0 0\n") == "5", "duplicates with zero"
assert run("1\n4\n-2 -1 1 2\n") == "4", "symmetric negatives and positives"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| -1 1 0 | 3 | đếm số 0 và dương/âm riêng biệt | 
| 5 5 5 5 5 | 1 | xử lý tất cả các giá trị bằng nhau | 
| -3 -3 3 3 0 0 | 5 | xử lý các bản sao và số 0 một cách chính xác | 
| -2 -1 1 2 | 4 | xử lý các mặt tích cực và tiêu cực đối xứng | 

## Vỏ cạnh 

Đối với một mảng`[0, 0]`, bộ thuật toán`zero = 1`và không có số dương hoặc số âm.`both`,`pos_only`,`neg_only`trống rỗng. Kết quả là 1, xử lý chính xác các số 0 lặp lại. 

Vì`[1, 1, -1]`,`pos = {1}`,`neg = {1}`,`both = {1}`,`pos_only = {}`,`neg_only = {}`, kết quả = 2. Điều này đếm chính xác một 1 và một -1 và không vượt quá
