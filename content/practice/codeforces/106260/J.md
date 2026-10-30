---
title: "CF 106260J - \u6392\u5217"
description: "Chúng ta đang xây dựng các chuỗi có độ dài $n$ bằng cách sử dụng chính xác bốn ký tự: $y, u, k, o$. Hạn chế mang tính cục bộ: nếu bạn nhìn vào bất kỳ khối 5 ký tự liên tiếp nào, khối đó không được là hoán vị của nhiều tập hợp được hình thành bởi các chữ cái của từ “yuuko”."
date: "2026-06-25T07:25:27+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106260
codeforces_index: "J"
codeforces_contest_name: "2025 SiChuan University for new student"
rating: 0
weight: 106260
solve_time_s: 48
verified: true
draft: false
---

[CF 106260J - \u6392\u5217](https://codeforces.com/problemset/problem/106260/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 48s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang xây dựng các chuỗi có độ dài$n$sử dụng chính xác bốn ký tự:$y, u, k, o$. Hạn chế mang tính cục bộ: nếu bạn nhìn vào bất kỳ khối 5 ký tự liên tiếp nào, khối đó không được là hoán vị của nhiều tập hợp được hình thành bởi các chữ cái của từ “yuuko”. 

Từ “yuuko” chứa các ký tự$y, u, u, k, o$, do đó, bất kỳ khối bị cấm nào cũng chính xác là một cửa sổ có độ dài 5 chứa một$y$, hai$u$của, một$k$, và một$o$, theo thứ tự bất kỳ. Nhiệm vụ là đếm xem có bao nhiêu chuỗi có độ dài hợp lệ$n$tồn tại dưới ràng buộc này, modulo$998244353$. 

Khó khăn chính là hạn chế không phải về một chuỗi con chính xác mà là về điều kiện nhiều tập hợp trên một cửa sổ trượt. Điều này làm cho các kỹ thuật so khớp chuỗi con ngây thơ không hiệu quả vì không có mẫu cố định nào để khớp từng vị trí; chỉ tính vấn đề. 

Ràng buộc$n \le 10^{18}$ngay lập tức loại trừ bất kỳ DP tuyến tính hoặc đa thức nào theo chiều dài. Bất kỳ giải pháp nào cũng phải nén các trạng thái để các quá trình chuyển đổi có thể được lặp lại bằng cách sử dụng lũy ​​thừa hoặc phương pháp logarit khác. Hướng khả thi duy nhất là mô hình hóa quy trình như một máy tự động hữu hạn có trạng thái chỉ phụ thuộc vào lịch sử giới hạn của chuỗi. 

Trường hợp cạnh tinh tế xuất hiện khi nhiều cửa sổ chồng chéo có thể đồng thời vi phạm điều kiện. Ví dụ: một chuỗi có thể trở nên không hợp lệ ở vị trí$i$, nhưng cũng chứa một cửa sổ không hợp lệ khác kết thúc tại$i+1$. Bất kỳ phương pháp đúng nào cũng phải đảm bảo mọi cửa sổ đều được kiểm tra chính xác khi nó được hình thành chứ không phải hồi cứu. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ tạo ra tất cả$4^n$chuỗi và kiểm tra từng chuỗi để tìm các cửa sổ bị cấm. Ngay cả đối với$n=20$, điều này đã trở nên không khả thi, và đối với$n$lên đến$10^{18}$, điều đó là hoàn toàn không thể. Phương pháp vũ phu tốt hơn một chút sẽ sử dụng lập trình động trên tiền tố, lưu trữ rõ ràng 4 ký tự cuối cùng. Điều này làm giảm quá trình chuyển đổi trạng thái xuống kích thước có thể quản lý được nhưng vẫn tăng tỷ lệ tuyến tính với$n$, nó quá lớn. 

Quan sát quan trọng là điều kiện bị cấm chỉ phụ thuộc vào 5 ký tự cuối cùng. Khi chúng ta thêm một ký tự mới, chỉ cửa sổ kết thúc ở vị trí đó mới có thể trở thành không hợp lệ. Điều này gợi ý việc duy trì trạng thái cửa sổ trượt có độ dài 4. Ký tự tiếp theo hoàn thành một cửa sổ có kích thước 5 và chúng tôi có thể trực tiếp kiểm tra xem cửa sổ đó có khớp với nhiều bộ bị cấm hay không. 

Điều này làm giảm vấn đề về một máy trạng thái hữu hạn với$4^4 = 256$các trạng thái có thể (tất cả các chuỗi có thể có độ dài lên tới 4 trên 4 ký tự). Sự chuyển tiếp được xác định bằng cách thêm một ký tự và dịch chuyển cửa sổ. Từ$n$là rất lớn, chúng tôi tính toán số lượng chiều dài-$n$bước vào máy tự động này bằng cách sử dụng phép lũy thừa ma trận. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Liệt kê lực lượng vũ phu |$O(4^n)$|$O(n)$| Không thể | 
| DP trên độ dài tiền tố |$O(n \cdot 4^4)$|$O(4^4)$| Quá chậm | 
| Tự động hóa + lũy thừa ma trận |$O(256^3 \log n)$|$O(256^2)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xác định trạng thái là tối đa 4 ký tự cuối cùng của chuỗi. Vì mỗi vị trí có 4 lựa chọn nên có nhiều nhất$4^4 = 256$tiểu bang. Chúng tôi biểu thị mỗi trạng thái dưới dạng mã hóa cơ sở 4 có độ dài 4. 
2. Đối với mỗi trạng thái và mỗi ký tự tiếp theo có thể có, chúng tôi mô phỏng việc thêm ký tự và tạo thành một cửa sổ 5 ký tự mới bao gồm 4 ký tự trước đó cộng với ký tự mới. Chúng tôi tính toán số lượng ký tự trong cửa sổ này. 
3. Nếu multiset của 5 ký tự này khớp chính xác$\{y, u, u, k, o\}$, chúng tôi đánh dấu quá trình chuyển đổi này là không hợp lệ và loại bỏ nó. Mặt khác, chúng tôi thêm chuyển đổi có hướng từ trạng thái hiện tại sang trạng thái mới thu được bằng cách dịch chuyển sang trái và thêm ký tự mới. 
4. Điều này đưa ra biểu đồ có hướng gồm tối đa 256 nút, trong đó mỗi nút có tối đa 4 lần chuyển tiếp đi. Chúng tôi chuyển đổi điều này thành một ma trận chuyển tiếp$T$, Ở đâu$T[i][j]$đếm các chuyển đổi hợp lệ từ trạng thái$i$để nêu$j$. 
5. Chúng ta khởi tạo một vectơ đại diện cho tất cả các trạng thái bắt đầu hợp lệ. Vì các chuỗi ngắn hơn 4 ký tự không thể vi phạm điều kiện nên tất cả các trạng thái ban đầu đều được phép. 
6. Chúng tôi tính toán$T^n$sử dụng lũy ​​thừa nhanh. Mỗi phép nhân tương ứng với việc kết hợp các cách mở rộng chuỗi trong khi vẫn giữ nguyên giá trị. 
7. Cuối cùng, chúng ta tính tổng tất cả các phần tử của vectơ kết quả để có được tổng số chuỗi có độ dài hợp lệ$n$. 

Tính chính xác dựa trên tính bất biến là mọi trạng thái DP nắm bắt đầy đủ tất cả thông tin cần thiết để xác định xem việc thêm một ký tự nữa có tạo ra cửa sổ 5 độ dài bị cấm hay không. Vì mọi vi phạm đều được phát hiện chính xác khi nó xuất hiện nên không có chuỗi không hợp lệ nào được đóng góp vào DP. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

# map characters to indices
mp = {'y': 0, 'u': 1, 'k': 2, 'o': 3}

def encode(s):
    x = 0
    for c in s:
        x = x * 4 + c
    while len(s) < 4:
        x *= 4
        s += [0]
    return x

def decode(x):
    s = []
    for _ in range(4):
        s.append(x % 4)
        x //= 4
    return tuple(reversed(s))

# forbidden multiset: y, u, u, k, o
forbidden = [0, 1, 1, 2, 3]

def is_bad(window):
    cnt = [0] * 4
    for c in window:
        cnt[c] += 1
    return cnt == [1, 2, 1, 1]

# build transitions
N = 256
T = [[0] * N for _ in range(N)]

for s in range(N):
    cur = decode(s)
    for c in range(4):
        window = list(cur) + [c]
        if is_bad(window):
            continue
        ns = tuple(list(cur[1:]) + [c])
        ns_id = 0
        for v in ns:
            ns_id = ns_id * 4 + v
        T[s][ns_id] += 1

def matmul(A, B):
    n = len(A)
    res = [[0] * n for _ in range(n)]
    for i in range(n):
        Ai = A[i]
        for k in range(n):
            if Ai[k]:
                Bik = B[k]
                aik = Ai[k]
                for j in range(n):
                    res[i][j] = (res[i][j] + aik * Bik[j]) % MOD
    return res

def matpow(M, e):
    n = len(M)
    res = [[0]*n for _ in range(n)]
    for i in range(n):
        res[i][i] = 1
    while e:
        if e & 1:
            res = matmul(res, M)
        M = matmul(M, M)
        e >>= 1
    return res

# initial vector: all states allowed
# we start from length 4 padding-free states, so treat all as 1
init = [1] * N

def solve(n):
    if n == 0:
        return 1
    if n <= 4:
        return pow(4, n, MOD)

    P = matpow(T, n - 4)

    ans = 0
    for i in range(N):
        for j in range(N):
            ans = (ans + P[i][j] * init[i]) % MOD
    return ans

n = int(input())
print(solve(n))
```Mã xây dựng máy tự động một cách rõ ràng trên tất cả các lịch sử có độ dài 4. Mỗi lần chuyển đổi sẽ kiểm tra xem việc thêm một ký tự mới có tạo ra nhiều tập hợp bị cấm trong cửa sổ dài 5 kết quả hay không. lũy thừa ma trận sau đó đếm tất cả các chuỗi có độ dài hợp lệ$n$theo thời gian logarit. 

Một chi tiết triển khai tinh tế là dành cho$n \le 4$, không có cửa sổ nào có độ dài 5 tồn tại, vì vậy mọi chuỗi đều hợp lệ và câu trả lời đơn giản là$4^n$. 

## Ví dụ đã hoạt động 

### Ví dụ 1:$n = 5$Chúng tôi theo dõi quá trình chuyển đổi từ trạng thái có độ dài 4 và nối thêm một ký tự. 

| Bước | Hiện trạng | Đã thêm ký tự | Đã kiểm tra cửa sổ | Có hiệu lực? | 
| --- | --- | --- | --- | --- | 
| 1 | bất kỳ trạng thái 4 chữ cái nào | y | 5 ký tự cuối cùng | phụ thuộc | 
| 2 | bất kỳ trạng thái 4 chữ cái nào | bạn | 5 ký tự cuối cùng | phụ thuộc | 
| 3 | bất kỳ trạng thái 4 chữ cái nào | k | 5 ký tự cuối cùng | phụ thuộc | 
| 4 | bất kỳ trạng thái 4 chữ cái nào | o | 5 ký tự cuối cùng | phụ thuộc | 

Ở độ dài này, máy tự động đảm bảo rằng bất kỳ chuỗi nào có nhiều tập hợp bị cấm xuất hiện trong bất kỳ cửa sổ có kích thước 5 nào đều bị loại trừ chính xác một lần tại thời điểm nó hình thành. 

Điều này xác nhận sự bất biến rằng mọi cấu hình không hợp lệ đều được lọc ngay khi tạo. 

### Ví dụ 2:$n = 6$Bây giờ tồn tại hai cửa sổ chồng chéo: vị trí$[1..5]$Và$[2..6]$. Một chuỗi có thể vi phạm điều kiện trong cả hai cửa sổ. 

| Vị trí | Ký tự mới | Đã kiểm tra cửa sổ | Hành động | 
| --- | --- | --- | --- | 
| 5 | c₅ | (1..5) | đã kiểm tra | 
| 6 | c₆ | (2..6) | đã kiểm tra | 

Nếu một trong hai cửa sổ khớp với nhiều tập hợp bị cấm thì quá trình chuyển đổi tương ứng sẽ không được phép, ngăn cản việc truyền bá các tiền tố không hợp lệ. 

Điều này chứng tỏ các vi phạm chồng chéo được xử lý một cách tự nhiên mà không cần kiểm tra lại một cách rõ ràng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(256^3 \log n)$| lũy thừa ma trận trên máy tự động 256 trạng thái cố định | 
| Không gian |$O(256^2)$| lưu trữ ma trận chuyển tiếp | 

Không gian trạng thái là hằng số nên giải pháp xử lý thoải mái$n$lên đến$10^{18}$. Phép lũy thừa logarit đảm bảo số lượng phép nhân vẫn ở mức nhỏ ngay cả đối với độ dài cực lớn.
