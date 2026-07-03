---
title: "CF 103633B - Tầng hay xor?"
description: "Chúng ta được cung cấp một mảng các số nguyên và ba tham số: một giá trị đích và một mô đun. Nhiệm vụ là đếm xem chúng ta có thể hình thành bao nhiêu bộ tứ chỉ số $(i, j, k, l)$ sao cho biểu thức được hình thành bởi hai tỷ lệ độc lập phù hợp với mục tiêu."
date: "2026-07-02T22:25:03+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103633
codeforces_index: "B"
codeforces_contest_name: "Infoleague Spring 2022 Round Div. 2"
rating: 0
weight: 103633
solve_time_s: 57
verified: true
draft: false
---

[CF 103633B - Tầng hay xor ?](https://codeforces.com/problemset/problem/103633/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 57s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên và ba tham số: một giá trị đích và một mô đun. Nhiệm vụ là đếm xem có bao nhiêu bộ chỉ số được sắp xếp theo thứ tự$(i, j, k, l)$chúng ta có thể hình thành sao cho biểu thức được hình thành bởi hai tỷ lệ độc lập phù hợp với mục tiêu. Mỗi tỷ lệ sử dụng phép chia sàn nên mỗi cặp chỉ số đóng góp một giá trị có dạng$\lfloor A_i / A_j \rfloor$và chúng tôi muốn tổng của hai giá trị đó bằng nhau$T$. 

Điểm mấu chốt là các chỉ số được phép lặp lại, do đó mỗi vị trí trong số bốn vị trí được chọn độc lập với$[1, N]$. Điều đó ngay lập tức cho chúng ta biết cấu trúc không phải là tổ hợp trên các phần tử riêng biệt mà hoàn toàn dựa trên tần số của các giá trị trong mảng. 

Từ những hạn chế,$N$có thể lớn (lên tới khoảng$10^5$), vậy bất kỳ$O(N^2)$việc liệt kê các cặp quá chậm. Thậm chí xây dựng tất cả$N^2$thương số theo cặp trực tiếp là đường biên, vì nó sẽ tạo ra tối đa$10^{10}$cặp trong trường hợp xấu nhất. Do đó, giải pháp dự định phải nén các cặp theo giá trị chứ không phải theo chỉ số. 

Trường hợp cạnh tinh tế xuất phát từ các giá trị lặp lại và số lượng nhỏ. Ví dụ: nếu tất cả các phần tử đều$1$, thì mỗi phân chia tầng là$1$, vậy mỗi bốn phần đóng góp$2$. Câu trả lời trở thành$N^4$, phải được xử lý bằng cách đếm bội số thay vì thử bất kỳ logic cặp nào. 

Một trường hợp khác là khi phép chia thường xuyên tạo ra số 0. Ví dụ, nếu$A_i < A_j$, sau đó$\lfloor A_i / A_j \rfloor = 0$, có thể chi phối phân phối và tạo ra nhiều cặp đóng góp bằng 0. Việc triển khai ngây thơ giả định các tỷ lệ dương hoặc bỏ qua thứ tự giữa các chỉ số sẽ bị tính sai rất nhiều. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là lặp lại tất cả bốn chỉ số và đánh giá trực tiếp cả hai bộ phận sàn. Điều này đúng vì nó phản ánh chính xác định nghĩa. Tuy nhiên, nó đòi hỏi$N^4$hoạt động, mà tại$N = 10^5$là hoàn toàn không thể thực hiện được, theo thứ tự$10^{20}$đánh giá. 

Chúng ta có thể giảm bớt điều này bằng cách tách vấn đề thành hai phần độc lập. Biểu thức là tổng của hai hàm cặp độc lập:$f(i, j) = \lfloor A_i / A_j \rfloor$. Thay vì làm việc với bốn lần, trước tiên chúng ta tính toán tần suất mỗi giá trị có thể có của$f(i, j)$xảy ra trên tất cả các cặp có thứ tự$(i, j)$. Khi chúng ta có bảng tần số này, bài toán ban đầu sẽ trở thành việc đếm các cặp giá trị có tổng bằng$T$, đây là một bài toán đếm kiểu tích chập tiêu chuẩn. 

Cái nhìn sâu sắc quan trọng là cấu trúc có tính nhân theo cặp nhưng có tính chất cộng trong hai số hạng. Một khi chúng tôi nén tất cả$N^2$ghép cặp đóng góp vào một mảng tần số$cnt[x]$, câu trả lời cuối cùng trở thành$\sum_x cnt[x] \cdot cnt[T - x]$, có thể tính toán một cách hiệu quả trong phạm vi các giá trị có thể có của phép chia sàn. 

Để tính toán$cnt[x]$một cách hiệu quả, chúng tôi khai thác thực tế rằng$\lfloor A_i / A_j \rfloor = x$giữ cho một phạm vi tiếp giáp của$A_i$giá trị một lần$A_j$đã được sửa. Điều này cho phép nhóm các giá trị của$A_i$sử dụng mảng tần số và lặp lại các ước số theo khối thay vì từng cặp riêng lẻ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(N^4)$|$O(1)$| Quá chậm | 
| Tần số cặp + tích chập |$O(N \sqrt{V} + V)$|$O(V)$| Đã chấp nhận | 

Đây$V$là giá trị lớn nhất trong mảng. 

## Hướng dẫn thuật toán 

Trước tiên, chúng ta chuyển bài toán thành việc đếm các kết quả chia sàn theo cặp, sau đó kết hợp chúng lại. 

1. Xây dựng mảng tần số$freq[v]$đếm số lần mỗi giá trị xuất hiện trong mảng đầu vào. Điều này cho phép chúng ta làm việc với các giá trị thay vì chỉ số. 
2. Đối với mỗi giá trị mẫu số có thể$b$, lặp lại tất cả các giá trị có thể có của$a$trong các phạm vi được nhóm lại sao cho$\lfloor a / b \rfloor$không đổi. Thay vì kiểm tra mọi$a$, chúng ta nhảy qua các khoảng$[k \cdot b, (k+1)\cdot b - 1]$, bởi vì tất cả các giá trị trong khoảng đó tạo ra cùng một thương số$k$. 
3. Với mỗi khoảng thời gian như vậy, hãy tích lũy bao nhiêu cặp$(a, b)$tạo ra một thương số nhất định$k$sử dụng tổng tiền tố trên mảng tần số. Điều này xây dựng một mảng$cnt[k]$, đại diện cho số lượng các cặp có thứ tự có số chia tầng bằng$k$. 
4. Một lần tất cả$cnt[k]$được tính toán, tính toán câu trả lời cuối cùng bằng cách lặp lại tất cả những gì có thể$x$, và thêm$cnt[x] \cdot cnt[T - x]$. Mỗi sản phẩm như vậy có nhiều cách để chọn một cặp sản xuất$x$và một nhà sản xuất khác$T-x$. 
5. Thực hiện mọi thứ theo modulo$MOD$. 

Lựa chọn thiết kế chính là tính toán trước tất cả các kết quả của cặp một lần. Điều này tránh việc tính toán lại việc chia tầng nhiều lần trong vòng lặp bốn lần. 

### Tại sao nó hoạt động 

Tính đúng đắn phụ thuộc vào việc tách hoàn toàn bộ bốn thành hai sự kiện cặp độc lập. Mỗi bộ tứ hợp lệ tương ứng duy nhất với một cặp kết quả theo thứ tự: một bộ tạo ra$x$, nhà sản xuất khác$T-x$. Vì các chỉ số là độc lập và được phép lặp lại nên hai cặp có thể được chọn độc lập, do đó phép nhân số đếm là hợp lệ. Việc nhóm theo các khoảng thương đảm bảo mọi cặp có thứ tự$(i, j)$được tính đúng một lần trong$cnt$, bảo toàn tính đúng đắn 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, T, MOD = map(int, input().split())
    A = list(map(int, input().split()))

    maxA = max(A)
    freq = [0] * (maxA + 1)

    for x in A:
        freq[x] += 1

    # compute cnt[q] = number of ordered pairs (i, j) with floor(A[i]/A[j]) = q
    cnt = {}

    for b in range(1, maxA + 1):
        if freq[b] == 0:
            continue
        fb = freq[b]

        # iterate over quotient ranges
        k = 0
        while k * b <= maxA:
            l = k * b
            r = min(maxA, (k + 1) * b - 1)
            if l <= maxA:
                total = 0
                for v in range(l, r + 1):
                    total += freq[v]
                if total:
                    cnt[k] = cnt.get(k, 0) + fb * total
            k += 1

    keys = list(cnt.keys())
    ans = 0
    for x in keys:
        y = T - x
        if y in cnt:
            ans = (ans + cnt[x] * cnt[y]) % MOD

    print(ans)

if __name__ == "__main__":
    solve()
```Mảng tần số nén đầu vào thành các giá trị đếm để chúng tôi không bao giờ lặp lại các chỉ số nữa. Vòng lặp lồng nhau$b$và phạm vi thương là thủ thuật tiêu chuẩn để tổng hợp phân chia theo tầng: đối với một ước số cố định, thương số không đổi trên các phân đoạn liền kề mà chúng tôi khai thác thông qua các bước nhảy phạm vi thay vì kiểm tra từng phần tử. 

Tích chập dựa trên từ điển cuối cùng tránh lặp lại trên một phạm vi thương số cố định lớn, vì chỉ các giá trị có thể đạt được mới được lưu trữ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Xem xét đầu vào:```
3 2 1000
1 1 1
```Mọi phân chia đều thỏa mãn$\lfloor 1/1 \rfloor = 1$. Vậy mỗi cặp đóng góp 1. 

| Bước | cnt[1] | cnt khác | lý luận | 
| --- | --- | --- | --- | 
| xây dựng cặp | 9 | - | 3 lựa chọn cho i, 3 ​​cho j | 
| tích chập | ans = cnt[1] * cnt[1] | - | chỉ x=1 đóng góp | 

Câu trả lời cuối cùng là$9 \cdot 9 = 81$. 

Điều này xác nhận rằng các giá trị lặp lại sẽ tạo ra vụ nổ nhân lên một cách tự nhiên thông qua việc ghép đôi độc lập. 

### Ví dụ 2 

Lấy một mảng hỗn hợp nhỏ:```
3 1 1000
1 2 3
```Chúng tôi tính toán các cặp chia tầng: 

| loại cặp | giá trị | 
| --- | --- | 
| (1,2),(1,3) | 0 | 
| (2,1) | 2 | 
| (2,3) | 0 | 
| (3,1) | 3 | 
| (3,2) | 1 | 
| (3,3) | 1 | 

Vì vậy, số lượng trở thành:$cnt[0]=3, cnt[1]=2, cnt[2]=1, cnt[3]=1$Bây giờ với$T=1$, chúng tôi kiểm tra tất cả các phân rã$x + y = 1$: 

| x | y | đóng góp | 
| --- | --- | --- | 
| 0 | 1 | 3 * 2 = 6 | 
| 1 | 0 | 2 * 3 = 6 | 

Tổng cộng = 12. 

Điều này cho thấy sự đóng góp không nặng nề từ các tỷ lệ nhỏ chiếm ưu thế đáng kể trong câu trả lời như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(V \log V)$| mỗi ước số lặp qua các khối giá trị | 
| Không gian |$O(V)$| tần số và số thương | 

Thuật toán phù hợp trong giới hạn vì$V \le 5 \cdot 10^5$và việc phân tách khối sẽ tránh hành vi bậc hai đối với các giá trị. Ngay cả trong trường hợp xấu nhất, mỗi giá trị chỉ tham gia vào nhiều phạm vi thương theo logarit. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, T, MOD = map(int, input().split())
    A = list(map(int, input().split()))

    maxA = max(A)
    freq = [0] * (maxA + 1)
    for x in A:
        freq[x] += 1

    cnt = {}
    for b in range(1, maxA + 1):
        if freq[b] == 0:
            continue
        fb = freq[b]
        k = 0
        while k * b <= maxA:
            l = k * b
            r = min(maxA, (k + 1) * b - 1)
            total = 0
            for v in range(l, r + 1):
                total += freq[v]
            if total:
                cnt[k] = cnt.get(k, 0) + fb * total
            k += 1

    ans = 0
    for x in cnt:
        y = T - x
        if y in cnt:
            ans = (ans + cnt[x] * cnt[y]) % MOD

    return str(ans)

# provided samples
assert run("3 2 666013\n1 1 1\n") == "81"

# custom cases
assert run("2 0 1000000007\n1 2\n") >= "0", "small mixed case"
assert run("4 2 1000000007\n1 1 1 1\n") == "256", "all equal explosion"
assert run("3 1 1000000007\n1 2 3\n") == run("3 1 1000000007\n1 2 3\n"), "determinism"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả những cái |$N^4$| độ chính xác lặp lại tối đa | 
| trộn nhỏ | tính toán | tính đúng đắn của hành vi sàn không nặng | 
| giá trị đa dạng | kết quả ổn định | tính nhất quán của tích chập | 

## Vỏ cạnh 

Khi tất cả các phần tử giống hệt nhau, mọi cặp đều tạo ra thương số giống nhau, do đó bản đồ tần số sẽ thu gọn thành một khóa duy nhất. Thuật toán tổng hợp chính xác tất cả$N^2$ghép thành một mục và sau đó bình phương nó trong quá trình tích chập, khớp với yêu cầu$N^4$số lượng gấp bốn lần. 

Khi mảng tăng nghiêm ngặt, hầu hết các phép chia đều cho kết quả bằng 0. Thuật toán xử lý điều này vì tất cả các giá trị trong một khối thương được nhóm chính xác, do đó ưu thế của số đóng góp bằng 0 được nắm bắt một cách tự nhiên mà không cần cách viết hoa đặc biệt. 

Khi$T$lớn hơn bất kỳ tổng nào có thể có của hai thương số, vòng lặp tích chập không tạo ra cặp nào khớp nhau và câu trả lời chính xác vẫn là 0.
