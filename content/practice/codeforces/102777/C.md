---
title: "CF 102777C - \u0426\u0432\u0435\u0442\u043d\u0430\u044f \u0434\u043e\u0441\u043a\u0430"
description: "Bảng có N dòng và M cột. Các tế bào của nó được tô màu bảy màu lặp lại theo chu kỳ dọc theo các đường chéo. Sau khi Alice đặt tên cho một ô (X, Y), chúng ta phải xác định số màu được gán cho ô đó."
date: "2026-07-28T15:44:59+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102777
codeforces_index: "C"
codeforces_contest_name: "ICPC Central Russia Regional Contest (CRRC 19), \u0427\u0435\u043c\u043f\u0438\u043e\u043d\u0430\u0442 \u0426\u0435\u043d\u0442\u0440\u0430\u043b\u044c\u043d\u043e\u0439 \u0420\u043e\u0441\u0441\u0438\u0438, \u043a\u0432\u0430\u043b\u0438\u0444\u0438\u043a\u0430\u0446\u0438\u043e\u043d\u043d\u044b\u0439 \u0440\u0430\u0443\u043d\u0434"
rating: 0
weight: 102777
solve_time_s: 48
verified: true
draft: false
---

[CF 102777C - \u0426\u0432\u0435\u0442\u043d\u0430\u044f \u0434\u043e\u0441\u043a\u0430](https://codeforces.com/problemset/problem/102777/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 48s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Hội đồng quản trị có`N`hàng và`M`cột. Các tế bào của nó được tô màu bảy màu lặp lại theo chu kỳ dọc theo các đường chéo. Sau khi Alice đặt tên cho một ô`(X, Y)`, chúng ta phải xác định số màu được gán cho ô đó. 

Kích thước bảng có thể lớn bằng`10^6 × 10^6`, nhưng chỉ có một ô được truy vấn. Điều đó ngay lập tức loại trừ bất kỳ cách tiếp cận nào nhằm xây dựng bảng một cách rõ ràng. Ngay cả việc lưu trữ màu sắc cũng cần tới`10^12`các tế bào, vượt xa cả giới hạn bộ nhớ và thời gian. Lời giải phải tính toán đáp án trực tiếp từ tọa độ. 

Quan sát quan trọng là mọi ô trên cùng một đường chéo có hằng số`X + Y`có cùng màu sắc. Di chuyển một bước tới đường chéo tiếp theo sẽ tăng`X + Y`tăng thêm một, do đó màu sắc cũng tăng lên một trong chu kỳ bảy màu. 

Một lỗi thường gặp là quên rằng các màu được đánh số từ`1`thay vì`0`. Ví dụ, đối với đầu vào```
1 1 1 1
```câu trả lời đúng là```
1
```bởi vì đường chéo đầu tiên bắt đầu bằng màu sắc`1`. sử dụng`(X + Y) % 7`trực tiếp sẽ tạo ra sai`2`. 

Một trường hợp tinh tế khác là khi chỉ số đường chéo là bội số của 7. Ví dụ,```
5 9 3 6
```có`X + Y = 9`. Vì đường chéo đầu tiên tương ứng với`X + Y = 2`, phần bù là`7`, kết thúc chính xác một lần trong chu kỳ. Màu đúng là`1`, không`7`. 

## Phương pháp tiếp cận 

Một mô phỏng đơn giản sẽ tạo ra toàn bộ bảng theo đường chéo, gán màu theo thứ tự lặp lại`1, 2, ..., 7, 1, ...`. Điều này đúng vì nó tuân theo quy tắc tô màu trực tiếp. Thật không may, bảng có thể chứa tới`10^12`các ô, làm cho cả thời gian chạy và bộ nhớ hoàn toàn không thực tế. 

Quan sát quan trọng là màu sắc chỉ phụ thuộc vào đường chéo chứa ô được truy vấn. Đường chéo đầu tiên có`X + Y = 2`và nhận được màu sắc`1`. Mỗi sự gia tăng của một trong`X + Y`di chuyển đến đường chéo tiếp theo và tăng màu lên một. Vì có bảy màu nên chúng ta chỉ cần độ lệch đường chéo modulo bảy. 

Độ lệch đường chéo là```
(X + Y) - 2
```và chuyển đổi nó trở lại`1`đánh số màu dựa trên mang lại```
((X + Y - 2) % 7) + 1
```Điều này tính toán câu trả lời trong thời gian không đổi mà không cần xây dựng bất kỳ phần nào của bảng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(NM) | O(NM) | Quá chậm | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc bốn số nguyên`N`,`M`,`X`, Và`Y`. Kích thước bảng không cần thiết cho việc tính toán vì tọa độ được truy vấn được đảm bảo hợp lệ. 
2. Tính độ lệch đường chéo như`X + Y - 2`. Đường chéo thứ nhất có tổng`2`, do đó trừ đi`2`làm cho phần bù của nó bằng 0. 
3. Tính màu bằng`((X + Y - 2) % 7) + 1`. Phép toán modulo bao bọc các màu theo bảy đường chéo và việc thêm một đường chéo sẽ chuyển kết quả trở lại số được yêu cầu từ`1`ĐẾN`7`. 
4. In màu kết quả. 

### Tại sao nó hoạt động 

Mọi ô trên cùng một đường chéo đều có cùng giá trị là`X + Y`, vì vậy chúng phải nhận được cùng một màu. Các đường chéo liền kề khác nhau ở`X + Y`bằng đúng một, khớp một bước về phía trước trong chuỗi bảy màu lặp lại. Vì đường chéo đầu tiên bắt đầu bằng màu`1`, dịch chuyển theo`(X + Y - 2)`đường chéo và lấy phần dư theo modulo bảy luôn tạo ra màu chính xác duy nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m, x, y = map(int, input().split())
print((x + y - 2) % 7 + 1)
```Chương trình đọc đầu vào, tính toán độ lệch đường chéo, áp dụng modulo 7, chuyển kết quả trở lại thành`1`đánh số dựa trên và in nó. 

Kích thước bảng không được cố ý sử dụng vì chúng không ảnh hưởng đến màu sắc khi biết ô được truy vấn. Việc trừ đi hai là điều chỉnh từng bước một. Nếu không có nó, đường chéo đầu tiên sẽ bắt đầu không chính xác từ màu sắc`3`thay vì màu sắc`1`. 

Số nguyên Python dễ dàng xử lý tổng tọa độ tối đa có thể, do đó không có vấn đề tràn. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3 3 3 3
```| X | Y | X + Y | Độ lệch = X + Y - 2 | Bù đắp % 7 | Màu sắc | 
| --- | --- | --- | --- | --- | --- | 
| 3 | 3 | 6 | 4 | 4 | 5 | 

Ô được truy vấn nằm trên đường chéo thứ năm trong chuỗi lặp lại, vì vậy màu của nó là`5`. 

### Mẫu 2 

đầu vào:```
5 9 3 6
```| X | Y | X + Y | Độ lệch = X + Y - 2 | Bù đắp % 7 | Màu sắc | 
| --- | --- | --- | --- | --- | --- | 
| 3 | 6 | 9 | 7 | 0 | 1 | 

Ví dụ này thể hiện sự bao quanh. Sau bảy lần dịch chuyển chéo, chu kỳ màu trở về`1`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ có một số phép tính số học được thực hiện. | 
| Không gian | O(1) | Không có cấu trúc dữ liệu bổ sung được phân bổ. | 

Thời gian chạy và mức sử dụng bộ nhớ không phụ thuộc vào kích thước bo mạch, do đó giải pháp dễ dàng đáp ứng các giới hạn nhất định ngay cả khi`N`Và`M`đang ở giá trị cực đại của chúng. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline
    n, m, x, y = map(int, input().split())
    print((x + y - 2) % 7 + 1)

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
assert run("3 3 3 3\n") == "5\n", "sample 1"
assert run("5 9 3 6\n") == "1\n", "sample 2"

# custom cases
assert run("1 1 1 1\n") == "1\n", "minimum board"
assert run("1000000 1000000 1000000 1000000\n") == "5\n", "maximum coordinates"
assert run("7 7 1 7\n") == "7\n", "boundary before wrap"
assert run("7 8 1 8\n") == "1\n", "exact wraparound"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1 1`|`1`| Bảng nhỏ nhất có thể | 
|`1000000 1000000 1000000 1000000`|`5`| Giá trị tọa độ tối đa | 
|`7 7 1 7`|`7`| Màu cuối cùng trước khi gói | 
|`7 8 1 8`|`1`| Bao bọc modulo đúng | 

## Vỏ cạnh 

Hãy xem xét đầu vào nhỏ nhất có thể:```
1 1 1 1
```Thuật toán tính toán`X + Y - 2 = 0`, sau đó`0 % 7 = 0`, và cuối cùng thêm một để có được màu`1`. Điều này xác nhận rằng đường chéo đầu tiên được xử lý chính xác. 

Bây giờ hãy xem xét một đường chéo có độ lệch chính xác là 7:```
5 9 3 6
```Phần bù là`3 + 6 - 2 = 7`. Lấy`7 % 7`cho`0`và thêm một sẽ tạo ra màu`1`. Điều này khởi động lại một cách chính xác trình tự lặp lại sau mỗi bảy đường chéo. 

Cuối cùng, hãy xem xét đường chéo cuối cùng trước khi gói:```
7 7 1 7
```Phần bù là`1 + 7 - 2 = 6`. Phần còn lại vẫn`6`, do đó màu sắc trở thành`7`. Đường chéo tiếp theo sẽ bao bọc lại màu sắc`1`, phù hợp với mẫu tuần hoàn được yêu cầu.
