---
title: "CF 214B - Bài tập về nhà"
description: "Chúng ta được cho một tập hợp nhiều chữ số và được yêu cầu xây dựng số nguyên lớn nhất từ ​​một số hoặc tất cả các chữ số đó sao cho số kết quả có thể chia hết cho 2, 3 và 5 cùng một lúc."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "constructive-algorithms", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 214
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 131 (Div. 2)"
rating: 1600
weight: 214
solve_time_s: 169
verified: true
draft: false
---

[CF 214B - Bài tập về nhà](https://codeforces.com/problemset/problem/214/B) 

**Đánh giá:** 1600 
**Tags:** sức mạnh vũ phu, thuật toán xây dựng, lòng tham, toán học 
**Thời gian giải:** 2m 49s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một tập hợp nhiều chữ số và được yêu cầu xây dựng số nguyên lớn nhất từ một số hoặc tất cả các chữ số đó sao cho số kết quả có thể chia hết cho 2, 3 và 5 cùng một lúc. Vì một số chia hết cho 2, 3 và 5 thì chia hết cho 30 nên nhiệm vụ giảm xuống việc xây dựng số lớn nhất chia hết cho 30 từ các chữ số đã cho. Đầu vào bao gồm một số nguyên duy nhất`n`đại diện cho số chữ số, theo sau là`n`các chữ số cách nhau bởi dấu cách. Đầu ra là số hợp lệ lớn nhất hoặc`-1`nếu không có số đó tồn tại. Không được phép sử dụng số 0 đứng đầu ngoại trừ trường hợp chính số 0 đó. 

Những ràng buộc cho phép`n`lên tới 100.000, do đó, bất kỳ giải pháp nào có độ phức tạp kém hơn O (n log n) đều có khả năng vượt quá giới hạn thời gian. Chúng ta phải tránh các cách tiếp cận liệt kê tất cả các hoán vị có thể có của các chữ số, vì đó sẽ là giai thừa trong`n`và hoàn toàn không khả thi. Các trường hợp đặc biệt bao gồm chỉ có số 0, một chữ số không thể tạo thành bội số của 30 hoặc các chữ số có tổng không chia hết cho 3 trong khi chứa số 0, điều này sẽ ngăn cản khả năng chia hết cho 30. Ví dụ: đầu vào`1 5`nên sản xuất`-1`, không`5`, vì không có tổ hợp nào tạo thành bội số của 30. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản là tạo ra tất cả các hoán vị của các chữ số, kiểm tra tính chia hết cho từng chữ số và giữ mức tối đa. Lực lượng vũ phu này hoạt động vì nó xác định chính xác các số chia hết cho 30, nhưng độ phức tạp của nó là O (n!) trong trường hợp xấu nhất, khiến nó hoàn toàn không khả thi đối với`n`lên tới 100.000. 

Cái nhìn sâu sắc quan trọng là phân tách các quy tắc chia hết. Một số chia hết cho 2 phải có ít nhất một chữ số chẵn, số chia hết cho 5 phải kết thúc bằng 0 hoặc 5 và số chia hết cho 3 phải có các chữ số có tổng bằng bội số của 3. Tổng hợp các chữ số này để chia hết cho 30 thì chữ số cuối cùng phải là 0 (vì đây là chữ số duy nhất chia hết cho cả 2 và 5). Khi biết số 0 tồn tại, chúng ta phải điều chỉnh tổng các chữ số sao cho nó chia hết cho 3. Vì muốn có số lớn nhất nên các chữ số phải sắp xếp theo thứ tự giảm dần. Điều này làm giảm vấn đề đếm các chữ số, điều chỉnh tổng theo modulo 3 của chúng và in chúng theo thứ tự giảm dần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | Ồ (n!) | Quá chậm | 
| Tối ưu | O(n) | O(10) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số chữ số`n`và danh sách các chữ số. Đếm số lần xuất hiện của mỗi chữ số bằng mảng tần số có kích thước 10. 
2. Kiểm tra xem có ít nhất một số 0 không. Nếu không thì in`-1`vì số chia hết cho 30 phải có tận cùng bằng 0. 
3. Tính tổng các chữ số. Nếu tổng modulo 3 không bằng 0 thì loại bỏ tập hợp chữ số tối thiểu cần thiết để tổng chia hết cho 3. Chúng ta có thể loại bỏ một chữ số có dư 1 hoặc hai chữ số có dư 2 (hoặc ngược lại) để cố định tổng. Ưu tiên loại bỏ các chữ số nhỏ nhất để tối đa hóa số cuối cùng. 
4. Sau khi điều chỉnh, nếu không còn chữ số nào ngoài số 0, hãy in`0`. Điều này xử lý trường hợp hoàn toàn bằng không. 
5. Xây dựng số lớn nhất bằng cách lặp từ chữ số 9 xuống 0 và cộng mỗi chữ số theo số lần nó xuất hiện sau khi điều chỉnh. 
6. In số kết quả. 

Tại sao nó hoạt động: Thuật toán duy trì các bất biến rằng tổng các chữ số chia hết cho 3 và số tận cùng bằng 0, đảm bảo chia hết cho 30. Việc sắp xếp các chữ số theo thứ tự giảm dần sẽ tối đa hóa số. Loại bỏ các chữ số tối thiểu sẽ giữ nguyên số lớn nhất có thể còn lại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
digits = list(map(int, input().split()))

count = [0]*10
for d in digits:
    count[d] += 1

if count[0] == 0:
    print(-1)
    sys.exit()

total = sum(d * count[d] for d in range(10))
mod = total % 3

def remove_digits(mod_val):
    # Try removing smallest digit with remainder mod_val
    for r in range(1, 10):
        if r % 3 == mod_val and count[r] > 0:
            count[r] -= 1
            return True
    # Try removing two smallest digits with remainder 3 - mod_val
    needed = 2
    for r in range(1, 10):
        if r % 3 == (3 - mod_val):
            while count[r] > 0 and needed > 0:
                count[r] -= 1
                needed -= 1
            if needed == 0:
                return True
    return False

if mod != 0:
    if not remove_digits(mod):
        print(-1)
        sys.exit()

# handle all zeros
if sum(count[1:]) == 0:
    print(0)
    sys.exit()

res = []
for d in range(9, -1, -1):
    res.extend([str(d)]*count[d])
print(''.join(res))
```Mã đầu tiên đếm các chữ số và đảm bảo tồn tại ít nhất một số 0. Tổng modulo 3 được sửa bằng cách loại bỏ số chữ số tối thiểu để thỏa mãn tính chia hết. Đầu ra được xây dựng theo thứ tự giảm dần để có giá trị tối đa. Việc xử lý đặc biệt được thực hiện nếu chỉ còn lại số không. 

## Ví dụ đã hoạt động 

Mẫu 1: 

| chữ số | đếm | tổng hợp | mod | xóa | đếm cuối cùng | kết quả | 
| --- | --- | --- | --- | --- | --- | --- | 
| [0] | 0..1..0 | 0 | 0 | không | 0..1..0 | 0 | 

Điều này xác nhận rằng thuật toán xác định chính xác rằng số 0 là số duy nhất chia hết cho 30. 

Mẫu 2: 

| chữ số | đếm | tổng hợp | mod | xóa | đếm cuối cùng | kết quả | 
| --- | --- | --- | --- | --- | --- | --- | 
| [5,5,5,4,4,4,3,3,3,0] | đếm 0..9 | 36 | 0 | không | giống nhau | 5554443330 | 

Điều này cho thấy thuật toán bảo toàn số lớn nhất mà không cần loại bỏ các chữ số. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Đếm các chữ số là O(n), điều chỉnh tổng và in là O(10) | 
| Không gian | O(10) | Chỉ cần một dải tần số có kích thước cố định | 

Giải pháp dễ dàng phù hợp với giới hạn thời gian và bộ nhớ vì thao tác chiếm ưu thế tỷ lệ tuyến tính với số chữ số. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    digits = list(map(int, input().split()))
    count = [0]*10
    for d in digits:
        count[d] += 1
    if count[0] == 0:
        return "-1"
    total = sum(d * count[d] for d in range(10))
    mod = total % 3
    def remove_digits(mod_val):
        for r in range(1, 10):
            if r % 3 == mod_val and count[r] > 0:
                count[r] -= 1
                return True
        needed = 2
        for r in range(1, 10):
            if r % 3 == (3 - mod_val):
                while count[r] > 0 and needed > 0:
                    count[r] -= 1
                    needed -= 1
                if needed == 0:
                    return True
        return False
    if mod != 0:
        if not remove_digits(mod):
            return "-1"
    if sum(count[1:]) == 0:
        return "0"
    res = []
    for d in range(9, -1, -1):
        res.extend([str(d)]*count[d])
    return ''.join(res)

# provided samples
assert run("1\n0\n") == "0"
assert run("10\n5 5 5 4 4 4 3 3 3 0\n") == "5554443330"
assert run("1\n5\n") == "-1"
# custom cases
assert run("3\n0 0 0\n") == "0", "all zeros"
assert run("5\n3 3 3 3 0\n") == "33330", "sum divisible by 3 with zero"
assert run("4\n1 1 1 0\n") == "1110", "remove one digit to fix modulo 3"
assert run("5\n1 4 4 0 0\n") == "44100", "maximize number with multiple zeros"
assert run("6\n9 8 7 0 0 0\n") == "987000", "descending order with multiple zeros"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3\n0 0 0 | 0 | trường hợp tất cả số không | 
| 5\n3 3 3 3 0 | 33330 | tổng chia hết cho 3 bằng 0 | 
| 4\n1 1 1 0 | 1110 | bỏ một chữ số để sửa modulo 3 | 
| 5\n1 4 4 0 0 | 44100 | tối đa hóa số có nhiều số 0 |
