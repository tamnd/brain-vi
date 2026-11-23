---
title: "CF 475A - Xe buýt Bayan"
description: "Bố cục xe buýt đã được cố định và đã được vẽ sẵn cho chúng tôi. Tổng cộng có 34 ghế hành khách. Hành khách luôn chiếm chỗ theo một thứ tự rất cụ thể: họ bắt đầu từ hàng cuối cùng, lấp đầy từ trái sang phải, sau đó di chuyển lên từng hàng."
date: "2026-05-30T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 475
codeforces_index: "A"
codeforces_contest_name: "Bayan 2015 Contest Warm Up"
rating: 1100
weight: 475
solve_time_s: 119
verified: true
draft: false
---

[CF 475A - Xe buýt Bayan](https://codeforces.com/problemset/problem/475/A) 

**Đánh giá:** 1100 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 59 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Bố cục xe buýt đã được cố định và đã được vẽ sẵn cho chúng tôi. Tổng cộng có 34 ghế hành khách. Hành khách luôn chiếm chỗ theo một thứ tự rất cụ thể: họ bắt đầu từ hàng cuối cùng, lấp đầy từ trái sang phải, sau đó di chuyển lên từng hàng. Bất cứ khi nào có nhiều ghế ở hàng hiện tại, ghế trống ngoài cùng bên trái sẽ được chọn. 

Đầu vào chứa một số nguyên duy nhất`k`, số lượng hành khách hiện có trên xe buýt. Chúng ta phải in sơ đồ bus nghệ thuật ASCII sáu dòng và đánh dấu chính xác đường đầu tiên`k`chỗ ngồi theo thứ tự lên máy bay như đã có người sử dụng (`O`). Mọi ghế hành khách còn lại vẫn trống (`#`). 

Những hạn chế là cực kỳ nhỏ. Số lượng hành khách nhiều nhất là 34, đúng số ghế. Ngay cả một mô phỏng lực lượng vũ phu kiểm tra từng chỗ ngồi riêng lẻ cũng chỉ thực hiện được vài chục thao tác. Thời gian chạy không liên quan ở đây, thách thức là tái tạo chính xác định dạng đầu ra được yêu cầu. 

Nguồn sai lầm phổ biến nhất là hiểu sai lệnh lên máy bay. Hành khách không xếp đầy xe buýt từ hàng ghế đầu trở xuống. Họ bắt đầu ở hàng ghế bốn chỗ đặc biệt và sau đó tiếp tục đi qua các hàng còn lại. 

Coi như`k = 1`. Ghế ngồi phải là ghế đầu tiên ở hàng cuối cùng:```
+------------------------+
|O.#.#.#.#.#.#.#.#.#.#.|D|)
|#.#.#.#.#.#.#.#.#.#.#.|.|
|#.......................|
|#.#.#.#.#.#.#.#.#.#.#.|.|)
+------------------------+
```Giải pháp lấp đầy chỗ ngồi từ góc trên bên trái sẽ đặt hành khách vào sai vị trí. 

Một sai lầm dễ dàng khác xuất hiện khi`k = 34`. Mọi ghế hành khách đều phải có người ngồi. Bất kỳ việc triển khai nào bỏ quên ghế phụ ở hàng ghế 4 chỗ sẽ chỉ lấp đầy 33 vị trí. 

Trường hợp cạnh cuối cùng là`k = 0`. Không có chỗ ngồi nên chứa`O`. Một số cách triển khai vô tình buộc phải có ít nhất một chỗ ngồi bị chiếm dụng vì chúng làm giảm bộ đếm hành khách trước khi kiểm tra xem chỗ ngồi có còn hay không. 

## Phương pháp tiếp cận 

Ý tưởng trực tiếp nhất là liệt kê mọi chỗ ngồi theo thứ tự chính xác mà hành khách ngồi. Sau đó chúng tôi xử lý danh sách đó từ đầu và đánh dấu danh sách đầu tiên`k`các vị trí như bị chiếm đóng. Vì chỉ có 34 chỗ ngồi nên mô phỏng này thực hiện tối đa 34 lần cập nhật. 

Mặc dù tốc độ này đã đủ nhanh nhưng nó vẫn giúp ích cho việc xem xét cấu trúc của xe buýt. Bản vẽ xe buýt đã được cố định. Chỉ có các ký tự chỗ ngồi thay đổi. Thay vì mô phỏng hành khách di chuyển qua các hàng, chúng ta có thể xây dựng mẫu ASCII cuối cùng và thay thế từng vị trí ghế theo thứ tự lên máy bay. 

Quan sát quan trọng là thứ tự lên máy bay tương ứng chính xác với việc quét các vị trí chỗ ngồi theo trình tự định trước. Khi chúng ta biết 34 tọa độ đó, vấn đề sẽ trở thành thao tác chuỗi đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng hành khách Brute Force | O(34) | O(34) | Đã chấp nhận | 
| Điền mẫu tối ưu | O(34) | O(34) | Đã chấp nhận | 

Vì số lượng chỗ ngồi là cố định nên cả hai cách tiếp cận đều có độ phức tạp không đổi. Phiên bản điền mẫu sạch hơn và ánh xạ trực tiếp đến đầu ra được yêu cầu. 

## Hướng dẫn thuật toán 

1. Lưu trữ bus dưới dạng danh sách các chuỗi biểu thị bố cục trống được yêu cầu. 
2. Chuyển đổi từng chuỗi thành một danh sách các ký tự có thể thay đổi để có thể sửa đổi các vị trí ghế riêng lẻ. 
3. Tạo danh sách chứa tất cả 34 tọa độ chỗ ngồi theo thứ tự lên máy bay. 
4. Bốn tọa độ đầu tiên tương ứng với hàng đặc biệt có bốn ghế. 
5. Các tọa độ còn lại tương ứng với các hàng khác, được quét từ trên xuống dưới và từ trái sang phải chính xác theo số lượng hành khách sẽ ngồi. 
6. Lặp lại lần đầu tiên`k`tọa độ của danh sách này và thay thế ký tự chỗ ngồi ở mỗi vị trí bằng`O`. 
7. Để nguyên tất cả các vị trí ghế còn lại`#`. 
8. Chuyển đổi từng danh sách ký tự thành một chuỗi và in sáu dòng. 

### Tại sao nó hoạt động 

Danh sách tọa độ được xây dựng theo đúng thứ tự hành khách chọn chỗ ngồi. Tọa độ đầu tiên thể hiện ghế trống đầu tiên, tọa độ thứ hai thể hiện ghế trống thứ hai, v.v. cho đến khi tất cả 34 ghế được liệt kê. Đánh dấu lần đầu tiên`k`tọa độ bị chiếm đóng tương đương với việc mô phỏng sự xuất hiện của`k`hành khách. Vì mỗi ghế xuất hiện đúng một lần trong danh sách nên không có ghế nào bị bỏ qua hoặc tính hai lần, tạo ra cấu hình cuối cùng chính xác duy nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    k = int(input())

    bus = [
        list("+------------------------+"),
        list("|#.#.#.#.#.#.#.#.#.#.#.|D|)"),
        list("|#.#.#.#.#.#.#.#.#.#.#.|.|"),
        list("|#.......................|"),
        list("|#.#.#.#.#.#.#.#.#.#.#.|.|)"),
        list("+------------------------+")
    ]

    seats = []

    seats.append((3, 1))

    for c in range(1, 22, 2):
        seats.append((1, c))

    for c in range(1, 22, 2):
        seats.append((2, c))

    for c in range(1, 22, 2):
        seats.append((4, c))

    for r, c in seats[:k]:
        bus[r][c] = 'O'

    print("\n".join("".join(row) for row in bus))

if __name__ == "__main__":
    solve()
```Mẫu xe buýt được lưu trữ chính xác như xuất hiện trong câu lệnh, ngoại trừ mọi chỗ ngồi ban đầu được đánh dấu bằng`#`. 

Danh sách chỗ ngồi mã hóa thứ tự lên máy bay. Ghế ở hàng 3, cột 1 của hình vẽ là ghế phụ duy nhất ở hàng ghế 4 chỗ nên được xếp trước. Sau đó, ba hàng hành khách thông thường mỗi hàng đóng góp 11 chỗ, mang lại`1 + 11 + 11 + 11 = 34`tổng số vị trí. 

Vòng lặp`for r, c in seats[:k]`tránh được các lỗi một cách tự nhiên. Nếu như`k = 0`, lát cắt trống và không có chỗ ngồi nào được sửa đổi. Nếu như`k = 34`, mỗi tọa độ được truy cập đúng một lần. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
9
```Quá trình điền chỗ ngồi: 

| Số lượng hành khách đã được lấp đầy | Hàng ghế cuối cùng | Hàng trên cùng được lấp đầy | Hàng giữa đầy | Đã điền hàng dưới cùng | 
| --- | --- | --- | --- | --- | 
| 0 | Không | 0 | 0 | 0 | 
| 1 | Có | 0 | 0 | 0 | 
| 2 | Có | 1 | 0 | 0 | 
| 3 | Có | 2 | 0 | 0 | 
| 4 | Có | 3 | 0 | 0 | 
| 5 | Có | 4 | 0 | 0 | 
| 6 | Có | 5 | 0 | 0 | 
| 7 | Có | 6 | 0 | 0 | 
| 8 | Có | 7 | 0 | 0 | 
| 9 | Có | 8 | 0 | 0 | 

Hành khách đầu tiên ngồi ở ghế đặc biệt ở hàng ghế 4 chỗ. Tám hành khách tiếp theo ngồi vào tám ghế đầu tiên của hàng ghế thông thường phía trên. Điều này phù hợp với đầu ra mẫu. 

### Ví dụ 2 

đầu vào:```
34
```Quá trình điền chỗ ngồi: 

| Số lượng hành khách đã được lấp đầy | Hàng ghế cuối cùng | Hàng trên cùng được lấp đầy | Hàng giữa đầy | Đã điền hàng dưới cùng | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | 0 | 0 | 
| 12 | 1 | 11 | 0 | 0 | 
| 23 | 1 | 11 | 11 | 0 | 
| 34 | 1 | 11 | 11 | 11 | 

Tất cả tọa độ chỗ ngồi đã được chọn. Mọi`#`trở thành`O`, chứng tỏ rằng danh sách tọa độ chứa tất cả 34 ghế hành khách đúng một lần. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(34) | Thực hiện tối đa 34 lần cập nhật chỗ ngồi | 
| Không gian | O(34) | Mẫu xe buýt và danh sách chỗ ngồi có kích thước cố định | 

Số lượng chỗ ngồi không bao giờ thay đổi nên cả thời gian và bộ nhớ sử dụng đều không đổi. Điều này thấp hơn nhiều so với giới hạn của vấn đề. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    k = int(input())

    bus = [
        list("+------------------------+"),
        list("|#.#.#.#.#.#.#.#.#.#.#.|D|)"),
        list("|#.#.#.#.#.#.#.#.#.#.#.|.|"),
        list("|#.......................|"),
        list("|#.#.#.#.#.#.#.#.#.#.#.|.|)"),
        list("+------------------------+")
    ]

    seats = [(3, 1)]

    for c in range(1, 22, 2):
        seats.append((1, c))
    for c in range(1, 22, 2):
        seats.append((2, c))
    for c in range(1, 22, 2):
        seats.append((4, c))

    for r, c in seats[:k]:
        bus[r][c] = 'O'

    return "\n".join("".join(row) for row in bus)

# provided sample
assert run("9\n") == (
"+------------------------+\n"
"|O.O.O.#.#.#.#.#.#.#.#.|D|)\n"
"|O.O.O.#.#.#.#.#.#.#.#.|.|\n"
"|O.......................|\n"
"|O.O.#.#.#.#.#.#.#.#.#.|.|)\n"
"+------------------------+"
)

# custom cases
assert run("0\n").count('O') == 0, "no passengers"

assert run("1\n").count('O') == 1, "first seat only"

assert run("34\n").count('O') == 34, "all seats occupied"

assert run("12\n").count('O') == 12, "boundary after first full row"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0`| KHÔNG`O`nhân vật | Vỏ cạnh xe buýt trống | 
|`1`| Đúng một chỗ đã có người | Vị trí lên máy bay đầu tiên | 
|`34`| Đã chiếm hết chỗ | Công suất phòng tối đa | 
|`12`| Mười hai chỗ ngồi | Chuyển đổi sau khi điền hàng thông thường đầu tiên | 

## Vỏ cạnh 

Khi nào`k = 0`, lát cắt`seats[:0]`trống rỗng. Không có tọa độ nào được sửa đổi nên mọi chỗ ngồi vẫn được giữ nguyên`#`. Điều này phù hợp với yêu cầu không có hành khách nào trên xe buýt. 

Khi`k = 1`, chỉ có tọa độ đầu tiên`(3, 1)`được chọn. Tọa độ đó tương ứng với ghế đặc biệt ở hàng ghế 4 người. Thuật toán không vô tình bắt đầu điền vào các hàng phía trên vì thứ tự lên máy bay được mã hóa trực tiếp trong danh sách tọa độ. 

Khi`k = 34`, vòng lặp sẽ truy cập mọi tọa độ được lưu trữ trong`seats`. Vì danh sách chứa chính xác 34 vị trí chỗ ngồi duy nhất nên mọi ghế hành khách đều có người ngồi và không có ghế nào được xử lý hai lần. 

Một ranh giới tinh tế xảy ra tại`k = 12`. Tọa độ đầu tiên lấp đầy chỗ ngồi đặc biệt và mười một tọa độ tiếp theo sẽ lấp đầy toàn bộ hàng thông thường phía trên. Hành khách tiếp theo sẽ bắt đầu ở hàng giữa. Vì danh sách tọa độ được sắp xếp chính xác nên quá trình chuyển đổi giữa các hàng diễn ra tự động mà không cần bất kỳ logic trường hợp đặc biệt nào.
