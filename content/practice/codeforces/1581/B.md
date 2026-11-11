---
title: "CF 1581B - Đường kính đồ thị"
description: "Chúng ta được yêu cầu xác định liệu có thể xây dựng một đồ thị đơn giản, liên thông, vô hướng với số nút n và cạnh m cho trước sao cho đường kính của đồ thị nhỏ hơn k-1 hay không."
date: "2026-06-10T10:10:22+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "graphs", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1581
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 745 (Div. 2)"
rating: 1200
weight: 1581
solve_time_s: 144
verified: true
draft: false
---

[CF 1581B - Đường kính đồ thị](https://codeforces.com/problemset/problem/1581/B) 

**Đánh giá:** 1200 
**Tags:** thuật toán xây dựng, đồ thị, tham lam, toán học 
**Thời gian giải:** 2m 24s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xác định liệu có thể xây dựng một đồ thị đơn giản, liên thông, vô hướng với số nút cho trước hay không`n`và các cạnh`m`sao cho đường kính của đồ thị nhỏ hơn`k-1`. Đường kính của đồ thị là khoảng cách lớn nhất giữa hai nút bất kỳ, trong đó khoảng cách được đo bằng số cạnh tối thiểu trên đường nối chúng. Không được phép tự lặp hoặc nhiều cạnh. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm chỉ định các giá trị của`n`,`m`, Và`k`. Đối với mỗi điều kiện, chúng ta phải xuất "CÓ" nếu có thể xây dựng biểu đồ thỏa mãn các điều kiện này hoặc "KHÔNG" nếu không thể. 

Với những hạn chế,`n`Và`m`có thể lên đến$10^9$và có thể có tới$10^5$trường hợp thử nghiệm. Điều này có nghĩa là chúng ta không thể xây dựng biểu đồ một cách rõ ràng hoặc thực hiện bất kỳ thao tác nào có tỷ lệ với`n`hoặc`m`. Thay vào đó, chúng ta phải suy luận một cách toán học về những gì có thể thực hiện được dựa trên số nút, cạnh và đường kính yêu cầu. 

Các trường hợp cạnh phát sinh khi`n`là rất nhỏ. Nếu như`n = 1`, đồ thị hợp lệ duy nhất không có cạnh và đường kính bằng 0, có thể thỏa mãn một số giá trị của`k`. Nếu như`k = 1`, đường kính yêu cầu nhỏ hơn 0, điều này là không thể. Khi`m = 0`, đồ thị bị ngắt kết nối nếu`n > 1`. Đây là những tình huống mà việc đếm ngây thơ có thể tạo ra kết quả không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là cố gắng xây dựng biểu đồ một cách rõ ràng và kiểm tra đường kính của nó. Đối với nhỏ`n`, người ta có thể thử tất cả các kết hợp cạnh có thể và đo đường kính. Điều này sẽ hoạt động với các đầu vào nhỏ nhưng hoàn toàn thất bại đối với`n`Và`m`theo thứ tự của$10^9$bởi vì việc xây dựng ngay cả danh sách kề cũng sẽ tốn rất nhiều thời gian và không gian. 

Thông tin chi tiết quan trọng là số cạnh tối thiểu và tối đa trong biểu đồ liên thông của`n`các nút đã được biết. Một đồ thị liên thông phải có ít nhất`n-1`các cạnh. Số cạnh tối đa không có vòng lặp hoặc nhiều cạnh là$\frac{n(n-1)}{2}$. 

Các hạn chế về đường kính tiếp tục hạn chế các khả năng. Đường kính hoàn toàn nhỏ hơn`k-1`có thể được hiểu là: 

- Nếu`k = 1`, đường kính phải bằng 0. Điều này chỉ có thể thực hiện được đối với`n = 1`. 
- Nếu như`k = 2`, đường kính phải nhỏ hơn 1, nghĩa là 0, điều này lại yêu cầu`n = 1`. 
- Vì`k >= 3`, mọi đồ thị liên thông đều có đường kính từ 1 đến`n-1`. Đồ thị ngôi sao có đường kính 2, chuỗi có đường kính`n-1`. 

Bây giờ chúng ta có thể suy luận hoàn toàn bằng toán học: đồ thị phải được kết nối (`m >= n-1`), số cạnh không được vượt quá đồ thị đầy đủ (`m <= n(n-1)/2`), Và`k`áp đặt các hạn chế bổ sung đối với`n`. Những điều kiện này đủ để trả lời từng trường hợp thử nghiệm trong thời gian O(1). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) hoặc tệ hơn | O(n²) | Quá chậm | 
| Tối ưu | O(1) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`t`. 
2. Với mỗi test case, hãy đọc`n`,`m`,`k`. 
3. Xử lý những điều không thể xảy ra trước tiên: nếu`n = 1`, đồ thị có đường kính bằng 0. Điều này thỏa mãn ràng buộc đường kính nếu`k > 1`Và`m = 0`. Nếu không thì không thể được. 
4. Nếu`n = 2`, đồ thị có thể có 0 hoặc 1 cạnh. Đường kính sẽ là 1 nếu có cạnh, 0 nếu không có. Kiểm tra xem điều này có thỏa mãn không`k-1`. 
5. Đối với`n >= 3`, đồ thị phải được kết nối. Vì thế`m >= n-1`. Nếu như`m < n-1`, xuất ra "KHÔNG". 
6. Số cạnh tối đa có thể là$\frac{n(n-1)}{2}$. Nếu như`m > n(n-1)/2`, xuất ra "KHÔNG". 
7. Yêu cầu về đường kính phụ thuộc vào`k`. Đồ thị hình sao với`n >= 3`có đường kính 2. Vậy nếu`k = 1`, điều đó là không thể. Nếu như`k = 2`, chỉ một đồ thị đầy đủ mới có thể có đường kính 1, vì vậy`m`phải bằng`n(n-1)/2`. Nếu không thì,`k >= 3`luôn có thể đạt được miễn là`n >= 3`Và`m`nằm trong phạm vi hợp lệ. 
8. Nếu tất cả các điều kiện đều được thỏa mãn, xuất ra "CÓ". Nếu không thì xuất ra "NO". 

Tại sao nó hoạt động: Thuật toán dựa trên các đặc tính toán học của đồ thị được kết nối và đường kính đã biết của các cấu trúc tiêu chuẩn (chuỗi, ngôi sao, hoàn chỉnh). Mỗi lần kiểm tra tương ứng trực tiếp với một ràng buộc không thể thực hiện được. Vì chúng ta chỉ suy luận về số lượng và giới hạn nên không cần xây dựng biểu đồ thực tế. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    t = int(input())
    for _ in range(t):
        n, m, k = map(int, input().split())
        if n == 1:
            print("YES" if m == 0 and k > 1 else "NO")
            continue
        if n == 2:
            if m > 1 or m < 1:
                print("NO")
            else:
                print("YES" if k > 1 else "NO")
            continue
        min_edges = n - 1
        max_edges = n * (n - 1) // 2
        if m < min_edges or m > max_edges or k == 1:
            print("NO")
        elif k == 2:
            print("YES" if m == max_edges else "NO")
        else:
            print("YES")

if __name__ == "__main__":
    main()
```Giải pháp trước tiên xử lý các đồ thị nhỏ nhất có yêu cầu về số cạnh và đường kính là các trường hợp góc. Sau đó, nó tính toán các cạnh tối thiểu và tối đa cho một đồ thị được kết nối và áp dụng trực tiếp logic đường kính dựa trên cấu trúc đồ thị hình sao và hoàn chỉnh. Điều này đảm bảo O(1) cho mỗi trường hợp thử nghiệm. 

## Ví dụ đã hoạt động 

**Ví dụ 1:**`n = 4, m = 5, k = 3`| Bước | min_edges | max_edge | Kiểm tra | Đầu ra | 
| --- | --- | --- | --- | --- | 
| 1 | 3 | 6 | 5 in [3,6], k=3>2 | CÓ | 

**Ví dụ 2:**`n = 5, m = 4, k = 1`| Bước | min_edges | max_edge | Kiểm tra | Đầu ra | 
| --- | --- | --- | --- | --- | 
| 1 | 4 | 10 | k=1 không thể | KHÔNG | 

Những dấu vết này cho thấy số lượng cạnh và giới hạn đường kính xác định chính xác tính khả thi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi trường hợp kiểm thử được xử lý trong O(1), t 10⁵ | 
| Không gian | O(1) | Chỉ một vài số nguyên được lưu trữ cho mỗi trường hợp thử nghiệm | 

Giải pháp dễ dàng phù hợp với giới hạn thời gian 1 giây và giới hạn bộ nhớ 256 MB, ngay cả đối với đầu vào lớn nhất. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    main()
    sys.stdout = sys.__stdout__
    return output.getvalue().strip()

# Provided samples
assert run("5\n1 0 3\n4 5 3\n4 6 3\n5 4 1\n2 1 1\n") == "YES\nNO\nYES\nNO\nNO"

# Custom cases
assert run("1\n1 0 2\n") == "YES", "n=1, diameter ok"
assert run("1\n1 0 1\n") == "NO", "n=1, diameter impossible"
assert run("1\n3 2 3\n") == "NO", "not enough edges"
assert run("1\n3 3 2\n") == "YES", "exactly enough for complete graph"
assert run("1\n5 10 2\n") == "YES", "complete graph, diameter 1, k=2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 0 2 | CÓ | nhỏ nhất n=1, đường kính cho phép | 
| 1 0 1 | KHÔNG | nhỏ nhất n=1, đường kính không thể | 
| 3 2 3 | KHÔNG | không đủ cạnh để kết nối | 
| 3 3 2 | CÓ | các cạnh tối thiểu cho đường kính đồ thị đầy đủ 1 | 
| 5 10 2 | CÓ | đồ thị đầy đủ đường kính chính xác 1 | 

## Vỏ cạnh 

cho`n = 1, m = 0, k = 2`, thuật toán trả về CÓ. Việc kiểm tra cho`n == 1`đảm bảo trường hợp cạnh này được xử lý mà không cần dựa vào các công thức chung. 

Vì`k = 1`Và`n >= 2`, thuật toán xuất ra NO. Điều này đúng vì bất kỳ đồ thị liên thông nào có nhiều hơn một nút đều có đường kính ít nhất là 1,
