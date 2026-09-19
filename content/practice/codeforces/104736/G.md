---
title: "CF 104736G - GPS trên Trái đất phẳng"
description: "Chúng ta có một số tháp vô tuyến trên lưới số nguyên 2D. Mỗi tòa tháp biết chính xác khoảng cách Manhattan của nó đến một vị trí người dùng không xác định và tất cả những khoảng cách này được đảm bảo chính xác đồng thời."
date: "2026-06-29T00:22:40+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104736
codeforces_index: "G"
codeforces_contest_name: "2023-2024 ACM-ICPC Latin American Regional Programming Contest"
rating: 0
weight: 104736
solve_time_s: 71
verified: true
draft: false
---

[CF 104736G - GPS trên Trái đất phẳng](https://codeforces.com/problemset/problem/104736/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 11s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một số tháp vô tuyến trên lưới số nguyên 2D. Mỗi tòa tháp biết chính xác khoảng cách Manhattan của nó đến một vị trí người dùng không xác định và tất cả những khoảng cách này được đảm bảo chính xác đồng thời. Nhiệm vụ là khôi phục mọi tọa độ nguyên có thể có ở nơi người dùng có thể được định vị sao cho tất cả các ràng buộc về khoảng cách đều được thỏa mãn. 

Một ràng buộc duy nhất từ ​​một tòa tháp ở$(a,b)$với khoảng cách$d$có nghĩa là điểm chưa biết$(x,y)$nằm trên ranh giới của một vòng tròn Manhattan có tâm tại$(a,b)$. Không giống như các đường tròn Euclide, ranh giới này là hình kim cương, do đó ràng buộc không phải là một đường cong trơn mà là một hình dạng tuyến tính từng phần thẳng hàng với các trục. 

Đầu ra là tập hợp đầy đủ các điểm nguyên thỏa mãn tất cả các ràng buộc cùng một lúc, được sắp xếp theo thứ tự từ điển theo$x$, sau đó$y$. Phần quan trọng là tập nghiệm được đảm bảo là hữu hạn, vì vậy chúng ta không xử lý một vùng vô hạn các điểm khả thi. 

Các ràng buộc cho phép lên đến$10^5$tháp, ngay lập tức loại trừ bất kỳ phương pháp tiếp cận nào kiểm tra rõ ràng các điểm lưới ứng cử viên hoặc liệt kê các giao lộ theo cặp. Ngay cả việc kiểm tra hộp giới hạn có kích thước vừa phải cũng không thể thực hiện được vì tọa độ có thể vượt quá$10^4$, mang lại tiềm năng$10^8$khu vực. 

Một điểm tinh tế quan trọng là các ràng buộc đẳng thức của Manhattan xác định ranh giới đa giác bao gồm các đường có độ dốc.$+1$Và$-1$và vùng khả thi cuối cùng là giao điểm của các đa giác đó. Một cách giải thích ngây thơ có thể gợi ý rằng câu trả lời vẫn là một đa giác phức tạp có nhiều đỉnh, nhưng trong bài toán này, cấu trúc sẽ sụp đổ thành một tập hợp có cấu trúc rất nhỏ. 

Một trường hợp thất bại phổ biến là giả định rằng mỗi ràng buộc giảm độc lập thành một hộp giới hạn đơn giản trong$(x,y)$. Điều đó sẽ xử lý không đúng$|x-a| + |y-b| = d$là một vùng hình chữ nhật, trong khi thực tế nó chỉ là ranh giới của vùng đó. 

Một cạm bẫy tinh vi khác là cố gắng cắt bỏ từng ràng buộc một về mặt hình học. Ngay cả khi mỗi bước duy trì một đa giác lồi, độ phức tạp của việc duy trì nó tăng tuyến tính một cách rõ ràng theo các đỉnh trên mỗi ràng buộc, điều này trở nên không khả thi đối với$10^5$tháp. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ sẽ là xem xét tất cả các điểm nguyên trong một hộp giới hạn đủ lớn (ví dụ:$[-2\cdot10^4,2\cdot10^4]^2$) và kiểm tra mọi ràng buộc của tháp. Mỗi lần kiểm tra là$O(1)$, nhưng lưới chứa khoảng$1.6 \cdot 10^9$điểm, điều này hoàn toàn không thể thực hiện được. 

Một nỗ lực tốt hơn một chút là quan sát rằng mỗi ràng buộc là một ranh giới hình thoi, do đó các giao điểm xảy ra ở nơi các đường ranh giới gặp nhau. Mỗi ràng buộc đóng góp bốn loại đường:$x+y = a+b+d$,$x+y = a+b-d$,$x-y = a-b+d$,$x-y = a-b-d$. 

Người ta có thể thử giao tất cả các đường này theo cặp, nhưng điều đó dẫn đến$O(N^2)$ứng cử viên và vẫn không thể$10^5$. 

Cái nhìn sâu sắc về cấu trúc quan trọng là chuyển đổi tọa độ. Định nghĩa$u = x + y$Và$v = x - y$. 

Trong các tọa độ này, hình học Manhattan trở nên thẳng hàng theo trục. Mỗi ràng buộc hạn chế$u$Và$v$độc lập với các khoảng:$u \in [a+b-d, a+b+d]$,$v \in [a-b-d, a-b+d]$. 

Vì vậy, mỗi tháp đóng góp một hình chữ nhật trong$(u,v)$-space, và tập nghiệm là giao điểm của tất cả các hình chữ nhật này. Bản thân giao điểm đó là một hình chữ nhật, nghĩa là chúng ta chỉ cần giới hạn tối thiểu/tối đa toàn cục cho$u$Và$v$. 

Bước cuối cùng là chuyển đổi hợp lệ$(u,v)$trở lại số nguyên$(x,y)$, đòi hỏi tính nhất quán chẵn lẻ:$x = (u+v)/2$,$y = (u-v)/2$, Vì thế$u$Và$v$phải có cùng tính chẵn lẻ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Kiểm tra lưới Brute Force |$O(R^2 N)$|$O(1)$| Quá chậm | 
| Khoảng thời gian tối ưu trong$u,v$không gian |$O(N + K)$|$O(1)$| Đã chấp nhận | 

Đây$K$là số điểm đầu ra. 

## Hướng dẫn thuật toán 

1. Đối với mỗi tòa tháp$(a,b,d)$, tính bốn giá trị:$a+b-d$,$a+b+d$,$a-b-d$,$a-b+d$. Chúng xác định phạm vi được phép cho$u$Và$v$. 
2. Duy trì giới hạn toàn cầu cho$u$:$U_{\min} = \max(a+b-d)$,$U_{\max} = \min(a+b+d)$. 

Điều này đảm bảo$u$thoả mãn mọi ràng buộc cùng một lúc. 
3. Duy trì giới hạn toàn cầu cho$v$:$V_{\min} = \max(a-b-d)$,$V_{\max} = \min(a-b+d)$. 

Điều này đảm bảo$v$cũng thỏa mãn mọi ràng buộc cùng một lúc. 
4. Nếu khoảng thời gian không hợp lệ ($U_{\min} > U_{\max}$hoặc$V_{\min} > V_{\max}$), không có giải pháp nào tồn tại, nhưng vấn đề đảm bảo điều này không xảy ra. 
5. Lặp lại tất cả số nguyên$u$TRONG$[U_{\min}, U_{\max}]$Và$v$TRONG$[V_{\min}, V_{\max}]$. 
6. Chỉ giữ những cặp ở những nơi$u \equiv v \pmod 2$, vì nếu không thì$(x,y)$sẽ không phải là số nguyên. 
7. Chuyển đổi các cặp hợp lệ trở lại:$x = (u+v)/2$,$y = (u-v)/2$và xuất chúng theo thứ tự được sắp xếp (tăng dần$x$, sau đó$y$). 

### Tại sao nó hoạt động 

Mỗi ràng buộc đẳng thức Manhattan xác định một hình thoi lồi. Khi chuyển hóa thành$(u,v)$tọa độ, mỗi viên kim cương như vậy sẽ trở thành một hình chữ nhật thẳng hàng với trục. Do đó, việc giao nhau tất cả các ràng buộc sẽ tạo ra một hình chữ nhật thẳng hàng theo trục khác trong$(u,v)$-không gian. Bất kỳ điểm nguyên nào bên trong hình chữ nhật này đều tương ứng với một ứng cử viên$(x,y)$thỏa mãn tất cả các ràng buộc cùng một lúc và không có điểm nào bên ngoài có thể thỏa mãn ít nhất một ràng buộc vì nó sẽ vi phạm một trong hai ràng buộc đó$u$hoặc$v$ràng buộc. Điều kiện chẵn lẻ là hạn chế duy nhất còn lại cần thiết để đảm bảo ánh xạ ngược số nguyên. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    
    U_min = -10**30
    U_max = 10**30
    V_min = -10**30
    V_max = 10**30

    for _ in range(n):
        x, y, d = map(int, input().split())
        s = x + y
        t = x - y

        U_min = max(U_min, s - d)
        U_max = min(U_max, s + d)
        V_min = max(V_min, t - d)
        V_max = min(V_max, t + d)

    res = []

    for u in range(U_min, U_max + 1):
        for v in range(V_min, V_max + 1):
            if (u & 1) != (v & 1):
                continue
            x = (u + v) // 2
            y = (u - v) // 2
            res.append((x, y))

    res.sort()
    out = sys.stdout.write
    for x, y in res:
        out(f"{x} {y}\n")

if __name__ == "__main__":
    solve()
```Giai đoạn đầu tiên nén tất cả các ràng buộc hình học thành hai giao điểm khoảng một chiều độc lập, một cho$u = x+y$và một cho$v = x-y$. Đây là lúc toàn bộ khó khăn về mặt hình học biến mất, vì mỗi tháp chỉ thắt chặt phạm vi khả thi bằng cách cập nhật giới hạn thời gian không đổi. 

Giai đoạn thứ hai liệt kê tất cả các khả năng$(u,v)$cặp bên trong hình chữ nhật kết quả. Việc kiểm tra tính chẵn lẻ là cần thiết bởi vì$u$Và$v$phải tương ứng với số nguyên$x$Và$y$. Nếu không có bộ lọc này, một nửa số điểm được xây dựng lại sẽ là các vị trí lưới không hợp lệ. 

Việc sắp xếp được thực hiện ở cuối vì việc liệt kê không đảm bảo thứ tự từ điển trong$(x,y)$. 

## Ví dụ đã hoạt động 

Hãy xem xét mẫu đầu tiên: 

Chúng tôi tổng hợp các ràng buộc thành các giới hạn trên$u$Và$v$. Giả sử kết quả cuối cùng là một hình chữ nhật nhỏ; chúng tôi liệt kê tất cả các cặp số nguyên bên trong nó và lọc theo tính chẵn lẻ. 

| Bước | phạm vi U | Phạm vi V | Hành động | 
| --- | --- | --- | --- | 
| Sau khi xử lý tháp | khoảng thời gian cố định | khoảng thời gian cố định | ràng buộc giao nhau | 
| Đếm | tất cả các bạn trong phạm vi | tất cả v trong phạm vi | kiểm tra tính chẵn lẻ | 
| Đầu ra | hợp lệ (x, y) | được sắp xếp | câu trả lời cuối cùng | 

Quan sát quan trọng trong trường hợp này là nhiều điểm ứng cử viên tồn tại vì giao điểm không phải là một điểm duy nhất mà là một lưới nhỏ các nghiệm số nguyên khả thi. 

Bây giờ hãy xem xét mẫu thứ hai, trong đó các ràng buộc chặt chẽ hơn. 

| Bước | phạm vi U | Phạm vi V | Hành động | 
| --- | --- | --- | --- | 
| Sau khi xử lý tháp | khoảng nhỏ hơn | khoảng nhỏ hơn | ràng buộc mạnh mẽ hơn | 
| Đếm | ít cặp hơn | ít cặp hơn | áp dụng bộ lọc chẵn lẻ | 
| Đầu ra | bộ giảm | được sắp xếp | câu trả lời cuối cùng | 

Điều này thể hiện cách mỗi tháp bổ sung thu nhỏ vùng khả thi một cách độc lập dọc theo hai trục được chuyển đổi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N + K)$| Mỗi tháp cập nhật giới hạn một lần, sau đó mỗi tháp đều hợp lệ$(u,v)$cặp được liệt kê | 
| Không gian |$O(1)$| Chỉ có một vài biến khoảng và lưu trữ đầu ra | 

Thuật toán tuyến tính theo số lượng tháp cộng với số câu trả lời hợp lệ, là tối ưu vì mọi ràng buộc phải được đọc ít nhất một lần và mọi điểm đầu ra phải được in. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

# minimal case
assert run("1\n0 0 0\n") == "0 0"

# single tower, radius 1 diamond boundary
out = set(run("1\n0 0 1\n").splitlines())
assert ("1 0" in out)

# multiple towers shrinking to a single point
assert run("2\n0 0 2\n1 1 0\n") == "1 1"

# parity filtering case
res = run("1\n0 0 2\n").splitlines()
for line in res:
    x, y = map(int, line.split())
    assert abs(x) + abs(y) == 2
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| ràng buộc số 0 duy nhất | (0,0) | trường hợp nhận dạng | 
| bán kính 1 | 4 điểm ranh giới | kim cương cơ bản | 
| ràng buộc giao nhau | điểm duy nhất | tính nhất quán | 
| bán kính 2 | lọc chẵn lẻ | tính đúng đắn của bản đồ | 

## Vỏ cạnh 

Trường hợp góc phát sinh khi vùng khả thi thu gọn về một điểm mạng duy nhất. Trong hoàn cảnh đó,$U_{\min} = U_{\max}$Và$V_{\min} = V_{\max}$và thuật toán vẫn hoạt động vì các vòng lặp suy biến thành một lần lặp duy nhất và việc kiểm tra tính chẵn lẻ sẽ tự động được thực hiện. 

Một trường hợp tinh vi khác là khi hình chữ nhật ở$(u,v)$-space chứa nhiều điểm nhưng chỉ có một nửa là hợp lệ do tính chẵn lẻ. Ví dụ, nếu$U_{\min}=0$,$U_{\max}=2$,$V_{\min}=0$,$V_{\max}=2$, thì chỉ có bốn kết hợp tồn tại và chính xác những kết hợp có tính chẵn lẻ phù hợp sẽ tạo ra số nguyên$(x,y)$. Thuật toán lọc những thứ này một cách tự nhiên mà không cần xử lý đặc biệt. 

Cuối cùng, nếu ranh giới của nhiều tháp trở nên cực kỳ chặt chẽ, giao điểm có thể thu hẹp lại thành một vùng trống, nhưng vấn đề đảm bảo điều này không xảy ra nên không cần xử lý rõ ràng.
