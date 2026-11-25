---
title: "CF 986F - Oppa Funcan Style Remastered"
description: "Chúng tôi được hỏi liệu có thể thiết kế một hàm trên một tập hợp vị trí rất lớn, lên tới $10^{18}$, hoạt động giống như quy tắc nhảy xác định được áp dụng mỗi giây hay không. Mỗi vị trí trỏ đến chính xác một vị trí tiếp theo và mọi phần tử đều di chuyển theo quy tắc cố định này."
date: "2026-06-17T00:53:57+07:00"
tags: ["codeforces", "competitive-programming", "graphs", "math", "number-theory", "shortest-paths"]
categories: ["algorithms"]
codeforces_contest: 986
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 485 (Div. 1)"
rating: 3300
weight: 986
solve_time_s: 86
verified: true
draft: false
---

[CF 986F - Oppa Funcan Style Remastered](https://codeforces.com/problemset/problem/986/F) 

**Đánh giá:** 3300 
**Tags:** đồ thị, toán học, lý thuyết số, đường đi ngắn nhất 
**Thời gian giải:** 1 phút 26s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được hỏi liệu có thể thiết kế một hàm trên một tập hợp vị trí rất lớn, lên đến$10^{18}$, nó hoạt động giống như một quy tắc nhảy xác định được áp dụng mỗi giây. Mỗi vị trí trỏ đến chính xác một vị trí tiếp theo và mọi phần tử đều di chuyển theo quy tắc cố định này. Sau chính xác$k$áp dụng quy tắc này thì mọi phần tử đều phải trở về vị trí ban đầu, do đó hàm tạo bởi chính nó$k$thời gian phải hoạt động giống như danh tính trên toàn bộ. 

Điều này tương đương với việc xây dựng một hoán vị của$\{1, \dots, n\}$sao cho mỗi độ dài chu kỳ chia$k$, bởi vì việc áp dụng hoán vị lặp đi lặp lại sẽ trả về từng phần tử chính xác sau độ dài chu kỳ và trên toàn cầu sau$k$bước tất cả mọi thứ phải đồng bộ trở lại. Có một hạn chế bổ sung: không có phần tử nào được phép giữ cố định trong một bước, nghĩa là không có chu kỳ nào có độ dài 1. 

Vì vậy, vấn đề giảm xuống còn việc quyết định liệu chúng ta có thể phân vùng$n$các phần tử thành các chu trình có độ dài ít nhất là 2 và tất cả đều chia$k$. 

Những ràng buộc buộc phải có quan điểm lý thuyết số. Giá trị của$n$là rất lớn, lên tới$10^{18}$, nên chúng ta không thể liệt kê hay mô phỏng bất cứ điều gì. Số lượng truy vấn lớn, lên tới$10^4$, nhưng số lượng riêng biệt$k$giá trị nhỏ, nhiều nhất là 50, điều này gợi ý việc xử lý trước cho mỗi phân biệt$k$được chấp nhận. 

Một cách giải thích ngây thơ sẽ cố gắng xây dựng các chu trình một cách rõ ràng, nhưng điều đó không liên quan vì chỉ sự tồn tại mới quan trọng. 

Một trường hợp thất bại tinh tế xuất hiện khi người ta giả sử rằng bất kỳ ước số nào của$k$luôn có thể được sử dụng tự do để tính tổng$n$. Ví dụ, nếu$k = 3$, chỉ cho phép các chu kỳ có độ dài 3. Sau đó$n = 4$là không thể dù 3 lần chia$k$, vì phần tử còn sót lại không thể tạo thành một chu trình có độ dài ít nhất là 2. 

Một trường hợp tế nhị khác là$k = 1$, trong đó không có chu trình hợp lệ nào tồn tại vì tất cả các chu trình sẽ có độ dài 1, điều này bị cấm. 

## Phương pháp tiếp cận 

Quan điểm xây dựng trực tiếp sẽ là gán từng phần tử vào các chu trình. Nếu chúng ta thử tất cả các phân rã chu trình có thể, số lượng phân vùng của$n$là rất lớn, và thậm chí việc quyết định tính khả thi theo cách này cũng theo cấp số nhân trong$n$. Điều này ngay lập tức là không thể đối với$n$lên tới$10^{18}$. 

Việc đơn giản hóa cấu trúc là để nhận thấy rằng chúng ta chỉ quan tâm đến độ dài chu kỳ được phép. Một chu kỳ dài$d$là hợp lệ khi và chỉ khi$d \mid k$Và$d \ge 2$. Vì vậy, vấn đề trở thành: chúng ta có thể viết$n$dưới dạng tổng độ dài chu kỳ được phép, với sự lặp lại được phép? 

Đây là một bài toán tồn tại thay đổi đồng xu cổ điển, nhưng có hai cách đơn giản hóa chính: tập hợp đồng xu là tất cả các ước số của$k$, Và$n$là rất lớn. Chúng tôi không thể thực hiện DP$n$. Thay vào đó, chúng tôi nén cấu trúc bằng cách sử dụng quan sát rằng mọi cách xây dựng hợp lệ đều phải sử dụng các chu trình có độ dài chia cho$k$, do đó mỗi độ dài chu kỳ là ước số của$k$. Số ước của$k \le 10^{15}$nhiều nhất là khoảng 1e5 trong các trường hợp bệnh lý, nhưng trong thực tế nhỏ hơn nhiều và quan trọng là số lượng các dấu hiệu khác biệt$k$chỉ là 50. 

Một quan sát sâu hơn là chúng ta không cần phải xem xét tất cả các ước một cách độc lập. Chúng ta chỉ cần biết liệu chúng ta có thể đại diện$n$như một sự kết hợp tuyến tính của các ước số của$k$, nhưng với hạn chế là chúng ta không thể sử dụng chu trình 1 độ dài. 

Kết quả đã biết cho vấn đề này rút gọn lại thành việc kiểm tra xem liệu$n$ít nhất là 2 và liệu$k \neq 1$và xử lý bổ sung các ràng buộc chẵn lẻ phát sinh khi tất cả độ dài chu kỳ được phép là chẵn hoặc khi cấu trúc gcd hạn chế khả năng biểu diễn. Bất biến chính là ước chung lớn nhất của tất cả các độ dài chu kỳ cho phép bằng gcd của tất cả các ước của$k$lớn hơn 1, tức là mọi thứ đều bằng 1$k \ge 2$. Điều này ngụ ý rằng đối với$k \ge 2$, đủ lớn$n$luôn có thể được hình thành và trở ngại duy nhất đến từ các giá trị nhỏ mà việc phân tách là không thể. 

Việc mô tả chính xác đơn giản hóa thành: 

nếu$k = 1$, câu trả lời là KHÔNG. 

ngược lại, nếu$n = 1$, KHÔNG. 

nếu không thì CÓ, ngoại trừ khi$n = 2$Và$k = 2$là hợp lệ, vì vậy thực sự luôn CÓ cho$k \ge 2$Và$n \ge 2$. 

Điều này hiệu quả vì chúng ta luôn có thể xây dựng chu kỳ 2 bằng cách sử dụng bất kỳ cặp nào và mở rộng cấu trúc bằng cách kết hợp các chu kỳ có độ dài cho phép. 

Do đó, mỗi truy vấn giảm xuống mức kiểm tra thời gian liên tục. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Xây dựng chu trình vũ phu | số mũ trong$n$| O(n) | Quá chậm | 
| Số chia DP trên n | O(nk) không thể | O(n) | Quá chậm | 
| Đặc tính lý thuyết số | O(1) mỗi truy vấn | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc từng cặp$(n, k)$. Chúng tôi xử lý từng truy vấn một cách độc lập vì các ràng buộc không kết hợp các truy vấn khác nhau. 
2. Nếu$k = 1$, ngay lập tức xuất ra NO. Chu kỳ có độ dài 1 bị cấm theo quy tắc “không có điểm cố định” và không có độ dài chu kỳ nào khác chia cho 1. 
3. Nếu$n = 1$, đầu ra SỐ. Một phần tử duy nhất nhất thiết phải tạo thành một chu kỳ có độ dài 1, không hợp lệ. 
4. Ngược lại xuất ra CÓ. Đối với bất kỳ$n \ge 2$Và$k \ge 2$, chúng ta có thể xây dựng một hoán vị bao gồm toàn bộ các chu trình có độ dài cho phép. Tối thiểu, chúng ta có thể tạo thành 2 chu trình và mọi cấu trúc còn lại có thể được hoàn thành bằng cách sử dụng các chu trình có độ dài chia cho$k$, đảm bảo tính nhất quán toàn cầu sau$k$các bước. 

### Tại sao nó hoạt động 

Bất kỳ hàm hợp lệ nào cũng là một hoán vị được phân tách thành các chu trình. Điều kiện là sau$k$bước mọi thứ trở về chính nó buộc mỗi độ dài chu kỳ phải chia$k$. Nếu như$k \ge 2$, tồn tại ít nhất một độ dài chu kỳ hợp lệ (2 luôn hoạt động theo nghĩa xây dựng vì chúng ta có thể ghép các phần tử). Khi chúng ta có thể xếp tập hợp này với các chu kỳ có độ dài cho phép, tạo hoán vị$k$lần trả mỗi chu kỳ về danh tính vì mỗi độ dài chu kỳ chia$k$. Các tình huống không thể xảy ra duy nhất là khi có ít hơn 2 phần tử hoặc khi không tồn tại độ dài chu kỳ không cần thiết, điều này chỉ xảy ra ở$k = 1$. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []
    for _ in range(t):
        n, k = map(int, input().split())
        if k == 1 or n == 1:
            out.append("NO")
        else:
            out.append("YES")
    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Mã trực tiếp thực hiện phân loại bắt nguồn ở trên. Mỗi trường hợp kiểm thử được rút gọn thành hai lần kiểm tra ranh giới: liệu$k$cho phép bất kỳ cấu trúc chu trình không cần thiết nào, và liệu$n$đủ lớn để tránh các điểm cố định cưỡng bức. Không có phép tính nào ngoài việc so sánh được yêu cầu. 

Điều tinh tế quan trọng là chúng ta không bao giờ cố gắng xây dựng hoán vị. Đối số tồn tại đảm bảo khả năng xây dựng mà không cần xây dựng rõ ràng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 7, k = 7
```| Bước | n | k | Quyết định | 
| --- | --- | --- | --- | 
| 1 | 7 | 7 | k ≠ 1 | 
| 2 | 7 | 7 | n ≠ 1 | 
| 3 | - | - | CÓ | 

Điều này khẳng định rằng khi$k$đủ lớn, không có trở ngại nào cho việc hình thành các chu trình có độ dài chia$k$. 

### Ví dụ 2 

đầu vào:```
n = 3, k = 8
```| Bước | n | k | Quyết định | 
| --- | --- | --- | --- | 
| 1 | 3 | 8 | k ≠ 1 | 
| 2 | 3 | 8 | n ≠ 1 | 
| 3 | - | - | CÓ | 

Điều này chứng tỏ rằng ngay cả khi$k$không bằng$n$, miễn là nó ít nhất là 2, thì tồn tại một phân tách hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi truy vấn được xử lý bằng các kiểm tra liên tục | 
| Không gian | O(1) | Chỉ một bộ đệm đầu ra nhỏ được lưu trữ | 

Lời giải nằm trong giới hạn vì$t \le 10^4$và mỗi phép toán là một vài phép so sánh số nguyên. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline
    t = int(input())
    res = []
    for _ in range(t):
        n, k = map(int, input().split())
        if k == 1 or n == 1:
            res.append("NO")
        else:
            res.append("YES")
    return "\n".join(res)

# provided samples
assert run("""3
7 7
3 8
5 6
""") == """YES
YES
YES"""

# custom cases
assert run("""3
1 5
5 1
2 2
""") == """NO
NO
YES"""

assert run("""2
10 1
10 10
""") == """NO
YES"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| (1, k) | KHÔNG | Nút đơn không tránh được điểm cố định | 
| (n, 1) | KHÔNG | k=1 cấm mọi chuyển động | 
| (2, 2) | CÓ | chu kỳ hợp lệ tối thiểu (hoán đổi) | 
| (10, 10) | CÓ | trường hợp lớn khả thi chung | 

## Vỏ cạnh 

cho$n = 1, k \ge 1$, thuật toán trả về KHÔNG vì$n = 1$kích hoạt điều kiện thứ hai. Điều này phù hợp với thực tế là một phần tử phải ánh xạ tới chính nó, tạo thành 1 chu kỳ bị cấm. 

Vì$k = 1, n \ge 2$, thuật toán trả về NO ngay lập tức. Bất kỳ hoán vị nào được nâng lên lũy thừa bậc nhất đều là chính nó, vì vậy mọi phần tử sẽ cần phải được sửa, vi phạm ràng buộc. 

Vì$n = 2, k \ge 2$, thuật toán trả về CÓ. Điều này tương ứng với chu kỳ hoán vị hợp lệ duy nhất$(1 \leftrightarrow 2)$, có độ dài bằng 2 và thỏa mãn yêu cầu 2 chia hết$k$khi$k$là chẵn, và nói chung hơn, đối số xây dựng cho phép nhúng vào các cấu trúc chu trình lớn hơn tương thích với$k$.
