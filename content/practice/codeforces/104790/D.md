---
title: "CF 104790D - Đặt tên dân chủ"
description: "Mỗi thành phố trong quận mới đều có tên có cùng độ dài. Tên cuối cùng của quận được chọn từng ký tự một. Đối với mỗi vị trí, mỗi thành phố sẽ bỏ phiếu cho chữ cái xuất hiện ở vị trí đó trong tên riêng của mình."
date: "2026-06-28T16:41:29+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104790
codeforces_index: "D"
codeforces_contest_name: "2023 Benelux Algorithm Programming Contest (BAPC 23)"
rating: 0
weight: 104790
solve_time_s: 75
verified: true
draft: false
---

[CF 104790D - Cách đặt tên dân chủ](https://codeforces.com/problemset/problem/104790/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 15s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi thành phố trong quận mới đều có tên có cùng độ dài. Tên cuối cùng của quận được chọn từng ký tự một. Đối với mỗi vị trí, mỗi thành phố sẽ bỏ phiếu cho chữ cái xuất hiện ở vị trí đó trong tên riêng của mình. Chữ cái có số phiếu bầu cao nhất sẽ trở thành chữ cái tương ứng của tên quận. Nếu nhiều chữ cái nhận được số phiếu bầu cao nhất như nhau thì chữ cái nhỏ nhất theo thứ tự bảng chữ cái sẽ được chọn. 

Dữ liệu đầu vào bao gồm số lượng thành phố, độ dài chung của mỗi tên thành phố và sau đó là tên của chính chúng. Đầu ra bắt buộc là tên duy nhất được tạo bằng cách áp dụng quy tắc biểu quyết một cách độc lập ở mọi vị trí ký tự. 

Những hạn chế là khá nhỏ. Có nhiều nhất 1000 tên thành phố, mỗi tên chứa tối đa 1000 ký tự. Điều này có nghĩa là có tổng cộng tối đa một triệu ký tự đầu vào. Bất kỳ thuật toán nào xử lý mỗi ký tự một lần hoặc một số lần không đổi đều dễ dàng phù hợp với giới hạn cuộc thi thông thường. Các thuật toán liên tục quét lại tất cả các chuỗi cho mọi câu trả lời có thể vẫn được chấp nhận ở đây, nhưng không có lý do gì để thực hiện thêm công việc khi đã có giải pháp đếm trực tiếp. 

Trường hợp cạnh không rõ ràng đầu tiên là sự ràng buộc giữa nhiều chữ cái. Xem xét đầu vào```
2 1
b
a
```Cả hai chữ cái đều nhận được một phiếu bầu, vì vậy kết quả đúng là```
a
```Việc triển khai bất cẩn chỉ giữ mức tối đa đầu tiên gặp phải sẽ tạo ra kết quả không chính xác.`b`. 

Một trường hợp khác là khi mọi thành phố đều có tên giống nhau. Ví dụ,```
3 4
code
code
code
```Câu trả lời phải là```
code
```Vì mọi quan điểm đều có sự đồng thuận nhất trí nên không có logic ràng buộc nào được can thiệp. 

Trường hợp cuối cùng là mỗi vị trí đều có người chiến thắng khác nhau. Ví dụ,```
3 3
abc
bbc
cac
```Đầu ra là```
abc
```Mỗi cột phải được xử lý độc lập. Kết hợp thông tin từ các vị trí khác nhau sẽ tạo ra kết quả sai. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất là xử lý từng vị trí riêng biệt. Đối với một cột, hãy đếm số lần mỗi chữ cái xuất hiện trong số tất cả các tên thành phố, sau đó chọn chữ cái có tần suất cao nhất. Nếu nhiều chữ cái có cùng tần số, hãy chọn chữ cái nhỏ nhất theo thứ tự bảng chữ cái. Lặp lại điều này cho tất cả các vị trí sẽ tạo ra câu trả lời cần thiết. 

Cách tiếp cận này chỉ thực hiện một lần chuyển qua mỗi ký tự đầu vào. Vì có nhiều nhất một triệu ký tự nên tổng công việc là khoảng một triệu thao tác đếm cộng với việc kiểm tra 26 chữ cái viết thường cho mỗi vị trí. Điều đó dễ dàng đủ nhanh. 

Điều quan trọng là mọi vị trí ký tự đều hoàn toàn độc lập với mọi vị trí khác. Quyết định về bức thư đầu tiên không bao giờ ảnh hưởng đến quyết định về bức thư thứ hai. Vì bảng chữ cái chỉ chứa 26 chữ cái viết thường nên việc duy trì dải tần số có kích thước 26 cho mỗi cột vừa đơn giản vừa hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
|---|---:|---:|---| 
| Lực lượng vũ phu | O(n × m × 26) | O(26) | Đã chấp nhận | 
| Tối ưu | O(n × m + 26 × m) | O(26) | Đã chấp nhận | 

Mặc dù cả hai hàng về cơ bản đều có độ phức tạp tiệm cận như nhau vì kích thước bảng chữ cái là không đổi, công thức đếm tần số là giải pháp tự nhiên và được dự định trước. 

## Hướng dẫn thuật toán 

1. Đọc giá trị của`n`Và`m`, sau đó lưu trữ tất cả tên thành phố. 

2. Tạo một danh sách trống chứa các ký tự của tên quận cuối cùng. 

3. Đối với mọi vị trí ký tự từ`0`ĐẾN`m - 1`, tạo một mảng tần số có độ dài 26 được khởi tạo bằng 0. 

4. Truy cập từng tên thành phố và tăng bộ đếm tương ứng với chữ cái xuất hiện ở vị trí hiện tại. 

Mỗi thành phố đóng góp chính xác một phiếu bầu vào cột hiện tại, do đó, sau lần vượt qua này, mảng tần số chứa kết quả bầu cử đầy đủ cho vị trí đó. 

5. Quét 26 quầy theo thứ tự bảng chữ cái và theo dõi chữ cái có tần suất lớn nhất. 

Bởi vì quá trình quét được thực hiện từ`'a'`ĐẾN`'z'`, chỉ cập nhật câu trả lời khi tìm thấy tần số lớn hơn nghiêm ngặt sẽ tự động giữ chữ cái nhỏ nhất theo thứ tự bảng chữ cái bất cứ khi nào tần số bị ràng buộc. 

6. Thêm chữ cái đã chọn vào câu trả lời. 

7. Sau khi tất cả các vị trí đã được xử lý, nối các chữ cái đã thu thập thành một chuỗi và in ra. 

### Tại sao nó hoạt động 

Đối với mỗi vị trí, thuật toán sẽ tính chính xác số phiếu bầu mà mỗi lá thư có thể nhận được. Những con số này khớp chính xác với định nghĩa của cuộc bầu cử. Việc chọn chữ cái có số lượng lớn nhất thỏa mãn quy tắc đa số và việc quét các chữ cái theo thứ tự bảng chữ cái đảm bảo rằng các mối quan hệ được giải quyết theo hướng có lợi cho chữ cái nhỏ nhất. Vì mọi vị trí được xử lý độc lập nên mọi ký tự của tên được xây dựng đều chính xác, làm cho toàn bộ đầu ra chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m = map(int, input().split())
names = [input().strip() for _ in range(n)]

answer = []

for col in range(m):
    freq = [0] * 26

    for name in names:
        freq[ord(name[col]) - ord('a')] += 1

    best = 0
    for i in range(1, 26):
        if freq[i] > freq[best]:
            best = i

    answer.append(chr(best + ord('a')))

print("".join(answer))
```Chương trình bắt đầu bằng cách đọc tất cả tên thành phố để có thể truy cập từng cột một cách hiệu quả. 

Đối với mỗi cột, nó tạo ra một mảng tần số mới với một mục nhập cho mỗi chữ cái viết thường. Mỗi thành phố đóng góp một phiếu bầu bằng cách tăng số phiếu bầu thích hợp. 

Biến`best`lưu trữ chỉ mục của bức thư chiến thắng hiện tại. Quá trình quét bắt đầu bằng`'a'`với tư cách là ứng cử viên ban đầu. Việc so sánh sử dụng`>`thay vì`>=`. Chi tiết này là những gì thực hiện quy tắc ràng buộc. Khi hai chữ cái có tần số bằng nhau, chữ cái trước đó trong bảng chữ cái vẫn được chọn vì nó xuất hiện đầu tiên. 

Cuối cùng, các chữ cái đã chọn sẽ được chuyển đổi lại thành các ký tự, được tập hợp thành một danh sách và nối thành tên quận cuối cùng. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào```
3 5
apple
maple
alpha
```| Vị trí | Thư | Thư trúng thưởng | Trả lời cho đến nay | 
|---:|---|---|---| 
| 0 | một, m, một | một | một | 
| 1 | p, a, l | một | aa | 
| 2 | p, p, p | p | aap | 
| 3 | l, l, h | tôi | aapl | 
| 4 | đ, đ, a | e | táo | 

Câu trả lời cuối cùng là`aaple`. Cột thứ hai thể hiện quy tắc hòa. các chữ cái`a`,`l`, Và`p`mỗi cái xuất hiện một lần, nên chữ cái nhỏ nhất,`a`, được chọn. 

### Mẫu 2 

đầu vào```
3 4
icpc
back
laps
```| Vị trí | Thư | Thư trúng thưởng | Trả lời cho đến nay | 
|---:|---|---|---| 
| 0 | tôi, b, l | b | b | 
| 1 | c, a, a | một | ba | 
| 2 | p, c, p | p | bap | 
| 3 | c, k, s | c | bapc | 

Câu trả lời cuối cùng là`bapc`. Mỗi cột được xử lý độc lập, xác nhận rằng các quyết định trước đó không bao giờ ảnh hưởng đến những quyết định sau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
|---|---:|---| 
| Thời gian | O(n × m + 26 × m) | Mỗi ký tự đầu vào được đếm một lần, sau đó 26 bộ đếm được kiểm tra cho mỗi cột. | 
| Không gian | O(26) | Chỉ cần một mảng tần số có kích thước cố định bên cạnh đầu vào. | 

Tổng số ký tự được xử lý tối đa là một triệu và mỗi ký tự được xử lý chính xác một lần. Việc quét thêm 26 chữ cái trên mỗi cột là không đáng kể. Thuật toán phù hợp thoải mái trong các ràng buộc của vấn đề. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve():
    input = sys.stdin.readline

    n, m = map(int, input().split())
    names = [input().strip() for _ in range(n)]

    ans = []

    for col in range(m):
        freq = [0] * 26
        for name in names:
            freq[ord(name[col]) - ord('a')] += 1

        best = 0
        for i in range(1, 26):
            if freq[i] > freq[best]:
                best = i

        ans.append(chr(best + ord('a')))

    print("".join(ans))

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out

# provided samples
assert run("3 5\napple\nmaple\nalpha\n") == "aaple\n", "sample 1"
assert run("3 4\nicpc\nback\nlaps\n") == "bapc\n", "sample 2"

# minimum size
assert run("1 1\nz\n") == "z\n", "single city"

# tie breaking
assert run("2 1\nb\na\n") == "a\n", "alphabetical tie"

# all equal
assert run("3 3\ncat\ncat\ncat\n") == "cat\n", "identical names"

# different winners per column
assert run("3 3\nabc\nbbc\ncac\n") == "abc\n", "independent columns"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
|---|---|---| 
| Một thành phố với một chữ cái |`z`| Kích thước đầu vào tối thiểu | 
| Hai cái tên`b`Và`a`|`a`| Liên kết theo bảng chữ cái | 
| Ba cái tên giống hệt nhau | Cùng tên | Nhất trí biểu quyết | 
|`abc`,`bbc`,`cac`|`abc`| Mỗi cột được xử lý độc lập | 

## Vỏ cạnh 

Hãy xem xét ví dụ về sự ràng buộc.```
2 1
b
a
```Mảng tần số cho cột duy nhất trở thành`[1, 1, 0, ..., 0]`. Quá trình quét bắt đầu với`'a'`với tư cách là người chiến thắng hiện tại. Khi`'b'`được kiểm tra, tần số của nó bằng chứ không lớn hơn nên người chiến thắng không bị thay đổi. Đầu ra là chính xác`a`. 

Bây giờ hãy xem xét bỏ phiếu nhất trí.```
3 4
code
code
code
```Mỗi cột chỉ chứa một chữ cái riêng biệt. Mỗi mảng tần số có một mức tối đa duy nhất, vì vậy các chữ cái được chọn là`c`,`o`,`d`, Và`e`. Đầu ra của thuật toán`code`đúng như mong đợi. 

Cuối cùng, hãy xem xét những người chiến thắng khác nhau ở các cột khác nhau.```
3 3
abc
bbc
cac
```Cột đầu tiên có tần số`{a:1, b:1, c:1}`, Vì thế`a`chiến thắng theo thứ tự bảng chữ cái. Cột thứ hai có`b`xuất hiện hai lần, khiến nó trở thành người chiến thắng. Cột thứ ba có`c`trong mọi tên, làm cho nó nhất trí. Thuật toán không bao giờ trộn lẫn thông tin giữa các cột, tạo ra câu trả lời đúng`abc`.
