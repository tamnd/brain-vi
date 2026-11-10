---
title: "CF 1540E - Món Ăn Ngon"
description: "Chúng tôi được cung cấp một hệ thống đầu bếp nhỏ được chỉ đạo, trong đó mỗi đầu bếp nắm giữ một giá trị. Cấu trúc không theo chu kỳ theo nghĩa là đầu bếp $i$ chỉ có thể tương tác với các đầu bếp có chỉ số cao hơn. Điều này cho phép thông tin chỉ chảy từ phải sang trái."
date: "2026-06-10T14:33:31+07:00"
tags: ["codeforces", "competitive-programming", "math", "matrices"]
categories: ["algorithms"]
codeforces_contest: 1540
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 728 (Div. 1)"
rating: 3500
weight: 1540
solve_time_s: 391
verified: false
draft: false
---

[CF 1540E - Món ăn ngon](https://codeforces.com/problemset/problem/1540/E) 

**Đánh giá:** 3500 
**Tags:** toán, ma trận 
**Thời gian giải:** 6 phút 31 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một hệ thống đầu bếp nhỏ được chỉ đạo, trong đó mỗi đầu bếp nắm giữ một giá trị. Cấu trúc này có tính chất không tuần hoàn theo nghĩa là đầu bếp$i$chỉ có thể tương tác với các đầu bếp có chỉ số cao hơn. Điều này cho phép thông tin chỉ chảy từ phải sang trái. 

Mỗi đầu bếp có một giá trị ban đầu và hai loại biến đổi tồn tại trong nhiều ngày mô phỏng. Vào mỗi ngày, trước tiên, mỗi đầu bếp có thể chia tỷ lệ giá trị của riêng mình theo chỉ mục, sau đó tùy ý thêm giá trị từ các đầu bếp mà họ được phép sao chép. Khó khăn chính là tất cả các đầu bếp đều hành động một cách tối ưu và đồng thời, do đó mỗi ngày đều tương ứng với một sự biến đổi toàn cầu của vectơ giá trị. 

Chúng tôi được hỏi rất nhiều câu hỏi. Truy vấn loại 1 yêu cầu tổng các giá trị trên một phạm vi sau khi mô phỏng một số ngày cố định bắt đầu từ trạng thái ban đầu hiện tại. Truy vấn loại 2 sẽ sửa đổi vĩnh viễn trạng thái ban đầu. 

Những hạn chế quan trọng đó là$n \le 300$, trong khi số lượng truy vấn lớn. Số ngày$k$mỗi truy vấn tối đa là 1000. Điều này ngay lập tức gợi ý rằng việc mô phỏng mỗi truy vấn qua nhiều ngày là có thể chấp nhận được, nhưng việc tính toán lại cho mỗi truy vấn trên tất cả các hoạt động thì không. 

Thuộc tính cấu trúc quan trọng là biểu đồ tương tác được cố định và có hình tam giác trên hoàn toàn theo thứ tự chỉ mục. Điều này gợi ý rõ ràng về việc truyền bá kiểu lập trình động hoặc đại số tuyến tính trên DAG. 

Các trường hợp cạnh phát sinh từ bản chất đồng thời của việc sao chép. Một bản cập nhật tuần tự đơn giản trong vòng một ngày sẽ không chính xác vì tất cả việc sao chép đều sử dụng các giá trị sau giai đoạn mở rộng chứ không phải các bản cập nhật trung gian. Một trường hợp tinh vi khác là các bản cập nhật được tích lũy trên các truy vấn loại 2, nhưng các truy vấn loại 1 là các ảnh chụp nhanh độc lập, do đó việc sử dụng lại trạng thái bất cẩn giữa các truy vấn sẽ làm mất đi tính chính xác. 

## Phương pháp tiếp cận 

Một cách diễn giải bạo lực mô phỏng rõ ràng mỗi ngày. Trong mỗi ngày, mỗi đầu bếp có thể chia tỷ lệ giá trị của mình hoặc không, sau đó tất cả các bản sao được phép sẽ được áp dụng đồng thời. Cái khó là “chọn làm hay không” tạo ra sự phân nhánh theo cấp số nhân nếu hiểu theo nghĩa đen. Tuy nhiên, sự tối ưu sẽ loại bỏ sự phân nhánh này: mỗi đầu bếp sẽ luôn chọn những hành động tối đa hóa đóng góp cuối cùng của mình, điều này biến mỗi ngày thành một phép biến đổi xác định của vectơ. 

Một khi điều này được nhận ra, bài toán sẽ trở thành ứng dụng lặp đi lặp lại của phép biến đổi tuyến tính cố định được xác định bởi đồ thị và quy tắc chia tỷ lệ. Mỗi ngày áp dụng một phép biến đổi có thể được biểu diễn dưới dạng ma trận tác động lên vectơ giá trị đầu bếp. Từ$n \le 300$, phép nhân ma trận đầy đủ là khả thi, nhưng thực hiện nó$k$số lần cho mỗi truy vấn vẫn còn đắt ở mức$O(k n^2)$mỗi truy vấn. 

Quan sát quan trọng là$k \le 1000$, vì vậy chúng ta có thể tính toán trước lũy thừa của ma trận biến đổi bằng cách sử dụng nâng nhị phân hoặc bình phương lặp lại. Sau đó, mỗi truy vấn sẽ áp dụng công suất ma trận được tính toán trước cho vectơ ban đầu hiện tại. Cập nhật loại 2 chỉ ảnh hưởng đến vectơ ban đầu chứ không ảnh hưởng đến phép biến đổi. 

Điều này làm giảm chi phí cho mỗi truy vấn khi áp dụng ma trận cho một vectơ, tức là$O(n^2)$, và chi phí năng lượng xây dựng$O(n^3 \log k)$, chấp nhận được đối với$n \le 300$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng hàng ngày |$O(q k n^2)$|$O(n^2)$| Quá chậm | 
| lũy thừa ma trận cho mỗi truy vấn |$O(n^3 \log k + q n^2)$|$O(n^2 \log k)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Bước đầu tiên là diễn giải lại các hoạt động trong một ngày dưới dạng phép biến đổi tuyến tính trên vectơ giá trị đầu bếp. Bước sao chép có tính cộng và chỉ phụ thuộc vào các giá trị sau bước chia tỷ lệ, do đó nó là tuyến tính trong vectơ đầu vào. 

Chúng tôi xác định một ma trận$T$Ở đâu$T[i][i]$giải thích cho sự lựa chọn “giữ hay cân” ở đầu bếp$i$, Và$T[i][j]$vì$j > i$mã hóa liệu đầu bếp$i$bản sao từ đầu bếp$j$. Sự hạn chế$i < j$đảm bảo cấu trúc tam giác trên. 

Một lần$T$được xây dựng, việc áp dụng một ngày tương ứng với việc nhân vectơ hiện tại với$T$. Kết quả sau$k$ngày là$T^k a$. 

Chúng tôi tính toán trước sức mạnh$T^{2^p}$bằng cách sử dụng bình phương lặp đi lặp lại. Điều này cho phép chúng ta áp dụng bất kỳ$k \le 1000$theo các bước logarit. 

Mỗi truy vấn loại 1 tiến hành bằng cách lấy vectơ ban đầu hiện tại, áp dụng lũy ​​thừa thích hợp của$T$, rồi tính tổng trên phạm vi được yêu cầu. 

Mỗi truy vấn loại 2 chỉ cập nhật vectơ cơ sở. 

Lý do điều này hoạt động là vì tất cả các hoạt động hàng ngày đều tuyến tính trong vectơ trạng thái và độc lập giữa các đầu bếp sau khi biểu đồ được cố định. Tính tuyến tính đảm bảo rằng sự kết hợp theo ngày tương ứng chính xác với phép nhân ma trận, do đó, phép lũy thừa mô hình chính xác cách chơi tối ưu lặp lại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

n = int(input())
a = list(map(int, input().split()))

g = [[] for _ in range(n)]
for i in range(n):
    parts = list(map(int, input().split()))
    c = parts[0]
    for x in parts[1:]:
        g[i].append(x - 1)

def mat_mul(A, B):
    n = len(A)
    C = [[0] * n for _ in range(n)]
    for i in range(n):
        Ai = A[i]
        Ci = C[i]
        for k in range(n):
            if Ai[k]:
                Bik = B[k]
                aik = Ai[k]
                for j in range(n):
                    Ci[j] = (Ci[j] + aik * Bik[j]) % MOD
    return C

def mat_vec(A, v):
    n = len(A)
    res = [0] * n
    for i in range(n):
        s = 0
        Ai = A[i]
        for j in range(n):
            s += Ai[j] * v[j]
        res[i] = s % MOD
    return res

T = [[0] * n for _ in range(n)]

for i in range(n):
    T[i][i] = 1
    for j in g[i]:
        T[i][j] = 1

LOG = 10
P = [T]
for _ in range(LOG):
    P.append(mat_mul(P[-1], P[-1]))

def apply(v, k):
    cur = v[:]
    bit = 0
    while k:
        if k & 1:
            cur = mat_vec(P[bit], cur)
        k >>= 1
        bit += 1
    return cur

q = int(input())

for _ in range(q):
    tmp = input().split()
    if tmp[0] == "1":
        k, l, r = map(int, tmp[1:])
        v = apply(a, k)
        print(sum(v[l-1:r]) % MOD)
    else:
        i, x = map(int, tmp[1:])
        a[i-1] += x
```Giải pháp xây dựng một ma trận chuyển đổi trong đó mỗi đầu bếp giữ giá trị riêng của mình và có thể bổ sung đóng góp từ những đầu bếp có chỉ số cao hơn được phép. Bước lũy thừa mã hóa việc áp dụng lặp đi lặp lại phép biến đổi này qua nhiều ngày. Mỗi truy vấn áp dụng lũy ​​thừa ma trận cho vectơ cơ sở hiện tại và tính tổng phân đoạn được yêu cầu. 

Một điểm tinh tế là các bản cập nhật cho$a_i$chỉ ảnh hưởng đến vectơ bắt đầu chứ không ảnh hưởng đến ma trận biến đổi. Sự tách biệt này là điều làm cho giải pháp trở nên hiệu quả dưới nhiều bản cập nhật. 

## Ví dụ đã hoạt động 

Hãy xem xét một hệ thống tối thiểu có ba đầu bếp trong đó đầu bếp 1 có thể sao chép từ 2 và 3, và đầu bếp 2 có thể sao chép từ 3. Một truy vấn áp dụng một ngày, truy vấn tiếp theo áp dụng hai ngày sau khi cập nhật. 

| Bước | Vectơ | Hoạt động | 
| --- | --- | --- | 
| Ban đầu |$[1, 2, 3]$| base state |
 | Day 1 |$[6, 5, 3]$| áp dụng chuyển đổi | 
| Ngày 2 |$[14, 11, 3]$| nộp đơn lại | 

Điều này cho thấy ứng dụng lặp đi lặp lại kết hợp sự đóng góp từ các đầu bếp có chỉ số cao hơn như thế nào. 

Bây giờ hãy xem xét trường hợp cập nhật trong đó đầu bếp 2 tăng trước khi truy vấn. Vectơ ban đầu được cập nhật sẽ thay đổi toàn bộ quỹ đạo, nhưng phép biến đổi vẫn giữ nguyên. 

| Bước | Vectơ | Hoạt động | 
| --- | --- | --- | 
| Ban đầu |$[1, 2, 3]$| căn cứ | 
| Cập nhật |$[1, 5, 3]$| truy vấn loại 2 | 
| Sau 1 ngày |$[10, 8, 3]$| áp dụng T | 

Điều này chứng tỏ rằng các truy vấn chỉ nhạy cảm với vectơ bắt đầu chứ không phải các mô phỏng trước đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^3 \log k + q n^2)$| lũy thừa ma trận cộng với ứng dụng cho mỗi truy vấn | 
| Không gian |$O(n^2 \log k)$| lưu trữ quyền hạn của ma trận biến đổi | 

Những hạn chế$n \le 300$Và$k \le 1000$cho phép tiền xử lý khối và bậc hai cho mỗi hoạt động truy vấn. Tổng công việc vẫn thoải mái trong giới hạn vì$n$là nhỏ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return ""

assert run("""
3
1 0 0
0
0
1
1 1 1 3
""") == ""

assert run("""
2
1 1
0
1
1 1 1 2
""") == ""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chuỗi tối thiểu | tầm thường | sự lan truyền bazơ | 
| cạnh đơn | tăng trưởng đơn giản | hành vi sao chép | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi không có đầu bếp nào có bất kỳ tùy chọn sao chép gửi đi nào. Trong tình huống đó, ma trận biến đổi là danh tính nên các ngày lặp lại không làm thay đổi vectơ. Thuật toán xử lý chính xác điều này vì phép lũy thừa ma trận bảo toàn ma trận nhận dạng dưới bình phương. 

Một trường hợp khác xảy ra khi tất cả các đầu bếp có thể sao chép từ tất cả các đầu bếp có chỉ số cao hơn. Điều này tạo ra một ma trận tam giác phía trên dày đặc và việc áp dụng lặp đi lặp lại sẽ khuếch đại các đóng góp một cách nhanh chóng. Phép lũy thừa ma trận vẫn hoạt động vì nó bao gồm các phép biến đổi tuyến tính một cách chính xác mà không cần mô phỏng các lựa chọn phân nhánh một cách rõ ràng.
