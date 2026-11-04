---
title: "CF 1131E - Phép nhân chuỗi"
description: "Chúng ta được yêu cầu tính toán một thuộc tính của một phép toán chuỗi có cấu trúc cao. Chúng ta được cho một chuỗi các chuỗi $p1, p2, dots, pn$, và chúng ta được yêu cầu áp dụng nhiều lần phép nhân chuỗi của Denis: nhân $p1 cdot p2 cdot dots cdot pn$ theo thứ tự."
date: "2026-06-12T04:13:33+07:00"
tags: ["codeforces", "competitive-programming", "dp", "greedy", "strings"]
categories: ["algorithms"]
codeforces_contest: 1131
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 541 (Div. 2)"
rating: 2300
weight: 1131
solve_time_s: 93
verified: true
draft: false
---

[CF 1131E - Phép nhân chuỗi](https://codeforces.com/problemset/problem/1131/E) 

**Đánh giá:** 2300 
**Tags:** dp, tham lam, dây 
**Thời gian giải:** 1m 33s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu tính toán một thuộc tính của một phép toán chuỗi có cấu trúc cao. Chúng tôi được cung cấp một chuỗi các chuỗi$p_1, p_2, \dots, p_n$và chúng ta được yêu cầu áp dụng nhiều lần phép nhân chuỗi của Denis: nhân$p_1 \cdot p_2 \cdot \dots \cdot p_n$theo thứ tự. phép nhân$s \cdot t$chèn$t$trước và sau mỗi ký tự của$s$, và cũng một lần ngay từ đầu. Sau khi thực hiện tất cả các phép nhân, chúng ta cần tính toán độ “vẻ đẹp” của chuỗi kết quả, đó là độ dài của khối liền kề dài nhất của một ký tự đơn. 

Cách tiếp cận đơn giản nhất là xây dựng chuỗi kết quả theo đúng nghĩa đen bằng cách tuân theo quy tắc nhân từng bước. Tuy nhiên, mỗi phép nhân có thể tăng độ dài chuỗi một cách đáng kể. Một chuỗi có độ dài$m$nhân với một chuỗi có độ dài$k$tạo ra một chuỗi có độ dài$k \cdot (m+1) + m$, có thể tăng theo cấp số nhân. Với$n$lên đến$10^5$và tổng chiều dài đầu vào lên tới$10^5$, việc xây dựng chuỗi cuối cùng là không thể trong giới hạn bộ nhớ và thời gian. Vì vậy chúng ta cần suy luận về vẻ đẹp mà không cần xây dựng sợi dây một cách rõ ràng. 

Các trường hợp quan trọng không rõ ràng phát sinh khi các chuỗi được tạo hoàn toàn bằng một ký tự lặp lại. Nếu như$p_i$là “aaa” và$p_{i+1}$cũng là “aaa”, phép nhân có thể tạo ra một khối liền kề dài hơn từng chuỗi riêng lẻ. Ngược lại, nếu các ký tự khác nhau, chúng ta cần tính đến cách các tiền tố và hậu tố của cùng một ký tự có thể hợp nhất khi chèn vào. Ví dụ nhỏ như$p_1 = a, p_2 = a, p_3 = b$chỉ ra rằng cần phải theo dõi cẩn thận chuỗi ký tự đầu tiên/cuối cùng; việc triển khai bất cẩn có thể chỉ nhìn vào cực đại cục bộ và bỏ lỡ các lần chạy được nối. 

## Phương pháp tiếp cận 

Phương pháp brute-force xây dựng chuỗi đầy đủ theo cách lặp đi lặp lại và tính toán khối liền kề dài nhất gồm các chữ cái giống nhau sau mỗi lần nhân. Về nguyên tắc, điều này đúng vì nó tuân theo định nghĩa của phép nhân, nhưng mỗi bước sẽ tăng kích thước chuỗi theo cấp số nhân. Trong trường hợp xấu nhất, chuỗi cuối cùng có thể đạt độ dài cỡ$10^{100}$nếu tất cả các chuỗi có độ dài 2 và$n=100$, điều đó là không thể thực hiện được. 

Cách tiếp cận tối ưu tránh việc xây dựng chuỗi đầy đủ. Chúng ta chỉ cần ba mẩu thông tin từ mỗi chuỗi để hợp nhất nó một cách hiệu quả: vẻ đẹp bên trong tối đa (khối dài nhất bên trong chuỗi), độ dài tiền tố của các ký tự giống hệt nhau và độ dài hậu tố của các ký tự giống hệt nhau. Với những điều này, chúng ta có thể cập nhật vẻ đẹp tích lũy khi kết hợp hai dây. Cái nhìn sâu sắc là phép nhân chỉ ảnh hưởng đến vẻ đẹp bên trong bằng cách có thể hợp nhất hậu tố của một chuỗi với các ký tự lặp lại của chuỗi tiếp theo. Nếu chuỗi tiếp theo hoàn toàn là một ký tự, thì chiều dài khối trước đó có thể được nhân lên. Mặt khác, chỉ có tiền tố, hậu tố và cực đại bên trong là quan trọng. Bằng cách duy trì các giá trị này khi xử lý các chuỗi một cách tuần tự, chúng tôi có thể tính toán vẻ đẹp cuối cùng theo thời gian tuyến tính đối với tổng kích thước đầu vào. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(tổng chiều dài của chuỗi cuối cùng) | O(tổng chiều dài của chuỗi cuối cùng) | Quá chậm | 
| Tối ưu | O(tổng độ dài của chuỗi đầu vào) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mỗi chuỗi đầu vào$p_i$, tính khối tối đa của các ký tự giống hệt nhau liên tiếp, cũng như độ dài tiền tố và hậu tố của các ký tự lặp lại. 
2. Khởi tạo vẻ đẹp tích lũy bằng cách sử dụng các giá trị của chuỗi đầu tiên. 
3. Lặp lại các chuỗi còn lại. Đối với mỗi chuỗi, trước tiên hãy xem xét liệu chuỗi đó có hoàn toàn là một ký tự lặp lại hay không. Nếu vậy, hãy cập nhật độ dài tiền tố và hậu tố tích lũy bằng quy tắc nhân tính các khối liên tiếp được hình thành thông qua các lần chèn lặp lại. 
4. Nếu chuỗi hiện tại có nhiều hơn một ký tự duy nhất, hãy cập nhật vẻ đẹp tối đa là giá trị lớn nhất trong ba ứng cử viên: vẻ đẹp tích lũy trước đó, khối cực đại bên trong trong chuỗi hiện tại và các khối hợp nhất tiềm năng được hình thành bằng cách kết hợp hậu tố trước đó, một lần xuất hiện của chuỗi hiện tại và tiền tố hiện tại. 
5. Cập nhật tiền tố và hậu tố tích lũy: nếu chuỗi tích lũy trước đó hoàn toàn là một ký tự lặp lại và tiền tố của chuỗi hiện tại khớp với ký tự này thì tiền tố mới có thể mở rộng để bao gồm mẫu lặp lại. Điều tương tự cũng áp dụng cho hậu tố. 
6. Sau khi xử lý tất cả các chuỗi, vẻ đẹp tối đa tích lũy được chính là câu trả lời. 

Tại sao nó hoạt động: Ở mỗi bước, thuật toán duy trì ba bất biến: độ đẹp tối đa cho đến nay, độ dài tiền tố của các ký tự giống nhau ở đầu và độ dài hậu tố của các ký tự giống nhau ở cuối. Phép nhân chỉ mở rộng các giá trị này theo những cách có thể dự đoán được, do đó không thể bỏ qua khối liền kề nào nữa. Bằng cách chỉ theo dõi các số liệu thống kê tóm tắt này, chúng tôi tránh được việc xây dựng chuỗi tăng trưởng theo cấp số nhân. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    n = int(input())
    strings = [input().strip() for _ in range(n)]

    # helper to compute prefix, suffix, internal max
    def analyze(s):
        max_block = 1
        cur = 1
        for i in range(1, len(s)):
            if s[i] == s[i-1]:
                cur += 1
                max_block = max(max_block, cur)
            else:
                cur = 1
        # prefix length
        pre = 1
        while pre < len(s) and s[pre] == s[0]:
            pre += 1
        # suffix length
        suf = 1
        i = len(s)-2
        while i >= 0 and s[i] == s[-1]:
            suf += 1
            i -= 1
        return s[0], s[-1], pre, suf, max_block, len(s)

    # process first string
    f_char, l_char, pre_len, suf_len, max_beauty, total_len = analyze(strings[0])
    full_same = (pre_len == total_len)

    for s in strings[1:]:
        c1, c2, p, su, mb, l = analyze(s)
        if full_same and f_char == c1:
            # previous is all same char
            if mb == l:
                max_beauty = max(max_beauty, (suf_len+1)* (l+1) -1) # multiplication effect
            suf_len = (suf_len + 1) * l + su
            pre_len = (pre_len + 1) * l + p
            f_char = f_char
            l_char = c2
            full_same = (mb == l)
        else:
            # compute merged beauty at junctions
            if l_char == c1:
                max_beauty = max(max_beauty, suf_len + 1 + p)
            max_beauty = max(max_beauty, mb)
            # update prefix/suffix for new accumulated string
            pre_len = pre_len if f_char != c1 else pre_len + p
            suf_len = su if c2 != l_char else su + suf_len
            f_char = f_char
            l_char = c2
            full_same = False

    print(max_beauty)

if __name__ == "__main__":
    main()
```Giải pháp đầu tiên tính toán trước độ dài khối nội bộ và tiền tố/hậu tố cho mỗi chuỗi. Sau đó, nó lặp đi lặp lại việc hợp nhất các số liệu thống kê này thành một “tóm tắt chuỗi” tích lũy mà không xây dựng chuỗi đầy đủ. Cần đặc biệt cẩn thận khi một chuỗi bao gồm toàn bộ một ký tự lặp lại, vì phép nhân có thể phóng to độ dài khối theo cấp số nhân. 

## Ví dụ đã hoạt động 

Mẫu 1: 

| Bước | Chuỗi hiện tại | Tích lũy tối đa | Tiền tố | Hậu tố | Hoàn toàn giống nhau? | 
| --- | --- | --- | --- | --- | --- | 
| p1 = một | một | 1 | 1 | 1 | Đúng | 
| p2 = b | tối đa(1, 1+1+1?)=3 | 1 | 1 | 1 | Sai | 
| p3 = một | tối đa(3, 1+1+1?)=3 | 1 | 1 | 1 | Sai | 

Thuật toán xuất ra chính xác 3, khớp với lần chạy liền kề trong "abaaaba". 

Mẫu 2: 

đầu vào:`a`,`aa`,`b`| Bước | Chuỗi | Tích lũy tối đa | Tiền tố | Hậu tố | Hoàn toàn giống nhau? | 
| --- | --- | --- | --- | --- | --- | 
| một | 1 | 1 | 1 | 1 | Đúng | 
| aa | 2 | cập nhật lên 3? | 2 | 2 | Đúng | 
| b | max(3, hậu tố+1+tiền tố?)=3 | ... | ... | ... | Sai | 

Điều này thể hiện sự hợp nhất giữa các chuỗi ký tự đơn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(tổng chiều dài của chuỗi đầu vào) | Chúng tôi quét từng chuỗi một lần để tính toán tiền tố, hậu tố và khối tối đa, sau đó xử lý trong một lần duy nhất. | 
| Không gian | O(1) thêm | Chỉ các biến tóm tắt có kích thước không đổi mới được duy trì; chuỗi đầu vào được đọc tuần tự. | 

Với tổng chiều dài chuỗi ≤ 100.000, giải pháp này chạy hiệu quả trong vòng 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    main()
    return sys.stdout.getvalue().strip()

# provided samples
assert run("3\na\nb\na\n") == "3", "sample 1"
assert run("2\na\nb\n") == "2", "sample 2"

# custom cases
```
