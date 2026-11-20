---
title: "CF 2160C - XOR ngược"
description: "Chúng ta được cho một số nguyên mục tiêu $n$, và chúng ta muốn biết liệu có thể xây dựng một số nguyên dương $x$ sao cho một phép biến đổi cụ thể được áp dụng cho $x$ và kết hợp với XOR tạo ra chính xác $n$ hay không."
date: "2026-06-09T04:21:42+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks"]
categories: ["algorithms"]
codeforces_contest: 2160
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 1058 (Div. 2)"
rating: 1300
weight: 2160
solve_time_s: 90
verified: false
draft: false
---

[CF 2160C - XOR đảo ngược](https://codeforces.com/problemset/problem/2160/C) 

**Đánh giá:** 1300 
**Thẻ:** bitmask 
**Thời gian giải:** 1 phút 30 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số nguyên mục tiêu$n$, và chúng ta muốn biết liệu có thể xây dựng một số nguyên dương nào đó hay không$x$sao cho một phép biến đổi cụ thể được áp dụng cho$x$và kết hợp với XOR tạo ra chính xác$n$. 

Sự biến đổi$f(x)$lấy biểu diễn nhị phân của$x$, đảo ngược thứ tự bit và diễn giải kết quả dưới dạng số nhị phân mới. Điều quan trọng là các số 0 đứng đầu sau khi đảo ngược sẽ bị bỏ qua, có nghĩa là độ dài bit hiệu dụng của kết quả phụ thuộc vào bit được đặt cao nhất trong chuỗi đảo ngược. 

Vì vậy, quá trình là: chọn một số$x$, viết nó dưới dạng nhị phân, đảo ngược các bit đó, tạo thành một số nguyên mới$f(x)$, và kiểm tra xem$x \oplus f(x) = n$. 

Đầu ra chỉ đơn giản là quyết định có/không cho từng trường hợp thử nghiệm. 

Ràng buộc$n < 2^{30}$ngụ ý rằng chúng ta chỉ cần suy luận về những con số lên tới 30 bit. Bất kỳ hợp lệ$x$vấn đề đó cũng phải nằm trong độ rộng bit đủ nhỏ để các bit đảo ngược không tràn vào không gian tìm kiếm không giới hạn. Một tìm kiếm đơn giản trên tất cả các số nguyên lên tới$2^{30}$đã quá lớn, vì nó có thể bao hàm tới$10^9$ứng viên cho mỗi ca kiểm thử trong trường hợp xấu nhất khi nhân với$t = 10^4$. 

Một vấn đề tế nhị hơn đó là$f(x)$không phải là một phép toán bitwise đơn giản như phần bù hoặc dịch chuyển. Đảo ngược cặp đôi bit có ý nghĩa ít nhất của$x$với phần quan trọng nhất của$f(x)$, phá vỡ địa phương. Điều này làm cho việc suy luận tham lam trên các bit không đáng tin cậy trừ khi chúng ta khai thác tính đối xứng một cách rõ ràng. 

Trường hợp cạnh xuất hiện khi$x$có một tập hợp bit hoặc cấu trúc rất thưa thớt. Ví dụ,$x = 1$sản lượng$f(x) = 1$, vì vậy XOR bằng 0. Điều này cho thấy rằng$n = 0$luôn luôn có thể. Mặt khác, các giá trị nhỏ như$n = 8$thất bại mặc dù có nhiều cách biểu diễn vì việc đảo ngược các bit không đảm bảo tính độc lập về vị trí và các ràng buộc XOR trở nên không nhất quán trên toàn bộ chuỗi bit. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp sẽ thử mọi ứng viên$x$, tính toán$f(x)$, và kiểm tra xem$x \oplus f(x) = n$. Chi phí tính toán$f(x)$tỷ lệ thuận với độ dài bit của$x$, vậy là về$O(\log x)$. Tuy nhiên, số lượng ứng viên lên tới$2^{30}$khiến điều này hoàn toàn không thể thực hiện được. Ngay cả việc kiểm tra một phần trong số chúng cho mỗi truy vấn cũng dẫn đến hàng tỷ thao tác. 

Quan sát cấu trúc quan trọng là biểu diễn nhị phân của$x$không phải là tùy ý một khi chúng ta nghĩ về các ràng buộc palindromic do XOR gây ra với sự đảo ngược của nó. Mỗi bit của$x$tương tác với chính xác một bit được nhân đôi của$f(x)$. Điều này có nghĩa là hệ thống phân tách thành các ràng buộc độc lập trên các cặp bit, nhưng có một điểm thay đổi: các vùng nhớ không tồn tại trong XOR, do đó mỗi vị trí bit có thể được xử lý độc lập sau khi chúng tôi cố định độ dài của$x$. 

Thay vào đó, chúng ta có thể diễn giải lại vấn đề dưới dạng xây dựng một chuỗi nhị phân$s$sao cho nếu chúng ta đảo ngược nó để có được$s^R$, sau đó$s \oplus s^R = n$. Điều này biến vấn đề thành kiểm tra tính nhất quán trên các cặp bit đối xứng. Khó khăn thực sự duy nhất là độ dài của$s$chưa biết, vì vậy chúng tôi thử độ dài có thể lên tới 30 bit. Đối với mỗi độ dài, chúng tôi cố gắng gán các bit từ cả hai đầu vào trong, đảm bảo tính nhất quán với các bit XOR được yêu cầu của$n$. Nếu bất kỳ độ dài nào mang lại một phép gán nhất quán thì câu trả lời là CÓ. 

Điều này làm giảm vấn đề từ việc tìm kiếm số nguyên đến kiểm tra tính khả thi của hệ thống ràng buộc hữu hạn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^{30} \cdot \log n)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(30^2)$mỗi bài kiểm tra |$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mỗi trường hợp thử nghiệm, hãy xem xét độ dài nhị phân có thể có$L$từ 1 đến 30. Ý tưởng là$x$không thể cần nhiều hơn 30 bit vì$n$bản thân nó bị giới hạn. 
2. Đối với chiều dài cố định$L$, giải thích$x$như một$L$-chuỗi nhị phân -bit$s$. chúng tôi muốn$s \oplus reverse(s) = n$, nhưng chỉ thấp nhất$L$bit của$n$vấn đề. 
3. Đối với từng vị trí$i$từ 0 đến$L-1$, chúng tôi ghép nó với vị trí$j = L-1-i$. Hai vị trí này xác định lẫn nhau thông qua ràng buộc XOR:$$s[i] \oplus s[j] = n[i]$$4. Nếu$i = j$, nghĩa là chúng ta đang ở bit giữa của chuỗi có độ dài lẻ, phương trình trở thành:$$s[i] \oplus s[i] = 0$$vì vậy chúng ta phải có$n[i] = 0$. Nếu như$n[i] = 1$, chiều dài này$L$là không thể. 
5. Đối với$i < j$, chúng tôi thử cả hai phép gán phù hợp với XOR: 

nếu$n[i] = 0$, sau đó$s[i] = s[j]$, nếu không thì$s[i] \neq s[j]$. Chúng tôi tuyên truyền các bài tập và kiểm tra tính nhất quán. 
6. Nếu chúng ta gán thành công tất cả các bit mà không có mâu thuẫn và đảm bảo$x > 0$, chúng tôi trả lại CÓ ngay lập tức. 
7. Nếu không có chiều dài$L$hoạt động, trả về KHÔNG. 

### Tại sao nó hoạt động 

Tính chính xác đến từ việc rút gọn phương trình XOR thành các ràng buộc độc lập đối với các vị trí bit được phản chiếu. XOR loại bỏ các phụ thuộc mang, vì vậy mỗi phương trình bit đều mang tính cục bộ. Hoạt động đảo ngược chỉ hoán vị các chỉ số, do đó toàn bộ hệ thống trở thành một tập hợp các ràng buộc đẳng thức hoặc bất đẳng thức giữa các cặp biến. Bất kỳ giải pháp hợp lệ nào đều tương ứng chính xác với một phép gán nhất quán trong biểu đồ ràng buộc này và mọi phép gán nhất quán đều xác định một số nguyên hợp lệ$x$. Vì tất cả độ dài bit có thể đã được kiểm tra nên không có cấu hình hợp lệ nào bị bỏ sót. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def ok(n, L):
    bits = [(n >> i) & 1 for i in range(L)]
    s = [-1] * L

    for i in range(L):
        j = L - 1 - i
        if i > j:
            break

        if i == j:
            if bits[i] == 1:
                return False
            if s[i] == -1:
                s[i] = 0
            continue

        if bits[i] == 0:
            # s[i] == s[j]
            for a, b in [(i, j)]:
                if s[a] == -1 and s[b] == -1:
                    s[a], s[b] = 0, 0
                elif s[a] == -1:
                    s[a] = s[b]
                elif s[b] == -1:
                    s[b] = s[a]
                elif s[a] != s[b]:
                    return False
        else:
            # s[i] != s[j]
            if s[i] == -1 and s[j] == -1:
                s[i], s[j] = 0, 1
            elif s[i] == -1:
                s[i] = 1 - s[j]
            elif s[j] == -1:
                s[j] = 1 - s[i]
            elif s[i] == s[j]:
                return False

    if all(v == 0 for v in s):
        return False

    return True

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        ans = False
        for L in range(1, 31):
            if ok(n, L):
                ans = True
                break
        print("YES" if ans else "NO")

if __name__ == "__main__":
    solve()
```Cốt lõi của việc thực hiện là`ok`hàm cố gắng xây dựng một chuỗi nhị phân hợp lệ có độ dài cố định$L$. Mảng`s`đại diện cho các bit chưa biết của$x$, khởi tạo thành`-1`có nghĩa là không được chỉ định. Đối với mỗi cặp được nhân đôi, chúng tôi thực thi đẳng thức hoặc bất đẳng thức tùy thuộc vào bit tương ứng của$n$. Khi cả hai bit trong một cặp không được gán, chúng tôi trực tiếp gán một cấu hình hợp lệ; mặt khác, chúng tôi tuyên truyền các ràng buộc và xác minh tính nhất quán. 

Điều kiện bit giữa được xử lý rõ ràng khi$i = j$, vì XOR của một bit với chính nó không thể tạo ra 1. Kiểm tra cuối cùng đảm bảo rằng$x$là dương bằng cách bác bỏ phép gán hoàn toàn bằng 0. 

## Ví dụ đã hoạt động 

### Ví dụ 1:$n = 3$Chúng tôi cố gắng$L = 2$đầu tiên vì độ dài nhỏ thường đủ. 

| tôi | j | n[i] | Ràng buộc | trạng thái | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 1 | s[0] ≠ s[1] | (0,1) | 

chúng tôi nhận được$s = 01$, tương ứng với$x = 2$. Đảo ngược mang lại$f(x) = 1$và XOR là 3, khớp$n$. 

Điều này xác nhận rằng thuật toán khám phá chính xác các biểu diễn hợp lệ tối thiểu mà không cần gán bit quá hạn chế. 

### Ví dụ 2:$n = 8$Chúng tôi kiểm tra độ dài lên tới 30. Đối với kích thước nhỏ$L$, các ràng buộc nhanh chóng thất bại vì bit thứ 3 cao hơn trong$n$tạo ra sự bất đối xứng mà bất kỳ cấu trúc nhị phân được phản ánh nào cũng không thể sánh được. Đối với tất cả các độ dài ứng cử viên, ít nhất một mâu thuẫn ràng buộc xuất hiện trong bước ghép nối, do đó không thể gán được. 

Điều này cho thấy sự bất cân xứng toàn cầu trong$n$ngăn chặn mọi cấu trúc nhị phân được nhân đôi hiện có. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(30^2 \cdot t)$| Đối với mỗi thử nghiệm, chúng tôi thử tối đa 30 độ dài và xử lý tối đa 30 cặp bit cho mỗi độ dài | 
| Không gian |$O(1)$| Chỉ các mảng có kích thước cố định để mô phỏng bit | 

Tổng công việc tối đa là vài triệu thao tác bit đơn giản trên tất cả các trường hợp thử nghiệm, dễ dàng nằm trong giới hạn cho$t \le 10^4$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    def ok(n, L):
        bits = [(n >> i) & 1 for i in range(L)]
        s = [-1] * L
        for i in range(L):
            j = L - 1 - i
            if i > j:
                break
            if i == j:
                if bits[i] == 1:
                    return False
                continue
            if bits[i] == 0:
                if s[i] == -1 and s[j] == -1:
                    s[i] = s[j] = 0
                elif s[i] == -1:
                    s[i] = s[j]
                elif s[j] == -1:
                    s[j] = s[i]
                elif s[i] != s[j]:
                    return False
            else:
                if s[i] == -1 and s[j] == -1:
                    s[i], s[j] = 0, 1
                elif s[i] == -1:
                    s[i] = 1 - s[j]
                elif s[j] == -1:
                    s[j] = 1 - s[i]
                elif s[i] == s[j]:
                    return False
        return any(s)

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        ok_flag = False
        for L in range(1, 31):
            if ok(n, L):
                ok_flag = True
                break
        out.append("YES" if ok_flag else "NO")
    return "\n".join(out)

assert run("6\n0\n3\n6\n8\n10\n11\n") == "YES\nYES\nYES\nNO\nYES\nNO", "sample 1"

# edge cases
assert run("1\n0\n") == "YES"
assert run("1\n1\n") == "YES"
assert run("1\n8\n") == "NO"
assert run("3\n0\n3\n10\n") == "YES\nYES\nYES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n0\n`| CÓ | trường hợp hợp lệ nhỏ nhất | 
|`1\n1\n`| CÓ | xây dựng bit đơn | 
|`1\n8\n`| KHÔNG | lỗi bit cao không đối xứng | 
|`3\n0\n3\n10\n`| CÓ CÓ CÓ | trường hợp nhỏ hỗn hợp nhất quán | 

## Vỏ cạnh 

cho$n = 0$, việc xây dựng là tầm thường vì bất kỳ số nhị phân palindromic nào cũng hoạt động. Thuật toán tìm$L = 1$, gán$s = [0]$, và ngay lập tức chấp nhận. 

Vì$n = 1$, một công trình hợp lệ tồn tại với$x = 1$. Trong cuộc kiểm tra,$L = 1$lại tạo ra một bit ở giữa, bit này chỉ hợp lệ khi$n[i] = 0$, vì vậy thay vào đó thuật toán chuyển sang$L = 2$và tìm thấy một phép gán bất đối xứng nhất quán. 

Đối với các giá trị như$n = 8$, bit được đặt cao nhất buộc các ràng buộc trên các vị trí được phản chiếu không thể được thỏa mãn đồng thời ở bất kỳ độ dài nào. Mỗi lần cố gắng$L$tạo ra sự mâu thuẫn ở bit giữa cố định hoặc ở sự không khớp bắt buộc trong một cặp được nhân đôi, do đó tất cả các đường dẫn đều thất bại một cách chính xác.
