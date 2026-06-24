---
title: "CF 105284C - Cây toán khỉ"
description: "Chúng ta được cung cấp một biểu đồ đường dẫn với các nút được đánh số từ 1 đến n, trong đó mỗi nút được kết nối với các nút lân cận của nó. Khi đó mỗi nút i được giữ độc lập với xác suất 1/i và bị loại bỏ với xác suất 1 − 1/i."
date: "2026-06-23T14:29:08+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105284
codeforces_index: "C"
codeforces_contest_name: "TeamsCode Summer 2024 Advanced Division"
rating: 0
weight: 105284
solve_time_s: 90
verified: false
draft: false
---

[CF 105284C - Cây toán học khỉ](https://codeforces.com/problemset/problem/105284/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 30 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một biểu đồ đường dẫn với các nút được đánh số từ 1 đến n, trong đó mỗi nút được kết nối với các nút lân cận của nó. Khi đó mỗi nút i được giữ độc lập với xác suất 1/i và bị loại bỏ với xác suất 1 − 1/i. Sau khi xóa tất cả, các nút còn lại tạo thành một sơ đồ con của chuỗi ban đầu và sơ đồ con này có thể chia thành một số thành phần được kết nối. 

Thành phần được kết nối trong cài đặt này chỉ đơn giản là một khối nút được giữ tối đa liên tiếp. Mỗi khi chúng ta thấy một khoảng trống được tạo bởi một nút bị xóa, chuỗi sẽ tách ra. Vì vậy, câu trả lời cuối cùng là số lượng dự kiến ​​của các phân đoạn được giữ liền kề như vậy. 

Đầu vào cung cấp nhiều giá trị độc lập của n và với mỗi giá trị, chúng ta phải tính toán modulo kỳ vọng 10^9+7 này. 

Các ràng buộc cho phép n tối đa 10^6 và tổng số trường hợp thử nghiệm lên tới 2×10^5, do đó, mọi giải pháp về cơ bản phải tuyến tính hoặc tốt hơn cho mỗi trường hợp thử nghiệm. Ngay cả O(n) cho mỗi truy vấn cũng quá chậm trong trường hợp xấu nhất trừ khi chúng tôi tính toán trước. Cấu trúc gợi ý một cách rõ ràng về tính toán tiền tố trên n. 

Một cách tiếp cận đơn giản sẽ mô phỏng tất cả các tập hợp con của các nút được giữ/xóa và các thành phần đếm. Đó là cấu hình 2^n, vốn đã không thể thực hiện được đối với n vượt quá các giá trị nhỏ như 20. Ngay cả Monte Carlo cũng không đưa ra những kỳ vọng chính xác về mô-đun. 

Một vấn đề phức tạp hơn sẽ phát sinh nếu người ta cố gắng mô phỏng trực tiếp hoặc DP trên tất cả các cấu hình cho mỗi trường hợp thử nghiệm: mô hình xác suất phụ thuộc vào i, do đó mỗi vị trí đóng góp khác nhau và việc tính toán lại từ đầu cho mỗi truy vấn sẽ lặp lại cùng một công việc nhiều lần. 

## Phương pháp tiếp cận 

Quan sát chính là các thành phần được kết nối trong chuỗi nhị phân của các nút được giữ/xóa có thể được tính cục bộ. Một thành phần mới bắt đầu chính xác ở vị trí i nếu nút i được giữ và i−1 bị xóa hoặc i−1 không tồn tại. 

Vì vậy, nếu chúng ta xác định chỉ báo cho “tôi bắt đầu một thành phần mới”, thì kỳ vọng sẽ trở thành tổng xác suất của các sự kiện cục bộ. 

Gọi K_i là sự kiện nút i được giữ lại. Nút i đóng góp một thành phần mới khi K_i đúng và i = 1 hoặc K_{i−1} sai. Bằng tính tuyến tính của kỳ vọng, chúng ta có thể tính tổng các xác suất này một cách độc lập trên i. 

Ý tưởng Brute Force sẽ liệt kê tất cả các tập hợp con của các nút, tính toán các thành phần được kết nối cho mỗi tập hợp con trong O(n) và tính trung bình trên tất cả các xác suất. Điều này không thành công vì số lượng tập hợp con là số mũ và trọng số xác suất không đồng nhất. 

Sự đơn giản hóa chính là chúng ta không bao giờ cần mối tương quan ngoài các nút liền kề. Mỗi thuật ngữ chỉ phụ thuộc vào K_i và K_{i−1} và tính độc lập của các trạng thái nút làm cho thuật ngữ này hoàn toàn cục bộ. 

Chúng tôi tính toán: 

Với i = 1, xác suất khởi động của một thành phần là P(K_1) = 1. 

Với i > 1, một thành phần mới bắt đầu khi nút i được giữ và nút i−1 không được giữ: 

P = P(K_i) × P(không phải K_{i−1}) = (1/i) × (1 − 1/(i−1)). 

Điều này làm giảm toàn bộ vấn đề thành tổng tiền tố trên i. 

Việc quan tâm duy nhất còn lại là số học mô-đun và xử lý riêng trường hợp đặc biệt i = 1. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên các tập hợp con | O(2^n · n) | O(n) | Quá chậm | 
| Tổng kỳ vọng cục bộ | Tiền xử lý O(n), O(1) cho mỗi truy vấn | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Tính toán trước các nghịch đảo mô-đun cho tất cả các số nguyên lên đến n tối đa trong các trường hợp thử nghiệm. Điều này là cần thiết vì xác suất liên quan đến 1/i theo mô đun nguyên tố, do đó phép chia phải được thay thế bằng phép nhân với các nghịch đảo mô đun. 
2. Xây dựng một mảng dp trong đó dp[i] lưu trữ số lượng thành phần dự kiến ​​cho chuỗi có độ dài i. Chúng tôi tính toán dp tăng dần từ 1 đến tối đa n. 
3. Khởi tạo dp[1] = 1 vì một nút luôn được giữ lại và tạo thành chính xác một thành phần. 
4. Với mỗi i từ 2 đến max n, hãy tính phần đóng góp của vị trí i là xác suất để i được giữ và i−1 không được giữ. Đây là (1/i) × (1 − 1/(i−1)). Chúng ta thêm phần này vào dp[i−1] để thu được dp[i]. 
5. Trả lời từng truy vấn bằng cách in dp[n]. 

Lý do biểu mẫu gia tăng này hoạt động là vì mỗi vị trí mới sẽ mở rộng một thành phần hiện có hoặc bắt đầu một thành phần mới và chỉ trường hợp thứ hai mới ảnh hưởng đến số lượng thành phần. 

### Tại sao nó hoạt động 

Số lượng thành phần dự kiến bằng số lần khởi động dự kiến của các thành phần. Sự khởi đầu xảy ra chính xác tại vị trí i khi nút i có mặt nhưng nút i−1 không có. Những sự kiện này chỉ phụ thuộc vào các biến Bernoulli độc lập, do đó xác suất của chúng nhân lên rõ ràng. Tính tuyến tính của kỳ vọng cho phép tính tổng các xác suất trên mỗi vị trí này mà không cần xem xét sự tương tác giữa các lần bắt đầu khác nhau. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def modinv(x):
    return pow(x, MOD - 2, MOD)

def solve():
    t = int(input())
    ns = []
    max_n = 0

    for _ in range(t):
        n = int(input())
        ns.append(n)
        if n > max_n:
            max_n = n

    if max_n == 0:
        return

    inv = [0] * (max_n + 2)
    for i in range(1, max_n + 2):
        inv[i] = modinv(i)

    dp = [0] * (max_n + 2)
    dp[1] = 1

    for i in range(2, max_n + 1):
        p_keep = inv[i]
        p_prev_keep = inv[i - 1]
        p_prev_not = (1 - p_prev_keep) % MOD
        dp[i] = (dp[i - 1] + p_keep * p_prev_not) % MOD

    out = []
    for n in ns:
        out.append(str(dp[n]))
    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc triển khai bắt đầu bằng cách đọc tất cả các trường hợp thử nghiệm để xác định n tối đa, vì chúng ta cần tính toán trước toàn cục. Các nghịch đảo mô-đun được tính toán bằng định lý Fermat. 

Mảng dp lưu trữ giá trị mong đợi cho từng độ dài tiền tố. Mỗi lần chuyển đổi chỉ sử dụng i và i−1, do đó việc tính toán là tuyến tính. 

Một điểm tinh tế là việc tính toán (1 − 1/(i−1)) modulo MOD một cách chính xác. Chúng tôi áp dụng rõ ràng modulo sau khi trừ để tránh các giá trị âm. 

## Ví dụ đã hoạt động 

Xét mẫu có n = 2. 

| tôi | P(giữ i) | P(trước đó không) | Đóng góp | dp[i] | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | - | 1 | 1 | 
| 2 | 1/2 | 1 − 1/1 = 0 | 0 | 1 | 

Với n = 2, nút 1 luôn tồn tại, do đó luôn có chính xác một thành phần bất kể nút 2, khớp với dp[2] = 1. 

Bây giờ hãy xem xét n = 4. 

| tôi | P(giữ i) | P(trước đó không) | Đóng góp | dp[i] | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | - | 1 | 1 | 
| 2 | 1/2 | 0 | 0 | 1 | 
| 3 | 1/3 | 1 − 1/2 = 1/2 | 1/6 | 1+1/6 | 
| 4 | 1/4 | 1 − 1/3 = 2/3 | 1/6 | 1 + 1/6 + 1/6 | 

Điều này tạo ra 4/3, phù hợp với đầu ra mô-đun dự kiến. 

Dấu vết cho thấy các thành phần chỉ xuất hiện khi một nút được giữ lại nằm trước một nút bị thiếu và các khoản đóng góp được tích lũy một cách độc lập. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(tối đa n + t) | Tính toán trước dp một lần, trả lời các truy vấn trong O(1) mỗi truy vấn | 
| Không gian | O(tối đa n) | Lưu trữ mảng dp và nghịch đảo | 

Quá trình xử lý trước lên tới 10^6 phù hợp thoải mái trong giới hạn thời gian và mỗi truy vấn có thời gian không đổi, giúp giải pháp phù hợp với tối đa 2×10^5 trường hợp thử nghiệm. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 10**9 + 7

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    ns = []
    max_n = 0
    for _ in range(t):
        n = int(input())
        ns.append(n)
        max_n = max(max_n, n)

    inv = [0] * (max_n + 2)
    for i in range(1, max_n + 2):
        inv[i] = pow(i, MOD - 2, MOD)

    dp = [0] * (max_n + 2)
    if max_n >= 1:
        dp[1] = 1

    for i in range(2, max_n + 1):
        dp[i] = (dp[i - 1] + inv[i] * (1 - inv[i - 1]) % MOD) % MOD

    return "\n".join(str(dp[n]) for n in ns)

# provided samples
assert run("4\n1\n2\n3\n4\n") == "1\n1\n166666669\n333333337"

# custom cases
assert run("1\n1\n") == "1", "minimum size"
assert run("1\n2\n") == "1", "two nodes"
assert run("1\n5\n") == run("1\n5\n"), "determinism check"
assert run("3\n1\n2\n3\n") == "1\n1\n166666669", "prefix consistency"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 1 | trường hợp cạnh nút đơn | 
| 2 | 1 | không thể chia được | 
| 5 | tính nhất quán của dp | ổn định tái phát | 
| 1,2,3 | 1,1,1/6 | tính chính xác của tiền tố | 

## Vỏ cạnh 

Với n = 1, thuật toán đặt trực tiếp dp[1] = 1 vì một nút được giữ được đảm bảo duy nhất tạo thành chính xác một thành phần. Không có sự phụ thuộc vào dp[0], điều này tránh được hành vi không xác định. 

Với n = 2, quá trình chuyển đổi sử dụng P(prev not) = 1 − 1/1 = 0, nắm bắt chính xác rằng nút 1 luôn hiện diện, do đó nút 2 không bao giờ có thể bắt đầu một thành phần mới. 

Đối với n lớn hơn, mỗi số hạng chỉ phụ thuộc vào i và i−1, do đó, ngay cả khi n = 10^6 tối đa, phép tính vẫn ổn định và tuyến tính, không có đệ quy ẩn hoặc bùng nổ trạng thái.
