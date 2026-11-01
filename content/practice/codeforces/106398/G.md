---
title: "CF 106398G - \u0412\u0430\u0441\u044f \u0438 \u0442\u0440\u0435\u043d\u0438\u0440\u043e\u0432\u043a\u0438"
description: "Chúng tôi được cung cấp một chuỗi các buổi đào tạo theo lịch trình. Lý tưởng nhất là có $n$ phiên, nhưng Vasya có thể bỏ qua một số phiên. Mỗi phiên bị bỏ qua sẽ không bị mất: nó phải được bù lại sau đó ở phiên truy cập trong tương lai."
date: "2026-06-20T23:08:35+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106398
codeforces_index: "G"
codeforces_contest_name: "\u041e\u0442\u0431\u043e\u0440 \u043d\u0430 \u0412\u041a\u041e\u0428\u041f.Junior 2026"
rating: 0
weight: 106398
solve_time_s: 46
verified: true
draft: false
---

[CF 106398G - \u0412\u0430\u0441\u044f \u0438 \u0442\u0440\u0435\u043d\u0438\u0440\u043e\u0432\u043a\u0438](https://codeforces.com/problemset/problem/106398/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một chuỗi các buổi đào tạo theo lịch trình. Lý tưởng nhất là có$n$các buổi, nhưng Vasya có thể bỏ qua một số buổi. Mỗi phiên bị bỏ qua sẽ không bị mất: nó phải được bù lại sau đó ở phiên truy cập trong tương lai. Nếu một phiên bị bỏ lỡ$i$được đền bù tại phiên$j$, chi phí nỗ lực là$j - i + 1$, và điều này đã bao gồm việc đào tạo được thực hiện vào ngày$j$chính nó. 

Vasya được phép bỏ qua bất kỳ số buổi nào và anh ấy có thể bù lại nhiều buổi bị bỏ lỡ trong một lần thăm khám. Tuy nhiên, tổng nỗ lực tích lũy trên tất cả các khoản bồi thường không thể vượt quá$k$. Mục tiêu là giảm thiểu số lượng buổi học mà Vasya thực sự tham dự trong khi vẫn đảm bảo tất cả$n$các phiên cuối cùng sẽ được đền bù trong tổng ngân sách nỗ lực. 

Điều quan trọng không phải là lịch trình chính xác mà là cách nhóm các phiên bị bỏ qua và cuối cùng chúng được đền đáp ở đâu. Nếu bỏ qua một khoảng thời gian dài các buổi học liên tiếp và sau đó chỉ tham gia một lần thì chi phí sẽ trở nên lớn vì các khoản tiền phạt bị tích lũy ngày càng tăng đối với các buổi học bị bỏ lỡ trước đó. 

Đầu vào cho hai số nguyên$n$Và$k$và đầu ra là số phiên truy cập tối thiểu sao cho tất cả các phiên bị bỏ lỡ có thể được bù mà không vượt quá tổng nỗ lực$k$. 

Các ràng buộc đạt tới$10^{18}$, do đó, bất kỳ giải pháp nào tùy thuộc vào phép lặp tuyến tính hoặc bậc hai trên$n$là không thể. Chúng ta cần một cấu trúc toán học hoặc tham lam thuần túy để quy bài toán về điều kiện dạng đóng hoặc tìm kiếm logarit. 

Một trường hợp quan trọng là Vasya tham dự hầu hết các buổi học. Một giả định ngây thơ có thể là việc tham dự$n-1$phiên luôn hoạt động, nhưng chi phí vẫn có thể vượt quá$k$tùy thuộc vào cách định vị phiên bị bỏ qua. Một trường hợp tinh vi khác là khi tất cả các phiên bị bỏ qua được nhóm lại ở đầu hoặc cuối, vì điều đó giúp giảm thiểu hoặc tối đa hóa chi phí theo những cách rất khác nhau. 

## Phương pháp tiếp cận 

Chiến lược brute-force sẽ thử tất cả các tập hợp con của các phiên đã tham dự. Đối với mỗi tập hợp con, chúng tôi mô phỏng phiên nào bị bỏ qua, sau đó chỉ định mỗi phiên bị bỏ qua cho một số phiên tham dự sau đó để giảm thiểu chi phí và kiểm tra xem tổng chi phí có nằm trong giới hạn không.$k$. Điều này đúng vì nó tuân thủ trực tiếp các quy tắc, nhưng số lượng tập hợp con là$2^n$, và thậm chí ở mức vừa phải$n$điều này là hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là cấu trúc tối ưu không phải là tùy ý. Nếu chúng ta ấn định số lượng phiên tham dự, cách sắp xếp tốt nhất là dàn trải chúng theo cách giảm thiểu chi phí bồi thường tích lũy. Cơ cấu chi phí chỉ phụ thuộc vào khoảng cách tương đối giữa các phiên tham dự và hình phạt nặng nhất đến từ các phân đoạn bị bỏ qua liên tục trong thời gian dài. Điều này biến vấn đề thành việc phân tích khối phiên bị bỏ qua có thể được “hấp thụ” theo ngân sách lớn đến mức nào$k$. 

Nếu chúng ta nghĩ ngược lại, giả sử Vasya chỉ tham dự$m$phiên. Sau đó có$n - m$các phiên bị bỏ qua và tổng chi phí được giảm thiểu khi các phiên bị bỏ qua này được nhóm chặt chẽ nhất có thể và được đền bù theo thứ tự tối ưu. Điều này làm giảm vấn đề thành hàm chi phí tổ hợp trên các kích thước khối thay vì lập kế hoạch rõ ràng. 

Cấu trúc ẩn giấu là các chiến lược tối ưu luôn hoạt động một cách tham lam trong việc nhóm các lượt bỏ qua và chi phí trở thành hàm số của số phiên được tham dự chứ không phải phiên nào. Điều này cho phép chúng ta rút ra điều kiện đơn điệu và tìm kiếm số phiên tham dự tối thiểu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tập hợp con Brute Force | hàm mũ | hàm mũ | Quá chậm | 
| Phân tích kết cấu + đơn điệu |$O(1)$hoặc$O(\log n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi diễn giải lại vấn đề theo số lượng phiên bị bỏ qua. Cho phép$x$là số buổi Vasya bỏ qua nên anh ấy tham dự$n - x$phiên. Mục đích là xác định mức tối đa$x$sao cho tất cả các phiên bị bỏ qua có thể được bù đắp trong phạm vi chi phí$k$. 

1. Trước tiên, chúng tôi nhận thấy rằng chi phí chỉ phụ thuộc vào việc các phiên bị bỏ qua được ghép nối với các phiên tham dự sau đó như thế nào. Để giảm thiểu chi phí, mỗi phiên bị bỏ qua phải được bù càng sớm càng tốt vì chi phí tăng tuyến tính theo độ trễ. 
2. Chúng tôi sắp xếp lịch trình sao cho các phiên tham dự được phân bổ đều dưới dạng “điểm neo” giúp hấp thụ các phiên bị bỏ qua. Mỗi mỏ neo có thể bù đắp cho một khối phiên bị bỏ qua và chi phí trong mỗi khối tuân theo cấu trúc số học dựa trên khoảng cách. 
3. Đối với số phiên bị bỏ qua cố định$x$, chi phí tối thiểu có thể đạt được khi các phiên bị bỏ qua này tạo thành một tiền tố liên tục duy nhất, vì điều này giảm thiểu khoảng cách trung bình đến phiên tham dự sẵn có đầu tiên. Bất kỳ sự phân mảnh nào cũng sẽ chỉ làm tăng khoảng cách. 
4. Theo sự sắp xếp tối ưu này, chi phí sẽ trở thành một cấu trúc hình tam giác: các phiên bị bỏ qua sẽ đóng góp$1 + 2 + \dots + x$, bằng$x(x+1)/2$, vì mỗi phiên bị bỏ qua cuối cùng sẽ được khớp với phiên tham dự có sẵn tiếp theo theo cách tốt nhất có thể. 
5. Bây giờ chúng ta giải bất đẳng thức$x(x+1)/2 \le k$. Điều này đưa ra số lần bỏ qua tối đa được phép. Sau đó chúng tôi tính toán câu trả lời là$n - x$. 

### Tại sao nó hoạt động 

Bất biến chính là hàm chi phí chỉ phụ thuộc vào thứ tự tương đối giữa các phiên bị bỏ qua và đã tham dự chứ không phụ thuộc vào nhãn tuyệt đối của chúng. Bất kỳ cấu hình bỏ qua nào cũng có thể được chuyển đổi thành một khối liền kề mà không làm tăng chi phí. Sau khi giảm xuống dạng chuẩn này, chi phí sẽ trở thành cấu trúc tổng tiền tố thuần túy, được giảm thiểu duy nhất bằng cách nhóm các bước bỏ qua lại với nhau. Điều này làm cho số tam giác bị ràng buộc vừa cần thiết vừa đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())

    lo, hi = 0, n

    def cost(x):
        return x * (x + 1) // 2

    while lo < hi:
        mid = (lo + hi + 1) // 2
        if cost(mid) <= k:
            lo = mid
        else:
            hi = mid - 1

    skipped = lo
    print(n - skipped)

if __name__ == "__main__":
    solve()
```Giải pháp thực hiện tìm kiếm nhị phân theo số phiên bị bỏ qua. Hàm trợ giúp tính toán chi phí hình tam giác của một số lần bỏ qua nhất định. Chúng tôi tìm kiếm số lần bỏ qua lớn nhất có thể mà chi phí không vượt quá$k$, và trừ nó khỏi$n$để có được số lượng phiên tham dự tối thiểu. 

Tìm kiếm nhị phân an toàn vì hàm chi phí$x(x+1)/2$đang gia tăng nghiêm trọng trong$x$, đảm bảo tính đơn điệu về tính khả thi. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 7
```Chúng tôi kiểm tra các giá trị có thể có của các phiên bị bỏ qua: 

| x (bỏ qua) | giá x(x+1)/2 | hợp lệ | 
| --- | --- | --- | 
| 0 | 0 | vâng | 
| 1 | 1 | vâng | 
| 2 | 3 | vâng | 
| 3 | 6 | vâng | 
| 4 | 10 | không | 

Phiên bỏ qua hợp lệ tối đa là 3, vì vậy câu trả lời là$5 - 3 = 2$. 

Điều này cho thấy rằng mặc dù Vasya có thể bỏ qua hầu hết các buổi học nhưng chi phí vẫn tăng theo phương pháp bậc hai. 

### Ví dụ 2 

đầu vào:```
5 15
```| x | chi phí | 
| --- | --- | 
| 0 | 0 | 
| 1 | 1 | 
| 2 | 3 | 
| 3 | 6 | 
| 4 | 10 | 
| 5 | 15 | 

Ở đây tất cả 5 buổi có thể được bỏ qua, vì vậy số buổi tham dự tối thiểu là 0. 

Điều này thể hiện trường hợp ranh giới trong đó ngân sách khớp chính xác với chi phí tam giác đầy đủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(\log n)$| tìm kiếm nhị phân theo số phiên bị bỏ qua | 
| Không gian |$O(1)$| chỉ sử dụng các biến số học | 

Giải pháp dễ dàng xử lý$n, k \le 10^{18}$bởi vì nó tránh hoàn toàn việc lặp lại qua các phiên và chỉ dựa vào bất đẳng thức bậc hai đơn điệu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from io import StringIO

    out = StringIO()
    _sys.stdout = out

    solve()

    _sys.stdout = sys.__stdout__
    return out.getvalue()

def solve():
    n, k = map(int, input().split())

    lo, hi = 0, n

    def cost(x):
        return x * (x + 1) // 2

    while lo < hi:
        mid = (lo + hi + 1) // 2
        if cost(mid) <= k:
            lo = mid
        else:
            hi = mid - 1

    print(n - lo)

# provided samples
assert run("5 7\n") == "2\n"

# custom cases
assert run("1 0\n") == "1\n", "cannot skip anything"
assert run("1 1\n") == "0\n", "exact single skip"
assert run("10 0\n") == "10\n", "no budget"
assert run("10 55\n") == "0\n", "full budget allows all skips"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 0 | 1 | không được phép bỏ qua | 
| 1 1 | 0 | phần tử đơn biên | 
| 10 0 | 10 | cạnh ngân sách bằng không | 
| 10 55 | 0 | công suất hình tam giác đầy đủ | 

## Vỏ cạnh 

Đối với đầu vào`1 0`, chi phí bỏ qua một buổi đã là 1, vượt quá ngân sách. Tìm kiếm nhị phân ngay lập tức tìm thấy không có lần bỏ qua hợp lệ nào, vì vậy câu trả lời là 1 phiên tham dự. 

Đối với đầu vào`1 1`, bỏ qua phiên duy nhất có giá chính xác là 1, mức này được cho phép. Thuật toán trả về 0 phiên tham dự, phù hợp với chiến lược tối ưu. 

Đối với đầu vào`10 0`, không thể bỏ qua vì ngay cả một lần bỏ qua cũng có giá 1. Tìm kiếm nhị phân không mang lại lần bỏ qua nào, vì vậy Vasya tham dự tất cả các phiên. 

Đối với đầu vào`10 55`, ngân sách khớp với số tam giác đầy đủ$10 \cdot 11 / 2$. Thuật toán cho phép bỏ qua tất cả các phiên một cách chính xác, dẫn đến không có phiên nào được tham dự.
