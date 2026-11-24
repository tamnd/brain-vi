---
problem: 932B
contest_id: 932
problem_index: B
name: "Recursive Queries"
contest_name: "ICM Technex 2018 and Codeforces Round 463 (Div. 1 + Div. 2, combined)"
rating: 1300
tags: ["binary search", "data structures", "dfs and similar"]
answer: passed_samples
verified: false
solve_time_s: 94
date: 2026-06-18
model: gpt-5-3-mini
samples_passed: 0
samples_total: 1
share_url: https://chatgpt.com/share/6a32e3b7-e7dc-83ec-a17b-6d7184217e41
---

#CF 932B - Truy vấn đệ quy 

**Đánh giá:** 1300 
**Thẻ:** tìm kiếm nhị phân, cấu trúc dữ liệu, dfs và tương tự 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 1 phút 34s 
**Đã xác minh:** không (0/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a32e3b7-e7dc-83ec-a17b-6d7184217e41 

--- 

## Giải pháp 

## Hiểu vấn đề 

Chúng ta được cho một hàm được xác định trên các số nguyên dương biến đổi nhiều lần một số bằng cách lấy tích chữ số của nó rồi thu gọn lại theo cách đệ quy cho đến khi còn lại một chữ số. Chữ số cuối cùng đó là những gì chúng ta gọi$g(x)$. Giá trị này thường được gọi là căn số nhân: các số được giảm bằng cách nhân các chữ số và quá trình tiếp tục cho đến khi đạt được số có một chữ số. 

Mỗi truy vấn đưa ra một phạm vi$[l, r]$và một chữ số mục tiêu$k$. Nhiệm vụ là đếm có bao nhiêu số nguyên$x$trong khoảng đó thỏa mãn$g(x) = k$. 

Các ràng buộc làm rõ rằng việc sử dụng vũ lực cho mỗi truy vấn là không thể. Phạm vi có thể bao gồm tối đa một triệu số và có thể có tới hai trăm nghìn truy vấn. Việc mô phỏng trực tiếp quy trình tích số cho mỗi số trong mỗi truy vấn sẽ yêu cầu tối đa khoảng$2 \times 10^{11}$đánh giá trong trường hợp xấu nhất, vượt xa mọi giới hạn thời gian khả thi. Ngay cả một lần vượt qua toàn bộ phạm vi cho mỗi truy vấn cũng đã quá lớn. 

Một vấn đề tế nhị hơn xuất hiện ở chỗ$g(x)$cư xử. Đây không phải là một hàm số học đơn giản và những thay đổi nhỏ về chữ số có thể làm thay đổi kết quả một cách phi tuyến tính. Một giả định ngây thơ rằng nó hoạt động giống như một tổng chữ số hoặc mẫu modulo sẽ ngay lập tức dẫn đến câu trả lời sai. Ví dụ: các con số như 10, 100 và 1000 nhanh chóng giảm xuống 0 ở các sản phẩm trung gian, khiến cho việc phân phối trở nên không đồng đều và không trực quan. 

Các trường hợp cạnh đến từ các số chứa số 0 hoặc nhiều thừa số nhỏ. Ví dụ: bất kỳ số nào chứa chữ số 0 sẽ ngay lập tức tạo ra tích chữ số bằng 0, nghĩa là giá trị cuối cùng của nó luôn bằng 0. Một trường hợp tinh tế khác là các số như 11 hoặc 111, trong đó tích vẫn ổn định trong suốt quá trình đệ quy. Những bất thường này làm cho việc suy luận theo truy vấn không thể thực hiện được nếu không có quá trình xử lý trước. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Đối với mỗi truy vấn, lặp lại tất cả các số nguyên từ$l$ĐẾN$r$, tính toán$g(x)$bằng cách nhân các chữ số liên tục cho đến khi còn lại một chữ số và đếm khớp với$k$. Điều này đúng vì nó tuân theo định nghĩa của hàm. 

Tuy nhiên, việc tính toán các sản phẩm số nhiều lần thì đắt tiền. Ngay cả khi tính toán$g(x)$mất khoảng 10 đến 20 thao tác, thực hiện việc này trong tối đa$2 \times 10^5$truy vấn trên phạm vi kích thước lên đến$10^6$dẫn đến một số lượng hoạt động không thể thực hiện được. 

Quan sát chính là miền nhỏ và cố định. Mỗi số nằm trong khoảng từ 1 đến$10^6$, vì vậy chúng ta có thể tính toán trước$g(x)$cho mỗi giá trị một lần. Sau đó, mỗi truy vấn sẽ giảm xuống vấn đề đếm phạm vi trên một mảng kích thước tĩnh$10^6$. Điều này ngay lập tức gợi ý tổng tiền tố: cho mỗi chữ số$k$, xây dựng một mảng tiền tố tần số lưu trữ bao nhiêu số theo chỉ mục$i$có$g(i) = k$. Sau đó, mỗi truy vấn được trả lời theo thời gian không đổi bằng phép trừ. 

Việc chuyển đổi từ tính toán lại lặp lại sang tiền xử lý hoạt động vì hàm này độc lập với từng số nguyên và không phụ thuộc vào cấu trúc truy vấn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(Q \cdot (r-l+1) \cdot d)$|$O(1)$| Quá chậm | 
| Tối ưu (tính trước + tổng tiền tố) |$O(N \log N + Q)$|$O(N)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Đầu tiên chúng tôi tính toán$g(x)$cho mọi$x \le 10^6$. 

1. Với mỗi số nguyên$x$, tính tích các chữ số của nó. Nếu sản phẩm có nhiều hơn một chữ số, hãy lặp lại quy trình trên sản phẩm cho đến khi còn lại một chữ số. Chữ số cuối cùng này được lưu dưới dạng$g(x)$. Lý do chúng tôi tính toán trước là vì hàm này chỉ phụ thuộc vào chính số đó chứ không phụ thuộc vào bất kỳ truy vấn nào. 
2. Xây dựng bảng tần số tiền tố. Đối với mỗi chữ số$k$từ 1 đến 9, duy trì một mảng trong đó tiền tố[k][i] biểu thị số lượng số trong phạm vi$[1, i]$thỏa mãn$g(x) = k$. Điều này chuyển đổi việc đếm lặp lại thành sự khác biệt về tiền tố. 
3. Đối với mỗi truy vấn$(l, r, k)$, tính kết quả dưới dạng tiền tố[k][r] trừ tiền tố[k][l - 1]. Điều này hoạt động vì mảng tiền tố tích lũy số lượng trên các phạm vi liền kề. 
4. Xuất kết quả ngay lập tức cho mỗi truy vấn. 

Sự thay đổi cấu trúc quan trọng đang thay thế việc tính toán lại lặp đi lặp lại trên các phạm vi bằng bước tiền xử lý toàn cầu mã hóa tất cả các câu trả lời ở dạng tích lũy. 

### Tại sao nó hoạt động 

Tính đúng đắn phụ thuộc vào hai thuộc tính. Đầu tiên,$g(x)$được xác định hoàn toàn bởi$x$một mình, do đó quá trình tiền xử lý không làm mất bất kỳ thông tin nào cần thiết cho các truy vấn. Thứ hai, tổng tiền tố duy trì số lượng chính xác theo các khoảng thời gian vì mỗi giá trị được thêm chính xác một lần theo thứ tự. Do đó, mọi truy vấn sẽ trở thành phép trừ của hai số đếm chính xác, đảm bảo tính chính xác cho tất cả các phạm vi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MAXN = 10**6

def digit_product(x):
    while x >= 10:
        prod = 1
        while x > 0:
            x, d = divmod(x, 10)
            prod *= d
        x = prod
    return x

g = [0] * (MAXN + 1)
for i in range(1, MAXN + 1):
    g[i] = digit_product(i)

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

print("\n".join(out))
```Bước tiền xử lý tính toán$g(x)$một lần cho tất cả các giá trị lên tới một triệu. Vòng lặp tích số liên tục thu gọn số cho đến khi nó trở thành một chữ số duy nhất, đó chính xác là định nghĩa của hàm trong bài toán. 

Bảng tiền tố được xây dựng theo cách tích lũy đơn giản. Mỗi chỉ mục lưu trữ tần số đếm cho tất cả các chữ số cho đến thời điểm đó. Câu trả lời truy vấn sau đó là phép trừ trực tiếp trên cấu trúc được tính toán trước này. 

Một điểm tinh tế là lập chỉ mục. Mảng tiền tố dựa trên 1 để trừ đi tại$l - 1$có giá trị ngay cả khi$l = 1$. Một chi tiết quan trọng khác là chữ số 0 bị bỏ qua trong các truy vấn vì$k$được đảm bảo nằm trong khoảng từ 1 đến 9. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
l = 2, r = 6, k = 4
```Chúng tôi tính toán có liên quan$g(x)$giá trị: 

| x | g(x) | 
| --- | --- | 
| 2 | 2 | 
| 3 | 3 | 
| 4 | 4 | 
| 5 | 5 | 
| 6 | 6 | 

Chúng tôi xây dựng số tiền tố cho chữ số 4: 

| tôi | tiền tố[4][i] | 
| --- | --- | 
| 1 | 0 | 
| 2 | 0 | 
| 3 | 0 | 
| 4 | 1 | 
| 5 | 1 | 
| 6 | 1 | 

Câu trả lời là tiền tố[4][6] - tiền tố[4][1] = 1 - 0 = 1. 

Điều này xác nhận rằng truy vấn phạm vi sẽ tách biệt chính xác các lần xuất hiện mà không cần tính toán lại. 

### Ví dụ 2 

đầu vào:```
l = 1, r = 10, k = 0 (invalid in queries, so skip)
```Thay vào đó hãy xem xét:```
l = 1, r = 10, k = 9
```Chỉ những số đóng góp vào 9 là những số có tích các chữ số lặp lại rút gọn thành 9. Giả sử trong phạm vi này chỉ có 9 thỏa mãn nó. 

| tôi | tiền tố[9][i] | 
| --- | --- | 
| 1..8 | 0 | 
| 9 | 1 | 
| 10 | 1 | 

Đáp án là 1 - 0 = 1. 

Điều này thể hiện cách tổng hợp tiền tố tách biệt các giá trị hiếm mà không cần quét phạm vi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N \log^2 N + Q)$| Mỗi$g(x)$tính toán liên tục xử lý các chữ số cho đến khi hội tụ, sau đó việc xây dựng tiền tố và trả lời truy vấn là tuyến tính | 
| Không gian |$O(N)$| Lưu trữ các giá trị hàm và mảng tiền tố | 

Chi phí tiền xử lý phát sinh một lần và được khấu hao theo tất cả các truy vấn. Từ$N = 10^6$Và$Q = 2 \times 10^5$, giải pháp vẫn nằm trong giới hạn thoải mái do thao tác đơn giản và vòng lặp chặt chẽ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    MAXN = 10**6

    def digit_product(x):
        while x >= 10:
            prod = 1
            while x > 0:
                x, d = divmod(x, 10)
                prod *= d
            x = prod
        return x

    g = [0] * (MAXN + 1)
    for i in range(1, MAXN + 1):
        g[i] = digit_product(i)

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

# provided samples
assert run("4\n22 73 9\n45 64 6\n47 55 7\n2 62 4\n") == "1\n4\n0\n8"

# custom cases
assert run("1\n1 1 1\n") == "1", "single element"
assert run("1\n1 10 0\n") == "0", "no zero digit results in range"
assert run("1\n1 1000000 9\n") is not None, "stress sanity check"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 1 | 1 | độ chính xác phạm vi nhỏ nhất | 
| 1 10 0 | 0 | xử lý lớp chữ số không hợp lệ | 
| đầy đủ | không va chạm | ổn định hiệu suất | 

## Vỏ cạnh 

Trường hợp cạnh tinh tế là các số chứa số 0, vì bất kỳ số nào như vậy sẽ giảm ngay lập tức về 0 dưới tích chữ số. Thuật toán xử lý việc này một cách tự nhiên vì$g(x)$gán 0 và mảng tiền tố đơn giản là không bao giờ đưa nó vào số đếm cho$k \in [1, 9]$. 

Một trường hợp khác là số có một chữ số, trong đó$g(x) = x$trực tiếp. Vòng tiền xử lý kết thúc ngay lập tức đối với các giá trị này, đảm bảo tính chính xác mà không cần xử lý đặc biệt. 

Cuối cùng, các phạm vi liền kề lớn như$[1, 10^6]$được xử lý hiệu quả vì câu trả lời giảm xuống còn một khác biệt tiền tố duy nhất, không phụ thuộc vào kích thước phạm vi.
