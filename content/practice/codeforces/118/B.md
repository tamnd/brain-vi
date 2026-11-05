---
title: "CF 118B - Quà của Lena"
description: "Chúng ta cần in một viên kim cương đối xứng được làm từ các con số. Hàng giữa chứa các số tăng dần từ 0 đến n, sau đó giảm dần về 0. Mọi hàng trên và dưới đều tuân theo cùng một mẫu với giá trị tối đa nhỏ hơn."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "implementation"]
categories: ["algorithms"]
codeforces_contest: 118
codeforces_index: "B"
codeforces_contest_name: "Codeforces Beta Round 89 (Div. 2)"
rating: 1000
weight: 118
solve_time_s: 102
verified: true
draft: false
---

[CF 118B - Quà từ Lena](https://codeforces.com/problemset/problem/118/B) 

**Đánh giá:** 1000 
**Tas:** thuật toán xây dựng, triển khai 
**Thời gian giải:** 1 phút 42s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta cần in một viên kim cương đối xứng được làm từ các con số. Hàng giữa chứa các số tăng dần từ`0`lên đến`n`, sau đó giảm dần về`0`. Mọi hàng bên trên và bên dưới đều tuân theo cùng một mẫu với giá trị tối đa nhỏ hơn. 

Ví dụ, khi`n = 3`, hàng giữa là:```
0 1 2 3 2 1 0
```Hàng phía trên nó dừng lại ở`2`:```
0 1 2 1 0
```Mẫu tiếp tục cho đến hàng trên cùng và dưới cùng, chỉ chứa`0`. 

Phần khó khăn là định dạng. Các hàng phải được căn chỉnh thành hình thoi bằng cách sử dụng khoảng trắng ở đầu. Các số liền kề trong một hàng cách nhau đúng một dấu cách và không bao giờ được có dấu cách ở cuối dòng. 

Ràng buộc là rất nhỏ, vì`n ≤ 9`. Ngay cả một giải pháp rất kém hiệu quả cũng sẽ phù hợp một cách thoải mái trong thời gian giới hạn. Thách thức thực sự là độ chính xác của việc triển khai, đặc biệt là khoảng cách và tính đối xứng. 

Một lỗi phổ biến là in thêm dấu cách ở cuối dòng. Ví dụ: với đầu vào:```
2
```hàng thứ ba đúng là:```
0 1 2 1 0
```Việc thực hiện bất cẩn có thể tạo ra:```
0 1 2 1 0
```với một khoảng trống ở cuối. Các thẩm phán của Codeforce so sánh kết quả đầu ra một cách chính xác, vì vậy điều này không thành công. 

Một lỗi phổ biến khác là tính toán sai thụt lề. Các khoảng trống phía trước giảm dần khi chúng ta di chuyển về phía trung tâm và tăng lên sau đó. Vì`n = 2`, hình đúng là:```
    0
  0 1 0
0 1 2 1 0
  0 1 0
    0
```Nếu chúng ta chỉ in một khoảng trống ở đầu mỗi cấp độ thay vì hai, thì hình thoi sẽ bị lệch vì bản thân các số đã chứa khoảng trắng ở giữa chúng. 

Vấn đề tế nhị thứ ba là nhân đôi số đỉnh. Ví dụ, đối với`n = 3`, hàng giữa phải là:```
0 1 2 3 2 1 0
```không:```
0 1 2 3 3 2 1 0
```Dãy giảm dần phải bắt đầu từ`i - 1`, không phải từ`i`. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là coi đầu ra như một lưới các ký tự và điền vào mọi vị trí theo cách thủ công. Chúng ta có thể phân bổ một ma trận 2D lớn, tính toán chính xác vị trí của mỗi chữ số và sau đó in từng hàng ma trận. Vì kích thước đầu ra tối đa rất nhỏ nên điều này dễ dàng hoạt động trong giới hạn. 

Vấn đề với cách tiếp cận đó không phải là hiệu suất mà là sự phức tạp không cần thiết. Việc quản lý tọa độ, khoảng cách và tính đối xứng bên trong lưới ký tự sẽ tạo ra nhiều cơ hội mắc các lỗi riêng lẻ. 

Cấu trúc của hình thoi cho khả năng quan sát rõ ràng hơn. Mỗi hàng được xác định hoàn toàn bởi ba điều: 

1. Số lượng thụt lề. 
2. Trình tự tăng dần từ`0`ĐẾN`i`. 
3. Dãy số giảm dần từ`i - 1`quay lại`0`. 

Khi chúng tôi nhận ra rằng mỗi hàng có thể được xây dựng độc lập dưới dạng một chuỗi, vấn đề sẽ trở thành việc xây dựng chuỗi đơn giản. 

Đối với hàng`i`: 

- Vết lõm là`2 * (n - i)`không gian. 
- Nửa bên trái là`0 1 2 ... i`. 
- Nửa bên phải là`i-1 ... 1 0`. 

Trước tiên, chúng tôi tạo nửa trên bao gồm hàng giữa, sau đó phản chiếu nó xuống dưới. 

Bởi vì bản thân đầu ra chứa`O(n²)`ký tự, bất kỳ giải pháp đúng phải dành ít nhất`O(n²)`thời gian in chúng. Giải pháp tối ưu phù hợp chính xác với giới hạn dưới này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Xây dựng lưới Brute Force | O(n²) | O(n²) | Đã chấp nhận | 
| Xây dựng hàng trực tiếp | O(n²) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên`n`. 
2. Lặp qua tất cả các chỉ số hàng từ`0`ĐẾN`n`. 

Mỗi chỉ mục hàng đại diện cho số lượng tối đa xuất hiện trong hàng đó. 
3. Tính các khoảng trống phía trên. 

Hàng càng xa trung tâm thì càng cần thụt lề. Mỗi cấp độ số bị thiếu đóng góp hai khoảng trắng vì các số bên trong hàng được phân tách bằng dấu cách đơn. 
4. Xây dựng dãy tăng dần từ`0`ĐẾN`i`. 

Điều này tạo thành nửa bên trái của hàng và bao gồm giá trị cao nhất. 
5. Xây dựng dãy giảm dần từ`i - 1`xuống`0`. 

Chúng tôi bắt đầu từ`i - 1`để tránh trùng lặp số đỉnh. 
6. Nối cả hai chuỗi và nối chúng bằng dấu cách đơn. 

sử dụng`" ".join(...)`đảm bảo khoảng cách chính xác và tự động tránh dấu cách. 
7. In hàng với phần thụt lề của nó. 
8. Lặp lại logic tương tự cho các hàng từ`n - 1`xuống`0`. 

Điều này tạo ra nửa đối xứng phía dưới của hình thoi. 

### Tại sao nó hoạt động 

Mỗi hàng được xác định duy nhất bởi giá trị tối đa của nó`i`. Trình tự tăng dần đảm bảo các số sẽ tăng dần về phía trung tâm, trong khi trình tự giảm dần sẽ khôi phục tính đối xứng sau đó. Việc thụt lề chỉ phụ thuộc vào khoảng cách từ hàng giữa, do đó các hàng sẽ tự động căn chỉnh thành hình thoi. 

Vì thuật toán xây dựng từng hàng theo cấu trúc toán học chính xác của mẫu nên mỗi dòng in ra đều khớp với định dạng đầu ra được yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def build_row(i, n):
    left = [str(x) for x in range(i + 1)]
    right = [str(x) for x in range(i - 1, -1, -1)]

    row = left + right
    spaces = " " * (2 * (n - i))

    return spaces + " ".join(row)

def solve():
    n = int(input())

    for i in range(n + 1):
        print(build_row(i, n))

    for i in range(n - 1, -1, -1):
        print(build_row(i, n))

solve()
```các`build_row`hàm gói gọn cấu trúc của một dòng duy nhất. Việc tách logic này sẽ tránh được sự trùng lặp vì nửa trên và nửa dưới sử dụng các quy tắc định dạng giống hệt nhau. 

Phần tăng có công dụng:```
range(i + 1)
```vậy giá trị đỉnh`i`được bao gồm. 

Phần giảm dần sử dụng:```
range(i - 1, -1, -1)
```bắt đầu một bước dưới mức đỉnh. Điều này ngăn ngừa sự trùng lặp giá trị trung tâm. 

Sử dụng thụt lề:```
2 * (n - i)
```bởi vì mỗi vị trí số thực sự chiếm hai vị trí ký tự, một cho chữ số và một cho khoảng cách ngăn cách. 

sử dụng`" ".join(row)`an toàn hơn việc thêm khoảng trắng theo cách thủ công. Nó đảm bảo chính xác một khoảng trắng giữa các số liền kề và không có dấu cách ở cuối dòng. 

Vòng lặp đầu tiên in nửa trên bao gồm cả hàng giữa. Vòng lặp thứ hai in nửa dưới được phản chiếu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2
```#### Nửa trên 

| Hàng ngang`i`| Không gian hàng đầu | Tăng Phần | Phần Giảm | Hàng cuối cùng | 
| --- | --- | --- | --- | --- | 
| 0 | 4 |`0`| trống |`    0`| 
| 1 | 2 |`0 1`|`0`|`  0 1 0`| 
| 2 | 0 |`0 1 2`|`1 0`|`0 1 2 1 0`| 

#### Nửa dưới 

| Hàng ngang`i`| Không gian hàng đầu | Tăng Phần | Phần Giảm | Hàng cuối cùng | 
| --- | --- | --- | --- | --- | 
| 1 | 2 |`0 1`|`0`|`  0 1 0`| 
| 0 | 4 |`0`| trống |`    0`| 

Dấu vết này cho thấy vết lõm co lại về phía trung tâm và sau đó mở rộng đối xứng như thế nào. 

### Ví dụ 2 

đầu vào:```
3
```#### Hàng đã chọn 

| Hàng ngang`i`| Không gian hàng đầu | Tăng Phần | Phần Giảm | Hàng cuối cùng | 
| --- | --- | --- | --- | --- | 
| 0 | 6 |`0`| trống |`      0`| 
| 1 | 4 |`0 1`|`0`|`    0 1 0`| 
| 2 | 2 |`0 1 2`|`1 0`|`  0 1 2 1 0`| 
| 3 | 0 |`0 1 2 3`|`2 1 0`|`0 1 2 3 2 1 0`| 

Ví dụ này thể hiện tính bất biến trung tâm: mỗi hàng tăng đến giá trị tối đa đúng một lần, sau đó giảm đối xứng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n²) | có`2n + 1`hàng, mỗi hàng chứa tối đa`2n + 1`số | 
| Không gian | O(n) | Danh sách tạm thời được sử dụng để xây dựng một hàng | 

Các ràng buộc là cực kỳ nhỏ, vì`n ≤ 9`. Giải pháp chạy ngay lập tức và sử dụng bộ nhớ không đáng kể. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    def build_row(i, n):
        left = [str(x) for x in range(i + 1)]
        right = [str(x) for x in range(i - 1, -1, -1)]

        row = left + right
        spaces = " " * (2 * (n - i))

        return spaces + " ".join(row)

    n = int(input())

    out = []

    for i in range(n + 1):
        out.append(build_row(i, n))

    for i in range(n - 1, -1, -1):
        out.append(build_row(i, n))

    sys.stdout.write("\n".join(out))

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    return sys.stdout.getvalue()

# provided sample
assert run("2\n") == (
    "    0\n"
    "  0 1 0\n"
    "0 1 2 1 0\n"
    "  0 1 0\n"
    "    0"
)

# minimum valid n
assert run("0\n") == "0"

# small symmetric case
assert run("1\n") == (
    "  0\n"
    "0 1 0\n"
    "  0"
)

# larger case checks center row
assert "0 1 2 3 4 3 2 1 0" in run("4\n")

# maximum constraint
out = run("9\n")
assert "0 1 2 3 4 5 6 7 8 9 8 7 6 5 4 3 2 1 0" in out

# no duplicated peak value
assert "0 1 2 2 1 0" not in run("2\n")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0`| Đơn`0`hàng | Hình thoi nhỏ nhất có thể | 
|`1`| Ba hàng đối xứng | Phản chiếu trên/dưới cơ bản | 
|`4`| Hàng giữa đúng | Xây dựng tăng giảm | 
|`9`| Mẫu hợp lệ lớn nhất | Xử lý hạn chế tối đa | 
|`2`với kiểm tra trùng lặp | Không có giá trị trung tâm lặp lại | Off-by-one trong trình tự giảm dần | 

## Vỏ cạnh 

Xét hình thoi nhỏ nhất có thể:```
0
```Thuật toán xử lý việc này một cách tự nhiên. Vòng lặp trên chạy một lần với`i = 0`, chỉ tạo ra số`0`. Vòng lặp dưới bắt đầu từ`-1`, vì vậy nó không thực thi. Không cần xử lý đặc biệt. 

Một trường hợp tinh tế khác là tránh các giá trị trung tâm trùng lặp. Đối với đầu vào:```
2
```hàng giữa phải là:```
0 1 2 1 0
```Trong quá trình thi công: 

- Trình tự tăng dần trở thành`0 1 2`- Dãy số giảm trở thành`1 0`Phần giảm bắt đầu từ`i - 1`, Vì thế`2`không được lặp lại. 

Căn chỉnh khoảng cách là một nguồn lỗi phổ biến khác. Đối với đầu vào:```
1
```hàng trên cùng yêu cầu chính xác hai khoảng trắng ở đầu:```
  0
```Công thức`2 * (n - i)`mang lại:```
2 * (1 - 0) = 2
```mà căn chỉnh hình thoi một cách chính xác. Chỉ sử dụng`n - i`khoảng trắng sẽ dịch chuyển các hàng quá xa về bên trái và phá vỡ hình dạng kim cương.
