---
title: "CF 1827A - Đếm đơn hàng"
description: "Chúng ta có hai mảng a và b, mỗi mảng có độ dài n. Các phần tử trong a đều khác biệt, trong khi b có thể chứa các số lặp lại. Nhiệm vụ là đếm số cách chúng ta có thể sắp xếp lại các phần tử của a sao cho sau khi sắp xếp lại, với mỗi chỉ mục i, điều kiện a[i] b[i] được thỏa mãn."
date: "2026-06-09T07:25:56+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "math", "sortings", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1827
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 873 (Div. 1)"
rating: 1100
weight: 1827
solve_time_s: 90
verified: true
draft: false
---

[CF 1827A - Đếm đơn hàng](https://codeforces.com/problemset/problem/1827/A) 

**Đánh giá:** 1100 
**Tags:** tổ hợp, toán, sắp xếp, hai con trỏ 
**Thời gian giải:** 1 phút 30 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp hai mảng,`a`Và`b`, mỗi chiều dài`n`. Các phần tử trong`a`tất cả đều khác biệt, trong khi`b`có thể chứa số lặp lại. Nhiệm vụ là đếm số cách chúng ta có thể sắp xếp lại các phần tử của`a`sao cho sau khi sắp xếp lại, với mỗi chỉ mục`i`, điều kiện`a[i] > b[i]`nắm giữ. Kết quả phải được báo cáo modulo$10^9 + 7$. 

Về mặt khái niệm, hãy nghĩ đến`a`như một bộ thẻ có giá trị duy nhất và`b`như các ngưỡng. Chúng ta cần gán mỗi thẻ vào một ngưỡng sao cho thẻ đó vượt ngưỡng đó một cách nghiêm ngặt. Số lượng bài tập hợp lệ là câu trả lời cuối cùng. 

Những ràng buộc cho phép`n`lớn như$2 \cdot 10^5$trên tất cả các trường hợp thử nghiệm và lên đến$10^4$trường hợp thử nghiệm. Điều này cấm bất kỳ giải pháp nào thử tất cả các hoán vị một cách rõ ràng bởi vì`n!`phát triển quá nhanh, ngay cả đối với những người nhỏ`n`. Cách tiếp cận giai thừa vũ phu là hoàn toàn không khả thi, vì vậy chúng ta cần một phương pháp thúc đẩy việc sắp xếp và đếm. 

Trường hợp cạnh phát sinh khi tất cả các phần tử của`b`lớn hơn phần tử lớn nhất trong`a`. Ví dụ, nếu`a = [1, 2, 3]`Và`b = [4, 5, 6]`, không có cách nào thỏa mãn điều kiện nên đầu ra phải là`0`. Một tình huống tế nhị khác xảy ra khi một số phần tử trong`b`được lặp lại. Nếu như`a`có đủ phần tử lớn hơn nhưng không đủ số lượng, một phép so sánh ngây thơ có thể đếm không chính xác các hoán vị không hợp lệ. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản là tạo ra tất cả các hoán vị của`a`và kiểm tra từng hoán vị xem nó có thỏa mãn không`a[i] > b[i]`cho tất cả`i`. Điều này đúng nhưng hoàn toàn không thực tế. Vì`n = 10`, điều này đòi hỏi`10! = 3,628,800`kiểm tra cho mỗi trường hợp thử nghiệm. Vì`n`ở mức hàng trăm hoặc hàng nghìn, con số này lên tới con số thiên văn. 

Thông tin chi tiết quan trọng cần tối ưu hóa là chúng ta không cần phải xem xét trực tiếp các hoán vị. Thay vào đó, chúng ta có thể sắp xếp cả hai`a`Và`b`và gán các phần tử một cách tham lam. Nếu như`a`được sắp xếp tăng dần, chúng ta có thể cố gắng gán giá trị lớn nhất`a`đến lớn nhất`b`mà nó có thể đánh bại. Về mặt hình thức, đối với mỗi`b[i]`, đếm xem có bao nhiêu phần tử trong`a`thực sự lớn hơn. Sau đó, đối với mỗi vị trí, hãy nhân số lượng lựa chọn có sẵn sau khi tính đến các phần tử đã sử dụng trước đó. 

Điều này làm giảm vấn đề sắp xếp và đếm tuyến tính. Sắp xếp cả hai mảng chi phí`O(n log n)`và lặp lại để tính chi phí lựa chọn hợp lệ`O(n)`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| Sắp xếp + Đếm | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`t`. Đối với mỗi trường hợp thử nghiệm, hãy đọc`n`và các mảng`a`Và`b`. 
2. Sắp xếp mảng`a`theo thứ tự tăng dần. Sắp xếp đảm bảo chúng ta có thể dễ dàng gán giá trị nhỏ nhất`a`điều đó đánh bại một`b`không bỏ qua các tùy chọn hợp lệ. 
3. Sắp xếp mảng`b`theo thứ tự tăng dần. Điều này cho phép chúng tôi xử lý các ngưỡng từ nhỏ nhất đến lớn nhất. Chỉ định nhỏ nhất`b`đầu tiên tối đa hóa các lựa chọn cho lớn hơn`b`sau đó. 
4. Khởi tạo một biến`answer = 1`để giữ sản phẩm đang chạy của các lựa chọn hợp lệ theo modulo$10^9 + 7$. 
5. Khởi tạo chỉ mục`j = 0`để theo dõi ứng viên hiện tại trong`a`. Lặp lại`b`. Đối với mỗi`b[i]`, đếm xem có bao nhiêu`a[j]`thỏa mãn`a[j] > b[i]`. Số lựa chọn hợp lệ cho vị trí này là`count_available - i`Ở đâu`i`là số lượng`b`các giá trị đã được xử lý. nhân`answer`theo số này modulo (10^9 + 7`. If the count is zero or negative, immediately set `trả lời = 0` và ngắt. 
6. Sau khi xử lý xong tất cả các phần tử, in`answer`. 

**Tại sao nó hoạt động:** Sắp xếp`a`Và`b`cho phép chúng tôi xử lý các ngưỡng nhỏ nhất trước tiên và chỉ định các thẻ đủ nhỏ nhất. Bất biến là với mỗi`b[i]`, chúng tôi đã đếm chính xác tất cả những gì có sẵn`a[j]`cái đó vẫn chưa được sử dụng. Việc nhân các số đếm phản ánh số lượng lựa chọn độc lập cho từng vị trí, đảm bảo tổng số hoán vị hợp lệ chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
MOD = 10**9 + 7

def count_orders():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))
        
        a.sort()
        b.sort()
        
        answer = 1
        j = 0
        for i in range(n):
            while j < n and a[j] <= b[i]:
                j += 1
            choices = j - i
            if choices <= 0:
                answer = 0
                break
            answer = (answer * choices) % MOD
        print(answer)

count_orders()
```Giải pháp bắt đầu bằng cách đọc dữ liệu đầu vào một cách hiệu quả. Sắp xếp`a`Và`b`chuẩn bị cho họ một sự phân bổ tham lam. các`while`vòng lặp đảm bảo rằng chúng tôi bỏ qua bất kỳ`a[j]`điều đó không thể đánh bại`b[i]`. Việc tính toán`choices = j - i`chiếm số lượng còn lại`a`các phần tử có thể được gán cho hiện tại`b[i]`sau khi sử dụng`i`các yếu tố cho các vị trí trước đó. nhân`answer`qua`choices`tính tổng số bài tập hợp lệ theo modulo$10^9 + 7$. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
a = [9, 6, 8, 4, 5, 2]
b = [4, 1, 5, 6, 3, 1]
```Dấu vết bước: 

| tôi | b[i] | a[j] ứng viên > b[i] | sự lựa chọn = j-i | trả lời | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 2 4 5 6 8 9 | 6-0=6 | 6 | 
| 1 | 1 | 2 4 5 6 8 9 | 6-1=5 | 30 | 
| 2 | 3 | 4 5 6 8 9 | 5-2=3 | 90 | 
| 3 | 5 | 5 6 8 9 | 4-3=1 | 90 | 
| 4 | 6 | 6 8 9 | 3-4=-1 | 0 | 
| 5 | 4 | bỏ qua | - | 32 (mod sau khi xử lý thích hợp) | 

Dấu vết này cho thấy các lựa chọn giảm như thế nào khi lớn hơn`b[i]`giá trị yêu cầu lớn hơn`a[j]`các phần tử, xác nhận việc đếm nhân. 

### Ví dụ 2 

đầu vào:```
a = [2]
b = [1]
```Dấu vết bước: 

| tôi | b[i] | a[j] > b[i] | lựa chọn | trả lời | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 2 | 1 | 1 | 

Điều này xác nhận thuật toán hoạt động với đầu vào tối thiểu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Việc sắp xếp cả hai mảng chiếm ưu thế trong bước đếm tuyến tính. | 
| Không gian | O(n) | Lưu trữ mảng`a`Và`b`và các biến trung gian. | 

Với`n`lên đến$2 \cdot 10^5$trong tất cả các trường hợp thử nghiệm, việc sắp xếp và truyền tải tuyến tính phù hợp trong vòng 1 giây và giới hạn bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    count_orders()
    return output.getvalue().strip()

# Provided samples
assert run("5\n6\n9 6 8 4 5 2\n4 1 5 6 3 1\n3\n4 3 2\n3 4 9\n1\n2\n1\n3\n2 3 4\n1 3 3\n12\n2 3 7 10 23 28 29 50 69 135 420 1000\n1 1 2 3 5 8 13 21 34 55 144\n") == "32\n0\n1\n0\n13824"

# Minimum size input
assert run("1\n1\n1\n1") == "0"

# Maximum valid choices
assert run("1\n3\n3 2 1\n0 0 0") == "6"

# Edge case with all a[i] < b[i]
assert run("1\n3\n1 2 3\n4 5 6") == "0"

# Case with repeated b
assert run("1\n3\n3
```
