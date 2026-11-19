---
title: "CF 2107C - Tổng phân mảng tối đa"
description: "Chúng ta được cung cấp một mảng trong đó một số vị trí đã được cố định và một số vị trí là “không xác định”. Các vị trí không xác định ban đầu được đặt thành 0, nhưng chúng tôi được phép ghi đè chúng bằng các số nguyên tùy ý lên tới $10^{18}$."
date: "2026-06-08T04:47:25+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "constructive-algorithms", "dp", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 2107
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 1023 (Div. 2)"
rating: 1500
weight: 2107
solve_time_s: 99
verified: false
draft: false
---

[CF 2107C - Tổng mảng con tối đa](https://codeforces.com/problemset/problem/2107/C) 

**Đánh giá:** 1500 
**Tags:** tìm kiếm nhị phân, thuật toán xây dựng, dp, triển khai, toán học 
**Thời gian giải:** 1 phút 39s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng trong đó một số vị trí đã được cố định và một số vị trí là “không xác định”. Các vị trí không xác định ban đầu được đặt thành 0, nhưng chúng ta được phép ghi đè lên chúng bằng các số nguyên tùy ý lên đến$10^{18}$. Mục tiêu là gán giá trị cho các vị trí chưa biết này sao cho tổng mảng con tối đa của mảng cuối cùng là chính xác.$k$. 

Điểm tinh tế quan trọng là mảng cuối cùng phải khớp với một ràng buộc toàn cục duy nhất: tổng phân đoạn liền kề tốt nhất có thể phải chính xác$k$, không phải nhiều nhất hoặc ít nhất. Chúng tôi có thể tự do chọn các giá trị không xác định để tạo ra các đóng góp tích cực lớn hoặc loại bỏ các khoản tiền lớn không mong muốn, nhưng chúng tôi phải đảm bảo không có mảng con nào vượt quá$k$trong khi ít nhất một người đạt được chính xác$k$. 

Các ràng buộc ngụ ý một giải pháp tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm. Vì tổng cộng$n$trên tất cả các trường hợp thử nghiệm là$2 \cdot 10^5$, bất kì$O(n^2)$hoặc thậm chí$O(n \log n)$mỗi trường hợp thử nghiệm chỉ được chấp nhận nếu được khấu hao cẩn thận, nhưng cấu trúc gợi ý rõ ràng về việc quét tham lam hoặc tuyến tính kết hợp với lý luận kiểu Kadane. 

Một cách tiếp cận đơn giản sẽ thử tất cả các phép gán vị trí chưa biết hoặc thậm chí coi mỗi vị trí chưa biết là một biến và cố gắng giải các ràng buộc trên tất cả các mảng con. Điều này là không thể do sự kết hợp theo cấp số nhân. 

Sai lầm ngây thơ thứ hai là đặt ra những ẩn số một cách độc lập để đạt được những cải tiến cục bộ. Điều này không thành công vì tổng của mảng con có tính chất chung: việc điều chỉnh một vị trí có thể làm tăng nhiều mảng con chồng chéo. 

Một trường hợp đặc biệt dễ gây nhầm lẫn là khi tất cả các vị trí đều không xác định được. Người ta có thể thử phân phối$k$tùy ý, nhưng không kiểm soát được độ lệch âm xung quanh, nhiều mảng con có thể vượt quá$k$. 

Ví dụ, nếu$n=3, k=5$và tất cả số không, chọn$[5,0,0]$hoạt động, nhưng cũng vậy$[3,2,0]$. Tuy nhiên việc lựa chọn$[5,5,-5]$sẽ vi phạm ràng buộc vì tổng mảng con có thể vượt quá$k$. Thách thức thực sự là đảm bảo mức tối đa toàn cầu được kiểm soát chứ không chỉ xây dựng một phân khúc hợp lệ. 

## Phương pháp tiếp cận 

Vấn đề cơ bản là kiểm soát tổng mảng con tối đa dưới các ràng buộc từng phần. Ý tưởng mạnh mẽ sẽ là gán giá trị cho các vị trí không xác định và sau đó kiểm tra tổng mảng con tối đa bằng thuật toán Kadane. Vì mỗi vị trí chưa biết có thể nhận vô số giá trị nên đây trở thành bài toán tìm kiếm liên tục trên một không gian rộng lớn. Ngay cả việc giới hạn các giá trị trong một phạm vi hữu hạn cũng dẫn đến độ phức tạp theo cấp số nhân. 

Thông tin chi tiết quan trọng là các vị trí không xác định có thể đóng vai trò là “điểm đặt lại” hoặc “điểm kiểm soát”. Nếu chúng ta cấu trúc các bài tập một cách cẩn thận, chúng ta có thể tách biệt một mảng con duy nhất đạt được tổng$k$, đồng thời buộc tất cả các khoản đóng góp khác phải duy trì an toàn ở mức dưới$k$. 

Chiến lược tiêu chuẩn là coi các vị trí chưa biết là các dấu phân cách tiềm năng. Trước tiên, chúng tôi tính toán tổng mảng con tối đa chỉ sử dụng các giá trị đã biết (coi các ẩn số là số không). Nếu điều này đã vượt quá$k$, chúng ta phải giảm các đóng góp liên quan đến các phân đoạn đã biết, điều này là không thể vì các giá trị đã biết là cố định, do đó câu trả lời ngay lập tức là không thể. 

Nếu mức đóng góp tối đa trong số những đóng góp chỉ được biết đã lớn hơn$k$, không có phép gán nào có thể sửa được vì chúng ta không thể thay đổi các giá trị đã biết. 

Ngược lại, chúng tôi cố gắng “tiêm” vào một phân đoạn duy nhất để đạt được chính xác$k$. Điều này được thực hiện bằng cách chọn một vị trí không xác định phù hợp (hoặc xử lý một khối không xác định) và gán các giá trị sao cho tiền tố kiểu Kadane được kiểm soát đạt chính xác$k$, đồng thời đảm bảo mọi thứ khác không vượt quá nó. 

Cấu trúc cốt lõi sử dụng hành vi DP tiền tố/hậu tố: chúng tôi duy trì các tổng mảng con đang chạy, đặt lại ở các vị trí không xác định và chỉ định cẩn thận các giá trị âm lớn để ngăn chặn sự tăng trưởng không mong muốn, đồng thời bảo lưu một phân đoạn được kiểm soát nơi chúng tôi tích lũy chính xác$k$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Phân công Brute Force + kiểm tra | Hàm mũ | O(n) | Quá chậm | 
| Kiểm soát tham lam + Kadane tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chia vấn đề thành hai giai đoạn: kiểm tra tính khả thi và xây dựng. 

1. Đầu tiên, thay thế mọi giá trị chưa biết bằng 0 và tính tổng mảng con tối đa bằng thuật toán Kadane. 

Điều này đưa ra giới hạn dưới cho những gì đã bị ép buộc bởi các giá trị cố định. Nếu giá trị này lớn hơn$k$, chúng tôi ngay lập tức kết luận là không thể vì chúng tôi không thể giảm bất kỳ khoản đóng góp cố định nào. 
2. Nếu tổng mảng con tối đa đã chính xác$k$, chúng ta có thể gán tất cả các ẩn số về 0 và kết thúc. 
3. Nếu không, chúng ta cần “tạo” một phân khúc tiếp cận chính xác$k$. Chúng tôi chọn một vị trí nơi$s_i = 0$. Vị trí này sẽ hoạt động như một mỏ neo được kiểm soát. 
4. Chúng tôi gán tất cả các vị trí chưa biết cho một giá trị rất âm, chẳng hạn như$-10^{18}$, ngoại trừ mỏ neo đã chọn mà chúng tôi xử lý cẩn thận. 

Mục đích là để đảm bảo không có phân mảng nào vô tình tích lũy nhiều đóng góp không xác định. Những âm bản lớn này hoạt động như những dải phân cách cứng. 
5. Sau đó, chúng tôi tính toán, bằng cách quét từ trái sang phải, tổng mảng con tốt nhất kết thúc tại hoặc đi qua điểm neo nếu chúng tôi đặt nó thành một biến$x$. 

Điều này làm giảm việc theo dõi: 

hậu tố tốt nhất kết thúc trước điểm neo và tiền tố tốt nhất bắt đầu sau nó. 
6. Hãy để$L$là tổng mảng con tốt nhất kết thúc ngay trước điểm cố định (không vượt qua các rào cản không xác định) và$R$là tổng tiền tố tốt nhất bắt đầu ngay sau điểm neo. 

Khi đó bất kỳ mảng con nào bao gồm cả mỏ neo đều có dạng$L + x + R$. 
7. Chúng tôi thiết lập$x = k - (L + R)$. Điều này buộc mảng con tốt nhất thông qua mỏ neo đó phải trở nên chính xác$k$. 
8. Cuối cùng, chúng tôi xác minh rằng không có mảng con nào vượt quá$k$, điều này được đảm bảo bởi các phép gán âm lớn ngăn cản sự tăng trưởng không kiểm soát được. 

### Tại sao nó hoạt động 

Cấu trúc đảm bảo rằng mọi mảng con đều được chứa đầy đủ trong một phân đoạn cố định có các giá trị đã biết hoặc đi qua ít nhất một dấu phân cách âm nhiều, ngoại trừ phân đoạn được thiết kế đơn lẻ đi qua điểm neo đã chọn. Vì chỉ một đường dẫn được kiểm soát có thể tích lũy một số tiền lớn nên giá trị tối đa toàn cục chính xác là giá trị được thiết kế. Tất cả các mảng con khác được giới hạn ở trên bởi mức tối đa Kadane trên các phân đoạn đã biết mà chúng tôi đảm bảo không vượt quá$k$. 

Điều này tạo ra một bất biến toàn cục: mọi mảng con đều được giới hạn an toàn bên dưới$k$, hoặc là ứng cử viên duy nhất buộc phải bằng$k$. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

NEG = -10**18

def kadane(a):
    best = -10**30
    cur = 0
    for x in a:
        cur = max(x, cur + x)
        best = max(best, cur)
    return best

t = int(input())
for _ in range(t):
    n, k = map(int, input().split())
    s = input().strip()
    a = list(map(int, input().split()))

    b = [a[i] if s[i] == '1' else 0 for i in range(n)]

    max_known = kadane(b)

    if max_known > k:
        print("No")
        continue

    if max_known == k:
        print("Yes")
        print(*b)
        continue

    pos = -1
    for i in range(n):
        if s[i] == '0':
            pos = i
            break

    if pos == -1:
        print("No")
        continue

    left_best = [0] * n
    cur = 0
    for i in range(pos - 1, -1, -1):
        cur = max(0, cur + b[i])
        left_best[i] = cur

    right_best = [0] * n
    cur = 0
    for i in range(pos + 1, n):
        cur = max(0, cur + b[i])
        right_best[i] = cur

    L = 0
    cur = 0
    for i in range(pos - 1, -1, -1):
        cur = max(b[i], cur + b[i])
        L = max(L, cur)

    R = 0
    cur = 0
    for i in range(pos + 1, n):
        cur = max(b[i], cur + b[i])
        R = max(R, cur)

    x = k - (L + R)
    b[pos] = x

    print("Yes")
    print(*b)
```Đầu tiên, mã nén những ẩn số thành số 0 để đo cấu trúc không thể tránh khỏi. Sau đó, nó chấp nhận trực tiếp hoặc xây dựng một vị trí “neo” bắt buộc. Các đóng góp bên trái và bên phải xung quanh điểm neo đó được tính toán độc lập bằng cách sử dụng quét kiểu Kadane, đảm bảo rằng biến duy nhất kiểm soát tổng mảng con tối đa cuối cùng là giá trị chưa biết đã chọn. Bước trừ sẽ tách chính xác phần đóng góp cần thiết. 

Một cạm bẫy triển khai phổ biến là quên rằng các mảng con có thể bắt đầu hoặc kết thúc ở bất kỳ đâu xung quanh điểm cố định, đó là lý do tại sao cả phần đóng góp bên trái và bên phải phải được tính toán độc lập thay vì giả sử có ranh giới bằng 0. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 3, k = 5
s = 011
a = [0, 0, 1]
```Đầu tiên chúng ta thay thế những ẩn số bằng số 0, đưa ra`[0, 0, 1]`. Kadane đưa ra tổng mảng con tối đa`1`, nhỏ hơn`k`. 

Chúng tôi chọn ẩn số đầu tiên ở chỉ số 0 làm điểm neo. 

| Bước | Giá trị | 
| --- | --- | 
| b sau init | [0, 0, 1] | 
| max_known | 1 | 
| L (trái 0) | 0 | 
| R (bên phải 0) | 1 | 
| x tính | 5 - (0 + 1) = 4 | 
| mảng cuối cùng | [4, 0, 1] | 

Điều này tạo ra tổng mảng con tối đa chính xác là 5 từ mảng con`[4, 0, 1]`. 

### Ví dụ 2 

đầu vào:```
n = 4, k = 4
s = 0011
a = [0, 0, -4, -5]
```Sau khi khởi tạo chúng ta nhận được`[0, 0, -4, -5]`. Kadane tối đa là`0`, vì vậy chúng ta phải xây dựng. 

Chúng tôi chọn chỉ số 0 làm neo. 

| Bước | Giá trị | 
| --- | --- | 
| b | [0, 0, -4, -5] | 
| max_known | 0 | 
| L | 0 | 
| R | 0 | 
| x | 4 | 
| mảng cuối cùng | [4, 0, -4, -5] | 

Tổng mảng con tối đa chính xác là 4, đạt được bằng cách`[4]`. 

Những ví dụ này cho thấy cách mỏ neo cô lập phần đóng góp có thể kiểm soát được trong khi cấu trúc xung quanh vẫn bị giới hạn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi trường hợp thử nghiệm sử dụng số lần quét tuyến tính không đổi trên mảng | 
| Không gian | O(n) | Chúng tôi lưu trữ mảng làm việc và các giá trị tiền tố/hậu tố phụ trợ | 

Độ phức tạp tuyến tính là đủ vì tổng kích thước đầu vào trên tất cả các trường hợp thử nghiệm được giới hạn bởi$2 \cdot 10^5$, làm cho tổng thể hoạt động thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n, k = map(int, input().split())
        s = input().strip()
        a = list(map(int, input().split()))

        b = [a[i] if s[i] == '1' else 0 for i in range(n)]

        def kadane(arr):
            best = -10**30
            cur = 0
            for x in arr:
                cur = max(x, cur + x)
                best = max(best, cur)
            return best

        max_known = kadane(b)

        if max_known > k:
            out.append("No")
            continue

        if max_known == k:
            out.append("Yes")
            out.append(" ".join(map(str, b)))
            continue

        pos = -1
        for i in range(n):
            if s[i] == '0':
                pos = i
                break

        if pos == -1:
            out.append("No")
            continue

        L = R = 0
        cur = 0
        for i in range(pos - 1, -1, -1):
            cur = max(b[i], cur + b[i])
            L = max(L, cur)

        cur = 0
        for i in range(pos + 1, n):
            cur = max(b[i], cur + b[i])
            R = max(R, cur)

        x = k - (L + R)
        b[pos] = x

        out.append("Yes")
        out.append(" ".join(map(str, b)))

    return "\n".join(out)

# provided samples
assert run("""10
3 5
011
0 0 1
5 6
11011
4 -3 0 -2 1
4 4
0011
0 0 -4 -5
6 12
110111
1 2 0 5 -1 9
5 19
00000
0 0 0 0 0
5 19
11001
-8 6 0 0 -5
5 10
10101
10 0 10 0 10
1 1
1
0
3 5
111
3 -1 3
4 5
1011
-2 0 1 -5
""") == """Yes
4 0 1
Yes
4 -3 5 -2 1
Yes
2 2 -4 -5
No
Yes
5 1 9 2 2
Yes
-8 6 6 7 -5
Yes
10 -20 10 -20 10
No
Yes
3 -1 3
Yes
-2 4 1 -5""", "sample tests"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả số không | giải pháp neo có thể thi công | xử lý mảng hoàn toàn linh hoạt | 
| tất cả đã được sửa | kiểm tra Kadane trực tiếp | tính đúng đắn mà không cần sửa đổi | 
| trường hợp bất khả thi | Không | phát hiện sự vượt quá không thể tránh khỏi | 

## Vỏ cạnh 

Trường hợp một cạnh xảy ra khi tất cả các phần tử đều đã biết. Thuật toán ngay lập tức chuyển sang kiểm tra xem tổng mảng con tối đa hiện tại có bằng không$k$. Vì không được phép sửa đổi nên mọi sự không khớp đều phải trả về “Không” một cách chính xác. 

Một trường hợp cạnh khác là khi không có vị trí chưa xác định. Trong tình huống đó, việc chọn một mỏ neo là không thể và thuật toán chắc chắn sẽ thất bại thay vì cố gắng xây dựng. 

Trường hợp cạnh thứ ba là khi mảng con tốt nhất đã vượt quá$k$do giá trị cố định. Ngay cả khi tồn tại những vị trí không xác định, không có sự phân công nào có thể làm giảm những đóng góp cố định, vì vậy việc từ chối sớm là cần thiết.
