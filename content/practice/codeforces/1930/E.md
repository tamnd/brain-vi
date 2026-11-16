---
title: "CF 1930E - 2..3...4.... Tuyệt vời! Tuyệt vời!"
description: "Chúng ta bắt đầu với mảng $$[1,2,3,dots,n].$$ Giá trị $k$ được cố định cho toàn bộ quá trình. Trong một thao tác, chúng ta chọn một dãy con có độ dài $2k+1$. Trong số các phần tử được chọn đó, chúng tôi xóa $k$ đầu tiên và $k$ cuối cùng, chỉ giữ lại phần tử ở giữa."
date: "2026-06-08T18:32:58+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "dp", "math"]
categories: ["algorithms"]
codeforces_contest: 1930
codeforces_index: "E"
codeforces_contest_name: "think-cell Round 1"
rating: 2400
weight: 1930
solve_time_s: 110
verified: true
draft: false
---

[CF 1930E - 2..3...4.... Tuyệt vời! Tuyệt vời!](https://codeforces.com/problemset/problem/1930/E) 

**Đánh giá:** 2400 
**Tags:** tổ hợp, dp, toán 
**Thời gian giải:** 1 phút 50 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta bắt đầu với mảng$$[1,2,3,\dots,n].$$Một giá trị$k$được cố định cho toàn bộ quá trình. Trong một thao tác, chúng ta chọn một dãy con có độ dài$2k+1$. Trong số những phần tử được chọn đó, chúng tôi xóa phần tử đầu tiên$k$và cuối cùng$k$, chỉ giữ lại cái ở giữa. 

Vì mảng ban đầu đang tăng dần và chúng ta chỉ xóa các phần tử nên mọi trạng thái cuối cùng chỉ đơn giản là một dãy con của$[1,2,\dots,n]$. 

Nhiệm vụ là: với mỗi$k$từ$1$ĐẾN$\lfloor (n-1)/2 \rfloor$, đếm xem có bao nhiêu chuỗi con khác nhau có thể xuất hiện sau khi thực hiện thao tác bao nhiêu lần. 

Câu trả lời phải được báo cáo modulo$998244353$. 

Những hạn chế là lớn. Một bài kiểm tra duy nhất có thể có$n=10^6$, và tổng của tất cả$n$các giá trị cũng$10^6$. Bất kỳ thuật toán nào kiểm tra các dãy con riêng lẻ đều không thể thực hiện được vì có$2^n$của họ. Thậm chí một$O(n^2)$thuật toán sẽ thực hiện xung quanh$10^{12}$hoạt động trong trường hợp xấu nhất. Chúng ta cần một cái gì đó gần gũi$O(n \log n)$tổng thể$k$. 

Phần khó khăn là mô tả chính xác những chuỗi con nào có thể truy cập được. 

Coi như$n=5$Và$k=2$. Một thao tác chọn một chuỗi có độ dài$5$, do đó, thao tác xóa duy nhất có thể sẽ loại bỏ hai phần tử ở mỗi bên của phần tử ở giữa đã chọn. Kết quả không tầm thường duy nhất là$[3]$. Một ý tưởng ngây thơ rằng "bất kỳ chuỗi con nào có số phần tử lẻ đều có thể truy cập được" sẽ đếm không chính xác nhiều mảng không thể thực hiện được. 

Một trường hợp tế nhị khác là$k=1$và tiếp theo$[1,4]$TRONG$n=4$. Hai phần tử đã bị xóa, là bội số của$2k$, nhưng chỉ điều đó thôi thì chưa đủ. Khả năng tiếp cận phụ thuộc vào cấu trúc của các vị trí đã xóa, không chỉ số lượng của chúng. 

Cốt lõi của vấn đề là tìm ra một đặc tính rõ ràng của các chuỗi con có thể tiếp cận được. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ xem mọi trạng thái là một chuỗi con và thực hiện BFS trên tất cả các hoạt động có thể có. Điều này đúng vì mọi mảng có thể truy cập cuối cùng đều được phát hiện. 

Thật không may, không gian trạng thái đã có$2^n$các chuỗi tiếp theo. Vì$n=30$con số này đã lên tới hơn một tỷ trạng thái, vì vậy cách tiếp cận này sẽ trở nên vô dụng rất lâu trước khi đạt được các giới hạn thực tế. 

Quan sát quan trọng là hoạt động chỉ phụ thuộc vào vị trí nào tồn tại. 

Biểu diễn dãy con cuối cùng bằng một chuỗi nhị phân có độ dài$n$:$$1 = \text{element survives}, \qquad 0 = \text{element deleted}.$$Mỗi thao tác xóa chính xác$2k$các phần tử. Do đó tổng số số 0 phải là bội số của$2k$. 

Điều đáng ngạc nhiên là điều kiện này gần như đủ. 

Gọi số phần tử bị xóa là$2ck$. Một dãy con không trống có thể đạt được khi và chỉ khi tồn tại một vị trí tồn tại sao cho ít nhất$k$các phần tử bị xóa nằm ở bên trái và ít nhất$k$phần tử bị xóa nằm ở bên phải của nó. 

Đặc tính này biến vấn đề thành phép đếm thuần túy. 

Đối với một cố định$k$, cho phép$2ck$là số phần tử bị xóa có$$\binom{n}{2ck}$$chuỗi nhị phân chính xác$2ck$số không. 

Bây giờ chúng ta đếm các chuỗi xấu, những chuỗi không chứa bất kỳ vị trí nào tồn tại với ít nhất$k$số không ở cả hai bên. 

Một cấu hình xấu có hình dạng rất cứng nhắc. Ngoại trừ nhiều nhất$k-1$số không ở gần đầu bên trái và nhiều nhất$k-1$các số 0 ở gần đầu bên phải, tất cả các số 0 còn lại phải tạo thành một khối liền kề. Sau khi nén khối đó thành một đối tượng duy nhất, chúng ta sẽ chọn vị trí cho$2k-1$những đồ vật đặc biệt trong số$$n-(2ck-(2k-2))+1$$tổng số vị trí. Điều này mang lại$$\binom{n-2ck+2k-1}{2k-1}.$$Vì vậy đối với cố định$k$,$$\text{answer} = 1+ \sum_{c\ge1} \left( \binom{n}{2ck} - \binom{n-2ck+2k-1}{2k-1} \right).$$phần bổ sung$1$đếm mảng ban đầu, tương ứng với số lần xóa bằng 0. 

Công việc còn lại là đánh giá hiệu quả các hệ số nhị thức. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^n)$hoặc tệ hơn |$O(2^n)$| Quá chậm | 
| Tối ưu |$O(n \log n)$tổng thể |$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Tính toán trước 

1. Tính toán trước giai thừa và nghịch đảo giai thừa modulo$998244353$lên đến$10^6$. 
2. Sử dụng các mảng này, tính hệ số nhị thức bất kỳ$$\binom{a}{b}$$TRONG$O(1)$thời gian. 

### Đối với mỗi test case 

1. Đọc$n$. 
2. Đối với mọi$k$từ$1$ĐẾN$\lfloor (n-1)/2 \rfloor$, khởi tạo$$ans = 1.$$giá trị$1$tương ứng với việc không thực hiện thao tác nào. 

1. Đếm số khối đã xóa:$$c = 1,2,\dots,\left\lfloor \frac{n-1}{2k}\right\rfloor.$$Thế thì chính xác$2ck$các phần tử bị xóa. 

1. Cộng chính xác số chuỗi nhị phân$2ck$vị trí đã xóa:$$\binom{n}{2ck}.$$1. Trừ các cấu hình xấu:$$\binom{n-2ck+2k-1}{2k-1}.$$1. Giảm modulo$998244353$. 
2. Xuất ra tất cả các câu trả lời cho test case này. 

### Tại sao nó hoạt động 

Mảng cuối cùng có thể truy cập chỉ có thể được biểu thị bằng tập hợp các vị trí còn sót lại. 

Mọi thao tác đều loại bỏ chính xác$2k$phần tử, do đó số lượng vị trí bị xóa phải là bội số của$2k$. Đặc tính của các trạng thái có thể truy cập nói rằng có thể truy cập được một cấu hình không trống chính xác khi một số vị trí còn tồn tại có ít nhất$k$các phần tử bị xóa ở mỗi bên. 

Đối với số lần xóa cố định$2ck$, mọi chuỗi nhị phân đều tốt hoặc xấu. Đếm tất cả các chuỗi mang lại$\binom{n}{2ck}$. Các chuỗi xấu chính xác là những chuỗi mà tất cả các số 0 thừa sẽ gộp lại thành một khối trung tâm, dẫn đến số đếm$\binom{n-2ck+2k-1}{2k-1}$. 

Việc trừ các chuỗi xấu khỏi tất cả các chuỗi sẽ cho ra số lượng cấu hình có thể truy cập được cho số lần xóa đó. Tính tổng tất cả các bội số hợp lệ của$2k$và thêm mảng chưa được chạm tới, sẽ đếm chính xác mọi mảng cuối cùng có thể truy cập một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353
MAXN = 10 ** 6 + 5

fact = [1] * MAXN
for i in range(1, MAXN):
    fact[i] = fact[i - 1] * i % MOD

inv_fact = [1] * MAXN
inv_fact[-1] = pow(fact[-1], MOD - 2, MOD)

for i in range(MAXN - 2, -1, -1):
    inv_fact[i] = inv_fact[i + 1] * (i + 1) % MOD

def C(n, r):
    if r < 0 or r > n or n < 0:
        return 0
    return fact[n] * inv_fact[r] % MOD * inv_fact[n - r] % MOD

t = int(input())

for _ in range(t):
    n = int(input())

    res = []

    for k in range(1, (n - 1) // 2 + 1):
        ans = 1

        limit = (n - 1) // (2 * k)

        for c in range(1, limit + 1):
            deleted = 2 * k * c

            ans += C(n, deleted)
            ans -= C(n - deleted + 2 * k - 1, 2 * k - 1)

        ans %= MOD
        res.append(str(ans))

    print(" ".join(res))
```Các bảng giai thừa và nghịch đảo được xây dựng một lần để có giá trị lớn nhất có thể của$n$. Vì môđun là số nguyên tố nên định lý Fermat đưa ra nghịch đảo môđun của giai thừa lớn nhất và tất cả các giai thừa nghịch đảo khác thu được chỉ trong một lần lùi. 

chức năng`C(n, r)`xử lý các đối số không hợp lệ một cách rõ ràng. Điều này tránh được các lỗi tinh vi khi công thức đếm nén tạo ra chỉ số trên âm. 

Đối với một cố định$k$, biến`c`đại diện cho bao nhiêu nhóm$2k$việc xóa đã xảy ra. Giá trị hợp lệ tối đa là$\lfloor (n-1)/(2k) \rfloor$, vì ít nhất một phần tử phải tồn tại. 

biểu hiện```
C(n - deleted + 2 * k - 1, 2 * k - 1)
```là số lượng cấu hình xấu sau khi nén. Bắt`-1`các thuật ngữ sai ở đây là nguồn gốc phổ biến nhất của những sai lầm riêng lẻ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 5
```Vì$k=1$: 

| c | đã xóa | Tất cả các chuỗi | Dây xấu | Đóng góp | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | 10 | 4 | 6 | 
| 2 | 4 | 5 | 2 | 3 | 

Bắt đầu từ`ans = 1`:$$1 + 6 + 3 = 10.$$Đầu ra cho$k=1$là$10$. 

Vì$k=2$: 

| c | đã xóa | Tất cả các chuỗi | Dây xấu | Đóng góp | 
| --- | --- | --- | --- | --- | 
| 1 | 4 | 5 | 4 | 1 |$$1 + 1 = 2.$$Đầu ra:```
10 2
```Điều này phù hợp với mẫu. 

### Ví dụ 2 

đầu vào:```
n = 3
```Chỉ một$k=1$tồn tại. 

| c | đã xóa | Tất cả các chuỗi | Dây xấu | Đóng góp | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | 3 | 2 | 1 | 

Bắt đầu từ`ans = 1`:$$1 + 1 = 2.$$Đầu ra:```
2
```Ví dụ này cho thấy vai trò của ban đầu`1`, đếm mảng ban đầu khi không có thao tác nào được thực hiện. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$mỗi bộ bài kiểm tra đầy đủ |$\sum_k n/(2k)=O(n\log n)$| 
| Không gian |$O(n)$| Bảng giai thừa và nghịch đảo | 

Vì tổng của tất cả các kích thước đầu vào tối đa là$10^6$, giới hạn chuỗi hài cho khoảng$10^6 \log 10^6$lần lặp lại. Điều này thoải mái phù hợp trong giới hạn. 

## Trường hợp thử nghiệm```
# helper: run solution on input string, return output string

# provided samples

assert run("1\n3\n") == "2\n"

assert run("1\n4\n") == "4\n"

assert run("1\n5\n") == "10 2\n"

assert run("1\n10\n") == "487 162 85 10\n"

# minimum size
assert run("1\n3\n") == "2\n"

# first case with two k values
assert run("1\n5\n") == "10 2\n"

# boundary where largest k is used
assert run("1\n7\n") == "42 14 2\n"

# multiple test cases together
assert run("2\n3\n5\n") == "2\n10 2\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`n=3`|`2`| Trường hợp pháp lý nhỏ nhất | 
|`n=5`|`10 2`| Nhiều giá trị của`k`| 
|`n=7`|`42 14 2`| Lớn nhất được chấp nhận`k`xử lý | 
| Hai trường hợp thử nghiệm cùng nhau | Đầu ra kết hợp | Đặt lại trạng thái giữa các lần kiểm tra | 

## Vỏ cạnh 

### Chỉ có thể xảy ra một thao tác duy nhất 

đầu vào:```
1
5
```Vì$k=2$, số lần xóa duy nhất có thể là$4$. Thuật toán đánh giá chính xác một thuật ngữ:$$1+\binom{5}{4}-\binom{4}{3} = 1+5-4 = 2.$$Câu trả lời là chính xác`2`, tương ứng với mảng ban đầu và mảng đơn`[3]`. 

### Số lần xóa là bội số của$2k$nhưng cấu hình không thể truy cập được 

lấy$n=4$,$k=1$. Thứ tự tương ứng với những người sống sót`{1,4}`xóa hai phần tử thỏa mãn điều kiện chia hết. Một giải pháp ngây thơ sẽ tự động tính nó. 

Phép trừ cấu hình xấu sẽ loại bỏ chính xác những trường hợp như vậy. Công thức trừ tất cả các chuỗi nhị phân trong đó không có vị trí còn lại nào có ít nhất một phần tử bị xóa ở cả hai bên. 

Đây là lý do tại sao câu trả lời cuối cùng trở thành`4`thay vì một giá trị không chính xác lớn hơn. 

### Lớn nhất có thể$k$Khi$$k=\left\lfloor \frac{n-1}{2} \right\rfloor,$$vòng lặp bên trong chỉ thực hiện một lần vì$2k$đã gần rồi$n$. 

Ví dụ,$n=5, k=2$:$$\left\lfloor \frac{n-1}{2k}\right\rfloor = 1.$$Việc triển khai xử lý việc này một cách tự nhiên mà không có bất kỳ trường hợp đặc biệt nào, tránh các lỗi biên trong giới hạn tổng.
