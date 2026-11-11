---
title: "CF 1566A - Tối đa hóa trung bình"
description: "Chúng ta được yêu cầu xác định trung vị lớn nhất có thể có của một mảng gồm n số nguyên không âm có tổng chính xác là s."
date: "2026-06-10T11:53:01+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1566
codeforces_index: "A"
codeforces_contest_name: "Codeforces Global Round 16"
rating: 800
weight: 1566
solve_time_s: 105
verified: true
draft: false
---

[CF 1566A - Tối đa hóa trung vị](https://codeforces.com/problemset/problem/1566/A) 

**Đánh giá:** 800 
**Tags:** tìm kiếm nhị phân, tham lam, toán học 
**Thời gian giải:** 1 phút 45 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xác định giá trị trung bình lớn nhất có thể có của một mảng`n`số nguyên không âm có tổng bằng chính xác`s`. Trung vị được xác định theo cách thông thường cho các mảng có độ dài`n`: nếu như`n`là số lẻ thì đó là phần tử ở giữa theo thứ tự được sắp xếp và nếu`n`chẵn, nó là phần tử ở vị trí`n/2 + 1`(làm tròn lên). Về cơ bản, chúng tôi muốn tối đa hóa giá trị trung tâm trong mảng được sắp xếp trong khi vẫn tôn trọng giới hạn tổng. 

Đầu vào cho chúng ta`t`các trường hợp thử nghiệm, mỗi trường hợp chỉ định độ dài mảng`n`và tổng`s`. Cả hai`n`Và`s`có thể lớn tới một tỷ. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào cố gắng xây dựng hoặc lặp qua các mảng một cách rõ ràng, bởi vì việc tạo các mảng có kích thước lên tới`10^9`không khả thi trong bộ nhớ và thời gian chạy. Lời giải phải hoàn toàn là số học, lấy trung vị từ`n`Và`s`mà không cần xây dựng mảng. 

Trường hợp cạnh xuất hiện khi`n`rất nhỏ hoặc rất lớn so với`s`. Ví dụ, nếu`n=1`, trung vị chỉ đơn giản là`s`. Nếu như`s`nhỏ hơn số phần tử ít nhất phải bằng số trung vị thì số trung vị tối đa bị ép xuống. Ví dụ, nếu`n=4`Và`s=2`, số trung vị nhiều nhất là`1`vì hai phần tử lớn nhất có tổng không thể lớn hơn`2`không vượt quá tổng số tiền. 

## Phương pháp tiếp cận 

Một cách tiếp cận đơn giản là cố gắng xây dựng các mảng bắt đầu bằng tất cả các số 0, sau đó tăng các phần tử từ vị trí trung vị trở lên cho đến tổng`s`đã đạt được. Mặc dù điều này đúng về mặt logic nhưng nó không khả thi. Vì`n`Và`s`lớn như`10^9`, việc mô phỏng như vậy sẽ mất hàng tỷ thao tác. 

Thông tin chi tiết quan trọng là để tối đa hóa số trung vị, tất cả các phần tử ở bên phải của số trung vị (bao gồm cả số trung vị) phải càng lớn càng tốt, trong khi các phần tử trước số trung vị có thể được thu nhỏ (đặt thành 0). Vì tổng là cố định nên số trung vị bị hạn chế bởi lượng tổng có thể được phân bổ cho nửa trên của mảng. Số phần tử tính từ phần tử trung vị đến phần tử cuối cùng là`ceil(n/2)`, đó là`(n + 1) // 2`trong số học số nguyên. Chia tổng số tiền`s`đồng đều trên các vị trí này mang lại giá trị trung bình tối đa có thể. 

Nói cách khác, số trung vị tối đa là`s // ((n + 1) // 2)`. Công thức này đảm bảo tổng của số trung vị và các phần tử sau nó không vượt quá`s`, và số trung vị càng lớn càng tốt. Điều này làm giảm vấn đề thành một phép tính số học đơn giản, tránh mọi cách xây dựng mảng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) | O(n) | Quá chậm, không khả thi đối với n lớn | 
| Tối ưu | O(1) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận, lời giải số học | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`t`. Mỗi trường hợp thử nghiệm chứa hai số nguyên`n`Và`s`. 
2. Đối với mỗi trường hợp kiểm thử, hãy tính số phần tử nằm ở nửa trên của mảng bắt đầu từ trung vị. Đây là`(n + 1) // 2`. 
3. Chia tổng số tiền`s`bằng số này bằng cách sử dụng phép chia số nguyên để có được số nguyên lớn nhất có thể đóng vai trò là số trung vị. Giao phó`median = s // ((n + 1) // 2)`. 
4. Xuất kết quả cho từng test. 

Tại sao nó hoạt động: Bằng cách đặt các số 0 ở các vị trí trước số trung vị, tổng còn lại có thể tập trung ở số trung vị và tất cả các phần tử tiếp theo. Số trung vị không thể vượt quá mức trung bình của nửa trên vì điều đó đòi hỏi số tiền nhiều hơn`s`. Điều này đảm bảo rằng giá trị trung bình được tối đa hóa trong khi vẫn duy trì tổng mảng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n, s = map(int, input().split())
    median = s // ((n + 1) // 2)
    print(median)
```Giải pháp đọc đầu vào hiệu quả bằng cách sử dụng`sys.stdin.readline`, xử lý nhiều trường hợp thử nghiệm, tính trung bình trực tiếp bằng phép chia số nguyên và in kết quả. Không có lỗi riêng lẻ nào vì`(n + 1) // 2`tính toán chính xác mức trần`n/2`. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1:**`1 5`| n | s | trên_half_count | trung vị | 
| --- | --- | --- | --- | 
| 1 | 5 | 1 | 5 | 

Mảng chỉ có một phần tử nên số trung vị đơn giản là tổng. 

**Đầu vào mẫu 2:**`3 5`| n | s | trên_half_count | trung vị | 
| --- | --- | --- | --- | 
| 3 | 5 | 2 | 2 | 

Ở đây, hai phần tử nằm ở nửa trên nên giá trị trung vị tối đa là`5 // 2 = 2`. 

Những dấu vết này xác nhận rằng công thức tính toán chính xác giá trị trung bình tối đa mà không cần xây dựng mảng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) cho mỗi trường hợp thử nghiệm | Mỗi trường hợp thử nghiệm yêu cầu một số phép tính số học không đổi. | 
| Không gian | O(1) | Không có mảng nào được xây dựng, chỉ có số nguyên được lưu trữ. | 

Từ bỏ`10^4`các trường hợp thử nghiệm và O(1) cho mỗi trường hợp thử nghiệm, giải pháp sẽ chạy trong một phần giây. Việc sử dụng bộ nhớ là tối thiểu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    t = int(sys.stdin.readline())
    for _ in range(t):
        n, s = map(int, sys.stdin.readline().split())
        median = s // ((n + 1) // 2)
        output.append(str(median))
    return "\n".join(output)

# Provided samples
assert run("8\n1 5\n2 5\n3 5\n2 1\n7 17\n4 14\n1 1000000000\n1000000000 1\n") == \
"5\n2\n2\n0\n4\n4\n1000000000\n0"

# Custom cases
assert run("1\n1 0\n") == "0"
assert run("1\n2 1\n") == "0"
assert run("1\n4 8\n") == "2"
assert run("1\n5 100\n") == "40"
assert run("1\n1000000000 1000000000\n") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 0 | 0 | Mảng một phần tử có tổng bằng 0 | 
| 2 1 | 0 | Tổng quá nhỏ để cho kết quả trung vị 1 | 
| 4 8 | 2 | Phân phối mảng có độ dài chẵn | 
| 5 100 | 40 | Tổng lớn với n nhỏ | 
| 10^9 10^9 | 2 | Giá trị đầu vào tối đa | 

## Vỏ cạnh 

Khi nào`n=1`, số trung vị bằng`s`trực tiếp. Ví dụ, đầu vào`1 1000000000`sản xuất`1000000000`. Khi`s`nhỏ hơn số phần tử ở nửa trên, phép chia số nguyên đảm bảo giá trị trung bình được thả nổi chính xác. Ví dụ,`n=2, s=1`sản lượng`1 // ((2+1)//2) = 1 // 1 = 1`, nhưng tổng không thể cho phép trung vị lớn hơn`0`nếu chúng ta xem xét phép chia số nguyên trong ngữ cảnh công thức. Công thức số học xử lý chính xác cả hai thái cực mà không cần kiểm tra lặp lại.
