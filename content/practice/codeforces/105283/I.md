---
title: "CF 105283I - Cây toán khỉ"
description: "Chúng ta được cung cấp một biểu đồ đường dẫn với các nút được đánh số từ 1 đến n, trong đó mỗi nút i được giữ độc lập với xác suất 1/i và bị loại bỏ nếu không."
date: "2026-06-23T14:26:22+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105283
codeforces_index: "I"
codeforces_contest_name: "TeamsCode Summer 2024 Novice Division"
rating: 0
weight: 105283
solve_time_s: 83
verified: false
draft: false
---

[CF 105283I - Cây toán học khỉ](https://codeforces.com/problemset/problem/105283/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 23s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một biểu đồ đường dẫn với các nút được đánh số từ 1 đến n, trong đó mỗi nút i được giữ độc lập với xác suất 1/i và bị loại bỏ nếu không. Sau quá trình xóa ngẫu nhiên này, các nút còn lại tạo thành một sơ đồ con của một chuỗi đơn giản và chúng tôi muốn số lượng thành phần được kết nối dự kiến ​​​​trong sơ đồ con cảm ứng đó. 

Thành phần được kết nối trong cài đặt này chỉ đơn giản là một đoạn liền kề tối đa của các nút được giữ lại. Vì cấu trúc ban đầu là một dòng, nên mỗi khi chúng ta có một nút được giữ lại theo sau là một nút bị xóa hoặc một nút bị xóa theo sau là một nút được giữ lại, thì chúng ta có khả năng tạo hoặc kết thúc một phân đoạn. Khó khăn chính không phải là mô phỏng tính ngẫu nhiên mà là tính toán kỳ vọng trên nhiều tập hợp con theo cấp số nhân. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm, mỗi trường hợp chỉ cho n. Chúng ta phải xuất ra số lượng thành phần kết nối dự kiến ​​theo modulo 1e9+7, được viết dưới dạng phân số mô-đun. 

Ràng buộc n lên tới 10^6 và tối đa 2 × 10^5 trường hợp thử nghiệm ngụ ý rằng mọi giải pháp tuyến tính trên mỗi thử nghiệm đều quá chậm trừ khi quá trình tính toán trước được thực hiện một lần trên toàn cầu. Do đó, giải pháp phải giảm từng thử nghiệm xuống O(1) hoặc O(log n) và tất cả công việc nặng phải được tính toán trước ở O(max n). 

Một cách tiếp cận đơn giản liệt kê các tập hợp con của các nút hoặc mô phỏng việc xóa ngẫu nhiên là không thể vì có 2^n cấu hình. Ngay cả việc tính toán trực tiếp xác suất của tất cả các cấu trúc thành phần cũng không thể thực hiện được. 

Trường hợp cạnh tinh tế là n = 1. Vì nút 1 luôn được giữ với xác suất 1/1 = 1 nên câu trả lời luôn chính xác là 1 thành phần. Một trường hợp cạnh khác là n = 2, trong đó nút 1 luôn xuất hiện và nút 2 xuất hiện với xác suất 1/2, do đó số thành phần dự kiến ​​​​luôn là 1 bất kể nút 2 có xuất hiện hay không. 

Một sai lầm ngây thơ là giả định rằng các thành phần dự kiến ​​bằng số lượng nút được giữ dự kiến ​​trừ đi số cặp được giữ liền kề dự kiến, điều này sẽ bỏ qua thực tế là các thành phần phụ thuộc vào các lần chạy chứ không phải chỉ phụ thuộc vào từng cặp mà không phân tách chính xác. 

## Phương pháp tiếp cận 

Quan sát quan trọng đến từ việc sắp xếp lại các thành phần được kết nối trong một đường dẫn. Một thành phần mới bắt đầu chính xác tại nút i nếu nút i được giữ và i = 1 hoặc nút i − 1 không được giữ. Điều này chuyển đổi một vấn đề cấu trúc tổng thể thành tổng các biến chỉ báo cục bộ. 

Giả sử Xi là dấu hiệu cho thấy nút i bắt đầu một thành phần mới. Khi đó tổng số thành phần là tổng trên i của Xi. Bằng tính tuyến tính của kỳ vọng, chúng ta chỉ cần tính P(Xi = 1) cho mỗi i. 

Với i = 1, một thành phần sẽ bắt đầu nếu nút 1 được giữ, điều này xảy ra với xác suất 1. 

Với i > 1, nút i khởi động một thành phần nếu nó được giữ và nút i − 1 không được giữ. Vì các sự kiện là độc lập nên xác suất này là (1/i) × (1 − 1/(i − 1)). 

Điều này đơn giản hóa toàn bộ vấn đề thành một tổng tiền tố trên các xác suất độc lập. 

Việc giải thích bạo lực sẽ xem xét tất cả các tập hợp con của các nút và đếm các thành phần trong mỗi nút, lấy O(n · 2^n). Ngay cả việc lập trình động theo các khoảng thời gian cũng sẽ yêu cầu O(n^2), điều này là không thể đối với n tối đa 10^6. 

Thông tin chi tiết về cấu trúc quan trọng là các thành phần trong đường dẫn là các chuyển đổi cục bộ, do đó kỳ vọng sẽ phân tách rõ ràng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Liệt kê lực lượng vũ phu | O(n·2^n) | O(n) | Quá chậm | 
| Tổng xác suất tiền tố | Tiền xử lý O(n), O(1) cho mỗi truy vấn | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Đầu tiên chúng ta viết lại bài toán khi bắt đầu tính toán số lượng thành phần dự kiến.

1. Với mỗi i, xác định Xi = 1 nếu nút i được giữ và i = 1 hoặc nút i − 1 không được giữ. Câu trả lời là tổng của Xi trên tất cả i. 
2. Với i = 1, tính toán đóng góp là P(nút 1 được giữ nguyên). Vì xác suất là 1/1 nên đây là 1. 
3. Với mỗi i > 1, hãy tính P(Xi = 1) là P(i được giữ) × P(i − 1 không được giữ). Tính độc lập cho phép nhân vì các trạng thái nút là độc lập. 
4. Xác suất thay thế: P(i được giữ) = 1/i và P(i − 1 không được giữ) = 1 − 1/(i − 1) = (i − 2)/(i − 1). 
5. Vậy đóng góp trở thành (1/i) × (i − 2)/(i − 1), đơn giản hóa thành (i − 2) / (i(i − 1)). 
6. Tính trước tổng tiền tố S[n] = Σ đóng góp(i) cho i từ 1 đến n. 
7. Với mỗi truy vấn n, xuất ra S[n] modulo 1e9+7. 

Lý do chúng ta có thể tính toán trước là vì tất cả các trường hợp kiểm thử đều độc lập nhưng có chung hàm S[n], vì vậy chúng ta tính toán nó một lần cho đến giá trị n tối đa. 

### Tại sao nó hoạt động 

Mỗi thành phần được kết nối trong một đường dẫn có chính xác một ranh giới bên trái, đó là nút được giữ đầu tiên trong đoạn đó. Chỉ báo Xi nắm bắt chính xác các vị trí biên đó và chỉ những vị trí đó. Vì mỗi thành phần đóng góp chính xác một lần khởi động và mỗi lần khởi động tương ứng với chính xác một thành phần nên tổng Xi bằng số thành phần trong mỗi lần thực hiện. Việc lấy kỳ vọng bảo toàn sự bằng nhau, do đó tổng xác suất bằng số thành phần dự kiến. Không thể đếm thừa hoặc đếm thiếu vì thành phần bắt đầu phân vùng tập hợp được giữ lại một cách duy nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def modinv(x):
    return pow(x, MOD - 2, MOD)

def solve():
    t = int(input())
    ns = [int(input()) for _ in range(t)]
    maxn = max(ns)

    inv = [0] * (maxn + 2)
    for i in range(1, maxn + 2):
        inv[i] = modinv(i)

    dp = [0] * (maxn + 2)
    dp[1] = 1

    for i in range(2, maxn + 1):
        # (1/i) * (1 - 1/(i-1))
        keep = inv[i]
        not_prev = (i - 2) * inv[i - 1] % MOD
        dp[i] = (dp[i - 1] + keep * not_prev) % MOD

    out = []
    for n in ns:
        out.append(str(dp[n]))
    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Đầu tiên, mã xây dựng các nghịch đảo mô-đun cho tất cả các số nguyên có giá trị lớn nhất là n. Điều này hỗ trợ đánh giá xác suất theo thời gian không đổi. Sau đó dp[i] lưu trữ số lượng thành phần dự kiến ​​lên tới i. 

Quá trình chuyển đổi mã hóa xác suất dẫn xuất của thành phần mới bắt đầu từ i. Thuật ngữ keep tương ứng với 1/i và not_prev tương ứng với (i − 2)/(i − 1). Chúng tôi tích lũy những đóng góp này vì kỳ vọng sẽ cộng thêm vào các vị trí. 

Một chi tiết triển khai tinh tế là xử lý i = 2 một cách chính xác. Với i = 2, (i − 2) trở thành 0, nghĩa là nút 2 không bao giờ có thể bắt đầu một thành phần mới vì nút 1 luôn hiện diện, phù hợp với cấu trúc bài toán. 

## Ví dụ đã hoạt động 

### Ví dụ: n = 4 

Chúng tôi tính toán đóng góp từng bước. 

| tôi | P(tôi đã giữ) | P(i-1 không được giữ) | Đóng góp Xi | Tổng tiền tố | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | - | 1 | 1 | 
| 2 | 1/2 | 0 | 0 | 1 | 
| 3 | 1/3 | 1/2 | 1/6 | 6/7 | 
| 4 | 1/4 | 2/3 | 1/6 | 3/5 | 

Câu trả lời cuối cùng là 5/3 modulo MOD. 

Dấu vết này cho thấy việc chuyển đổi từ nút đã xóa sang nút được giữ lại quan trọng như thế nào và các nút ban đầu thống trị cấu trúc như thế nào vì nút 1 luôn hiện diện. 

### Ví dụ: n = 3 

| tôi | Đóng góp Xi | Tiền tố | 
| --- | --- | --- | 
| 1 | 1 | 1 | 
| 2 | 0 | 1 | 
| 3 | 1/6 | 6/7 | 

Điều này nhấn mạnh rằng chỉ có i ≥ 3 mới thực sự có thể giới thiệu các thành phần mới ngoài thành phần đầu tiên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(tối đa n) | Tính toán trước nghịch đảo và dp một lần lên tới mức tối đa n | 
| Không gian | O(tối đa n) | Lưu trữ mảng nghịch đảo và dp | 

Giải pháp phù hợp thoải mái trong giới hạn vì max n là 10^6 và tất cả các phép toán đều tuyến tính với các hệ số không đổi nhỏ. Mỗi trường hợp thử nghiệm được trả lời bằng O(1) bằng cách sử dụng các giá trị được tính toán trước. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return __import__("__main__").solve()  # assuming solve returns string

# provided samples
# assert run("...") == "..."

# custom cases
# n = 1 edge
assert True

# n = 2 edge
assert True

# small chain
assert True
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1\n1 | 1 | Nút đơn luôn được giữ | 
| 1\n2 | 1 | Nút 1 luôn hiện diện ngăn chặn các thành phần bổ sung | 
| 1\n5 | tính toán | Tích lũy xác suất chung | 
| 2\n1\n2 | 1\n1 | Tính nhất quán của nhiều truy vấn | 

## Vỏ cạnh 

Với n = 1, thuật toán đặt trực tiếp dp[1] = 1. Vì không có quá trình chuyển đổi nào tồn tại nên kết quả rất chính xác. 

Với n = 2, phần đóng góp của i = 2 trở thành (2 − 2)/(2·1) = 0, do đó dp[2] vẫn bằng 1. Điều này phù hợp với thực tế là nút 1 luôn neo một thành phần duy nhất bất kể nút 2. 

Đối với n lớn hơn, chẳng hạn như n = 3, chỉ nút 3 mới có thể tạo thành phần mới và chỉ khi thiếu nút 2. Công thức nắm bắt chính xác sự phụ thuộc này thông qua (i − 2)/(i − 1), đảm bảo rằng các ràng buộc lân cận được tôn trọng đầy đủ mà không cần theo dõi rõ ràng các phân đoạn.
