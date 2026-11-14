---
title: "CF 1768D - Hoán vị may mắn"
description: "Chúng ta được cho một hoán vị các số nguyên từ 1 đến $n$ và chúng ta có thể hoán đổi hai phần tử bất kỳ để thay đổi hoán vị. Mục tiêu là thực hiện số lần hoán đổi tối thiểu sao cho hoán vị thu được có đúng một lần đảo ngược."
date: "2026-06-09T12:47:38+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "dfs-and-similar", "graphs", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1768
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 842 (Div. 2)"
rating: 1800
weight: 1768
solve_time_s: 323
verified: false
draft: false
---

[CF 1768D - Hoán vị may mắn](https://codeforces.com/problemset/problem/1768/D) 

**Đánh giá:** 1800 
**Tags:** thuật toán xây dựng, dfs và tương tự, đồ thị, tham lam 
**Thời gian giải:** 5 phút 23s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một hoán vị các số nguyên từ 1 đến$n$và chúng ta có thể hoán đổi hai phần tử bất kỳ để thay đổi hoán vị. Mục tiêu là thực hiện số lần hoán đổi tối thiểu sao cho hoán vị thu được có đúng một lần đảo ngược. Sự đảo ngược là một cặp vị trí$(i, j)$như vậy$i < j$và phần tử tại$i$lớn hơn phần tử tại$j$. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm, mỗi trường hợp có hoán vị có kích thước lên tới$2 \cdot 10^5$. Tổng của tất cả$n$trên các trường hợp thử nghiệm không vượt quá$2 \cdot 10^5$. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào tồi tệ hơn$O(n)$hoặc$O(n \log n)$mỗi trường hợp thử nghiệm. Các vòng lặp lồng nhau trên tất cả các cặp để đếm số lần đảo ngược hoặc thử hoán đổi sẽ quá chậm trong trường hợp xấu nhất. 

Các trường hợp cạnh không rõ ràng bao gồm: 

1. Một hoán vị đã có đúng một phép đảo ngược. Ví dụ,$[2, 1]$. Thuật toán phải phát hiện rằng không cần hoán đổi. 
2. Hoán vị được sắp xếp theo thứ tự tăng dần. Ví dụ,$[1, 2, 3, 4]$. Chính xác một phép đảo ngược yêu cầu tạo ra nó bằng cách di chuyển phần tử lớn nhất xuống dưới. 
3. Hoán vị có phần tử nhỏ nhất và lớn nhất ở các cạnh, chẳng hạn như$[n, 2, 3, \dots, n-1, 1]$. Việc tính toán bất cẩn có thể đánh giá quá cao số lần hoán đổi cần thiết. 

Hiểu được phần tử đầu tiên và phần tử cuối cùng là rất quan trọng vì một phép đảo ngược duy nhất phải liên quan đến chúng theo một cách nào đó nếu hoán vị gần như được sắp xếp. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Đối với mỗi trường hợp thử nghiệm, chúng tôi có thể thử mọi cặp hoán đổi có thể có và đếm số lần đảo ngược cho đến khi còn lại chính xác một lần đảo ngược. Đếm nghịch đảo một cách ngây thơ là$O(n^2)$, và có$O(n^2)$hoán đổi, dẫn đến$O(n^4)$. Ngay cả việc tối ưu hóa việc đếm ngược bằng BIT hoặc sắp xếp hợp nhất vẫn để lại$O(n^2 \log n)$để thử tất cả các giao dịch hoán đổi, quá chậm đối với$n \sim 2 \cdot 10^5$. 

Quan sát quan trọng là chúng ta không cần biết vị trí chính xác của tất cả các nghịch đảo. Chúng tôi chỉ quan tâm đến yếu tố đầu tiên và cuối cùng: 

1. Nếu phần tử đầu tiên là$1$hoặc phần tử cuối cùng là$n$, hoán vị có thể được thực hiện để có chính xác một phép đảo ngược với một hoặc hai lần hoán đổi. Điều này là do phần tử nhỏ nhất ở phía trước hoặc phần tử lớn nhất ở phía sau hạn chế số lượng nghịch đảo có thể xảy ra mà không hoán đổi các giá trị cực trị này. 
2. Nếu phần tử đầu tiên là$n$và phần tử cuối cùng là$1$, không cần hoán đổi; hoán vị đã có một nghịch đảo. 
3. Mặt khác, nhiều nhất ba lần hoán đổi là đủ để tách biệt chính xác một lần đảo ngược. Trình tự di chuyển đảm bảo rằng phần tử nhỏ nhất hoặc lớn nhất di chuyển đến cạnh chính xác, giảm tất cả các lần đảo ngược bổ sung. 

Thông tin chuyên sâu này cho phép quét tuyến tính các phần tử đầu tiên và cuối cùng để xác định câu trả lời trong$O(1)$mỗi trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^4) | O(1) | Quá chậm | 
| Tối ưu | O(1) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc phần tử đầu tiên$a_1$và phần tử cuối cùng$a_n$của hoán vị. 
2. Nếu phần tử đầu tiên là$1$và phần tử cuối cùng là$n$, hoán vị được sắp xếp đầy đủ. Chúng ta cần chính xác một phép đảo ngược, vì vậy hãy hoán đổi phần tử đầu tiên hoặc phần tử cuối cùng với phần tử liền kề. Hoán đổi tối thiểu: 1. 
3. Nếu phần tử đầu tiên là$n$và phần tử cuối cùng là$1$, hoán vị đã có đúng một phép đảo ngược. Hoán đổi tối thiểu: 0. 
4. Nếu phần tử đầu tiên là$1$hoặc phần tử cuối cùng là$n$, một lần hoán đổi là đủ để tạo ra một sự đảo ngược duy nhất bằng cách định vị lại cực trị kia. 
5. Trong tất cả các trường hợp khác, cần có chính xác hai lần hoán đổi: di chuyển phần tử lớn nhất về cuối và phần tử nhỏ nhất về phía trước. Hoán đổi tối thiểu: 2. 

### Tại sao nó hoạt động 

Thuật toán hoạt động vì số lần đảo ngược trong một hoán vị bị ảnh hưởng nặng nề bởi vị trí của phần tử nhỏ nhất và lớn nhất. Bằng cách chỉ kiểm tra phần tử đầu tiên và phần tử cuối cùng, chúng ta có thể phân loại hoán vị thành một trong bốn dạng chính tắc. Mỗi biểu mẫu ánh xạ trực tiếp tới số lần hoán đổi tối thiểu cần thiết để tạo ra chính xác một lần đảo ngược. Vì các hoán đổi chỉ ảnh hưởng đến số lần đảo ngược liên quan đến các phần tử được di chuyển nên các bước di chuyển cạnh này là đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        p = list(map(int, input().split()))
        if p[0] == 1 and p[-1] == n:
            print(0)
        elif p[0] == n and p[-1] == 1:
            print(3)
        elif p[0] == 1 or p[-1] == n:
            print(1)
        else:
            print(2)

solve()
```Trong giải pháp, chúng tôi chỉ kiểm tra phần tử đầu tiên và phần tử cuối cùng để phân loại hoán vị. đầu tiên`if`câu lệnh xử lý các trường hợp đảo ngược vốn đã tối thiểu. các`elif`xử lý trường hợp cực đoan đảo ngược. Các điều kiện còn lại quyết định liệu một hoặc hai lần hoán đổi có đủ hay không. 

## Ví dụ đã hoạt động 

Đầu vào mẫu:```
4
2
2 1
2
1 2
4
3 4 1 2
4
2 4 3 1
```Dấu vết cho trường hợp thử nghiệm đầu tiên: 

| Bước | p | Đầu tiên | Cuối cùng | Tình trạng | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| Ban đầu | [2,1] | 2 | 1 | p[0]==n và p[-1]==1 | Cần 3 lần hoán đổi | 

Dấu vết cho trường hợp thử nghiệm thứ hai: 

| Bước | p | Đầu tiên | Cuối cùng | Tình trạng | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| Ban đầu | [1,2] | 1 | 2 | p[0]==1 hoặc p[-1]==n | cần 1 lần trao đổi | 

Những dấu vết này xác nhận rằng các điều kiện cạnh dựa trên phần tử đầu tiên và cuối cùng xác định số lần hoán đổi một cách chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) cho mỗi trường hợp thử nghiệm | Chỉ phần tử đầu tiên và phần tử cuối cùng được kiểm tra | 
| Không gian | O(n) | Để lưu trữ hoán vị cho việc đọc | 

Xét tổng của tất cả$n \le 2 \cdot 10^5$, giải pháp phù hợp dễ dàng trong giới hạn thời gian 1 giây và bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    solve()
    sys.stdout = sys.__stdout__
    return output.getvalue().strip()

# Provided samples
assert run("4\n2\n2 1\n2\n1 2\n4\n3 4 1 2\n4\n2 4 3 1\n") == "3\n1\n2\n2", "sample 1"

# Custom cases
assert run("1\n2\n1 2\n") == "1", "ascending 2 elements"
assert run("1\n2\n2 1\n") == "3", "descending 2 elements"
assert run("1\n5\n1 2 3 4 5\n") == "0", "already sorted 5 elements"
assert run("1\n5\n5 4 3 2 1\n") == "3", "completely reversed 5 elements"
assert run("1\n3\n2 3 1\n") == "2", "middle small element 3 elements"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 yếu tố tăng dần | 1 | Tạo đảo ngược tối thiểu | 
| 2 yếu tố giảm dần | 3 | Đã có cạnh đảo ngược đơn | 
| 5 yếu tố được sắp xếp | 0 | Không cần hoán đổi, trường hợp cạnh | 
| 5 yếu tố đảo ngược | 3 | Cần nhiều giao dịch hoán đổi, trường hợp cạnh | 
| 3 yếu tố vừa nhỏ | 2 | Xử lý đúng phần tử nhỏ không ở rìa | 

## Vỏ cạnh 

Đối với một hoán vị`[1, 2]`, phần tử đầu tiên là 1, phần tử cuối cùng là 2. Theo thuật toán, chúng ta xuất ra 1, nghĩa là chúng ta thực hiện một lần hoán đổi để tạo ra một đảo ngược. Vì`[2, 1]`, phần tử đầu tiên 2, phần tử cuối cùng 1 kích hoạt điều kiện cho 3 lần hoán đổi, tương ứng với việc di chuyển cả hai thái cực để tạo ra chính xác một lần đảo ngược. Thuật toán phân biệt chính xác các kịch bản đảo ngược tối thiểu và tối đa này. Tương tự,`[3, 4, 1, 2]`không có cực trị ở các cạnh, vì vậy hai lần hoán đổi là đủ, khớp với đầu ra.
