---
title: "CF 102788A - Hình vuông ma thuật thông thường"
description: "Một ma phương bình thường cấp n là một cách sắp xếp n × n chứa mọi số từ 1 đến n² đúng một lần. Tổng của mỗi hàng, mỗi cột và cả hai đường chéo chính đều có cùng giá trị. Nhiệm vụ không phải là xây dựng hình vuông."
date: "2026-07-27T18:19:57+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102788
codeforces_index: "A"
codeforces_contest_name: "2017-2018 ICPC Central Quarter Final of Northeastern European Regional Collegiate Programming Contest"
rating: 0
weight: 102788
solve_time_s: 39
verified: true
draft: false
---

[CF 102788A - Hình vuông ma thuật thông thường](https://codeforces.com/problemset/problem/102788/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 39s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Một hình vuông ma thuật bình thường có trật tự`n`là một`n × n`sắp xếp chứa mọi số từ`1`ĐẾN`n²`đúng một lần. Tổng của mỗi hàng, mỗi cột và cả hai đường chéo chính đều có cùng giá trị. Nhiệm vụ không phải là xây dựng hình vuông. Chúng ta chỉ cần tổng các số ở hàng đầu tiên của hình vuông đó. Đầu vào là độ dài cạnh của hình vuông và đầu ra là tổng ma thuật thông thường này. 

Đơn đặt hàng có thể lớn như`1000`, do đó, việc xây dựng toàn bộ hình vuông sẽ yêu cầu lưu trữ tới một triệu ô. Điều đó vẫn có thể thực hiện được ở một số ngôn ngữ, nhưng không cần thiết. Giải pháp dự định chỉ nên sử dụng tính chất toán học của hình vuông và kết thúc trong thời gian không đổi. Bất kỳ mô phỏng nào cố gắng đặt các giá trị hoặc xác minh các hàng và cột đều thực hiện công việc tỷ lệ thuận với`n²`, trong khi câu trả lời có thể được suy ra trực tiếp. 

Các trường hợp đặc biệt chính đến từ các đơn đặt hàng nhỏ và tính chẵn lẻ. Vì`n = 1`, hình vuông chỉ chứa số`1`, vậy câu trả lời là`1`. Một chương trình áp dụng công thức xây dựng một cách mù quáng mà không xử lý trường hợp này có thể thất bại. Ví dụ:```
Input:
1

Output:
1
```Thứ tự`2`không bao giờ xuất hiện vì hình vuông ma thuật bình thường có kích thước đó không tồn tại. Việc thực hiện bất cẩn mang lại mọi điều tích cực`n`hợp lệ có thể cố gắng xây dựng một hình vuông không thể. Các ràng buộc loại trừ trường hợp này, vì vậy chương trình chỉ cần đọc phạm vi đầu vào hợp lệ. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là thực sự tạo ra một hình vuông ma thuật bình thường và tính toán hàng đầu tiên. Đối với các mệnh lệnh lẻ, người ta có thể sử dụng cách xây dựng Xiêm, trong khi các mệnh lệnh khác yêu cầu cách xây dựng phức tạp hơn. Sau khi xây dựng`n × n`lưới, việc đọc hàng đầu tiên sẽ mất một hàng khác`O(n)`hoạt động. Vấn đề với cách tiếp cận này là nó giải quyết được một vấn đề khó hơn yêu cầu. Trường hợp xấu nhất với`n = 1000`tạo ra một triệu tế bào và thực hiện một lượng lớn công việc không cần thiết. 

Quan sát quan trọng là hàng đầu tiên có tổng bằng với mọi hàng khác. Toàn bộ hình vuông chứa các số`1`bởi vì`n²`, do đó tổng của tất cả các ô là chuỗi số học:$$1 + 2 + \dots + n^2 = \frac{n^2(n^2+1)}{2}$$Có chính xác`n`hàng và mỗi hàng có tổng bằng nhau. Chia tổng số tiền cho số hàng sẽ được số tiền kỳ diệu:$$\frac{n^2(n^2+1)}{2n} = \frac{n(n^2+1)}{2}$$Do đó, câu trả lời được xác định hoàn toàn bởi`n`. Sự sắp xếp thực tế của hình vuông là không liên quan. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n²) | Quá chậm và không cần thiết | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc thứ tự`n`của hình vuông ma thuật. 
2. Tính tổng các số từ`1`ĐẾN`n²`sử dụng công thức cấp số cộng:$$\frac{n^2(n^2+1)}{2}$$Điều này có tác dụng vì một hình vuông ma thuật bình thường chứa mỗi số này đúng một lần. 

1. Chia tổng số tiền cho`n`, vì hình vuông có`n`hàng và tất cả các hàng có tổng bằng nhau. 
2. Xuất ra giá trị kết quả là tổng của hàng đầu tiên. 

Tại sao nó hoạt động: 

Thuộc tính xác định của hình vuông ma thuật là mỗi hàng có tổng bằng nhau. Vì tất cả các số từ`1`ĐẾN`n²`xuất hiện một lần, tổng của tất cả các ô được cố định bất kể các số được sắp xếp như thế nào. Chia đều tổng số cố định này cho các`n`row cung cấp giá trị của mỗi hàng, kể cả hàng đầu tiên. Công thức không thể phụ thuộc vào cách xây dựng vì tất cả các ô vuông ma thuật bình thường hợp lệ đều có cùng tổng số và cùng số hàng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    print(n * (n * n + 1) // 2)

if __name__ == "__main__":
    solve()
```Chương trình chỉ giữ giá trị của`n`, vì vậy nó không bao giờ tự phân bổ hình vuông đó. biểu thức`n * (n * n + 1) // 2`suy ra trực tiếp từ công thức dẫn xuất. 

Số nguyên Python xử lý phép nhân trung gian một cách an toàn. Giá trị lớn nhất đạt được khi`n = 1000`, trong đó kết quả vẫn thấp hơn nhiều so với giới hạn số nguyên của Python. Phép chia số nguyên là chính xác vì tổng kỳ diệu luôn là số nguyên cho các bậc hợp lệ trong bài toán. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên: 

| Bước | n | n² | Giá trị công thức | Đầu ra | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | 3 | 9 | 3 × (9 + 1) / 2 | 15 | 

Công thức cho`15`, khớp với tổng hàng đầu tiên của bất kỳ`3 × 3`hình vuông ma thuật bình thường. Ví dụ này thể hiện trường hợp thứ tự lẻ tổng quát. 

Đối với mẫu thứ hai: 

| Bước | n | n² | Giá trị công thức | Đầu ra | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | 1 | 1 | 1 × (1 + 1) / 2 | 1 | 

Việc tính toán xử lý bình phương tầm thường một cách tự nhiên. Không cần xây dựng đặc biệt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ một số phép tính số học được thực hiện | 
| Không gian | O(1) | Thuật toán chỉ lưu trữ giá trị đầu vào | 

Giải pháp dễ dàng phù hợp với các giới hạn vì nó tránh được việc xây dựng một hình vuông có kích thước`n²`. Ngay cả thứ tự tối đa được phép cũng chỉ yêu cầu số học theo thời gian không đổi. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline
    n = int(input())
    return str(n * (n * n + 1) // 2)

# provided samples
assert solve("3\n") == "15", "sample 1"
assert solve("1\n") == "1", "sample 2"

# custom cases
assert solve("5\n") == "65", "odd order magic sum"
assert solve("1000\n") == "500000500", "maximum size input"
assert solve("7\n") == "175", "larger odd order"
assert solve("9\n") == "369", "off-by-one formula check"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`5`|`65`| Kiểm tra thứ tự lẻ thông thường | 
|`1000`|`500000500`| Kiểm tra giá trị tối đa được phép | 
|`7`|`175`| Kiểm tra công thức trên hình vuông lớn hơn | 
|`9`|`369`| Bắt các công thức số học sai | 

## Vỏ cạnh 

cho`n = 1`, thuật toán tính toán:$$\frac{1(1^2+1)}{2} = 1$$Đầu ra là:```
Input:
1

Output:
1
```Công thức hoạt động vì hàng duy nhất chứa ô duy nhất trong hình vuông. 

Đối với đơn hàng lớn nhất được phép:```
Input:
1000
```Thuật toán không cố gắng tạo ra một triệu ô. Nó tính trực tiếp:$$\frac{1000(1000^2+1)}{2}=500000500$$và kết quả đầu ra:```
500000500
```Điều này xác nhận tại sao việc sử dụng thuộc tính toán học là cần thiết: khối lượng công việc vẫn giữ nguyên ngay cả khi hình vuông trở nên rất lớn.
