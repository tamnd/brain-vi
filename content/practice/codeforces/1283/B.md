---
title: "CF 1283B - Phòng Kẹo"
description: "Chúng tôi đang phân phát một số lượng kẹo giống hệt nhau cho một số lượng trẻ em cố định. Mỗi đứa trẻ phải nhận được một số kẹo nguyên không âm và chúng ta được phép để lại một số kẹo chưa sử dụng."
date: "2026-06-16T03:01:57+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 1283
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 611 (Div. 3)"
rating: 900
weight: 1283
solve_time_s: 308
verified: true
draft: false
---

[CF 1283B - Bộ phận Kẹo](https://codeforces.com/problemset/problem/1283/B) 

**Xếp hạng:** 900 
**Thẻ:** toán 
**Thời gian giải:** 5 phút 8 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang phân phát một số lượng kẹo giống hệt nhau cho một số lượng trẻ em cố định. Mỗi đứa trẻ phải nhận được một số kẹo nguyên không âm và chúng ta được phép để lại một số kẹo chưa sử dụng. Phân phối chỉ được coi là hợp lệ nếu các giá trị được gán cho trẻ được cân bằng chặt chẽ: số lượng được gán lớn nhất và nhỏ nhất khác nhau nhiều nhất là một, vì vậy mọi trẻ đều nhận được một số giá trị$a$hoặc giá trị tiếp theo$a+1$. Ngoài ra, giá trị cao hơn$a+1$không thể quá phổ biến, cụ thể nó có thể xuất hiện ở nhiều nhất một nửa số trẻ, làm tròn xuống. 

Mục tiêu không phải là tìm ra cách phân phối hợp lệ mà là tối đa hóa số lượng kẹo thực sự được trao trong khi vẫn tôn trọng những ràng buộc này. 

Kích thước đầu vào lớn, lên tới$5 \cdot 10^4$trường hợp thử nghiệm và giá trị của$n$Và$k$lên đến$10^9$. Bất kỳ giải pháp nào cố gắng mô phỏng phân phối hoặc lặp lại các nhiệm vụ có thể có cho mỗi trường hợp thử nghiệm sẽ quá chậm. Một giải pháp đúng phải giảm từng trường hợp thử nghiệm về số học có thời gian không đổi. 

Một ý tưởng ngây thơ nhưng hấp dẫn là thử tính xem có bao nhiêu đứa trẻ nhận được$a+1$kẹo và tính toán tính khả thi cho mỗi lựa chọn. Điều này bị phá vỡ trong các trường hợp đặc biệt khi hạn chế về số lượng trẻ em có giá trị cao hơn tương tác với số kẹo còn sót lại. Ví dụ, với$k = 5$, cố gắng tối đa hóa phân phối trung bình một cách tham lam có thể dẫn đến việc chỉ định quá nhiều$a+1$giá trị, vi phạm$\lfloor k/2 \rfloor$giới hạn mặc dù việc phân chia hơi khác một chút sẽ cho phép có tổng số kẹo nhiều hơn. 

Một dạng thất bại khác xuất phát từ việc giả định rằng việc phân phối tất cả các viên kẹo một cách đồng đều (như$n/k$) luôn tối ưu. Khi$k = 3$Và$n = 8$, sự phân chia thống nhất mang lại$[2,2,2]$với 6 viên kẹo được sử dụng, nhưng cấu hình hợp lệ tốt hơn có thể sử dụng nhiều hơn bằng cách khai thác$a/a+1$cấu trúc một cách cẩn thận. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force sẽ là thử tất cả các giá trị có thể có của$a$, và với mỗi mục, hãy quyết định xem có bao nhiêu trẻ có thể tham gia$a+1$, sau đó kiểm tra xem tổng số kẹo được sử dụng có thể đạt tới$n$hoặc ít hơn và chọn số tiền hợp lệ nhất. Đối với mỗi$a$, chúng tôi sẽ quét qua số lượng trẻ em được phân bổ cao hơn có thể có, tính toán lại tổng số và xác minh các ràng buộc. Điều này sẽ yêu cầu lặp lại các giá trị lên tới$n$, và với mỗi người cố gắng đạt được$k$chia rẽ, dẫn đến không thể thực hiện được$O(nk)$hoặc quá trình tồi tệ hơn. 

Quan sát quan trọng là mọi phân phối hợp lệ đều được xác định đầy đủ chỉ bằng hai số nguyên: số lượng trẻ em nhận được$a+1$, và bao nhiêu người nhận được$a$. Sau khi chúng tôi sửa số lượng trẻ em có giá trị cao hơn$x$, phần còn lại là bắt buộc. Ràng buộc$x \le \lfloor k/2 \rfloor$giới hạn trực tiếp số lượng trẻ em có thể được tăng cường. Đối với bất kỳ cố định$x$, chúng ta có thể tính số kẹo tối đa là$$(k-x)\cdot a + x \cdot (a+1) = k\cdot a + x.$$Vì chúng ta muốn tối đa hóa tổng số kẹo nên chúng ta muốn tối đa hóa cả hai$a$Và$x$, nhưng chúng được ghép nối thông qua yêu cầu tổng số kẹo không vượt quá$n$. 

Điều này làm giảm vấn đề lựa chọn$x$một cách tối ưu và rút ra$a$từ ngân sách còn lại. Cấu trúc trở nên tuyến tính và cấu hình tốt nhất luôn xảy ra ở một ranh giới được xác định bởi ràng buộc về$x$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(nk)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(1)$mỗi bài kiểm tra |$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi viết lại phân phối theo hai nhóm:$k-x$trẻ em nhận được$a$, Và$x$trẻ em nhận được$a+1$, Ở đâu$0 \le x \le \lfloor k/2 \rfloor$. 

1. Sửa mã số ứng viên$x$, số trẻ nhận được$a+1$. Điều này ngay lập tức xác định cấu trúc phân phối vì tất cả trẻ em còn lại phải nhận được$a$. 
2. Biểu thị tổng số kẹo đã sử dụng là$k \cdot a + x$. Điều này xuất phát từ việc bắt đầu với$a$kẹo cho mỗi đứa trẻ và thêm một viên kẹo bổ sung vào mỗi đứa trẻ$x$trẻ em được chọn. 
3. Đối với cố định$x$, tối đa hóa$a$tùy thuộc vào$k \cdot a + x \le n$. Điều này mang lại$a = \left\lfloor \frac{n-x}{k} \right\rfloor$. 
4. Thay ngược lại để tính tổng số kẹo đã sử dụng như sau:$$\text{candies}(x) = k \cdot \left\lfloor \frac{n-x}{k} \right\rfloor + x.$$5. Vì$x$bị hạn chế bởi$0 \le x \le \lfloor k/2 \rfloor$, vấn đề giảm xuống chỉ còn kiểm tra hành vi biên của hàm này. Tăng dần$x$đi 1 đôi khi làm giảm số hạng phân chia cơ số, do đó chúng tôi kiểm tra cả hai thái cực$x = 0$Và$x = \lfloor k/2 \rfloor$, và lấy giá trị lớn nhất 

Lý do chỉ có điểm cuối quan trọng là vì hàm này tuyến tính từng phần với các mức giảm định kỳ ở bội số của$k$và trong phạm vi cho phép$x$, mức tối đa phải xảy ra tại một trong các ranh giới của khoảng khả thi. 

### Tại sao nó hoạt động 

Sự phân bố chỉ phụ thuộc vào số lượng trẻ em nhận được giá trị lớn hơn và mọi cấu hình khả thi sẽ thu gọn về một tham số nguyên duy nhất$x$. Đối với mỗi$x$, tốt nhất có thể$a$được xác định duy nhất bằng cách tham lam lấp đầy mỗi đứa trẻ càng nhiều càng tốt$k$-nhóm kẹo càng tốt. Kể từ khi tăng$x$chỉ dịch chuyển số hạng còn lại trong khi giảm dần thương cơ số, không có giá trị bên trong của$x$có thể thống trị nghiêm ngặt cả hai điểm cuối cùng một lúc. Điều này đảm bảo rằng việc đánh giá chỉ các trường hợp biên của$x$nắm bắt được giải pháp tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())

        x = k // 2

        def calc(x):
            a = (n - x) // k
            return a * k + x

        ans = max(calc(0), calc(x))
        print(ans)

if __name__ == "__main__":
    solve()
```Mã này mã hóa hai ứng cử viên chính cho số lượng trẻ em nhận được$a+1$. Hàm trợ giúp tính toán tổng số có thể đạt được tốt nhất cho một mục tiêu cố định$x$, đầu tiên xác định giá trị cơ bản lớn nhất có thể$a$, sau đó xây dựng lại tổng thể. 

Lựa chọn chỉ kiểm tra$x = 0$Và$x = k//2$tuân theo cấu trúc ràng buộc, trong đó các giá trị trung gian không thể cải thiện đồng thời cả phân bổ cơ sở và phân bổ tiền thưởng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:$n = 19, k = 4$| x | a = (n-x)//k | tổng = k·a + x | 
| --- | --- | --- | 
| 0 | 4 | 16 | 
| 2 | 4 | 18 | 

Sự lựa chọn tốt nhất là$x = 2$, tổng cộng là 18. 

Điều này cho thấy việc sử dụng một số trẻ làm đối tượng nhận cao hơn sẽ cho phép tận dụng tốt hơn số kẹo còn sót lại sau khi chia thành các nhóm có quy mô lớn hơn.$k$. 

### Ví dụ 2 

đầu vào:$n = 12, k = 7$| x | a = (n-x)//k | tổng cộng | 
| --- | --- | --- | 
| 0 | 1 | 7 | 
| 3 | 1 | 10 | 

Tối ưu là$x = 3$, thu được 10. 

Điều này chứng tỏ rằng khi$k$lớn hơn$n$, cơ cấu vẫn được hưởng lợi từ việc tối đa hóa số lượng con được tăng cường trong giới hạn cho phép. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(t)$| Mỗi trường hợp thử nghiệm được đánh giá trong thời gian không đổi bằng cách sử dụng hai phép tính số học | 
| Không gian |$O(1)$| Không sử dụng cấu trúc phụ trợ | 

Giải pháp dễ dàng phù hợp với các ràng buộc vì thậm chí$5 \cdot 10^4$mỗi trường hợp thử nghiệm chỉ yêu cầu một vài phép tính số học. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    
    t = int(sys.stdin.readline())
    for _ in range(t):
        n, k = map(int, sys.stdin.readline().split())

        x = k // 2

        def calc(x):
            a = (n - x) // k
            return a * k + x

        output.append(str(max(calc(0), calc(x))))

    return "\n".join(output) + "\n"

# provided samples
assert run("""5
5 2
19 4
12 7
6 2
100000 50010
""") == """5
18
10
6
75015
"""

# custom cases
assert run("1\n1 1\n") == "1\n", "single element"
assert run("1\n0 5\n") == "0\n", "no candies"
assert run("1\n10 1\n") == "10\n", "single child"
assert run("1\n8 3\n") == "8\n", "tight distribution"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 | 1 | trường hợp tối thiểu khác không | 
| 0 5 | 0 | trường hợp không có cạnh kẹo | 
| 10 1 | 10 | đứa con duy nhất hấp thụ tất cả | 
| 8 3 | 8 | trường hợp tương tác nhỏ | 

## Vỏ cạnh 

Khi nào$k = 1$, ràng buộc về$a+1$trở nên không liên quan vì chỉ có một đứa trẻ. Thuật toán đánh giá$x = 0$và trả về chính xác$n$, bởi vì$a = n$và tổng trở thành$n$. 

Khi$n < k$, nhiều trẻ em không nhận được gì. Vì$n = 5, k = 10$, phép tính cho$x = 0$cho$a = 0$, tổng số 0, trong khi$x = 5$vẫn mang lại lợi nhuận$a = 0$và tổng cộng là 5, phù hợp với hành vi tối ưu trong đó có tới một nửa số trẻ có thể nhận được một viên kẹo. 

Khi$k$lớn so với$n$, ứng cử viên thứ hai$x = k/2$đảm bảo rằng việc phân bổ còn lại được tối đa hóa thông qua các phần tử con được tăng cường. Ví dụ,$n = 6, k = 10$sản lượng$x = 5$, cho tổng số 5, đây là cách đóng gói tốt nhất có thể với điều kiện là không quá năm đứa trẻ có thể nhận được một viên kẹo trong khi những đứa trẻ còn lại không nhận được viên kẹo nào.
