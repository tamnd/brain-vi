---
title: "CF 104833F - \u820c\u5207\u96c0\uff08Phiên bản dễ dàng\uff09"
description: "Chúng ta được cung cấp một hàm được xác định trên các số tự nhiên. Đối với một số $x$, chúng ta xem xét liệu có tồn tại số mũ nguyên $k 1$ sao cho $x^k$ là số hữu tỷ hay không. Hàm $f(x)$ xuất ra 1 nếu số mũ đó tồn tại, nếu không thì nó xuất ra 0."
date: "2026-06-28T11:53:45+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104833
codeforces_index: "F"
codeforces_contest_name: "The 2023 Zhejiang SCI-TECH University Freshman Programming Contest"
rating: 0
weight: 104833
solve_time_s: 42
verified: true
draft: false
---

[CF 104833F - \u820c\u5207\u96c0\uff08Easy Version\uff09](https://codeforces.com/problemset/problem/104833/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 42s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một hàm được xác định trên các số tự nhiên. Đối với một số$x$, chúng ta xem xét liệu có tồn tại số mũ nguyên hay không$k > 1$như vậy$x^k$là một số hữu tỉ. chức năng$f(x)$xuất ra 1 nếu số mũ như vậy tồn tại, nếu không thì xuất ra 0. Đối với mỗi trường hợp thử nghiệm, chúng ta được yêu cầu tính tổng của$f(i)$trên tất cả các số nguyên$i$từ 1 đến$n$. 

Đầu vào chứa nhiều truy vấn và mỗi truy vấn cung cấp một giá trị duy nhất$n$lên đến$10^{12}$. Điều này ngay lập tức gợi ý rằng bất kỳ giải pháp nào lặp lại trên tất cả các giá trị lên đến$n$là không thể, vì ngay cả một trường hợp thử nghiệm cũng có thể yêu cầu tới$10^{12}$hoạt động. 

Điểm tinh tế quan trọng trong câu lệnh này là chúng ta đang đánh giá các số nguyên đầu vào và điều kiện liên quan đến lũy thừa của các số nguyên đó. Một cạm bẫy phổ biến là suy nghĩ quá mức về điều kiện như một cái gì đó liên quan đến lý thuyết số sâu hoặc đầu vào không nguyên. Tuy nhiên, vì mỗi$i$là một số tự nhiên, mọi lũy thừa$i^k$cũng là một số nguyên và do đó tự động là một số hữu tỷ. 

Một cách hiểu sai ngây thơ sẽ là cố gắng kiểm tra xem liệu có tồn tại số mũ nào đó làm cho kết quả trở nên hợp lý thông qua lý luận đại số phức tạp hay không. Điều đó là không cần thiết ở đây và sẽ dẫn đến việc triển khai quá phức tạp mà vẫn thiếu sự đơn giản hóa cốt lõi. 

Một trường hợp điển hình để kiểm tra trực giác là$i = 1$. Từ$1^k = 1$, nó luôn hợp lý, vì vậy$f(1) = 1$. Đối với bất kỳ số nguyên nào khác, giả sử$i = 2$, chúng tôi vẫn có$2^k$là số nguyên cho tất cả$k$, do đó hợp lý, do đó$f(2) = 1$. Mẫu này đúng cho mọi số tự nhiên. 

Vì vậy, mọi số hạng trong tổng đều đóng góp 1. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ trực tiếp đánh giá định nghĩa cho từng$i$từ 1 đến$n$, và với mỗi trường hợp như vậy$i$, hãy thử các giá trị khác nhau của$k > 1$và kiểm tra xem$i^k$là hợp lý. Từ$i^k$có thể được tính toán chính xác cho các số nguyên, cách tiếp cận này sẽ luôn xác nhận điều kiện. Tuy nhiên, ngay cả khi chúng ta giới hạn mình trong một phạm vi nhỏ$k$, chúng tôi vẫn đang biểu diễn$O(n)$hoạt động trên mỗi truy vấn, điều này vượt xa khả thi khi$n$có thể đạt được$10^{12}$. 

Quan sát quan trọng là điều kiện không thực sự lọc ra bất kỳ số tự nhiên nào. Với mọi số nguyên$i \ge 1$và bất kỳ số mũ số nguyên nào$k > 1$, giá trị$i^k$vẫn là số nguyên nên thuộc tập hợp số hữu tỉ. Điều này có nghĩa là điều kiện tồn tại trong định nghĩa của$f(x)$luôn được thỏa mãn. 

Khi chúng tôi nhận ra rằng mọi phần tử đều đóng góp 1, vấn đề sẽ giảm xuống việc tính tổng tiền tố đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(nT)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(T)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case$T$. Mỗi trường hợp thử nghiệm là độc lập và yêu cầu tổng phạm vi lên tới$n$. 
2. Với mỗi test, hãy đọc số nguyên$n$. 
3. Nhận biết mọi số nguyên$i$TRONG$[1, n]$thỏa mãn$f(i) = 1$, do đó tổng được yêu cầu chỉ đơn giản là số số nguyên trong phạm vi. 
4. Tính đáp án dưới dạng$n$. 
5. Xuất kết quả. 

### Tại sao nó hoạt động 

chức năng$f(i)$phụ thuộc vào việc có tồn tại số mũ hay không$k > 1$như vậy$i^k$là hợp lý. Từ$i$là một số tự nhiên, bất kỳ lũy thừa nào$i^k$vẫn là số nguyên. Mọi số nguyên đều là số hữu tỉ nên điều kiện luôn đúng. Điều này làm cho$f(i)$giống hệt nhau bằng 1 trên toàn bộ miền$[1, n]$, đảm bảo tổng chính xác$n$. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        print(n)

if __name__ == "__main__":
    solve()
```Giải pháp chỉ cần đọc từng truy vấn và kết quả đầu ra$n$, vì mỗi số hạng đóng góp chính xác một đơn vị vào tổng. 

Chi tiết triển khai tinh tế duy nhất là đảm bảo I/O nhanh vì$T$có thể lớn như$2 \times 10^3$, nhưng ngay cả đầu vào/đầu ra tiêu chuẩn ở đây cũng đủ do quá trình xử lý liên tục theo thời gian cho mỗi trường hợp thử nghiệm. 

## Ví dụ đã hoạt động 

Hãy xem xét một đầu vào có nhiều giá trị của$n$. 

Vì$n = 1$, chúng ta tính tổng theo một giá trị duy nhất. Từ$f(1) = 1$, kết quả là 1. 

cho$n = 5$, mỗi giá trị từ 1 đến 5 đóng góp 1. 

| tôi | f(i) | 
| --- | --- | 
| 1 | 1 | 
| 2 | 1 | 
| 3 | 1 | 
| 4 | 1 | 
| 5 | 1 | 

Tổng là 5. 

Điều này xác nhận rằng hàm này không đưa ra bất kỳ biến thể nào trong phạm vi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(T)$| Mỗi test case được xử lý trong thời gian không đổi | 
| Không gian |$O(1)$| Không có bộ nhớ phụ ngoài biến | 

Các ràng buộc cho phép lên tới 2000 trường hợp thử nghiệm và giá trị lên tới$10^{12}$, nhưng vì mỗi truy vấn đều được trả lời ngay lập tức nên giải pháp dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def solve():
        t = int(input())
        for _ in range(t):
            n = int(input())
            print(n)

    old_stdout = sys.stdout
    sys.stdout = io.StringIO()
    solve()
    out = sys.stdout.getvalue().strip()
    sys.stdout = old_stdout
    return out

# provided sample-style checks
assert run("1\n1\n") == "1"
assert run("2\n1\n5\n") == "1\n5"

# custom cases
assert run("3\n10\n100\n1000\n") == "10\n100\n1000"
assert run("1\n1000000000000\n") == "1000000000000"
assert run("4\n2\n3\n4\n5\n") == "2\n3\n4\n5"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đơn nhỏ n | 1 | trường hợp cơ sở đúng đắn | 
| nhiều truy vấn | n giá trị | tính độc lập của các truy vấn | 
| lớn | n | xử lý các ràng buộc lớn | 
| phạm vi liên tiếp | cùng số | không có sự biến đổi ẩn giấu | 

## Vỏ cạnh 

cho$n = 1$, vòng lặp chỉ chứa một phần tử duy nhất và vì$f(1)$luôn là 1, đầu ra là 1. Thuật toán xử lý việc này trực tiếp mà không cần bất kỳ sự phân nhánh đặc biệt nào. 

Đối với rất lớn$n$, chẳng hạn như$10^{12}$, việc tính toán không phụ thuộc vào việc lặp lại trong phạm vi, do đó áp dụng logic thời gian không đổi tương tự. Đầu ra vẫn còn$n$, xác nhận rằng không có hiện tượng tràn hoặc xấp xỉ nào xảy ra do số nguyên Python xử lý kích thước tùy ý một cách an toàn.
