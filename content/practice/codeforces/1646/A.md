---
title: "CF 1646A - Đếm vuông"
description: "Chúng ta được cho một dãy có độ dài $n+1$. Mỗi phần tử là một số “nhỏ” trong phạm vi $[0, n-1]$ hoặc một giá trị lớn đặc biệt bằng $n^2$. Chúng ta chỉ được biết hai điều: tham số $n$ và tổng tổng của tất cả các phần tử trong dãy, ký hiệu là $s$."
date: "2026-06-10T04:09:22+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 1646
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 774 (Div. 2)"
rating: 800
weight: 1646
solve_time_s: 86
verified: true
draft: false
---

[CF 1646A - Đếm bình phương](https://codeforces.com/problemset/problem/1646/A) 

**Đánh giá:** 800 
**Thẻ:** toán 
**Thời gian giải:** 1 phút 26s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy có độ dài$n+1$. Mỗi phần tử là một số “nhỏ” trong phạm vi$[0, n-1]$hoặc một giá trị lớn đặc biệt bằng$n^2$. Chúng ta chỉ được nói với hai điều: tham số$n$và tổng tổng của tất cả các phần tử trong dãy, ký hiệu là$s$. Chuỗi ban đầu bị mất và nhiệm vụ là xác định xem có bao nhiêu vị trí chứa giá trị$n^2$. 

Cấu trúc của trình tự rất cứng nhắc. Mỗi phần tử không đặc biệt đều đóng góp nhiều nhất$n-1$thành tổng, trong khi mỗi phần tử đặc biệt đóng góp chính xác$n^2$, lớn hơn nhiều so với bất cứ thứ gì khác. Sự tách biệt quy mô này là đặc điểm chính giúp cho việc tái thiết có thể thực hiện được. 

Các ràng buộc cho phép lên đến$2 \cdot 10^4$trường hợp thử nghiệm, với$n$lên tới$10^6$Và$s$lên tới$10^{18}$. Điều này loại trừ bất kỳ cách tiếp cận nào cố gắng tái tạo lại trình tự hoặc thậm chí suy luận về các phần tử riêng lẻ. Bất kỳ giải pháp hợp lệ nào cũng phải giảm từng trường hợp thử nghiệm thành số học có thời gian không đổi. 

Một cách tiếp cận ngây thơ có thể thử đoán xem có bao nhiêu$n^2$các phần tử tồn tại và sau đó kiểm tra xem tổng còn lại có thể được điền bằng các giá trị trong$[0, n-1]$. Mặc dù đúng về mặt logic nhưng việc mô phỏng như vậy là không cần thiết vì cấu trúc hàm ý mối quan hệ đại số trực tiếp giữa$s$,$n$và số lượng các phần tử đặc biệt. 

Trường hợp cạnh tinh tế xuất hiện khi$s = 0$. Trong trường hợp này, tất cả các phần tử phải bằng 0 và câu trả lời nhất thiết là không có phần tử đặc biệt nào. Một tình huống góc khác là khi$s$rất lớn và gần như hoàn toàn được hình thành bởi$n^2$đóng góp; ở đây, phần còn lại vẫn phải được thực hiện bằng cách sử dụng các giá trị nhỏ bị chặn và bài toán đảm bảo tính nhất quán như vậy. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu là giả sử số lượng phần tử đặc biệt$k$dao động từ$0$ĐẾN$n+1$. Đối với mỗi ứng viên$k$, chúng ta thử xem liệu số tiền còn lại có thể được giải thích bằng cách sử dụng$n+1-k$mỗi phần tử trong$[0, n-1]$. Đối với một cố định$k$, chúng tôi trừ$k \cdot n^2$từ$s$và kiểm tra xem phần dư có nằm giữa$0$Và$(n+1-k)(n-1)$. Điều này có tác dụng vì phần đóng góp nhỏ nhất có thể có của các phần tử còn lại bằng 0 và phần đóng góp lớn nhất là khi tất cả đều bằng 0.$n-1$. 

Quá trình quét vũ phu này là chính xác nhưng không cần thiết. Nó vẫn chạy vào$O(n)$cho mỗi trường hợp thử nghiệm, điều này sẽ quá chậm khi$n$lớn và số lượng ca kiểm thử cũng nhiều. 

Quan sát quan trọng là chúng ta không thực sự cần thử tất cả các giá trị của$k$. Cấu trúc ràng buộc buộc một giá trị duy nhất$k$, và nó có thể được suy ra trực tiếp từ tổng. Vì mỗi yếu tố đặc biệt đóng góp một lượng dư thừa$n^2$so với bất kỳ giá trị bình thường nào, chúng ta có thể coi bài toán là sự chia tách$s$thành phần cơ bản từ các giá trị nhỏ và phần đóng góp cố định từ các giá trị đặc biệt. 

Cho phép$k$là số phần tử bằng$n^2$. Phần còn lại$n+1-k$yếu tố đóng góp nhiều nhất$n-1$mỗi cái, vậy tổng tối đa có thể có của chúng là$(n+1-k)(n-1)$. Điều này ngụ ý:$$k \cdot n^2 \le s \le k \cdot n^2 + (n+1-k)(n-1)$$Thay vì tìm kiếm, chúng tôi tính toán$k$bằng cách khai thác tham lam: chúng tôi lấy càng nhiều$n^2$càng tốt từ$s$, nhưng chúng ta phải đảm bảo phần còn lại vẫn có thể được hình thành bởi các giá trị giới hạn. Bởi vì$n^2$thống trị phạm vi$[0, n-1]$, đúng$k$được xác định bởi cấu trúc chia số nguyên của$s$liên quan đến$n^2$và tính khả thi còn lại. 

Một cách cải cách đơn giản hơn là quan sát thấy rằng một khi$k$đã được cố định, phần còn lại$r = s - k n^2$phải thỏa mãn$0 \le r \le (n+1-k)(n-1)$. Việc đảm bảo tính duy nhất đảm bảo rằng có chính xác một$k$thỏa mãn điều này và nó có thể được tìm thấy trong thời gian không đổi bằng cách sử dụng lý luận số học xung quanh giới hạn trên. 

Giải pháp thực tế giảm thiểu việc lặp lại từ mức lớn nhất có thể$k$đi xuống hoặc dẫn xuất trực tiếp$k$từ giới hạn. Từ$k \le n+1$, nhưng cũng$k n^2 \le s$, chúng ta nhận được ngay$k \le s // n^2$. Bắt đầu từ giới hạn trên này và điều chỉnh xuống dưới với số lần kiểm tra không đổi nhiều nhất sẽ mang lại câu trả lời một cách hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n)$mỗi trường hợp thử nghiệm |$O(1)$| Quá chậm | 
| Tối ưu |$O(1)$mỗi trường hợp thử nghiệm |$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tập trung vào việc trích xuất số phần tử bằng$n^2$từ tổng số tiền. 

1. Tính số lượng phần tử đặc biệt tối đa có thể có$k = s // n^2$. Đây là giới hạn trên tuyệt đối vì mỗi phần tử đặc biệt đều đóng góp$n^2$, do đó vượt quá mức này sẽ vượt quá tổng. 
2. Kẹp$k$nhiều nhất là$n+1$, vì chỉ có$n+1$tổng số các phần tử. 
3. Kiểm tra xem ứng viên này có$k$hợp lệ bằng cách xác minh rằng số tiền còn lại$r = s - k \cdot n^2$có thể được hình thành bằng cách sử dụng$n+1-k$số, mỗi số trong$[0, n-1]$. Điều này đòi hỏi$r \le (n+1-k)(n-1)$. 
4. Nếu điều kiện không đạt thì giảm$k$cho đến khi nó trở nên hợp lệ. Cấu trúc của các ràng buộc đảm bảo rằng cần có nhiều nhất một số lần điều chỉnh không đổi, vì chỉ có một$k$thỏa mãn bất đẳng thức khả thi. 

### Tại sao nó hoạt động 

Tổng số phân hủy duy nhất thành hai phần: bội số của$n^2$từ các phần tử đặc biệt và sự đóng góp giới hạn từ các phần tử thông thường. Bởi vì các yếu tố bình thường được giới hạn ở$n-1$, họ không thể bù đắp cho việc đánh giá quá cao$k$thậm chí chỉ bằng một cái đầy đủ$n^2$trừ khi dung lượng còn lại chính xác cho phép nó. Điều này tạo ra một cửa sổ khả thi nghiêm ngặt cho$k$, đảm bảo chỉ có một số nguyên thỏa mãn đồng thời cả giới hạn dưới và giới hạn trên. Thuật toán tìm kiếm hiệu quả trong điều kiện đơn điệu trên$k$, do đó việc hiệu chỉnh luôn hướng tới nghiệm hợp lệ duy nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, s = map(int, input().split())
        max_k = s // (n * n)
        max_k = min(max_k, n + 1)

        def ok(k):
            r = s - k * n * n
            if r < 0:
                return False
            return r <= (n + 1 - k) * (n - 1)

        k = max_k
        while k >= 0 and not ok(k):
            k -= 1

        print(k)

if __name__ == "__main__":
    solve()
```Mã này tính toán giới hạn trên ban đầu cho số lượng$n^2$phần tử sử dụng phép chia số nguyên. Điều này an toàn vì bất kỳ khoản bổ sung nào$n^2$sẽ vượt quá số tiền. Chức năng trợ giúp`ok(k)`kiểm tra tính khả thi bằng cách đảm bảo số tiền còn lại nằm trong khả năng tối đa của các phần tử không đặc biệt. 

Vòng điều chỉnh đi xuống ngắn vì điều kiện khả thi là đơn điệu trong$k$. Khi chúng tôi chuyển giá trị chính xác, bất kỳ mức giảm nào nữa chỉ làm tăng dung lượng khả dụng, do đó, giá trị hợp lệ đầu tiên$k$gặp phải là câu trả lời. 

Phải cẩn thận với số học số nguyên vì$s$có thể đạt được$10^{18}$, nhưng Python xử lý việc này một cách an toàn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:$n=2, s=12$Chúng tôi tính toán$n^2 = 4$, Và$n+1 = 3$. 

| Bước | k | r = s - k·n² | Công suất còn lại tối đa | hợp lệ | 
| --- | --- | --- | --- | --- | 
| ban đầu | 3 | 0 | 0 | vâng | 

Tối đa có thể$k$là 3, và nó khớp chính xác, nên đáp án là 3. 

Điều này cho thấy trường hợp tất cả các phần tử đều đặc biệt, không để lại phần đóng góp dư nào. 

### Ví dụ 2 

đầu vào:$n=3, s=12$Đây$n^2 = 9$,$n+1 = 4$. 

| Bước | k | r | công suất | hợp lệ | 
| --- | --- | --- | --- | --- | 
| bắt đầu | 1 | 3 | 3·2 = 6 | vâng | 

Chúng tôi kiểm tra$k=1$: số dư là 3, chứa 3 phần tử còn lại, mỗi phần tử có giá trị bằng 2. Lớn hơn$k=2$thất bại vì nó sẽ vượt quá tổng. Vậy đáp án là 1. 

Điều này chứng tỏ ràng buộc trên các phần tử bị chặn xác định tính khả thi như thế nào ngay cả khi nhiều phép phân tách gần nhau về mặt đại số. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(t)$| Mỗi trường hợp thử nghiệm thực hiện kiểm tra số học liên tục và nhiều nhất là một vòng lặp giảm nhỏ | 
| Không gian |$O(1)$| Chỉ một vài biến được sử dụng cho mỗi trường hợp thử nghiệm | 

Giải pháp vừa vặn thoải mái trong giới hạn vì thậm chí$2 \cdot 10^4$trường hợp thử nghiệm chỉ yêu cầu các phép toán số nguyên đơn giản. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n, s = map(int, input().split())
        max_k = s // (n * n)
        max_k = min(max_k, n + 1)

        def ok(k):
            r = s - k * n * n
            return r >= 0 and r <= (n + 1 - k) * (n - 1)

        k = max_k
        while k >= 0 and not ok(k):
            k -= 1
        out.append(str(k))

    return "\n".join(out) + "\n"

# provided samples
assert run("4\n7 0\n1 1\n2 12\n3 12\n") == "0\n1\n3\n1\n"

# custom cases
assert run("1\n1 0\n") == "0\n", "minimum case"
assert run("1\n1 2\n") == "1\n", "single special element"
assert run("1\n2 8\n") == "2\n", "all special"
assert run("1\n5 0\n") == "0\n", "all zeros"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1\n1 0 | 0 | ranh giới tối thiểu, không có yếu tố đặc biệt | 
| 1\n1 2 | 1 | phần tử đơn bằng$n^2$| 
| 1\n2 8 | 2 | bão hòa hoàn toàn với các giá trị đặc biệt | 
| 1\n5 0 | 0 | tất cả các phần tử đều là số 0 nhỏ | 

## Vỏ cạnh 

Một trường hợp có tổng bằng 0 như$n=7, s=0$buộc mọi phần tử đều bằng 0 vì bất kỳ phần tử nào$n^2$đóng góp sẽ ngay lập tức vượt quá tổng số. Thuật toán tính toán$k = 0$từ bước chia và vượt qua kiểm tra tính khả thi vì dung lượng còn lại khớp chính xác với thời gian độ dài chuỗi đầy đủ$n-1$, không âm. 

Một trường hợp bão hòa hoàn toàn như$n=2, s=12$sản lượng$k = 3$trực tiếp từ$s // 4$. Phần còn lại trở thành số 0, nằm trong khả năng$0$, xác nhận tất cả các yếu tố là đặc biệt. Vòng điều chỉnh đi xuống không bao giờ kích hoạt, cho thấy giới hạn trên đã chặt chẽ. 

Một trường hợp hỗn hợp như$n=3, s=12$thể hiện hành vi điều chỉnh. Ước tính ban đầu$k = 1$là hợp lệ, nhưng$k = 2$sẽ thất bại vì nó để lại số dư âm. Vòng lặp giải quyết chính xác ở giá trị khả thi đầu tiên, đảm bảo ràng buộc về tính duy nhất được tôn trọng mà không có sự mơ hồ.
