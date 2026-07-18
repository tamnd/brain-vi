---
title: "CF 103462A - Hoán vị mảng"
description: "Chúng tôi đang xây dựng một mảng có độ dài $n$ bằng cách sử dụng một quy trình phát triển nó thành từng khối. Ban đầu mảng trống. Tại bất kỳ thời điểm nào, nếu độ dài hiện tại là $m$, chúng ta chọn một số $x$ trong khoảng từ $1$ đến $n-m$. Sau khi chọn $x$, chúng ta nối chuỗi $1, 2, 3, dots, x$ vào mảng."
date: "2026-07-03T07:00:15+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103462
codeforces_index: "A"
codeforces_contest_name: "The Hangzhou Normal U Qualification Trials for ZJPSC 2021"
rating: 0
weight: 103462
solve_time_s: 44
verified: true
draft: false
---

[CF 103462A - Hoán vị mảng](https://codeforces.com/problemset/problem/103462/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 44s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang xây dựng một mảng có chiều dài$n$sử dụng một quy trình phát triển nó theo từng khối. Ban đầu mảng trống. Tại bất kỳ thời điểm nào, nếu độ dài hiện tại là$m$, ta chọn một số$x$giữa$1$Và$n-m$. Sau khi chọn$x$, chúng tôi nối thêm chuỗi$1, 2, 3, \dots, x$vào mảng. Chúng tôi lặp lại điều này cho đến khi độ dài mảng trở nên chính xác$n$. 

Điểm mấu chốt là mọi quyết định không chỉ thêm một số mà còn thêm toàn bộ khối tăng dần bắt đầu từ 1 cho đến số đã chọn.$x$. Các chuỗi lựa chọn khác nhau có thể tạo ra các mảng cuối cùng khác nhau và nhiệm vụ là đếm xem có thể có bao nhiêu mảng cuối cùng riêng biệt cho một mảng nhất định.$n$, modulo$10^9+7$. 

Ràng buộc$n \le 10^6$và lên đến$10^3$các trường hợp thử nghiệm ngay lập tức loại trừ bất kỳ phương pháp nào cố gắng mô phỏng tất cả các cấu trúc hoặc thực hiện đệ quy hàm mũ cho mỗi trường hợp thử nghiệm. Thậm chí$O(n^2)$mỗi trường hợp thử nghiệm sẽ quá chậm trong trường hợp xấu nhất. Giải pháp phải đưa vấn đề về dạng đóng hoặc tệ nhất là tính toán trước tuyến tính đơn giản. 

Trường hợp cạnh tinh tế xuất hiện khi$n = 1$. Quá trình buộc phải có một sự lựa chọn duy nhất$x = 1$, chỉ tạo ra mảng$[1]$. Bất kỳ công thức đúng nào cũng phải tự nhiên tạo ra 1 ở đây. Một sự hiểu lầm ngây thơ là nghĩ rằng các chuỗi khác nhau có thể tạo ra các mảng khác nhau ngay cả khi tổng số khối được chọn là như nhau, nhưng ở đây mọi khối đều hoàn toàn xác định một lần$x$được chọn, do đó chỉ có trình tự của$x$-giá trị quan trọng. 

## Phương pháp tiếp cận 

Việc xây dựng có thể được xem là liên tục chọn số lượng phần tử để nối thêm cho đến khi chúng ta điền chính xác độ dài$n$. Giả sử chúng ta ghi lại các giá trị đã chọn là$x_1, x_2, \dots, x_k$. Mỗi$x_i$đóng góp chính xác$x_i$các phần tử vào mảng cuối cùng, vì vậy chúng ta phải có$$x_1 + x_2 + \cdots + x_k = n.$$Khi chuỗi này được cố định, mảng cuối cùng được xác định đầy đủ vì mỗi khối luôn là mẫu cố định$1,2,\dots,x_i$. Không có sự tự do bổ sung bên trong một khối. 

Vì vậy, vấn đề trở thành đếm có bao nhiêu dãy số nguyên dương có thứ tự tổng bằng$n$. Đây chính xác là số lượng tác phẩm của$n$. 

Một cách mạnh mẽ để thấy điều này là lập trình động trong đó$dp[i]$đếm cách để đạt được tổng$i$. Từ một tiểu bang$i$, chúng tôi thử tất cả các kích thước khối tiếp theo có thể$x \ge 1$, chuyển sang$i+x$. Điều này đúng nhưng có chi phí rõ ràng: cho mỗi$i$, chúng ta có thể lặp lại tới$n-i$, cho$O(n^2)$cho mỗi trường hợp thử nghiệm trong trường hợp xấu nhất, quá chậm để$n = 10^6$. 

Cái nhìn sâu sắc về cấu trúc là các thành phần của$n$tương đương với việc đặt dấu phân cách giữa$n$đơn vị nguyên tử. Giữa mỗi cặp phần tử liền kề trong một chuỗi có độ dài$n$, chúng ta có thể cắt hoặc không cắt. có$n-1$các khoảng trống và mỗi khoảng trống sẽ xác định một cách độc lập liệu một khối mới có bắt đầu hay không. Điều này tạo ra chính xác$2^{n-1}$khả năng. 

Điều đó làm giảm toàn bộ vấn đề về lũy thừa mô-đun. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force DP trên tổng số tiền |$O(n^2)$mỗi bài kiểm tra |$O(n)$| Quá chậm | 
| Sức mạnh của hai công thức |$O(\log n)$mỗi bài kiểm tra |$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Quan sát rằng mọi công trình đều tương ứng với một chuỗi các kích thước khối đã chọn$x_1, x_2, \dots, x_k$. Điều này là do mỗi thao tác nối thêm chính xác$x_i$các phần tử, do đó quá trình được mô tả đầy đủ bởi các giá trị này. 
2. Chuyển ràng buộc xây dựng thành phương trình$x_1 + x_2 + \cdots + x_k = n$, ở đâu mỗi$x_i \ge 1$. Điều này loại bỏ hoàn toàn cấu trúc mảng và giảm vấn đề đếm các chuỗi số nguyên dương hợp lệ. 
3. Giải thích từng giải pháp như một phân vùng của một dòng$n$vị trí thành các đoạn liên tiếp. Mỗi phân đoạn tương ứng với một khối được chọn và được cố định bên trong như$1,2,\dots,x_i$. Các giá trị bên trong không ảnh hưởng đến việc đếm. 
4. Chuyển đổi chế độ xem phân vùng thành bài toán quyết định nhị phân: giữa mọi cặp vị trí liền kề$i$Và$i+1$, quyết định cắt (bắt đầu một khối mới) hay không cắt (tiếp tục khối hiện tại). có$n-1$những quyết định như vậy. 
5. Đếm tất cả các mẫu cắt có thể. Mỗi trong số$n-1$khoảng trống có hai lựa chọn độc lập nên tổng số công trình là$2^{n-1}$. 
6. Tính toán trước lũy thừa từ hai đến$10^6$modulo$10^9+7$, sau đó trả lời từng truy vấn trong thời gian không đổi bằng cách trả về giá trị được tính toán trước. 

### Tại sao nó hoạt động 

Tính đúng đắn dựa trên sự song ánh giữa các chuỗi xây dựng hợp lệ và các tập hợp con của$n-1$ranh giới giữa các vị trí. Mỗi chuỗi kích thước khối hợp lệ xác định chính xác một cách để cắt mảng thành các phân đoạn tăng dần liền kề và mỗi lựa chọn cắt sẽ xác định duy nhất một chuỗi kích thước khối hợp lệ. Vì cả hai hướng đều xác định và không mất dữ liệu nên việc đếm giảm xuống còn đếm các tập hợp con của một tập hợp kích thước$n-1$, đó là$2^{n-1}$. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7
MAXN = 10**6

# precompute powers of 2
pow2 = [1] * (MAXN + 1)
for i in range(1, MAXN + 1):
    pow2[i] = (pow2[i - 1] * 2) % MOD

t = int(input())
out = []

for _ in range(t):
    n = int(input())
    out.append(str(pow2[n - 1]))

print("\n".join(out))
```Giải pháp này tính toán trước tất cả lũy thừa của hai một lần, giúp tránh việc lũy thừa lặp lại lên đến$10^3$trường hợp thử nghiệm. Sự tái diễn$pow2[i] = 2 \cdot pow2[i-1]$trực tiếp mã hóa tính chất nhân đôi của việc thêm một vị trí cắt tiềm năng nữa. 

Mỗi truy vấn chỉ đơn giản là ánh xạ$n$ĐẾN$2^{n-1}$, chú ý xử lý trường hợp cơ bản$n=1$, mang lại kết quả chính xác$2^0 = 1$. 

## Ví dụ đã hoạt động 

### Ví dụ 1:$n = 3$Chúng tôi tính toán tất cả các thành phần của 3. 

| Bước | Thành phần | Giải thích như vết cắt | 
| --- | --- | --- | 
| 1 | [3] | không cắt | 
| 2 | [1,2] | cắt sau 1 | 
| 3 | [2,1] | cắt sau 2 | 
| 4 | [1,1,1] | cắt sau 1 và 2 | 

Điều này cho 4 mảng, phù hợp$2^{3-1} = 4$. Bảng cho thấy mọi tập hợp con của hai ranh giới bên trong tương ứng với chính xác một cách xây dựng hợp lệ. 

### Ví dụ 2:$n = 4$| Bước | Thành phần | Giải thích như vết cắt | 
| --- | --- | --- | 
| 1 | [4] | không cắt | 
| 2 | [1,3] | cắt sau 1 | 
| 3 | [2,2] | cắt sau 2 | 
| 4 | [3,1] | cắt sau 3 | 
| 5 | [1,1,2] | cắt sau 1,2 | 
| 6 | [1,2,1] | cắt sau 1,3 | 
| 7 | [2,1,1] | cắt sau 2,3 ​​| 
| 8 | [1,1,1,1] | rốt cuộc cũng cắt giảm | 

Có 8 trường hợp xác nhận$2^{4-1} = 8$. Dấu vết này làm cho tính độc lập của từng quyết định ranh giới trở nên rõ ràng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N + T)$| tính toán trước lên đến$10^6$cộng với thời gian không đổi cho mỗi truy vấn | 
| Không gian |$O(N)$| lưu trữ lũy thừa được tính toán trước của hai | 

Chi phí tiền xử lý là tuyến tính ở mức tối đa$n$, phù hợp thoải mái trong các ràng buộc. Mỗi trường hợp thử nghiệm được trả lời trong thời gian không đổi, giúp giải pháp trở nên hiệu quả ngay cả đối với$10^3$truy vấn. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 10**9 + 7
MAXN = 10**6

pow2 = [1] * (MAXN + 1)
for i in range(1, MAXN + 1):
    pow2[i] = (pow2[i - 1] * 2) % MOD

def solve(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        out.append(str(pow2[n - 1]))
    return "\n".join(out)

# minimum size
assert solve("1\n1\n") == "1", "n=1 base case"

# small case
assert solve("1\n3\n") == "4", "n=3 should be 4"

# another small case
assert solve("1\n4\n") == "8", "n=4 should be 8"

# multiple tests
assert solve("3\n1\n2\n5\n") == "1\n2\n16", "mixed cases"

# large boundary check (just structure)
assert solve("1\n10\n") == str(pow2[9]), "n=10 check"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n=1 | 1 | trường hợp cơ sở đúng đắn | 
| n=3 | 4 | liệt kê thành phần nhỏ | 
| n=4 | 8 | tính nhất quán của mô hình tăng trưởng | 
| hỗn hợp | 1,2,16 | xử lý nhiều truy vấn | 
| n=10 | 512 | tính chính xác của việc lập chỉ mục số mũ | 

## Vỏ cạnh 

cho$n = 1$, thuật toán trả về$2^{0} = 1$. Điều này phù hợp với cách xây dựng duy nhất có thể: chọn$x=1$, tạo ra mảng$[1]$. Công thức ranh giới vẫn được giữ nguyên vì không có khoảng trống bên trong để quyết định nên số lượng mẫu cắt là một. 

Đối với rất lớn$n$, chẳng hạn như$n = 10^6$, thuật toán vẫn hoạt động ổn định vì nó chỉ dựa vào phép lũy thừa mô-đun được tính toán trước. giá trị$2^{n-1} \bmod (10^9+7)$được xác định rõ ràng và được tính toán trước mà không cần lo ngại về độ sâu tràn hoặc đệ quy.
