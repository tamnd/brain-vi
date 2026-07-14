---
title: "CF 103347C - Vườn Juliet"
description: "Chúng ta có một khu vườn hình tròn với $n$ bồn hoa được dán nhãn từ $1$ đến $n$, trong đó sau $n$ chúng ta sẽ quay trở lại $1$. Juliet bắt đầu trên giường $1$. Thời gian diễn ra theo từng phút rời rạc và trong phút $i$, kích thước bước chuyển động của cô ấy chính xác là $i$, tăng lên mỗi phút."
date: "2026-07-03T13:44:04+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103347
codeforces_index: "C"
codeforces_contest_name: "UTPC Contest 10-15-21 Div. 2 (Beginner)"
rating: 0
weight: 103347
solve_time_s: 50
verified: true
draft: false
---

[CF 103347C - Khu vườn của Juliet](https://codeforces.com/problemset/problem/103347/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một khu vườn hình tròn với$n$bồn hoa được dán nhãn từ$1$ĐẾN$n$, sau đó ở đâu$n$chúng tôi quay trở lại$1$. Juliet bắt đầu trên giường$1$. Thời gian diễn ra theo từng phút rời rạc, và trong từng phút$i$, kích thước bước chuyển động của cô ấy chính xác là$i$, tăng lên từng phút. 

Chính xác hơn, nếu cô ấy ở vị trí$c$vào đầu phút$i$, cô ấy bước về phía trước$i$bước theo chiều kim đồng hồ dọc theo vòng tròn và kết thúc phút đó ở vị trí$c + i \pmod n$. Sau khi dừng lại ở đó một thời gian ngắn, cô ấy tiếp tục từ vị trí đó trong phút tiếp theo. 

Câu hỏi đặt ra là liệu trong một khoảng thời gian vô hạn, Juliet có dừng lại chính xác trên mỗi luống hoa ít nhất một lần hay không. Nói cách khác, chúng ta muốn biết liệu tập hợp các vị trí dừng đã ghé thăm cuối cùng có bao gồm tất cả các phần dư theo modulo hay không.$n$. 

Quan sát quan trọng là quá trình này mang tính quyết định và thuần túy số học. Sau đó$k$phút, vị trí của cô ấy là tổng của phút đầu tiên$k$modulo số tự nhiên$n$, là một số tam giác:$$S_k = 1 + 2 + \dots + k = \frac{k(k+1)}{2}$$Vì vậy, vấn đề giảm xuống còn việc hỏi liệu chuỗi$S_k \bmod n$cuối cùng đạt tới mọi modulo lớp dư lượng$n$. 

Ràng buộc$n \le 10^4$ngay lập tức loại trừ mọi mô phỏng qua nhiều bước kết hợp với việc kiểm tra tất cả các trạng thái được truy cập một cách đơn giản trong phạm vi lớn, vì chúng ta có thể cần phải suy luận về tối đa$O(n)$hoặc nhiều dư lượng riêng biệt hơn và quá trình này là vô hạn. Mô phỏng trực tiếp cũng gây hiểu nhầm vì chuỗi tăng theo bậc hai, nên suy nghĩ ngây thơ về tính tuần hoàn không có cấu trúc có thể thất bại. 

Trường hợp cạnh tinh tế xuất hiện khi nhỏ$n$hành xử khác với những cái lớn hơn. Ví dụ,$n = 2$xen kẽ một cách tầm thường giữa hai dư lượng, nhưng$n = 3$không thể che phủ hết các chất cặn mặc dù có chuyển động không tầm thường. Điều này gợi ý một cấu trúc lý thuyết số ẩn hơn là một bài toán truyền tải. 

## Phương pháp tiếp cận 

Một cách diễn giải thô bạo sẽ mô phỏng vị trí của Juliet trong một số lượng lớn các bước, lưu trữ các phần dư đã truy cập trong một tập hợp cho đến khi tất cả$n$được nhìn thấy hoặc một mô hình lặp lại được phát hiện. Mỗi bước là$O(1)$, nhưng số bước yêu cầu không bị giới hạn bởi$n$, vì dãy modulo$n$phụ thuộc vào số tam giác và có thể lặp lại với chu kỳ dài. Trong trường hợp xấu nhất, chúng ta có thể mô phỏng vượt xa$O(n^2)$các bước trước khi phát hiện cấu trúc, cấu trúc này không đáng tin cậy trong giới hạn 1 giây. 

Cái nhìn sâu sắc quan trọng là ngừng suy nghĩ về "chuyển động trên một vòng tròn" và thay vào đó coi các vị trí đã ghé thăm là giá trị của modulo đa thức bậc hai$n$. Trình tự$S_k = k(k+1)/2$được điều chỉnh bởi số học mô-đun và tập hợp các dư lượng có thể tiếp cận được xác định bằng cách xem đa thức bậc hai này có phải là mô-đun tính từ hay không$n$. 

Vấn đề sau đó sẽ trở thành một cấu trúc cổ điển: hành vi chỉ phụ thuộc vào việc liệu$n$là sức mạnh của hai. Khi$n$có thừa số nguyên tố lẻ, cấu trúc thặng dư bậc hai sẽ bị thu gọn và một số thặng dư trở nên không thể tiếp cận được. Khi$n$là lũy thừa của hai, cấu trúc nhân đôi theo từng phần đảm bảo bao phủ toàn bộ dư lượng theo thời gian. 

Vì vậy, thay vì mô phỏng, chúng tôi tính hệ số$n$và kiểm tra xem nó có chứa yếu tố lẻ nào lớn hơn$1$. Nếu có thì câu trả lời là KHÔNG. Ngược lại, câu trả lời là CÓ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(T) với T không giới hạn, có khả năng rất lớn | O(n) | Quá chậm/không đáng tin cậy | 
| Kiểm tra hệ số nguyên tố / lũy thừa hai | O(log n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên$n$, đại diện cho kích thước của khu vườn hình tròn. Mục đích là để phân loại xem tất cả các vị trí có thể được truy cập dưới dạng điểm dừng hay không. 
2. Chia liên tục$n$qua$2$trong khi nó chẵn. Điều này cô lập tất cả các lũy thừa của hai trong quá trình phân tích nhân tử của nó. Bước này tương đương với việc trích xuất cấu trúc 2-adic của mô đun, đây là cấu trúc duy nhất cho phép bao phủ toàn bộ trong quy trình này. 
3. Sau khi loại bỏ tất cả các thừa số của 2, hãy kiểm tra xem giá trị còn lại có phải là$1$. Nếu nó lớn hơn$1$, sau đó$n$có yếu tố kỳ lạ. 
4. Nếu tồn tại hệ số lẻ, ngay lập tức kết luận rằng không thể bao phủ toàn bộ và xuất ra NO. Ngược lại, nếu giá trị giảm là$1$, xuất ra CÓ. 

Lý do điều này có tác dụng là vì dãy số tam giác modulo$n$hoạt động giống như một bản đồ bậc hai trên một vành hữu hạn. Trên các mô-đun có thừa số lẻ, bản đồ này thu gọn thành một tập con bị hạn chế vì nhân với$1/2$được xác định rõ ràng và giới thiệu cấu trúc ngăn chặn sự bao phủ thống nhất. Đối với quyền hạn của hai, việc thiếu tính không thể đảo ngược của$2$tạo ra sự pha trộn đủ dư lượng, cho phép trình tự cuối cùng đạt đến mọi lớp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input().strip())
    
    # remove all factors of 2
    while n % 2 == 0:
        n //= 2
    
    # if anything other than 1 remains, there is an odd factor
    if n == 1:
        print("YES")
    else:
        print("NO")

if __name__ == "__main__":
    solve()
```Việc triển khai phản ánh trực tiếp logic phân tích nhân tử. Việc loại bỏ các hệ số vòng lặp của hai là an toàn vì$n \le 10^4$, vậy nhiều nhất$O(\log n)$sự lặp lại xảy ra. Kiểm tra cuối cùng để phân biệt lũy thừa thuần túy của hai với tất cả các số nguyên khác. 

Một lỗi phổ biến là cố gắng mô phỏng trực tiếp chuyển động của Juliet. Cách tiếp cận đó không thành công vì vị trí tăng theo phương trình bậc hai và các chu trình không rõ ràng trong các cửa sổ mô phỏng nhỏ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2
```| Bước | n | Hành động | Còn lại n | 
| --- | --- | --- | --- | 
| 1 | 2 | chia cho 2 | 1 | 

Sau khi giảm,$n = 1$, vì vậy đầu ra là CÓ. 

Điều này xác nhận rằng với hai luống hoa, cấu trúc xen kẽ được tạo ra bởi các tổng tam giác cuối cùng sẽ chạm tới cả hai phần dư. 

### Ví dụ 2 

đầu vào:```
3
```| Bước | n | Hành động | Còn lại n | 
| --- | --- | --- | --- | 
| 1 | 3 | không thể chia được | 3 | 

Vì giá trị còn lại không$1$, đầu ra là KHÔNG. 

Điều này cho thấy rằng việc giới thiệu một mô đun lẻ sẽ phá vỡ phạm vi bao phủ đầy đủ và một số lớp dư lượng không bao giờ đạt được bằng cấp số tam giác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log n) | Mỗi lần lặp sẽ loại bỏ hệ số 2, do đó ở hầu hết các bước logarit trong$n$| 
| Không gian | O(1) | Chỉ có một số biến số nguyên được sử dụng | 

Giải pháp dễ dàng phù hợp với các ràng buộc vì$n \le 10^4$, làm cho vòng lặp cực kỳ nhanh ngay cả trong giới hạn chặt chẽ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import builtins

    output = io.StringIO()
    old_stdout = sys.stdout
    sys.stdout = output

    try:
        solve()
    finally:
        sys.stdout = old_stdout

    return output.getvalue().strip()

# provided samples
assert run("2\n") == "YES"
assert run("3\n") == "NO"

# custom cases
assert run("1\n") == "YES", "single node is trivially covered"
assert run("4\n") == "YES", "power of two should pass"
assert run("6\n") == "NO", "has odd factor"
assert run("8\n") == "YES", "pure power of two"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | CÓ | trường hợp tối thiểu | 
| 4 | CÓ | sức mạnh nhỏ của hai | 
| 6 | KHÔNG | yếu tố hỗn hợp | 
| 8 | CÓ | sức mạnh lớn hơn của hai | 

## Vỏ cạnh 

cho$n = 1$, thuật toán ngay lập tức đưa ra CÓ vì chỉ có một luống hoa nên điều kiện được thỏa mãn một cách tầm thường. 

Vì$n = 2$, việc giảm một nửa lặp đi lặp lại làm giảm nó thành$1$, do đó, nó xuất ra CÓ một cách chính xác, khớp với các tổng tam giác xen kẽ bao gồm cả hai phần dư. 

Vì$n = 3$, không thể loại bỏ hệ số hai, do đó thuật toán đưa ra NO ngay lập tức, nắm bắt chính xác các thặng dư tam giác modulo 3 không bao gồm tất cả các lớp. 

Vì$n = 8$, năng suất phép chia lặp đi lặp lại$1$, xác nhận phạm vi bảo hiểm đầy đủ theo quyền hạn của hai. Thuật toán xác định chính xác điều này mà không cần mô phỏng.
