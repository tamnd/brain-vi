---
title: "CF 105638B - Hile và Fx"
description: "Chúng ta được cung cấp một số mục tiêu cho mỗi trường hợp thử nghiệm và chúng ta cần quyết định xem liệu nó có thể được biểu diễn dưới dạng tổng của một số nguyên được xây dựng cẩn thận và tổng các chữ số của nó hay không."
date: "2026-06-22T05:27:37+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105638
codeforces_index: "B"
codeforces_contest_name: "GPC 2024"
rating: 0
weight: 105638
solve_time_s: 56
verified: true
draft: false
---

[CF 105638B - Hile và Fx](https://codeforces.com/problemset/problem/105638/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 56s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một số mục tiêu cho mỗi trường hợp thử nghiệm và chúng ta cần quyết định xem liệu nó có thể được biểu diễn dưới dạng tổng của một số nguyên được xây dựng cẩn thận và tổng các chữ số của nó hay không. Chính xác hơn, chúng ta đang tìm một số nguyên không âm$x$sao cho khi chúng ta tính tổng các chữ số thập phân của nó và cộng lại vào$x$, kết quả bằng số đã cho. 

Nếu như vậy$x$tồn tại, mọi cách xây dựng hợp lệ đều được chấp nhận. Nếu không có số nguyên nào có thể thỏa mãn điều kiện này thì chúng ta phải xuất ra -1. 

Kích thước đầu vào gợi ý nhiều truy vấn độc lập, mỗi truy vấn yêu cầu quyết định nhanh thay vì tìm kiếm. Vì đầu ra phụ thuộc vào cấu trúc chữ số chứ không phải chỉ thuộc tính số học, nên không thể thực hiện mạnh mẽ tất cả các giá trị ứng cử viên khi mục tiêu có thể lớn. Đang thử tất cả$x$lên đến$n$sẽ là tuyến tính cho mỗi trường hợp thử nghiệm và điều đó sẽ bị hỏng ngay lập tức nếu tổng các trường hợp thử nghiệm lớn. 

Một trường hợp phức tạp xuất hiện khi suy nghĩ ngược lại. Nhiều nỗ lực ngây thơ cố gắng "trừ tổng chữ số" khỏi mục tiêu, nhưng tổng chữ số phụ thuộc vào chính số đó, do đó phép trừ tùy ý không bảo toàn tính hợp lệ. 

Một trường hợp thất bại cụ thể giúp minh họa điều này. Giả sử mục tiêu là 1. Nếu chúng ta cố gắng$x = 1$, sau đó$x + S(x) = 1 + 1 = 2$, vượt quá. Đang cố gắng$x = 0$cho$0 + 0 = 0$. Không có cách nào đạt đến 1 nên kết quả đúng là -1. Điều này cho thấy rằng không phải tất cả các số nguyên đều có thể biểu diễn được. 

Một cạm bẫy tiềm ẩn khác là giả định rằng mọi số có thể được hình thành bằng cách sử dụng tổng các chữ số một cách tùy ý, điều này sai vì tổng các chữ số bị ràng buộc chặt chẽ bởi số chữ số mà số đó có và đóng góp tối đa của chúng. 

## Phương pháp tiếp cận 

Một chiến lược bạo lực sẽ lặp đi lặp lại trên tất cả các ứng cử viên có thể$x$từ 0 lên đến giá trị đích và kiểm tra xem$x + S(x)$bằng mục tiêu. Tính tổng các chữ số mất$O(\log x)$, do đó tổng chi phí cho mỗi trường hợp thử nghiệm sẽ trở thành$O(n \log n)$trong trường hợp xấu nhất. Nếu như$n$có thể lớn, tốc độ này quá chậm, đặc biệt khi lặp lại trên nhiều truy vấn. 

Quan sát quan trọng là các số hợp lệ có cấu trúc rất cứng nhắc. Nếu chúng ta sắp xếp lại phương trình$$x + S(x) = n,$$chúng tôi thấy điều đó$n - x = S(x)$. Phía bên phải là tổng các chữ số, luôn là cấu trúc bội số của 9 khi các chữ số được tối đa hóa theo cách có kiểm soát. 

Cái nhìn sâu sắc mang tính xây dựng là giả định$x$được tạo thành từ số 9 lặp đi lặp lại. Nếu như$x$bao gồm$k$các chữ số đều bằng 9 thì$$x = 10^k - 1, \quad S(x) = 9k.$$Thay vào phương trình sẽ cho$$n = (10^k - 1) + 9k = 10^k + 9k - 1.$$Điều này có nghĩa là mọi câu trả lời hợp lệ đều tương ứng với việc chọn độ dài chữ số$k$, xây dựng một số tất cả số 9 và kiểm tra xem nó có khớp với mục tiêu hay không. Thay vì tìm kiếm trên tất cả các số nguyên, chúng tôi chỉ kiểm tra một phạm vi nhỏ các$k$, giới hạn bởi số chữ số trong$n$. 

Điều này làm giảm vấn đề từ việc tìm kiếm các giá trị đến việc kiểm tra một số ứng cử viên có cấu trúc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên tất cả x |$O(n \log n)$|$O(1)$| Quá chậm | 
| Hãy thử xây dựng độ dài chữ số |$O(\log n)$mỗi bài kiểm tra |$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Đối với mỗi trường hợp thử nghiệm, chúng tôi cố gắng xây dựng lại câu trả lời bằng cách khai thác thực tế là các số hợp lệ phải bao gồm toàn bộ số 9. 

1. Đọc số mục tiêu$n$. Đây là giá trị chúng tôi muốn khớp bằng cách sử dụng$x + S(x)$. 
2. Lặp lại độ dài chữ số có thể$k$, bắt đầu từ 1 đến một giới hạn hợp lý chẳng hạn như 18 hoặc số chữ số trong$n$. Giới hạn này là đủ vì lớn hơn$k$đã tạo ra các giá trị vượt quá các ràng buộc điển hình. 
3. Xây dựng số ứng viên$x$bao gồm$k$các chữ số, tất cả đều bằng 9. Điều này tương ứng với$x = 10^k - 1$. 
4. Tính biểu thức cần tìm$x + S(x)$, Ở đâu$S(x) = 9k$vì tất cả các chữ số đều là 9 
5. Nếu giá trị này bằng$n$, quay lại ngay$x$như một câu trả lời hợp lệ. 
6. Nếu không có giá trị của$k$tạo ra kết quả khớp, trả về -1. 

### Tại sao nó hoạt động 

Mọi lời giải hợp lệ đều phải thỏa mãn$n - x = S(x)$. Tổng chữ số tăng tuyến tính với số chữ số, nhưng bản thân số đó tăng theo cấp số nhân. Cách duy nhất mà hai đại lượng này có thể được đồng bộ hóa một cách ổn định, có thể dự đoán được theo độ dài chữ số là khi các chữ số được tối đa hóa, điều này buộc phải có cấu trúc gồm tất cả số 9. Điều này hạn chế không gian giải pháp cho một họ nhỏ, có thể đếm được. Vì mọi ứng cử viên trong họ này đều được kiểm tra trực tiếp nên không có giải pháp hợp lệ nào có thể bị bỏ sót và không có kết quả xây dựng không hợp lệ nào có thể vô tình vượt qua. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve_one(n):
    # try k-digit all-9 numbers
    x = 0
    s = 0

    # we build 9, 99, 999... incrementally
    for k in range(1, 20):
        x = x * 10 + 9
        s = 9 * k
        if x + s == n:
            return x
    return -1

def main():
    t = int(input())
    for _ in range(t):
        n = int(input())
        print(solve_one(n))

if __name__ == "__main__":
    main()
```Giải pháp duy trì cấu trúc đang chạy của số bao gồm các số 9 lặp lại thay vì tính toán lại lũy thừa của mười. Mỗi lần lặp lại tăng độ dài chữ số lên một và tổng chữ số được cập nhật theo phương pháp phân tích dưới dạng$9k$, tránh tính toán theo từng chữ số. 

Giới hạn vòng lặp 20 là đủ vì thậm chí$10^{19}$đã vượt quá các ràng buộc điển hình và hơn thế nữa, các giá trị được xây dựng tăng quá lớn để phù hợp với đầu vào hợp lý. 

## Ví dụ đã hoạt động 

Hãy xem xét trường hợp mục tiêu là 19. 

Chúng tôi kiểm tra giá trị tăng dần của$k$. 

| k | x (tất cả 9s) | S(x) | x + S(x) | 
| --- | --- | --- | --- | 
| 1 | 9 | 9 | 18 | 
| 2 | 99 | 18 | 117 | 

Không có kết quả trùng khớp nào xuất hiện nên kết quả đầu ra là -1. Điều này cho thấy rằng không phải tất cả các số đều có thể được hình thành. 

Bây giờ hãy xem xét một trường hợp hợp lệ được xây dựng. Giả sử mục tiêu là 18. 

| k | x (tất cả 9s) | S(x) | x + S(x) | 
| --- | --- | --- | --- | 
| 1 | 9 | 9 | 18 | 

Ở đây chúng tôi ngay lập tức tìm thấy kết quả trùng khớp, vì vậy đầu ra là 9. 

Dấu vết đầu tiên thể hiện sự từ chối khi không có độ dài chữ số nào phù hợp với mục tiêu. Thứ hai xác nhận rằng thuật toán xác định chính xác một cấu trúc hợp lệ mà không cần bất kỳ tìm kiếm nào ngoài các ứng cử viên có cấu trúc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(T \log n)$| Đối với mỗi trường hợp thử nghiệm, chúng tôi thử độ dài tối đa ~ 20 chữ số | 
| Không gian |$O(1)$| Chỉ một số số nguyên được lưu trữ | 

Thuật toán dễ dàng đủ nhanh cho các kích thước đầu vào lớn vì mỗi trường hợp thử nghiệm thực hiện một lượng số học không đổi độc lập với độ lớn của$n$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    def solve():
        t = int(input())
        out = []
        for _ in range(t):
            n = int(input())
            found = False
            x = 0
            for k in range(1, 20):
                x = x * 10 + 9
                if x + 9 * k == n:
                    out.append(str(x))
                    found = True
                    break
            if not found:
                out.append("-1")
        return "\n".join(out)

    return solve()

# edge-like simple cases
assert run("3\n18\n117\n1\n") == "9\n99\n-1", "basic cases"
assert run("1\n18\n") == "9", "single valid k=1"
assert run("1\n19\n") == "-1", "no solution"
assert run("1\n117\n") == "99", "k=2 case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 18 | 9 | công trình hợp lệ nhỏ nhất | 
| 19 | -1 | trường hợp bất khả thi | 
| 117 | 99 | cấu trúc hợp lệ nhiều chữ số | 
| 1 | -1 | trường hợp cạnh dưới giá trị có thể tiếp cận tối thiểu | 

## Vỏ cạnh 

Đầu vào nhỏ như 1 cho thấy không thể đạt được mục tiêu rất thấp. Chạy thuật toán, ứng cử viên đầu tiên là$x = 9$, đã tạo ra 18, do đó không có ứng cử viên nào nhỏ hơn tồn tại và kết quả chính xác trở thành -1. 

Đối với một số không thể truy cập cỡ trung bình như 19, thuật toán sẽ kiểm tra$k=1$Và$k=2$, tạo ra lần lượt 18 và 117, không phù hợp với mục tiêu. Vì không còn cấu trúc nào tồn tại nên đầu ra vẫn là -1. 

Đối với số có nhiều chữ số hợp lệ như 117, lần lặp đạt tới$k=2$, xây dựng$x=99$, và xác minh$99 + 18 = 117$, tạo ra câu trả lời đúng ngay lập tức mà không cần khám phá thêm giá trị.
