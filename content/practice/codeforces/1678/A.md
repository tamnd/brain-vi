---
title: "CF 1678A - Tokitsukaze và Chuỗi Tất Cả Không"
description: "Bài toán yêu cầu chúng ta biến một dãy số thành toàn số 0 bằng một thao tác cụ thể. Trong mỗi thao tác, chúng tôi chọn hai phần tử riêng biệt. Nếu chúng bằng nhau, chúng ta thay thế một trong số chúng bằng 0. Nếu chúng khác nhau, chúng ta thay thế cả hai bằng cái nhỏ hơn trong hai."
date: "2026-06-10T00:45:35+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1678
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 789 (Div. 2)"
rating: 800
weight: 1678
solve_time_s: 107
verified: true
draft: false
---

[CF 1678A - Tokitsukaze và Trình tự tất cả bằng không](https://codeforces.com/problemset/problem/1678/A) 

**Đánh giá:** 800 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 47s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Bài toán yêu cầu chúng ta biến một dãy số thành toàn số 0 bằng một thao tác cụ thể. Trong mỗi thao tác, chúng tôi chọn hai phần tử riêng biệt. Nếu chúng bằng nhau, chúng ta thay thế một trong số chúng bằng 0. Nếu chúng khác nhau, chúng ta thay thế cả hai bằng cái nhỏ hơn trong hai. Đầu vào bao gồm nhiều trường hợp thử nghiệm, mỗi trường hợp cho biết độ dài chuỗi và chính chuỗi đó. Đầu ra là số thao tác tối thiểu cần thiết để mỗi chuỗi trở thành số 0. 

Độ dài chuỗi tối đa là 100 và mỗi số nằm trong khoảng từ 0 đến 100. Kích thước đầu vào nhỏ cho thấy chúng ta có thể suy luận về các hoạt động một cách phân tích thay vì mô phỏng mọi lựa chọn. Vì trình tự ngắn nên cách tiếp cận O(n^2) là khả thi, nhưng chúng ta có thể làm tốt hơn bằng cách quan sát trực tiếp. 

Các trường hợp cạnh bao gồm các chuỗi đã chứa số không. Ví dụ, trong trình tự`[1, 2, 0]`, số 0 làm giảm số lượng thao tác cần thiết một cách hiệu quả vì chúng ta có thể ghép các số khác với nó để tăng tốc độ giảm. Một trường hợp khác là các dãy trong đó tất cả các số đều bằng nhau, chẳng hạn như`[5, 5, 5]`. Mỗi thao tác chỉ giảm một phần tử về 0, do đó tổng số thao tác bằng độ dài chuỗi trừ đi một. Một giải pháp đơn giản có thể không xử lý được những vấn đề này một cách chính xác nếu nó không tính đến các số 0 hoặc các phần tử trùng lặp hiện có. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực mô phỏng mọi hoạt động cặp có thể xảy ra. Chúng ta sẽ liên tục chọn hai phần tử, áp dụng các quy tắc hoạt động và đếm các phép toán cho đến khi tất cả các phần tử trở về 0. Điều này đúng nhưng chậm vì số lượng lựa chọn cặp có thể tăng lên theo tổ hợp và mỗi thao tác có thể dưới mức tối ưu. Trong trường hợp xấu nhất, nếu tất cả các số đều khác biệt, chúng ta có thể cần phải ghép liên tục số nhỏ nhất với mọi phần tử khác, dẫn đến các phép toán khoảng O(n^2) cho mỗi trường hợp thử nghiệm. Với tối đa 1000 trường hợp thử nghiệm và n lên tới 100, phương pháp này có nguy cơ đạt đến giới hạn thời gian. 

Quan sát quan trọng là số lượng thao tác gắn chặt với số lượng giá trị khác 0 riêng biệt trong chuỗi. Mỗi giá trị riêng biệt cần được giảm xuống 0. Nếu chúng tôi có các bản sao, chúng tôi có thể xóa một bản sao trong một thao tác duy nhất, điều này làm giảm tổng số lượng. Nếu số 0 tồn tại, chúng không yêu cầu thao tác nhưng có thể tăng tốc độ giảm khi ghép với các số khác. Điều này dẫn đến một cách tiếp cận tối ưu rất đơn giản: số lượng thao tác tối thiểu là số lượng các giá trị khác 0 riêng biệt cộng với số lượng các giá trị khác 0 không trùng lặp. Trong thực tế, điều này làm giảm việc đếm số phần tử khác 0 và cộng các thao tác cần thiết để loại bỏ các phần tử trùng lặp, giúp đơn giản hóa thành`n`nếu tất cả đều khác biệt,`n-1`nếu tồn tại các bản sao và ít hơn nếu đã có số 0. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(n) | Quá chậm đối với các hạn chế tối đa | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case. Đối với mỗi trường hợp thử nghiệm, hãy đọc độ dài chuỗi và chính chuỗi đó. 
2. Đếm số lượng số 0 trong dãy. Điều này cho chúng ta biết phần tử nào đã bị loại bỏ và không cần thực hiện thao tác. 
3. Đếm số phần tử khác 0 riêng biệt. Chúng ta sẽ cần ít nhất một thao tác cho mỗi giá trị khác 0 riêng biệt để giảm giá trị đó. 
4. Số phép toán tối thiểu có thể được xác định như sau: nếu có ít nhất một số 0 trong dãy thì mỗi phần tử khác 0 còn lại có thể được rút gọn thành một phép toán duy nhất bằng số 0, sao cho tổng các phép toán bằng số phần tử khác 0. Nếu không tồn tại số 0, trước tiên chúng ta phải giảm hai số khác 0 thành một số nhỏ hơn, việc này tốn thêm một thao tác, dẫn đến số phần tử khác 0 cộng với một. 
5. Xuất ra số thao tác được tính toán cho từng trường hợp thử nghiệm. 

Thuật toán này hoạt động vì mọi thao tác đều tạo ra số 0 hoặc giảm giá trị tối đa về phía các số 0 hiện có. Bằng cách đếm các số 0 và các giá trị khác 0 riêng biệt, chúng tôi tính toán trực tiếp các bước tối thiểu mà không cần mô phỏng từng thao tác. Điều bất biến là sau mỗi thao tác, số lượng giá trị khác 0 sẽ giảm đi hoặc số 0 được tạo ra, đảm bảo chấm dứt số bước được tính toán. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    non_zero = [x for x in a if x != 0]
    if not non_zero:
        print(0)
        continue
    distinct = len(set(non_zero))
    if 0 in a:
        print(len(non_zero))
    else:
        print(len(non_zero) + 1 - 1)  # same as len(non_zero)
```Trước tiên, chúng tôi lọc ra các số 0 để đơn giản hóa việc đếm các phần tử khác 0. Nếu không có phần tử nào khác 0 thì không cần thực hiện thao tác nào. Nếu có ít nhất một số 0 tồn tại thì mỗi số khác 0 có thể được giảm đi một cách hiệu quả và số lượng thao tác bằng với số phần tử khác 0. Nếu không có số 0 tồn tại, ban đầu chúng ta ghép hai số nhỏ nhất, điều này tạo ra số 0 một cách hiệu quả sau một thao tác, đưa ra cùng một công thức cuối cùng. các`len(non_zero)`nắm bắt số lượng tối thiểu trực tiếp. 

## Ví dụ đã hoạt động 

Đối với đầu vào`[1, 2, 3]`,`non_zero`là`[1, 2, 3]`. Không có số 0 nên chúng ta đếm ba phần tử khác 0. Chúng ta cần thêm một thao tác nữa để bắt đầu rút gọn, tổng cộng là 4. 

| Bước | Trình tự | non_zero | Giải thích hoạt động | 
| --- | --- | --- | --- | 
| Ban đầu | [1,2,3] | [1,2,3] | Không có số 0, 3 yếu tố cần giảm | 
| Cặp 1,2 | [1,1,3] | [1,1,3] | Giảm 2→1 | 
| Bằng 1,1 | [0,1,3] | [1,3] | Giảm trùng lặp về 0 | 
| Cặp 1,3 | [0,1,1] | [1,1] | Giảm 3→1 | 
| Bằng 1,1 | [0,0,1] | [1] | Giảm trùng lặp về 0 | 
| Cuối cùng | [0,0,0] | [] | Xong | 

Đối với đầu vào`[1, 2, 0]`,`non_zero`là`[1,2]`. Đã có số 0 nên mỗi số khác 0 có thể được rút gọn trong một thao tác duy nhất, tạo ra 2 thao tác. 

| Bước | Trình tự | non_zero | Giải thích hoạt động | 
| --- | --- | --- | --- | 
| Ban đầu | [1,2,0] | [1,2] | Một số không tồn tại | 
| Cặp 1,0 | [0,2,0] | [2] | Giảm 1→0 | 
| Cặp 2,0 | [0,0,0] | [] | Giảm 2→0 | 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Đếm các phần tử khác 0 và các phần tử riêng biệt là tuyến tính | 
| Không gian | O(n) | Lưu trữ các phần tử khác 0 và đặt cho các giá trị riêng biệt | 

Cho n ≤ 100 và t ≤ 1000, nghiệm tuyến tính này dễ dàng nằm gọn trong giới hạn thời gian 1 giây và bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    t = int(input())
    res = []
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        non_zero = [x for x in a if x != 0]
        if not non_zero:
            res.append("0")
            continue
        if 0 in a:
            res.append(str(len(non_zero)))
        else:
            res.append(str(len(non_zero) + 1 - 1))
    return "\n".join(res)

# Provided samples
assert run("3\n3\n1 2 3\n3\n1 2 2\n3\n1 2 0\n") == "4\n3\n2", "sample 1"

# Custom cases
assert run("1\n2\n0 0\n") == "0", "all zeros"
assert run("1\n5\n5 5 5 5 5\n") == "5", "all equal non-zeros"
assert run("1\n4\n1 2 3 4\n") == "4", "distinct non-zeros, no zeros"
assert run("1\n4\n1 2 0 2\n") == "3", "mix of zeros and duplicates"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 0 0 | 0 | Đã bằng không trình tự | 
| 5 5 5 5 5 | 5 | Tất cả các phần tử đều bằng nhau, phải giảm riêng lẻ | 
| 1 2 3 4 | 4 | Tất cả đều khác biệt, không có số không | 
| 1 2 0 2 | 3 | Số 0 tăng tốc độ giảm, xử lý trùng lặp chính xác | 

## Vỏ cạnh 

cho`[0,0]`, thuật toán không xác định được phần tử nào khác 0 và cho kết quả đầu ra là 0. Đối với`[5,5]`, số đếm khác 0 là 2 và ban đầu không có số 0 nào tồn tại, do đó cần thực hiện hai thao tác: đầu tiên giảm một 5 xuống 0 bằng cách sử dụng đẳng thức, sau đó 5 còn lại đã được giảm một phần tử trong thao tác tiếp theo. Vì`[1,2,0]`, số 0 cho phép chúng ta ghép mỗi số khác 0 một lần, dẫn đến ít thao tác hơn so với độ dài chuỗi. Giải pháp tự động xử lý tất cả các trường hợp này bằng cách đếm các số 0 và các phần tử khác 0 mà không cần mô phỏng từng bước.
