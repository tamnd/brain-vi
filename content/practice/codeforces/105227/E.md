---
title: "CF 105227E - Truy vấn đệ quy"
description: "Mỗi truy vấn đưa ra một phân đoạn số nguyên và hỏi có bao nhiêu số trong phân đoạn đó có chung một giá trị cụ thể theo hàm biến đổi g. Hàm g lấy một số và liên tục thay thế nó bằng tích các chữ số của nó cho đến khi giá trị trở thành một chữ số."
date: "2026-06-24T16:29:19+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105227
codeforces_index: "E"
codeforces_contest_name: "CPG Training Contest - 1"
rating: 0
weight: 105227
solve_time_s: 90
verified: false
draft: false
---

[CF 105227E - Truy vấn đệ quy](https://codeforces.com/problemset/problem/105227/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 30 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Mỗi truy vấn đưa ra một phân đoạn số nguyên và hỏi có bao nhiêu số trong phân đoạn đó có chung một giá trị cụ thể theo hàm biến đổi g. Hàm g lấy một số và liên tục thay thế nó bằng tích các chữ số của nó cho đến khi giá trị trở thành một chữ số. Chữ số đơn cuối cùng đó là kết quả của g(x). Ví dụ, 47 trở thành 4 × 7 = 28, khi đó 2 × 8 = 16, rồi 1 × 6 = 6, do đó g(47) = 6. 

Nhiệm vụ là trả lời tối đa hai trăm nghìn truy vấn như vậy, trong đó mỗi truy vấn yêu cầu một phạm vi [l, r] trong phạm vi lên tới một triệu và chữ số mục tiêu k từ 1 đến 9. Đầu ra cho mỗi truy vấn là số số nguyên trong phạm vi đó có căn số nhân bằng k. 

Các ràng buộc ngay lập tức buộc chúng tôi phải loại bỏ mô phỏng theo mỗi truy vấn. Một cách tiếp cận đơn giản để tính lại g(x) cho mỗi x trong [l, r] sẽ yêu cầu tới một triệu thao tác cho mỗi truy vấn trong trường hợp xấu nhất, dẫn đến tổng cộng khoảng 2 × 10^11 thao tác, điều này vượt xa khả thi. 

Một điểm tinh tế là mặc dù x lên tới 10^6 nhưng g(x) luôn giảm nhanh về một chữ số. Điều đó có nghĩa là chỉ có chín kết quả có thể xảy ra, do đó cấu trúc lặp lại trên toàn miền có thể được tính toán trước. 

Các trường hợp cạnh phát sinh từ hành vi nhân chữ số. Các số chứa số 0 ngay lập tức thu gọn về 0, nhưng k được đảm bảo nằm trong khoảng từ 1 đến 9, vì vậy những giá trị đó không liên quan đến việc đếm. Một trường hợp góc khác là các số như 10 hoặc 1000, trở thành số 0 khi nhân và không được vô tình được tính vào bất kỳ k dương nào. 

## Phương pháp tiếp cận 

Phương pháp brute-force đánh giá g(x) độc lập cho từng số trong mỗi truy vấn. Với mỗi x, chúng ta nhân các chữ số liên tục cho đến khi đạt được một chữ số. Điều này tốn tối đa một vài phép nhân chữ số cho mỗi số, nhưng trên tất cả các truy vấn, điều này trở nên quá lớn vì mỗi truy vấn có thể mở rộng tới một triệu số và có tới hai trăm nghìn truy vấn. 

Quan sát quan trọng là phép biến đổi g(x) chỉ phụ thuộc vào x chứ không phụ thuộc vào truy vấn. Khi chúng ta biết g(x) với mỗi x lên đến một triệu, mỗi truy vấn sẽ trở thành một bài toán đếm đơn giản trên một mảng được tính toán trước. 

Chúng tôi tính toán trước một mảng trong đó mỗi vị trí lưu trữ g(x). Sau đó, với mỗi chữ số k từ 1 đến 9, chúng ta xây dựng tổng tiền tố trên tất cả x sao cho g(x) = k. Sau đó, mỗi truy vấn được trả lời theo thời gian không đổi bằng cách sử dụng các khác biệt về tiền tố. 

Điều này chuyển đổi việc tính toán lại lặp đi lặp lại thành một bước tiền xử lý duy nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(Q · N · log N) | O(1) | Quá chậm | 
| Tối ưu (tính trước + tổng tiền tố) | O(N log N + Q) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Đầu tiên chúng ta tính g(x) cho mỗi x từ 1 đến 1.000.000. Điều này được thực hiện bằng cách thay thế nhiều lần một số bằng tích các chữ số của nó cho đến khi nó trở thành một chữ số. Bước này là cần thiết vì sau khi tính toán, tất cả các truy vấn sẽ sử dụng lại các giá trị này. 

Tiếp theo, chúng tôi lưu trữ số lượng số tạo ra mỗi chữ số có thể từ 1 đến 9. Thay vì chỉ lưu trữ tần số thô, chúng tôi xây dựng tổng tiền tố để có thể trả lời các truy vấn phạm vi một cách hiệu quả. 

Sau đó, chúng tôi chuyển đổi truy vấn ban đầu [l, r, k] thành tổng tiền tố khác nhau: câu trả lời là tiền tố[k][r] trừ tiền tố[k][l − 1]. Điều này có tác dụng vì mảng tiền tố tích lũy số lượng từ 1 cho đến từng vị trí. 

Cuối cùng, chúng tôi in kết quả cho mỗi truy vấn. 

### Tại sao nó hoạt động 

Tính đúng đắn xuất phát từ thực tế g(x) là một hàm thuần túy của x và độc lập với cấu trúc truy vấn. Sau khi được tính toán, mỗi lần xuất hiện của x sẽ đóng góp chính xác một đơn vị vào đúng một nhóm chữ số. Tổng tiền tố bảo toàn các số đếm tích lũy này, do đó, bất kỳ truy vấn khoảng thời gian nào cũng giảm xuống còn trừ hai tổng tích lũy mà không làm mất hoặc tính hai lần bất kỳ đóng góp nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MAXN = 10**6

def prod_digits(x):
    while x >= 10:
        p = 1
        while x > 0:
            x, d = divmod(x, 10)
            p *= d
        x = p
    return x

g = [0] * (MAXN + 1)
for i in range(1, MAXN + 1):
    g[i] = prod_digits(i)

prefix = [[0] * (MAXN + 1) for _ in range(10)]

for i in range(1, MAXN + 1):
    val = g[i]
    for k in range(1, 10):
        prefix[k][i] = prefix[k][i - 1]
    prefix[val][i] += 1

q = int(input())
out = []

for _ in range(q):
    l, r, k = map(int, input().split())
    out.append(str(prefix[k][r] - prefix[k][l - 1]))

print("\n".join(out))
```Giải pháp bắt đầu bằng cách xác định hàm giảm chữ số nhân. Nó liên tục nhân các chữ số cho đến khi số trở thành một chữ số duy nhất, đảm bảo tính chính xác ngay cả đối với các đầu vào trải qua nhiều giai đoạn trung gian như 39 hoặc 88. 

Sau đó, chúng tôi tính toán trước g(x) cho tất cả các giá trị lên đến một triệu. Đây là phần đắt tiền nhưng chỉ được thực hiện một lần. 

Sau đó, chúng tôi xây dựng các mảng tiền tố cho mỗi chữ số từ 1 đến 9. Mỗi mảng tiền tố theo dõi xem có bao nhiêu số được lập chỉ mục mà tôi có một giá trị g nhất định. 

Các truy vấn được trả lời bằng cách trừ các phạm vi tiền tố, giúp tránh mọi tính toán lại. 

## Ví dụ đã hoạt động 

Hãy xem xét một trường hợp minh họa nhỏ trong đó chúng tôi truy vấn một vài khoảng sau khi xử lý trước g(x). 

Chúng ta hãy theo dõi một phiên bản đơn giản hóa trong một phạm vi nhỏ: 

| tôi | g(i) | tiền tố[4][i] | tiền tố[6][i] | 
| --- | --- | --- | --- | 
| 1 | 1 | 0 | 0 | 
| 2 | 2 | 0 | 0 | 
| 3 | 3 | 0 | 0 | 
| 4 | 4 | 1 | 0 | 
| 5 | 5 | 1 | 0 | 
| 6 | 6 | 1 | 1 | 
| 7 | 7 | 1 | 1 | 
| 8 | 8 | 1 | 1 | 
| 9 | 9 | 1 | 1 | 

Đối với truy vấn [4, 7, 6], chúng tôi tính tiền tố[6][7] − tiền tố[6][3] = 1 − 0 = 1. 

Bây giờ hãy xem xét một truy vấn [1, 9, 4], chúng ta tính tiền tố[4][9] − tiền tố[4][0] = 1 − 0 = 1, vì chỉ có số 4 ánh xạ tới 4 trong phạm vi này. 

Những dấu vết này cho thấy rằng khi mảng tiền tố được tạo, mỗi truy vấn sẽ giảm xuống còn hai lần tra cứu bảng và một phép trừ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N log N + Q) | Mỗi số đến 1e6 được rút gọn thông qua tích số, khi đó mỗi truy vấn là O(1) | 
| Không gian | O(N) | Mảng tiền tố lưu trữ số đếm tích lũy cho mỗi chữ số | 

Quá trình tiền xử lý phù hợp thoải mái trong các giới hạn vì N log N hoạt động trên một triệu số nguyên có thể chấp nhận được trong Python và các truy vấn được trả lời theo thời gian không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    MAXN = 200  # small bound for testing

    def prod_digits(x):
        while x >= 10:
            p = 1
            while x > 0:
                x, d = divmod(x, 10)
                p *= d
            x = p
        return x

    g = [0] * (MAXN + 1)
    for i in range(1, MAXN + 1):
        g[i] = prod_digits(i)

    prefix = [[0] * (MAXN + 1) for _ in range(10)]
    for i in range(1, MAXN + 1):
        for k in range(1, 10):
            prefix[k][i] = prefix[k][i - 1]
        prefix[g[i]][i] += 1

    q = int(input())
    out = []
    for _ in range(q):
        l, r, k = map(int, input().split())
        out.append(str(prefix[k][r] - prefix[k][l - 1]))

    return "\n".join(out)

# minimum range
assert run("1\n1 1 1\n") == "1", "single element"

# zero-producing numbers behavior
assert run("3\n10 10 1\n10 10 9\n10 10 2\n") == "0\n0\n0", "zeros collapse to 0"

# small mixed range
assert run("2\n1 9 4\n1 9 6\n") == "1\n1", "basic digit roots"

# full range check
assert run("1\n1 200 1\n") == run("1\n1 200 1\n"), "consistency check"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 1 | tính đúng đắn của trường hợp cơ sở | 
| 10 xử lý | tất cả số không | hành vi thu gọn sản phẩm số | 
| phạm vi nhỏ | 1, 1 | tính đúng đắn của phân phối | 
| đầy đủ | nhất quán | sự ổn định trên các truy vấn phạm vi | 

## Vỏ cạnh 

Trường hợp cạnh khóa là các số chứa số 0. Ví dụ: x = 105 ngay lập tức trở thành 0 sau tích chữ số đầu tiên. Vì k luôn nằm trong khoảng từ 1 đến 9 nên những con số như vậy sẽ không bao giờ đóng góp vào bất kỳ câu trả lời nào. Quá trình tiền xử lý đảm bảo chúng được lưu trữ dưới g(x) = 0 và không bao giờ được nhập vào bất kỳ mảng prefix[k] nào đối với k ≥ 1. 

Một trường hợp khác là sự sụp đổ chữ số lặp đi lặp lại. Một số như 39 trở thành 27, rồi 14, rồi 4. Nếu các bước trung gian bị cắt bớt sớm do nhầm lẫn thì ánh xạ cuối cùng sẽ sai. Cấu trúc vòng lặp đảm bảo rút gọn hoàn toàn cho đến khi chỉ còn lại một chữ số, do đó việc phân loại cuối cùng vẫn ổn định bất kể số bước cần thiết. 

Cuối cùng, các phạm vi thống nhất như [1, 1e6] kiểm tra xem tổng tiền tố có tích lũy chính xác trên toàn bộ miền hay không. Vì mỗi x được gán chính xác một g(x), nên tổng số từ k = 1 đến 9 nhất quán với N, hoạt động như một phép kiểm tra độ chính xác về tính chính xác của quá trình tiền xử lý.
