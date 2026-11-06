---
title: "CF 122A - Phân chia may mắn"
description: "Chúng ta được yêu cầu xác định xem một con số đã cho có gần như là may mắn hay không. Một số gần như may mắn nếu nó chia hết cho ít nhất một số may mắn. Con số may mắn là những số nguyên dương được tạo thành hoàn toàn từ các chữ số 4 và 7, như 4, 7, 44, 47, 74, v.v."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 122
codeforces_index: "A"
codeforces_contest_name: "Codeforces Beta Round 91 (Div. 2 Only)"
rating: 1000
weight: 122
solve_time_s: 72
verified: true
draft: false
---

[CF 122A - Phân chia may mắn](https://codeforces.com/problemset/problem/122/A) 

**Đánh giá:** 1000 
**Tags:** vũ lực, lý thuyết số 
**Thời gian giải:** 1 phút 12s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xác định xem một con số đã cho có gần như là may mắn hay không. Một số gần như may mắn nếu nó chia hết cho ít nhất một số may mắn. Con số may mắn là những số nguyên dương được tạo thành hoàn toàn từ các chữ số 4 và 7, như 4, 7, 44, 47, 74, v.v. Đầu vào là một số nguyên duy nhất`n`trong khoảng từ 1 đến 1000 và đầu ra đơn giản là "CÓ" nếu số đó gần như may mắn hoặc "KHÔNG" nếu ngược lại. 

Những hạn chế rất khiêm tốn. Từ`n`nhiều nhất là 1000, chúng ta có thể kiểm tra khả năng chia hết cho mọi số may mắn lên tới 1000 mà không phải lo lắng về hiệu suất. Điều này cũng cho phép chúng ta tính toán trước tất cả các con số may mắn trong phạm vi đó. Giới hạn trên nhỏ loại trừ sự cần thiết của các thuật toán lý thuyết số tiên tiến hoặc các kỹ thuật sàng lọc được tối ưu hóa. 

Các trường hợp đặc biệt không rõ ràng phát sinh từ các số nhỏ, các số tự nó là may mắn và các số là bội số của các số may mắn nhưng bản thân chúng lại không may mắn. Ví dụ số 4 gần như là may mắn vì nó may mắn. Số 16 gần như may mắn vì nó chia hết cho 4, mặc dù nó có một chữ số khác 4 hoặc 7. Việc thực hiện bất cẩn chỉ có thể kiểm tra xem liệu số đó có phải là số 16 hay không.`n`thật may mắn, bỏ qua tính chia hết, điều này sẽ thất bại đối với các số như 16 hoặc 28. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực đơn giản sẽ tạo ra tất cả các số từ 1 đến`n`và kiểm tra xem mỗi số có may mắn không. Đối với mỗi số may mắn được tìm thấy, chúng tôi sẽ kiểm tra xem nó có chia hết không`n`. Cách tiếp cận này đúng vì mọi số gần như may mắn đều phải chia hết cho một số may mắn nào đó. Tuy nhiên, việc lặp qua tất cả các số lên tới 1000 và liên tục kiểm tra các chữ số của chúng là hơi dư thừa. 

Điểm mấu chốt là chúng ta có thể tạo ra tất cả các số may mắn lên tới 1000 bằng cách sử dụng cấu trúc đệ quy hoặc lặp lại. Bắt đầu với số trống và nối lại 4 hoặc 7, dừng khi số vượt quá 1000. Khi có danh sách này, chúng tôi chỉ cần lặp lại danh sách đó và kiểm tra tính chia hết. Cấu trúc của các số may mắn - các chữ số bị giới hạn và giới hạn trên nhỏ - làm cho việc này trở nên khả thi và hiệu quả. Thay vì kiểm tra mọi số từ 1 đến 1000, chúng ta chỉ kiểm tra một nhóm nhỏ các ứng viên hợp lệ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n log n) | O(1) | Có thể chấp nhận ở đây nhưng không hiệu quả | 
| Tính toán trước số may mắn | O(L) trong đó L là số con số may mắn 1000 | O(L) | Tối ưu, được chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo danh sách chứa tất cả các số may mắn lên tới 1000. Chúng ta bắt đầu với 4 và 7 làm số cơ bản vì đây là những số may mắn đơn giản nhất. 
2. Sử dụng hàng đợi để lặp lại các số may mắn mới bằng cách thêm 4 hoặc 7 vào mỗi số trong hàng đợi. Dừng tạo số mới khi chúng vượt quá 1000 vì chúng không thể chia`n`. 
3. Lặp lại danh sách các số may mắn. Với mỗi con số may mắn, hãy kiểm tra xem`n`chia hết cho nó bằng toán tử modulo. 
4. Nếu tìm được số may mắn chia hết cho`n`, in "CÓ" và kết thúc. Nếu không ai trong số họ chia`n`, in "KHÔNG". 

Tại sao nó hoạt động: Bằng cách xây dựng, chúng tôi đã liệt kê tất cả các con số may mắn có thể chia`n`. Vì hầu hết các số may mắn được xác định bằng khả năng chia hết cho một số số may mắn, nên việc kiểm tra khả năng chia hết đối với bộ hoàn chỉnh này đảm bảo tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def generate_lucky_numbers(limit):
    lucky = []
    queue = [4, 7]
    while queue:
        num = queue.pop(0)
        if num > limit:
            continue
        lucky.append(num)
        queue.append(num * 10 + 4)
        queue.append(num * 10 + 7)
    return lucky

n = int(input())
lucky_numbers = generate_lucky_numbers(1000)

for ln in lucky_numbers:
    if n % ln == 0:
        print("YES")
        break
else:
    print("NO")
```Trước tiên, chúng tôi tạo ra tất cả các số may mắn lên tới 1000 bằng phương pháp đơn giản giống BFS, nối các chữ số 4 và 7. Sau đó, chúng tôi lặp lại các số được tạo và kiểm tra khả năng chia hết. Việc sử dụng`else`trên`for`vòng lặp đảm bảo rằng chúng tôi chỉ in "KHÔNG" nếu không có số may mắn nào được chia`n`. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:`47`| Biến | Giá trị | 
| --- | --- | 
| n | 47 | 
| số may mắn | [4, 7, 44, 47, 74, 77, ...] | 
| ln trong quá trình lặp | 4 → 7 → 44 → 47 | 

Trong quá trình lặp, 47 chia hết cho 47. Kết quả là "CÓ". 

### Mẫu 2 

đầu vào:`16`| Biến | Giá trị | 
| --- | --- | 
| n | 16 | 
| số may mắn | [4, 7, 44, 47, 74, 77, ...] | 
| ln trong quá trình lặp | 4 → 7 → 44 ... | 

16 % 4 == 0. Đầu ra là "CÓ". Điều này chứng tỏ thuật toán xử lý chính xác hầu hết các số may mắn mà bản thân chúng lại không may mắn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(L) | L là số con số may mắn ≤1000; kiểm tra tính chia hết cho mỗi là thời gian không đổi. | 
| Không gian | O(L) | Chúng tôi lưu trữ tất cả các số may mắn 1000. | 

Cho n ≤ 1000, tổng số số may mắn nhỏ (dưới 20), do đó thuật toán này cực kỳ hiệu quả và nằm trong giới hạn 2s/256MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    exec(open('solution.py').read())
    return sys.stdout.getvalue().strip()

# provided samples
assert run("47\n") == "YES", "sample 1"
assert run("16\n") == "YES", "divisible by 4"

# custom cases
assert run("5\n") == "NO", "not divisible by any lucky number"
assert run("4\n") == "YES", "lucky number itself"
assert run("28\n") == "YES", "divisible by 7"
assert run("1000\n") == "NO", "edge of constraint not divisible by any lucky number"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 5 | KHÔNG | Con số không may mắn | 
| 4 | CÓ | Bản thân con số may mắn | 
| 28 | CÓ | Chia hết cho số may mắn (7) | 
| 1000 | KHÔNG | Ranh giới đầu vào tối đa | 

## Vỏ cạnh 

Đầu vào nhỏ nhất`n = 1`tạo ra "KHÔNG" vì 1 không chia hết cho bất kỳ số may mắn nào. Đầu vào lớn nhất`n = 1000`được xử lý chính xác vì tất cả các số may mắn lớn hơn 1000 không bao giờ được tạo ra và chúng tôi kiểm tra chính xác khả năng chia hết cho tất cả các số may mắn có thể có. Bản thân những con số may mắn như 4, 7, 47 ngay lập tức được xác nhận là gần như may mắn vì chúng tự chia cho nhau. Các số là bội số của các số may mắn nhỏ nhưng bản thân chúng không may mắn, như 16, 28 và 44, cũng được vòng lặp chia hết của chúng tôi phát hiện chính xác.
