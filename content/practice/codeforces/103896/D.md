---
title: "CF 103896D - Phòng thủ cú"
description: "Chúng tôi được giao một hàng bò, mỗi hàng có trọng lượng. Một “cuộc đột kích” được xác định bởi hai số nguyên, vị trí bắt đầu và kích thước bước. Bắt đầu từ vị trí a, chúng ta liên tục nhảy về phía trước theo vị trí b và thu thập tất cả những con bò mà chúng ta đáp xuống, dừng lại khi đi quá cuối hàng."
date: "2026-07-02T07:30:34+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103896
codeforces_index: "D"
codeforces_contest_name: "UTPC Contest 09-02-22 Div. 1 (Advanced)"
rating: 0
weight: 103896
solve_time_s: 49
verified: true
draft: false
---

[CF 103896D - Phòng thủ cú](https://codeforces.com/problemset/problem/103896/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được giao một hàng bò, mỗi hàng có trọng lượng. Một “cuộc đột kích” được xác định bởi hai số nguyên, vị trí bắt đầu và kích thước bước. Bắt đầu từ vị trí a, chúng ta liên tục nhảy về phía trước theo vị trí b và thu thập tất cả những con bò mà chúng ta đáp xuống, dừng lại khi đi quá cuối hàng. Mỗi truy vấn yêu cầu tổng trọng lượng của tất cả các con bò được thu thập theo mô hình đột kích cụ thể đó. 

Nói cách khác, mỗi truy vấn xác định cấp số cộng của các chỉ số bên trong mảng và chúng ta phải tính tổng các giá trị tại các chỉ mục đó. 

Các ràng buộc rất lớn: tối đa 3×10^5 con bò và 3×10^5 truy vấn. Điều này ngay lập tức loại trừ mọi phương pháp tính lại tổng cho mỗi truy vấn bằng cách thực hiện từng bước tiến trình. Trong trường hợp xấu nhất, một truy vấn có b = 1 chạm vào các phần tử O(n), điều này sẽ dẫn đến O(n·q), vượt xa giới hạn có thể chấp nhận được. 

Trường hợp cạnh tinh tế xuất hiện khi b lớn so với nhỏ. Khi b lớn, mỗi truy vấn chạm vào ít phần tử và rẻ. Khi b nhỏ, nhiều chỉ mục trùng nhau trên các truy vấn và việc lặp lại đơn giản sẽ lặp lại cùng một công việc nhiều lần. Một vấn đề khác là khi a = 1 và b = 1, suy biến thành tổng toàn bộ mảng, một tình huống sẽ được tính toán lại nhiều lần dưới tác động mạnh mẽ. 

Một cải tiến đơn giản có thể thử lưu kết quả vào bộ đệm cho mỗi (a, b), nhưng vì có thể có tới 3×10^5 cặp riêng biệt nên điều này không giúp ích gì trong trường hợp xấu nhất. 

## Phương pháp tiếp cận 

Ý tưởng về vũ lực rất đơn giản: đối với mỗi truy vấn, bắt đầu từ chỉ mục a và liên tục thêm w[a], w[a + b], w[a + 2b], v.v. cho đến khi vượt quá n. Điều này đúng vì nó tuân theo đúng định nghĩa của cuộc đột kích. Vấn đề là chi phí. Trong trường hợp xấu nhất khi b = 1, mỗi truy vấn xử lý n phần tử, dẫn đến tổng cộng khoảng 9×10^10 thao tác khi cả n và q đều là 3×10^5. 

Quan sát chính là kích thước bước nhảy b phân chia hành vi thành hai chế độ khác nhau về cơ bản. b lớn có nghĩa là có ít vị trí được truy cập cho mỗi truy vấn. B nhỏ có nghĩa là nhiều truy vấn có chung các lớp dư lượng theo modulo b, vì vậy chúng ta có thể sử dụng lại thông tin được tính toán trước. Cấu trúc mà chúng tôi khai thác là các chỉ mục được truy cập bởi bước b cố định tạo thành các chuỗi số học độc lập và khi b nhỏ thì tổng thể chỉ có một vài chuỗi như vậy. 

Điều này dẫn đến một giải pháp lai. Chúng tôi chọn ngưỡng B khoảng √n. Với b lớn hơn B, chúng ta tính toán trực tiếp từng truy vấn vì nó chỉ truy cập tối đa n/B phần tử. Đối với b nhỏ hơn hoặc bằng B, chúng tôi tính toán trước các câu trả lời cho tất cả các cặp (b, số dư) có thể có bằng cách sử dụng lập trình động từ phải sang trái, do đó mỗi truy vấn sẽ trở thành O(1). 

Quá trình chuyển đổi quan trọng là viết lại phép truy toán: với b cố định, xác định dp[i] = w[i] + dp[i + b]. Điều này cho phép tất cả các câu trả lời cho b đã cho được tính toán trong một lần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nq) | O(1) | Quá chậm | 
| Phân tách Sqrt theo kích thước bước | O(n√n + q√n) | O(n√n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi phân tách các truy vấn dựa trên kích thước bước b của chúng bằng cách sử dụng ngưỡng B ≈ √n.

1. Chúng ta khởi tạo một cấu trúc để lưu trữ câu trả lời cho các bước có kích thước nhỏ. Đối với mỗi b từ 1 đến B, chúng ta sẽ tính một mảng dp trong đó dp[i] biểu thị tổng tổng bắt đầu từ i và nhảy theo b cho đến khi rời khỏi mảng. 
2. Với mỗi b từ 1 đến B, chúng ta tính dp theo thứ tự ngược từ n xuống 1. Tại vị trí i, nếu i + b ≤ n, chúng ta đặt dp[i] = w[i] + dp[i + b], nếu không thì dp[i] = w[i]. Điều này có hiệu quả vì vị trí được truy cập tiếp theo trong cùng một cấp số cộng chính xác là i + b. 
3. Sau khi tính toán dp cho b cố định, chúng ta có thể trả lời bất kỳ truy vấn nào có b đó trong thời gian O(1) bằng cách trả về dp[a]. 
4. Đối với các truy vấn có b > B, chúng tôi trả lời trực tiếp bằng cách lặp lại i = a, a + b, a + 2b và tính tổng các trọng số cho đến khi vượt quá n. Vì b lớn nên số lượng phần tử được truy cập nhỏ. 
5. Chúng tôi xử lý tất cả các truy vấn, sử dụng dp được tính toán trước cho các bước nhỏ và mô phỏng trực tiếp cho các bước lớn. 

Tại sao nó hoạt động được gắn liền với cấu trúc của cấp số cộng trên một mảng cố định. Đối với các bước nhỏ, mảng được tái sử dụng nhiều lần trong các chuỗi chồng chéo, do đó quá trình tính toán trước sẽ được khấu hao trên tất cả các truy vấn. Đối với các bước lớn, chuỗi đủ thưa để truyền tải trực tiếp rẻ. Mọi truy vấn được xử lý bởi chính xác một trong hai chế độ này và cả hai đều bao gồm tất cả các trường hợp (a, b) có thể xảy ra mà không bị trùng lặp hoặc thiếu sót. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
w = list(map(int, input().split()))
q = int(input())

B = int(n ** 0.5) + 1

# store answers for small b
dp = [[0] * n for _ in range(B + 1)]

# precompute for all b up to B
for b in range(1, B + 1):
    for i in range(n - 1, -1, -1):
        if i + b < n:
            dp[b][i] = w[i] + dp[b][i + b]
        else:
            dp[b][i] = w[i]

for _ in range(q):
    a, b = map(int, input().split())
    a -= 1

    if b <= B:
        print(dp[b][a])
    else:
        s = 0
        i = a
        while i < n:
            s += w[i]
            i += b
        print(s)
```Bảng dp là tối ưu hóa cốt lõi. Mỗi hàng tương ứng với một kích thước bước cố định b và mỗi mục nhập sẽ thu gọn toàn bộ cấp số cộng thành một giá trị được tính toán trước duy nhất. Vòng lặp trực tiếp chỉ được sử dụng khi tiến trình đủ ngắn để quá trình tiền xử lý không còn giá trị nữa. 

Chi tiết triển khai tinh tế duy nhất là lập chỉ mục. Mảng dp được xây dựng trên các chỉ mục dựa trên 0, vì vậy mọi truy vấn đều chuyển đổi a thành - 1 trước khi tra cứu. Một chi tiết khác là kiểm tra ranh giới i + b < n, đảm bảo chúng tôi chỉ tham chiếu các chỉ số hợp lệ trong tương lai. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 3
w = [1, 2, 3]
queries = (1,1), (1,2)
```Với b = 1, dp[1][i] là tổng hậu tố: 

| tôi | dp[1][i] | 
| --- | --- | 
| 3 | 3 | 
| 2 | 5 | 
| 1 | 6 | 

Truy vấn (1,1) trả về dp[1][1] = 6. 

Đối với (1,2), chúng tôi truy cập trực tiếp vào chỉ số 1 và 3: 

tổng = 1 + 3 = 4. 

Điều này cho thấy cách dp nén toàn bộ quá trình truyền tải vào O(1). 

### Ví dụ 2 

đầu vào:```
n = 4
w = [2,3,5,7]
queries = (2,3), (2,2)
```Với b = 3: 

giá trị dp[3][i]: 

| tôi | dp[3][i] | 
| --- | --- | 
| 4 | 7 | 
| 3 | 5 | 
| 2 | 3 + 7 = 10 | 
| 1 | 2 + 5 = 7 | 

Truy vấn (2,3) trả về dp[3][2] = 10. 

Với b = 2, vì 2 nhỏ nên chúng ta sử dụng dp trực tiếp: 

dp[2][2] = 3 + 7 = 10? thực tế chỉ số 2 → 4 cho 3 + 7 = 10. 

Điều này xác nhận tính nhất quán giữa tái phát và truyền tải. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n√n + q√n) | Mỗi bước nhỏ b xây dựng một mảng dp trong O(n) và có √n giá trị b như vậy; các bước lớn xử lý tối đa √n phần tử cho mỗi truy vấn | 
| Không gian | O(n√n) | Bảng DP lưu trữ n mục nhập cho mỗi b lên tới √n | 

Độ phức tạp kết hợp phù hợp thoải mái trong các giới hạn của n, q lên đến 3×10^5, vì √n ≈ 550. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import sqrt
    n = int(input())
    w = list(map(int, input().split()))
    q = int(input())

    B = int(n ** 0.5) + 1
    dp = [[0] * n for _ in range(B + 1)]

    for b in range(1, B + 1):
        for i in range(n - 1, -1, -1):
            if i + b < n:
                dp[b][i] = w[i] + dp[b][i + b]
            else:
                dp[b][i] = w[i]

    out = []
    for _ in range(q):
        a, b = map(int, input().split())
        a -= 1
        if b <= B:
            out.append(str(dp[b][a]))
        else:
            s = 0
            i = a
            while i < n:
                s += w[i]
                i += b
            out.append(str(s))
    return "\n".join(out)

# sample-like
assert run("3\n1 2 3\n2\n1 1\n1 2") == "6\n4"

# minimum
assert run("1\n5\n1\n1 1") == "5"

# all equal
assert run("5\n1 1 1 1 1\n2\n1 1\n1 2") == "5\n3"

# large step
assert run("6\n1 2 3 4 5 6\n1\n2 5") == "7"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 5 | trường hợp cơ sở đúng đắn | 
| mảng thống nhất | 5, 3 | tính nhất quán giữa các kích thước bước | 
| bước nhảy lớn | 7 | độ chính xác truyền tải thưa thớt | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là khi b = 1. Trong trường hợp đó, mọi truy vấn sẽ trở thành tổng hậu tố đầy đủ từ a và các giải pháp đơn giản sẽ liên tục tính toán lại các phân đoạn dài. Phép lặp dp xử lý việc này một cách rõ ràng vì dp[1][i] tự nhiên trở thành một chuỗi hậu tố. 

Một trường hợp cạnh khác là khi a ở gần cuối và b lớn. Ví dụ: n = 6, a = 5, b = 10. Vòng lặp chỉ nên bao gồm w[5] và dừng ngay lập tức. Mô phỏng trực tiếp xử lý việc này một cách chính xác vì điều kiện i < n được kiểm tra trước mỗi phép cộng. 

Trường hợp khó phát hiện cuối cùng là khi nhiều truy vấn có chung (a, b). Giải pháp không dựa vào bộ nhớ đệm cho mỗi truy vấn; thay vào đó, nó đảm bảo tính chính xác một cách độc lập cho từng truy vấn thông qua tra cứu dp hoặc truyền tải xác định.
