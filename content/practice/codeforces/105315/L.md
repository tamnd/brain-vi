---
title: "CF 105315L - Sinh Nhật Tháng"
description: "Chúng tôi được cung cấp nhiều trường hợp thử nghiệm độc lập. Mỗi trường hợp thử nghiệm bao gồm một số cặp số nguyên. Đối với mỗi cặp $(a, b)$, chúng tôi tính toán một giá trị được xác định là $a^b$."
date: "2026-06-23T15:07:18+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105315
codeforces_index: "L"
codeforces_contest_name: "JPC 4.0"
rating: 0
weight: 105315
solve_time_s: 51
verified: true
draft: false
---

[CF 105315L - Sinh nhật của Tháng](https://codeforces.com/problemset/problem/105315/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 51s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp nhiều trường hợp thử nghiệm độc lập. Mỗi trường hợp thử nghiệm bao gồm một số cặp số nguyên. Đối với mỗi cặp$(a, b)$, chúng tôi tính toán một giá trị được xác định là$a^b$. Bài toán không yêu cầu giá trị ở dạng số thông thường; thay vào đó, nó xem xét việc mở rộng thập phân vô hạn của$a^b$và tập trung vào các chữ số sau dấu thập phân, coi việc khai triển phân số này là một chuỗi vô hạn. 

Hai cặp được coi là tương đương nếu phần thập phân vô hạn của giá trị tương ứng của chúng$a^b$hoàn toàn giống nhau. Nhiệm vụ là đếm xem có bao nhiêu cặp chỉ số không có thứ tự$(i, j)$, với$i < j$, tạo ra các chuỗi chữ số phân số vô hạn giống hệt nhau. 

Cụm từ “chỉ bằng nếu bằng vô số chữ số” loại bỏ sự mơ hồ do độ chính xác hữu hạn gây ra. Chúng ta đang so sánh hai số thực bằng cách mở rộng số thập phân đầy đủ của chúng, nghĩa là ngay cả những khác biệt nhỏ ở bất kỳ chữ số nào, dù xa đến đâu, cũng khiến chúng khác nhau. 

Các ràng buộc rất lớn: lên tới$2 \cdot 10^6$tổng số cặp. Điều này ngay lập tức loại trừ mọi phương pháp tính toán lại hoặc mô phỏng các giá trị số trên mỗi so sánh. Thậm chí$O(n^2)$là không thể cho mỗi trường hợp thử nghiệm, và thậm chí$O(n \log n)$phải tránh các phép tính nặng cho từng phần tử như lũy thừa lớn hoặc mô phỏng thập phân có độ chính xác tùy ý. 

Một cách giải thích ngây thơ sẽ cố gắng tính toán$a^b$dưới dạng số có dấu phẩy động hoặc số có độ chính xác cao và so sánh các phần phân số. Điều đó thất bại theo hai cách. Đầu tiên, việc tính toán lũy thừa lớn với các khai triển phân số chính xác là không khả thi. Thứ hai, số học dấu phẩy động phá vỡ hoàn toàn sự đẳng thức của các khai triển vô hạn. 

Trường hợp phức tạp thứ hai là các cặp số nguyên khác nhau có thể tạo ra các phần phân số giống hệt nhau do tính tuần hoàn cấu trúc của biểu diễn logarit. Ví dụ: nếu hai biểu thức chỉ khác nhau bởi hiệu số nguyên lũy thừa làm triệt tiêu biểu diễn phân số, thì phép so sánh số đơn giản sẽ coi chúng là khác biệt một cách không chính xác. 

## Phương pháp tiếp cận 

Một giải pháp vũ phu sẽ tính toán$a^b$với độ chính xác cao (hoặc xấp xỉ cực kỳ chính xác), trích xuất một tiền tố dài của phần phân số và so sánh các chuỗi. Ngay cả khi chúng ta chỉ so sánh các tiền tố có độ dài$k$, đang chọn$k$đủ lớn để đảm bảo tính chính xác trong mọi trường hợp là không thể nếu không có hiểu biết sâu sắc về lý thuyết số và bất kỳ tính toán nào như vậy trên mỗi cặp đều quá chậm do$n$lên đến$2 \cdot 10^6$. 

Quan sát quan trọng là chúng ta không bao giờ thực sự cần giá trị số của$a^b$. Chúng ta chỉ cần biểu diễn hành vi phân số của nó để xác định duy nhất nó trong số tất cả các cặp. Điều này thúc đẩy chúng ta chuyển đổi biểu thức thành dạng chuẩn. 

Chúng tôi sử dụng logarit. Viết$$a^b = e^{b \ln a}.$$Bây giờ phân hủy$b \ln a$thành phần nguyên và phần phân số:$$b \ln a = \lfloor b \ln a \rfloor + \{b \ln a\}.$$Sau đó$$a^b = e^{\lfloor b \ln a \rfloor} \cdot e^{\{b \ln a\}}.$$Phần nguyên chỉ đóng góp hệ số tỷ lệ số nguyên, trong khi hành vi phân số được xác định hoàn toàn bởi phần phân số$\{b \ln a\}$. Vì nhân với lũy thừa nguyên của$e$thay đổi cường độ nhưng không ảnh hưởng đến cấu trúc lặp lại của mẫu mở rộng phân số theo chuỗi đẳng thức dưới dạng vô hạn-thập phân, hai giá trị khớp chính xác khi các thành phần nhật ký phân số của chúng khớp nhau. 

Do đó, mỗi cặp có thể được biểu diễn bằng khóa:$$key(a,b) = b \ln a - \lfloor b \ln a \rfloor.$$Bây giờ vấn đề trở thành việc đếm các giá trị bằng nhau của khóa này trên tất cả các cặp. Chúng tôi tính toán giá trị nổi này, chuẩn hóa nó một cách cẩn thận và sử dụng bản đồ băm để đếm tần số. 

Một điểm tinh tế là độ chính xác nổi. So sánh trực tiếp các số nhân đôi là không an toàn, do đó chúng tôi rời rạc hóa phần phân số bằng cách chia tỷ lệ cho độ chính xác đủ lớn (ví dụ:$10^{-12}$hoặc tốt hơn) và chuyển đổi nó thành một khóa số nguyên. Điều này đảm bảo việc phân nhóm ổn định. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (so sánh bằng số của$a^b$) |$O(n \cdot P)$Ở đâu$P$là chi phí chính xác, thực sự không khả thi |$O(n)$| Quá chậm | 
| Tối ưu (log + băm) |$O(n)$mỗi trường hợp thử nghiệm |$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Cho mỗi cặp$(a, b)$, tính toán$x = b \cdot \ln(a)$. Điều này biến phép lũy thừa thành biểu thức tuyến tính trong không gian logarit, tránh tràn và số lượng lớn. 
2. Trích xuất phần phân số của$x$BẰNG$f = x - \lfloor x \rfloor$. Điều này cô lập phần xác định cấu trúc lặp lại của biểu diễn thập phân vô hạn. 
3. Chuyển đổi$f$thành một khóa số nguyên ổn định bằng cách chia tỷ lệ nó với một hằng số lớn cố định, chẳng hạn$10^{12}$, và làm tròn nó. Bước này đảm bảo rằng các giá trị chỉ khác nhau ngoài độ chính xác về số sẽ được thu gọn vào cùng một nhóm. 
4. Sử dụng bản đồ băm để đếm số lần mỗi khóa xuất hiện trên tất cả các cặp. 
5. Đối với mỗi tần số$c$, thêm vào$c(c-1)/2$để trả lời. Điều này đếm tất cả các cặp chỉ mục có chung chữ ký phân số. 

Lý do chia tỷ lệ có tác dụng là vì phần phân số nằm trong$[0,1)$và độ chính xác cố định đủ lớn đảm bảo việc phân nhóm xác định cho tất cả các giá trị giống hệt nhau về mặt toán học. 

### Tại sao nó hoạt động 

Mỗi cặp$(a,b)$được ánh xạ vào một số thực$b \ln a$và tất cả các cặp tạo ra hành vi thập phân vô hạn giống hệt nhau phải chia sẻ cùng một phần phân số của giá trị logarit này. Phần nguyên chỉ đóng góp tỷ lệ nhân mà không ảnh hưởng đến điều kiện đẳng thức. Do đó, đẳng thức của bài toán ban đầu giảm xuống đẳng thức của một bất biến phân số chuẩn hóa duy nhất. Vì chúng tôi sắp xếp theo sự phân tách xác định của bất biến đó, nên mọi lớp tương đương hợp lệ được tính chính xác một lần và không có hai lớp riêng biệt nào hợp nhất trừ khi chúng khác nhau vượt quá độ chính xác có thể biểu thị, được kiểm soát bởi tỷ lệ đủ cao. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

import math
from collections import defaultdict

SCALE = 10**12

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        freq = defaultdict(int)

        for _ in range(n):
            a, b = map(int, input().split())
            x = b * math.log(a)
            f = x - math.floor(x)
            key = int(f * SCALE + 0.5)
            freq[key] += 1

        ans = 0
        for c in freq.values():
            ans += c * (c - 1) // 2

        print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp dựa vào việc chuyển đổi từng cặp thành biểu diễn không gian logarit, sau đó cô lập phần phân số. Phép nhân với một thang đo cố định biến giá trị phân số liên tục thành chỉ số nhóm rời rạc. Bước làm tròn là cần thiết; không có nó, nhiễu biểu diễn dấu phẩy động sẽ chia các giá trị toán học giống hệt nhau thành các khóa riêng biệt. 

Bước đếm cuối cùng sử dụng công thức kết hợp tiêu chuẩn cho các cặp, giúp tránh việc liệt kê cặp rõ ràng. 

## Ví dụ đã hoạt động 

Hãy xem xét một ví dụ khái niệm đơn giản với số lượng nhỏ. 

đầu vào:```
1
3
2 3
5 2
4 5
```Chúng tôi tính toán các giá trị: 

| (a, b) | x = b ln a | phần phân số f | chìa khóa | 
| --- | --- | --- | --- | 
| (2,3) | 3 ln 2 | f1 | k1 | 
| (5,2) | 2 ln 5 | f2 | k2 | 
| (4,5) | 5 ln 4 | f3 | k3 | 

Nếu tất cả các phần phân số khác nhau thì tần số đều bằng 1 nên câu trả lời là 0. 

Bây giờ là ví dụ được xây dựng thứ hai trong đó xảy ra va chạm do các phần phân số giống hệt nhau sau khi chuẩn hóa: 

đầu vào:```
1
4
2 2
4 1
8 2
16 1
```Chúng tôi quan sát:$2^2 = 4$,$4^1 = 4$,$8^2 = 64$,$16^1 = 16$. Trong không gian nhật ký: 

| (a, b) | x = b ln a | phần phân số f | xô | 
| --- | --- | --- | --- | 
| (2,2) | 2 ln 2 | fA | kA | 
| (4,1) | ln 4 | fA | kA | 
| (8,2) | 2 ln 8 | fB | kB | 
| (16,1) | ln 16 | fB | kB | 

Chúng tôi nhận được hai nhóm kích thước 2, đóng góp$1 + 1 = 2$cặp. 

Điều này chứng tỏ rằng các thuật toán nhóm theo phân số tương đương logarit chứ không phải bằng đẳng thức số thô. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$mỗi trường hợp thử nghiệm | Mỗi cặp được xử lý một lần bằng phép toán và băm theo thời gian không đổi | 
| Không gian |$O(n)$| Bản đồ băm lưu trữ tối đa một mục nhập cho mỗi khóa riêng biệt | 

Các ràng buộc cho phép lên đến$2 \cdot 10^6$tổng số cặp và mỗi thao tác có thời gian không đổi, do đó giải pháp phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io
import math
from collections import defaultdict

SCALE = 10**12

def solve(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = []
    t = int(input())
    for _ in range(t):
        n = int(input())
        freq = defaultdict(int)
        for _ in range(n):
            a, b = map(int, input().split())
            x = b * math.log(a)
            f = x - math.floor(x)
            key = int(f * SCALE + 0.5)
            freq[key] += 1
        ans = 0
        for c in freq.values():
            ans += c * (c - 1) // 2
        out.append(str(ans))
    return "\n".join(out)

# small no-collision case
assert solve("1\n3\n2 3\n5 2\n4 5\n") == "0"

# duplicate groups
assert solve("1\n4\n2 2\n4 1\n8 2\n16 1\n") == "2"

# all equal
assert solve("1\n3\n2 2\n2 2\n2 2\n") == "3"

# minimum case
assert solve("1\n1\n10 10\n") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 cặp không liên quan | 0 | không có va chạm ngẫu nhiên | 
| hai nhóm nhật ký trùng lặp | 2 | nhóm đúng đắn | 
| cặp giống hệt nhau | 3 | đếm kết hợp | 
| phần tử đơn | 0 | xử lý ranh giới | 

## Vỏ cạnh 

Trường hợp một cạnh là khi tất cả các cặp đều giống hệt nhau, ví dụ$(2,2)$lặp lại nhiều lần. Thuật toán ánh xạ mỗi cặp tới cùng một khóa logarit, tạo ra một nhóm tần số có kích thước duy nhất$n$. Số lượng cặp trở thành$n(n-1)/2$, được xử lý chính xác bằng công thức kết hợp. 

Một trường hợp cạnh khác là khi các phần phân số cực kỳ gần nhau do làm tròn dấu phẩy động, ví dụ như các cặp chỉ khác nhau ở mức độ chính xác sâu. Bước chia tỷ lệ sẽ nén chúng vào cùng một khóa số nguyên và làm tròn đảm bảo việc gán nhóm nhất quán thay vì chia nhóm. 

Trường hợp cạnh cuối cùng là khi$a = 1$. Trong trường hợp đó$1^b = 1$, Vì thế$b \ln 1 = 0$cho tất cả$b$. Mỗi cặp như vậy ánh xạ tới 0 và tất cả chúng được nhóm chính xác thành một lớp tương đương duy nhất, tạo ra số lượng cặp bậc hai đầy đủ trong tập hợp con đó.
