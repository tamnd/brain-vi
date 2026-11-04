---
title: "CF 1166B - Xin tất cả các nguyên âm"
description: "Chúng tôi đang cố gắng xây dựng một chuỗi có độ dài $k$ có thể được sắp xếp lại thành một lưới hình chữ nhật với $n$ hàng và $m$ cột sao cho mỗi hàng và mỗi cột chứa tất cả năm nguyên âm: a, e, i, o, u ít nhất một lần."
date: "2026-06-12T02:13:04+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1166
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 561 (Div. 2)"
rating: 1100
weight: 1166
solve_time_s: 91
verified: true
draft: false
---

[CF 1166B - Vui lòng sử dụng tất cả các nguyên âm](https://codeforces.com/problemset/problem/1166/B) 

**Đánh giá:** 1100 
**Tags:** xây dựng thuật toán, toán, lý thuyết số 
**Thời gian giải:** 1 phút 31s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang cố gắng xây dựng một chuỗi có độ dài$k$có thể được sắp xếp lại thành một lưới hình chữ nhật với$n$hàng và$m$các cột sao cho mỗi hàng và mỗi cột chứa đủ năm nguyên âm: a, e, i, o, u ít nhất một lần. 

Quy tắc xây dựng sửa cách chuỗi được ánh xạ vào lưới: chúng tôi điền từng hàng từ trái sang phải. Vì vậy, đầu tiên$m$ký tự tạo thành hàng 1, tiếp theo$m$tạo thành hàng 2, v.v. Khi đã đặt vào lưới này, chúng ta không được phép sắp xếp lại các ký tự nên cấu trúc lưới cuối cùng hoàn toàn được xác định bởi chuỗi. 

Yêu cầu chính rất cao: mỗi hàng phải chứa đủ năm nguyên âm và mỗi cột cũng phải chứa đủ năm nguyên âm. Điều này ngay lập tức buộc cả hai chiều$n$Và$m$ít nhất là 5, nếu không thì một hàng hoặc cột không thể chứa tất cả các nguyên âm. 

Đầu vào chỉ là số nguyên$k$và chúng ta phải quyết định liệu một lưới như vậy có tồn tại cho bất kỳ hệ số hóa nào không$k = n \cdot m$và nếu có, hãy xuất ra bất kỳ chuỗi hợp lệ nào hoạt động. Nếu không thì xuất -1. 

Ràng buộc$k \le 10^4$có nghĩa là chúng ta có thể thử một cách an toàn tất cả các ước của$k$TRONG$O(\sqrt{k})$, vì có nhiều nhất khoảng 100 ứng viên. Bất cứ điều gì bậc hai trong$k$sẽ không cần thiết nhưng vẫn có thể chấp nhận được; bất cứ điều gì theo cấp số nhân là không liên quan. 

Điểm tinh tế chính là điều kiện không cục bộ đối với các hàng hoặc cột một cách độc lập. Một nỗ lực ngây thơ có thể đảm bảo các hàng chứa tất cả các nguyên âm, nhưng các cột sẽ thất bại trừ khi việc xây dựng được định kỳ cẩn thận. 

Một số trường hợp đặc biệt làm nổi bật cấu trúc: 

Khi nào$k = 7$, không có phép nhân tử nào tạo ra cả hai$n \ge 5$Và$m \ge 5$, vì vậy câu trả lời là không thể mặc dù$k$không phải là nhỏ. 

Khi$k = 25$, chúng ta có thể lấy$n = 5, m = 5$và xây dựng một lưới hợp lệ. 

Thách thức cốt lõi là phải hiểu cấu trúc nào đảm bảo tính đầy đủ theo hàng và theo cột cùng một lúc. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ sẽ là thử mọi cách phân tích nhân tử$k = n \cdot m$và với mỗi cặp hãy cố gắng xây dựng một lưới. Đối với mỗi lưới ứng viên, chúng ta có thể thử điền ngẫu nhiên hoặc quay lui để đảm bảo mỗi hàng và cột chứa tất cả năm nguyên âm. Tuy nhiên, ngay cả đối với một cặp cố định, không gian tìm kiếm vẫn rất lớn: mỗi ô có 5 lựa chọn, do đó số lượng lưới là$5^{nm}$, điều này hoàn toàn không khả thi ngay cả đối với kích thước vừa phải. 

Điều quan trọng cần lưu ý là yêu cầu “mỗi hàng chứa tất cả các nguyên âm” đã buộc phải có một cấu trúc chặt chẽ: mỗi hàng phải chứa ít nhất 5 ký tự riêng biệt, do đó$m \ge 5$. Tương tự, mỗi cột phải có chiều cao ít nhất là 5, vì vậy$n \ge 5$. Điều này làm giảm vấn đề tìm một hệ số$k = n \cdot m$với cả hai$n \ge 5$Và$m \ge 5$. 

Khi hệ số hóa như vậy tồn tại, chúng ta có thể xây dựng một mẫu nguyên âm 5 x 5 định kỳ và xếp nó trên lưới. Cấu trúc tuần hoàn kiểu Latin 5x5 đảm bảo mỗi hàng và cột chứa tất cả các nguyên âm chính xác một lần và việc lặp lại nó qua bội số của 5 sẽ giữ nguyên thuộc tính. Phần còn lại của lưới chỉ là sự lặp lại của cấu trúc cơ bản này. 

Vì vậy, vấn đề quy giản hoàn toàn về lý thuyết số: liệu$k$có một số chia$m$sao cho cả hai$m \ge 5$Và$k/m \ge 5$? 

Nếu có, chúng tôi sẽ xây dựng lưới; nếu không, chúng tôi xuất ra -1. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tìm kiếm dạng lưới) | hàm mũ$O(5^{k})$|$O(k)$| Quá chậm | 
| Số chia + cách xây dựng |$O(\sqrt{k})$|$O(k)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi muốn tìm kích thước hình chữ nhật hợp lệ và sau đó điền nó theo cách có cấu trúc. 

1. Lặp lại tất cả các ước số$m$của$k$. Với mỗi số chia, hãy tính$n = k / m$. Chúng tôi chỉ quan tâm đến những cặp mà cả hai$n \ge 5$Và$m \ge 5$, vì kích thước nhỏ hơn khiến không thể chứa đủ năm nguyên âm trong một hàng hoặc cột. 
2. Khi chúng tôi tìm thấy một cặp như vậy, chúng tôi sẽ sửa nó thành kích thước lưới của chúng tôi. Bước này là hợp lý bởi vì bất kỳ giải pháp hợp lệ nào cũng phải tương ứng với một số nhân tử của$k$. 
3. Xây dựng mẫu cơ sở 5x5 bằng cách sử dụng sự dịch chuyển nguyên âm theo chu kỳ. Ý tưởng là sắp xếp các nguyên âm sao cho mỗi hàng là một vòng xoay của "aeiou" và mỗi cột cũng xoay vòng qua tất cả các nguyên âm. 
4. Mở rộng khối 5x5 này thành một$n \times m$lưới bằng cách lặp lại các hàng và cột theo modulo 5. Cụ thể, ô$(i, j)$được gán nguyên âm dựa trên$(i \bmod 5, j \bmod 5)$. 
5. Làm phẳng từng hàng lưới thành một chuỗi có độ dài$k$. 

Nếu không tìm thấy hệ số hợp lệ, chúng tôi trả về -1. 

### Tại sao nó hoạt động 

Cấu trúc đảm bảo rằng mỗi hàng bao gồm việc lặp lại một hoán vị của năm nguyên âm trong một chu kỳ có độ dài 5. Vì mỗi hàng được xây dựng từ cùng một cấu trúc tuần hoàn nên mỗi hàng chứa tất cả các nguyên âm ít nhất một lần. Điều tương tự cũng áp dụng cho các cột vì mẫu cột giống hệt với dịch chuyển theo chu kỳ. Tính tuần hoàn đảm bảo rằng việc mở rộng cấu trúc 5x5 cơ sở sẽ duy trì tính hoàn chỉnh theo hàng và theo cột bất kể có bao nhiêu khối được lặp lại. 

Điều bất biến là cứ 5 vị trí liên tiếp trong bất kỳ hàng hoặc cột nào đều chứa một hoán vị của các nguyên âm và tính chất này được bảo toàn khi lặp lại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    k = int(input().strip())
    
    vowels = "aeiou"
    
    # find valid factorization n x m
    n = m = -1
    for i in range(5, int(k**0.5) + 1):
        if k % i == 0:
            j = k // i
            if j >= 5:
                n, m = i, j
                break
            if i >= 5:
                n, m = j, i
    
    if n == -1:
        print(-1)
        return
    
    # build grid using cyclic pattern
    grid = []
    for i in range(n):
        row = []
        for j in range(m):
            row.append(vowels[(i + j) % 5])
        grid.append(row)
    
    # flatten
    res = []
    for i in range(n):
        res.extend(grid[i])
    
    print("".join(res))

if __name__ == "__main__":
    solve()
```Mã bắt đầu bằng cách chọn hệ số hợp lệ của$k$. Nó chỉ chấp nhận các kích thước trong đó cả hai cạnh ít nhất là 5, vì kích thước nhỏ hơn ngay lập tức vi phạm yêu cầu rằng một hàng hoặc cột phải chứa tất cả năm nguyên âm. 

Việc xây dựng lưới sử dụng dịch chuyển chu kỳ chéo: mục nhập$(i, j)$được xác định bởi$(i + j) \bmod 5$. Điều này đảm bảo rằng việc di chuyển sang phải hoặc xuống sẽ chuyển qua các nguyên âm theo một hoán vị cố định, đảm bảo bao phủ mọi hàng và cột. 

Cuối cùng, lưới được làm phẳng theo thứ tự hàng lớn, khớp với ánh xạ đầu vào vào lưới của vấn đề. 

## Ví dụ đã hoạt động 

### Ví dụ 1: k = 7 

Không có hệ số hóa của 7 trong đó cả hai bên ít nhất là 5. Các cặp duy nhất là (1,7) và (7,1), cả hai đều không hợp lệ. 

| Bước | n | m | Có hiệu lực? | Hành động | 
| --- | --- | --- | --- | --- | 
| kiểm tra 1×7 | 1 | 7 | không | hàng quá nhỏ | 
| kiểm tra 7×1 | 7 | 1 | không | cột quá nhỏ | 

Thuật toán không tìm thấy lưới hợp lệ và kết quả đầu ra là -1. Điều này phù hợp với yêu cầu vì một hàng hoặc một cột không thể chứa tất cả năm nguyên âm. 

### Ví dụ 2: k = 25 

Chúng ta có thể chọn$n = 5, m = 5$. 

| tôi | j | (i+j)%5 | lá thư | 
| --- | --- | --- | --- | 
| 0 | 0 | 0 | một | 
| 0 | 1 | 1 | e | 
| 0 | 2 | 2 | tôi | 
| 0 | 3 | 3 | o | 
| 0 | 4 | 4 | bạn | 

Hàng 0 đã chứa tất cả các nguyên âm. Mỗi hàng khác là một sự dịch chuyển theo chu kỳ của hàng này. 

Mẫu tương tự cũng áp dụng cho các cột vì cột$j$cũng là một sự dịch chuyển theo chu kỳ giữa các hàng. 

Chuỗi dẹt có độ dài 25 có giá trị theo cách xây dựng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(\sqrt{k})$| Chúng tôi kiểm tra các ứng cử viên chia số lên đến$\sqrt{k}$| 
| Không gian |$O(k)$| Chúng tôi xây dựng chuỗi kết quả có độ dài một cách rõ ràng$k$| 

Sự ràng buộc$k \le 10^4$làm cho việc tìm kiếm số chia trở nên tầm thường và việc xây dựng đầu ra có kích thước tuyến tính là tối ưu vì chúng ta phải in$k$dù sao thì nhân vật cũng vậy. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from contextlib import redirect_stdout
    out = io.StringIO()
    
    # assuming solve() is defined above
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# sample
assert run("7\n") == "-1"

# k is valid square
assert len(run("25\n")) == 25

# smallest impossible
assert run("1\n") == "-1"

# prime > 5
assert run("11\n") == "-1"

# valid non-square factorization
assert len(run("30\n")) == 30
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 7 | -1 | phân tích nhân tử không thể | 
| 25 | chiều dài 25 chuỗi | xây dựng hình vuông hợp lệ | 
| 1 | -1 | trường hợp cạnh tối thiểu | 
| 11 | -1 | trường hợp chính | 
| 30 | chiều dài 30 chuỗi | trường hợp hợp lệ hình chữ nhật | 

## Vỏ cạnh 

cho$k = 1$, thuật toán ngay lập tức không tìm được cặp ước số nào có cả hai vế ≥ 5 nên trả về -1. Điều này đúng vì một ký tự không thể đáp ứng được yêu cầu dù chỉ một hàng. 

Đối với các giá trị nguyên tố như$k = 11$, các phân tích nhân tử duy nhất là (1,11) và (11,1), cả hai đều không hợp lệ. Việc quét số chia phát hiện điều này một cách rõ ràng. 

Đối với các giá trị tổng hợp như$k = 30$, chúng ta có thể chọn$5 \times 6$. Việc xây dựng sử dụng mẫu 5 chu kỳ và tính đầy đủ của cả hàng và cột đều được giữ nguyên vì cả hai kích thước đều vượt quá hoặc bằng 5, cho phép thực hiện các chu kỳ đầy đủ theo cả hai hướng.
