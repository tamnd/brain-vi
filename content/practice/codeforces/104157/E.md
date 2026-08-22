---
title: "CF 104157E - Động não không cần não"
description: "Chúng ta được cấp một chuỗi các khe thời gian $N$. Trong mỗi vị trí $i$, có ba nhân viên và mỗi nhân viên sẽ đóng góp một số ý tưởng đã biết nếu được mời trong vị trí đó. Tuy nhiên, Michael có hai hạn chế tương tác theo cách không cần thiết."
date: "2026-07-02T01:15:32+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104157
codeforces_index: "E"
codeforces_contest_name: "UTPC Contest 01-27-23 Div. 2 (Beginner)"
rating: 0
weight: 104157
solve_time_s: 68
verified: false
draft: false
---

[CF 104157E - Động não không cần não](https://codeforces.com/problemset/problem/104157/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 8 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi$N$khe thời gian. Trong mỗi khe$i$, có ba nhân viên và mỗi nhân viên sẽ đóng góp một số ý tưởng đã biết nếu được mời trong thời gian đó. Tuy nhiên, Michael có hai hạn chế tương tác theo cách không cần thiết. Đầu tiên, tại mỗi vị trí, anh ta có thể mời tối đa một nhân viên, do đó, mỗi bước thời gian sẽ không đóng góp ý tưởng nào hoặc chính xác một trong ba giá trị có sẵn. Thứ hai, anh ta không thể sắp xếp các cuộc họp theo các khung giờ liên tiếp nên sau khi chọn một khung giờ, khung giờ tiếp theo phải được bỏ qua. 

Nhiệm vụ là chọn một tập hợp con các vị trí không có hai vị trí liền kề và đối với mỗi vị trí được chọn, hãy chọn một trong ba giá trị, tối đa hóa tổng. 

Những hạn chế$N \le 1000$và các giá trị lên tới 1000 có nghĩa là giải pháp quy hoạch động bậc hai là hoàn toàn an toàn. Bất cứ điều gì$O(N^3)$hoặc liên quan đến việc liệt kê tập hợp con đầy đủ trở nên không cần thiết và sẽ quá chậm trong trường hợp xấu nhất vì$2^{1000}$việc lựa chọn là không thể thực hiện được. 

Một số tình huống cận biên quan trọng đối với tính chính xác: 

Nếu$N = 1$, chúng tôi chỉ cần chọn giá trị tốt nhất trong số ba giá trị trong ô đó. Việc triển khai DP ngây thơ giả định các trạng thái trước đó tồn tại có thể thất bại nếu nó không khởi tạo các trường hợp cơ sở một cách chính xác. 

Nếu tất cả các giá trị đều bằng 0 thì câu trả lời đúng sẽ là 0 bất kể có bỏ qua các ràng buộc hay không. Cách tiếp cận tham lam luôn lấy mức tối đa cục bộ cho mỗi vị trí không thành công vì nó có thể chọn các vị trí liền kề và vi phạm quy tắc. 

Nếu một vị trí có giá trị rất lớn nhưng lại liền kề với một vị trí có kích thước vừa phải khác, thì giải pháp tối ưu có thể bỏ qua hoàn toàn vị trí vừa phải để lấy cả vị trí lớn và vị trí trong tương lai xa. Điều này loại trừ việc lựa chọn tham lam chỉ theo giá trị vị trí. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là xem xét mọi tập hợp con của các vị trí không có hai chỉ số liên tiếp và đối với mỗi vị trí được chọn, hãy chọn người giỏi nhất trong ba nhân viên. Số tập hợp con hợp lệ của các vị trí không liền kề là dạng Fibonacci, đại khái là$F_{N+2}$, là số mũ trong$N$. Ngay cả trước khi xem xét lựa chọn nhân viên, điều này đã trở nên quá lớn đối với$N = 1000$. Nhân với 3 lựa chọn cho mỗi vị trí đã chọn thậm chí còn tệ hơn. 

Cấu trúc của bài toán gợi ý một quy trình động tiêu chuẩn trên các vị trí. Quan sát quan trọng là quyết định tại thời điểm$i$chỉ phụ thuộc vào việc chúng ta có giành được vị trí hay không$i-1$. Nếu chúng ta chiếm chỗ$i$, chúng ta phải đến từ$i-2$. Nếu chúng ta bỏ qua vị trí$i$, chúng ta kế thừa giá trị tốt nhất từ$i-1$. Điều này làm giảm vấn đề xuống mức tái phát tuyến tính trong đó mỗi trạng thái chỉ phụ thuộc vào hai chỉ số trước đó. 

Chúng tôi tính toán, đối với mỗi vị trí, giá trị tốt nhất có thể có được nếu chúng tôi chọn vị trí đó, đơn giản là$\max(a_i, b_i, c_i)$. Sau đó, chúng tôi áp dụng DP “tổng tối đa của các phần tử không liền kề” cổ điển trên mảng dẫn xuất này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^N)$|$O(N)$| Quá chậm | 
| DP tối ưu |$O(N)$|$O(N)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

hãy để$w_i = \max(a_i, b_i, c_i)$, mức tăng tốt nhất có thể đạt được nếu vị trí$i$được chọn. 

1. Tính toán trước$w_i$cho mỗi khe$i$. Điều này nén lựa chọn của nhân viên thành một giá trị duy nhất cho mỗi khoảng thời gian vì dù sao thì chỉ có thể chọn một nhân viên. 
2. Xác định mảng DP trong đó$dp[i]$đại diện cho tổng số ý tưởng tối đa mà chúng ta có thể có được chỉ khi xem xét ý tưởng đầu tiên$i$khe cắm. 
3. Khởi tạo$dp[0] = 0$, vì không có slot nghĩa là không có ý tưởng. Bộ$dp[1] = w_1$, vì chỉ có một chỗ trống nên chúng tôi có thể lấy hoặc bỏ, và việc lấy luôn là tối ưu. 
4. Đối với mỗi vị trí$i$từ 2 đến$N$, tính:$$dp[i] = \max(dp[i-1], dp[i-2] + w_i)$$Thuật ngữ đầu tiên tương ứng với việc bỏ qua vị trí$i$, giữ kết quả tốt nhất cho đến nay. Thuật ngữ thứ hai tương ứng với việc lấy slot$i$, lực lượng khe$i-1$bị loại trừ. 
5. Câu trả lời là$dp[N]$. 

### Tại sao nó hoạt động 

Tại mỗi vị trí$i$, mọi lịch trình hợp lệ phải thuộc đúng một trong hai loại: lịch trình không sử dụng slot$i$và lịch trình sử dụng thời điểm$i$. Nếu khe$i$không được sử dụng thì giải pháp tối ưu chính xác là giải pháp tốt nhất trong lần đầu tiên$i-1$khe cắm, đó là$dp[i-1]$. Nếu khe$i$được sử dụng, khe cắm$i-1$bị cấm và tổng số tốt nhất có thể trở thành giải pháp tối ưu lên đến$i-2$cộng thêm$w_i$, đó là$dp[i-2] + w_i$. Vì hai trường hợp này bao gồm tất cả các giải pháp hợp lệ mà không trùng lặp, nên tính tối ưu bảo toàn tối đa của chúng theo cách quy nạp trên tất cả các tiền tố. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))
    c = list(map(int, input().split()))
    
    w = [max(a[i], b[i], c[i]) for i in range(n)]
    
    if n == 0:
        print(0)
        return
    if n == 1:
        print(w[0])
        return
    
    dp0 = 0
    dp1 = w[0]
    
    for i in range(1, n):
        dpi = max(dp1, dp0 + w[i])
        dp0, dp1 = dp1, dpi
    
    print(dp1)

if __name__ == "__main__":
    solve()
```Việc triển khai sẽ nén từng vị trí thành một giá trị tốt nhất có thể đạt được, loại bỏ hoàn toàn thứ nguyên nhân viên. Sau đó, DP được duy trì bằng cách sử dụng các biến cuộn thay vì mảng, vì chỉ cần có hai trạng thái trước đó. Điều này tránh việc sử dụng bộ nhớ không cần thiết trong khi vẫn giữ logic đơn giản. 

Việc khởi tạo xử lý ranh giới một cách rõ ràng bằng cách tách các phần$n = 1$trường hợp. Việc lặp lại bắt đầu ở chỉ số 1 vì chỉ số 0 đã được biểu diễn trong`dp1`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
3 0 3 1 2
3 1 4 4 4
1 2 1 4 4
```Tính toán đầu tiên$w_i$: 

| tôi | a_i | b_i | c_i | w_i | 
| --- | --- | --- | --- | --- | 
| 1 | 3 | 3 | 1 | 3 | 
| 2 | 0 | 1 | 2 | 2 | 
| 3 | 3 | 4 | 1 | 4 | 
| 4 | 1 | 4 | 4 | 4 | 
| 5 | 2 | 4 | 4 | 4 | 

Bây giờ tiến hóa DP: 

| tôi | w_i | dp[i-2] | dp[i-1] | dp[i] | 
| --- | --- | --- | --- | --- | 
| 1 | 3 | - | 0 | 3 | 
| 2 | 2 | 0 | 3 | 3 | 
| 3 | 4 | 3 | 3 | 7 | 
| 4 | 4 | 3 | 7 | 7 | 
| 5 | 4 | 7 | 7 | 11 | 

Câu trả lời cuối cùng là 11. 

Dấu vết này cho thấy vị trí 3 trở thành một bước ngoặt như thế nào: lấy nó sẽ mở khóa tổng điểm cao hơn mặc dù vị trí 2 bị bỏ qua, điều này xác nhận lý do tại sao lựa chọn cục bộ tham lam sẽ thất bại. 

### Ví dụ 2 

đầu vào:```
4
5 1 1 10
1 5 1 1
1 1 5 1
```Tính toán$w$: 

| tôi | w_i | 
| --- | --- | 
| 1 | 5 | 
| 2 | 5 | 
| 3 | 5 | 
| 4 | 10 | 

DP: 

| tôi | w_i | dp[i-2] | dp[i-1] | dp[i] | 
| --- | --- | --- | --- | --- | 
| 1 | 5 | - | 0 | 5 | 
| 2 | 5 | 0 | 5 | 5 | 
| 3 | 5 | 5 | 5 | 10 | 
| 4 | 10 | 5 | 10 | 15 | 

Câu trả lời là 15, đạt được bằng cách lấy các vị trí 1, 3 và 4 là vùng lân cận không hợp lệ nên thực sự tối ưu là các vị trí 1, 3, 4 sẽ vi phạm ràng buộc, DP ngăn chặn chính xác điều đó bằng cách thực thi các quy tắc bỏ qua. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N)$| Mỗi vị trí được xử lý một lần với công việc liên tục | 
| Không gian |$O(1)$| Chỉ có hai trạng thái DP luân phiên được lưu trữ | 

Độ phức tạp tuyến tính dễ dàng phù hợp với$N \le 1000$và mức sử dụng bộ nhớ là không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    solve()
    return sys.stdout.getvalue().strip()

# sample
assert run("""5
3 0 3 1 2
3 1 4 4 4
1 2 1 4 4
""") == "11"

# minimum case
assert run("""1
5
1
2
""") == "5"

# all zeros
assert run("""3
0 0 0
0 0 0
0 0 0
""") == "0"

# alternating high values
assert run("""5
10 1 10 1 10
1 10 1 10 1
1 1 1 1 1
""") == "30"

# boundary adjacency trap
assert run("""4
10 1 1 10
1 10 1 1
1 1 10 1
""") == "20"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Hộp đựng 1 khe | 5 | khởi tạo cơ sở | 
| tất cả số không | 0 | không có lựa chọn tiêu cực hoặc ép buộc | 
| mức cao xen kẽ | 30 | bỏ qua tính đúng đắn logic | 
| bẫy lân cận | 20 | ngăn chặn việc lấy các khe liên tiếp | 

## Vỏ cạnh 

cho$N = 1$, việc khởi tạo DP trực tiếp trả về giá trị tối đa của ba giá trị vì không có trạng thái trước đó để so sánh. Sự lặp lại không bao giờ được sử dụng, điều này tránh được việc truy cập không hợp lệ vào$dp[i-2]$. 

Đối với đầu vào hoàn toàn bằng 0, mọi$w_i = 0$, do đó mọi chuyển đổi đều giữ DP ở mức 0. Cả việc lấy và bỏ qua đều tạo ra kết quả như nhau và thuật toán luôn duy trì số 0 mà không có sự tích lũy ngẫu nhiên. 

Đối với trường hợp các giá trị lớn xuất hiện trong các vị trí liền kề, phép truy toán buộc phải lựa chọn giữa chúng. Nếu khe$i$được lấy, khe$i-1$bị loại trừ bất kể giá trị của nó, do đó DP tránh được sự tích lũy tham lam không hợp lệ một cách tự nhiên và duy trì tính chính xác thông qua việc phân tách có hiệu lực.
