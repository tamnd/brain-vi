---
title: "CF 441A - Valera và đồ cổ"
description: "Valera muốn mua chính xác một món đồ cổ từ một nhóm người bán. Mỗi người bán cung cấp nhiều mặt hàng với giá đấu giá hiện tại. Valera chỉ có thể đảm bảo một thỏa thuận nếu anh ta đưa ra mức giá cao hơn mức giá hiện tại của một mặt hàng và anh ta có ngân sách cố định v."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 441
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 252 (Div. 2)"
rating: 1000
weight: 441
solve_time_s: 81
verified: true
draft: false
---

[CF 441A - Valera và Đồ cổ](https://codeforces.com/problemset/problem/441/A) 

**Đánh giá:** 1000 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 21s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Valera muốn mua chính xác một món đồ cổ từ một nhóm người bán. Mỗi người bán cung cấp nhiều mặt hàng với giá đấu giá hiện tại. Valera chỉ có thể đảm bảo một thỏa thuận nếu anh ta đưa ra mức giá cao hơn mức giá hiện tại của một mặt hàng và anh ta có ngân sách cố định.`v`. Mục tiêu là tìm tất cả người bán mà anh ta có thể mua thành công ít nhất một mặt hàng. 

Đầu vào cung cấp số lượng người bán`n`và ngân sách của Valera`v`. Đối với mỗi người bán, chúng tôi nhận được số lượng mặt hàng họ có và danh sách giá hiện tại của họ. Đầu ra phải cho biết có bao nhiêu người bán mà Valera có thể giao dịch, theo sau là các chỉ số dựa trên 1 của họ theo thứ tự tăng dần. 

Các hạn chế là nhỏ:`n`và số lượng mặt hàng trên mỗi người bán`k_i`nhiều nhất là 50. Giá cả và ngân sách của Valera lên tới 10^6. Điều này ngụ ý rằng việc tìm kiếm đơn giản trên tất cả các mặt hàng cho mỗi người bán là khả thi vì tổng số lần kiểm tra giá trong trường hợp xấu nhất là 50 × 50 = 2500, một con số không đáng kể đối với các CPU hiện đại trong vòng chưa đầy một giây. 

Một trường hợp khó nhận thấy là khi tất cả các mặt hàng từ người bán đều có giá cao hơn ngân sách của Valera. Một cách tiếp cận ngây thơ cho rằng bất kỳ người bán nào có mặt hàng đều có thể được chọn sẽ bao gồm chúng một cách không chính xác. Một trường hợp khác là khi nhiều mặt hàng rẻ hơn`v`; mỗi người bán chỉ có một lần trả giá cao hơn thành công là đủ, nhưng việc thực hiện bất cẩn có thể bị tính nhiều lần. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực về cơ bản cũng là phương pháp tối ưu do kích thước đầu vào nhỏ. Chúng tôi có thể lặp lại từng người bán, sau đó đến từng mặt hàng của họ, kiểm tra xem giá có thấp hơn ngân sách của Valera hay không. Mặt hàng đầu tiên thỏa mãn điều kiện này xác nhận rằng Valera có thể giao dịch với người bán này và chúng tôi có thể chuyển sang người bán tiếp theo. Điều này đúng vì bài toán chỉ yêu cầu mỗi người bán có ít nhất một mặt hàng có giá cả phải chăng và mặt hàng chính xác được chọn không quan trọng. Trong trường hợp xấu nhất, chúng tôi thực hiện`n * k_i`so sánh, tối đa là 2500 cho đầu vào lớn nhất. 

Quan sát cho phép giải quyết vấn đề này một cách hiệu quả là chúng ta không cần phải theo dõi tất cả các mục bên dưới`v`cho mỗi người bán; chúng ta chỉ cần phát hiện sự tồn tại của một. Điều này tránh được việc tính toán không cần thiết và làm cho giải pháp trở nên đơn giản và hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu / Tối ưu | O(n * max k_i) = O(2500) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`Và`v`từ đầu vào. 
2. Khởi tạo danh sách trống`deal_sellers`để lưu trữ chỉ số của người bán mà Valera có thể thực hiện giao dịch. 
3. Đối với mỗi người bán`i`từ 1 đến`n`: 

1. Đọc số mục`k_i`và giá cả của chúng. 
2. Kiểm tra tuần tự giá của từng mặt hàng. 
3. Nếu giá của một mặt hàng thấp hơn`v`, nối thêm`i`ĐẾN`deal_sellers`và phá vỡ vòng lặp cho người bán này. 
4. In số lượng người bán trong`deal_sellers`. 
5. In chỉ số của những người bán này theo thứ tự tăng dần. 

Tại sao nó hoạt động: điều bất biến là bất cứ khi nào người bán được thêm vào`deal_sellers`, Valera có thể mua được ít nhất một món đồ từ người bán đó. Việc lặp lại từng mặt hàng đảm bảo không bỏ sót người bán nào và việc hủy sớm sẽ tránh được việc kiểm tra dư thừa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, v = map(int, input().split())
deal_sellers = []

for i in range(1, n + 1):
    data = list(map(int, input().split()))
    k_i, prices = data[0], data[1:]
    for price in prices:
        if price < v:
            deal_sellers.append(i)
            break

print(len(deal_sellers))
print(' '.join(map(str, deal_sellers)))
```Mã đọc số lượng người bán và ngân sách của Valera. Đối với mỗi người bán, nó sẽ đọc các mặt hàng của họ và kiểm tra xem ít nhất một mặt hàng có giá cả phải chăng hay không. Khi một mặt hàng có giá cả phải chăng được tìm thấy, nó sẽ ghi lại chỉ số của người bán và tiếp tục. Đầu ra in tổng số người bán và chỉ số của họ, khớp với định dạng được yêu cầu. 

## Ví dụ đã hoạt động 

Mẫu 1: 

đầu vào:```
3 50000
1 40000
2 20000 60000
3 10000 70000 190000
```| người bán | giá | mục < v? | thỏa thuận_người bán | 
| --- | --- | --- | --- | 
| 1 | 40000 | vâng | [1] | 
| 2 | 20000, 60000 | có (20000) | [1, 2] | 
| 3 | 10000, 70000, 190000 | có (10000) | [1, 2, 3] | 

Đầu ra:```
3
1 2 3
```Dấu vết này xác nhận rằng thuật toán xác định chính xác mặt hàng đầu tiên mà Valera có thể mua được cho mỗi người bán. 

Mẫu 2: 

đầu vào:```
2 10000
2 20000 30000
1 15000
```| người bán | giá | mục < v? | thỏa thuận_người bán | 
| --- | --- | --- | --- | 
| 1 | 20000, 30000 | không | [] | 
| 2 | 15000 | không | [] | 

Đầu ra:```
0
```Điều này xác nhận thuật toán xử lý chính xác trường hợp khi không thể thực hiện được giao dịch nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n * max k_i) | Chúng tôi kiểm tra từng mặt hàng của từng người bán cho đến khi tìm được mặt hàng có giá cả phải chăng. | 
| Không gian | O(n) | Chúng tôi lưu trữ các chỉ số của người bán mà Valera có thể giao dịch. | 

Cho n 50 và k_i 50, tổng số phép toán dưới 2500, dễ dàng khớp trong vòng 1 giây. Mức sử dụng bộ nhớ không đáng kể so với giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, v = map(int, input().split())
    deal_sellers = []
    for i in range(1, n + 1):
        data = list(map(int, input().split()))
        k_i, prices = data[0], data[1:]
        for price in prices:
            if price < v:
                deal_sellers.append(i)
                break
    out = f"{len(deal_sellers)}\n{' '.join(map(str, deal_sellers))}"
    return out

# Provided samples
assert run("3 50000\n1 40000\n2 20000 60000\n3 10000 70000 190000\n") == "3\n1 2 3", "sample 1"
assert run("2 10000\n2 20000 30000\n1 15000\n") == "0\n", "sample 2"

# Custom cases
assert run("1 5000\n1 4999\n") == "1\n1", "minimum-size input"
assert run("2 1000000\n50 " + " ".join(str(x) for x in range(1000000, 1000050)) + "\n50 " + " ".join(str(x) for x in range(500000, 500050)) + "\n") == "1\n2", "max-size inputs"
assert run("3 10000\n2 10000 10000\n2 9999 10001\n2 10001 10002\n") == "1\n2", "all-equal prices and boundary check"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 5000\n1 4999 | 1\n1 | Đầu vào kích thước tối thiểu | 
| 2 người bán với các mặt hàng có kích thước tối đa | 1\n2 | Xử lý kích thước đầu vào lớn nhất có thể | 
| Giá bình đẳng và giá biên | 1\n2 | Xử lý đúng ranh giới ngân sách chính xác và trùng lặp | 

## Vỏ cạnh 

Nếu tất cả các mặt hàng của một người bán đều đắt hơn ngân sách của Valera thì thuật toán sẽ không bao giờ thêm người bán đó vào. Ví dụ: với đầu vào:```
2 10000
1 15000
2 12000 13000
```Dấu vết: 

| người bán | giá | mục < v? | thỏa thuận_người bán | 
| --- | --- | --- | --- | 
| 1 | 15000 | không | [] | 
| 2 | 12000, 13000 | không | [] | 

Đầu ra:```
0
```Điều này xác nhận rằng những người bán có mặt hàng giá quá cao đã bị loại trừ một cách chính xác. Thuật toán kiểm tra từng mục và chỉ ngắt khi tìm thấy mục hợp lệ, ngăn chặn kết quả dương tính giả.
