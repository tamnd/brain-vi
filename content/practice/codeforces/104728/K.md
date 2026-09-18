---
title: "CF 104728K - \u4e0d\u5b9a\u9879\u9009\u62e9\u9898"
description: "Chúng ta đang giải quyết một tình huống trong đó có $n$ tùy chọn riêng biệt và câu trả lời đúng là một tập hợp con không trống chưa biết nào đó của các tùy chọn này. Ban đầu, không có tùy chọn nào được chọn."
date: "2026-06-29T02:51:13+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104728
codeforces_index: "K"
codeforces_contest_name: "Huazhong University of Science of Technology Freshmen Cup 2023"
rating: 0
weight: 104728
solve_time_s: 76
verified: true
draft: false
---

[CF 104728K - \u4e0d\u5b9a\u9879\u9009\u62e9\u9898](https://codeforces.com/problemset/problem/104728/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 16s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta đang giải quyết một tình huống có$n$các tùy chọn riêng biệt và câu trả lời đúng là một số tập hợp con không trống chưa biết của các tùy chọn này. Ban đầu, không có tùy chọn nào được chọn. Trong một lần di chuyển, chúng ta được phép bật hoặc tắt một tùy chọn, nghĩa là chúng ta có thể tự do di chuyển giữa các tập hợp con của$n$các phần tử bằng cách lật từng phần tử một. 

Thời điểm tập hợp con hiện được chọn của chúng tôi khớp với tập hợp con chính xác bị ẩn, quá trình sẽ dừng ngay lập tức. Vì tập hợp con chính xác chưa được xác định và có thể là bất kỳ tập hợp con nào không trống, nên chúng tôi muốn thiết kế một chuỗi các trạng thái tập hợp con để đảm bảo rằng cuối cùng chúng tôi sẽ đạt được mọi tập hợp con không trống có thể có. Chi phí là số lượng hoạt động chuyển đổi và chúng tôi quan tâm đến số lượng hoạt động tối thiểu có thể có trong trường hợp xấu nhất trên tất cả các câu trả lời ẩn có thể có. 

Giải thích chính là chúng ta đang xây dựng một bước đi trên đồ thị của tất cả các tập hợp con của một$n$- tập hợp phần tử, trong đó các cạnh kết nối các tập hợp con khác nhau bởi chính xác một phần tử. Chúng ta bắt đầu từ tập trống và chúng ta muốn một bước đi ghé thăm mọi tập con không trống ít nhất một lần, giảm thiểu độ dài của bước đi trong trường hợp xấu nhất, tương đương với việc tìm đường đi ngắn nhất bao phủ tất cả các đỉnh không trống trong biểu đồ siêu khối này bắt đầu từ đỉnh trống. 

Ràng buộc$n \le 20$ngụ ý rằng có nhiều nhất$2^{20}$tập hợp con, tức là khoảng một triệu. Điều này ngay lập tức gợi ý rằng bất kỳ giải pháp nào liệt kê rõ ràng tất cả các tập hợp con đều khả thi, nhưng bất kỳ giải pháp nào theo cấp số nhân trong cơ sở lớn hơn hoặc liên quan đến hoán vị trên tất cả các tập hợp con sẽ là quá lớn. Tuy nhiên, cấu trúc này có tính đối xứng và gợi ý mạnh mẽ về việc duyệt theo kiểu mã Gray hơn là tìm kiếm tùy ý. 

Trường hợp cạnh tinh tế xuất hiện khi$n = 1$. Chỉ có một tập hợp con không trống nên câu trả lời chỉ đơn giản là một thao tác. Một trường hợp cạnh khác là$n = 2$, trong đó độ truyền tối thiểu không phải là 2 mà là 3, bởi vì chúng ta phải di chuyển qua các trạng thái theo cách bao gồm cả hai tập con đơn bắt đầu từ tập trống. 

Ví dụ, với$n = 2$, tập hợp con là$\emptyset, \{1\}, \{2\}, \{1,2\}$. Bất kỳ chuỗi hợp lệ nào bắt đầu từ trống đều phải chú ý đến các ràng buộc kề và không thể nhảy giữa các tập hợp con. Một giả định ngây thơ rằng chúng ta có thể tiếp cận tất cả các tập hợp con trong$2^n - 1$các bước sai vì nó tính các nút chứ không phải các cạnh trong một đường dẫn. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là mô hình hóa vấn đề bằng cách tìm kiếm trên tất cả các chuỗi trạng thái tập hợp con bắt đầu từ tập hợp trống, trong đó mỗi bước lật một bit. Chúng tôi muốn một chuỗi truy cập vào mọi tập hợp con không trống ít nhất một lần và chúng tôi muốn giảm thiểu tổng số lần di chuyển trong trường hợp xấu nhất. Điều này tương đương với việc tìm đường đi ngắn nhất đi qua tất cả các đỉnh trong một$n$đồ thị siêu khối có chiều bắt đầu từ$0$. 

Việc tìm kiếm đơn giản trên tất cả các bước đi có thể là không thể bởi vì ngay cả khi giới hạn ở những đường đi đơn giản, số lượng đường đi Hamilton trong một siêu khối tăng lên cực kỳ nhanh. Ngay cả việc tạo ra tất cả các hoán vị của các tập hợp con sẽ theo thứ tự$(2^n)!$, điều này hoàn toàn không thể thực hiện được ngay cả đối với$n = 10$. 

Quan sát quan trọng là cấu trúc của siêu khối cho phép sắp xếp thứ tự mã Gray. Mã Gray là một chuỗi gồm tất cả các mặt nạ bit có độ dài$n$sao cho các mặt nạ liên tiếp khác nhau đúng một bit. Thuộc tính này khớp chính xác với hoạt động được phép. Nếu chúng ta có thể tạo mã Gray bắt đầu ở mặt nạ trống thì việc duyệt qua chuỗi này sẽ truy cập vào tất cả các tập hợp con và số thao tác chính xác là số lần chuyển đổi, tức là$2^n - 1$. 

Cái nhìn sâu sắc hơn là chúng ta không cần coi bài toán là “truy cập tất cả các tập hợp con” theo nghĩa lý thuyết đồ thị. Thay vào đó, chúng ta nhận ra rằng siêu khối có đường đi Hamilton bắt đầu từ 0 và mã Gray đưa ra cấu trúc rõ ràng của đường đi đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên mọi bước đi | Vượt cấp số mũ$2^n$| Hàm mũ | Quá chậm | 
| Xây dựng mã màu xám |$O(2^n)$|$O(2^n)$hoặc$O(1)$chỉ đầu ra | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta chuyển bài toán thành việc tạo ra chuỗi mã Gray trên$n$-bit số nguyên. 

1. Bắt đầu từ mặt nạ$0$, đại diện cho không có tùy chọn nào được chọn. Điều này tương ứng với trạng thái ban đầu. 
2. Với mỗi số nguyên$i$từ$1$ĐẾN$2^n - 1$, tính giá trị mã Gray$g(i) = i \oplus (i >> 1)$. Công thức này đảm bảo rằng các giá trị liên tiếp khác nhau đúng một bit. 
3. Theo dõi quá trình chuyển đổi giữa các giá trị mã Gray liên tiếp. Mỗi lần chuyển đổi tương ứng với việc chuyển đổi chính xác một tùy chọn. 
4. Đếm số lần chuyển đổi cần thiết để đạt được mọi tập con khác 0. Kể từ khi chúng tôi bắt đầu tại$g(0) = 0$, tổng số thao tác là số cạnh trong đường dẫn, tức là$2^n - 1$. 

Do đó, đầu ra là trực tiếp$2^n - 1$. 

### Tại sao nó hoạt động 

Bộ của tất cả$n$-bit mặt nạ tạo thành một$n$đồ thị siêu khối chiều, trong đó các cạnh tương ứng với việc lật một bit. Thứ tự mã Gray là đường đi Hamilton trong biểu đồ này, nghĩa là nó truy cập mọi đỉnh đúng một lần trong khi di chuyển dọc theo các cạnh hợp lệ. Vì chúng ta bắt đầu từ tập hợp con trống, mã Gray bắt đầu từ 0 đảm bảo rằng mọi tập hợp con đều được gặp chính xác một lần, do đó mọi câu trả lời đúng có thể có đều được nhấn chính xác một lần. Do đó, số bước di chuyển chính xác là số cạnh trên đường đi này, ít hơn một so với số đỉnh đã đi qua. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input().strip())
print((1 << n) - 1)
```Giải pháp tránh việc tạo ra các tập con hoặc xây dựng chuỗi mã Gray một cách rõ ràng. Điểm mấu chốt là nhận ra rằng đường truyền tối ưu tồn tại và độ dài của nó được xác định hoàn toàn bằng số đỉnh trong siêu khối. 

biểu thức$(1 << n) - 1$tính toán$2^n - 1$, tương ứng với tất cả các tập con không trống. Vì chiến lược tối ưu tương ứng với đường đi Hamilton bắt đầu từ tập trống, nên câu trả lời chính xác là số lần chuyển đổi trong đường đi đó. 

Không có vấn đề ranh giới nào ngoài việc đảm bảo rằng$n = 0$không được phép bởi các ràng buộc của vấn đề. Vì$n = 1$, công thức mang lại kết quả chính xác$1$. 

## Ví dụ đã hoạt động 

cho$n = 2$, chúng ta có bốn tập con:$00, 01, 10, 11$. Một chuỗi mã Gray hợp lệ là$00 \rightarrow 01 \rightarrow 11 \rightarrow 10$. 

| Bước | Mặt nạ hiện tại | Hoạt động | 
| --- | --- | --- | 
| 0 | 00 | bắt đầu | 
| 1 | 01 | chuyển đổi bit 0 | 
| 2 | 11 | chuyển đổi bit 1 | 
| 3 | 10 | chuyển đổi bit 0 | 

Dấu vết này cho thấy rằng tất cả các tập hợp con đều được truy cập và tổng số phép toán là 3, khớp với$2^2 - 1$. 

Vì$n = 3$, một chuỗi có thể là:$000 \rightarrow 001 \rightarrow 011 \rightarrow 010 \rightarrow 110 \rightarrow 111 \rightarrow 101 \rightarrow 100$. 

| Bước | Mặt nạ hiện tại | Hoạt động | 
| --- | --- | --- | 
| 0 | 000 | bắt đầu | 
| 1 | 001 | bit lật 0 | 
| 2 | 011 | lật bit 1 | 
| 3 | 010 | bit lật 0 | 
| 4 | 110 | lật bit 2 | 
| 5 | 111 | bit lật 0 | 
| 6 | 101 | lật bit 1 | 
| 7 | 100 | bit lật 0 | 

Điều này xác nhận rằng tất cả 8 tập hợp con được truy cập chính xác một lần với 7 lần chuyển tiếp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(1)$| Chỉ tính lũy thừa của hai và trừ đi một | 
| Không gian |$O(1)$| Không sử dụng cấu trúc phụ trợ | 

Việc tính toán là thời gian không đổi bất kể$n$, đó là tầm thường dưới sự ràng buộc$n \le 20$. Mặc dù cấu trúc tổ hợp cơ bản liên quan đến$2^n$nêu rõ, vấn đề chuyển thành biểu thức dạng đóng, làm cho nó cực kỳ hiệu quả. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline
    n = int(input().strip())
    return str((1 << n) - 1)

# provided samples
assert run("1\n") == "1", "sample 1"
assert run("2\n") == "3", "sample 2"
assert run("3\n") == "7", "sample 3"

# custom cases
assert run("4\n") == "15", "basic exponential growth check"
assert run("5\n") == "31", "larger small n"
assert run("10\n") == "1023", "power of two minus one correctness"
assert run("20\n") == str((1 << 20) - 1), "upper bound stress case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 4 | 15 | độ chính xác nhỏ ngoài mẫu | 
| 5 | 31 | tính nhất quán của mẫu | 
| 10 | 1023 | độ đúng theo cấp số nhân lớn hơn | 
| 20 |$2^{20}-1$| độ đúng của điều kiện biên | 

## Vỏ cạnh 

Khi nào$n = 1$, chỉ có một tập con không rỗng. Bắt đầu từ chỗ trống, chúng ta cần chính xác một lần chuyển đổi để đạt được nó. Công thức$2^1 - 1 = 1$khớp trực tiếp với điều này và không tồn tại sự mơ hồ về đường dẫn vì chỉ có một đỉnh để truy cập. 

Khi$n = 2$, cấu trúc trở thành siêu khối không tầm thường nhỏ nhất. Một kỳ vọng ngây thơ có thể là hai bước di chuyển là đủ vì có hai tập hợp con không trống, nhưng các ràng buộc kề cận buộc phải chuyển đổi trạng thái trung gian dẫn đến tổng cộng ba bước di chuyển. Cấu trúc mã Gray đảm bảo rằng tất cả các tập hợp con vẫn được truy cập một cách tối ưu và giá trị được tính toán$2^2 - 1 = 3$khớp chính xác với độ dài truyền tải được yêu cầu.
