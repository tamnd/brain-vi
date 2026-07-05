---
title: "CF 102899G - KK \u770b\u8df3\u821e"
description: "Chúng tôi được cung cấp nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm mô tả một chuỗi $n$ số nguyên riêng biệt, là một hoán vị của $1 ldots n$. Trình tự này thể hiện thứ tự các vũ công đi qua trước mặt người quan sát."
date: "2026-07-04T08:21:12+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102899
codeforces_index: "G"
codeforces_contest_name: "The 2nd Hangzhou Normal University Freshman Programming Contest"
rating: 0
weight: 102899
solve_time_s: 42
verified: true
draft: false
---

[CF 102899G - KK \u770b\u8df3\u821e](https://codeforces.com/problemset/problem/102899/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 42s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp nhiều trường hợp thử nghiệm. Mỗi ca kiểm thử mô tả một chuỗi các$n$các số nguyên riêng biệt, đó là một hoán vị của$1 \ldots n$. Trình tự này thể hiện thứ tự các vũ công đi qua trước mặt người quan sát. Tại thời điểm quan sát, các vũ công được sắp xếp thành một vòng tròn và di chuyển theo chiều kim đồng hồ hoặc ngược chiều kim đồng hồ. Do chuyển động tròn này, những gì người quan sát nhìn thấy là một đoạn liên tiếp của vòng tròn, nhưng đoạn đó có thể được đọc theo một trong hai hướng tùy theo hướng. 

Nhiệm vụ là quyết định xem trình tự quan sát được có thể đến từ việc sắp xếp vòng tròn các$1 \ldots n$và đọc tất cả các phần tử liên tiếp theo chiều kim đồng hồ hoặc ngược chiều kim đồng hồ. Nói cách khác, chúng ta được hỏi liệu hoán vị là một phép quay tròn theo thứ tự đồng nhất hay một phép quay tròn theo thứ tự ngược lại. 

Ràng buộc$n \le 10^4$cho mỗi trường hợp thử nghiệm và nhiều nhất là 10 trường hợp thử nghiệm ngụ ý tổng kích thước đầu vào khoảng$10^5$. Điều này loại trừ bất kỳ$O(n^2)$so sánh trên mỗi trường hợp thử nghiệm dưới dạng đường biên nhưng vẫn chỉ có khả năng được chấp nhận với các hằng số nhỏ. Tuy nhiên, cấu trúc cho thấy việc kiểm tra tuyến tính đơn giản hơn nhiều là đủ. 

Ở đây thường xảy ra một sự hiểu lầm ngây thơ: người ta có thể nghĩ bất kỳ hoán vị nào có thể được sắp xếp lại thành một chu trình đều hợp lệ, nhưng điều kiện chặt chẽ hơn. Trình tự xung quanh vòng tròn phải đều đều, không có bước nhảy. 

Một vài trường hợp khó khăn minh họa cho yêu cầu này. 

Coi như$n = 4$, sự liên tiếp$[1, 3, 2, 4]$. Đây không phải là đường tròn một chiều, vì sau khi đi từ 1 đến 3, chúng ta bỏ qua 2 rồi quay lại, điều này phá vỡ tính kề cận trên chu trình. 

Coi như$n = 5$, sự liên tiếp$[3, 4, 5, 1, 2]$. Điều này hợp lệ vì nó là sự xoay vòng của thứ tự đã sắp xếp. 

Coi như$n = 4$, sự liên tiếp$[4, 3, 2, 1]$. Điều này cũng hợp lệ vì nó tương ứng với việc truyền tải ngược. 

Thuộc tính chính là modulo kề$n$. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là thử mọi điểm bắt đầu có thể có trong vòng tròn và cố gắng khớp chuỗi theo modulo thứ tự tăng hoặc giảm$n$. Đối với mỗi chỉ mục bắt đầu, chúng tôi mô phỏng quá trình truyền tải và kiểm tra xem hoán vị có căn chỉnh hay không. Mỗi chi phí mô phỏng$O(n)$, và có$n$vị trí bắt đầu, dẫn đến$O(n^2)$mỗi trường hợp thử nghiệm. Với$n = 10^4$, điều này trở thành$10^8$hoạt động trên mỗi trường hợp thử nghiệm, quá chậm so với giới hạn thời gian. 

Cấu trúc của bài toán loại bỏ sự cần thiết phải thử tất cả các phép quay. Nếu chuỗi hợp lệ thì mỗi cặp liên tiếp phải đại diện cho các hàng xóm trong chu trình. Điều đó có nghĩa là một khi chúng ta xác định hướng bằng bước đầu tiên thì tất cả các bước tiếp theo sẽ bị buộc phải thực hiện. Điều mơ hồ duy nhất là hướng: bậc tăng hoặc bậc modulo giảm$n$. 

Vì vậy, thay vì kiểm tra tất cả các phép quay, chúng tôi kiểm tra xem hoán vị có phù hợp với modulo chuỗi bước +1 hay không$n$(có bao quanh) hoặc modulo trình tự -1 bước$n$. Điều này làm giảm vấn đề xuống còn hai lần quét tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta coi các con số là các điểm trên một chu kỳ có độ dài$n$, sau đó ở đâu$n$đến 1 và trước 1 đến$n$. 

1. Đọc dãy và tính hiệu giữa các phần tử liên tiếp theo modulo$n$. Thay vì trừ thô, chúng ta chuẩn hóa tính kề nhau như thể hai số liên tiếp khác nhau đúng 1 đơn vị tiến hoặc 1 đơn vị lùi trong chu kỳ. Điều này có nghĩa là đối với các giá trị liền kề$a$Và$b$, chúng tôi kiểm tra xem$b = a + 1$,$b = a - 1$, hoặc hộp bọc$(a, b) = (n, 1)$hoặc$(1, n)$. 
2. Xác định hướng từ cặp đầu tiên. Nếu quá trình chuyển đổi đầu tiên tăng thêm 1 (bao gồm cả gói từ$n \to 1$), chúng tôi kiểm tra toàn bộ chuỗi giả định hướng thuận. Nếu nó giảm đi 1 (bao gồm cả gói từ$1 \to n$), chúng tôi kiểm tra hướng ngược lại. Nếu không đúng thì câu trả lời ngay lập tức là KHÔNG. 
3. Đối với hướng đã chọn, hãy quét trình tự từ trái sang phải và xác minh rằng mọi cặp liền kề đều tuân theo cùng một quy tắc bước. Hướng phải duy trì nhất quán cho tất cả các chuyển đổi. 
4. Nếu tất cả các chuyển đổi khớp với hướng tiến hoặc lùi, thì xuất CÓ, nếu không thì xuất ra KHÔNG. 

### Tại sao nó hoạt động 

Một phép duyệt vòng tròn hợp lệ có cấu trúc cục bộ cứng nhắc: mỗi phần tử có chính xác hai phần tử lân cận trên chu trình. Khi chúng tôi quan sát hai phần tử liên tiếp, chúng tôi xác định mối quan hệ lân cận nào đang được sử dụng. Vì chu trình là đồng nhất nên quyết định cục bộ này quyết định cấu trúc toàn cầu. Nếu bất kỳ quá trình chuyển đổi nào sau đó lệch khỏi mẫu kề cận này thì chuỗi không thể tương ứng với việc truyền liên tục của chu trình theo bất kỳ hướng nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def check(a):
    n = len(a)

    def ok_forward():
        for i in range(n - 1):
            if a[i + 1] != a[i] + 1 and not (a[i] == n and a[i + 1] == 1):
                return False
        return True

    def ok_backward():
        for i in range(n - 1):
            if a[i + 1] != a[i] - 1 and not (a[i] == 1 and a[i + 1] == n):
                return False
        return True

    return ok_forward() or ok_backward()

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    print("YES" if check(a) else "NO")
```Việc triển khai trực tiếp mã hóa hai lần truyền tải có thể có của chu trình. Kiểm tra chuyển tiếp thực thi rằng mỗi bước sẽ tăng thêm một, ngoại trừ trường hợp bọc đặc biệt từ$n$quay lại$1$. Việc kiểm tra ngược thực thi các phép giảm một cách đối xứng bằng cách bao bọc từ$1$ĐẾN$n$. 

Một điểm tinh tế là chúng tôi không cố gắng “xoay” trình tự. Phép quay không liên quan vì tính kề cận được bảo toàn khi xoay. Mọi phép duyệt chu trình hợp lệ sẽ thỏa mãn thuộc tính kề bất kể điểm bắt đầu. 

## Ví dụ đã hoạt động 

### Ví dụ 1:$[1, 2, 3, 4]$| tôi | một [tôi] | a[i+1] | Kiểm tra | 
| --- | --- | --- | --- | 
| 0 | 1 | 2 | +1 hợp lệ | 
| 1 | 2 | 3 | +1 hợp lệ | 
| 2 | 3 | 4 | +1 hợp lệ | 

Kiểm tra chuyển tiếp hoàn toàn thành công, vì vậy đầu ra là CÓ. 

Kiểm tra ngược không thành công ngay ở lần chuyển đổi đầu tiên vì 2 không phải là 0 hoặc ngắt quãng từ 1. 

Điều này thể hiện sự truyền tải đơn điệu rõ ràng theo một hướng. 

### Ví dụ 2:$[3, 2, 1, 4]$| tôi | một [tôi] | a[i+1] | Kiểm tra | 
| --- | --- | --- | --- | 
| 0 | 3 | 2 | -1 hợp lệ | 
| 1 | 2 | 1 | -1 hợp lệ | 
| 2 | 1 | 4 | bọc hợp lệ | 

Hướng lùi được duy trì xuyên suốt nên đầu ra là CÓ. 

Điều này xác nhận rằng phép quay theo thứ tự đảo ngược được chấp nhận. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi trường hợp kiểm thử sẽ quét mảng một số lần không đổi | 
| Không gian |$O(1)$| Không có cấu trúc phụ trợ ngoài kho lưu trữ đầu vào | 

Với$\sum n \le 10^5$, giải pháp chạy thoải mái trong giới hạn vì mỗi phần tử được xử lý với số lần không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        def check(a):
            n = len(a)

            def ok_forward():
                for i in range(n - 1):
                    if a[i + 1] != a[i] + 1 and not (a[i] == n and a[i + 1] == 1):
                        return False
                return True

            def ok_backward():
                for i in range(n - 1):
                    if a[i + 1] != a[i] - 1 and not (a[i] == 1 and a[i + 1] == n):
                        return False
                return True

            return ok_forward() or ok_backward()

        output.append("YES" if check(a) else "NO")
    return "\n".join(output)

# sample-like tests
assert run("1\n3\n1 2 3\n") == "YES"
assert run("1\n3\n1 3 2\n") == "YES"

# custom tests
assert run("1\n4\n1 3 2 4\n") == "NO"
assert run("1\n4\n4 3 2 1\n") == "YES"
assert run("1\n5\n2 3 4 5 1\n") == "YES"
assert run("1\n5\n1 3 2 4 5\n") == "NO"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 3 / 1 2 3 | CÓ | chu kỳ chuyển tiếp cơ bản | 
| 1 3 / 1 3 2 | CÓ | chu kỳ ngược lại | 
| 1 4 / 1 3 2 4 | KHÔNG | liền kề bị hỏng | 
| 1 4 / 4 3 2 1 | CÓ | đảo ngược hoàn toàn | 
| 1 5 / 2 3 4 5 1 | CÓ | vòng quay của chu kỳ | 
| 1 5 / 1 3 2 4 5 | KHÔNG | nhảy hỗn hợp | 

## Vỏ cạnh 

Một trường hợp tối thiểu như$n = 1$luôn trôi qua, vì một phần tử đơn lẻ tạo thành một chu trình tiến và lùi một cách tầm thường. 

Vì$n = 2$, cả hai$[1, 2]$Và$[2, 1]$là hợp lệ vì mỗi bước là một nước đi lân cận hợp lệ trên chu kỳ hai nút. Thuật toán chấp nhận cả hai vì mỗi lần kiểm tra hướng đều thành công ngay lập tức. 

Đối với các chuỗi nặng như$[n, 1, 2, \ldots, n-1]$, việc kiểm tra chuyển tiếp sẽ xác thực quá trình chuyển đổi gói từ$n \to 1$và tiếp tục một cách nhất quán. Việc kiểm tra ngược không thành công ngay lập tức nhưng chỉ cần một hướng. 

Đối với các chuỗi hỗn hợp như$[1, 3, 2, 4]$, quá trình chuyển đổi đầu tiên có thể gợi ý một hướng, nhưng quá trình chuyển đổi thứ hai vi phạm tính liền kề. Quá trình quét sẽ phát hiện điều này ngay lập tức ở lần so sánh thứ hai, ngăn chặn mọi sự chấp nhận sai.
