---
title: "CF 1007B - Lát đường song song"
description: "Cho một hình hộp chữ nhật cố định có độ dài các cạnh $A, B, C$. Chúng ta muốn đếm xem có bao nhiêu bộ ba khác nhau $(a,b,c)$, được sắp xếp sao cho $a le b le c$, có thể đóng vai trò là một khối xây dựng sao cho các bản sao của hộp nhỏ hơn này có thể xếp chồng chính xác lên hộp lớn hơn, miễn là mọi bản sao đều được sử dụng…"
date: "2026-06-16T23:05:44+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "brute-force", "combinatorics", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1007
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 497 (Div. 1)"
rating: 2400
weight: 1007
solve_time_s: 124
verified: true
draft: false
---

[CF 1007B - Mở đường song song](https://codeforces.com/problemset/problem/1007/B) 

**Đánh giá:** 2400 
**Tags:** bitmasks, lực lượng vũ phu, tổ hợp, toán học, lý thuyết số 
**Thời gian giải:** 2m 4s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Ta có một hình hộp chữ nhật cố định có độ dài các cạnh$A, B, C$. Chúng tôi muốn đếm có bao nhiêu bộ ba khác nhau$(a,b,c)$, ra lệnh sao cho$a \le b \le c$, có thể đóng vai trò như một khối xây dựng sao cho các bản sao của hộp nhỏ hơn này có thể xếp chính xác vào hộp lớn hơn, miễn là mọi bản sao đều được sử dụng theo cùng một hướng. 

Ràng buộc hình học quan trọng là việc xếp gạch chỉ có thể thực hiện được khi mỗi cạnh của hộp nhỏ thẳng hàng nhất quán với một trục của hộp lớn, do đó hướng đã chọn của$(a,b,c)$phải chia các kích thước tương ứng của hộp lớn mà không có phần dư. Chúng tôi không được phép xoay các ô riêng lẻ một cách độc lập, nhưng chúng tôi được phép chọn căn chỉnh tổng thể của hộp nhỏ so với hộp lớn. 

Nhiệm vụ là đếm xem có bao nhiêu bộ ba không giảm khác nhau$(a,b,c)$thỏa mãn điều kiện xếp gạch này. 

Đầu vào bao gồm tối đa$10^5$trường hợp thử nghiệm và mỗi trường hợp thử nghiệm có kích thước lên tới$10^5$. Điều này loại trừ bất kỳ giải pháp nào liệt kê trực tiếp tất cả các bộ ba. Ngay cả việc liệt kê tất cả các ước số của từng số một cách riêng biệt và sau đó thử tất cả các kết hợp sẽ trở nên quá chậm nếu được thực hiện một cách ngây thơ cho mỗi trường hợp thử nghiệm mà không có cấu trúc cẩn thận, vì số lượng và kết hợp ước số trong trường hợp xấu nhất vẫn có thể xảy ra khi lý luận tích chéo đầy đủ. 

Một trường hợp lỗi phổ biến phát sinh khi một giải pháp giả định rằng bất kỳ bộ ba ước số nào cũng có thể được hoán vị tùy ý trên các trục mà không ảnh hưởng đến tính chính xác. Ví dụ, trong một khối$2 \times 2 \times 2$, bộ ba$(1,1,2)$hoạt động, nhưng việc gán giá trị cho các trục không chính xác có thể từ chối sai hoặc đếm gấp đôi giá trị đó tùy thuộc vào cách xử lý hoán vị. Một vấn đề tế nhị khác xuất hiện khi tất cả các chiều đều bằng nhau; cấu hình đối xứng có xu hướng bị tính quá mức nếu việc gán trục không được chuẩn hóa. 

Khó khăn chính không phải là tìm các ước số hợp lệ mà là đếm các kết hợp có cấu trúc của chúng dưới một ràng buộc thứ tự và tính nhất quán của việc căn chỉnh trục. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp sẽ liệt kê mỗi ứng cử viên gấp ba lần$(a,b,c)$lên đến$\min(A,B,C)$, kiểm tra xem mỗi kích thước có chia kích thước theo một số hướng hợp lệ hay không và đếm những kích thước phù hợp. Về nguyên tắc thì điều này đúng, nhưng nó quá chậm. Số lượng gấp ba lên đến$10^5$theo thứ tự của$10^{15}$, khiến điều này không thể thực hiện được. 

Sự đơn giản hóa cấu trúc quan trọng là tách hình học khỏi việc đếm. Đối với phép gán trục cố định, giả sử chúng tôi quyết định rằng$a$phù hợp với một chiều,$b$với người khác, và$c$với điều kiện cuối cùng, điều kiện hợp lệ trở thành ràng buộc chia hết:$$a \mid A,\quad b \mid B,\quad c \mid C.$$Điều này làm giảm vấn đề trong việc chọn một ước số từ mỗi chiều và thực thi$a \le b \le c$. 

Khi chúng tôi khắc phục cách giải thích này, vấn đề sẽ trở thành việc đếm các bộ ba có thứ tự được rút ra từ ba bộ ước số với một ràng buộc đơn điệu. Cấu trúc đó cho phép chúng ta thay thế phép liệt kê ba lần bằng cách đếm tiền tố. 

Chúng tôi tính toán các ước số của từng thứ nguyên, sắp xếp chúng và sau đó đếm bộ ba$(a,b,c)$thỏa mãn$a \le b \le c$sử dụng quét qua phần tử ở giữa$b$. Đối với mỗi$b$, chúng tôi đếm có bao nhiêu hợp lệ$a$là$\le b$và có bao nhiêu hợp lệ$c$là$\ge b$, sau đó nhân lên. 

Điều này biến đổi một tổ hợp bậc ba thành một tổ hợp gần tuyến tính trên số chia. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên tất cả các bộ ba |$O(N^3)$mỗi bài kiểm tra |$O(1)$| Quá chậm | 
| Ước số + đếm theo thứ tự |$O(d(A)+d(B)+d(C))$mỗi bài kiểm tra |$O(d)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp các kích thước sao cho$A \le B \le C$. 

Điều này khắc phục sự định hướng nhất quán cho việc đếm, tránh sự mơ hồ do hoán vị các trục. 
2. Tính tất cả các ước của$A$,$B$, Và$C$, lưu trữ chúng trong mảng được sắp xếp$D_A, D_B, D_C$. 

Chúng đại diện cho tất cả các giá trị có thể có mà mỗi bên của hộp nhỏ có thể nhận nếu nó thẳng hàng với trục đó. 
3. Với mỗi ước số$b \in D_B$, đếm xem có bao nhiêu phần tử$a \in D_A$thỏa mãn$a \le b$. 

Điều này đảm bảo ràng buộc về thứ tự giữa hai chiều đầu tiên. 
4. Tương tự$b$, đếm xem có bao nhiêu phần tử$c \in D_C$thỏa mãn$c \ge b$. 

Điều này thực thi$b \le c$. 
5. Nhân hai số đếm này và cộng dồn thành đáp án. 

Mỗi cặp lựa chọn hợp lệ cho$a$Và$c$kết hợp với giá trị trung bình cố định$b$tạo thành một bộ ba không giảm hợp lệ. 
6. Xuất số tiền tích lũy cuối cùng. 

### Tại sao nó hoạt động 

Khi các trục đã được cố định và sắp xếp, mọi lát gạch hợp lệ đều tương ứng chính xác với việc chọn ước số$a,b,c$sao cho mỗi chiều phân chia kích thước được chỉ định của nó và ràng buộc thứ tự được giữ nguyên. Các ràng buộc chia hết là độc lập trên mỗi trục, trong khi ràng buộc thứ tự chỉ kết hợp các lựa chọn liền kề. Bằng cách neo vào giá trị trung bình$b$, chúng tôi phân tách ràng buộc thành hai điều kiện tiền tố độc lập, duy trì tính chính xác trong khi tránh liệt kê rõ ràng tất cả các bộ ba. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def divisors(x):
    small = []
    large = []
    i = 1
    while i * i <= x:
        if x % i == 0:
            small.append(i)
            if i * i != x:
                large.append(x // i)
        i += 1
    return small + large[::-1]

def solve():
    t = int(input())
    for _ in range(t):
        A, B, C = map(int, input().split())
        A, B, C = sorted((A, B, C))

        DA = divisors(A)
        DB = divisors(B)
        DC = divisors(C)

        DA.sort()
        DB.sort()
        DC.sort()

        # prefix counts for DA
        ans = 0

        # two pointers for DA and DC
        i = 0
        k = 0
        nA = len(DA)
        nC = len(DC)

        for b in DB:
            while i < nA and DA[i] <= b:
                i += 1
            while k < nC and DC[k] < b:
                k += 1

            cntA = i
            cntC = nC - k
            ans += cntA * cntC

        print(ans)

if __name__ == "__main__":
    solve()
```Mã bắt đầu bằng cách chuẩn hóa từng trường hợp thử nghiệm để các thứ nguyên được sắp xếp. Điều này cho phép chúng ta xử lý kích thước nhỏ nhất một cách nhất quán như kích thước được ghép nối với$a$, ở giữa với$b$, và lớn nhất với$c$. 

Hàm tạo số chia chạy trong$O(\sqrt{n})$, thu thập các cặp ước số một cách hiệu quả mà không cần làm việc dư thừa. Nó chia các ước số nhỏ và lớn để duy trì thứ tự được sắp xếp với chi phí tối thiểu. 

Trong mỗi trường hợp thử nghiệm, chúng ta lặp lại tất cả các ước của$B$, đóng vai trò là giá trị trung bình. Hai con trỏ duy trì có bao nhiêu ước số của$A$hiện có giá trị như$a \le b$, và có bao nhiêu ước của$C$thỏa mãn$c \ge b$. Bởi vì cả hai lần quét con trỏ chỉ di chuyển về phía trước nên tổng chi phí vẫn tuyến tính theo số ước. 

Một điểm tinh tế là chúng tôi không bao giờ đặt lại con trỏ cho mỗi$b$, đó là điều cần thiết cho hiệu quả. Việc đặt lại sẽ làm tăng độ phức tạp lên bậc hai trong số lượng ước số. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
1 6 1
```Sau khi sắp xếp:$A=1, B=1, C=6$Số chia:$D_A = [1]$,$D_B = [1]$,$D_C = [1,2,3,6]$| b | cntA (<= b) | cntC (>= b) | đóng góp | 
| --- | --- | --- | --- | 
| 1 | 1 | 4 | 4 | 

Đầu ra là 4. 

Điều này xác nhận rằng tất cả các lựa chọn hợp lệ đều phát sinh từ việc sửa lỗi$b=1$và tự do lựa chọn$a=1$và bất kỳ ước số nào của$6$BẰNG$c$. 

### Ví dụ 2 

đầu vào:```
1
2 2 2
```Các kích thước được sắp xếp vẫn còn$2,2,2$Số chia:$D_A = D_B = D_C = [1,2]$| b | cntA | cntC | đóng góp | 
| --- | --- | --- | --- | 
| 1 | 1 | 2 | 2 | 
| 2 | 2 | 1 | 2 | 

Tổng cộng = 4. 

Điều này phù hợp với việc liệt kê đầy đủ các bộ ba không giảm hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(\sqrt{A} + \sqrt{B} + \sqrt{C})$mỗi bài kiểm tra | Mỗi số được phân tích thành thừa số một lần và mảng ước số được quét tuyến tính | 
| Không gian |$O(d(A)+d(B)+d(C))$| Lưu trữ danh sách ước số cho từng chiều | 

Tổng thời gian chạy vẫn nằm trong giới hạn vì mỗi số đóng góp tối đa vài trăm ước số trong trường hợp xấu nhất và số trường hợp kiểm thử được xử lý độc lập bằng cách quét tuyến tính đơn giản. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdout.getvalue() if False else solve_capture(inp)

def solve_capture(inp: str) -> str:
    import sys
    input = sys.stdin.readline
    def divisors(x):
        small, large = [], []
        i = 1
        while i * i <= x:
            if x % i == 0:
                small.append(i)
                if i * i != x:
                    large.append(x // i)
            i += 1
        return small + large[::-1]

    out = []
    t = int(inp.split()[0])
    idx = 1
    lines = inp.strip().splitlines()
    for _ in range(t):
        A, B, C = map(int, lines[idx].split())
        idx += 1
        A, B, C = sorted((A, B, C))

        DA = divisors(A)
        DB = divisors(B)
        DC = divisors(C)

        DA.sort()
        DB.sort()
        DC.sort()

        i = k = 0
        ans = 0

        for b in DB:
            while i < len(DA) and DA[i] <= b:
                i += 1
            while k < len(DC) and DC[k] < b:
                k += 1
            ans += i * (len(DC) - k)

        out.append(str(ans))

    return "\n".join(out)

# provided samples
assert solve_capture("4\n1 1 1\n1 6 1\n2 2 2\n100 100 100\n") == "1\n4\n4\n165"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|$1,1,1$| 1 | Khối tối thiểu | 
|$1,6,1$| 4 | Trải rộng phân chia theo trục đơn | 
|$2,2,2$| 4 | liệt kê khối đối xứng | 
|$100,100,100$| 165 | cấu trúc ước số lớn hơn | 

## Vỏ cạnh 

Một hộp đối xứng hoàn toàn như$1 \times 1 \times 1$cho biết liệu việc triển khai có xử lý chính xác các bộ chia phần tử đơn hay không. Trong trường hợp này, mỗi danh sách ước chỉ chứa$[1]$, do đó thuật toán thực hiện một lần lặp và trả về chính xác một cấu hình. 

Một hộp có độ lệch cao như$1 \times 1 \times C$kiểm tra xem phương pháp có xử lý chính xác một chiều là không bị ràng buộc trong thực tế hay không. Ở đây, cả hai$A$Và$B$chỉ đóng góp ước số 1 và câu trả lời thu gọn về số ước số của$C$, mà thuật toán tái tạo thông qua$b=1$quét. 

Một khối lớn như$100 \times 100 \times 100$nhấn mạnh cơ chế đếm số chia. Thuật toán xử lý nó bằng cách lặp lại trung bình hơn 25 ước số trên mỗi trục và logic tiền tố tổng hợp chính xác tất cả các bộ ba không giảm hợp lệ mà không bị trùng lặp hoặc tràn.
