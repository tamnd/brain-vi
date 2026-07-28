---
title: "CF 102741C - Trang trí lại của Isabelle"
description: "Căn phòng là một lưới N × M gồm các ô sàn hình vuông. Isabelle muốn thay thế toàn bộ lưới bằng những viên gạch hình chữ L giống hệt nhau. Mỗi ô bao gồm bốn ô: ba ô thẳng hàng và một ô phụ được gắn vào một đầu và ô có thể xoay được."
date: "2026-07-29T00:43:54+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102741
codeforces_index: "C"
codeforces_contest_name: "UTPC Contest 9-25-20 Div. 1"
rating: 0
weight: 102741
solve_time_s: 63
verified: true
draft: false
---

[CF 102741C - Trang trí lại của Isabelle](https://codeforces.com/problemset/problem/102741/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 3s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Căn phòng là một`N × M`lưới các ô sàn vuông. Isabelle muốn thay thế toàn bộ lưới bằng những viên gạch hình chữ L giống hệt nhau. Mỗi ô bao gồm bốn ô: ba ô thẳng hàng và một ô phụ được gắn vào một đầu và ô có thể xoay được. Nhiệm vụ là quyết định xem liệu cách sắp xếp các ô này có thể bao phủ toàn bộ hình chữ nhật mà không có khoảng trống hoặc chồng lên nhau hay không. 

Một ô luôn bao phủ chính xác bốn ô, vì vậy hạn chế đầu tiên đến từ diện tích. Diện tích hình chữ nhật phải chia hết cho bốn. Tuy nhiên, chỉ điều kiện này thôi là chưa đủ. L-tetrominoes có hạn chế chẵn lẻ mạnh hơn: mọi hình chữ nhật hợp lệ cũng cần có diện tích chia hết cho 8 và hình chữ nhật có cạnh dài bằng 1 không thể che được vì mỗi ô chiếm ít nhất hai hàng và hai cột. 

Giá trị đầu vào tối đa là 1000, nghĩa là giải pháp phải dựa trên quan sát toán học thay vì mô phỏng các vị trí. Một lưới có tới một triệu ô đã quá lớn để tìm kiếm các cách sắp xếp ô có thể. Giải pháp quay lui sẽ phân nhánh trên nhiều vị trí ô có thể và nhanh chóng trở nên không khả thi. 

Những trường hợp phức tạp là những trường hợp mà diện tích có thể phân chia đủ nhưng hình học lại loại bỏ hình chữ nhật. 

Ví dụ, một căn phòng có kích thước`1 8`có khu vực thứ tám, nhưng câu trả lời là`NO`. Một giải pháp bất cẩn chỉ kiểm tra`N*M % 8 == 0`sẽ chấp nhận nó, mặc dù một hàng không thể chứa một ô hình chữ L. 

Một ví dụ khác là`2 2`. Diện tích là bốn, nhưng câu trả lời là`NO`. Hình chữ nhật quá nhỏ để chứa dù chỉ một L-tetromino. 

Trường hợp thỏa mãn điều kiện là`2 4`. Diện tích là tám và cả hai chiều đều lớn hơn một, vì vậy câu trả lời là`YES`. Hai viên gạch hình chữ L có thể được xếp để lấp đầy hình chữ nhật. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là thử đặt từng viên gạch lên sàn. Tìm kiếm brute-force có thể chọn một ô không được che chắn, thử mọi khả năng xoay của ô hình chữ L bao phủ ô đó và tiếp tục đệ quy. Điều này đúng vì nó khám phá mọi cách ốp lát có thể. Vấn đề là số lượng các cách sắp xếp có thể tăng theo cấp số nhân. Ngay cả đối với một hình chữ nhật có kích thước vừa phải, cây tìm kiếm cũng trở nên khổng lồ vì nhiều thứ tự ô xếp khác nhau thể hiện cùng một cách sắp xếp cuối cùng. 

Quan sát hữu ích là không cần phải xây dựng vị trí chính xác. Chúng ta chỉ cần nhận biết khi nào một hình chữ nhật có kiểu xếp hợp lệ. L-tetromino bao gồm bốn ô, nhưng đối số tô màu cột đưa ra điều kiện mạnh hơn: một ô xếp hợp lệ luôn sử dụng số ô chẵn, có nghĩa là tổng diện tích phải chia hết cho 8. Điều này loại bỏ nhiều hình chữ nhật ngay lập tức. 

Câu hỏi còn lại là liệu điều kiện này có đủ hay không. Nếu cả hai kích thước đều lớn hơn một và diện tích chia hết cho 8 thì hình chữ nhật luôn có thể được chia thành các hình chữ nhật nhỏ hơn có thể xếp được. Khi một cạnh chẵn, hình chữ nhật có thể được chia thành`2 × 4`các khối và mỗi khối như vậy có thể được lấp đầy bởi hai L-tetromino. Khi một cạnh là số lẻ, điều kiện diện tích buộc cạnh kia là bội số của 8, cho phép hình chữ nhật được chia thành`3 × 8`khối và`2 × 4`khối. Cả hai loại khối đều có các ô L-tetromino hợp lệ. 

Toàn bộ vấn đề quy về việc kiểm tra hai điều kiện: căn phòng không được có kích thước bằng một và diện tích của nó phải chia hết cho 8. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(NM) | Quá chậm | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc kích thước hai phòng`N`Và`M`. 
2. Kiểm tra xem một trong hai chiều có bằng một hay không. Nếu có thì xuất ra`NO`bởi vì L-tetromino không thể vừa với một hàng hoặc một cột. 
3. Tính toán`N * M`và kiểm tra xem nó có chia hết cho 8 không. Điều kiện chia hết nắm bắt giới hạn số lượng ô được tạo bởi hình dạng của L-tetromino. 
4. Đầu ra`YES`nếu cả hai điều kiện đều được thỏa mãn. Ngược lại, xuất ra`NO`. 

Tại sao nó hoạt động: Mọi lát gạch hợp lệ phải đáp ứng giới hạn diện tích và giới hạn kích thước, do đó, việc từ chối các hình chữ nhật không đáp ứng một trong hai điều kiện là an toàn. Đối với mọi hình chữ nhật thỏa mãn cả hai điều kiện, hình chữ nhật có thể được phân tách thành các vùng nhỏ hơn có thể xếp được riêng lẻ bằng L-tetrominoes. Các điều kiện đều cần và đủ nên thuật toán luôn trả về câu trả lời đúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m = map(int, input().split())

if n == 1 or m == 1:
    print("NO")
elif (n * m) % 8 == 0:
    print("YES")
else:
    print("NO")
```Mã đọc hai chiều và ngay lập tức áp dụng hai phép kiểm tra toán học từ thuật toán. Điều kiện đầu tiên xử lý các phòng mỏng, nơi không thể bố trí được. Điều kiện thứ hai sử dụng giới hạn diện tích. 

phép nhân`n * m`là an toàn trong Python vì số nguyên có độ chính xác tùy ý, mặc dù ngay cả số nguyên 32 bit có chiều rộng cố định cũng đủ cho các giới hạn nhất định vì diện tích tối đa chỉ là một triệu. 

Không có vòng lặp, do đó không có vấn đề về truyền tải ranh giới hoặc lỗi sai lệch. Sai lầm duy nhất có thể xảy ra là quên trường hợp một chiều và chấp nhận các hình chữ nhật có diện tích chia hết cho 8. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
6 4
```Thuật toán kiểm tra hai điều kiện: 

| Bước | N | M | Khu vực | Quyết định | 
| --- | --- | --- | --- | --- | 
| Ban đầu | 6 | 4 | 24 | Cả hai chiều đều lớn hơn một | 
| Kiểm tra khu vực | 6 | 4 | 24 | 24 chia hết cho 8 | 
| Cuối cùng | 6 | 4 | 24 | CÓ | 

Hình chữ nhật có đủ diện tích và kích thước để chia thành các phần có thể xếp được hợp lệ. 

### Mẫu 2 

đầu vào:```
9 6
```| Bước | N | M | Khu vực | Quyết định | 
| --- | --- | --- | --- | --- | 
| Ban đầu | 9 | 6 | 54 | Cả hai chiều đều lớn hơn một | 
| Kiểm tra khu vực | 9 | 6 | 54 | 54 không chia hết cho 8 | 
| Cuối cùng | 9 | 6 | 54 | KHÔNG | 

Hình chữ nhật có hình dạng đúng nhưng số lượng ô không thể chia thành các nhóm phủ sóng L-tetromino hoàn chỉnh. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Thuật toán chỉ thực hiện một vài phép kiểm tra số học. | 
| Không gian | O(1) | Không có cấu trúc dữ liệu bổ sung nào được tạo. | 

Giải pháp dễ dàng phù hợp với các ràng buộc vì nó không phụ thuộc vào kích thước của lưới. Ngay cả căn phòng lớn nhất có thể,`1000 × 1000`, được xử lý bằng công việc liên tục. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve(data: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(data)
    n, m = map(int, sys.stdin.readline().split())

    if n == 1 or m == 1:
        ans = "NO"
    elif (n * m) % 8 == 0:
        ans = "YES"
    else:
        ans = "NO"

    sys.stdin = old_stdin
    return ans

# provided samples
assert solve("6 4\n") == "YES", "sample 1"
assert solve("9 6\n") == "NO", "sample 2"

# custom cases
assert solve("1 8\n") == "NO", "single row cannot fit L tiles"
assert solve("2 4\n") == "YES", "small valid rectangle"
assert solve("2 2\n") == "NO", "area is not enough"
assert solve("1000 1000\n") == "YES", "maximum size case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 8`|`NO`| Từ chối phòng một chiều | 
|`2 4`|`YES`| Khẳng định công trình có giá trị chung nhỏ nhất | 
|`2 2`|`NO`| Giải pháp nắm bắt chỉ kiểm tra khả năng chia cắt diện tích | 
|`1000 1000`|`YES`| Xác nhận đầu vào lớn được xử lý trong thời gian không đổi | 

## Vỏ cạnh 

cho`1 8`, trước tiên thuật toán thấy rằng một chiều là một và ngay lập tức trả về`NO`. Mặc dù diện tích chia hết cho 8 nhưng mọi L-tetromino đều cần không gian theo hai hướng nên hình chữ nhật không thể bị che phủ. 

Vì`2 2`, diện tích chỉ có bốn. Thuật toán từ chối nó vì bốn không chia hết cho tám. Giải pháp chỉ dựa trên thực tế là mỗi ô bao gồm bốn ô sẽ chấp nhận trường hợp này một cách không chính xác. 

Vì`2 4`, thuật toán kiểm tra xem cả hai chiều có lớn hơn một và diện tích bằng 8 hay không. Nó trở lại`YES`. Đây là một cách xây dựng cơ sở hợp lệ trong đó hai L-tetromino bao phủ chính xác hình chữ nhật. 

Vì`9 6`, thuật toán đạt đến kiểm tra khu vực sau khi vượt qua kiểm tra kích thước. Diện tích là`54`, Và`54 % 8`không bằng 0 nên nó trả về`NO`. Hình chữ nhật đủ lớn về mặt hình học, nhưng số lượng ô không thể được phân chia thành các nhóm ô L hợp lệ.
