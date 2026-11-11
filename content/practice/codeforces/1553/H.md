---
title: "CF 1553H - XOR và Khoảng cách"
description: "Chúng ta có một tập hợp các số nguyên riêng biệt $a1,dots,an$, mỗi số nằm trong phạm vi $[0,2^k)$. Đối với mỗi mặt nạ $x$, chúng tôi XOR mọi phần tử mảng có $x$, tạo ra tập hợp $${a1oplus x,dots,anoplus x}.$$ Trong số tất cả các cặp trong tập hợp được chuyển đổi đó, chúng tôi muốn có sự khác biệt tuyệt đối nhỏ nhất."
date: "2026-06-10T13:07:43+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "divide-and-conquer", "trees"]
categories: ["algorithms"]
codeforces_contest: 1553
codeforces_index: "H"
codeforces_contest_name: "Harbour.Space Scholarship Contest 2021-2022 (open for everyone, rated, Div. 1 + Div. 2)"
rating: 2900
weight: 1553
solve_time_s: 169
verified: true
draft: false
---

[CF 1553H - XOR và Khoảng cách](https://codeforces.com/problemset/problem/1553/H) 

**Xếp hạng:** 2900 
**Tags:** bitmasks, phân chia và chinh phục, cây cối 
**Thời gian giải:** 2m 49s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Ta có tập hợp các số nguyên phân biệt$a_1,\dots,a_n$, mỗi cái nằm trong khoảng$[0,2^k)$. 

Cho mỗi chiếc mặt nạ$x$, chúng tôi XOR mọi phần tử mảng với$x$, sản xuất bộ$$\{a_1\oplus x,\dots,a_n\oplus x\}.$$Trong số tất cả các cặp trong tập được biến đổi đó, chúng ta muốn có sự khác biệt tuyệt đối nhỏ nhất. Hãy để giá trị đó là$f(x)$. 

Nhiệm vụ là tính toán$f(x)$cho mỗi mặt nạ$x\in[0,2^k)$. 

Điều đầu tiên khiến vấn đề này trở nên khó khăn là có tới$2^{19}=524288$những chiếc mặt nạ khác nhau, và chúng ta cần câu trả lời cho từng chiếc mặt nạ đó. Bất kỳ giải pháp nào xử lý từng mặt nạ truy vấn một cách độc lập đều đang gặp rắc rối. 

Trở ngại thứ hai đó là$n$cũng có thể lớn như$2^k$. Ngay cả việc kiểm tra tất cả các cặp để tìm một mặt nạ cũng sẽ mất$O(n^2)$, điều đó là hoàn toàn không thể. 

Phần thú vị là$k\le 19$. Bất cứ khi nào độ rộng bit nhỏ như vậy, một giải pháp hoạt động trực tiếp trên toàn bộ$2^k$không gian mặt nạ trở nên hiện thực. Xung quanh sự phức tạp$O(k2^k)$là khoảng mười triệu hoạt động, vừa vặn thoải mái. 

Một sai lầm phổ biến là nghĩ rằng XOR bảo toàn sự khác biệt. Nó không. Ví dụ, với số$0$Và$3$, sự khác biệt là$3$, nhưng sau XOR với$1$họ trở thành$1$Và$2$, sự khác biệt của nó chỉ là$1$. 

Một cái bẫy dễ dàng khác là giả sử cặp gần nhất trước khi XOR vẫn là cặp gần nhất sau đó. Coi như:```
a = [0, 3, 6]
```Vì$x=0$, câu trả lời là$3$. Vì$x=1$, các số được biến đổi là$[1,2,7]$, và câu trả lời trở thành$1$. Danh tính của cặp tối ưu đã thay đổi. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. 

Cho mỗi chiếc mặt nạ$x$, tính toán tất cả các giá trị$a_i\oplus x$, sắp xếp chúng và quét các phần tử liền kề để tìm ra sự khác biệt nhỏ nhất. Cặp gần nhất trong một mảng được sắp xếp phải liền kề nhau. 

Điều này đúng nhưng độ phức tạp$$2^k \cdot O(n\log n).$$Trong trường hợp xấu nhất$n=2^k=524288$, có giá trị lớn về mặt thiên văn. 

Quan sát quan trọng là chúng ta không thực sự quan tâm đến bản thân các số được biến đổi. Chúng tôi chỉ quan tâm đến ba số lượng cho mỗi mặt nạ:$$\text{minimum value}, \qquad \text{maximum value}, \qquad \text{minimum pair distance}.$$Các đại lượng này có thể được hợp nhất đệ quy dọc theo cấu trúc bit của các số. 

Hãy nghĩ về tất cả$k$-bit số như lá của một trie nhị phân. Một nút ở bit$d$chia thành các số có$d$-bit thứ là$0$và những con số có$d$-bit thứ là$1$. 

Khi chúng tôi quyết định$d$-bit thứ của mặt nạ truy vấn$x$, XOR giữ hai nửa đó theo thứ tự tự nhiên hoặc hoán đổi chúng. Độ lệch tương đối giữa các nửa chính xác là$2^d$. 

Điều đó có nghĩa là nếu chúng ta đã biết, đối với cả hai phần tử con, giá trị biến đổi tối thiểu, giá trị biến đổi tối đa và câu trả lời đúng nhất thì chúng ta có thể hợp nhất chúng trong thời gian không đổi. 

Chạy sự hợp nhất này cho từng bit và mọi mặt nạ đều mang lại một$O(k2^k)$giải pháp. Phép truy hồi về cơ bản là một DP chia để trị trên siêu khối. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^k n\log n)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(k2^k)$|$O(2^k)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Định nghĩa trạng thái 

Cho mỗi chiếc mặt nạ$m$, duy trì ba giá trị.`mn[m]`là giá trị chuyển đổi tối thiểu hiện có thể đạt được.`mx[m]`là giá trị chuyển đổi tối đa hiện có thể đạt được.`F[m]`là khoảng cách tối thiểu giữa hai giá trị được chuyển đổi bất kỳ. 

Ban đầu chỉ tồn tại các phần tử mảng ban đầu. 

Đối với mọi giá trị có trong mảng:```
mn[value] = mx[value] = 0
```Với mọi giá trị vắng mặt:```
mn = +INF
mx = -INF
F  = +INF
```### Hợp nhất giải thích 

Giả sử chúng ta đang xử lý bit$d$. 

Cho phép```
l = mask with bit d = 0
r = l xor (1<<d)
```đại diện cho hai nút con của nút tri. 

Nếu bit truy vấn$x_d=0$, đứa trẻ phù hợp sẽ đóng góp thêm$2^d$. 

Nếu bit truy vấn$x_d=1$, đứa trẻ bên trái đóng góp thêm$2^d$. 

Điều này mang lại sự chuyển đổi cho cực tiểu và cực đại:$$mn[l]=\min(mn_l,mn_r+2^d)$$

$$mn[r]=\min(mn_r,mn_l+2^d)$$và tương tự cho cực đại. 

### Tính khoảng cách tốt nhất 

Cặp tối ưu có thể nằm hoàn toàn bên trong một phần tử con hoặc có thể sử dụng một phần tử từ mỗi phần tử con. 

Trường hợp đầu tiên đã được lưu trữ ở trẻ em. 

Đối với một cặp chéo, sau khi đặt hàng, khoảng cách nhỏ nhất có thể là$$(\text{minimum right})-(\text{maximum left}).$$Dưới$x_d=0$, điều đó trở thành$$mn[r]-mx[l]+2^d.$$Dưới$x_d=1$, nó trở thành$$mn[l]-mx[r]+2^d.$$Chúng tôi giảm thiểu câu trả lời với những giá trị đó. 

### Thủ tục đầy đủ 

1. Tạo mảng`mn`,`mx`, Và`F`kích thước$2^k$. 
2. Khởi tạo số hiện tại bằng`mn=mx=0`. 
3. Xử lý các bit từ thấp đến cao. 
4. Đối với mỗi cặp mặt nạ chỉ khác nhau ở bit hiện tại, hãy hợp nhất thông tin của chúng. 
5. Cập nhật`F`sử dụng cả câu trả lời con và ứng cử viên con chéo. 
6. Sau khi tất cả các bit được xử lý,`F[x]`bằng với câu trả lời cần thiết cho mặt nạ`x`. 

### Tại sao nó hoạt động 

Ở bất kỳ giai đoạn nào, một trạng thái biểu thị tất cả các số bên trong cây con trie sau khi áp dụng các bit truy vấn đã được xử lý.`mn`Và`mx`mô tả các giá trị được biến đổi cực độ bên trong cây con đó.`F`lưu trữ khoảng cách cặp tốt nhất hoàn toàn có trong cây con đó. 

Khi hai cây con được hợp nhất, mỗi cặp hợp lệ thuộc về đúng một trong ba loại: hoàn toàn ở con bên trái, hoàn toàn ở con bên phải hoặc giao thoa giữa chúng. Sự truy hồi rõ ràng có mức tối thiểu trong cả ba khả năng. 

Vì mọi cặp đều được xem xét chính xác tại nút thấp nhất chứa cả hai phần tử nên không có ứng cử viên nào bị bỏ sót và không có ứng cử viên không hợp lệ nào được đưa ra. Bằng cách cảm ứng trên các bit đã được xử lý, kết quả cuối cùng`F[x]`chính xác là khoảng cách cặp tối thiểu cho mặt nạ truy vấn`x`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    a = list(map(int, input().split()))

    M = 1 << k
    INF = 10 ** 9

    mn = [INF] * M
    mx = [-INF] * M
    F = [INF] * M

    for x in a:
        mn[x] = 0
        mx[x] = 0

    for d in range(k):
        bit = 1 << d

        for r in range(M):
            if (r >> d) & 1:
                l = r ^ bit

                if F[r] < F[l]:
                    F[l] = F[r]
                elif F[l] < F[r]:
                    F[r] = F[l]

                v = mn[r] - mx[l] + bit
                if v < F[l]:
                    F[l] = v

                v = mn[l] - mx[r] + bit
                if v < F[r]:
                    F[r] = v

                mnl, mnr = mn[l], mn[r]
                mxl, mxr = mx[l], mx[r]

                mn[l] = min(mnl, mnr + bit)
                mn[r] = min(mnr, mnl + bit)

                mx[l] = max(mxl, mxr + bit)
                mx[r] = max(mxr, mxl + bit)

    print(*F)

solve()
```Việc khởi tạo đặt một khoảng có độ dài bằng 0 ở mọi giá trị có trong tập đầu vào. Các giá trị vắng mặt hoạt động như các lá trống thông qua`INF`Và`-INF`lính canh. 

Vòng lặp bên ngoài xử lý các bit giống hệt như việc hợp nhất các mức của bộ ba nhị phân. Mỗi cặp`(l,r)`tương ứng với hai cây con anh em. 

Phần tinh tế đang cập nhật`F`trước khi ghi đè`mn`Và`mx`. Khoảng cách cây con chéo phải sử dụng thông tin con cũ. Việc cập nhật cực trị trước sẽ làm hỏng phép truy xuất. 

Một chi tiết nữa là mảng đáp án chính xác`F`. Sau cấp độ hợp nhất cuối cùng, mỗi chỉ mục tương ứng với một mặt nạ truy vấn hoàn chỉnh. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 3
6 0 3
```Mặt nạ hoạt động ban đầu: 

| Mặt nạ | Hiện tại | 
| --- | --- | 
| 0 | Có | 
| 3 | Có | 
| 6 | Có | 

Kết quả cuối cùng: 

| x | f(x) | 
| --- | --- | 
| 0 | 3 | 
| 1 | 1 | 
| 2 | 1 | 
| 3 | 2 | 
| 4 | 2 | 
| 5 | 1 | 
| 6 | 1 | 
| 7 | 3 | 

Đầu ra:```
3 1 1 2 2 1 1 3
```Ví dụ này cho thấy rằng việc thay đổi mặt nạ truy vấn có thể thay đổi hoàn toàn cặp nào trở nên tối ưu. 

### Ví dụ 2 

đầu vào:```
3 4
13 4 2
```Câu trả lời cuối cùng: 

| x | f(x) | 
| --- | --- | 
| 0 | 2 | 
| 1 | 2 | 
| 2 | 6 | 
| 3 | 6 | 
| ... | ... | 

Đầu ra:```
2 2 6 6 3 1 2 2 2 2 1 3 6 6 2 2
```Ví dụ này chứng tỏ rằng câu trả lời rất không đơn điệu đối với mặt nạ truy vấn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(k2^k)$| Mỗi bit xử lý tất cả các mặt nạ một lần | 
| Không gian |$O(2^k)$| Ba mảng kích thước$2^k$| 

Với$k\le 19$, chúng ta có nhiều nhất$524288$mặt nạ. Tổng khối lượng công việc là khoảng$19 \cdot 524288$, dễ dàng phù hợp trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    input = sys.stdin.readline

    def solve():
        n, k = map(int, input().split())
        a = list(map(int, input().split()))

        M = 1 << k
        INF = 10 ** 9

        mn = [INF] * M
        mx = [-INF] * M
        F = [INF] * M

        for x in a:
            mn[x] = 0
            mx[x] = 0

        for d in range(k):
            bit = 1 << d
            for r in range(M):
                if (r >> d) & 1:
                    l = r ^ bit

                    F[l] = min(F[l], F[r])
                    F[r] = min(F[r], F[l])

                    F[l] = min(F[l], mn[r] - mx[l] + bit)
                    F[r] = min(F[r], mn[l] - mx[r] + bit)

                    mnl, mnr = mn[l], mn[r]
                    mxl, mxr = mx[l], mx[r]

                    mn[l] = min(mnl, mnr + bit)
                    mn[r] = min(mnr, mnl + bit)

                    mx[l] = max(mxl, mxr + bit)
                    mx[r] = max(mxr, mxl + bit)

        print(*F)

    solve()
    out = sys.stdout.getvalue()

    sys.stdin = old_stdin
    sys.stdout = old_stdout

    return out

# provided samples
assert run("3 3\n6 0 3\n") == "3 1 1 2 2 1 1 3\n"

assert run("3 4\n13 4 2\n") == \
       "2 2 6 6 3 1 2 2 2 2 1 3 6 6 2 2\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu 1 |`3 1 1 2 2 1 1 3`| Tính đúng đắn cơ bản | 
| Mẫu 2 | Cho đầu ra | Độ rộng bit lớn hơn | 
| Hai số liền kề | Tất cả các câu trả lời bằng 1 | Khoảng cách tối thiểu có thể | 
| Trọn bộ mặt nạ | Tất cả các câu trả lời bằng 1 | Hành vi đầu vào dày đặc | 
| Số ở hai nửa đối diện | Có thể có câu trả lời lớn | Chuyển đổi cây con chéo | 

## Vỏ cạnh 

Hãy xem xét:```
2 1
0 1
```Đối với cả hai mặt nạ truy vấn, tập hợp được chuyển đổi vẫn giữ nguyên`{0,1}`theo một thứ tự nào đó. Câu trả lời luôn là`1`. 

DP xử lý việc này một cách tự nhiên vì cặp hợp lệ duy nhất là cặp con chéo ở mức hợp nhất thấp nhất, tạo ra khoảng cách$2^0=1$. 

Bây giờ hãy xem xét:```
2 3
0 7
```Đối với mỗi mặt nạ truy vấn, các giá trị được chuyển đổi vẫn là điểm cuối bổ sung của phạm vi. Sự khác biệt của họ luôn`7`. 

Phép lặp tính toán điều này thông qua việc hợp nhất nhiều cây con lặp đi lặp lại. Không có ứng cử viên nào nhỏ hơn tồn tại bên trong bất kỳ cây con nào vì mỗi cây con chỉ chứa một phần tử. 

Cuối cùng, hãy xem xét trường hợp cặp gần nhất thay đổi sau XOR:```
3 3
0 3 6
```Tại`x=0`, câu trả lời là`3`. 

Tại`x=1`, các giá trị được chuyển đổi là`1,2,7`, và câu trả lời trở thành`1`. 

Vì DP không theo dõi một cặp cụ thể mà thay vào đó theo dõi mức tối thiểu trên tất cả các cặp có thể có trong mỗi cây con nên nó sẽ tự động điều chỉnh khi cặp tối ưu thay đổi.
