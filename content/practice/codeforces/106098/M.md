---
title: "CF 106098M - MEDAA, Farouk và Hói"
description: "Chúng ta được cho một số nguyên dương n dưới dạng một chuỗi nhị phân. Chúng ta cần đếm có bao nhiêu số nguyên dương x tồn tại sao cho biểu diễn nhị phân của x không có nhiều bit hơn n và ba giá trị n + x, n Phần mấu chốt của bài toán là hiểu khi nào phép cộng hoạt động chính xác…"
date: "2026-06-25T11:57:15+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106098
codeforces_index: "M"
codeforces_contest_name: "The American University in Cairo CSEA Fall 2025 contest"
rating: 0
weight: 106098
solve_time_s: 56
verified: true
draft: false
---

[CF 106098M – MEDAA, Farouk và Bald](https://codeforces.com/problemset/problem/106098/M) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 56s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một số nguyên dương`n`dưới dạng một chuỗi nhị phân. Ta cần đếm có bao nhiêu số nguyên dương`x`tồn tại sao cho biểu diễn nhị phân của`x`không có nhiều bit hơn`n`, và ba giá trị`n + x`,`n | x`, Và`n ^ x`đều giống hệt nhau. Câu trả lời phải được in modulo`10^9 + 7`. 

Phần quan trọng của vấn đề là hiểu được khi nào phép cộng hoạt động giống hệt như các phép toán theo bit. Trong phép cộng nhị phân, sự khác biệt duy nhất giữa`n + x`Và`n | x`đến từ mang. Nếu một bit được thiết lập trong cả hai`n`Và`x`, việc cộng hai bit đó sẽ tạo ra một số mang. Nếu không có vị trí nào mà cả hai số đều có`1`, việc bổ sung chỉ là đặt các bit hiện có lại với nhau, đó chính xác là những gì OR thực hiện. 

Điều kiện thứ hai là OR và XOR phải khớp nhau. Tại một thời điểm mà cả hai số đều có`1`, HOẶC tạo ra`1`trong khi XOR tạo ra`0`. Tại tất cả các vị trí bit khác chúng bằng nhau. Vậy điều kiện này cũng đòi hỏi`n`Và`x`không bao giờ chia sẻ một chút. 

Cả hai yêu cầu đều giảm xuống cùng một điều kiện đơn giản:```
n & x = 0
```Bây giờ vấn đề trở thành đếm các số nguyên dương`x`với nhiều nhất`m`bit, ở đâu`m`là độ dài của chuỗi nhị phân, chỉ sử dụng các vị trí trong đó`n`có số không. 

Giới hạn độ dài có nghĩa là chỉ số bit có thể sử dụng cao nhất là`m - 1`. Bit hàng đầu của`n`luôn luôn là`1`, do đó vị trí đó không bao giờ có thể được sử dụng bởi`x`. Mọi số 0 trong số các vị trí còn lại đều là sự lựa chọn tự do: chúng ta có thể đặt một trong hai`0`hoặc`1`TRONG`x`. Từ`x`phải dương, chúng ta không thể chọn tất cả số không. 

Kích thước đầu vào có thể đạt tới`10^6`bit. Điều này loại trừ việc chuyển đổi chuỗi nhị phân thành một số nguyên bình thường và sử dụng các vòng lặp số học trên giá trị, vì bản thân giá trị đó quá lớn. Thuật toán phải xử lý chuỗi trực tiếp và thực hiện một lượng công việc không đổi trên mỗi bit, đưa ra một`O(m)`giải pháp. 

Một sai lầm phổ biến là quên điều đó`x`không thể bằng không. Ví dụ, nếu`n = 111`, mọi bit đều bị chiếm. Số duy nhất không có bit tập hợp chồng chéo là`0`, nhưng nó không được phép, nên câu trả lời là`0`. 

Một trường hợp cạnh khác là khi có chính xác một bit trống. Đối với đầu vào:```
m = 3
n = 101
```Các giá trị hợp lệ của`x`chỉ là`{2}`bởi vì`010`là mặt nạ tích cực duy nhất tránh được các bit đã đặt của`n`. Đầu ra đúng là:```
1
```Giải pháp đếm tất cả các mặt nạ mà không trừ mặt nạ trống sẽ xuất ra không chính xác`2`. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu là cố gắng mọi cách có thể`x`từ`1`ĐẾN`2^m - 1`, kiểm tra xem`n & x`bằng 0 và đếm các lựa chọn hợp lệ. Điều này đúng vì điều kiện rút ra ở trên vừa cần vừa đủ. Vấn đề là ở chỗ đó`m`có thể`10^6`, do đó số lượng ứng cử viên là theo cấp số nhân. Ngay cả đối với vài trăm bit, việc liệt kê là không thể. 

Quan sát hữu ích là các bit của`x`là độc lập. Đối với mỗi vị trí bit ở đó`n`có số không,`x`có thể chọn một trong hai`0`hoặc`1`. Đối với mỗi vị trí bit ở đó`n`có một cái,`x`buộc phải lựa chọn`0`. 

Nếu có`cnt`0 bit bên trong chiều dài của`n`, có`2^cnt`mặt nạ có thể bao gồm cả mặt nạ hoàn toàn bằng không. Từ`x`phải tích cực, câu trả lời cuối cùng là`2^cnt - 1`. 

Thử thách duy nhất còn lại là tính toán`cnt`. Chúng tôi quét chuỗi nhị phân và đếm số ký tự bằng 0. Bởi vì ký tự đầu tiên luôn là`1`, đếm trực tiếp tất cả các ký tự 0 sẽ cho số lượng vị trí trống. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^m) | O(1) | Quá chậm | 
| Tối ưu | O(m) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc độ dài của chuỗi nhị phân và chính chuỗi đó. Độ dài chỉ cần thiết để biết số lượng vị trí bit có sẵn. 
2. Đếm xem chuỗi có bao nhiêu ký tự`0`. Mỗi vị trí như vậy đại diện cho một chút nơi`x`có thể tự do lựa chọn xem nó có được thiết lập hay không. 
3. Tính toán`2^cnt`modulo`10^9 + 7`, Ở đâu`cnt`là số bit 0. 
4. Trừ một kết quả khỏi kết quả để loại bỏ lựa chọn không hợp lệ trong đó mỗi bit của`x`là số không. 
5. In giá trị kết quả theo modulo`10^9 + 7`. 

Tại sao nó hoạt động: mọi thứ đều hợp lệ`x`tương ứng với việc chọn một tập hợp con các vị trí 0 của`n`. Hai tập con khác nhau tạo ra hai số nguyên dương khác nhau và mọi tập con như vậy đều thỏa mãn`n & x = 0`. Tập con duy nhất không được phép là tập con trống, nó tạo ra`x = 0`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def solve():
    m_line = input().strip()
    if not m_line:
        return
    m = int(m_line)
    s = input().strip()

    cnt = s.count('0')
    ans = pow(2, cnt, MOD) - 1
    print(ans % MOD)

if __name__ == "__main__":
    solve()
```Việc thực hiện trực tiếp theo đối số đếm. Thao tác chuỗi`count('0')`cung cấp số lượng vị trí bit có thể sử dụng mà không cần chuyển đổi số nhị phân thành số nguyên. 

Công suất được tính theo lũy thừa mô-đun vì số lượng bit 0 có thể lớn bằng`10^6`. của Python`pow`với một mô đun giữ cho các giá trị trung gian nhỏ và chạy theo thời gian logarit đối với số mũ. 

Phép trừ một được thực hiện sau khi lấy modulo lũy thừa`MOD`. Hoạt động modulo cuối cùng xử lý trường hợp giá trị trở thành âm. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
5
11010
```Chuỗi có ba bit 0. Dấu vết là: 

| Bước | Giá trị hiện tại | Không có bit nào được tính | Kết quả | 
| --- | --- | --- | --- | 
| Bắt đầu | 11010 | 0 | Chưa có lựa chọn nào | 
| Đọc bit 1 | 1 | 0 | Bit bị chặn | 
| Đọc bit 2 | 1 | 0 | Bit bị chặn | 
| Đọc bit 3 | 0 | 1 | Một sự lựa chọn miễn phí | 
| Đọc bit 4 | 1 | 1 | Bit bị chặn | 
| Đọc bit 5 | 0 | 2 | Hai sự lựa chọn miễn phí | 
| Kết thúc | 11010 | 2 |`2^2 - 1 = 3`| 

Hai vị trí số 0 có thể tạo mặt nạ`01`,`10`, Và`11`ở những vị trí đó. Mặt nạ trống được loại trừ. 

### Ví dụ tùy chỉnh 2 

đầu vào:```
4
1111
```Dấu vết là: 

| Bước | Giá trị hiện tại | Không có bit nào được tính | Kết quả | 
| --- | --- | --- | --- | 
| Bắt đầu | 1111 | 0 | Chưa có lựa chọn nào | 
| Đọc bit 1 | 1 | 0 | Bit bị chặn | 
| Đọc bit 2 | 1 | 0 | Bit bị chặn | 
| Đọc bit 3 | 1 | 0 | Bit bị chặn | 
| Đọc bit 4 | 1 | 0 | Bit bị chặn | 
| Kết thúc | 1111 | 0 |`2^0 - 1 = 0`| 

Không có số dương nào tránh được tất cả các bit đã đặt của`n`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m) | Chuỗi được quét một lần và lũy thừa mô-đun là logarit theo số bit 0. | 
| Không gian | O(1) | Chỉ có bộ đếm số 0 và câu trả lời được lưu trữ. | 

Giải pháp phù hợp với giới hạn vì thao tác duy nhất tỷ lệ thuận với kích thước đầu vào là đọc và quét chuỗi nhị phân một lần. Ngay cả với một triệu ký tự, điều này vẫn dễ dàng nằm trong giới hạn lập trình cạnh tranh thông thường. 

## Trường hợp thử nghiệm```python
import sys
import io

MOD = 10**9 + 7

def solve_io(data: str) -> str:
    sys.stdin = io.StringIO(data)
    input = sys.stdin.readline

    m_line = input().strip()
    if not m_line:
        return ""

    m = int(m_line)
    s = input().strip()

    cnt = s.count("0")
    return str((pow(2, cnt, MOD) - 1) % MOD)

# sample 1
assert solve_io("5\n11010\n") == "3", "sample 1"

# all bits are one
assert solve_io("4\n1111\n") == "0", "no valid x"

# only one zero bit
assert solve_io("3\n101\n") == "1", "single choice"

# minimum size
assert solve_io("1\n1\n") == "0", "one bit number"

# many zeros
assert solve_io("6\n100000\n") == "31", "all lower bits are free"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`5 / 11010`|`3`| Trường hợp đếm mẫu | 
|`4 / 1111`|`0`| Không có vị trí bit sẵn có | 
|`3 / 101`|`1`| Chính xác một giá trị dương hợp lệ | 
|`1 / 1`|`0`| Ranh giới chiều dài tối thiểu | 
|`6 / 100000`|`31`| Số lượng lớn các vị trí miễn phí | 

## Vỏ cạnh 

cho`n = 111`, mọi vị trí đều đã chứa một`1`. Thuật toán đếm số vị trí trống bằng 0, tính toán`2^0 - 1`, và trả về`0`. Điều này phù hợp với thực tế là không có tích cực`x`có thể tránh chồng chéo với`n`. 

Vì`n = 101`, vị trí 0 duy nhất là bit ở giữa. Thuật toán đếm một vị trí trống và tính toán`2^1 - 1 = 1`. Giá trị hợp lệ duy nhất là`x = 010`, Và`101 & 010 = 0`, do đó cả ba biểu thức đều bằng nhau. 

Đối với một số như`n = 100000`, mọi bit thấp hơn đều miễn phí. Có năm vị trí 0, vì vậy mọi tập hợp con không trống của các vị trí đó đều hợp lệ. Câu trả lời trở thành`2^5 - 1 = 31`, điều này cho thấy thuật toán xử lý chính xác trường hợp có số lượng lựa chọn tối đa.
