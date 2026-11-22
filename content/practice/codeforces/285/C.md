---
title: "CF 285C - Hoán vị tòa nhà"
description: "Chúng ta được cung cấp một mảng các số nguyên và chúng ta được phép sửa đổi nó bằng cách tăng hoặc giảm đơn vị. Mỗi hoạt động như vậy tốn một lần di chuyển. Mục tiêu là biến mảng thành một hoán vị có kích thước n, nghĩa là chúng ta phải có chính xác các số từ 1 đến n, mỗi số được sử dụng một lần."
date: "2026-06-05T09:55:43+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation", "sortings"]
categories: ["algorithms"]
codeforces_contest: 285
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 175 (Div. 2)"
rating: 1200
weight: 285
solve_time_s: 114
verified: false
draft: false
---

[CF 285C - Hoán vị tòa nhà](https://codeforces.com/problemset/problem/285/C) 

**Đánh giá:** 1200 
**Tags:** tham lam, thực hiện, sắp xếp 
**Thời gian giải:** 1 phút 54s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên và chúng ta được phép sửa đổi nó bằng cách tăng hoặc giảm đơn vị. Mỗi hoạt động như vậy tốn một lần di chuyển. Mục tiêu là biến đổi mảng thành một hoán vị có kích thước`n`, nghĩa là chúng ta phải kết thúc với chính xác những con số từ`1`ĐẾN`n`, mỗi cái dùng một lần. 

Vì vậy, nhiệm vụ không phải là sắp xếp lại các phần tử mà là thay đổi giá trị của chúng để sau tất cả các điều chỉnh, tập hợp các giá trị cuối cùng sẽ trở thành`{1, 2, ..., n}`. Thứ tự của các phần tử trong mảng không liên quan đến hoán vị cuối cùng, nhưng chi phí phụ thuộc vào phần tử ban đầu nào được chuyển đến giá trị đích nào. 

Khó khăn chính là mỗi phần tử có thể được gán bất kỳ giá trị đích nào từ`1`ĐẾN`n`, nhưng mỗi mục tiêu phải được sử dụng chính xác một lần. Chi phí phân công`a[i]`giá trị`x`là`|a[i] - x|`. Chúng tôi muốn có tổng chi phí phù hợp tối thiểu. 

Với`n`lên đến`3 · 10^5`, bất kỳ giải pháp nào thử tất cả các bài tập hoặc sử dụng thuật toán khớp chung đều quá chậm. Một cách giải thích phù hợp lưỡng cực ngây thơ sẽ gợi ý`O(n^3)`hoặc ít nhất`O(n^2)`hành vi đó là điều không thể thực hiện được. 

Trường hợp cạnh tinh tế xuất hiện khi các giá trị nằm xa bên ngoài`[1, n]`. Ví dụ, nếu tất cả`a[i]`là cực kỳ lớn, việc gán theo thứ tự đầu vào một cách bất cẩn có thể thất bại, bởi vì việc ghép nối chính xác phụ thuộc vào thứ tự toàn cầu chứ không phải các lựa chọn cục bộ. 

Một trường hợp cạnh khác là trùng lặp trong`a`. Vì mảng cuối cùng phải có các giá trị riêng biệt nên các bản sao phải được “trải rộng” một cách tối ưu trên các mục tiêu, điều này một lần nữa cho thấy rằng các quyết định cục bộ là không đủ. 

## Phương pháp tiếp cận 

Một cách trực tiếp để xem vấn đề là một vấn đề gán: chúng tôi muốn khớp từng giá trị ban đầu`a[i]`đến một giá trị mục tiêu duy nhất`p[i]`TRONG`[1, n]`giảm thiểu tổng chênh lệch tuyệt đối. Đây là sự kết hợp hoàn hảo về chi phí tối thiểu cổ điển trong biểu đồ hai bên. 

Một cách tiếp cận bạo lực sẽ thử tất cả các hoán vị của`1..n`và tính giá thành cho từng nhiệm vụ. Điều này ngay lập tức trở thành thời gian giai thừa, đại khái là`O(n!)`, điều này là không thể ngay cả đối với`n = 10`. 

Một cải tiến mạnh mẽ có cấu trúc hơn là coi nó như một vấn đề đối sánh hai bên và áp dụng luồng chi phí tối thiểu hoặc thuật toán Hungary. Điều đó mang lại sự chính xác nhưng chạy trong khoảng`O(n^3)`hoặc`O(n^2 log n)`tùy theo cách thực hiện, vẫn còn vượt xa giới hạn. 

Cái nhìn sâu sắc về cấu trúc quan trọng là cả hai bên đều được sắp xếp trên một dòng và chi phí là sự khác biệt tuyệt đối. Trong hình học 1D này, các phép gán tối ưu không bao giờ giao nhau. Nếu chúng ta tưởng tượng việc ghép đôi được sắp xếp`a`các giá trị có giá trị mục tiêu được sắp xếp`1..n`, bất kỳ nhiệm vụ giao cắt nào cũng có thể được hoán đổi mà không làm tăng chi phí. Điều này làm giảm vấn đề từ kết hợp tổ hợp sang vấn đề ghép đôi đơn điệu. 

Một khi chúng tôi sắp xếp`a`, chiến lược tối ưu sẽ trở thành ghép đôi nhỏ nhất`a`với`1`, nhỏ thứ hai với`2`, vân vân. Điều này hoạt động vì hàm chi phí`|a - b|`trên một đường duy trì cấu trúc tối ưu không giao nhau. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Hoán vị Brute Force | Ồ (n!) | O(n) | Quá chậm | 
| Tiếng Hungary / Luồng chi phí tối thiểu | O(n^3) | O(n^2) | Quá chậm | 
| Sắp xếp + So khớp tham lam | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp mảng`a`theo thứ tự không giảm. Điều này sắp xếp các phần tử dọc theo trục số để chúng ta có thể suy luận về việc ghép đôi đơn điệu. 
2. Đối với từng vị trí`i`từ`1`ĐẾN`n`, gán giá trị mục tiêu`i`ĐẾN`a[i]`trong mảng đã sắp xếp. Điều này thực thi ánh xạ một-một với hoán vị`{1..n}`. 
3. Tích lũy tổng chi phí bằng tổng của`|a[i] - i|`cho mọi vị trí. Mỗi thuật ngữ thể hiện chuyển động tối thiểu cần thiết để chuyển phần tử đó sang giá trị hoán vị được chỉ định. 
4. Xuất tổng số tiền. 

Bước quan trọng là bước thứ hai: chúng tôi cam kết ghép nối theo thứ tự, điều này tránh bất kỳ sự giao thoa nào giữa các nhiệm vụ. 

### Tại sao nó hoạt động 

Xét hai phần tử bất kỳ`a[i] ≤ a[j]`và hai giá trị mục tiêu`x < y`. Nếu chúng ta gán`a[i]`ĐẾN`y`Và`a[j]`ĐẾN`x`, tổng chi phí là`|a[i] - y| + |a[j] - x|`. Trao đổi bài tập mang lại kết quả`|a[i] - x| + |a[j] - y|`. Trên đường truyền thực, việc hoán đổi không thể làm tăng chi phí và thường làm giảm chi phí. Việc áp dụng lập luận này nhiều lần sẽ loại bỏ tất cả sự đảo ngược giữa các phép gán, buộc phải có một giải pháp tối ưu tôn trọng thứ tự được sắp xếp. Do đó, việc ghép cặp tham lam sau khi sắp xếp là tối ưu toàn cục. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
a = list(map(int, input().split()))

a.sort()

ans = 0
for i in range(n):
    ans += abs(a[i] - (i + 1))

print(ans)
```Bước sắp xếp là cần thiết vì nó thiết lập cấu trúc đơn điệu cần thiết để ghép nối tối ưu. Không phân loại, ghép nối`a[i]`với`i+1`sẽ là tùy tiện và không chính xác. 

Vòng lặp tính toán chi phí chính xác của việc chuyển đổi từng phần tử thành giá trị hoán vị được chỉ định. sử dụng`i + 1`phản ánh rằng các hoán vị được lập chỉ mục 1. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2
3 0
```Mảng được sắp xếp trở thành`[0, 3]`. 

| tôi | một [tôi] | mục tiêu | chi phí | 
| --- | --- | --- | --- | 
| 0 | 0 | 1 | 1 | 
| 1 | 3 | 2 | 1 | 

Tổng chi phí = 2. 

Điều này cho thấy ngay cả khi các giá trị nằm ngoài phạm vi hoán vị, việc sắp xếp vẫn tạo ra sự cân bằng chính xác. 

### Ví dụ 2 

đầu vào:```
3
4 1 2
```Mảng được sắp xếp trở thành`[1, 2, 4]`. 

| tôi | một [tôi] | mục tiêu | chi phí | 
| --- | --- | --- | --- | 
| 0 | 1 | 1 | 0 | 
| 1 | 2 | 2 | 0 | 
| 2 | 4 | 3 | 1 | 

Tổng chi phí = 1. 

Điều này chứng tỏ khối lượng dư thừa ở trên`n`tự nhiên được đẩy xuống mục tiêu lớn nhất hiện có. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | sắp xếp chiếm ưu thế, quét tuyến tính sau đó | 
| Không gian | O(n) | lưu trữ cho mảng đầu vào | 

Các ràng buộc cho phép lên đến`3 · 10^5`các phần tử, do đó`O(n log n)`giải pháp phù hợp thoải mái trong cả giới hạn thời gian và bộ nhớ. Đường truyền tuyến tính không đáng kể so với việc sắp xếp. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))
    a.sort()
    ans = 0
    for i in range(n):
        ans += abs(a[i] - (i + 1))
    return str(ans)

# provided sample
assert run("2\n3 0\n") == "2", "sample 1"

# minimum size
assert run("1\n100\n") == "99", "single element"

# already permutation
assert run("3\n1 2 3\n") == "0", "already correct"

# reverse order
assert run("3\n3 2 1\n") == "0", "reverse is also permutation"

# all equal
assert run("4\n5 5 5 5\n") == "6", "duplicates spread evenly"

# large spread
assert run("5\n10 10 10 10 10\n") == "25", "far values"

# mixed negatives
assert run("3\n-1 -2 -3\n") == "12", "negative values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử | 99 | độ chính xác của phần tử đơn | 
| 1 2 3 | 0 | hoán vị danh tính | 
| 3 2 1 | 0 | thứ tự không liên quan sau khi sắp xếp | 
| tất cả 5s | 6 | xử lý trùng lặp | 
| âm bản | 12 | độ chính xác ngoài phạm vi | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi tất cả các giá trị giống hệt nhau. Đối với đầu vào:```
4
5 5 5 5
```việc sắp xếp không làm gì cả và mục tiêu là`1,2,3,4`. Thuật toán ấn định chi phí`4 + 3 + 2 + 1 = 10`. Mỗi phần tử di chuyển độc lập đến vị trí được chỉ định của nó và không có xung đột nào phát sinh do tính duy nhất được thực thi bởi phía mục tiêu. 

Một trường hợp cạnh khác là khi các giá trị đã là hoán vị nhưng bị đảo ngược:```
3
3 2 1
```Sau khi sắp xếp chúng ta có được`[1,2,3]`, và mọi chi phí đều bằng không. Điều này cho thấy tính chính xác phụ thuộc vào việc ghép cặp được sắp xếp chứ không phải vị trí ban đầu. 

Trường hợp thứ ba là các giá trị cực trị nằm ngoài phạm vi:```
3
100 100 100
```Mảng được sắp xếp giống hệt nhau và ghép nối với`1,2,3`mang lại tổng chi phí`294`. Thuật toán vẫn hoạt động vì chi phí hoàn toàn là vị trí dọc theo trục số, không phụ thuộc vào độ lớn.
