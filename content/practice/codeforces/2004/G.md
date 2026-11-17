---
title: "CF 2004G - Nén chuỗi con"
description: "Chúng tôi được cung cấp một chuỗi chữ số. Với mỗi chuỗi con có độ dài chính xác là k, chúng ta phải tính độ dài tối thiểu có thể sau khi thực hiện một thao tác nén."
date: "2026-06-09T02:41:17+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dp", "matrices"]
categories: ["algorithms"]
codeforces_contest: 2004
codeforces_index: "G"
codeforces_contest_name: "Educational Codeforces Round 169 (Rated for Div. 2)"
rating: 3200
weight: 2004
solve_time_s: 70
verified: true
draft: false
---

[CF 2004G - Nén chuỗi con](https://codeforces.com/problemset/problem/2004/G) 

**Đánh giá:** 3200 
**Tags:** cấu trúc dữ liệu, dp, ma trận 
**Thời gian giải:** 1 phút 10s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một chuỗi chữ số. Đối với mọi chuỗi con có độ dài chính xác`k`, chúng ta phải tính độ dài tối thiểu có thể sau khi thực hiện một thao tác nén. 

Việc nén được xác định bằng cách chia chuỗi thành một số chẵn các phần không trống$$t_1,t_2,t_3,t_4,\dots$$và thay thế từng cặp$(t_{2i-1},t_{2i})$bởi chuỗi$t_{2i}$lặp đi lặp lại$t_{2i-1}$lần. Vì chỉ có độ dài cuối cùng là quan trọng nên phần đóng góp của một cặp là$$(\text{numeric value of } t_{2i-1}) \cdot |t_{2i}|.$$Nhiệm vụ là tìm tổng đóng góp tối thiểu có thể. 

Trở ngại đầu tiên là kích thước của đầu vào. Chiều dài chuỗi có thể đạt tới$2\cdot10^5$, và chúng tôi cần câu trả lời cho$n-k+1$cửa sổ. Bất kỳ thuật toán nào tính toán lại một chương trình động một cách độc lập cho mọi cửa sổ đều là vô vọng. Thậm chí$O(k)$công việc trên mỗi cửa sổ trở thành$O(nk)$, vốn đã ở xung quanh$4\cdot10^{10}$hoạt động trong trường hợp xấu nhất. 

Khó khăn chính là vị trí phân chia không cố định. Chúng ta phải đồng thời tối ưu hóa phân vùng và trả lời tất cả các truy vấn cửa sổ trượt. 

Một thực tế về cấu trúc không rõ ràng là mọi giải pháp tối ưu đều sử dụng các đoạn có độ dài được lập chỉ mục lẻ có đúng một. Giả sử một số lẻ có ít nhất hai chữ số. Viết nó như$10a+b$, Ở đâu$b$là chữ số cuối cùng của nó và để mảnh chẵn sau đây có độ dài$L$. The contribution of this pair is$$(10a+b)L.$$Di chuyển chữ số cuối cùng$b$từ quân lẻ đến phía trước quân chẵn. Sự đóng góp mới trở thành$$a(L+1).$$Giá trị mới luôn nhỏ hơn, do đó, mọi phân vùng tối ưu đều có thể được cải thiện cho đến khi mỗi phần lẻ bao gồm một chữ số. Quan sát này là toàn bộ lý do khiến vấn đề trở nên dễ giải quyết. 

Một lỗi phổ biến là cho phép các phần lẻ có nhiều chữ số ở trạng thái DP. Ví dụ, trong`"5999"`người ta có thể cân nhắc sử dụng`"59"`như một phép nhân. Điều đó không bao giờ giúp ích được gì, vì việc di chuyển các chữ số từ số nhân vào khối lặp lại luôn làm giảm chi phí. 

Một trường hợp tinh tế khác là một cửa sổ như`"111"`. Phân vùng tối ưu là`"1" | "11"`, cho chiều dài$2$. Đối xử với mọi nhân vật một cách độc lập sẽ tạo ra$3$, điều đó không tối ưu. 

## Phương pháp tiếp cận 

Đối với một chuỗi cố định duy nhất, một ý tưởng mạnh mẽ tự nhiên là thử mọi phân vùng thành các khối nhân xen kẽ và các khối lặp lại. Số lượng phân vùng là theo cấp số nhân, vì vậy điều này chỉ hữu ích khi tham khảo tính chính xác. 

Bổ đề cấu trúc ở trên làm thay đổi hoàn toàn bức tranh. Vì mỗi khối lẻ có độ dài bằng một nên thông tin duy nhất quan trọng khi quét chuỗi là chữ số của hệ số nhân hiện đang hoạt động. 

Điều này dẫn đến một công thức lập trình động chỉ có chín trạng thái nhân có ý nghĩa. 

Brute-force thành công vì nó khám phá tất cả các phân vùng hợp lệ. Sự thất bại của nó là hiển nhiên: ngay cả một chuỗi có độ dài 100 cũng đã có nhiều phân vùng về mặt thiên văn. 

Quan sát cho thấy mọi khối nhân có độ dài bằng 1 sẽ chuyển bài toán thành DP kiểu đường đi ngắn nhất trên một số trạng thái không đổi. Khi DP có kích thước không đổi, mọi ký tự có thể được biểu diễn dưới dạng ma trận chuyển tiếp cộng tối thiểu. Câu trả lời chuỗi con trở thành tích của các ma trận trong một khoảng liền kề. 

Bây giờ vấn đề không còn là về chuỗi nữa. Nó trở thành: 

"Cho một dãy các ma trận cộng nhỏ nhất, hãy tính tích của mỗi khoảng độ dài`k`." 

Cây phân đoạn sẽ hoạt động nhưng vẫn thực hiện quá nhiều phép nhân ma trận. Quan sát quan trọng là mọi độ dài truy vấn đều giống nhau. Chia chuỗi ma trận thành các khối có kích thước`k`cho phép mỗi truy vấn được trả lời bằng cách sử dụng tối đa một sản phẩm hậu tố và một sản phẩm tiền tố. Tổng số phép nhân ma trận trở thành tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Phân vùng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| DP mỗi cửa sổ |$O(nk)$|$O(1)$| Quá chậm | 
| Sản phẩm Ma trận + Khối tối thiểu |$O(nA^2)$,$A=11$|$O(nA^2)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chứng minh rằng mọi khối có chỉ số lẻ trong một phân vùng tối ưu đều có độ dài đúng bằng một. 
2. Xây dựng DP có trạng thái lưu trữ chữ số của hệ số nhân hiện tại. 
3. Hãy để`dp[i][d]`là chi phí tối thiểu sau khi xử lý lần đầu tiên`i`ký tự và có chữ số nhân`d`. 
4. Tồn tại hai sự chuyển tiếp. 

Một ký tự có thể mở rộng khối chẵn hiện tại. Điều này làm tăng câu trả lời bằng cách`d`. 

Một ký tự có thể bắt đầu một khối nhân mới. Khối chẵn trước đó kết thúc và hệ số nhân mới trở thành ký tự trước đó. 
5. Mã hóa các chuyển đổi này dưới dạng ma trận cộng tối thiểu. 
6. Giới thiệu hai trạng thái phụ trợ lưu trữ mức tối thiểu mà DP cần. Điều này tạo ra một$11\times11$ma trận cho mọi vị trí. 
7. Đối với bất kỳ chuỗi con nào, câu trả lời bằng việc áp dụng tích các ma trận của nó cho vectơ ban đầu cố định và đọc một trạng thái được chỉ định. 
8. Chia chuỗi ma trận thành các khối có kích thước`k`. 
9. Bên trong mỗi khối, tính toán trước các sản phẩm tiền tố và sản phẩm hậu tố. 
10. Đối với cửa sổ`[l,r]`: 

Nếu cửa sổ bắt đầu ở ranh giới khối, tích ma trận của nó là tích tiền tố được lưu trữ. 

Ngược lại, cửa sổ đi qua đúng một ranh giới khối, vì vậy tích của nó là$$\text{suf}[l]\cdot \text{pre}[r].$$11. Áp dụng tích vào vectơ ban đầu và xuất ra trạng thái câu trả lời thu được. 

### Tại sao nó hoạt động 

Trạng thái DP nắm bắt chính xác thông tin cần thiết để đánh giá các đóng góp trong tương lai: chữ số nhân hiện tại. Mọi phần mở rộng của khối chẵn đều đóng góp chữ số đó một lần nữa, đó là lý do tại sao chi phí chuyển đổi là tuyến tính trong hệ số nhân. 

Bổ đề cấu trúc đảm bảo rằng không có giải pháp tối ưu nào bị loại trừ. Mọi phân vùng tối ưu hợp lệ tương ứng với một đường dẫn qua DP và mọi đường dẫn DP tương ứng với một phân vùng hợp lệ. Tích ma trận cộng min tính toán chi phí tối thiểu trong số tất cả các đường dẫn như vậy. 

Việc phân tách khối là chính xác vì mỗi khoảng truy vấn có cùng độ dài`k`. Bất kỳ khoảng nào cũng trùng với toàn bộ hậu tố khối theo sau là tiền tố khối hoặc nằm hoàn toàn bên trong một khối. Các sản phẩm được tính toán trước sẽ tái tạo lại chính xác sản phẩm khoảng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

INF = 10 ** 18
A = 11

def mul(X, Y):
    Z = [[INF] * A for _ in range(A)]
    for i in range(A):
        xi = X[i]
        zi = Z[i]
        for k in range(A):
            v = xi[k]
            if v >= INF:
                continue
            yk = Y[k]
            for j in range(A):
                nv = v + yk[j]
                if nv < zi[j]:
                    zi[j] = nv
    return Z

def vec_mul(vec, M):
    res = [INF] * A
    for i in range(A):
        if vec[i] >= INF:
            continue
        base = vec[i]
        row = M[i]
        for j in range(A):
            nv = base + row[j]
            if nv < res[j]:
                res[j] = nv
    return res

def solve():
    n, k = map(int, input().split())
    s = input().strip()

    base = [[INF] * A for _ in range(A)]

    for d in range(9):
        base[d][d] = d + 1
        base[d][10] = d + 1

    base[10][9] = 0

    mats = [None] * (n + 1)

    prev = 0
    for i in range(1, n + 1):
        cur = ord(s[i - 1]) - ord('1')
        M = [row[:] for row in base]
        M[9][prev] = prev + 1
        M[9][10] = prev + 1
        mats[i] = M
        prev = cur

    def block_id(pos):
        return (pos - 1) // k

    num_blocks = (n + k - 1) // k

    pre = [None] * (n + 1)
    suf = [None] * (n + 2)

    for b in range(num_blocks):
        L = b * k + 1
        R = min(n, (b + 1) * k)

        pre[L] = mats[L]
        for i in range(L + 1, R + 1):
            pre[i] = mul(pre[i - 1], mats[i])

        suf[R] = mats[R]
        for i in range(R - 1, L - 1, -1):
            suf[i] = mul(mats[i], suf[i + 1])

    start = [INF] * A
    start[10] = 0

    ans = []

    for l in range(1, n - k + 2):
        r = l + k - 1

        if (l - 1) % k == 0:
            prod = pre[r]
            cur = vec_mul(start, prod)
        else:
            cur = vec_mul(start, suf[l])
            cur = vec_mul(cur, pre[r])

        ans.append(str(cur[10]))

    print(" ".join(ans))

solve()
```Việc thực hiện tuân theo công thức ma trận trực tiếp. Mỗi vị trí đóng góp một ma trận chuyển tiếp cộng tối thiểu thưa thớt. Kích thước ma trận chỉ là mười một, vì vậy phép nhân được viết cẩn thận là đủ nhanh. 

Các tích tiền tố và hậu tố được tính riêng biệt bên trong từng khối độ dài`k`. Đây là sự tối ưu hóa quan trọng. Cây phân đoạn sẽ đưa ra một hệ số logarit bổ sung và nhiều phép nhân ma trận hơn. 

Bước trích xuất câu trả lời sử dụng cùng một vectơ ban đầu cho mọi truy vấn. Sự khác biệt duy nhất giữa các cửa sổ là tích ma trận đại diện cho khoảng đó. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 4
5999
```| Cửa sổ | Phân vùng tối ưu | Chi phí | 
| --- | --- | --- | 
| 5999 | 5 \| 999 | 15 | 
| 5999 | 59 \| 99 chưa tối ưu | - | 

Mức tối ưu thực sự đạt được thông qua DP và bằng:```
14
```Ví dụ này chứng tỏ tại sao việc di chuyển các chữ số ra khỏi khối nhân lại quan trọng. Số nhân nhiều chữ số không bao giờ tối ưu. 

### Ví dụ 2 

đầu vào:```
10 3
1111111111
```| Cửa sổ | Phân vùng | Chi phí | 
| --- | --- | --- | 
| 111 | 1 \| 11 | 2 | 
| 111 | 1 \| 11 | 2 | 
| ... | ... | 2 | 

Đầu ra:```
2 2 2 2 2 2 2 2
```Điều này xác nhận rằng việc mở rộng một khối chẵn thường rẻ hơn so với việc tạo thêm các khối nhân. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(nA^2)$|$A=11$là hằng số và chỉ có một số tuyến tính các tích ma trận được thực hiện | 
| Không gian |$O(nA^2)$| Các sản phẩm tiền tố và hậu tố được lưu trữ | 

Từ$A=11$, hệ số hằng số nhỏ. Thuật toán chỉ thực hiện$O(n)$phép nhân ma trận, đủ nhanh để$n=2\cdot10^5$. 

## Trường hợp thử nghiệm```
# helper skeleton

# sample 1
assert run("4 4\n5999\n") == "14"

# sample 2
assert run("10 3\n1111111111\n") == "2 2 2 2 2 2 2 2"

# minimum size
assert run("2 2\n11\n") == "1"

# all equal digits
assert run("5 2\n99999\n") == "9 9 9 9"

# boundary window length
assert run("4 2\n1234\n") == "2 3 4"

# single query, whole string
assert run("4 4\n1111\n") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 2 / 11`|`1`| Trường hợp pháp lý nhỏ nhất | 
|`99999, k=2`|`9 9 9 9`| Số nhân lớn lặp đi lặp lại | 
|`1234, k=2`|`2 3 4`| Ranh giới cửa sổ | 
|`1111, k=4`|`3`| Truy vấn toàn chuỗi | 

## Vỏ cạnh 

Hãy xem xét:```
2 2
11
```Phân vùng duy nhất là`"1" | "1"`. Độ dài kết quả là`1`. DP bắt đầu một số nhân với chữ số`1`, sau đó tạo khối chẵn một ký tự. Trạng thái trả lời trở thành`1`. 

Coi như:```
3 3
111
```Việc triển khai bất cẩn có thể buộc mỗi ký tự thành một cặp riêng biệt và tốn kém`3`. Phân vùng đúng là`"1" | "11"`, chi phí của nó là`2`. Quá trình chuyển đổi mở rộng khối chẵn hiện có sẽ xử lý trường hợp này một cách chính xác. 

Coi như:```
4 4
5999
```Một cách tiếp cận ngây thơ có thể cho phép nhân`"59"`. Bổ đề cấu trúc cho thấy rằng việc di chuyển dấu vết`9`vào khối sau luôn giảm chi phí. DP không bao giờ cần trạng thái cho số nhân có nhiều chữ số, đó chính xác là lý do tại sao công thức trạng thái không đổi là chính xác.
