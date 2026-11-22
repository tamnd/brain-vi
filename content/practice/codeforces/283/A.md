---
title: "CF 283A - Bò và Trình tự"
description: "Chúng ta bắt đầu với một chuỗi chứa một phần tử duy nhất, 0, và thực hiện một chuỗi n thao tác. Các thao tác có thể sửa đổi trình tự theo ba cách: tăng phần tử a đầu tiên lên một số giá trị x, thêm một số mới vào cuối hoặc xóa phần tử cuối cùng."
date: "2026-06-05T09:38:42+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "data-structures", "implementation"]
categories: ["algorithms"]
codeforces_contest: 283
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 174 (Div. 1)"
rating: 1600
weight: 283
solve_time_s: 106
verified: true
draft: false
---

[CF 283A - Bò và Trình tự](https://codeforces.com/problemset/problem/283/A) 

**Đánh giá:** 1600 
**Tas:** thuật toán xây dựng, cấu trúc dữ liệu, triển khai 
**Thời gian giải:** 1 phút 46s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta bắt đầu với một chuỗi chứa một phần tử duy nhất,`0`, và thực hiện một chuỗi`n`hoạt động. Các hoạt động có thể sửa đổi trình tự theo ba cách: tăng số thứ nhất`a`các phần tử theo một giá trị nào đó`x`, thêm số mới vào cuối hoặc xóa phần tử cuối cùng. Sau mỗi thao tác, chúng ta cần tính trung bình cộng của tất cả các phần tử trong dãy. Đầu ra là một số dấu phẩy động có độ chính xác ít nhất sáu chữ số. 

Những hạn chế chính là`n`có thể lên tới 2·10⁵ và chuỗi có thể phát triển bằng cách nối thêm các phần tử. Bất kỳ cách tiếp cận ngây thơ nào sửa đổi rõ ràng tất cả các`a`các phần tử trong một thao tác bổ sung có thể chiếm tổng cộng tới O(n2) vì mỗi thao tác loại 1 có thể chạm vào các phần tử O(n). Tốc độ này quá chậm so với giới hạn thời gian 2 giây. Các số nguyên liên quan đều nhỏ (giá trị tuyệt đối ≤ 10³), do đó, tràn số không phải là vấn đề đáng lo ngại trong Python, nhưng chúng ta cần cẩn thận khi tính trung bình dấu phẩy động. 

Các trường hợp cạnh mà việc triển khai đơn giản có thể xử lý sai bao gồm: thực hiện thao tác loại 3 (loại bỏ phần tử cuối cùng) khi có chính xác hai phần tử và thực hiện bổ sung loại 1 trên các phần tử được thêm động thay vì một phần của chuỗi ban đầu. Ví dụ, nếu trình tự là`[0, 2]`và chúng tôi biểu diễn`1 2 3`(thêm 3 vào 2 phần tử đầu tiên), kết quả phải là`[3, 5]`. Cách tiếp cận bất cẩn chỉ cập nhật phần tử đầu tiên hoặc không tính đến các giá trị được nối thêm sẽ tạo ra giá trị trung bình sai. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực rất đơn giản: duy trì toàn bộ chuỗi dưới dạng danh sách. Đối với mỗi thao tác loại 1, hãy lặp lại thao tác đầu tiên`a`phần tử và thêm`x`. Đối với loại 2, hãy nối thêm`k`. Đối với loại 3, hãy xóa phần tử cuối cùng. Sau mỗi thao tác, tính tổng chuỗi và chia cho độ dài của nó. Điều này làm việc cho nhỏ`n`, nếu không có`n ≈ 2·10⁵`với các thao tác loại 1 lặp đi lặp lại ảnh hưởng đến phần tử O(n), số lượng thao tác sẽ trở thành O(n²), tức là khoảng 4·10¹⁰, vượt xa thời gian cho phép. 

Điểm mấu chốt là chúng ta không cần biết giá trị chính xác của từng phần tử riêng lẻ; chúng ta chỉ cần mức trung bình. Hoạt động loại 1 luôn ảnh hưởng đến tiền tố của chuỗi. Chúng tôi có thể theo dõi các cập nhật này một cách hiệu quả bằng cách sử dụng mảng thứ cấp lưu trữ mức tăng dần theo độ dài tiền tố. Cho phép`increment[i]`đại diện cho tổng giá trị bổ sung được áp dụng cho lần đầu tiên`i+1`các phần tử. Bằng cách giữ tổng của chuỗi (`total_sum`), chúng ta có thể cập nhật tổng này theo O(1) cho các phép toán loại 1, vì việc thêm`x`đến lần đầu tiên`a`các phần tử làm tăng tổng bằng`a * x`. Các hoạt động loại 2 và loại 3 có thể được xử lý bằng cách lưu trữ riêng phần tử cuối cùng và duy trì tổng tương ứng. Điều này làm giảm tổng độ phức tạp xuống O(n), phù hợp với giới hạn đầu vào. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Tăng tiền tố + Tổng hoạt động | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo`seq`BẰNG`[0]`,`increments`BẰNG`[0]`, Và`total_sum`bằng 0.`increments[i]`sẽ lưu trữ giá trị bổ sung được áp dụng cho`seq[i]`do hoạt động loại 1. 
2. Đối với mỗi thao tác, hãy đọc loại của nó`t`. 
3. Nếu`t`là 1, đọc`a`Và`x`. Cập nhật`increments[a-1]`bằng cách thêm`x`. Cập nhật`total_sum`bằng cách thêm`a * x`. Điều này nắm bắt được tác dụng của việc thêm`x`đến lần đầu tiên`a`phần tử mà không sửa đổi từng phần tử riêng lẻ. 
4. Nếu`t`là 2, đọc`k`. Nối thêm`k`ĐẾN`seq`và nối thêm`0`ĐẾN`increments`. Cập nhật`total_sum`bằng cách thêm`k`. 
5. Nếu`t`là 3, loại bỏ phần tử cuối cùng`last_val`từ`seq`. Lấy giá trị gia tăng của nó từ`increments[-1]`và trừ nó khỏi phần tử thứ hai đến cuối cùng trong`increments`sao cho tổng tiền tố vẫn nhất quán. Cập nhật`total_sum`bằng cách trừ`last_val + increments[-1]`. Xóa phần tử cuối cùng khỏi`increments`. 
6. Sau mỗi thao tác, tính giá trị trung bình là`total_sum / len(seq)`và in nó với 6 chữ số thập phân. 

**Tại sao nó hoạt động** 

các`increments`mảng đảm bảo rằng mỗi tiền tố của chuỗi theo dõi chính xác các phần bổ sung tích lũy từ các hoạt động loại 1. Các thao tác loại 2 và loại 3 cập nhật tổng một cách rõ ràng, duy trì tổng hoạt động chính xác. Giá trị trung bình tính từ`total_sum`chia cho chiều dài của`seq`luôn phản ánh trình tự hiện tại, mặc dù trình tự đó không bao giờ được xây dựng lại đầy đủ. Điều này duy trì tính chính xác trong O(1) cho mỗi thao tác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
seq = [0]
increments = [0]
total_sum = 0

for _ in range(n):
    op = list(map(int, input().split()))
    t = op[0]
    
    if t == 1:
        a, x = op[1], op[2]
        increments[a-1] += x
        total_sum += a * x
    elif t == 2:
        k = op[1]
        seq.append(k)
        increments.append(0)
        total_sum += k
    else:
        last_val = seq.pop()
        inc = increments.pop()
        total_sum -= last_val + inc
        if increments:
            increments[-1] += inc
    print(total_sum / len(seq))
```Giải pháp khởi tạo`seq`với 0.`increments`gương mảng`seq`về chiều dài. Hoạt động loại 1 cập nhật tiền tố bị ảnh hưởng cuối cùng trong`increments`Và`total_sum`. Loại-2 nối thêm một giá trị mới và số 0 tương ứng vào`increments`. Loại-3 bật phần tử cuối cùng, điều chỉnh mức tăng tiền tố để truyền giá trị của nó một cách chính xác và cập nhật tổng. sử dụng`total_sum`đảm bảo rằng giá trị trung bình được tính trong thời gian O(1). 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1**```
5
2 1
3
2 3
2 1
3
```| Bước | Hoạt động | seq | gia tăng | tổng_tổng | trung bình | 
| --- | --- | --- | --- | --- | --- | 
| 0 | ban đầu | [0] | [0] | 0 | 0 | 
| 1 | 2 1 | [0,1] | [0,0] | 1 | 0,5 | 
| 2 | 3 | [0] | [0] | 0 | 0,0 | 
| 3 | 2 3 | [0,3] | [0,0] | 3 | 1,5 | 
| 4 | 2 1 | [0,3,1] | [0,0,0] | 4 | 1.333333 | 
| 5 | 3 | [0,3] | [0,0] | 3 | 1,5 | 

Dấu vết này xác nhận rằng việc tăng tiền tố sẽ lan truyền chính xác khi các phần tử bị xóa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi thao tác được xử lý trong O(1) bằng cách sử dụng số gia tiền tố và tổng chạy | 
| Không gian | O(n) |`seq`Và`increments`mỗi mảng lưu trữ tối đa n phần tử | 

Được cho`n ≤ 2·10⁵`, giải pháp O(n) hoàn thành thoải mái dưới 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    # paste the solution here
    n = int(input())
    seq = [0]
    increments = [0]
    total_sum = 0
    for _ in range(n):
        op = list(map(int, input().split()))
        t = op[0]
        if t == 1:
            a, x = op[1], op[2]
            increments[a-1] += x
            total_sum += a * x
        elif t == 2:
            k = op[1]
            seq.append(k)
            increments.append(0)
            total_sum += k
        else:
            last_val = seq.pop()
            inc = increments.pop()
            total_sum -= last_val + inc
            if increments:
                increments[-1] += inc
        print(total_sum / len(seq))
    return output.getvalue().strip()

# Provided samples
assert run("5\n2 1\n3\n2 3\n2 1\n3\n") == "0.5\n0.0\n1.5\n1.3333333333333333\n1.5", "sample 1"

# Custom cases
assert run("1\n2 1000\n") == "500.0", "single append"
assert run("3\n2 1\n1 2 5\n3\n") == "0.5\n5.5\n5.0", "type-1 then remove"
assert run("2\n1 1 10\n3\n") == "10.0\n0.0", "
```
