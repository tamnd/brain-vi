---
title: "CF 102793D - \u0414\u043e\u043c\u0438\u043d\u043e"
description: "Quân domino trong bài toán này được thể hiện bằng một hàng dấu chấm có một dấu phân cách dọc. Các dấu chấm ở bên trái và bên phải của dải phân cách là số điểm trên hai nửa viên gạch. Chúng ta được cấp hai viên gạch như vậy và cần phải quyết định xem liệu chúng có thể được đặt cùng nhau hay không."
date: "2026-07-27T17:58:45+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102793
codeforces_index: "D"
codeforces_contest_name: "\u0426\u0438\u043a\u043b \u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434, \u0421\u0435\u0437\u043e\u043d 2020-21, \u0412\u0442\u043e\u0440\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430"
rating: 0
weight: 102793
solve_time_s: 38
verified: true
draft: false
---

[CF 102793D - \u0414\u043e\u043c\u0438\u043d\u043e](https://codeforces.com/problemset/problem/102793/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 38s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Quân domino trong bài toán này được thể hiện bằng một hàng dấu chấm có một dấu phân cách dọc. Các dấu chấm ở bên trái và bên phải của dải phân cách là số điểm trên hai nửa viên gạch. Chúng ta được cấp hai viên gạch như vậy và cần phải quyết định xem liệu chúng có thể được đặt cùng nhau hay không. 

Hai quân domino có thể giống nhau nếu có một số từ 0 đến 6 xuất hiện trên ít nhất một nửa số domino đầu tiên và cũng xuất hiện trên ít nhất một nửa số domino thứ hai. Đầu ra là`Yes`khi tồn tại một nửa giá trị chung như vậy và`No`nếu không thì. Báo cáo vấn đề ban đầu mô tả chính xác điều kiện tương thích này cho hai quân domino ảo. 

Mỗi nửa chứa tối đa sáu dấu chấm, do đó chuỗi đầu vào cực kỳ nhỏ. Việc kiểm tra trực tiếp tất cả các giá trị có thể có đã là thời gian không đổi. Ngay cả khi chúng ta bỏ qua giới hạn nhỏ và tưởng tượng các chuỗi lớn hơn, chúng ta chỉ cần kiểm tra hai nửa của mỗi domino một lần, điều này loại trừ mọi nhu cầu về thuật toán đồ thị, lập trình động hoặc mô phỏng. 

Các trường hợp cạnh chính xuất phát từ thực tế là cho phép không có dấu chấm. Một nửa domino không có dấu chấm được biểu thị bằng một cạnh trống của dấu phân cách, do đó dấu phân cách có thể là ký tự đầu tiên hoặc cuối cùng. 

Ví dụ:```
|
.
```Quân domino đầu tiên có giá trị 0 và 0, trong khi quân thứ hai có giá trị 0 và 1. Kết quả đúng là:```
Yes
```Việc thực hiện bất cẩn chỉ đếm dấu chấm và bỏ qua các cạnh trống sẽ xử lý quân domino đầu tiên không chính xác và bỏ lỡ kết quả khớp ở giá trị 0. 

Một trường hợp khác là khi giá trị bằng nhau không ở cùng một phía.```
.|...
....|.
```Quân domino đầu tiên chứa các giá trị 1 và 3. Quân thứ hai chứa các giá trị 4 và 1. Kết quả đúng là:```
Yes
```Một giải pháp chỉ so sánh nửa bên trái hoặc chỉ nửa bên phải sẽ loại bỏ cặp này một cách không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là tạo ra hai giá trị của quân domino đầu tiên, tạo ra hai giá trị của quân domino thứ hai và so sánh mọi giá trị từ quân domino này với mọi giá trị của quân domino kia. Chỉ có bốn so sánh, do đó số lượng hoạt động là không đổi. Cách tiếp cận này đã đủ nhanh vì đầu vào chỉ chứa hai quân domino. 

Một sai lầm phổ biến là làm phức tạp vấn đề quá mức bằng cách cố gắng mô phỏng hành động kết nối các quân domino. Điều kiện đơn giản hơn nhiều: hai ô chỉ cần một nửa giá trị khớp nhau. Vị trí của các nửa phù hợp không quan trọng. 

Quan sát quan trọng là mỗi domino có thể được giảm xuống thành một bộ gồm nhiều nhất hai số. Khi cả hai bộ được xây dựng, câu trả lời chỉ là liệu giao điểm của chúng có trống hay không. So sánh vũ phu và chế độ xem dựa trên tập hợp là tương đương nhau, nhưng cách biểu diễn tập hợp làm cho lý do rõ ràng hơn và tránh các trường hợp thiếu liên quan đến số 0. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(1) | O(1) | Đã chấp nhận | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tách từng chuỗi đầu vào tại dấu phân cách dọc. Số chấm trước và sau dấu phân cách cho hai giá trị ghi trên quân domino. 
2. Lưu trữ hai giá trị của mỗi quân domino. Giá trị có thể bằng 0, do đó phần trống của chuỗi vẫn phải được tính là một nửa hợp lệ. 
3. Kiểm tra xem bất kỳ giá trị nào từ quân domino thứ nhất có bằng bất kỳ giá trị nào từ quân domino thứ hai hay không. 
4. In`Yes`nếu tìm thấy giá trị phù hợp. Nếu không thì in`No`. 

Lý do điều này có tác dụng là vì toàn bộ định nghĩa về khả năng tương thích chỉ phụ thuộc vào việc hai bộ nửa giá trị có chia sẻ ít nhất một phần tử hay không. Vị trí vật lý của các nửa và hướng của các quân domino không bao giờ ảnh hưởng đến câu trả lời. 

## Tại sao nó hoạt động 

Thuật toán lưu giữ thông tin chính xác cần thiết từ mỗi domino. Một quân domino chỉ có hai thuộc tính liên quan: số chấm ở nửa bên trái và số chấm ở nửa bên phải. Nếu tồn tại một vị trí tương thích, một số giá trị`k`phải xuất hiện giữa hai giá trị này cho cả hai quân domino và thuật toán sẽ kiểm tra mọi cặp có thể. Nếu thuật toán tìm được một giá trị chung thì giá trị đó thỏa mãn điều kiện yêu cầu. Nếu nó không tìm thấy, không có giá trị`k`tồn tại. Vì vậy, câu trả lời được đưa ra luôn luôn đúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    a = input().strip()
    b = input().strip()

    def values(s):
        left, right = s.split('|')
        return {left.count('.'), right.count('.')}

    first = values(a)
    second = values(b)

    print("Yes" if first & second else "No")

if __name__ == "__main__":
    solve()
```các`values`hàm chuyển đổi mô tả domino thành hai số được viết trên hai nửa của nó. Chia theo`|`an toàn hơn so với quét thủ công vì nó xử lý một cách tự nhiên các nửa trống như`|`hoặc`...|`. 

Việc sử dụng một bộ sẽ loại bỏ sự trùng lặp. Ví dụ, một quân domino được mô tả bởi`|`trở thành`{0}`, và quân domino có cả hai nửa chứa ba dấu chấm sẽ trở thành`{3}`. Hoạt động giao cắt kiểm tra xem có tồn tại ít nhất một giá trị chung hay không. 

Không lập chỉ mục nào được sử dụng, do đó không có vấn đề về ranh giới. Đếm dấu chấm trực tiếp cũng tránh được các vấn đề về chuyển đổi số nguyên vì tất cả các giá trị có thể nằm trong khoảng từ 0 đến 6. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
..|....
....|...
```Việc thực hiện có thể được theo dõi như sau. 

| Bước | Giá trị domino đầu tiên | Giá trị domino thứ hai | Giao lộ | Kết quả | 
| --- | --- | --- | --- | --- | 
| Phân tích đầu vào | {2, 4} | {4, 3} | {4} | Có | 

Hai quân domino có chung giá trị 4 nên có thể khớp với nhau. Điều này chứng tỏ rằng các bên phù hợp không cần phải ở cùng một vị trí. 

### Ví dụ 2 

đầu vào:```
.|...
..|....
```| Bước | Giá trị domino đầu tiên | Giá trị domino thứ hai | Giao lộ | Kết quả | 
| --- | --- | --- | --- | --- | 
| Phân tích đầu vào | {1, 3} | {2, 4} | trống | Không | 

Không có số nào xuất hiện trên cả hai quân domino, vì vậy không có giá trị trùng khớp nào có thể tồn tại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ có bốn nửa giá trị được kiểm tra. | 
| Không gian | O(1) | Mỗi domino lưu trữ tối đa hai giá trị. | 

Các ràng buộc lớn hơn nhiều so với những gì giải pháp này cần. Chương trình thực hiện một số thao tác cố định bất kể kích thước đầu vào nên dễ dàng phù hợp với giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve_data(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    a = sys.stdin.readline().strip()
    b = sys.stdin.readline().strip()

    def values(s):
        left, right = s.split('|')
        return {left.count('.'), right.count('.')}

    ans = "Yes" if values(a) & values(b) else "No"
    sys.stdin = old_stdin
    return ans

assert solve_data("..|....\n....|...\n") == "Yes", "sample 1"
assert solve_data(".|...\n..|....\n") == "No", "sample 2"
assert solve_data("|\n|\n") == "Yes", "both dominoes are zero"
assert solve_data("|\n......|\n") == "Yes", "zero on one side"
assert solve_data(".|....\n..|...\n") == "No", "different values only"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|` | \n | \n`| 
|` | \n...... | \n`| 
|`. | ....\n.. | ...\n`| 

## Vỏ cạnh 

Đối với trường hợp cạnh đầu tiên:```
|
.
```Domino đầu tiên được phân tích cú pháp là`{0}`. Domino thứ hai được phân tích cú pháp là`{0, 1}`. Giao điểm của họ là`{0}`, do đó thuật toán in ra:```
Yes
```Chi tiết quan trọng là một cạnh trống vẫn là một nửa hợp lệ không chứa dấu chấm nào. 

Đối với trường hợp cạnh thứ hai:```
.|...
....|.
```Quân domino đầu tiên trở thành`{1, 3}`và thứ hai trở thành`{4, 1}`. Thuật toán so sánh các bộ hoàn chỉnh thay vì chỉ khớp các cạnh tương ứng, tìm giá trị chung`1`và in:```
Yes
```Điều này xác nhận rằng bất kỳ nửa nào của quân domino đều có thể được sử dụng để kết nối.
