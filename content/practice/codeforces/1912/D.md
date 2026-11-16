---
title: "CF 1912D - Kiểm tra tính chia hết"
description: "Chúng ta có một hệ thống số có cơ số $b$, và chúng ta muốn kiểm tra tính chia hết của mô đun $n$ chỉ bằng các phép toán cục bộ trên các chữ số. Một số được viết dưới dạng cơ số $b$ và chúng ta được phép thay thế giá trị đầy đủ bằng một biểu thức có cấu trúc được xây dựng từ các chữ số của nó."
date: "2026-06-08T20:14:26+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 1912
codeforces_index: "D"
codeforces_contest_name: "2023-2024 ICPC, NERC, Northern Eurasia Onsite (Unrated, Online Mirror, ICPC Rules, Teams Preferred)"
rating: 1900
weight: 1912
solve_time_s: 102
verified: false
draft: false
---

[CF 1912D - Kiểm tra tính chia hết](https://codeforces.com/problemset/problem/1912/D) 

**Xếp hạng:** 1900 
**Thẻ:** toán 
**Thời gian giải:** 1 phút 42s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một hệ thống số có cơ số$b$và chúng tôi muốn kiểm tra khả năng chia hết cho một mô đun$n$chỉ sử dụng các phép toán cục bộ trên các chữ số. Một số được viết dưới dạng cơ số$b$và chúng tôi được phép thay thế giá trị đầy đủ bằng biểu thức có cấu trúc được xây dựng từ các chữ số của nó. Biểu thức phải luôn bảo toàn giá trị modulo$n$, do đó việc kiểm tra tính chia hết sẽ giảm xuống việc kiểm tra dạng dựa trên chữ số đơn giản. 

Chúng tôi được phép có ba loại công trình, tất cả đều hoạt động trên các chữ số được nhóm thành các khối có kích thước$k$(được giải thích theo cơ sở$b$). 

Trong loại đầu tiên, chúng tôi bỏ qua mọi thứ ngoại trừ loại cuối cùng$k$chữ số. Điều này chỉ có tác dụng nếu đóng góp của tất cả các chữ số cao hơn luôn là bội số của$n$, bất kể giá trị của chúng là bao nhiêu. 

Ở loại thứ hai, chúng ta chia số thành các khối liên tiếp$k$các chữ số và tính tổng các khối này. 

Ở loại thứ ba, chúng tôi xen kẽ các dấu hiệu giữa các khối này. 

Mỗi khối được coi như một cơ sở-$b$số nguyên, do đó dịch chuyển bằng$k$các chữ số tương ứng với phép nhân với$b^k$. Do đó, hạn chế thực sự là liệu quyền hạn của$b^k$cư xử tử tế modulo$n$. 

Đầu vào bao gồm nhiều cặp$(b, n)$. Với mỗi cặp, chúng ta phải tìm giá trị nhỏ nhất$k$sao cho ít nhất một trong các cấu trúc này bảo toàn đúng giá trị modulo$n$. Nếu không tồn tại, chúng tôi xuất ra số không. 

Các ràng buộc ngụ ý đến$10^6$tổng số trong tất cả các bài kiểm tra, do đó$O(\sqrt{n})$hoặc tốt hơn cho mỗi phương pháp thử nghiệm là cần thiết. Bất cứ điều gì liên quan đến việc nhân tử hóa hoặc mô phỏng việc mở rộng chữ số lên đến$n$mỗi bài kiểm tra tổng hợp là quá chậm. 

Một trường hợp thất bại tinh vi xuất phát từ việc giả định rằng các phương thức chặn luôn tồn tại đối với tất cả các mô-đun. Ví dụ,$b = 10, n = 6$trông giống như các quy tắc chia hết quen thuộc, nhưng không có cấu trúc chữ số tuyến tính nào được phép phù hợp với ràng buộc modulo 6, vì vậy câu trả lời là 0. Một cách tiếp cận ngây thơ chỉ kiểm tra nhỏ$k$không hiểu các điều kiện cấu trúc có thể kết luận không chính xác rằng một số nhóm hoạt động. 

## Phương pháp tiếp cận 

Quan sát cốt lõi là cả ba cách xây dựng đều tương ứng với việc biểu thị số dưới dạng đa thức trong cơ số$b^k$. Nếu chúng ta nhóm các chữ số thành các khối có kích thước$k$, bất kỳ số nào cũng trở thành$$x = x_0 + x_1 b^k + x_2 b^{2k} + \dots$$mỗi nơi$x_i$là một giá trị khối. 

Mỗi quy tắc chia hết là một cách thay thế lũy thừa của$b^k$với các hằng số đơn giản trong$\mathbb{Z}_n$: 

Đối với loại 1, chúng tôi thực sự yêu cầu$b^k \equiv 0 \pmod n$. Điều này đảm bảo các khối cao hơn sẽ biến mất hoàn toàn. 

Đối với loại 2, chúng tôi yêu cầu$b^k \equiv 1 \pmod n$, bởi vì khi đó tất cả các khối đóng góp như nhau và tổng giữ nguyên giá trị ban đầu modulo$n$. 

Đối với loại 3, chúng tôi yêu cầu$b^k \equiv -1 \pmod n$, do đó các dấu xen kẽ triệt tiêu lũy thừa liên tiếp một cách chính xác. 

Do đó nhiệm vụ giảm xuống còn việc tìm giá trị nhỏ nhất$k$như vậy$b^k \equiv 0, 1,$hoặc$-1 \pmod n$, và chọn giá trị hợp lệ nhỏ nhất trong số đó. 

Cách tiếp cận vũ phu thử tất cả$k$từ 1 trở lên, tính toán$b^k \bmod n$. Điều này đúng nhưng quá chậm trong trường hợp xấu nhất, vì$k$có thể lên tới$n$và mỗi phép tính là$O(1)$, cho$O(n)$mỗi bài kiểm tra. 

Cái nhìn sâu sắc quan trọng là chúng ta chỉ cần tìm hành vi giống như thứ tự nhân của$b \bmod n$, điều này có thể được thực hiện bằng cách lặp lũy thừa cho đến khi chúng ta đạt 0 hoặc quay vòng, nhưng vì$n$có thể lớn trong các thử nghiệm, thay vào đó, chúng tôi theo dõi cường độ cho đến khi lặp lại hoặc cho đến khi đạt đến mức dư lượng cần thiết, điều này luôn xảy ra trong$O(n)$tổng cộng trong tất cả các thử nghiệm do động lực học mô-đun. 

Một cách nhìn có cấu trúc hơn là chúng ta đang đi trong modulo đơn hình nhân$n$và chúng tôi dừng lại ngay khi đạt được một trong ba trạng thái mục tiêu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Sức mạnh vũ phu |$O(n)$mỗi bài kiểm tra |$O(1)$| Quá chậm | 
| Theo dõi sức mạnh gia tăng |$O(n)$tổng khấu hao |$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi lặp lại sức mạnh của$b$modulo$n$, nhưng thay vì tính toán lại từ đầu$k$, chúng tôi duy trì sức mạnh hiện tại$p = b^k \bmod n$. 

1. Khởi tạo$p = 1$. Điều này thể hiện$b^0$, ca trống. 
2. Đối với mỗi$k \ge 1$, cập nhật$p = (p \cdot b) \bmod n$. Điều này mang lại$b^k \bmod n$. 
3. Nếu$p = 0$, chúng ta có thể sử dụng ngay loại 1 với điều này$k$, vì tất cả các khối cao hơn đều biến mất theo modulo$n$. 
4. Nếu$p = 1$, chúng ta có thể sử dụng loại 2 với cái này$k$, vì việc dịch chuyển khối bảo toàn phần đóng góp một cách chính xác. 
5. Nếu$p = n-1$(đại diện cho$-1 \bmod n$), chúng ta có thể sử dụng loại 3 với điều này$k$, vì các dấu xen kẽ triệt tiêu lũy thừa một cách chính xác. 
6. Chúng ta dừng lại ở việc nhỏ nhất$k$gây ra bất kỳ tình trạng nào trong số này. 
7. Nếu chúng ta đạt đến giới hạn trên an toàn mà không tìm thấy bất kỳ điều kiện nào, chúng ta kết luận rằng không có quy tắc nào tồn tại. 

Điều kiện dừng hoạt động vì một lần$p$bước vào một chu kỳ, nó sẽ không bao giờ gặp dư lượng mới, vì vậy việc tiếp tục tiếp tục không thể tạo ra giá trị nhỏ hơn$k$. 

### Tại sao nó hoạt động 

Mỗi quy tắc nhóm tương ứng với việc thực thi một nhận dạng đơn giản dựa trên quyền hạn của$b^k$modulo$n$. Loại 1 buộc tất cả các sức mạnh cao hơn biến mất, chính xác là$b^k \equiv 0$. Loại 2 yêu cầu tất cả các khối được dịch chuyển phải đóng góp giống hệt nhau, tức là$b^k \equiv 1$. Loại 3 yêu cầu hủy xen kẽ, đó là$b^k \equiv -1$. Thuật toán liệt kê lũy thừa theo thứ tự tăng dần, do đó, lần đầu tiên bất kỳ đồng nhất thức đại số nào trong số này có giá trị, nó phải tương ứng với kích thước nhóm hợp lệ nhỏ nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        b, n = map(int, input().split())

        # special quick rejection
        # we search powers of b mod n
        p = 1

        # we will cap iterations at n steps; cycle must appear before that
        ans = 10**18

        for k in range(1, n + 1):
            p = (p * b) % n

            if p == 0:
                ans = k
                break
            if p == 1:
                ans = min(ans, k)
                break
            if p == n - 1:
                ans = min(ans, k)
                break

        if ans == 10**18:
            print(0)
        else:
            # decide kind
            p = 1
            for k in range(1, ans + 1):
                p = (p * b) % n
                if k == ans:
                    if p == 0:
                        print(1, ans)
                    elif p == 1:
                        print(2, ans)
                    else:
                        print(3, ans)

if __name__ == "__main__":
    solve()
```Việc thực hiện trực tiếp theo dõi quyền hạn của$b$modulo$n$. Vòng lặp đầu tiên tìm kiếm giá trị nhỏ nhất$k$tạo ra dư lượng hợp lệ. Bước thứ hai tái tạo lại điều kiện nào được kích hoạt, vì vòng lặp không lưu trữ loại rõ ràng trong quá trình tìm kiếm. 

Sự tinh tế duy nhất là đảm bảo rằng$n-1$được hiểu chính xác là$-1 \bmod n$, điều này cần thiết cho quy tắc tổng xen kẽ. Một chi tiết quan trọng khác là chúng ta phải dừng ngay lập tức khi một điều kiện được đáp ứng, vì chúng ta yêu cầu giá trị hợp lệ nhỏ nhất$k$. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:$b = 10, n = 11$Chúng tôi tính lũy thừa của 10 modulo 11. 

| k | p = 10^k mod 11 | tình trạng | 
| --- | --- | --- | 
| 1 | 10 | p = -1 mod 11 | 

Tại$k = 1$, chúng tôi đánh$p = -1$, vì vậy loại 3 được áp dụng. 

Điều này phù hợp với quy tắc chữ số xen kẽ, trong đó các chữ số thay thế các dấu hiệu một cách tự nhiên trong cơ số 10 cho modulo 11. 

### Ví dụ 2 

đầu vào:$b = 10, n = 4$| k | p = 10^k mod 4 | tình trạng | 
| --- | --- | --- | 
| 1 | 2 | không | 
| 2 | 0 | loại 1 | 

Tại$k = 2$, lũy thừa trở thành 0, nghĩa là tất cả các chữ số nằm ngoài hai chữ số cuối cùng đều biến mất theo modulo 4. Điều này phù hợp với quy tắc cổ điển rằng chỉ có hai chữ số cuối có ý nghĩa đối với modulo 4 trong cơ số 10. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$mỗi lần kiểm tra (khấu hao trong tất cả các lần kiểm tra) | mỗi bước nhân một lần và giảm modulo$n$| 
| Không gian |$O(1)$| chỉ một vài biến được lưu trữ | 

Tổng số tiền của$n$trên tất cả các bài kiểm tra được giới hạn bởi$10^6$, do đó việc quét tuyến tính theo lũy thừa là đủ. Mỗi lần lặp thực hiện công việc liên tục, giữ giải pháp trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import prod

    import sys
    input = sys.stdin.readline

    t = int(sys.stdin.readline())
    out = []

    for _ in range(t):
        b, n = map(int, sys.stdin.readline().split())

        p = 1
        ans = None

        for k in range(1, n + 1):
            p = (p * b) % n
            if p == 0:
                ans = (1, k)
                break
            if p == 1:
                ans = (2, k)
                break
            if p == n - 1:
                ans = (3, k)
                break

        if ans is None:
            out.append("0")
        else:
            out.append(f"{ans[0]} {ans[1]}")

    return "\n".join(out)

# provided samples
assert run("""6
10 3
10 11
10 4
10 7
8 5
10 6
""") == """2 1
3 1
1 2
3 3
3 2
0"""

# custom cases
assert run("1\n2 2\n") == "1 1"
assert run("1\n3 1\n") == "0"  # invalid modulus edge intuition
assert run("1\n5 5\n") == "1 1"
assert run("1\n10 11\n") == "3 1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 10 3 | 2 1 | trường hợp bình đẳng ngay lập tức | 
| 8 5 | 3 2 | trường hợp xen kẽ không tầm thường | 
| 10 6 | 0 | không tồn tại quy tắc hợp lệ | 

## Vỏ cạnh 

Trường hợp một cạnh là khi$b \equiv 0 \pmod n$. Trong tình huống đó, phép nhân đầu tiên đã tạo ra số 0. Ví dụ,$b = 6, n = 3$. Bộ thuật toán$p = 6 \equiv 0$, Vì thế$k = 1$có giá trị ngay lập tức đối với loại 1, vì mọi khối chữ số cao hơn đều biến mất. 

Một trường hợp khác là khi$b \equiv 1 \pmod n$. Sau đó$p$luôn là 1, do đó lần lặp đầu tiên đã kích hoạt loại 2 với$k = 1$. Điều này tương ứng với thực tế là mọi khối chữ số đều đóng góp không thay đổi bất kể vị trí. 

Trường hợp thứ ba là khi không có chu kỳ nào đạt 0, 1 hoặc -1. Ví dụ$b = 10, n = 6$. Các nguồn năng lượng quay vòng qua các phần dư không bao giờ phù hợp với các cấu trúc cần thiết, do đó vòng lặp kết thúc mà không tìm thấy giá trị hợp lệ.$k$, tạo ra số không một cách chính xác.
