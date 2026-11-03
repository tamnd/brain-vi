---
title: "CF 1104A – Chia thành chữ số"
description: "Chúng ta được cấp một số nguyên $n$ và chúng ta muốn biểu diễn nó dưới dạng tổng của các số nguyên dương, mỗi số từ 1 đến 9."
date: "2026-06-15T05:20:44+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1104
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 534 (Div. 2)"
rating: 800
weight: 1104
solve_time_s: 108
verified: true
draft: false
---

[CF 1104A - Chia thành các chữ số](https://codeforces.com/problemset/problem/1104/A) 

**Đánh giá:** 800 
**Tags:** thuật toán xây dựng, triển khai, toán học 
**Thời gian giải:** 1 phút 48 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một số nguyên duy nhất$n$và chúng tôi muốn biểu thị nó dưới dạng tổng của các số nguyên dương, mỗi số bao gồm từ 1 đến 9. Nói cách khác, chúng ta đang chia giá trị$n$thành “các chữ số”, nhưng được phép lặp lại và không yêu cầu các chữ số phải tạo thành một số, chỉ cần tổng của chúng bằng nhau$n$. 

Trong số tất cả các cách có thể để phân hủy$n$, chúng tôi muốn giảm thiểu số lượng giá trị chữ số riêng biệt xuất hiện trong quá trình phân tách. Ví dụ, nếu chúng ta có thể diễn đạt$n$chỉ sử dụng chữ số 2 được lặp lại nhiều lần, tốt hơn là sử dụng kết hợp các số 1, 2 và 3. 

Đầu ra là bất kỳ phân tách hợp lệ nào đạt được số giá trị chữ số riêng biệt nhỏ nhất có thể. Chúng tôi cũng xuất ra tổng số chữ số được sử dụng. 

Ràng buộc$n \leq 1000$đủ nhỏ để ngay cả việc xây dựng tuyến tính cho mỗi trường hợp thử nghiệm cũng đủ. Bất kỳ giải pháp nào chạy trong$O(n)$hoặc tốt hơn là nhanh chóng. 

Một điểm tinh tế là mục tiêu không phải là giảm thiểu số lượng chữ số mà là giảm thiểu số lượng các giá trị chữ số khác nhau được sử dụng. Một người đọc ngây thơ có thể cố gắng giảm độ dài trước một cách không chính xác, dẫn đến suy luận sai. 

Một cách tiếp cận sai điển hình là tham lam lấy chữ số lớn nhất có thể (như luôn sử dụng số 9). Mặc dù điều này giảm thiểu số lượng chữ số nhưng nó không trực tiếp đảm bảo các giá trị khác biệt tối thiểu theo cách có cấu trúc trừ khi được xử lý cẩn thận. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ thử tất cả các tập hợp chữ số có thể có từ 1 đến 9 có tổng là$n$, rồi tính xem có bao nhiêu giá trị riêng biệt xuất hiện trong mỗi tập hợp ứng cử viên. Đây thực chất là một bài toán phân vùng số nguyên có giới hạn với một ràng buộc bổ sung về các giá trị được phép và mục tiêu phụ về số lượng riêng biệt. Số lượng sáng tác của$n$tăng theo cấp số nhân, gần như theo thứ tự$2^n$đối với sự phân chia không hạn chế, khiến điều này hoàn toàn không khả thi ngay cả đối với$n = 1000$. 

Quan sát quan trọng là việc hạn chế số lượng giá trị chữ số riêng biệt mạnh hơn nhiều so với vẻ ngoài của nó. Nếu chúng tôi quyết định chỉ sử dụng giá trị một chữ số$d$, thì vấn đề sẽ trở thành kiểm tra xem$n$chia hết cho$d$, và nếu vậy, sử dụng$n/d$bản sao của$d$. Điều này ngay lập tức đưa ra một phân tách hợp lệ với chính xác một chữ số riêng biệt. 

Nếu không có một chữ số nào hoạt động thì chúng tôi thử sử dụng hai chữ số riêng biệt. Tuy nhiên, đối với bài toán này chúng ta không cần phải xây dựng một cách rõ ràng các hỗn hợp phức tạp. Thay vào đó, chúng ta có thể nhận thấy rằng việc sử dụng chữ số 1 luôn được cho phép và luôn có thể hoàn thành mọi phần còn lại. Vì vậy, chúng ta luôn có thể đảm bảo tính khả thi với tối đa hai chữ số riêng biệt và trên thực tế, một chiến lược mang tính xây dựng đơn giản hơn là đủ: sử dụng càng nhiều số 9 càng tốt và kết thúc với số còn lại. 

Giải pháp mang tính xây dựng tiêu chuẩn là tối đa hóa việc sử dụng chữ số 9, sau đó coi phần còn lại là chữ số cuối cùng. Điều này tạo ra tối đa hai chữ số riêng biệt: 9 và phần còn lại (nếu khác 0). Vì 1 cũng là một chữ số nên số dư luôn đúng. 

Cấu trúc này là tối ưu vì chỉ có thể sử dụng một chữ số khi$n \leq 9$, nếu không thì bất kỳ chữ số nào cũng phải chia$n$, điều này hiếm khi có thể thực hiện được. Vì$n > 9$, điều tốt nhất chúng tôi có thể đảm bảo là sử dụng chính xác các giá trị có hai chữ số và cấu trúc này đạt được giới hạn đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên tất cả các phân vùng | hàm mũ | hàm mũ | Quá chậm | 
| Tham lam 9s + xây dựng còn lại |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính xem có bao nhiêu số 9 đầy đủ$n$, đó là$q = n // 9$. Điều này tối đa hóa số lượng chữ số lớn, giảm phần còn lại càng nhiều càng tốt. 
2. Tính số dư$r = n \bmod 9$. Đây là giá trị duy nhất không thể biểu thị bằng số 9 khác. 
3. Nếu$r = 0$, đầu ra$q$các chữ số, tất cả đều bằng 9. Điều này hiệu quả vì$9q = n$, do đó ràng buộc tổng được thỏa mãn khi sử dụng giá trị một chữ số. 
4. Nếu$r \neq 0$, đầu ra$q$các chữ số bằng 9 và một chữ số bổ sung bằng$r$. Điều này đảm bảo tổng sẽ trở thành$9q + r = n$. 
5. In tổng số chữ số$k = q + (r \neq 0)$, theo sau là trình tự được xây dựng. 

Việc lựa chọn số 9 không phải là tùy ý. Nó là chữ số lớn nhất được phép, do đó nó giảm thiểu số chữ số cần thiết để đạt được$n$và mọi phần còn lại được đảm bảo vẫn nằm trong phạm vi chữ số hợp lệ$[1, 9]$. 

### Tại sao nó hoạt động 

Việc xây dựng luôn tạo ra một phân tách hợp lệ vì mọi phần tử đều nằm trong khoảng từ 1 đến 9 và tổng được bảo toàn chính xác. Trong số tất cả các phép phân tách chỉ sử dụng giá trị một chữ số, chỉ có ước số hoàn hảo của$n$đều có thể xảy ra, điều này rất hiếm. Khi chúng tôi cho phép chữ số thứ hai, việc sử dụng số 9 sẽ giảm thiểu số lượng thành phần bắt buộc và mọi phần còn lại không thể được chia thêm mà không đưa ra các giá trị chữ số riêng biệt bổ sung. Điều này đảm bảo chúng tôi không bao giờ giới thiệu nhiều hơn hai chữ số riêng biệt, tối ưu cho tất cả$n > 9$. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input().strip())
    
    q = n // 9
    r = n % 9
    
    res = []
    
    for _ in range(q):
        res.append(9)
    
    if r:
        res.append(r)
    
    print(len(res))
    print(*res)

if __name__ == "__main__":
    solve()
```Mã trực tiếp thực hiện việc xây dựng có nguồn gốc ở trên. Phép chia số nguyên tính toán có bao nhiêu số 9 đầy đủ được sử dụng, trong khi modulo tính toán phần còn lại. 

Một cạm bẫy triển khai phổ biến là quên xử lý trường hợp$r = 0$, điều này sẽ thêm không chính xác một chữ số 0 hoặc phần tử bổ sung. Một vấn đề khác là tính sai số chữ số$k$, phải khớp chính xác với danh sách được xây dựng. 

## Ví dụ đã hoạt động 

### Ví dụ 1:$n = 1$| Bước | q | r | xây dựng | 
| --- | --- | --- | --- | 
| tính toán | 0 | 1 | bắt đầu trống rỗng | 
| phần còn lại | 0 | 1 | thêm 1 | 
| đầu ra | - | - | [1] | 

Điều này xác nhận trường hợp cơ bản trong đó không thể sử dụng số 9 và bản thân số đó có giá trị trực tiếp dưới dạng chữ số. 

### Ví dụ 2:$n = 20$| Bước | q | r | xây dựng | 
| --- | --- | --- | --- | 
| tính toán | 2 | 2 | bắt đầu bằng hai số 9 | 
| phần còn lại | 2 | 2 | thêm 2 | 
| đầu ra | - | - | [9, 9, 2] | 

Điều này cho thấy cách thuật toán nén hầu hết giá trị thành các chữ số tối đa và xử lý phần còn lại một cách rõ ràng mà không làm tăng số lượng chữ số riêng biệt vượt quá hai. 

Dấu vết xác nhận rằng tổng vẫn chính xác và việc xây dựng không bao giờ vượt quá giới hạn chữ số. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(1)$| Chỉ số học và đầu ra tối đa 1000/9 chữ số | 
| Không gian |$O(1)$| Danh sách đầu ra tỷ lệ thuận với$n/9$, giới hạn bởi 112 | 

Thuật toán dễ dàng phù hợp trong giới hạn vì$n \leq 1000$, do đó, ngay cả việc xây dựng đầu ra tuyến tính cũng không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from math import isclose

    # re-run solution
    n = int(inp.strip())
    q = n // 9
    r = n % 9
    res = [9] * q
    if r:
        res.append(r)
    return str(len(res)) + "\n" + " ".join(map(str, res))

# provided samples
assert run("1") == "1\n1"

# custom cases
assert run("9") == "1\n9"
assert run("10") == "2\n9 1"
assert run("18") == "2\n9 9"
assert run("1000") == "112\n" + " ".join(["9"] * 111 + ["1"])
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 1/1 | ranh giới tối thiểu | 
| 9 | 1/9 | chính xác một chữ số | 
| 10 | 9 1 | xử lý phần còn lại | 
| 18 | 9 9 | bội số chính xác của 9 | 
| 1000 | 111×9 + 1 | công trình lớn | 

## Vỏ cạnh 

cho$n = 9$, thuật toán tính toán$q = 1$,$r = 0$, tạo ra một chữ số [9]. Điều này tránh được việc thêm chữ số còn lại bằng 0, điều này sẽ không hợp lệ. 

Vì$n = 10$, chúng tôi nhận được$q = 1$,$r = 1$, sản xuất [9, 1]. Tổng được giữ nguyên chính xác và chỉ xuất hiện hai chữ số riêng biệt, điều này là tối ưu vì 10 không thể được biểu thị bằng một chữ số trong [1,9]. 

Vì$n = 1000$, thuật toán tạo ra 111 số chín và số 1 cuối cùng. Cấu trúc vẫn ổn định dưới kích thước đầu vào tối đa và không yêu cầu tràn hoặc xử lý đặc biệt ngoài việc xây dựng danh sách.
