---
title: "CF 105204C - \u0411\u043e\u0442\u0438\u043d\u043a\u0438 \u0438\u043b\u0438 \u0441\u0430\u043d\u0434\u0430\u043b\u0438\u0438"
description: "Tuyến đường được mô tả bằng một chuỗi bao gồm '0', '1' và '?'. Đoạn được đánh dấu '0' phải đi dép xăng đan, đoạn được đánh dấu '1' phải đi ủng và đoạn được đánh dấu '?' có thể được chỉ định một trong hai loại giày dép."
date: "2026-06-27T02:41:57+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105204
codeforces_index: "C"
codeforces_contest_name: "\u0412\u041a\u041e\u0428\u041f.Junior 2024"
rating: 0
weight: 105204
solve_time_s: 55
verified: true
draft: false
---

[CF 105204C - \u0411\u043e\u0442\u0438\u043d\u043a\u0438 \u0438\u043b\u0438 \u0441\u0430\u043d\u0434\u0430\u043b\u0438\u0438](https://codeforces.com/problemset/problem/105204/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 55s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Tuyến đường được mô tả bằng một chuỗi bao gồm`'0'`,`'1'`, Và`'?'`. 

Một đoạn được đánh dấu`'0'`phải đi dép xăng đan, đoạn đường được đánh dấu`'1'`phải đi bằng ủng và một đoạn được đánh dấu`'?'`có thể được chỉ định một trong hai loại giày dép. Chi phí đổi từ dép sang ủng`a`, khi chuyển từ bốt sang dép`b`. Khách du lịch có thể bắt đầu với một trong hai loại giày dép và cũng có thể kết thúc bằng một trong hai loại giày dép, vì vậy chỉ những thay đổi giữa các đoạn đường liên tiếp mới quan trọng. 

Nhiệm vụ là phân công mọi`'?'`hoặc`'0'`hoặc`'1'`sao cho tổng chi phí của tất cả các lần thay giày dép càng nhỏ càng tốt. 

Chiều dài tuyến đường tối đa là`200000`, vì vậy bất cứ điều gì bậc hai ngay lập tức là không thể. Ngay cả việc quét tất cả các bài tập có thể cũng là vô vọng vì mỗi`'?'`nhân đôi số khả năng. MỘT`O(n)`hoặc`O(n log n)`thuật toán dễ dàng đủ nhanh. 

Phần khó khăn là mọi`'?'`thuộc về một khối và việc gán các giá trị khác nhau trong một khối không bao giờ có ích. Ví dụ:```
0??1
```Nếu chúng ta gán nó là`0011`, có một sự chuyển tiếp. Nếu chúng ta gán nó là`0101`, có ba lần chuyển tiếp. Mọi thay đổi bổ sung bên trong khối chỉ làm tăng chi phí. 

Một trường hợp cạnh khác là khi toàn bộ chuỗi chỉ bao gồm`'?'`.```
???
```Câu trả lời đúng là`0`, bởi vì chúng ta có thể gán cho mọi vị trí một giá trị như nhau. Giải pháp giả sử ký tự được biết đầu tiên luôn tồn tại sẽ thất bại. 

Một trường hợp tế nhị hơn là khi một`'?'`khối chỉ chạm vào một mặt đã biết.```
??111
```Việc gán khối cho`'1'`không tạo ra sự chuyển tiếp nào cả. Một giải pháp cố gắng tối ưu hóa từng khối một cách độc lập mà không xem xét đến việc thiếu các khối lân cận có thể gây ra một sự thay đổi không cần thiết. 

## Phương pháp tiếp cận 

Một giải pháp vũ phu sẽ thử mọi nhiệm vụ của mọi`'?'`. Nếu có`k`dấu chấm hỏi, điều này đòi hỏi`2^k`bài tập. Mỗi bài tập có thể được kiểm tra theo thời gian tuyến tính, đưa ra`O(n · 2^k)`phức tạp, điều này trở nên không thể thực hiện được dù chỉ với vài chục dấu hỏi. 

Quan sát quan trọng là chỉ có sự chuyển tiếp giữa các ký tự lân cận mới góp phần tạo ra câu trả lời. Một khối liên tiếp`'?'`chỉ ảnh hưởng đến ranh giới bên trái và bên phải của nó. Vì việc đưa ra các thay đổi bên trong khối không bao giờ có thể làm giảm số lượng chuyển đổi ranh giới, nên mọi phép gán tối ưu sẽ lấp đầy toàn bộ khối bằng một ký tự giống hệt nhau. 

Điều này cho phép chúng ta bỏ qua độ dài khối chính xác. Thay vào đó, hãy loại bỏ mọi`'?'`về mặt khái niệm và chỉ nhìn vào chuỗi ký tự cố định. Bất cứ khi nào hai ký tự cố định liên tiếp khác nhau thì phải thay đổi giày dép không thể tránh khỏi. Nếu quá trình chuyển đổi là`0 → 1`, nó có giá`a`. Nếu nó là`1 → 0`, nó có giá`b`. 

Mọi`'?'`khối luôn có thể được lấp đầy để khớp với một bên lân cận bất cứ khi nào có thể, loại bỏ mọi chuyển đổi nhân tạo. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(n · 2^k)`|`O(n)`| Quá chậm | 
| Tối ưu |`O(n)`|`O(1)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc chuỗi lộ trình và các giá trị`a`Và`b`. 
2. Quét chuỗi từ trái sang phải cho đến ký tự đầu tiên không phải`'?'`. Ký tự này trở thành ký tự cố định trước đó. 
3. Nếu không tồn tại ký tự cố định, xuất ra`0`, bởi vì mọi vị trí đều có thể được gán cùng một giá trị. 
4. Tiếp tục quét phần còn lại của chuỗi. 
5. Bỏ qua mọi`'?'`, vì giá trị của nó sẽ được chọn sau. 
6. Bất cứ khi nào tìm thấy một ký tự cố định khác, hãy so sánh nó với ký tự cố định trước đó. 
7. Nếu chúng khác nhau, hãy thêm`a`cho một sự chuyển tiếp`0 → 1`hoặc thêm`b`cho một sự chuyển tiếp`1 → 0`. 
8. Cập nhật ký tự cố định trước đó và tiếp tục. 
9. Xuất chi phí tích lũy. 

### Tại sao nó hoạt động 

Mỗi khối liên tiếp của`'?'`chỉ có hai ký tự cố định lân cận. Việc lấp đầy khối bằng các giá trị hỗn hợp chỉ có thể tạo ra các chuyển tiếp bổ sung bên trong khối, điều này không bao giờ làm giảm tổng chi phí. Việc lấp đầy toàn bộ khối bằng một giá trị duy nhất ít nhất là tốt. 

Nếu cả hai ký tự cố định lân cận đều bằng nhau thì toàn bộ khối được gán giá trị đó, không tạo ra sự chuyển tiếp. 

Nếu chúng khác nhau thì không thể tránh khỏi chính xác một lần chuyển đổi bất kể nó xảy ra ở đâu bên trong khối. Chi phí chỉ phụ thuộc vào thứ tự của các ký tự cố định chứ không phụ thuộc vào độ dài khối. 

Sau khi loại bỏ tất cả`'?'`, mỗi lần chuyển đổi giữa các ký tự cố định liên tiếp chính xác là một lần thay đổi giày dép không thể tránh khỏi, vì vậy việc tính tổng các chi phí đó sẽ đưa ra câu trả lời tối thiểu có thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    s = input().strip()
    a, b = map(int, input().split())

    prev = None
    ans = 0

    for ch in s:
        if ch == '?':
            continue
        if prev is not None and prev != ch:
            if prev == '0':
                ans += a
            else:
                ans += b
        prev = ch

    print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai chỉ giữ lại ký tự cố định trước đó và câu trả lời hiện tại. 

Dấu hỏi bị bỏ qua ngay lập tức vì chúng không đóng góp trực tiếp vào câu trả lời. Bất cứ khi nào hai ký tự cố định liên tiếp khác nhau thì chi phí chuyển đổi tương ứng sẽ được thêm vào. 

Ký tự cố định đầu tiên khởi tạo trạng thái mà không phải thêm bất kỳ chi phí nào vì khách du lịch có thể bắt đầu bằng một trong hai loại giày dép. 

Nếu chuỗi chỉ chứa`'?'`,`prev`còn lại`None`, không có chuyển đổi nào được tính và câu trả lời chính xác vẫn là 0. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
?0110?1?1
5 9
```| Vị trí | Nhân vật | Trước Đã sửa | Chi phí bổ sung | Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | ? | Không có | 0 | 0 | 
| 2 | 0 | 0 | 0 | 0 | 
| 3 | 1 | 1 | 5 | 5 | 
| 4 | 1 | 1 | 0 | 5 | 
| 5 | 0 | 0 | 9 | 14 | 
| 6 | ? | 0 | 0 | 14 | 
| 7 | 1 | 1 | 5 | 19 | 
| 8 | ? | 1 | 0 | 19 | 
| 9 | 1 | 1 | 0 | 19 | 

Các ký tự cố định tạo thành chuỗi`0 1 1 0 1 1`. Mỗi cặp liền kề khác nhau tương ứng với một thay đổi không thể tránh khỏi, tạo ra chi phí tối thiểu của`19`. 

### Ví dụ được xây dựng 

đầu vào:```
??0???0??
3 7
```| Vị trí | Nhân vật | Trước Đã sửa | Chi phí bổ sung | Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | ? | Không có | 0 | 0 | 
| 2 | ? | Không có | 0 | 0 | 
| 3 | 0 | 0 | 0 | 0 | 
| 4 | ? | 0 | 0 | 0 | 
| 5 | ? | 0 | 0 | 0 | 
| 6 | ? | 0 | 0 | 0 | 
| 7 | 0 | 0 | 0 | 0 | 
| 8 | ? | 0 | 0 | 0 | 
| 9 | ? | 0 | 0 | 0 | 

Tất cả các ký tự cố định đều giống hệt nhau nên mọi dấu chấm hỏi cũng có thể trở thành`0`. Không cần thay đổi giày dép. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(n)`| Mỗi ký tự được xử lý chính xác một lần. | 
| Không gian |`O(1)`| Chỉ có một vài biến được lưu trữ. | 

Một lần quét tuyến tính đơn lẻ có thể dễ dàng đủ nhanh cho một chuỗi có độ dài`200000`và việc sử dụng bộ nhớ liên tục sẽ thỏa mãn giới hạn bộ nhớ một cách thoải mái. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve():
    input = sys.stdin.readline
    s = input().strip()
    a, b = map(int, input().split())

    prev = None
    ans = 0

    for ch in s:
        if ch == '?':
            continue
        if prev is not None and prev != ch:
            ans += a if prev == '0' else b
        prev = ch

    print(ans)

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    sys.stdin = backup_stdin
    sys.stdout = backup_stdout
    return out.getvalue()

# provided sample
assert run("?0110?1?1\n5 9\n") == "19\n"

# minimum size
assert run("?\n3 4\n") == "0\n"

# all question marks
assert run("????\n5 7\n") == "0\n"

# alternating fixed characters
assert run("0101\n2 3\n") == "7\n"

# question block between equal symbols
assert run("1???1\n6 8\n") == "0\n"

# question block between different symbols
assert run("0???1\n6 8\n") == "6\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`?\n3 4`|`0`| Phân đoạn đơn chưa biết | 
|`????\n5 7`|`0`| Toàn bộ lộ trình linh hoạt | 
|`0101\n2 3`|`7`| Mọi sự chuyển đổi đều không thể tránh khỏi | 
|`1???1\n6 8`|`0`| Điểm cuối bằng nhau loại bỏ mọi thay đổi | 
|`0???1\n6 8`|`6`| Các điểm cuối khác nhau yêu cầu chính xác một lần chuyển đổi | 

## Vỏ cạnh 

Xem xét đầu vào```
???
5 8
```Không có ký tự cố định tồn tại. Quá trình quét không bao giờ chỉ định`prev`, do đó câu trả lời tích lũy vẫn còn`0`. Việc gán mọi vị trí cho dép hoặc mọi vị trí cho ủng quả thực không cần phải thay giày. 

Bây giờ hãy xem xét```
??111
4 6
```Ký tự cố định đầu tiên là`1`. Mọi dấu hỏi trước đó đều bị bỏ qua trong quá trình quét vì sau này nó có thể được gán cho`1`. Không gặp phải các ký tự cố định khác nhau nên thuật toán đưa ra`0`. 

Cuối cùng, hãy xem xét```
0???1
2 9
```Quá trình quét quan sát các ký tự cố định`0`Và`1`. Chúng khác nhau một lần, nên nó thêm vào`2`. Bất kể có bao nhiêu dấu chấm hỏi nằm giữa chúng, mỗi bài tập phải có chính xác một sự chuyển đổi từ dép sang bốt, tạo nên`2`câu trả lời tối ưu.
