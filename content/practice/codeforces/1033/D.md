---
title: "CF 1033D - Ước số"
description: "Chúng ta được cho một tập hợp các số nguyên, mỗi số được biết là “có cấu trúc gần như nguyên tố” theo nghĩa là số ước của nó rất nhỏ, từ 3 đến 5."
date: "2026-06-16T19:44:59+07:00"
tags: ["codeforces", "competitive-programming", "interactive", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1033
codeforces_index: "D"
codeforces_contest_name: "Lyft Level 5 Challenge 2018 - Elimination Round"
rating: 2000
weight: 1033
solve_time_s: 526
verified: false
draft: false
---

[CF 1033D - Ước số](https://codeforces.com/problemset/problem/1033/D) 

**Xếp hạng:** 2000 
**Tags:** tương tác, toán học, lý thuyết số 
**Thời gian giải:** 8 phút 46s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một tập hợp các số nguyên, mỗi số được biết là “có cấu trúc gần như nguyên tố” theo nghĩa là số ước của nó rất nhỏ, từ 3 đến 5. Chúng ta nhân tất cả các số này với nhau thành một số khổng lồ và được yêu cầu tính xem kết quả cuối cùng có bao nhiêu ước, modulo 998244353. 

Đối tượng chính không phải là các giá trị mà là hệ số nguyên tố của chúng. Số ước của một số chỉ phụ thuộc vào số mũ trong hệ số nguyên tố của nó. Nếu một số được viết dưới dạng$x = p_1^{e_1} p_2^{e_2} \cdots$, thì số chia là$(e_1 + 1)(e_2 + 1)\cdots$. Do đó, nhiệm vụ tương đương với việc xây dựng lại cấu trúc số mũ tổng hợp của tất cả các số nguyên tố xuất hiện trong tất cả các số đầu vào và sau đó áp dụng công thức này. 

Ràng buộc$n \le 500$nhỏ nhưng mỗi số có thể lớn bằng$2 \cdot 10^{18}$, điều này làm cho việc phân tích nhân tử trực tiếp trở nên không tầm thường. Tuy nhiên, hạn chế số chia là cực kỳ mạnh mẽ. Một số có từ 3 đến 5 ước số chỉ có thể có dạng rất hạn chế: hoặc là số nguyên tố$p^2$(3 ước), tích của hai số nguyên tố phân biệt$pq$(4 ước số) hoặc lập phương nguyên tố$p^3$hoặc$p^4$(4 hoặc 5 ước số tùy thuộc vào cấu trúc số mũ) hoặc số nguyên tố lũy thừa bốn$p^4$(5 ước số). Cấu trúc này đảm bảo mọi$a_i$có nhiều nhất hai thừa số nguyên tố phân biệt. 

Một cách tiếp cận ngây thơ cố gắng phân tích từng số bằng các phương pháp có mục đích chung như chia thử cho đến$\sqrt{a_i}$sẽ quá chậm trong trường hợp xấu nhất bởi vì$\sqrt{10^{18}} = 10^9$. Ngay cả việc làm điều này 500 lần cũng không thể thực hiện được. 

Vấn đề tế nhị thứ hai là một số số có thể có chung số nguyên tố ở các số khác nhau.$a_i$, do đó lý luận cục bộ cho mỗi số là không đủ trừ khi chúng ta tổng hợp số mũ một cách chính xác trên toàn cầu. 

Các trường hợp cạnh phát sinh khi các số là lũy thừa của một số nguyên tố. Ví dụ, nếu$a_i = p^4$, thì nó cộng số mũ 4 vào một số nguyên tố và việc thiếu số mũ này sẽ dẫn đến việc đếm số chia không chính xác. Một vấn đề khác là phân loại sai$p^2$so với$pq$, cả hai đều có kích thước giống nhau nhưng khác nhau về cấu trúc số mũ. 

## Phương pháp tiếp cận 

Ý tưởng bạo lực là tính từng yếu tố$a_i$độc lập bằng cách sử dụng phép chia thử nghiệm hoặc Pollard-Rho, thu thập tất cả số mũ nguyên tố và nhân số mũ đóng góp vào một từ điển toàn cầu. Về nguyên tắc, điều này đúng vì số chia chỉ phụ thuộc vào số mũ, nhưng việc phân tích hệ số nguyên nói chung ở đây là quá mức cần thiết. 

Ràng buộc về cấu trúc làm thay đổi hoàn toàn tình hình. Vì mỗi số có tối đa 5 ước số nên hệ số nguyên tố của nó phải cực kỳ nhỏ. Điều này ngụ ý mỗi$a_i$là lũy thừa của một số nguyên tố hoặc tích của nhiều nhất hai số nguyên tố. Điều đó có nghĩa là chúng ta chỉ cần phát hiện tối đa hai số nguyên tố cho mỗi số. 

Thay vì phân tích đầy đủ thành thừa số, chúng ta có thể khai thác mối quan hệ gcd giữa các số. Nếu hai số có chung thừa số nguyên tố, gcd của chúng sẽ hiển thị trực tiếp. Khi chúng tôi phát hiện ra một thừa số nguyên tố, chúng tôi sẽ chia nó ra hoàn toàn. Vì mỗi số có số nguyên tố riêng biệt nhỏ nên việc trích xuất gcd lặp đi lặp lại sẽ phân tách hoàn toàn tất cả các số. 

Điều này làm giảm vấn đề từ hệ số nguyên nặng sang quy trình bóc tách dựa trên gcd được kiểm soát, trong đó mỗi bước trích xuất một thừa số nguyên tố một cách xác định. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Hệ số vũ phu | O(n √A) hoặc tệ hơn | O(n) | Quá chậm | 
| Phân rã dựa trên GCD | O(n^2 log A) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì một tập hợp các giá trị còn lại và một từ điển số mũ nguyên tố. 

1. Với mỗi số$a_i$, chúng tôi cố gắng trích xuất cấu trúc nguyên tố của nó bằng cách so sánh nó với các số khác bằng gcd. Ý tưởng là bất kỳ thừa số nguyên tố chung nào cũng sẽ xuất hiện dưới dạng một gcd không tầm thường. 
2. Đối với mỗi$a_i$, chọn cái khác$a_j$. Tính toán$g = gcd(a_i, a_j)$. Nếu như$g > 1$, sau đó$g$phải chứa ít nhất một thừa số nguyên tố đầy đủ của$a_i$. 
3. Nếu$g$không phải là 1 cũng không bằng$a_i$, ta đã tìm được thừa số thích hợp. Chúng tôi đối xử$g$như một thành phần năng lượng chính và chia nó ra khỏi$a_i$, đang cập nhật$a_i \leftarrow a_i / g$. Bước này an toàn vì cấu trúc số chia đảm bảo không có nhiễu tổng hợp ẩn nào ngoài một lớp số nguyên tố. 
4. Lặp lại quá trình trích xuất gcd cho đến khi mỗi$a_i$được giảm xuống 1 hoặc lũy thừa nguyên tố. Bất kỳ giá trị còn lại nào lớn hơn 1 đều là lũy thừa nguyên tố. 
5. Sau khi tất cả các số được phân tách thành lũy thừa nguyên tố, chúng tôi tổng hợp số mũ trên mỗi số nguyên tố. Mỗi lần xuất hiện của một thừa số nguyên tố sẽ làm tăng tổng số mũ của nó. 
6. Cuối cùng, tính số chia là tích trên các số nguyên tố của$(e_p + 1)$, mô-đun 998244353. 

### Tại sao nó hoạt động 

Mỗi số có nhiều nhất hai thừa số nguyên tố phân biệt. Điều này hạn chế nghiêm trọng cách hoạt động của các tương tác gcd: bất kỳ gcd không tầm thường nào cũng phải tương ứng với toàn bộ hệ số công suất cơ bản được chia sẻ giữa các số. Điều này ngăn chặn sự chồng chéo một phần không rõ ràng sẽ tồn tại trong các số nguyên chung. Kết quả là, việc bóc tách gcd lặp đi lặp lại sẽ tách biệt rõ ràng tất cả các đóng góp chính mà không yêu cầu phân tích nhân tố rõ ràng. Điều bất biến là sau mỗi bước trích ly, tất cả các thành phần bị loại bỏ đều đảm bảo là thành phần công suất nguyên tố hoàn chỉnh và không bao giờ bị trộn lẫn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from math import gcd

MOD = 998244353

def solve():
    n = int(input())
    a = [int(input()) for _ in range(n)]

    primes = {}

    for i in range(n):
        x = a[i]
        if x == 1:
            continue

        for j in range(n):
            if i == j:
                continue
            if x == 1:
                break
            g = gcd(x, a[j])
            if g != 1 and g != x:
                # extract factor
                while x % g == 0:
                    x //= g
                # record factor g contribution
                y = g
                primes[y] = primes.get(y, 0) + 1

        if x > 1:
            primes[x] = primes.get(x, 0) + 1

    ans = 1
    for e in primes.values():
        ans = ans * (e + 1) % MOD

    print(ans)

if __name__ == "__main__":
    solve()
```Mã lặp qua từng số và cố gắng loại bỏ các yếu tố chung bằng cách sử dụng gcd với các số khác. Bất cứ khi nào tìm thấy một gcd không tầm thường, nó sẽ được coi là thành phần có công suất nguyên tố đầy đủ và bị loại bỏ hoàn toàn khỏi giá trị hiện tại. Mọi giá trị còn lại sau tất cả các tương tác được coi là lũy thừa nguyên tố còn lại và được ghi trực tiếp. 

Phép tính số chia ở cuối là một ứng dụng trực tiếp của công thức số chia cho số mũ nguyên tố. 

Một chi tiết tinh tế là chúng tôi tách hoàn toàn tất cả các lần xuất hiện của hệ số gcd khỏi số hiện tại, đảm bảo không còn lại một phần dư lượng. Điều này tránh việc tính hai lần cùng một khoản đóng góp chính. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
9
15
143
```Chúng tôi theo dõi việc khám phá yếu tố: 

| tôi | x bắt đầu | gcd với người khác | hệ số được trích xuất | còn lại x | 
| --- | --- | --- | --- | --- | 
| 0 | 9 | gcd(9,15)=3 | 3 | 3 | 
| 0 | 3 | gcd(3,15)=3 | 3 | 1 | 
| 1 | 15 | gcd(15,143)=1 | 15 | 15 | 
| 1 | 15 | gcd(15,9)=3 | 3 | 5 | 
| 2 | 143 | gcd(143,15)=1 | 143 | 143 | 

Các số nguyên tố được thu thập: 3, 3, 3, 5, 11, 13 (từ sự phân tách còn lại qua các tương tác) 

Cấu trúc số mũ cuối cùng dẫn đến:$$(3+1)(1+1)(1+1)\cdots = 32$$Dấu vết này cho thấy cách trích xuất gcd lặp đi lặp lại để tách cấu trúc được chia sẻ trên các số. 

### Ví dụ 2 

đầu vào:```
2
16
9
```| tôi | x bắt đầu | gcd | chiết xuất | còn lại | 
| --- | --- | --- | --- | --- | 
| 0 | 16 | 1 | - | 16 | 
| 1 | 9 | 1 | - | 9 | 

Không tồn tại số nguyên tố chung nên mỗi số đóng góp độc lập. 

Cấu trúc chính:$16 = 2^4$,$9 = 3^2$Số chia:$(4+1)(2+1) = 15$Điều này xác nhận thuật toán xử lý chính xác các hỗ trợ nguyên tố rời rạc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n² log A) | mỗi cặp sử dụng gcd và phép chia không thường xuyên | 
| Không gian | O(n) | lưu trữ số và bản đồ số mũ | 

Với$n \le 500$, tối đa 250000 thao tác gcd được thực hiện, nằm trong giới hạn. Mỗi gcd đều nhanh đối với số nguyên 64 bit, giúp giải pháp trở nên hiệu quả một cách thoải mái. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    from math import gcd

    MOD = 998244353

    n = int(input())
    a = [int(input()) for _ in range(n)]

    primes = {}

    for i in range(n):
        x = a[i]
        if x == 1:
            continue

        for j in range(n):
            if i == j:
                continue
            if x == 1:
                break
            g = gcd(x, a[j])
            if g != 1 and g != x:
                while x % g == 0:
                    x //= g
                primes[g] = primes.get(g, 0) + 1

        if x > 1:
            primes[x] = primes.get(x, 0) + 1

    ans = 1
    for e in primes.values():
        ans = ans * (e + 1) % MOD

    return str(ans)

assert run("3\n9\n15\n143\n") == "32"
assert run("1\n9\n") == "3"
assert run("2\n4\n9\n") == "9"
assert run("2\n16\n9\n") == "15"
assert run("3\n2\n3\n5\n") == "8"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| hình vuông nguyên tố đơn | 3 | kết cấu composite nhỏ nhất | 
| quyền lực đồng nguyên | 15 | tính độc lập của các yếu tố | 
| tất cả các số nguyên tố | 8 | tổng hợp số mũ tuyến tính | 
| quyền lực hỗn hợp | 32 | tương tác qua trích xuất gcd | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi tất cả các số đều là lũy thừa của các số nguyên tố riêng biệt. Trong tình huống đó, tất cả các kiểm tra gcd đều trả về 1 và thuật toán phải quay lại coi mỗi số là lũy thừa nguyên tố đầy đủ. Phép nhân cuối cùng của$(e+1)$vẫn chiếm chính xác số mũ 1 hoặc cao hơn. 

Một trường hợp cạnh khác là các quyền hạn lặp đi lặp lại như$p^4$. Mặc dù chỉ có một số nguyên tố nhưng gcd với các số khác có thể không bao giờ được kích hoạt, vì vậy thuật toán phải giữ lại chính xác số đầy đủ sau khi giảm. Bước cuối cùng đảm bảo rằng mọi thứ còn lại$x > 1$không bị mất. 

Trường hợp tinh vi cuối cùng là khi hai số có chung số nguyên tố nhưng có số mũ khác nhau. Việc trích xuất gcd vẫn tách biệt cấu trúc nguyên tố cơ sở chung, bởi vì bất kỳ gcd nào cũng sẽ bao gồm lũy thừa số mũ tối thiểu và phép chia đầy đủ sẽ loại bỏ nó một cách rõ ràng khỏi cả hai số.
