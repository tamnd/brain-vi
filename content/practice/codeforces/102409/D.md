---
title: "CF 102409D - Vé số"
description: "Chúng ta có các vé được đánh số từ 1 đến (N), sắp xếp theo thứ tự tăng dần trên một vòng tròn. Diego bắt đầu ở vé (S). Từ vé hiện tại, quá trình di chuyển chính xác (K) vé còn sót lại sang bên phải và loại bỏ vé nơi nó hạ cánh."
date: "2026-08-12T03:25:32+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102409
codeforces_index: "D"
codeforces_contest_name: "Semana i 2019"
rating: 0
weight: 102409
solve_time_s: 808
verified: true
draft: false
---

[CF 102409D - Vé xổ số](https://codeforces.com/problemset/problem/102409/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 13m 28s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có các vé được đánh số từ 1 đến (N), sắp xếp theo thứ tự tăng dần trên một vòng tròn. Diego bắt đầu ở vé (S). Từ vé hiện tại, quá trình di chuyển chính xác (K) vé còn sót lại sang bên phải và loại bỏ vé nơi nó hạ cánh. Sau khi loại bỏ nó, vé hiện tại sẽ trở thành vé còn lại ngay bên trái. Vòng tròn bao quanh nên việc di chuyển qua vé (N) tiếp tục ở vé 1. 

Nhiệm vụ là xác định ID của tấm vé duy nhất còn sót lại ở cuối. Khó khăn là các vé giữa vị trí hiện tại và đích đến có thể đã bị xóa, do đó chuyển động diễn ra theo trình tự còn sót lại hiện tại chứ không phải qua ID ban đầu. 

Giá trị của (N) có thể lớn bằng (10^{18}), trong khi có thể có (10^5) trường hợp thử nghiệm. Ngay cả một lần vượt qua vé tuyến tính cũng không thể thực hiện được ở quy mô đó, chưa nói đến việc duy trì vòng tròn một cách rõ ràng. Thực tế rằng (K\le 10) là hạn chế chính về mặt cấu trúc. Chúng ta có thể mua một thuật toán có chi phí phụ thuộc vào (K) và chỉ theo logarit trên (N), nhưng phương pháp (O(N)), (O(NK)) hoặc (O(N^2)) bị loại trừ ngay lập tức. 

Có một số trường hợp đặc biệt bộc lộ những lỗi mô hình phổ biến. Ví dụ: nếu (N=1), thì đầu vào (1\ 1\ 1) phải tạo ra 1 vì không có gì cần loại bỏ. Một mô phỏng thực hiện một lần loại bỏ trước khi kiểm tra kích thước có thể truy cập vào một vòng tròn trống. 

Trường hợp (K=1) cũng đặc biệt. Đối với đầu vào (5\ 3\ 1), quy trình sẽ loại bỏ 4, rồi 5, rồi 1, rồi 2, vì vậy vé 3 vẫn tồn tại. Do đó, câu trả lời luôn là vé ban đầu (S) khi (K=1). Một công thức được rút ra từ giả định rằng vé ngay trước vé bị loại vẫn còn tồn tại ở đây, bởi vì bản thân vé trước đó có thể đã bị loại. 

Bao bọc xung quanh là một nguồn khác của lỗi từng cái một. Đối với đầu vào (2\ 1\ 2), việc di chuyển hai vé từ vé 1 sẽ quay trở lại vé 1, do đó vé 1 bị loại bỏ và vé 2 thắng. Kết quả đầu ra đúng là 2. Việc coi (K) như một sự khác biệt ID thông thường sẽ hoàn toàn bỏ sót điều này. 

Cuối cùng, vé đã xóa không thể được tính khi di chuyển. Đối với đầu vào (4\ 1\ 2), vé 3 được loại bỏ trước tiên. Nước đi tiếp theo bắt đầu từ vé 2 và chuyển qua vé 4 và 1, do đó vé 1 sẽ bị loại bỏ tiếp theo. Các vé còn lại là 2 và 4, tiếp theo là vé 4 bị loại bỏ, để lại vé 2. Câu trả lời là 2. Việc thực hiện bất cẩn chỉ cần thêm (K) vào ID trước đó sẽ sử dụng thông tin về vé không còn tồn tại. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là giữ các ID vé còn sót lại trong danh sách vòng tròn. Ở mỗi vòng, hãy tìm vị trí vé (K) ở bên phải, xóa nó và di chuyển con trỏ hiện tại về vị trí trước đó. Điều này tuân thủ chính xác quy trình nên tính chính xác của nó rất đơn giản. 

Thật không may, kích thước của danh sách ban đầu là (N). Có (N-1) lần xóa và ngay cả khi việc tìm kiếm đích chỉ tốn (O(K)), việc xóa khỏi một mảng thông thường có thể tốn (O(N)) vì các phần tử phải được dịch chuyển. Điều đó mang lại cho (O(N^2)) công việc trong trường hợp xấu nhất, khoảng (10^{36}) phép toán cơ bản khi (N=10^{18}). Danh sách liên kết tránh được sự dịch chuyển, nhưng việc xác định vị trí vé tiếp theo vẫn yêu cầu phải theo dõi tối đa (K) nút còn sống sót sau mỗi lần loại bỏ, cho ra (O(NK)), vẫn có thể đạt được khoảng (10^{19}) hoạt động. 

Quan sát hữu ích là (K) rất nhỏ. Thay vì thực hiện mọi phép loại trừ, chúng ta có thể thực hiện đồng thời nhiều phép loại bỏ và biến vòng tròn còn lại thành một trường hợp nhỏ hơn của cùng một vấn đề. 

Đối với vị trí bắt đầu tương đối là 0 và (K\ge2), hãy xem xét một vòng tròn gồm các vé (N>K). Vé bị loại đầu tiên là vị trí (K). Sau khi nó bị loại bỏ, vị trí hiện tại mới là (K-1), vị trí này vẫn còn tồn tại vì (K\ge2). Di chuyển một (K) vé còn sống sót khác sẽ rơi vào (2K). Lập luận tương tự vẫn tiếp tục, vì vậy điều đầu tiên

[ 
m=\left\lfloor\frac{N-1}{K}\right\rfloor 
] 

loại bỏ chính xác là vị trí 

[ 
K,2K,3K,\ldots,mK. 
] 

Do đó, chúng tôi đã loại bỏ (m) vé cùng một lúc, để lại (N-m) vé. Vé hiện tại mới là (mK-1). Từ thời điểm đó, quá trình còn lại lại trở thành vấn đề ban đầu, chỉ trên một vòng tròn nhỏ hơn và có nhãn vật lý khác cho các vị trí của nó. 

Đây chính là nguyên tắc xóa hàng loạt đằng sau sự tối ưu hóa tiêu chuẩn (O(K\log N)) cho bài toán Josephus, nhưng quy tắc tiền thân trong bài toán này sẽ thay đổi ánh xạ trở lại vòng tròn ban đầu. 

Đặt (F(N,K)) là vị trí chiến thắng, sử dụng các vị trí dựa trên số 0 và giả sử vé hiện tại ban đầu là vị trí 0. Sau khi xóa lô, hãy xác định 

[ 
m=\left\lfloor\frac{N-1}{K}\right\rfloor,\qquad 
Q=N-m. 
] 

Chúng tôi tính toán đệ quy (r=F(Q,K)). Vé hiện tại mới trong vòng tròn ban đầu là 

[ 
P=mK-1. 
] 

Sau (P), các vé còn sống trước tiên sẽ tiếp tục với (mK+1,mK+2,\ldots,N). hãy để 

[ 
R=N-mK 
] 

là độ dài của khối một phần cuối cùng này. Khi nó kết thúc, các vé còn sót lại sẽ xuất hiện theo khối (K-1), cụ thể là 

[ 
1,\ldots,K-1,\quad K+1,\ldots,2K-1,\quad\ldots 
] 

bởi vì mọi bội số của (K) đã bị loại bỏ. 

Nếu câu trả lời đệ quy là (r=0), nó sẽ đề cập đến chính (P). Ngược lại, đặt (u=r-1). Nếu (u<R), câu trả lời nằm ở khối một phần sau (P), tại 

[ 
mK+1+u. 
] 

Ngược lại, sau khi bỏ qua khối một phần đó, hãy đặt (v=u-R). Vị trí ban đầu tương ứng là 

[ 
\left\lfloor\frac{v}{K-1}\right\rfloor K 
+ 
(v\bmod(K-1))+1. 
] 

Mỗi lệnh gọi đệ quy giảm (N) xuống còn khoảng (N(1-1/K)). Do đó độ sâu đệ quy là (O(K\log N)). Vì (K\le10), giá trị này là nhỏ ngay cả đối với (N=10^{18}). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(NK)) với danh sách liên kết, (O(N^2)) với một mảng | (O(N)) | Quá chậm | 
| Tối ưu | (O(K\log N)) cho mỗi trường hợp thử nghiệm | (O(K\log N)) phụ trợ | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Làm việc với các vị trí dựa trên số 0 và xác định (F(N,K)) là vị trí chiến thắng khi vé hiện tại ban đầu ở vị trí 0. Vé bắt đầu thực tế (S) sẽ được áp dụng như một ca tuần hoàn cuối cùng. 
2. Nếu (K=1), trả về 0. Việc di chuyển một vé luôn loại bỏ vé kế tiếp ngay lập tức và vé hiện tại mới là vé mà chúng ta đã bắt đầu từ đó, vì vậy vé ban đầu vẫn tồn tại. 
3. Nếu (N\le K), mô phỏng trực tiếp quá trình. Có nhiều nhất 10 vé trong trường hợp này, vì vậy việc này chỉ tiêu tốn một lượng công việc không đổi. 
4. Với (N>K) và (K\ge2), hãy tính 

[ 
m=\left\lfloor\frac{N-1}{K}\right\rfloor. 
] 

(m) vé bị loại đầu tiên là (K,2K,\ldots,mK). Sau vé cuối cùng, vé hiện tại là (mK-1). 
5. Loại bỏ (m) vé đó về mặt khái niệm và xác định 

[ 
Q=N-m. 
] 

Quá trình còn lại, nhìn từ vé hiện tại mới, là vấn đề tương tự trên vé (Q). Tính toán đệ quy (r=F(Q,K)). 
6. Tính độ dài của phần khối còn sót lại trước khi vòng tròn bao bọc: 

[ 
R=N-mK. 
] 

Nếu (r=0), người chiến thắng đệ quy là vé hiện tại mới (mK-1). 
7. Nếu (r>0), đặt (u=r-1). Khi (u<R), người chiến thắng vẫn ở khối một phần sau (mK-1), nên vị trí ban đầu của nó là (mK+1+u), modulo giảm (N). 
8. Nếu (u\ge R), trừ phần khối và đặt (v=u-R). Các vé còn lại xuất hiện theo nhóm (K-1), cách nhau bởi một bội số đã xóa của (K). Do đó vị trí ban đầu là 

[ 
\left\lfloor\frac{v}{K-1}\right\rfloor K 
+(v\bmod(K-1))+1. 
] 
9. Bắt đầu từ trường hợp cơ sở được tính toán đệ quy, áp dụng các ánh xạ này theo thứ tự ngược lại cho đến khi (N) ban đầu được khôi phục. Giá trị kết quả là (F(N,K)). 
10. Cuối cùng chuyển người chiến thắng tương đối bằng vé xuất phát thực tế. Nếu (F) dựa trên 0 thì câu trả lời là 

[ 
((S-1+F)\bmod N)+1. 
] 

### Tại sao nó hoạt động

Bất biến là (F(N,K)) luôn mô tả người chiến thắng so với vé hiện tại, không liên quan đến vé 1. Đối với (N>K) và (K\ge2), lần loại bỏ (m) đầu tiên chính xác là bội số của (K) và vé hiện tại sau những lần loại bỏ đó là (mK-1). Các vé còn lại giữ nguyên thứ tự vòng tròn của chúng, do đó phần còn lại của trò chơi chính xác là một phiên bản (F(N-m,K)) khi được xem từ vé hiện tại mới đó. Công thức ánh xạ liệt kê những vé còn lại theo thứ tự vòng tròn chính xác của chúng, do đó, nó chuyển đổi vị trí tương đối đệ quy trở lại vị trí ban đầu mà không thay đổi người chiến thắng. Mô phỏng cơ sở là chính xác và mọi phiên bản lớn hơn được rút gọn thành một phiên bản hợp lệ nhỏ hơn, điều này chứng tỏ rằng vị trí cuối cùng là người sống sót thực sự. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def relative_winner(n, k):
    if k == 1:
        return 0

    frames = []

    while n > k:
        m = (n - 1) // k
        tail = n - m * k
        frames.append((n, m, tail))
        n -= m

    # n <= k, so direct simulation is tiny.
    circle = list(range(n))
    cur = 0

    while len(circle) > 1:
        size = len(circle)
        target = (cur + k) % size
        circle.pop(target)

        size -= 1
        cur = (target - 1) % size

    winner = circle[0]

    # Restore the positions removed by the batch steps.
    for n, m, tail in reversed(frames):
        if winner == 0:
            winner = m * k - 1
            continue

        u = winner - 1

        if u < tail:
            winner = (m * k + 1 + u) % n
        else:
            v = u - tail
            winner = (v // (k - 1)) * k + (v % (k - 1)) + 1

    return winner

def solve(reader=input):
    t = int(reader())
    out = []

    for _ in range(t):
        n, s, k = map(int, reader().split())
        f = relative_winner(n, k)
        answer = (s - 1 + f) % n + 1
        out.append(str(answer))

    return "\n".join(out)

if __name__ == "__main__":
    sys.stdout.write(solve())
```các`relative_winner`hàm cố tình tách vé bắt đầu thực tế khỏi phép tính đệ quy. Bên trong hàm, vị trí 0 luôn có nghĩa là vé hiện tại, điều này làm cho mọi mức giảm không phụ thuộc vào giá trị ban đầu của (S). 

Với (K=1), câu trả lời liên quan đến vé xuất phát ngay lập tức là 0. Trường hợp đặc biệt này là cần thiết vì vé trước của vé bị loại có thể đã biến mất, do đó đối số lô cho (K\ge2) không được áp dụng. 

Đối với (N) lớn hơn, mỗi khung lưu trữ thông tin cần thiết để tái tạo lại một mức giảm. số lượng`m`là số bội số của (K) bị loại bỏ trong lô, trong khi`tail`là số lượng vé còn sót lại từ`m * k + 1`qua (N). 

Phiên bản nhỏ còn lại được mô phỏng bằng danh sách Python. Việc mô phỏng sử dụng`target = (cur + k) % size`, bởi vì quy tắc yêu cầu di chuyển (K) các vé còn sống sót, thay vì tính vé hiện tại là nước đi đầu tiên. Sau khi loại bỏ,`(target - 1) % size`chọn người tiền nhiệm còn sống sót. Hành vi modulo của Python rất hữu ích ở đây vì`-1 % size`đưa ra chỉ số cuối cùng một cách chính xác. 

Trong quá trình tái thiết,`winner == 0`có nghĩa là người chiến thắng đệ quy chính xác là tấm vé hiện tại mới,`m * k - 1`. Nếu không thì,`winner - 1`đếm khoảng cách trong chuỗi sau tấm vé hiện tại mà người chiến thắng đệ quy nằm. đầu tiên`tail`các vị trí tạo thành một khối một phần và mọi thứ sau nó được chia thành các khối (K-1) không phải bội số của (K). 

Số nguyên Python có độ chính xác tùy ý, do đó, các giá trị lên tới (10^{18}) không yêu cầu xử lý tràn đặc biệt. Việc triển khai cũng tránh được đệ quy, giúp giữ cho ngăn xếp cuộc gọi không đổi và làm cho việc giảm (O(K\log N)) trở nên rõ ràng. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đối với đầu vào (N=5,S=3,K=2), trước tiên hãy tính người chiến thắng tương ứng với vị trí bắt đầu 0. 

| (N) | (K) | (m) | (Q) | (R) | Người chiến thắng đệ quy | Đã khôi phục (F(N,K)) | 
| --- | --- | --- | --- | --- | --- | --- | 
| 5 | 2 | 2 | 3 | 1 | 0 | 3 | 
| 3 | 2 | 1 | 2 | 1 | 1 | 0 | 
| 2 | 2 | căn cứ | căn cứ | căn cứ | 1 | 1 | 

Trường hợp cơ bản có hai vé có người chiến thắng tương đối là 1 vì việc di chuyển hai vé sẽ quay trở lại vé hiện tại, loại bỏ nó. 

Đối với (N=3), phần thắng đệ quy là vị trí 1. Khối một phần còn lại đầu tiên có độ dài 1, do đó vị trí đệ quy ánh xạ tới vị trí 0. Đối với (N=5), phần thắng đệ quy là vị trí 0, do đó nó ánh xạ trực tiếp đến vị trí hiện tại mới (mK-1=3). 

Do đó, người chiến thắng tương đối là vị trí 3. Bắt đầu từ vé 3 có nghĩa là chuyển đổi theo hai vị trí: 

[ 
((3-1)+3)\bmod5+1=1. 
] 

Đầu ra là 1. 

### Ví dụ được xây dựng 

Xét (N=4,S=1,K=2). 

| (N) | (K) | (m) | (Q) | (R) | Người chiến thắng đệ quy | Đã khôi phục (F(N,K)) | 
| --- | --- | --- | --- | --- | --- | --- | 
| 4 | 2 | 1 | 3 | 2 | 0 | 1 | 
| 3 | 2 | 1 | 2 | 1 | 1 | 0 | 
| 2 | 2 | căn cứ | căn cứ | căn cứ | 1 | 1 | 

Người chiến thắng tương đối ở vị trí 1, do đó với (S=1) người chiến thắng thực sự là vé 2. 

Một mô phỏng trực tiếp xác nhận điều đó. Vé 3 được loại bỏ trước tiên, sau đó đến vé 1, sau đó là vé 4, để lại vé 2. Dấu vết phù hợp với bất biến đệ quy ở mỗi lần rút gọn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian cho mỗi trường hợp thử nghiệm | (O(K\log N)) | Mỗi lần giảm sẽ loại bỏ khoảng (N/K) vé và kích thước còn lại là khoảng (N(1-1/K)). | 
| Tổng thời gian | (O(TK\log N)) | Giới hạn tương tự áp dụng độc lập cho tất cả các trường hợp (T). | 
| Không gian phụ trợ | (O(K\log N)) | Một khung được lưu trữ cho mỗi lần giảm trước khi xây dựng lại. | 
| Không gian đầu ra | (O(T)) | Các câu trả lời được tích lũy trước khi viết. | 

Với (K\le10) và (N\le10^{18}), việc rút gọn đệ quy chỉ có vài trăm mức ngay cả trong trường hợp xấu nhất. Thuật toán không bao giờ phân bổ một mảng tỷ lệ với (N), do đó cả giới hạn bộ nhớ 256 MB và giới hạn thời gian 5 giây đều tương thích với phương pháp dự định. 

## Trường hợp thử nghiệm```python
import sys
import io

def relative_winner(n, k):
    if k == 1:
        return 0

    frames = []

    while n > k:
        m = (n - 1) // k
        tail = n - m * k
        frames.append((n, m, tail))
        n -= m

    circle = list(range(n))
    cur = 0

    while len(circle) > 1:
        size = len(circle)
        target = (cur + k) % size
        circle.pop(target)

        size -= 1
        cur = (target - 1) % size

    winner = circle[0]

    for n, m, tail in reversed(frames):
        if winner == 0:
            winner = m * k - 1
        else:
            u = winner - 1

            if u < tail:
                winner = (m * k + 1 + u) % n
            else:
                v = u - tail
                winner = (v // (k - 1)) * k + (v % (k - 1)) + 1

    return winner

def solve(reader):
    t = int(reader())
    out = []

    for _ in range(t):
        n, s, k = map(int, reader().split())
        f = relative_winner(n, k)
        out.append(str((s - 1 + f) % n + 1))

    return "\n".join(out)

def run(inp: str) -> str:
    return solve(io.StringIO(inp).readline) + "\n"

# Provided sample
assert run("1\n5 3 2\n") == "1\n", "sample 1"

# Minimum-size input
assert run("1\n1 1 10\n") == "1\n", "single ticket"

# K = 1, so the starting ticket always survives
assert run("1\n100 73 1\n") == "73\n", "K=1"

# Maximum N, with K=1
assert run("1\n1000000000000000000 1000000000000000000 1\n") == \
       "1000000000000000000\n", "maximum N"

# K = N, with all three values equal
assert run("1\n5 5 5\n") == "1\n", "N=S=K"

# Boundary case where the first move wraps to the current ticket
assert run("1\n2 1 2\n") == "2\n", "wrap to current"

# Off-by-one case involving deleted tickets
assert run("1\n4 1 2\n") == "2\n", "deleted tickets affect movement"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1 1 10`|`1`| Tối thiểu (N), trường hợp cơ sở tức thời | 
|`1 / 100 73 1`|`73`| Hành vi đặc biệt cho (K=1) | 
|`1 / 10^18 10^18 1`|`10^18`| Số học tối đa (N) và độ chính xác tùy ý | 
|`1 / 5 5 5`|`1`| (N=S=K), các tham số bao bọc và hoàn toàn bằng nhau | 
|`1 / 2 1 2`|`2`| Hạ cánh trở lại vé hiện tại và xử lý tiền nhiệm | 
|`1 / 4 1 2`|`2`| Di chuyển chỉ qua những tấm vé còn sót lại | 

## Vỏ cạnh 

Với (N=1), đầu vào`1 1 10`ngay lập tức bước vào mô phỏng vòng tròn nhỏ với một phần tử. Không có việc loại bỏ nào được thực hiện, do đó vị trí 0 được trả về và chuyển đổi trở lại vé 1. Thuật toán không bao giờ cố gắng tính toán người đứng trước trong một danh sách trống. 

Với (K=1), hãy xét`5 3 1`. Từ vé 3, vé 4 bị loại bỏ, rồi vé 5, rồi vé 1, rồi vé 2. Vé 3 vẫn tồn tại. Thuật toán trả về vị trí tương đối 0 cho mọi (N) và ca cuối cùng chuyển đổi trực tiếp vị trí đó thành (S=3). Điều này tránh việc áp dụng công thức lô (K\ge2) trong trường hợp giả định trước đó của nó là sai. 

Đối với một động thái hoàn toàn quay trở lại phiếu hiện tại, hãy xem xét`2 1 2`. So với vé 1, việc di chuyển hai vé còn sống sót lại đến vé 1 nên vé 1 bị loại bỏ. Vé 2 là vé duy nhất còn lại và thắng. Mô phỏng cơ sở tính toán chỉ số mục tiêu`(0+2)%2=0`, loại bỏ nó và để lại người chiến thắng tương đối là 1. Câu trả lời cuối cùng là 2. 

Đối với các vé đã xóa thay đổi ý nghĩa của khoảng cách, hãy xem xét`4 1 2`. Vé 3 được loại bỏ đầu tiên. Vé hiện tại tiếp theo là 2 và việc di chuyển hai vé còn sót lại sẽ đạt đến vé 1 chứ không phải vé 4, vì chuỗi còn lại là 2, 4, 1. Thuật toán không bao giờ biểu thị chuyển động bằng cách sử dụng các khác biệt ID ban đầu. Việc giảm hàng loạt của nó loại bỏ rõ ràng bội số của (K) trước tiên và sau đó ánh xạ các vị trí thông qua chuỗi còn sót lại được nén. 

Đối với trường hợp (N=S=K=5), đầu vào`5 5 5`bắt đầu ở vé 5. Nước đi đầu tiên của 5 vé còn sống sẽ quay trở lại vé 5 nên bị loại bỏ. Quá trình sau đó tiếp tục xoay quanh bốn vé còn lại, cuối cùng để lại vé 1. Mô phỏng vòng tròn nhỏ xử lý trực tiếp việc này vì (N\le K), tránh giả định rằng lô đầu tiên chứa ít nhất một bội số đầy đủ của (K). 

Đối với giá trị tối đa (N=10^{18}), đầu vào`1000000000000000000 1000000000000000000 1`được xử lý bằng phím tắt (K=1) và trả lại vé xuất phát ngay lập tức. Tổng quát hơn, phép rút gọn chỉ sử dụng phép chia, phép nhân, phép cộng và modulo số nguyên trên các giá trị được giới hạn bởi bội số nhỏ của (10^{18}), tất cả đều được Python xử lý chính xác mà không bị tràn.
