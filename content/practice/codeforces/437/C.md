---
title: "CF 437C - Trẻ em và Đồ chơi"
description: "Chúng ta được tặng một món đồ chơi gồm n phần được nối với nhau bằng m sợi dây. Mỗi sợi dây nối hai phần riêng biệt và không có cặp phần nào có nhiều hơn một sợi dây nối giữa chúng. Mỗi phần i có chi phí năng lượng liên quan v[i]. Trẻ tháo từng phần một."
date: "2026-06-07T15:49:37+07:00"
tags: ["codeforces", "competitive-programming", "graphs", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 437
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 250 (Div. 2)"
rating: 1400
weight: 437
solve_time_s: 112
verified: true
draft: false
---

[CF 437C - Đứa trẻ và Đồ chơi](https://codeforces.com/problemset/problem/437/C) 

**Đánh giá:** 1400 
**Tags:** đồ thị, tham lam, sắp xếp 
**Thời gian giải:** 1m 52s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được tặng một món đồ chơi làm từ`n`các bộ phận được kết nối bởi`m`dây thừng. Mỗi sợi dây nối hai phần riêng biệt và không có cặp phần nào có nhiều hơn một sợi dây nối giữa chúng. Mỗi phần`i`có chi phí năng lượng liên quan`v[i]`. Trẻ tháo từng phần một. Chi phí để loại bỏ một bộ phận là tổng giá trị năng lượng của các bộ phận lân cận vẫn còn tồn tại tại thời điểm loại bỏ. Nhiệm vụ của chúng ta là xác định tổng năng lượng tối thiểu mà trẻ phải bỏ ra để loại bỏ tất cả các bộ phận. 

Kích thước đầu vào vừa phải, với`n`lên đến 1000 và`m`lên đến năm 2000. Điều này có nghĩa là giải pháp O(n^2) là khả thi, nhưng bất kỳ điều gì theo kiểu O(n!), chẳng hạn như thử tất cả các hoán vị của thứ tự loại bỏ, đều hoàn toàn không thực tế. Chúng ta cần một thuật toán tránh kiểm tra rõ ràng mọi chuỗi có thể. Trường hợp cạnh không rõ ràng phát sinh khi một bộ phận có năng lượng rất cao nhưng được kết nối với các bộ phận có năng lượng thấp. Nếu chúng ta loại bỏ nó quá sớm, tổng chi phí sẽ tăng lên một cách không cần thiết. Một trường hợp cạnh khác xảy ra khi biểu đồ bị ngắt kết nối: các phần trong các thành phần biệt lập chỉ tương tác trong thành phần của chúng và một phép tính tổng đơn giản có thể tính sai các đóng góp. 

Ví dụ, hãy xem xét ba phần có giá trị`[1, 100, 1]`được kết nối trong một dòng:`1-2-3`. Loại bỏ chi phí đầu tiên của phần 2`1+1 = 2`. Loại bỏ 1 hoặc 3 đầu tiên sẽ phát sinh`100`trị giá. Trình tự tối ưu ở đây là loại bỏ phần năng lượng cao cuối cùng, trái ngược với chiến lược "loại bỏ lớn nhất trước" ngây thơ. 

## Phương pháp tiếp cận 

Một giải pháp vũ phu sẽ liệt kê tất cả`n!`lệnh loại bỏ, tính toán chi phí cho mỗi lệnh và trả lại mức tối thiểu. Điều này đúng nhưng hoàn toàn không khả thi ngay cả đối với`n = 10`. Với`n = 1000`, chúng tôi không thể thử điều này. 

Nhận xét quan trọng là chi phí để loại bỏ một bộ phận chỉ phụ thuộc vào các bộ phận lân cận hiện tại của nó. Nếu một phần có năng lượng cao`v[i]`, loại bỏ nó khi nó vẫn còn nhiều hàng xóm cao`v[j]`đắt tiền. Ngược lại, việc loại bỏ những nước láng giềng có năng lượng thấp trước tiên sẽ làm giảm sự đóng góp cho việc loại bỏ trong tương lai. Điều này tương đương với ý tưởng tham lam “luôn loại bỏ phần nhỏ nhất có sẵn được kết nối với các phần còn lại”, nhưng chúng ta phải khái quát hóa nó thành một biểu đồ: mỗi sợi dây góp phần`min(v[i], v[j])`đến tổng năng lượng đúng một lần. Điều này là do đối với mỗi sợi dây, điểm cuối nhỏ hơn trong số hai điểm cuối của nó sẽ bị xóa trong khi điểm cuối còn lại vẫn tồn tại, cộng chính xác`min(v[i], v[j])`đến tổng chi phí. Vì vậy, bài toán rút gọn thành tổng`min(v[i], v[j])`trên tất cả các cạnh. 

Brute-force không thành công vì nó cố gắng tính toán trình tự một cách rõ ràng. Nhận thấy rằng mỗi cạnh chỉ đóng góp`min(v[i], v[j])`hãy để chúng tôi tính toán câu trả lời theo thời gian tuyến tính đối với số cạnh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tất cả các hoán vị) | Ồ (n!) | O(n + m) | Quá chậm | 
| Đóng góp tham lam | O(m) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`Và`m`, số phần và số dây, tiếp theo là mảng`v`của một phần giá trị năng lượng. 
2. Khởi tạo một biến`total_energy`về không. Điều này sẽ tích lũy năng lượng tối thiểu. 
3. Lặp lại từng bước`m`dây thừng. Đối với các bộ phận kết nối dây`x`Và`y`, thêm vào`min(v[x], v[y])`ĐẾN`total_energy`. 

Lý do là mỗi sợi dây đóng góp giá trị điểm cuối tối thiểu của nó đúng một lần trong quá trình loại bỏ. 
4. Sau khi xử lý tất cả các dây, in`total_energy`. Giá trị này thể hiện năng lượng tối thiểu cần thiết để loại bỏ tất cả các bộ phận. 

Tại sao nó hoạt động: Mỗi sợi dây đóng góp vào chi phí đúng một lần thông qua điểm cuối nhỏ hơn của nó, bởi vì điểm cuối đó sẽ bị xóa trong khi điểm cuối kia vẫn tồn tại. Tính tổng tất cả các cạnh sẽ thu được tổng năng lượng mà không cần mô phỏng rõ ràng trình tự loại bỏ. Không có số lượng gấp đôi và mọi đóng góp của cạnh đều cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m = map(int, input().split())
v = list(map(int, input().split()))

total_energy = 0
for _ in range(m):
    x, y = map(int, input().split())
    total_energy += min(v[x-1], v[y-1])

print(total_energy)
```Mã đầu tiên đọc đầu vào. Danh sách`v`được lập chỉ mục bằng 0, vì vậy khi đọc các cạnh, chúng ta trừ đi 1 từ`x`Và`y`để lập chỉ mục chính xác. Vòng lặp trên các cạnh trực tiếp tích lũy`min`của hai phần được kết nối. Không có cấu trúc dữ liệu bổ sung nào cần thiết ngoài việc lưu trữ`v`mảng và tổng số đang chạy. 

## Ví dụ đã hoạt động 

Đối với mẫu 1: 

| Bước | Cạnh | v[x-1] | v[y-1] | phút(v[x-1], v[y-1]) | tổng_năng lượng | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1-4 | 10 | 40 | 10 | 10 | 
| 2 | 1-2 | 10 | 20 | 10 | 20 | 
| 3 | 2-3 | 20 | 30 | 20 | 40 | 

Total_energy cuối cùng là 40, phù hợp với sản lượng dự kiến. 

Đối với Mẫu 2, giả sử tất cả các phần đều có giá trị 100 và được kết nối đầy đủ: 

| Bước | Cạnh | v[x-1] | v[y-1] | phút(v[x-1], v[y-1]) | tổng_năng lượng | 
| --- | --- | --- | --- | --- | --- | 
| Mỗi cạnh đóng góp 100 | - | - | - | - | tổng = số_cạnh * 100 | 

Điều này xác nhận thuật toán xử lý chính xác các giá trị thống nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m) | Chúng ta xử lý mỗi m cạnh chính xác một lần, tính toán một`min`mỗi cạnh | 
| Không gian | O(n) | Chúng tôi lưu trữ mảng`v`có chiều dài n; không cần cấu trúc bổ sung | 

Với những hạn chế`n <= 1000`Và`m <= 2000`, giải pháp này chạy thoải mái trong vòng 1 giây và sử dụng ít hơn 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, m = map(int, input().split())
    v = list(map(int, input().split()))
    total_energy = 0
    for _ in range(m):
        x, y = map(int, input().split())
        total_energy += min(v[x-1], v[y-1])
    return str(total_energy)

# provided samples
assert run("4 3\n10 20 30 40\n1 4\n1 2\n2 3\n") == "40", "sample 1"
assert run("3 3\n100 100 100\n1 2\n2 3\n1 3\n") == "300", "sample 2"

# custom cases
assert run("1 0\n5\n") == "0", "single part, no edges"
assert run("2 1\n1 100\n1 2\n") == "1", "two parts, one edge, remove smaller first"
assert run("4 0\n5 6 7 8\n") == "0", "no edges"
assert run("5 4\n1 2 3 4 5\n1 2\n2 3\n3 4\n4 5\n") == "10", "chain of 5 nodes"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 0\n5 | 0 | Nút đơn, không có cạnh | 
| 2 1\n1 100\n1 2\n | 1 | Đóng góp cạnh tối thiểu, giá trị nhỏ hơn được tính | 
| 4 0\n5 6 7 8 | 0 | Không có cạnh, tổng năng lượng bằng không | 
| 5 4\n1 2 3 4 5\n1 2\n2 3\n3 4\n4 5\n | 10 | Cấu trúc chuỗi, tích tụ chính xác trên các cạnh | 

## Vỏ cạnh 

Khi nào`m = 0`, thuật toán trả về chính xác 0, vì không có cạnh nào và do đó không có đóng góp năng lượng. Ví dụ, đầu vào`1 0\n5\n`mang lại 0. Khi tất cả các nút có giá trị bằng nhau, mỗi cạnh sẽ đóng góp giá trị đó đúng một lần. Ví dụ,`3 3\n100 100 100\n1 2\n2 3\n1 3\n`dẫn đến tổng năng lượng`300`, khớp với số lượng đóng góp điểm cuối tối thiểu dự kiến. Các chuỗi, ngôi sao và đồ thị bị ngắt kết nối đều được xử lý liền mạch vì mỗi cạnh được xử lý độc lập và chỉ đóng góp`min(v[i], v[j])`.
