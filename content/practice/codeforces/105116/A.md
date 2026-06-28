---
title: "CF 105116A - Hiện tại dạng khối"
description: "Chúng ta được cung cấp hai loại gạch vuông phải được xếp vào một hộp hình chữ nhật có chiều rộng cố định. Hộp có chiều rộng $K$ và chiều dài không xác định $X$ và tất cả các ô phải được đặt theo trục, không chồng chéo. Một loại ô là hình vuông $2 nhân 2$ và có $A$ trong số đó."
date: "2026-06-27T19:46:29+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105116
codeforces_index: "A"
codeforces_contest_name: "\u041e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 1\u0421 2024, \u043f\u0440\u0435\u0434\u043c\u0435\u0442\u043d\u044b\u0439 \u0442\u0443\u0440"
rating: 0
weight: 105116
solve_time_s: 56
verified: true
draft: false
---

[CF 105116A - Khối hiện tại](https://codeforces.com/problemset/problem/105116/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 56s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp hai loại gạch vuông phải được xếp vào một hộp hình chữ nhật có chiều rộng cố định. Hộp có chiều rộng$K$và một độ dài không xác định$X$và tất cả các ô phải được đặt theo trục, không chồng chéo. 

Một loại gạch là$2 \times 2$hình vuông và có$A$của họ. Loại còn lại là một$1 \times 1$hình vuông và có$B$của họ. Nhiệm vụ là xác định giá trị nhỏ nhất có thể của$X$sao cho tất cả các ô có thể được đặt bên trong một$K \times X$hình chữ nhật. 

Chiều rộng$K$được cố định và chia sẻ trên tất cả các trường hợp thử nghiệm. Sự tự do duy nhất là cách chúng ta sắp xếp các hình vuông và hình chữ nhật phải cao bao nhiêu để chứa chúng. 

Các ràng buộc đi lên đến$A, B \le 10^8$, do đó, bất kỳ giải pháp nào cố gắng mô phỏng từng ô vị trí hoặc xây dựng một lưới rõ ràng đều không thể thực hiện được ngay lập tức. Ngay cả một mô phỏng tham lam ngây thơ trên các hàng cũng sẽ thất bại vì chiều cao cũng có thể trở nên lớn và số lượng thao tác sẽ tăng theo câu trả lời thay vì kích thước đầu vào. Giải pháp phải hoàn toàn là số học. 

Trường hợp cạnh tinh tế xuất hiện khi$K$là nhỏ. Nếu như$K = 2$, chỉ có một$2 \times 2$gạch vừa với mỗi lát cắt ngang và không có tính linh hoạt theo chiều ngang. Nếu như$K = 3$, vẫn còn lãng phí chiều rộng bên trong một$2 \times 2$mẫu vị trí. Những trường hợp này thường phá vỡ lý luận “chỉ diện tích” không chính xác vì không gian còn lại tương tác với cách các khu vực hình vuông lớn chặn lại. 

Một sai lầm ngây thơ là tính tổng diện tích và chia cho$K$, bỏ qua hình học. Điều này thất bại vì$2 \times 2$gạch không tương đương với bốn độc lập$1 \times 1$gạch trong một thiết lập đóng gói dải. Họ áp đặt cấu trúc: họ chiếm một$2 \times 2$dấu chân hạn chế mức độ hiệu quả của chúng có thể được sắp xếp theo chiều rộng$K$. 

## Phương pháp tiếp cận 

Một cách tiếp cận đơn giản là nghĩ đến việc đặt các ô theo từng hàng. Chúng ta có thể mô phỏng việc xây dựng hình chữ nhật bằng cách đặt một$2 \times 2$ngói hoặc$1 \times 1$xếp một cách tham lam vào không gian hiện có và chuyển sang hàng tiếp theo khi đã đầy. Điều này đúng nếu được thực hiện cẩn thận, nhưng nó không khả thi vì$A$Và$B$có thể lớn và số lượng hàng cũng có thể lớn. Bất kỳ mô phỏng nào cũng sẽ xử lý hiệu quả từng ô hoặc từng hàng riêng lẻ, dẫn đến độ phức tạp tuyến tính trong$A + B$, quá chậm. 

The key observation is that the structure of optimal packing separates naturally into layers of height 2 for the large tiles. Mỗi$2 \times 2$gạch phải chiếm một$2 \times 2$khối, do đó, việc nhóm lưới thành các dải ngang có chiều cao 2 là điều đương nhiên. Bên trong một dải như vậy, chúng ta thực sự đang giải quyết có bao nhiêu$2 \times 2$gạch vừa với một dải chiều rộng$K$, điều này chỉ phụ thuộc vào số lượng cặp cột tồn tại. 

Mỗi$2 \times 2$ô tiêu thụ hai cột liền kề và hai hàng. Do đó, trong bất kỳ dải ngang có độ cao 2 nào, chúng ta có thể lắp tối đa$\lfloor K/2 \rfloor$gạch như vậy. Điều này biến vấn đề đặt$A$gạch lớn thành một câu hỏi công suất đơn giản: chúng ta cần bao nhiêu dải 2 hàng đầy đủ. 

Sau khi các ô lớn được đặt một cách tối ưu trong các dải này, các ô trống còn lại bên trong các dải đó có thể được tái sử dụng cho$1 \times 1$gạch lát. Chỉ sau khi lấp đầy tất cả không gian còn lại, chúng ta mới có thể cần thêm các hàng dành riêng cho$1 \times 1$gạch lát. 

Điều này làm giảm vấn đề từ việc đóng gói hình học sang sự kết hợp giữa năng lực và kế toán còn sót lại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng theo hàng |$O(A + B)$|$O(1)$| Quá chậm | 
| Dung lượng băng tần + số học còn sót lại |$O(1)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý lưới theo khối hai hàng vì mọi$2 \times 2$gạch kéo dài chính xác hai hàng. 

1. Tính xem có bao nhiêu$2 \times 2$gạch vừa khít với một dải ngang có chiều cao 2. Đây là$c = \lfloor K/2 \rfloor$. Mỗi ô tiêu thụ hai cột, vì vậy đây hoàn toàn là vấn đề phân vùng theo chiều rộng. 
2. Tính xem cần bao nhiêu dải đầy đủ để đặt tất cả$A$gạch lớn. Đây là$t = \lceil A / c \rceil$. Mỗi dải đóng góp chiều cao 2, do đó chiều cao hiện tại được phân bổ cho các ô lớn sẽ trở thành$2t$. 
3. Tính xem các dải này thực sự cung cấp bao nhiêu không gian. Mỗi ban nhạc có diện tích$2K$, vậy tổng diện tích trên các dải là$2Kt$. Gạch lớn tiêu thụ$4A$diện tích, vì vậy các ô trống còn sót lại sau khi đặt chúng là$free = 2Kt - 4A$. 
4. Sử dụng không gian trống này để đặt$1 \times 1$gạch lát. Nếu như$B \le free$, thì tất cả các ô nhỏ đều vừa với cấu trúc hiện có và câu trả lời đơn giản là$2t$. 
5. Ngược lại, tính các ô nhỏ còn lại$B' = B - free$. Chúng phải được đặt ở các hàng phụ có chiều cao bằng 1, mỗi hàng đóng góp công suất$K$. Số hàng bổ sung là$\lceil B'/K \rceil$. 
6. Câu trả lời cuối cùng là$X = 2t + \lceil B'/K \rceil$. 

### Tại sao nó hoạt động 

Bất biến chính là sau khi phân bổ$t$ban nhạc, tất cả$2 \times 2$gạch được đặt đầy đủ và không cần sắp xếp lại nữa có thể cải thiện việc sử dụng không gian vì mọi cách đóng gói khả thi$2 \times 2$gạch có chiều rộng$K$dải phân hủy thành các dải chiều cao 2 độc lập, mỗi dải có dung lượng$\lfloor K/2 \rfloor$. Bất kỳ sai lệch nào so với việc sử dụng toàn bộ băng tần sẽ chỉ gây lãng phí chiều rộng mà không tăng công suất cho các ô xếp lớn, do đó, tính tối ưu sẽ buộc độ bão hòa lên tới$t$ban nhạc. Khi cấu trúc đó được cố định, không gian còn lại hoàn toàn là hình chữ nhật và hoạt động giống như một vấn đề đóng gói thùng tiêu chuẩn cho các ô đơn vị. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    A = int(input().strip())
    B = int(input().strip())
    K = int(input().strip())

    c = K // 2  # number of 2x2 tiles per height-2 band

    if c == 0:
        # This case never happens since K >= 2, but kept for clarity
        c = 1

    t = (A + c - 1) // c

    total_area_in_bands = 2 * K * t
    used_by_large = 4 * A
    free = total_area_in_bands - used_by_large

    if B <= free:
        print(2 * t)
        return

    B -= free
    extra_rows = (B + K - 1) // K
    print(2 * t + extra_rows)

if __name__ == "__main__":
    solve()
```Việc triển khai phản ánh trực tiếp sự phân rã băng tần. Phần tinh tế duy nhất là tính toán chính xác công suất còn lại: thay vì theo dõi các vị trí hình học chính xác, chúng tôi dựa vào việc bảo toàn diện tích bên trong cấu trúc lát gạch tối ưu cố định. Việc phân chia trần đảm bảo chúng tôi không bao giờ đếm thiếu các dải hoặc hàng cần thiết. 

## Ví dụ đã hoạt động 

Vì tuyên bố không bao gồm rõ ràng các mẫu có cấu trúc nên chúng tôi minh họa hai kịch bản đại diện. 

### Ví dụ 1 

đầu vào:```
A = 4
B = 1
K = 5
```Chúng tôi tính toán$c = \lfloor 5/2 \rfloor = 2$. Vì vậy, mỗi dải chiều cao 2 có thể chứa 2 ô lớn. 

Chúng tôi cần$t = \lceil 4/2 \rceil = 2$ban nhạc. 

| Bước | Giá trị | 
| --- | --- | 
| Ban nhạc$t$| 2 | 
| Tổng diện tích ban nhạc |$2 \cdot 5 \cdot 2 = 20$| 
| Diện tích gạch lớn | 16 | 
| Dung lượng trống | 4 | 
| Còn lại B sau khi miễn phí | 0 | 

Vì tất cả$1 \times 1$gạch vừa vặn, chiều cao cuối cùng là$2t = 4$. 

Điều này cho thấy sự phân mảnh còn sót lại bên trong các dải được tái sử dụng một cách tự nhiên như thế nào. 

### Ví dụ 2 

đầu vào:```
A = 3
B = 20
K = 4
```Chúng tôi tính toán$c = 2$, Vì thế$t = \lceil 3/2 \rceil = 2$. 

| Bước | Giá trị | 
| --- | --- | 
| Ban nhạc$t$| 2 | 
| Khu vực ban nhạc | 16 | 
| Diện tích gạch lớn | 12 | 
| Dung lượng trống | 4 | 
| Còn lại B | 16 | 

Chúng tôi cần thêm hàng:$\lceil 16/4 \rceil = 4$. 

Chiều cao cuối cùng là$4 + 4 = 8$. 

Điều này chứng tỏ rằng một khi tất cả không gian có cấu trúc đã được sử dụng hết, vấn đề sẽ giảm xuống còn việc đóng gói 1D thuần túy. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(1)$| Chỉ một số phép tính số học cố định | 
| Không gian |$O(1)$| Không có công trình phụ trợ | 

Giải pháp là thời gian không đổi, phù hợp thoải mái với các ràng buộc lên đến$10^8$đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import ceil
    import builtins
    return sys.stdout.getvalue()

# Since full harness depends on integration, illustrative asserts are provided conceptually
# (In a real CF solution, solve() would be imported and called directly)

def solve_wrapper(inp):
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    sys.stdout = sys.__stdout__
    return out.getvalue().strip()

# minimal
assert solve_wrapper("1\n0\n2\n") == "2"

# only small tiles
assert solve_wrapper("0\n10\n3\n") == "4"

# only large tiles
assert solve_wrapper("5\n0\n4\n") == "6"

# mixed tight fit
assert solve_wrapper("3\n10\n4\n") == "6"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1,0,2 | 2 | lưới nhỏ nhất có thể | 
| 0,10,3 | 4 | chỉ đóng gói 1x1 | 
| 5,0,4 | 6 | chỉ đóng gói 2x2 | 
| 3,10,4 | 6 | tương tác giữa hàng thừa và hàng thừa | 

## Vỏ cạnh 

Khi nào$K = 2$, mỗi dải có thể chứa chính xác một$2 \times 2$ngói. Thuật toán vẫn hoạt động vì$c = 1$, do đó mỗi ô tiêu thụ toàn bộ dung lượng băng tần và$t = A$. Không có việc đóng gói phân số nào xảy ra và việc tính toán còn sót lại trở nên chính xác. 

Khi$B = 0$, thuật toán giảm một cách chính xác xuống chỉ tính toán số lượng băng tần cần thiết cho các ô lớn và không thêm hàng bổ sung nào. Điều này tránh việc phân bổ không gian 1 đơn vị không cần thiết. 

Khi$A = 0$, cấu trúc dải sụp đổ thành các dải có chiều cao bằng 0, vì vậy$t = 0$và giải pháp giảm xuống việc đóng gói hàng đơn giản$1 \times 1$gạch thành chiều rộng$K$, trở thành$\lceil B/K \rceil$.
