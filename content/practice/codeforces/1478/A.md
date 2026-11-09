---
title: "CF 1478A - Nezzar và những quả bóng đầy màu sắc"
description: "Nezzar có một chuỗi các quả bóng, mỗi quả bóng được gắn nhãn bằng một số từ một mảng không giảm. Anh ấy muốn tô màu những quả bóng bằng càng ít màu càng tốt với quy tắc rằng, nếu chúng ta tách riêng tất cả những quả bóng cùng màu thì số lượng của chúng phải tăng lên một cách nghiêm ngặt."
date: "2026-06-10T23:49:52+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1478
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 698 (Div. 2)"
rating: 800
weight: 1478
solve_time_s: 117
verified: true
draft: false
---

[CF 1478A - Nezzar và những quả bóng đầy màu sắc](https://codeforces.com/problemset/problem/1478/A) 

**Đánh giá:** 800 
**Tags:** vũ phu, tham lam 
**Thời gian giải:** 1 phút 57s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Nezzar có một chuỗi các quả bóng, mỗi quả bóng được gắn nhãn bằng một số từ một mảng không giảm. Anh ấy muốn tô màu những quả bóng bằng càng ít màu càng tốt với quy tắc rằng, nếu chúng ta tách riêng tất cả những quả bóng cùng màu thì số lượng của chúng phải tăng lên một cách nghiêm ngặt. Một dãy tăng dần yêu cầu mỗi số tiếp theo phải lớn hơn số trước. Đối với các quả bóng đơn lẻ hoặc các phần tử biệt lập, điều này không đáng kể. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm cho biết độ dài của mảng và chính mảng đó. Các số trên quả bóng dao động từ 1 đến$n$và mảng không giảm. Đầu ra phải là số lượng màu tối thiểu cần thiết cho mỗi trường hợp thử nghiệm. 

Các hạn chế là nhỏ:$n$lên tới 100 và có tối đa 100 trường hợp thử nghiệm. Điều này cho phép các thuật toán với$O(n^2)$phức tạp để chạy thoải mái trong thời gian giới hạn. Tuy nhiên, việc xử lý bất cẩn các số lặp lại có thể dẫn đến kết quả sai. Ví dụ, trong mảng`[1,1,2,2,3]`, một cách tiếp cận ngây thơ chỉ đơn giản là cố gắng gán màu một cách tham lam từ trái sang phải mà không xem xét sự trùng lặp có thể sử dụng nhiều màu hơn mức cần thiết. Câu trả lời đúng là 2 vì chúng ta có thể thay thế màu cho các số lặp lại:`[1,2,1,2,1]`. 

Các trường hợp cạnh bao gồm các mảng trong đó tất cả các số đều giống hệt nhau, các mảng có một phần tử duy nhất và các mảng có số lượng tăng dần. Các kịch bản này kiểm tra xem thuật toán có đếm chính xác các phần tử lặp lại hay không. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ là thử mọi cách gán màu có thể có cho mỗi quả bóng và kiểm tra xem chuỗi kết quả cho mỗi màu có tăng nghiêm ngặt hay không. Mặc dù đúng nhưng cách tiếp cận này có độ phức tạp theo cấp số nhân trong$n$và không thể thực hiện được ngay cả đối với$n=20$. 

Quan sát quan trọng là số lượng màu tối thiểu cần thiết được xác định bởi tần số tối đa của bất kỳ số đơn nào trong mảng. Mỗi số lặp lại cần phải đi vào một màu riêng để duy trì điều kiện tăng dần nghiêm ngặt cho mỗi màu. Ví dụ: nếu số 2 xuất hiện ba lần thì chúng ta phải có ít nhất ba màu, vì bất kỳ màu nào cũng có thể chứa nhiều nhất một lần xuất hiện của số 2. 

Vì vậy, cách tiếp cận tối ưu là đếm tần số của từng số và lấy giá trị lớn nhất. Điều này làm giảm vấn đề thành một vấn đề đếm đơn giản với độ phức tạp thời gian tuyến tính liên quan đến$n$. Từ$n$nhỏ nên phương pháp này cực kỳ hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(k^n) | O(n) | Quá chậm | 
| Tối ưu | O(n) cho mỗi trường hợp thử nghiệm | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case. 
2. Với mỗi test case, hãy đọc$n$và dãy số. 
3. Khởi tạo từ điển tần số hoặc mảng để đếm số lần xuất hiện của mỗi số. 
4. Lặp lại mảng và tăng số đếm cho mỗi số. 
5. Tìm tần số lớn nhất trong số tất cả các số. 
6. In tần số tối đa này thành số lượng màu tối thiểu cần thiết. 

Tại sao nó hoạt động: tần số tối đa của bất kỳ số nào thể hiện nút cổ chai chặt chẽ nhất. Không màu nào có thể chứa nhiều hơn một lần xuất hiện của cùng một số, vì vậy mỗi số lặp lại phải được chia thành nhiều màu. Bất kỳ tần số nào thấp hơn tần số tối đa sẽ vi phạm yêu cầu tăng nghiêm ngặt đối với một số số. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import Counter

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    freq = Counter(a)
    print(max(freq.values()))
```Mã đọc đầu vào hiệu quả bằng cách sử dụng`sys.stdin.readline`.`Counter`tự động đếm tần số của mỗi số. sử dụng`max(freq.values())`trích xuất trực tiếp tần số cao nhất, tương ứng với số lượng màu tối thiểu. Điều này tránh việc lặp lại thủ công và các lỗi tiềm ẩn. 

## Ví dụ đã hoạt động 

**Mẫu 1:** 

đầu vào:`[1, 1, 1, 2, 3, 4]`| Số | Tần số | 
| --- | --- | 
| 1 | 3 | 
| 2 | 1 | 
| 3 | 1 | 
| 4 | 1 | 

Tần số tối đa là 3 nên chúng ta cần 3 màu. 

**Mẫu 2:** 

đầu vào:`[1, 1, 2, 2, 3]`| Số | Tần số | 
| --- | --- | 
| 1 | 2 | 
| 2 | 2 | 
| 3 | 1 | 

Tần số tối đa là 2 nên chúng ta cần 2 màu. 

Những dấu vết này cho thấy thuật toán xác định chính xác yêu cầu về màu sắc dựa trên các số lặp lại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Tần số đếm là tuyến tính, tìm cực đại là tuyến tính theo số lượng số duy nhất, nhiều nhất là n | 
| Không gian | O(n) cho mỗi trường hợp thử nghiệm | Từ điển truy cập lưu trữ tần số cho mỗi số duy nhất, tối đa n mục | 

Được cho$n \le 100$Và$t \le 100$, tổng số thao tác nhiều nhất là$100*100=10^4$, trong giới hạn 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io
from collections import Counter

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        freq = Counter(a)
        output.append(str(max(freq.values())))
    return "\n".join(output)

# provided samples
assert run("5\n6\n1 1 1 2 3 4\n5\n1 1 2 2 3\n4\n2 2 2 2\n3\n1 2 3\n1\n1\n") == "3\n2\n4\n1\n1", "samples"

# custom cases
assert run("2\n3\n1 1 1\n4\n1 2 3 4\n") == "3\n1", "all same / all distinct"
assert run("1\n5\n2 2 2 2 2\n") == "5", "max frequency all same"
assert run("1\n6\n1 1 2 2 3 3\n") == "2", "multiple repeated numbers"
assert run("1\n1\n1\n") == "1", "single element"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3\n1 1 1\n4\n1 2 3 4`|`3\n1`| Xử lý đúng tất cả giống nhau và khác biệt | 
|`5\n2 2 2 2 2`|`5`| Tần số tối đa bằng kích thước mảng | 
|`6\n1 1 2 2 3 3`|`2`| Nhiều số có cùng tần số tối đa | 
|`1\n1`|`1`| Vỏ cạnh một phần tử | 

## Vỏ cạnh 

cho`[2,2,2,2]`, thuật toán đếm 4 lần xuất hiện của 2. Tần số tối đa là 4, do đó nó tạo ra 4. Điều này phản ánh chính xác rằng chúng ta cần 4 màu vì không có hai quả bóng nào có số 2 có thể có cùng một màu. 

Vì`[1]`, tần số 1 là 1, tần số tối đa là 1, đầu ra là 1. Mảng một phần tử luôn cần một màu. 

Vì`[1,2,3,4]`, tất cả các số đều khác biệt, tần số tối đa là 1, đầu ra là 1. Điều này xác nhận thuật toán xử lý chính xác các chuỗi tăng dần mà không có màu sắc không cần thiết. 

Tất cả các trường hợp đặc biệt đều chứng minh rằng thuật toán mạnh mẽ và phù hợp chính xác với yêu cầu của vấn đề.
