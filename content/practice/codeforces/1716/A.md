---
title: "CF 1716A - 2-3 nước đi"
description: "Chúng ta đang đứng ở vị trí 0 trên trục số và muốn đạt đến tọa độ đích n. Trong một phút, chúng ta có thể di chuyển 2 hoặc 3 đơn vị theo một trong hai hướng. Nhiệm vụ là tính toán số phút tối thiểu cần thiết để đạt được n một cách chính xác."
date: "2026-06-09T19:51:20+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1716
codeforces_index: "A"
codeforces_contest_name: "Educational Codeforces Round 133 (Rated for Div. 2)"
rating: 800
weight: 1716
solve_time_s: 130
verified: true
draft: false
---

[CF 1716A - 2-3 bước di chuyển](https://codeforces.com/problemset/problem/1716/A) 

**Đánh giá:** 800 
**Tags:** tham lam, toán học 
**Thời gian giải:** 2 phút 10 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta đang đứng ở vị trí số 0 trên trục số và muốn đạt tới tọa độ đích`n`. Trong một phút, chúng ta có thể di chuyển 2 hoặc 3 đơn vị theo một trong hai hướng. Nhiệm vụ là tính số phút tối thiểu cần thiết để đạt được`n`chính xác. Đầu vào bao gồm`t`trường hợp thử nghiệm, mỗi trường hợp đưa ra một số nguyên duy nhất`n`. Đối với mỗi`n`, chúng ta phải in số bước di chuyển cần thiết ít nhất. 

Những hạn chế chính là`n`có thể lớn như$10^9$và có tới$10^4$trường hợp thử nghiệm. Điều này loại trừ bất kỳ giải pháp nào lặp lại các vị trí hoặc thử từng chuỗi di chuyển một cách rõ ràng vì điều đó sẽ quá chậm. Thay vào đó, chúng ta cần một cách tiếp cận hoạt động theo thời gian không đổi hoặc logarit cho mỗi trường hợp thử nghiệm. 

Một trường hợp khó nhận thấy là khi`n`nhỏ, đặc biệt là`1`. Vì chúng ta chỉ có thể di chuyển theo gia số 2 hoặc 3 nên chúng ta không thể đạt tới 1 trong một nước đi. Số lần di chuyển tối thiểu khi đó sẽ là 2: đi 2 tiến rồi 1 lùi hoặc 3 tiến rồi 2 lùi, kết quả chính xác là 1. Tương tự, tất cả các số không chia hết cho 2 hoặc 3 phải được xử lý cẩn thận. Một sự ngây thơ`n // 3`hoặc`n // 2`tính toán sẽ thất bại đối với những phần bù nhỏ này. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là mô phỏng mọi sự kết hợp của các bước di chuyển cho đến khi chúng ta đạt được`n`. Điều này hoạt động bằng cách thử tất cả các chuỗi 2 và 3 cho đến khi tổng bằng`n`. Điều này đúng vì cuối cùng, bất kỳ số nguyên nào cũng có thể đạt được bằng cách cộng một tổ hợp nào đó của 2 và 3. Tuy nhiên, độ phức tạp của nó là theo cấp số nhân. Nếu như`n`là$10^9$, việc này sẽ mất quá nhiều thời gian. 

Cái nhìn sâu sắc quan trọng đến từ số học mô-đun. Mọi số đều có thể được biểu thị dưới dạng kết hợp của 2 và 3 vì ước số chung lớn nhất của 2 và 3 là 1. Điều đó đảm bảo khả năng tiếp cận. Để giảm thiểu số lần di chuyển, trước tiên chúng tôi ưu tiên nước đi lớn nhất có thể, 3, sau đó điền vào phần còn lại bằng 2 giây. 

Cụ thể, đối với một`n`, chúng ta có thể ước tính`n // 3`di chuyển 3. Nếu phần còn lại là 0, chúng ta đã hoàn thành. Nếu số dư là 1, chúng ta cần thay một số 3 bằng hai số 2, làm tăng tổng số nước đi lên 1. Nếu số dư là 2, chúng ta chỉ cần thêm một nước đi 2 nữa. Logic này hoạt động với tất cả các số nguyên dương, nhỏ hay lớn và chạy trong thời gian không đổi cho mỗi trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Tối ưu | O(1) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên`n`. Mục đích là tính toán số lần di chuyển tối thiểu để đạt được`n`từ 0 bằng cách sử dụng các bước di chuyển có kích thước 2 hoặc 3. 
2. Tính toán`q`BẰNG`n // 3`Và`r`BẰNG`n % 3`. Đây`q`đại diện cho số lần di chuyển 3 đơn vị tối đa và`r`là khoảng cách còn lại mà chúng ta vẫn cần phải đi qua. 
3. Nếu`r`là 0, câu trả lời đơn giản là`q`bởi vì di chuyển 3 đơn vị bao gồm chính xác`n`. 
4. Nếu`r`là 1, chúng ta không thể che nó bằng một nước đi 2 đơn vị, vì vậy chúng ta giảm một nước đi 3 đơn vị để tạo khoảng trống, sau đó sử dụng hai nước đi 2 đơn vị để che khoảng cách còn lại. Điều này làm tăng số lần di chuyển lên 1. 
5. Nếu`r`là 2, chúng ta có thể che nó bằng một nước đi 2 đơn vị bổ sung, do đó tổng số nước đi là`q + 1`. 

Tại sao nó hoạt động: Mọi số nguyên đều có thể truy cập được bằng cách sử dụng kết hợp nào đó của 2 và 3. Bằng cách tối đa hóa nước đi 3 đơn vị trước tiên, chúng tôi giảm thiểu tổng số nước đi. Các điều chỉnh dựa trên phần còn lại đảm bảo khoảng cách còn lại được bao phủ một cách tối ưu. Công thức đảm bảo rằng chúng ta không bao giờ đánh giá thấp số lần di chuyển vì bất kỳ số nước đi nào còn sót lại sẽ được sửa bằng cách thêm 2 hoặc thay thế số 3 bằng hai số 2. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    q, r = divmod(n, 3)
    if r == 0:
        print(q)
    elif r == 1:
        if q >= 1:
            print(q + 1)  # replace one 3 with two 2s
        else:
            print(2)      # n=1 or n=1 scenario
    else:  # r == 2
        print(q + 1)
```các`divmod`chia tách chức năng`n`thành thương số và số dư một cách hiệu quả. Chúng tôi kiểm tra phần còn lại và điều chỉnh các bước di chuyển cho phù hợp. Một sự tinh tế là xử lý rất nhỏ`n`các giá trị (như 1) trong đó không có đủ nước đi 3 đơn vị để hoán đổi, vì vậy chúng tôi trả về 2 nước đi theo cách thủ công trong kịch bản đó. 

## Ví dụ đã hoạt động 

### Ví dụ 1: n = 1 

| n | q | r | di chuyển được tính toán | 
| --- | --- | --- | --- | 
| 1 | 0 | 1 | 2 | 

Giải thích: Chúng ta không thể đạt tới 1 chỉ bằng một nước đi, vì vậy chúng ta cần hai nước đi: +2 rồi -1. Thuật toán xử lý việc này thông qua`q < 1`chi nhánh. 

### Ví dụ 2: n = 12 

| n | q | r | di chuyển được tính toán | 
| --- | --- | --- | --- | 
| 12 | 4 | 0 | 4 | 

Giải thích: 12 có thể được thực hiện bằng 4 nước đi cỡ 3. Không có phần dư nên không cần điều chỉnh thêm. 

### Ví dụ 3: n = 4 

| n | q | r | di chuyển được tính toán | 
| --- | --- | --- | --- | 
| 4 | 1 | 1 | 2 | 

Giải thích: Một nước đi 3 được 3 đơn vị. Số 1 còn lại được xử lý bằng cách thay thế số 3 bằng hai số 2. Tổng số lượt di chuyển: 2. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) cho mỗi trường hợp thử nghiệm | Tất cả các phép toán đều là số học và có điều kiện. | 
| Không gian | O(1) | Chỉ có một vài số nguyên được lưu trữ. | 

Từ bỏ$10^4$trường hợp thử nghiệm, điều này chạy trong$10^4$hoạt động nhiều nhất, cũng nằm trong giới hạn thời gian 1 giây. Việc sử dụng bộ nhớ là tối thiểu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    
    t = int(input())
    for _ in range(t):
        n = int(input())
        q, r = divmod(n, 3)
        if r == 0:
            print(q)
        elif r == 1:
            if q >= 1:
                print(q + 1)
            else:
                print(2)
        else:
            print(q + 1)
    
    sys.stdout = sys.__stdout__
    return output.getvalue().strip()

# Provided samples
assert run("4\n1\n3\n4\n12\n") == "2\n1\n2\n4", "Sample 1"

# Custom cases
assert run("3\n2\n5\n7\n") == "1\n2\n3", "Small numbers"
assert run("2\n10\n1000000000\n") == "4\n333333334", "Large numbers"
assert run("1\n1\n") == "2", "Minimum input"
assert run("1\n2\n") == "1", "Minimum 2 reachable in one move"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 5 7 | 1 2 3 | Số nhỏ có dư | 
| 10 1000000000 | 4 333333334 | Tính đúng đắn cho số rất lớn | 
| 1 | 2 | Trường hợp cạnh đầu vào tối thiểu | 
| 2 | 1 | Đầu vào tối thiểu có thể truy cập được trong một lần di chuyển | 

## Vỏ cạnh 

cho`n = 1`, thuật toán chọn 2 nước đi, xử lý chính xác tình trạng không thể đạt 1 chỉ bằng một nước đi. Vì`n = 2`, nó trực tiếp trả về 1, cho thấy các số nhỏ được ánh xạ chính xác tới các bước di chuyển tối thiểu. Vì`n = 4`, nó thay thế một nước đi 3 đơn vị bằng hai nước đi 2 đơn vị, tạo ra số nước đi tối thiểu. các`divmod`Cách tiếp cận với việc điều chỉnh phần dư đảm bảo tính chính xác trong tất cả các trường hợp biên lên đến$10^9$. 

Bài xã luận này trình bày một cách cụ thể, nhanh chóng và đáng tin cậy để tính số lần di chuyển tối thiểu cho bất kỳ số nguyên dương nào`n`chỉ sử dụng số học, tránh bất kỳ tìm kiếm vũ phu nào.
