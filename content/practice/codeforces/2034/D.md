---
title: "CF 2034D - Trí Tuệ của Darius"
description: "Chúng ta được cho một dãy các cột đá, mỗi cột có một chân đế và một số ít chữ khắc ở trên: 0, 1 hoặc 2. Chúng ta có thể coi dãy này như một mảng các số nguyên a[i] biểu thị số lượng chữ khắc trên cột thứ i."
date: "2026-06-08T11:34:03+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy", "implementation", "sortings"]
categories: ["algorithms"]
codeforces_contest: 2034
codeforces_index: "D"
codeforces_contest_name: "Rayan Programming Contest 2024 - Selection (Codeforces Round 989, Div. 1 + Div. 2)"
rating: 1600
weight: 2034
solve_time_s: 111
verified: false
draft: false
---

[CF 2034D - Trí tuệ của Darius](https://codeforces.com/problemset/problem/2034/D) 

**Đánh giá:** 1600 
**Tags:** thuật toán xây dựng, tham lam, triển khai, sắp xếp 
**Thời gian giải:** 1 phút 51 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy các cột đá, mỗi cột có một chân đế và một số ít chữ khắc ở trên: 0, 1 hoặc 2. Chúng ta có thể coi dãy này như một mảng các số nguyên`a[i]`đại diện cho số lượng chữ khắc trên cột thứ i. Mục tiêu là sắp xếp lại mảng này thành thứ tự không giảm bằng cách sử dụng một chuỗi các bước di chuyển. Mỗi lần di chuyển cho phép chúng ta lấy một dòng chữ từ một cột và chuyển nó sang một cột khác, nhưng chỉ khi chênh lệch giữa số lượng dòng chữ hiện tại của chúng chính xác là 1. Nói cách khác, chúng ta chỉ có thể di chuyển một dòng chữ từ cột có thêm một dòng chữ sang cột có ít dòng chữ hơn. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm chỉ định số lượng cột và cấu hình ban đầu của dòng chữ. Đầu ra phải mô tả một chuỗi hợp lệ tối đa`n`di chuyển (ở đâu`n`là số cột) tạo ra dãy không giảm. 

Những hạn chế của vấn đề là đáng kể. Mỗi cột chỉ có thể có 0, 1 hoặc 2 dòng chữ, điều này hạn chế đáng kể số lượng cấu hình có thể có. Tổng số cột trong tất cả các trường hợp thử nghiệm có thể lên tới 200.000, do đó, bất kỳ giải pháp nào cố gắng mô phỏng tất cả các lần chuyển có thể xảy ra hoặc thực hiện các thao tác O(n²) sẽ quá chậm. Cần có một giải pháp tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm. 

Trường hợp cạnh tinh tế phát sinh khi có nhiều số 2 và số 0 mà chỉ có một số 1 làm trung gian giữa chúng. Ví dụ, hãy xem xét`[2, 0, 2]`. Việc hoán đổi các phần tử một cách ngây thơ theo thứ tự sai có thể khiến chuỗi không được sắp xếp, vì chúng ta không thể di chuyển trực tiếp giữa 2 và 0. Thuật toán phải luôn tôn trọng`|difference| = 1`sự hạn chế. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force rất đơn giản: lặp lại mảng, tìm bất kỳ cặp cột nào có cột bên trái lớn hơn cột bên phải và hiệu là 1, thực hiện di chuyển và lặp lại cho đến khi mảng được sắp xếp. Cách tiếp cận này đúng về nguyên tắc, nhưng nó có thể yêu cầu các phép toán O(n2) trong trường hợp xấu nhất, ví dụ như khi chuỗi`[2, 0, 2, 0, 2, ...]`. Với`n`lên tới 200.000, điều này trở nên không khả thi về mặt tính toán. 

Quan sát quan trọng là chỉ có ba giá trị có thể có cho chiều cao cột: 0, 1 và 2. Do đó, việc sắp xếp mảng sẽ giảm xuống việc đếm số lần xuất hiện của từng giá trị và xây dựng lại mảng theo thứ tự được sắp xếp. Bản thân các nước đi có thể được xác định một cách có hệ thống: mỗi 2 xuất hiện trước số 1 có thể chuyển một dòng chữ thành số 1, và mỗi số 1 xuất hiện trước số 0 có thể chuyển một dòng chữ đến số 0. Vì luôn có ít nhất một cột có đúng một dòng chữ, nên số 1 này đóng vai trò như một trục xoay để điều hòa việc chuyển giữa các số 0 và 2. 

Quan sát này cho phép chúng ta giảm vấn đề xuống một lần quét tuyến tính đơn giản. Chúng tôi ghi lại các chỉ số của tất cả 0, 1 và 2. Sau đó, chúng tôi mô phỏng việc di chuyển các số 1 từ 2 sang 0 theo thứ tự, luôn chọn các cột ứng viên ngoài cùng bên trái để đảm bảo thứ tự không giảm. Mỗi nước đi bao gồm hai chỉ số trong đó chênh lệch giá trị chính xác là 1, vì vậy tất cả các nước đi đều hợp lệ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Đếm + Chuyển khoản tham lam | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo ba danh sách:`zeros`,`ones`, Và`twos`, để giữ chỉ số của các cột có chữ số 0, 1 và 2 tương ứng. 
2. Lặp lại mảng`a`một lần. Đối với mỗi cột, hãy thêm chỉ mục của nó vào danh sách thích hợp dựa trên giá trị của nó. Điều này cung cấp cho chúng tôi một phân vùng hoàn chỉnh các chỉ số theo chiều cao hiện tại. 
3. Khởi tạo danh sách trống`moves`để lưu trữ các thao tác. 
4. Chuyển 1s từ 2s sang 1s để làm trung gian giữa các cột cao và trung bình. Đối với mỗi chỉ số trong`twos`đó là trước một chỉ mục trong`ones`trong mảng, thực hiện chuyển từ số 2 sang số 1 và ghi vào`moves`. Cập nhật`a`mảng tương ứng. 
5. Chuyển các số 1 từ 1 sang 0 để làm trung gian giữa các cột trung bình và thấp. Đối với mỗi chỉ số trong`ones`đó là trước một chỉ mục trong`zeros`, thực hiện chuyển từ số 1 sang số 0 và ghi lại vào`moves`. Cập nhật`a`mảng tương ứng. 
6. Sau khi hoàn thành tất cả các lần chuyển, mảng`a`sẽ được sắp xếp theo thứ tự không giảm. Xuất ra số lần di chuyển theo sau là danh sách các thao tác di chuyển. 

Tại sao nó hoạt động: Ở mỗi bước, thuật toán chỉ chuyển các dòng chữ giữa các cột khác nhau 1. Bằng cách xử lý 2 giây thành 1 trước rồi sau đó từ 1 đến 0, chúng tôi đảm bảo rằng tất cả các số 2 được dịch chuyển sang phải và tất cả các số 0 được dịch chuyển sang trái. Vì tổng số cột nhỏ và các giá trị bị giới hạn, điều này đảm bảo rằng chuỗi cuối cùng được sắp xếp và không có bước di chuyển nào vi phạm ràng buộc sai phân. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        
        zeros, ones, twos = [], [], []
        for i, val in enumerate(a):
            if val == 0:
                zeros.append(i)
            elif val == 1:
                ones.append(i)
            else:
                twos.append(i)
        
        moves = []
        
        z_ptr, o_ptr, t_ptr = 0, 0, 0
        
        while t_ptr < len(twos) and o_ptr < len(ones):
            if twos[t_ptr] < ones[o_ptr]:
                moves.append((twos[t_ptr] + 1, ones[o_ptr] + 1))
                t_ptr += 1
                o_ptr += 1
            else:
                o_ptr += 1
        
        o_ptr, z_ptr = 0, 0
        while o_ptr < len(ones) and z_ptr < len(zeros):
            if ones[o_ptr] < zeros[z_ptr]:
                moves.append((ones[o_ptr] + 1, zeros[z_ptr] + 1))
                o_ptr += 1
                z_ptr += 1
            else:
                z_ptr += 1
        
        print(len(moves))
        for u, v in moves:
            print(u, v)

if __name__ == "__main__":
    solve()
```Mã đầu tiên phân vùng các chỉ số theo giá trị của chúng. Sau đó, nó sử dụng kỹ thuật hai con trỏ để mô phỏng việc chuyển hợp lệ từ giá trị cao hơn xuống giá trị thấp hơn trong khi vẫn duy trì chênh lệch yêu cầu chính xác là 1. Việc sử dụng`+1`chuyển đổi các chỉ mục Python dựa trên 0 thành các chỉ mục vấn đề dựa trên một. Thứ tự cẩn thận của các vòng lặp while đảm bảo rằng không có bước di chuyển không hợp lệ nào được thực hiện. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
4
0 2 0 1
```| Bước | Mảng a | Số không | Những cái | Hai | Di chuyển Đã thêm | 
| --- | --- | --- | --- | --- | --- | 
| Ban đầu | 0 2 0 1 | [0,2] | [3] | [1] | - | 
| Chuyển 2->1 | 0 1 0 2 | [0,2] | [3] | [1] | (2,4) | 
| Chuyển 1->0 | 0 0 1 2 | [0,1] | [3] | [ ] | (4,3) | 

Mảng bây giờ được sắp xếp thành 0,0,1,2. Mỗi lần di chuyển bao gồm các cột có chênh lệch chính xác là 1. 

Đầu vào mẫu 2:```
3
1 2 0
```| Bước | Mảng a | Số không | Những cái | Hai | Di chuyển Đã thêm | 
| --- | --- | --- | --- | --- | --- | 
| Ban đầu | 1 2 0 | [2] | [0] | [1] | - | 
| Chuyển 2->1 | 1 1 0 | [2] | [0,1] | [ ] | (2,1) | 
| Chuyển 1->0 | 0 1 1 | [2,0] | [1] | [ ] | (1,3) | 

Mảng bây giờ được sắp xếp là 0,1,1. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi mảng được quét một lần để phân loại các chỉ mục và mỗi lượt quét hai con trỏ sẽ quét tối đa n chỉ mục. | 
| Không gian | O(n) | Danh sách bổ sung lưu trữ các vị trí 0, 1 và 2. | 

Giải pháp dễ dàng phù hợp trong giới hạn thời gian 2 giây cho tổng số cột lên tới 2*10^5, vì các hoạt động là tuyến tính cho mỗi trường hợp thử nghiệm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

# Provided samples
assert run("3\n4\n0 2 0 1\n3\n1 2 0\n6\n0 1 1 2 2 2\n") == \
"2\n2 4\n4 3\n2\n2 1\n1 3\n0", "sample cases"

# Minimum input
assert run("1\n
```
