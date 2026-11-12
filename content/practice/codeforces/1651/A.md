---
title: "CF 1651A - Vòng loại"
description: "Chúng tôi đang mô phỏng một giải đấu loại trực tiếp trong đó số lượng người chơi là lũy thừa của hai. Người chơi được gắn nhãn từ 1 đến $2^n$ và họ liên tục đối đầu theo vòng cho đến khi chỉ còn lại một người."
date: "2026-06-10T03:48:07+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1651
codeforces_index: "A"
codeforces_contest_name: "Educational Codeforces Round 124 (Rated for Div. 2)"
rating: 800
weight: 1651
solve_time_s: 88
verified: true
draft: false
---

[CF 1651A - Vòng loại](https://codeforces.com/problemset/problem/1651/A) 

**Đánh giá:** 800 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 28s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang mô phỏng một giải đấu loại trực tiếp trong đó số lượng người chơi là lũy thừa của hai. Người chơi được dán nhãn từ 1 đến$2^n$, và họ liên tục đối đầu theo từng hiệp cho đến khi chỉ còn lại một người. 

Quy tắc ghép đôi được cố định và không phụ thuộc vào kỹ năng hoặc kết quả trước đó: trong mỗi vòng, người chơi được nhóm tuần tự theo cặp. Đầu tiên, 1 chơi 2, 3 chơi 4, v.v. Những người chiến thắng ở các cặp liền kề sẽ gặp nhau ở vòng tiếp theo theo cách có cấu trúc tương tự, do đó bảng đấu hoàn toàn mang tính quyết định. 

Điều làm cho vấn đề này trở nên thú vị là quy tắc bất thường quyết định từng trận đấu. Nếu hai người chơi$x$Và$y$cạnh tranh, sự ngang bằng kiểm soát mọi thứ. Khi$x+y$là số lẻ, chỉ số nhỏ hơn sẽ thắng. Khi$x+y$chẵn thì chỉ số nào lớn hơn sẽ thắng. Điều này có nghĩa là kết quả không đơn điệu theo thứ tự chỉ số và giải đấu không phải là một khung tiêu chuẩn “cao hơn luôn thắng” hoặc “thấp hơn luôn thắng”. 

Nhiệm vụ là xác định chỉ mục ban đầu nào tồn tại sau tất cả$n$vòng. 

Các ràng buộc là cực kỳ nhỏ về mặt tính toán. Với$n \le 30$, quy mô giải đấu có thể đạt tới$2^{30}$, tức là khoảng một tỷ người chơi. Điều đó ngay lập tức loại trừ mọi mô phỏng đối với tất cả người tham gia hoặc thậm chí xây dựng các vòng trung gian một cách rõ ràng. Bất kỳ giải pháp nào cũng phải chạy theo thời gian tỷ lệ thuận với$n$, không phải số lượng người chơi. 

Một cạm bẫy tinh vi là giả định tính nhất quán cục bộ hàm ý tính đơn điệu toàn cầu. Ví dụ, người ta có thể giả định không chính xác rằng chỉ số thấp hoặc chỉ số cao luôn chiếm ưu thế. Nhưng một tấm séc nhỏ đã phá vỡ trực giác này: vì$n=1$, người chơi 1 đánh bại 2, trong khi đối với các vòng cao hơn, các tương tác sẽ thay đổi tùy thuộc vào mức độ lan truyền chẵn lẻ, do đó, không có quy tắc đơn giản “luôn bên trái thắng” hoặc “luôn bên phải thắng” nào được áp dụng cục bộ. 

## Phương pháp tiếp cận 

Một mô phỏng bạo lực sẽ xây dựng rõ ràng tất cả$2^n$người chơi và liên tục mô phỏng các vòng đấu. Trong mỗi vòng, chúng tôi so sánh các cặp liền kề, tính toán người chiến thắng và lập danh sách tiếp theo. Mỗi vòng giảm một nửa số người chơi nên tổng công việc là$2^n + 2^{n-1} + \dots + 1$, đó là$O(2^n)$. Vì$n=30$, điều này hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là chúng tôi không cần theo dõi tất cả người chơi, chỉ cần theo dõi danh tính của người chiến thắng cuối cùng. Cấu trúc của việc ghép đôi và quy tắc xác định ngụ ý rằng giải đấu có một người chiến thắng toàn cầu cố định độc lập với việc mô phỏng các trạng thái trung gian. 

Nếu chúng ta xem xét các trường hợp nhỏ một cách cẩn thận, một khuôn mẫu sẽ xuất hiện. Vì$n=1$, người chiến thắng là 1. Vì$n=2$, người chiến thắng cuối cùng là 3. Vì$n=3$, nó là 7. Điều này cho thấy người chiến thắng luôn là chỉ số tối đa có thể,$2^n - 1$, ngoại trừ$n=1$trong đó là 1 vì chỉ có hai người chơi và quy tắc buộc chỉ số nhỏ hơn sẽ thắng trận đấu đó. 

Lý do sâu xa hơn là trong bất kỳ trận đấu nào giữa hai người sống sót từ các vòng trước, một trong số họ phải là đại diện lớn nhất hiện có trong phân khúc của nó và quy tắc chẵn lẻ đảm bảo rằng chỉ số lớn hơn sẽ chiếm ưu thế bất cứ khi nào nó được ghép đôi với một đối thủ có cấu trúc tương đương ở các vòng sau. Điều này dẫn đến hiệu ứng xếp tầng trong đó chỉ số có thể tiếp cận cao nhất vẫn tồn tại sau mỗi bước hợp nhất. 

Do đó, thay vì mô phỏng, chúng ta tính toán trực tiếp câu trả lời từ mẫu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu |$O(2^n)$|$O(2^n)$| Quá chậm | 
| Công thức trực tiếp |$O(1)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc$n$. Điều này xác định một giải đấu có quy mô$2^n$, nhưng chúng tôi sẽ không xây dựng nó. 
2. Nếu$n = 1$, trả lại trực tiếp 1 vì chỉ có một trận đấu và luật buộc người chơi 1 phải thắng. 
3. Ngược lại tính toán$2^n - 1$với tư cách là người chiến thắng. 
4. Xuất kết quả. 

Lý do chúng ta trừ 1 thay vì sử dụng$2^n$xuất phát từ cơ cấu loại bỏ. Mặc dù có$2^n$người tham gia, quy tắc tương tác đảm bảo rằng người sống sót cuối cùng không bao giờ là chỉ số tối đa đầy đủ mà là chỉ số đại diện cấu trúc cao thứ hai. 

### Tại sao nó hoạt động 

Cấu trúc giải đấu liên tục sáp nhập các phân đoạn liền kề có quy mô bằng nhau. Trong mỗi lần hợp nhất, đại diện hiệu quả của một phân khúc sẽ trở thành người sống sót được lập chỉ mục tối đa theo quy tắc chẵn lẻ. Khi các phân khúc kết hợp lại, chính những đại diện này sẽ cạnh tranh và mô hình thống trị tương tự sẽ lặp lại ở mọi cấp độ. Chỉ số cao nhất trong hệ thống luôn không có điểm yếu về cấu trúc trong các vòng sau vì nó luôn là yếu tố lớn nhất trong bối cảnh ghép nối cục bộ của nó. Sự ổn định quy nạp này đảm bảo nó tồn tại trong mọi sự hợp nhất và tất cả các ứng cử viên khác đều bị loại bỏ trong một số giai đoạn hợp nhất trước đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    if n == 1:
        print(1)
    else:
        print((1 << n) - 1)
```Việc triển khai mã hóa trực tiếp cấu trúc dẫn xuất thay vì mô phỏng giải đấu. Phép dịch trái tính toán$2^n$một cách hiệu quả trong thời gian liên tục. Chúng tôi trừ 1 cho tất cả các trường hợp ngoại trừ trường hợp cơ sở$n=1$, điều này tránh trả về sai 1 khi$2^1 - 1 = 1$trùng khớp ngẫu nhiên nhưng không đại diện cho mẫu chung. 

Sự phân nhánh cho$n=1$được đưa vào để làm cho logic rõ ràng và ngăn việc hiểu sai công thức là hợp lệ toàn cầu mà không cần xử lý ngoại lệ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 3
```Chúng tôi tính toán$2^3 - 1 = 7$. 

| Bước | Biểu hiện | Giá trị | 
| --- | --- | --- | 
| 1 | đọc n | 3 | 
| 2 | tính toán$2^n$| 8 | 
| 3 | trừ 1 | 7 | 

Điều này xác nhận rằng người sống sót cuối cùng là 7, phù hợp với kết quả mẫu đã biết. 

### Ví dụ 2 

đầu vào:```
n = 1
```| Bước | Biểu hiện | Giá trị | 
| --- | --- | --- | 
| 1 | đọc n | 1 | 
| 2 | áp dụng trường hợp cơ sở | trở lại 1 | 

Trường hợp này thể hiện việc ghi đè rõ ràng công thức chung để phù hợp với hành vi của giải đấu một trận. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(t)$| Mỗi trường hợp thử nghiệm được xử lý với số lượng thao tác không đổi | 
| Không gian |$O(1)$| Chỉ một số số nguyên được lưu trữ bất kể kích thước đầu vào | 

Giải pháp dễ dàng phù hợp với các ràng buộc bởi vì ngay cả$t = 30$dẫn đến tính toán không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        if n == 1:
            out.append("1")
        else:
            out.append(str((1 << n) - 1))
    return "\n".join(out)

# provided samples
assert run("2\n3\n1\n") == "7\n1"

# minimum size
assert run("1\n1\n") == "1"

# small higher case
assert run("1\n2\n") == "3"

# medium case
assert run("1\n4\n") == "15"

# larger case
assert run("1\n5\n") == "31"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n=1 | 1 | trường hợp cơ sở đúng đắn | 
| n=2 | 3 | giải đấu nhỏ nhất không tầm thường | 
| n=4 | 15 | tính nhất quán của mô hình quyền lực | 
| n=5 | 31 | chia tỷ lệ công thức chung | 

## Vỏ cạnh 

cho$n=1$, thuật toán trực tiếp trả về 1 thay vì áp dụng công thức. Điều này tránh việc dựa vào biểu thức$2^n - 1$như một quy luật phổ quát, mặc dù nó tình cờ mang lại kết quả tương tự ở đây. Nhánh rõ ràng đảm bảo tính chính xác bắt nguồn từ định nghĩa khớp đơn của vấn đề thay vì ngoại suy mẫu. 

Vì$n=2$, phép tính cho$2^2 - 1 = 3$. Tracing the tournament: 1 beats 2, 3 beats 4, then 1 faces 3. Since$1+3=4$chẵn thì chỉ số nào lớn hơn sẽ thắng nên 3 thắng. Điều này phù hợp chính xác với đầu ra của công thức. 

Đối với lớn hơn$n$, cấu trúc ghép nối lặp lại đảm bảo rằng chỉ số có thể tiếp cận cao nhất luôn được bảo toàn qua từng cấp độ hợp nhất, do đó không có ghép nối trung gian nào có thể loại bỏ nó.
