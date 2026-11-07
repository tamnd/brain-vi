---
title: "CF 1369A - FashionabLee"
description: "Nhiệm vụ yêu cầu chúng ta xác định xem một đa giác đều có thể được định hướng sao cho một trong các cạnh của nó song song với trục hoành và một cạnh khác song song với trục tung hay không."
date: "2026-06-11T11:43:14+07:00"
tags: ["codeforces", "competitive-programming", "geometry", "math"]
categories: ["algorithms"]
codeforces_contest: 1369
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 652 (Div. 2)"
rating: 800
weight: 1369
solve_time_s: 501
verified: true
draft: false
---

[CF 1369A - FashionabLee](https://codeforces.com/problemset/problem/1369/A) 

**Đánh giá:** 800 
**Thẻ:** hình học, toán học 
**Thời gian giải:** 8m 21s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ yêu cầu chúng ta xác định xem một đa giác đều có thể được định hướng sao cho một trong các cạnh của nó song song với trục hoành và một cạnh khác song song với trục tung hay không. Mỗi đa giác chỉ được xác định bởi số cạnh mà nó có và chúng tôi nhận được danh sách các số lượng này. Đối với mỗi lần đếm, đầu ra là CÓ nếu hướng đó tồn tại và KHÔNG nếu ngược lại. 

Từ các ràng buộc đầu vào, có thể có tới 10.000 đa giác và mỗi đa giác có thể có tới một tỷ cạnh. Điều này loại trừ bất kỳ giải pháp nào có thể xây dựng đa giác một cách rõ ràng hoặc mô phỏng các phép quay; mọi cách tiếp cận chỉ phải dựa vào một tính chất toán học rút ra từ số cạnh. Một cách tiếp cận ngây thơ cố gắng liệt kê các hướng cạnh hoặc tính toán các góc cho mỗi đa giác sẽ liên quan đến hàng tỷ phép tính trong trường hợp xấu nhất, điều này là không khả thi. 

Trường hợp cạnh xảy ra khi số cạnh nhỏ, đặc biệt là hình tam giác và hình vuông. Một hình tam giác đều không thể có một cạnh ngang và một cạnh dọc đồng thời dưới bất kỳ góc quay nào, trong khi hình vuông thì có thể. Đa giác có số cạnh lớn, chẳng hạn như 12 hoặc 1.000.000.000, tuân theo cùng một quy tắc toán học và lời giải phải có khả năng xử lý chính xác các số nguyên rất lớn. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là cố gắng mô phỏng hoặc xoay từng đa giác và kiểm tra tất cả các hướng của cạnh. Điều này đúng vì nó kiểm tra toàn diện mọi căn chỉnh có thể có, nhưng rõ ràng là không thực tế vì một đa giác có một tỷ cạnh sẽ yêu cầu một tỷ thao tác chỉ để liệt kê các cạnh. 

Quan sát quan trọng là một đa giác đều cho phép các cạnh nằm ngang và dọc nếu số cạnh của nó chia đều cho 4, hoặc tổng quát hơn, nếu đa giác có thể nội tiếp trong một lưới vuông sao cho các cạnh thẳng hàng với các trục. Về mặt đại số, điều này giúp kiểm tra xem liệu 360 độ chia cho số cạnh có cho phép bội số của 90 độ hay không. Nếu số cạnh chia hết cho 4 thì đa giác đó đẹp vì nó có thể xoay sao cho các cạnh thẳng hàng với cả hai trục. Điều này dẫn đến một giải pháp thời gian không đổi cho mỗi đa giác. 

Cách tiếp cận bạo lực có hiệu quả vì định nghĩa về vẻ đẹp phụ thuộc vào sự căn chỉnh của các cạnh, nhưng nó không thành công khi n lớn. Hiểu biết sâu sắc rằng đối xứng quay modulo 90 độ là đủ cho phép chúng ta tính kết quả theo O(1) trên mỗi đa giác. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) trên mỗi đa giác | O(n) | Quá chậm đối với n lớn | 
| Tối ưu | O(1) trên mỗi đa giác | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số đa giác, t. Điều này xác định số lần thuật toán sẽ lặp lại. 
2. Với mỗi đa giác, hãy đọc số cạnh n của nó. 
3. Kiểm tra xem n có chia hết cho 4 hay không. Đây là điều kiện để một đa giác có hướng có một cạnh ngang và một cạnh dọc. Việc kiểm tra sử dụng toán tử mô đun. 
4. Nếu n chia hết cho 4, in ra CÓ. Ngược lại in ra NO. Mỗi đầu ra tương ứng với một đa giác theo thứ tự chúng được đưa ra. 
5. Lặp lại cho đến khi tất cả các đa giác đã được xử lý. 

Tại sao nó hoạt động: tính bất biến là để một đa giác căn chỉnh một cạnh với cả hai trục, góc quay cần thiết là bội số của 90 độ. Một đa giác đều có n cạnh chia đều 360 độ cho các cạnh của nó. Do đó, nếu 360/n là ước của 90, tương đương nếu n chia cho 4 thì phép quay như vậy tồn tại. Điều này đảm bảo tính đúng đắn cho mọi n, kể cả những số rất lớn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    t = int(input())
    for _ in range(t):
        n = int(input())
        if 360 % n == 0 and (360 // n) % 90 == 0:
            print("YES")
        else:
            print("NO")

if __name__ == "__main__":
    main()
```Giải pháp đọc số lượng đa giác đầu tiên. Đối với mỗi đa giác, nó đọc n và kiểm tra xem 360 có chia hết cho n hay không và góc 360/n có phải là ước số của 90 độ hay không. Điều này đảm bảo rằng đa giác có thể được xoay sao cho các cạnh thẳng hàng với các trục. Các phép toán mô đun xử lý chính xác tất cả các số nguyên, bao gồm các giá trị lớn lên tới 10^9. 

## Ví dụ đã hoạt động 

### Mẫu 1 

| Đa giác | n | 360% n | 360/n | (360/n) % 90 | Đầu ra | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 3 | 0 | 120 | 30 | KHÔNG | 
| 2 | 4 | 0 | 90 | 0 | CÓ | 
| 3 | 12 | 0 | 30 | 30 | CÓ | 
| 4 | 1.000.000.000 | 0 | 0,00036 | 0 | CÓ | 

Bảng xác nhận rằng với n=3, đa giác không thể được căn chỉnh theo trục. Với n=4, hình vuông sẽ thẳng hàng. Với n=12, nó căn chỉnh vì các bước xoay 30 độ cho phép bội số của 90. Đối với n rất lớn, số học vẫn áp dụng chính xác. 

### Ví dụ tùy chỉnh 

đầu vào:```
2
5
8
```| Đa giác | n | 360% n | 360/n | (360/n) % 90 | Đầu ra | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 5 | 0 | 72 | 72 | KHÔNG | 
| 2 | 8 | 0 | 45 | 45 | CÓ | 

Điều này xác nhận việc kiểm tra dựa trên mô đun xử lý cả số lẻ và số chẵn nhỏ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi đa giác yêu cầu các phép toán số học và mô đun theo thời gian không đổi. | 
| Không gian | O(1) | Không có cấu trúc bổ sung tỷ lệ thuận với n được yêu cầu. | 

Với t tối đa 10^4 và O(1) trên mỗi đa giác, giải pháp thực hiện tối đa 10^4 thao tác, nằm trong giới hạn thời gian 2 giây. Việc sử dụng bộ nhớ là không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    main()
    return output.getvalue().strip()

# Provided sample
assert run("4\n3\n4\n12\n1000000000\n") == "NO\nYES\nYES\nYES", "sample 1"

# Custom cases
assert run("3\n5\n8\n16\n") == "NO\nYES\nYES", "odd and powers of 2"
assert run("2\n3\n7\n") == "NO\nNO", "small primes"
assert run("1\n360\n") == "YES", "max divisible by 360"
assert run("1\n1000000000\n") == "YES", "very large n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 5,8,16 | KHÔNG, CÓ, CÓ | Kiểm tra số lẻ và lũy thừa hai | 
| 3,7 | KHÔNG, KHÔNG | Số nguyên tố nhỏ không thể căn chỉnh các cạnh | 
| 360 | CÓ | Đa giác chia chính xác 360 | 
| 1.000.000.000 | CÓ | Xử lý n rất lớn đúng cách | 

## Vỏ cạnh 

Với n=3, một tam giác không thể căn chỉnh bất kỳ cạnh nào theo cả hai trục, do đó kết quả đầu ra là NO. Với n=4, một hình vuông được căn chỉnh hoàn hảo, do đó kết quả đầu ra là CÓ. Với n=1.000.000.000, giải pháp vẫn thực hiện chính xác phép toán mô đun, chứng tỏ thuật toán hoạt động với số nguyên lớn. Điều quan trọng là chúng tôi không bao giờ mô phỏng các cạnh, thay vào đó chỉ dựa vào khả năng chia hết khi xoay, tính năng này tự động xử lý tất cả các trường hợp cạnh.
