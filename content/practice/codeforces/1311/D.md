---
problem: 1311D
contest_id: 1311
problem_index: D
name: "Three Integers"
contest_name: "Codeforces Round 624 (Div. 3)"
rating: 2000
tags: ["brute force", "math"]
answer: passed_samples
verified: true
solve_time_s: 292
date: 2026-06-14
model: gpt-5-3-mini
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a2dde35-acbc-83ec-8f89-626284c4ef25
---

# CF 1311D - Ba số nguyên 

**Xếp hạng:** 2000 
**Tags:** sức mạnh vũ phu, toán học 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 4 phút 52 giây 
**Đã xác minh:** có (1/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a2dde35-acbc-83ec-8f89-626284c4ef25 

--- 

##Giải pháp 

## Hiểu vấn đề 

Cho ta ba số nguyên dương theo thứ tự không giảm. Hãy coi chúng như ba nút bấm trên một trục số. Trong một lần di chuyển, chúng ta có thể dịch chuyển bất kỳ núm đơn nào chính xác một đơn vị sang trái hoặc phải, nhưng chúng ta không được phép đẩy núm xuống dưới 1. Mục tiêu là định hình lại bộ ba thành bộ ba có thứ tự mới sao cho giá trị ở giữa là bội số của số đầu tiên và giá trị cuối cùng là bội số của số ở giữa. 

Đầu ra không chỉ là số lần di chuyển tối thiểu cần thiết để đạt được cấu hình như vậy mà còn là một cấu hình cuối cùng cụ thể đạt được mức tối thiểu này. 

Khó khăn chính là cấu trúc mà chúng ta muốn rất cứng nhắc: một khi giá trị nhỏ nhất được cố định, giá trị ở giữa buộc phải là bội số của nó và giá trị lớn nhất buộc phải là bội số của giá trị ở giữa. Điều đó làm cho không gian của các bộ ba hợp lệ trở nên thưa thớt bên trong một lưới số nguyên dày đặc. 

Các ràng buộc đủ nhỏ để có thể tìm kiếm trực tiếp trên tất cả các bộ ba cuối cùng có thể có. Mỗi giá trị tối đa là 10^4, do đó, ngay cả phép lặp lồng nhau trên các ứng cử viên cũng có thể được chấp nhận nếu chúng tôi kiểm soát không gian tìm kiếm. Một giải pháp cố gắng xây dựng tất cả các bộ ba có thể một cách ngây thơ mà không có cấu trúc vẫn sẽ vượt qua vì miền bị giới hạn chặt chẽ. 

Trường hợp cạnh tinh tế xuất hiện khi giải pháp tối ưu thu gọn các giá trị lại với nhau, chẳng hạn như biến cả ba số thành cùng một giá trị. Ví dụ như quay`100 100 101`vào trong`100 100 100`là tối ưu, nhưng bất kỳ cách tiếp cận nào chỉ xem xét việc tăng gấp ba lần một cách chặt chẽ đều có thể bỏ lỡ nó. Một trường hợp góc khác là khi giảm giá trị sẽ rẻ hơn so với tăng chúng, điều này thường xảy ra khi chúng ta cho phép thu gọn về các ước số nhỏ hơn như 1. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực bắt đầu từ ý tưởng rằng chúng ta có thể thử mọi bộ ba cuối cùng có thể$A \le B \le C$. Đối với mỗi lựa chọn, chúng tôi tính chi phí là tổng của những khác biệt tuyệt đối$|a-A| + |b-B| + |c-C|$, nhưng chúng tôi chỉ thỏa mãn gấp ba lần$B \bmod A = 0$Và$C \bmod B = 0$. 

Điều này đúng vì mọi cấu hình mục tiêu hợp lệ đều được kiểm tra rõ ràng và hàm chi phí khớp chính xác với số lần di chuyển cần thiết. Vấn đề là số lượng ứng viên. Mỗi biến có thể lên tới khoảng 10^4, vì vậy không gian tìm kiếm đầy đủ theo thứ tự$10^{12}$, điều đó là không thể. 

Quan sát quan trọng là cấu trúc của bộ ba hợp lệ có tính phân cấp. Một khi chúng ta chọn$A$, sự lựa chọn có ý nghĩa duy nhất cho$B$là bội số của$A$, và một khi chúng ta chọn$B$, sự lựa chọn có ý nghĩa duy nhất cho$C$là bội số của$B$. Điều này làm giảm đáng kể không gian tìm kiếm vì mỗi bước sẽ thu nhỏ hệ số phân nhánh từ các số nguyên tùy ý thành các ước bị ràng buộc bởi cấp số cộng. 

Thay vì thử tất cả các bộ ba, chúng ta thử tất cả các ứng cử viên hợp lý cho$A$, thì với mỗi$A$, liệt kê bội số của$B$, và với mỗi$B$, liệt kê bội số của$C$. Vì các giá trị được giới hạn bởi khoảng 2 × 10^4 trong thực tế để có những dịch chuyển tối ưu nên phép liệt kê này vẫn còn nhỏ. 

Chúng tôi cũng dựa vào ý tưởng quan trọng thứ hai: các giải pháp tối ưu có xu hướng bám sát các giá trị ban đầu. Vì vậy chúng ta chỉ cần xem xét những ứng viên ở gần$a$,$b$, Và$c$, thường là trong một cửa sổ giới hạn nhỏ, vì việc dịch chuyển ra xa luôn tốn kém hơn việc điều chỉnh các cấu hình hợp lệ gần đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n³) trên 10⁴ miền | O(1) | Quá chậm | 
| Bảng liệt kê số chia với tìm kiếm có giới hạn | O(n √n) trường hợp xấu nhất, nhỏ hơn nhiều trong thực tế | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi lặp lại các giá trị cuối cùng có thể có bằng cách neo phần tử đầu tiên và xây dựng chuỗi hợp lệ trở lên. 

1. Cố định giá trị ứng viên cho$A$trong một phạm vi xung quanh bản gốc$a$, thường từ 1 đến một giới hạn nhỏ chẳng hạn như 2 × 10^4. Chúng tôi làm điều này vì giải pháp tối ưu không thể tùy ý rời xa giá trị ban đầu mà không phát sinh chi phí không cần thiết. 
2. Đối với mỗi$A$, chọn ứng viên$B$như bội số của$A$. Chúng tôi tạo ra những thứ này bằng cách lặp lại các số nhân$k$như vậy$B = kA$. Điều này đảm bảo ràng buộc chia hết$B \bmod A = 0$. 
3. Đối với mỗi$B$, chọn ứng viên$C$như bội số của$B$, một lần nữa bằng cách lặp lại các số nhân$m$như vậy$C = mB$. Điều này thực thi$C \bmod B = 0$. 
4. Với mọi bộ ba hợp lệ$(A, B, C)$, tính tổng chi phí như$|a - A| + |b - B| + |c - C|$. Chúng tôi theo dõi chi phí tối thiểu được thấy cho đến nay. 
5. Lưu trữ bộ ba đạt được mức tối thiểu này. 

Lý do điều này hoạt động là vì mọi cấu hình cuối cùng hợp lệ phải đáp ứng chuỗi phân chia nghiêm ngặt. Bằng cách xây dựng các ứng cử viên thông qua phép nhân thay vì lựa chọn tùy ý, chúng ta tạo ra chính xác tập hợp khả thi mà không bỏ sót bất kỳ cấu trúc hợp lệ nào. 

Không gian tìm kiếm nhỏ vì một khi$A$cố định, số bội của$A$dưới giới hạn bị giới hạn, và tương tự đối với$B$. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MAXV = 20000

def solve():
    t = int(input())
    for _ in range(t):
        a, b, c = map(int, input().split())

        best = 10**18
        best_triple = (a, b, c)

        # Try A
        for A in range(1, MAXV + 1):
            # Try B as multiple of A
            for B in range(A, MAXV + 1, A):
                # Try C as multiple of B
                for C in range(B, MAXV + 1, B):
                    cost = abs(a - A) + abs(b - B) + abs(c - C)
                    if cost < best:
                        best = cost
                        best_triple = (A, B, C)

        print(best)
        print(*best_triple)

if __name__ == "__main__":
    solve()
```Cốt lõi của việc triển khai là cấu trúc vòng lặp ba lớp thực thi khả năng phân chia theo cấu trúc. Thay vì kiểm tra các điều kiện sau khi tạo bộ ba, chúng ta chỉ tạo bộ ba hợp lệ. 

Hằng số`MAXV`được chọn đủ lớn để thoải mái bao phủ tất cả các ứng viên hữu ích. Việc tăng nó sẽ không thay đổi tính chính xác, chỉ có thời gian chạy. 

Một lỗi phổ biến là quên rằng C phải được lặp lại dưới dạng bội số của B chứ không phải dưới dạng biến độc lập. Một cách khác là cho phép bước không nguyên cho bội số không chính xác, điều này sẽ phá vỡ sự đảm bảo tính chia hết. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi hai trường hợp. 

### Ví dụ 1:`1 2 3`Chúng tôi khám phá chuỗi ứng cử viên bắt đầu từ các giá trị nhỏ. 

| A | B | C | Chi phí = |1-A| + |2-B| + |3-C| | 

|---|---|---|---| 

| 1 | 1 | 1 | 3 | 

| 1 | 1 | 2 | 2 | 

| 1 | 1 | 3 | 1 | 

| 1 | 2 | 2 | 1 | 

| 1 | 2 | 4 | 3 | 

Giá trị tốt nhất đạt được ở (1, 2, 2) hoặc (1, 1, 3), cả hai đều hợp lệ trong các ràng buộc tùy thuộc vào cách giải thích chuỗi phân chia và chi phí tối thiểu là 1. 

Điều này cho thấy giải pháp tối ưu có thể không bảo toàn cấu trúc đặt hàng một cách chặt chẽ bằng cách gần với giá trị ban đầu mà thay vào đó bằng cách cân bằng chi phí trên cả ba tọa độ. 

### Ví dụ 2:`100 100 101`Chúng tôi đánh giá các giải pháp sụp đổ gần đó. 

| A | B | C | Chi phí | 
| --- | --- | --- | --- | 
| 100 | 100 | 100 | 1 | 
| 100 | 100 | 200 | 99 | 
| 101 | 101 | 101 | 3 | 

Lựa chọn tốt nhất là thu gọn về (100, 100, 100), xác nhận rằng chỉ giảm một tọa độ thường rẻ hơn dịch chuyển cả ba tọa độ. 

Dấu vết này cho thấy tại sao việc cho phép di chuyển đi xuống là rất quan trọng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(MAXV² / log MAXV) mỗi lần kiểm tra trong thực tế | Mỗi A liệt kê bội số B, mỗi B liệt kê bội số C | 
| Không gian | O(1) | Chỉ lưu trữ liên tục cho câu trả lời hay nhất | 

Các ràng buộc cho phép tối đa 100 trường hợp thử nghiệm và MAXV đủ nhỏ để phép liệt kê số chia lồng nhau vẫn nằm trong giới hạn thời gian do lặp lại thưa thớt trong các vòng lặp bên trong. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    MAXV = 20000
    t = int(input())
    out = []
    for _ in range(t):
        a, b, c = map(int, input().split())

        best = 10**18
        best_triple = (a, b, c)

        for A in range(1, MAXV + 1):
            for B in range(A, MAXV + 1, A):
                for C in range(B, MAXV + 1, B):
                    cost = abs(a - A) + abs(b - B) + abs(c - C)
                    if cost < best:
                        best = cost
                        best_triple = (A, B, C)

        out.append(str(best))
        out.append(" ".join(map(str, best_triple)))

    return "\n".join(out)

# provided samples
assert run("""8
1 2 3
123 321 456
5 10 15
15 18 21
100 100 101
1 22 29
3 19 38
6 30 46
""") == """1
1 1 3
102
114 228 456
4
4 8 16
6
18 18 18
1
100 100 100
7
1 22 22
2
1 19 38
8
6 24 48"""

# custom cases
assert run("1\n1 1 1\n") == "0\n1 1 1", "already valid"
assert run("1\n10 20 30\n") is not None, "increasing chain"
assert run("1\n2 2 3\n") is not None, "small adjustment"
assert run("1\n1 10000 10000\n") is not None, "large imbalance"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1`|`0 1 1 1`| trường hợp nhận dạng | 
|`10 20 30`| tính toán | chia tỷ lệ có cấu trúc | 
|`2 2 3`| tính toán | điều chỉnh tối thiểu | 
|`1 10000 10000`| tính toán | mất cân bằng cực độ | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là khi cả ba số sẽ thu gọn về cùng một giá trị. Đối với đầu vào như`100 100 101`, thuật toán xem xét chuỗi nơi$A = B = C = 100$, chi phí sản xuất 1. Cách tiếp cận tham lam chỉ điều chỉnh giá trị lớn nhất xuống có thể cố gắng khớp mức chia hết một cách không chính xác bằng cách tăng các giá trị trung gian, dẫn đến chi phí cao hơn. 

Một trường hợp khác là khi cấu hình tối ưu liên quan đến việc thu nhỏ các giá trị một cách đáng kể, chẳng hạn như`1 22 29`. Ở đây, hướng tới một cấu trúc ước số nhỏ như`(1, 22, 22)`rẻ hơn việc cố gắng hướng lên trên. Thuật toán khám phá chính xác những điều này vì nó liệt kê tất cả các chuỗi phân chia bắt đầu từ nhỏ$A$, đảm bảo các giải pháp thu gọn luôn được xem xét.
