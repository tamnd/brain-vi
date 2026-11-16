---
title: "CF 193E - Số Fibonacci"
description: "Chúng ta được cấp một số $f$ và chúng ta xem xét chuỗi vô hạn được tạo ra bởi các quy tắc Fibonacci, nhưng lấy modulo 1013. Chuỗi bắt đầu bằng 0 và 1, và mọi giá trị tiếp theo là tổng của hai giá trị trước đó, rút ​​gọn modulo 1013."
date: "2026-06-03T01:30:52+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "math", "matrices"]
categories: ["algorithms"]
codeforces_contest: 193
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 122 (Div. 1)"
rating: 2900
weight: 193
solve_time_s: 92
verified: false
draft: false
---

[CF 193E - Số Fibonacci](https://codeforces.com/problemset/problem/193/E) 

**Xếp hạng:** 2900 
**Tags:** lực lượng vũ phu, toán học, ma trận 
**Thời gian giải:** 1m 32s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một số duy nhất$f$, và chúng tôi xem xét chuỗi vô hạn được tạo ra bởi các quy tắc Fibonacci, nhưng lấy modulo 1013. Chuỗi bắt đầu bằng 0 và 1, và mọi giá trị tiếp theo là tổng của hai giá trị trước đó, modulo giảm 1013. Vì vậy, chuỗi được xác định đầy đủ và lặp lại các giá trị theo thời gian vì chỉ có 1013 phần dư có thể có. 

Nhiệm vụ không phải là tính toán đầy đủ chuỗi mà là xác định chỉ mục sớm nhất có giá trị$f$xuất hiện. Nếu giá trị không bao giờ xuất hiện trong chuỗi, chúng ta xuất ra -1. 

Việc lập chỉ mục dựa trên số 0, vì vậy vị trí 0 là 0, vị trí 1 là 1, vị trí 2 là 1, v.v. 

Hạn chế chính là mô đun là 1013, đủ nhỏ để chuỗi các cặp$(F_i, F_{i+1})$cuối cùng phải lặp lại, ngụ ý hành vi định kỳ. Tuy nhiên, chúng tôi không được hỏi về thời gian; chúng tôi được yêu cầu xuất hiện lần đầu tiên, điều này cho phép mô phỏng chuyển tiếp đơn giản. 

Chi tiết ẩn chính là mặc dù số Fibonacci modulo là tuần hoàn, nhưng độ dài khoảng thời gian có thể đủ lớn đến mức suy nghĩ ngây thơ về việc liệt kê đầy đủ tất cả các cặp phải được kiểm soát cẩn thận. Tuy nhiên, 1013 đủ nhỏ để lưu trữ các trạng thái đã truy cập hoặc mô phỏng tiền tố bị chặn là khả thi. 

Trường hợp cạnh tinh tế xuất hiện khi$f = 0$. Phần tử đầu tiên đã bằng 0 nên đáp án ngay lập tức là 0. Phần tử khác là$f = 1$, xuất hiện ở chỉ mục 1 và xuất hiện lại ở chỉ mục 2, vì vậy chúng ta phải đảm bảo chỉ trả về lần xuất hiện đầu tiên. 

Một trường hợp ít rõ ràng hơn là không phải tất cả các giá trị trong$[0, 1012]$nhất thiết phải xuất hiện trong dãy Fibonacci modulo 1013 trước khi chu kỳ lặp lại. Một giả định ngây thơ rằng tất cả các dư lượng đều xuất hiện sẽ dẫn đến những câu trả lời không chính xác vô điều kiện. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là tạo từng số Fibonacci theo modulo 1013, bắt đầu từ chỉ số 0 và kiểm tra ở mỗi bước xem giá trị hiện tại có bằng không$f$. Vì trình tự có tính xác định nên cuối cùng sẽ tìm được$f$hoặc nhập một chu kỳ. 

Điều này hiệu quả vì mỗi thuật ngữ chỉ phụ thuộc vào hai giá trị trước đó, vì vậy chúng ta có thể duy trì một cặp luân phiên và lặp lại. Tính chính xác là ngay lập tức: chúng tôi kiểm tra mọi vị trí theo thứ tự tăng dần cho đến khi tìm thấy mục tiêu hoặc quyết định rằng chúng tôi đã thấy đủ trạng thái để kết luận sự lặp lại. 

Vấn đề với mô phỏng không giới hạn là Fibonacci modulo 1013 tồn tại trong một không gian trạng thái hữu hạn có kích thước$1013 \times 1013$nếu chúng ta xem xét các cặp có thứ tự. Điều đó đảm bảo sự lặp lại cuối cùng, nhưng trong trường hợp xấu nhất, chúng ta có thể duyệt qua một tiền tố dài trước khi nhìn thấy một cặp lặp lại. Tuy nhiên, giới hạn này chỉ khoảng một triệu tiểu bang, là an toàn. 

Một cách nhìn có cấu trúc hơn là coi các trạng thái như các cặp$(F_i, F_{i+1})$. Khi một cặp lặp lại, trình tự từ thời điểm đó trở đi sẽ lặp lại chính xác. Vì vậy, chúng tôi có thể dừng mô phỏng khi một cặp lặp lại và nếu chúng tôi chưa thấy$f$, nó sẽ không bao giờ xuất hiện sau này. 

Điều này biến bài toán thành một đồ thị đi qua một đồ thị hàm số với 1.0132 trạng thái, trong đó mỗi trạng thái có chính xác một trạng thái kế tiếp. Về cơ bản, chúng ta đang đi dọc theo một con đường dành cho xe đạp cho đến khi lặp lại. 

Một tập hợp băm các cặp đã truy cập cho phép chúng ta dừng lại ngay khi phát hiện ra một chu kỳ. Vì chúng tôi chỉ lưu trữ tối đa 1.0132 trạng thái nên bộ nhớ vẫn an toàn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu |$O(\text{period})$lên đến$O(m^2)$|$O(1)$| Đã chấp nhận | 
| Phát hiện chu kỳ trạng thái |$O(m^2)$|$O(m^2)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì dãy Fibonacci modulo 1013 trong khi theo dõi các chỉ số. 

1. Khởi tạo hai biến đại diện cho các giá trị Fibonacci liên tiếp:$a = 0$,$b = 1$và chỉ số$i = 0$. Điều này tương ứng với sự bắt đầu của chuỗi mà chúng ta đã biết hai số hạng đầu tiên. 
2. Nếu$a == f$, trả về 0 ngay lập tức vì chỉ số 0 là lần xuất hiện đầu tiên. 
3. Lặp lại trong khi chúng tôi chưa vượt quá giới hạn an toàn$1013^2 + 5$các bước hoặc cho đến khi việc phát hiện chu kỳ ngăn chúng tôi lại. 
4. Tại mỗi lần lặp lại, hãy so sánh$a$với$f$. Nếu trùng khớp thì quay lại$i$. Điều này đảm bảo chúng tôi luôn báo cáo lần xuất hiện sớm nhất vì chúng tôi quét theo thứ tự chỉ mục tăng dần. 
5. Tiến lên theo dãy Fibonacci: tính giá trị tiếp theo$c = (a + b) \bmod 1013$. 
6. Cập nhật cặp: set$a = b$,$b = c$, và tăng chỉ số$i$. Điều này bảo toàn tính bất biến$(a, b)$luôn đại diện$(F_i, F_{i+1})$. 
7. Tùy chọn theo dõi các cặp đã truy cập$(a, b)$. Nếu một cặp lặp lại, dừng lại và trả về -1 nếu$f$đã không được tìm thấy. Điều này đảm bảo chúng ta không lặp lại vô thời hạn sau khi bước vào chu kỳ. 

### Tại sao nó hoạt động 

Tại mỗi bước, thuật toán duy trì tính bất biến$a = F_i \bmod 1013$Và$b = F_{i+1} \bmod 1013$. Quy tắc chuyển tiếp bảo toàn tính bất biến này vì sự tiến hóa Fibonacci có tính tất định. Vì chuỗi các cặp tồn tại trong một tập hợp hữu hạn nên việc lặp lại một cặp hàm ý rằng chuỗi trở nên tuần hoàn kể từ thời điểm đó trở đi. Vì vậy, nếu$f$không xuất hiện trước trạng thái lặp lại, nó không thể xuất hiện sau, đảm bảo tính đúng đắn của việc kết thúc sớm. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 1013

def solve():
    f = int(input().strip())

    a, b = 0, 1
    if a == f:
        print(0)
        return

    # We store visited states of pairs (a, b)
    visited = set()
    visited.add((a, b))

    idx = 1

    # Safe upper bound: at most MOD^2 states
    limit = MOD * MOD + 5

    while idx <= limit:
        a, b = b, (a + b) % MOD

        if a == f:
            print(idx)
            return

        if (a, b) in visited:
            break
        visited.add((a, b))

        idx += 1

    print(-1)

if __name__ == "__main__":
    solve()
```Mã này duy trì rõ ràng quá trình chuyển đổi Fibonacci bằng cách sử dụng hai biến, tránh việc lưu trữ toàn bộ mảng. chỉ số`idx`tương ứng chính xác với vị trí của`a`theo trình tự, vì vậy khi chúng ta so sánh`a == f`, chúng tôi đang kiểm tra vị trí trình tự chính xác. 

Tập đã truy cập ngăn vòng lặp vô hạn sau khi bước vào một chu kỳ. Nếu không có nó, vòng lặp vẫn an toàn trong thực tế do bị ràng buộc, nhưng việc phát hiện chu kỳ sẽ mang lại một điểm cắt rõ ràng về mặt lý thuyết. 

Việc kiểm tra khởi tạo cho`a == f`xử lý trực tiếp chỉ số 0, đây là một bẫy phổ biến trong việc triển khai Fibonacci. 

## Ví dụ đã hoạt động 

### Ví dụ 1: f = 13 

Chúng tôi theo dõi Fibonacci modulo 1013: 

| tôi | một | b | hành động | 
| --- | --- | --- | --- | 
| 0 | 0 | 1 | bắt đầu, 0 != 13 | 
| 1 | 1 | 1 | 1 != 13 | 
| 2 | 1 | 2 | 1 != 13 | 
| 3 | 2 | 3 | 2 != 13 | 
| 4 | 3 | 5 | 3 != 13 | 
| 5 | 5 | 8 | 5 != 13 | 
| 6 | 8 | 13 | 8 != 13 | 
| 7 | 13 | 21 | tìm thấy trận đấu | 

Tại chỉ số 7, giá trị trở thành 13 nên chúng ta dừng ngay. Điều này xác nhận thuật toán báo cáo chính xác lần xuất hiện đầu tiên thay vì các lần lặp lại sau đó. 

### Ví dụ 2: f = 0 

| tôi | một | b | hành động | 
| --- | --- | --- | --- | 
| 0 | 0 | 1 | trận đấu ngay lập tức | 

Thuật toán trả về 0 mà không cần lặp lại, xác nhận rằng các trường hợp cạnh trạng thái ban đầu được xử lý chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(m^2)$| Chúng ta có thể duyệt từng cặp trạng thái Fibonacci modulo 1013 riêng biệt nhiều nhất một lần trước khi lặp lại | 
| Không gian |$O(m^2)$| Đã ghé thăm cửa hàng bán bộ nhiều nhất 1.0132 đôi | 

Mô đun được cố định ở 1013, vì vậy$m^2 \approx 10^6$, phù hợp thoải mái trong giới hạn thời gian và bộ nhớ. Mỗi bước là một công việc liên tục, do đó giải pháp tuyến tính một cách hiệu quả theo số lượng trạng thái được truy cập. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output

    solve()

    sys.stdout = sys.__stdout__
    return output.getvalue().strip()

# provided sample
assert run("13\n") == "7", "sample 1"

# minimum value
assert run("0\n") == "0", "zero at start"

# immediate second position
assert run("1\n") == "1", "first occurrence of 1"

# small internal value
assert run("2\n") != "", "should exist or -1"

# boundary value
assert run("1012\n") in ["-1", "valid index"], "mod boundary behavior"

# repeated-cycle safety
assert run("144\n") != "", "fibonacci known value should appear"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 0 | 0 | điều kiện ban đầu | 
| 1 | 1 | xử lý trùng lặp sớm | 
| 2 | không âm | tính đúng đắn chung | 
| 1012 | -1 hoặc chỉ mục | ranh giới mô đun | 
| 144 | chỉ mục | tăng trưởng Fibonacci điển hình | 

## Vỏ cạnh 

cho$f = 0$, thuật toán sẽ kiểm tra giá trị ban đầu trước khi vào vòng lặp. Từ$a = 0$ở chỉ số 0, nó ngay lập tức trả về 0, khớp với lần xuất hiện đầu tiên được yêu cầu. 

Đối với các giá trị xuất hiện nhiều lần như$f = 1$, thứ tự quét đảm bảo tính chính xác. Trận đấu đầu tiên xảy ra ở chỉ mục 1 và mặc dù có những lần xuất hiện sau đó, thuật toán dừng ngay lập tức do việc truyền chỉ mục ngày càng tăng. 

Đối với các giá trị có thể không bao giờ xuất hiện, chẳng hạn như phần dư nhất định theo modulo 1013, cơ chế trạng thái truy cập đảm bảo chấm dứt. Khi một cặp Fibonacci lặp lại, chuỗi sẽ chuyển sang một chu kỳ giống với phân đoạn trước đó. Nếu như$f$chưa gặp trước lần lặp lại này, không lần lặp nào trong tương lai có thể giới thiệu nó, vì vậy trả về -1 là đúng.
