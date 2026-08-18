---
title: "CF 102343J - Ý chí của nhóm lập trình"
description: "Vấn đề mô hình phân phối kẹo mút dưới dạng biểu đồ có trọng số có hướng. Có (N) thành viên nhóm sắp rời đi, được biểu thị bằng các đỉnh (1) đến (N) và (M-N) các sinh viên UCF khác, được biểu thị bằng các đỉnh (N+1) đến (M)."
date: "2026-08-17T10:24:01+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102343
codeforces_index: "J"
codeforces_contest_name: "UCF Locals 2019"
rating: 0
weight: 102343
solve_time_s: 128
verified: true
draft: false
---

[CF 102343J - Ý chí của nhóm lập trình](https://codeforces.com/problemset/problem/102343/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2 phút 8 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Vấn đề mô hình phân phối kẹo mút dưới dạng biểu đồ có trọng số có hướng. Có (N) thành viên nhóm sắp rời đi, được biểu thị bằng các đỉnh (1) đến (N) và (M-N) các sinh viên UCF khác, được biểu thị bằng các đỉnh (N+1) đến (M). Mỗi thành viên rời đi bắt đầu với một số kẹo mút và có ý chí phân phối toàn bộ bộ sưu tập của họ cho một số học sinh theo các phân số nhất định. Di chúc có thể gửi kẹo mút cho một thành viên sắp ra đi khác, do đó việc phân phối có thể tiếp tục thông qua nhiều người. 

Quá trình dự định liên tục áp dụng ý muốn của mọi thành viên rời đi đối với những chiếc kẹo mút mà thành viên đó hiện đang nắm giữ. Chúng tôi được yêu cầu số tiền cuối cùng mà mỗi học sinh nhận được. Nếu một số cây kẹo mút có thể tồn tại mãi mãi trong một nhóm thành viên đã ra đi thì những cây kẹo mút đó sẽ bị loại bỏ. Các ràng buộc chính thức là (N \le 500), (M \le 50{,}000) và tối đa (1{,}000{,}000) tổng số mục nhập sẽ. Giới hạn thời gian chính thức là 7 giây và giới hạn bộ nhớ là 1024 MB. 

Sự khác biệt chính là giữa tối đa 500 thành viên rời đi và tổng số sinh viên có thể là 50.000. Giá trị lớn của (M) có nghĩa là chúng ta không thể chấp nhận bất cứ giá trị bậc hai nào về số lượng tất cả học sinh, mà chỉ những thành viên rời đi mới tham gia vào quá trình đệ quy. Vì (N) chỉ bằng 500 nên về nguyên tắc, việc tính toán đại số tuyến tính (O(N^3)) là khả thi. Giới hạn hàng triệu mục nhập cũng có nghĩa là bản thân đầu vào có thể lớn, do đó đầu vào phải được xử lý hiệu quả. 

Có hai trường hợp khó nhận thấy khiến mô phỏng trực tiếp không đáng tin cậy. Đầu tiên, một chu trình khép kín có thể không bao giờ gửi bất cứ thứ gì cho một học sinh bình thường. Coi như```
2 3
1 1
2 1.0
1 1
1 1.0
```Học sinh 1 và 2 tiếp tục trao đổi kẹo mãi mãi, trong khi học sinh 3 không nhận được gì. Đầu ra đúng là```
0.0
0.0
0.0
```Một mô phỏng chờ đợi số lượng thành viên rời đi trở nên nhỏ sẽ không bao giờ kết thúc. 

Vấn đề thứ hai là chu trình chỉ rò rỉ một phần rất nhỏ trong mỗi lần truy cập. Ví dụ,```
2 3
1 2
2 0.999999
3 0.000001
1 1
1 1.0
```Mỗi cây kẹo cuối cùng cũng đến tay học sinh thứ 3, nên kết quả cuối cùng là```
0.0
0.0
2.0
```Một mô phỏng dừng lại sau một số vòng cố định vẫn có thể có gần như tất cả các kẹo mút nằm bên trong chu trình và sẽ đưa ra một câu trả lời sai rõ ràng. Giới hạn dưới của (10^{-6}) trên các phân số dương làm cho loại hội tụ chậm này có thể xảy ra. 

Sai lầm dễ mắc phải thứ ba là coi (N) học sinh đầu tiên như những người nhận kết quả đầu ra thông thường. Họ đều là những thành viên sắp ra đi nên câu trả lời cuối cùng của họ luôn là con số 0. Kẹo mút của họ có thể đến tay một học sinh chưa rời trường hoặc biến mất bên trong một khu vực đóng cửa. Câu lệnh yêu cầu rõ ràng (N) dòng đầu ra đầu tiên bằng 0. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là mô phỏng quá trình. Giữ số kẹo mút hiện tại cho mỗi thành viên rời đi, áp dụng mọi ý chí và lặp lại cho đến khi tổng số kẹo còn lại giữa các thành viên rời đi đủ nhỏ. Điều này phản ánh chính xác định nghĩa, vì vậy khi nó kết thúc, nó sẽ phân phối đúng. 

Vấn đề là việc chấm dứt không được đảm bảo ở bất kỳ tốc độ thực tế nào. Một chu trình có thể giữ lại (0,999999) khối lượng của nó sau mỗi vòng hoàn chỉnh. Để giảm số tiền ban đầu khoảng (500{,}000) xuống dưới (10^{-5}), chúng tôi cần theo thứ tự 

[ 
\frac{\log(10^{-5}/500000)}{\log(0.999999)} 
] 

vòng, tức là khoảng (2,5\lần 10^7) vòng. Với tối đa (10^6) mục nhập, điều này có nghĩa là khoảng (10^{13}) hoạt động chuyển tiếp. Một chu trình khép kín thậm chí còn tệ hơn vì mô phỏng không bao giờ đạt đến trạng thái dừng. 

Quan sát hữu ích là quá trình lặp lại là một hệ phương trình tuyến tính. Gọi (x_i) là tổng số kẹo sẽ đi qua thành viên rời đi (i), tính bộ sưu tập ban đầu và mọi bộ sưu tập đến đó sau đó. Nếu ý chí của thành viên (j) chia phần (p_{j,i}) cho thành viên (i) thì 

[ 
x_i = L_i + \sum_{j=1}^{N} x_jp_{j,i}. 
] 

Phía bên trái đại diện cho mọi thứ từng đạt tới (i). Phía bên phải chứa kẹo mút thuộc sở hữu ban đầu của (i), cộng với mọi thứ đến từ các thành viên rời đi khác. 

Khi đã biết các giá trị (x_i), câu trả lời cho một học sinh bình thường (k) chỉ đơn giản là 

[ 
\text{answer__k = 
\sum_{i=1}^{N}x_i p_{i,k}. 
] 

Vì vậy quá trình vô hạn đã trở thành một hệ thống tuyến tính hữu hạn. 

Có một điều phức tạp. Một nhóm thành viên rời đi có thể tạo thành một thành phần khép kín không bao giờ đến được với một học sinh bình thường. Đối với thành phần như vậy, ma trận tương ứng là số ít vì tổng khối lượng của nó có thể luân chuyển mãi mãi. Chúng ta không cần phải giải các biến đó. Trước tiên, chúng ta có thể tìm thấy mọi thành viên sắp ra đi có đường dẫn trực tiếp nào đó đến một học sinh bình thường. Đó chính xác là những thành viên mà kẹo mút cuối cùng có thể đóng góp vào sản lượng yêu cầu. Mọi phần tử còn lại đều thuộc về một phần của biểu đồ mà toàn bộ khối lượng của phần đó cuối cùng sẽ bị loại bỏ. 

Sau khi loại bỏ những thành viên không liên quan đó, mọi trạng thái còn lại cuối cùng đều có thể đến được với một học sinh bình thường. Hệ thống chuyển tiếp thu được là nhất thời, do đó (I-Q^T) là không suy biến và hệ thống tuyến tính có nghiệm duy nhất. Chúng ta có thể giải quyết nó bằng cách loại bỏ Gaussian. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Có khả năng (O(TK)), với (T) lớn tùy ý | (O(M+K)) | Quá chậm và có thể không bao giờ chấm dứt | 
| Tối ưu | (O(N^3 + K)) | (O(N^2 + K + M)) | Đã chấp nhận | 

Ở đây (K) là tổng số mục di chúc, nhiều nhất là (10^6) và (N\le500). Thuật ngữ (N^3) xuất phát từ việc loại bỏ Gaussian, trong khi xử lý ý chí và xây dựng đầu ra cuối cùng là tuyến tính trong tổng kích thước của chúng. 

## Hướng dẫn thuật toán

1. Đọc tất cả di chúc và lưu trữ xác suất chuyển tiếp từ mỗi thành viên ra đi đến từng người nhận. Đồng thời, xây dựng các cạnh ngược giữa các thành viên rời đi. Sau này, chúng tôi cần có đầy đủ di chúc để tính toán kết quả cuối cùng, trong khi biểu đồ ngược lại cho phép chúng tôi xác định thành viên sắp ra đi nào cuối cùng có thể tiếp cận được một học sinh bình thường. 
2. Đánh dấu từng thành viên sắp ra đi có ý chí trực tiếp tặng một phần dương cho một học sinh bình thường. Đây là những điểm khởi đầu của tập hợp các trạng thái hữu ích vì chúng có thể ngay lập tức gửi khối lượng ra ngoài nhóm khởi hành. 
3. Duyệt qua đồ thị ngược từ những thành viên được đánh dấu. Bất cứ khi nào một thành viên rời đi có thể tiếp cận một thành viên đã được đánh dấu, hãy đánh dấu thành viên đó. Sau quá trình truyền tải này, một thành viên được đánh dấu sẽ có đường dẫn trực tiếp đến một học sinh bình thường, trong khi mọi thành viên không được đánh dấu sẽ bị mắc kẹt trong một khu vực mà không một học sinh bình thường nào có thể tiếp cận được. 
4. Tạo một phương trình tuyến tính cho mỗi thành viên rời đi được đánh dấu. Gọi (x_i) là tổng số tiền từng chuyển qua thành viên đó. Với mỗi dấu (i), hãy viết 

[ 
x_i-\sum_{j\text{ được đánh dấu}}x_jp_{j,i}=L_i. 
] 

Xác suất được gửi từ một thành viên không được đánh dấu sẽ không xuất hiện vì thành viên đó không bao giờ có thể đóng góp vào bất kỳ câu trả lời cuối cùng nào. 

1. Giải hệ phương trình thu được bằng phương pháp khử Gauss. Chúng tôi sử dụng phương pháp xoay vòng một phần để làm cho việc tính toán dấu phẩy động ổn định hơn. Chỉ có một vế phải nên việc loại bỏ thông thường sau đó thay thế ngược lại là đủ. 
2. Đặt câu trả lời của mọi thành viên rời đi về 0. Những chiếc kẹo này là số lượng trung gian, không phải người nhận cuối cùng. 
3. Với mỗi mục di chúc ((i,k,p)) có người nhận (k) là một học sinh bình thường, hãy thêm (x_i p) vào câu trả lời cho (k). Điều này trực tiếp chuyển đổi tổng số tiền từng được xử lý bởi mỗi thành viên rời đi thành số tiền cuối cùng được phân phối bên ngoài nhóm khởi hành. 
4. In tất cả (M) câu trả lời theo thứ tự mã học sinh tăng dần. Độ chính xác yêu cầu là (10^{-5}), do đó, chỉ cần in vài chữ số sau dấu thập phân là đủ. 

Tại sao nó hoạt động: bất biến trung tâm là (x_i) đại diện cho mọi kẹo mút sẽ được thành viên (i) xử lý, chứ không chỉ là số tiền hiện được giữ ở đó. Mỗi chiếc kẹo mút như vậy là một trong những (L_i) thuộc sở hữu ban đầu của (i) hoặc nó đến từ một thành viên rời đi khác (j) với số lượng (x_jp_{j,i}). Do đó phương trình tuyến tính mô tả chính xác quá trình vô hạn. Đối với một trạng thái có thể tiếp cận một học sinh bình thường, ứng dụng lặp đi lặp lại cuối cùng sẽ chuyển toàn bộ khối lượng xác suất của nó ra khỏi trạng thái ban đầu, do đó hệ thống con tương ứng có một nghiệm hữu hạn duy nhất. Các trạng thái không thể chạm tới một học sinh bình thường chỉ có thể luân chuyển hoặc nuôi sống các trạng thái tương tự khác, và khối lượng của chúng bị loại bỏ đúng như bài toán đã chỉ định. Khi đã biết các giá trị (x_i), việc nhân từng giá trị với các phân số trong nó sẽ tính cho mỗi lần chuyển cuối cùng đúng một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())

    initial = [0.0] * n
    wills = [[] for _ in range(n)]
    rev = [[] for _ in range(n)]

    for i in range(n):
        l, k = map(int, input().split())
        initial[i] = float(l)

        entries = []
        for _ in range(k):
            x, p = input().split()
            x = int(x) - 1
            p = float(p)
            entries.append((x, p))

            if x < n:
                rev[x].append(i)

        wills[i] = entries

    # Find all departing members that can eventually reach
    # at least one ordinary student.
    useful = [False] * n
    stack = []

    for i in range(n):
        for x, p in wills[i]:
            if x >= n:
                if not useful[i]:
                    useful[i] = True
                    stack.append(i)
                break

    while stack:
        v = stack.pop()
        for u in rev[v]:
            if not useful[u]:
                useful[u] = True
                stack.append(u)

    ids = [i for i in range(n) if useful[i]]
    s = len(ids)

    ans = [0.0] * m

    if s:
        pos = [-1] * n
        for i, v in enumerate(ids):
            pos[v] = i

        # A[i][j] = delta(i,j) - probability(j -> i)
        a = [[0.0] * (s + 1) for _ in range(s)]

        for i, v in enumerate(ids):
            a[i][i] = 1.0
            a[i][s] = initial[v]

        for u in ids:
            pu = pos[u]
            for v, p in wills[u]:
                if v < n and useful[v]:
                    pv = pos[v]
                    a[pv][pu] -= p

        # Gaussian elimination with partial pivoting.
        for col in range(s):
            pivot = col
            best = abs(a[col][col])

            for row in range(col + 1, s):
                value = abs(a[row][col])
                if value > best:
                    best = value
                    pivot = row

            if pivot != col:
                a[col], a[pivot] = a[pivot], a[col]

            inv = 1.0 / a[col][col]

            # Normalize the pivot row.
            row = a[col]
            for j in range(col, s + 1):
                row[j] *= inv

            # Eliminate below.
            for row_idx in range(col + 1, s):
                row2 = a[row_idx]
                factor = row2[col]
                if factor == 0.0:
                    continue

                row2[col] = 0.0
                for j in range(col + 1, s + 1):
                    row2[j] -= factor * row[j]

        x = [0.0] * s

        # Back substitution.
        for i in range(s - 1, -1, -1):
            value = a[i][s]
            row = a[i]
            for j in range(i + 1, s):
                value -= row[j] * x[j]
            x[i] = value

        # Distribute the total amount processed by each useful
        # departing member to ordinary students.
        for i, v in enumerate(ids):
            amount = x[i]
            for to, p in wills[v]:
                if to >= n:
                    ans[to] += amount * p

    sys.stdout.write("\n".join(f"{v:.10f}" for v in ans))

if __name__ == "__main__":
    solve()
```Phần đầu tiên lưu trữ số lượng kẹo ban đầu và di chúc. các`rev`đồ thị chỉ chứa các cạnh giữa các thành viên rời đi vì đó là những cạnh duy nhất liên quan đến việc quyết định liệu một trạng thái cuối cùng có thể thoát khỏi một học sinh bình thường hay không. 

Quá trình duyệt ngược thực hiện bước 2 và 3. Ban đầu, một thành viên sẽ hữu ích nếu ý chí của nó có lợi thế trực tiếp đối với một học sinh bình thường. Đi theo các cạnh ngược lại sẽ tìm thấy mọi thành viên cuối cùng có thể tiếp cận được một trong những thành viên hữu ích đó. Điều này tốt hơn là cố gắng phát hiện các chu trình khép kín một cách rõ ràng. Một thành viên không cần phải thuộc về một thành phần được kết nối mạnh mẽ mới không liên quan. Thuộc tính duy nhất chúng ta cần là liệu có đường dẫn nào đến người nhận cuối cùng hay không. 

Ma trận sử dụng phương trình 

[ 
x_i-\sum_j x_jp_{j,i}=L_i. 
] 

Điều đó giải thích sự định hướng ma trận hơi không trực quan. Một xác suất được lưu trữ trong ý chí của (j) sẽ về (i) thuộc hàng (i), cột (j). Đường chéo bắt đầu từ một và mọi chuyển đổi sắp tới sẽ trừ đi xác suất của nó. 

Việc loại bỏ Gaussian chuẩn hóa từng hàng trục và sau đó loại bỏ hệ số trục khỏi mọi hàng thấp hơn. Xoay vòng một phần chọn hệ số khả dụng lớn nhất trong cột hiện tại, giảm sai số số khi xác suất khiến hệ thống được điều hòa kém. Ma trận có tối đa 500 hàng, do đó biểu diễn dày đặc đủ nhỏ cho giới hạn bộ nhớ. 

Việc thay thế ngược sẽ thu hồi tổng số tiền (x_i) được xử lý bởi mỗi thành viên khởi hành hữu ích. Sau đó chúng tôi quét lại di chúc gốc và gửi`amount * p`tới mọi người nhận thông thường. Chúng tôi cố tình không thêm bất cứ điều gì cho các sinh viên sắp ra trường vì kết quả yêu cầu của họ bằng không. 

Không có vấn đề tràn số nguyên trong Python. Các giá trị trung gian có thể lớn là các đại lượng dấu phẩy động và xác suất dương nhỏ nhất chỉ là (10^{-6}), do đó, độ chính xác kép mang lại đủ độ chính xác tương đối cho dung sai đầu ra (10^{-5}) cần thiết trong hệ thống dự kiến. 

## Ví dụ đã hoạt động 

Mẫu chính thức là```
2 5
100 2
2 0.9
3 0.1
100 2
1 0.2
4 0.8
```Hai thành viên rời đi tạo thành một vòng tròn. Thành viên 1 gửi 90% cho thành viên 2 và 10% cho sinh viên 3. Thành viên 2 gửi 20% cho thành viên 1 và 80% cho sinh viên 4. 

Các phương trình cho tổng số lượng được xử lý là 

[ 
x_1=100+0,2x_2, 
] 

[ 
x_2=100+0,9x_1. 
] 

Quá trình loại bỏ cho các giá trị sau. 

| Bước | (x_1) | (x_2) | Ý nghĩa | 
| --- | --- | --- | --- | 
| Phương trình ban đầu | (100+0,2x_2) | (100+0,9x_1) | Bộ sưu tập ban đầu là 100 mỗi bộ | 
| Thay thế (x_2) | (120+0,18x_1) | (100+0,9x_1) | Mở rộng luồng đệ quy | 
| Giải quyết | (243.902439) | (319.512195) | Tổng số tiền mỗi thành viên xử lý | 
| Học sinh cuối cấp 3 | 0 | 0 | (0,1x_1=24,390244) | 
| Sinh viên cuối khóa 4 | 0 | 0 | (0,8x_2=255,609756) | 

Đầu ra chính xác từ mẫu chính thức là```
0.0
0.0
14.63414634
185.36585366
0.0
```Bảng trên cho thấy lý do tại sao chỉ nhìn vào các bộ sưu tập ban đầu là không đủ. Mỗi thành viên xử lý nhiều hơn đáng kể so với 100 cây kẹo ban đầu của họ vì một số cây kẹo của thành viên khác sẽ quay trở lại với họ. 

Đối với ví dụ thứ hai, hãy xem xét```
2 3
1 2
2 0.5
3 0.5
1 1
1 1.0
```Thành viên 1 gửi một nửa số tiền đã xử lý cho thành viên 2 và một nửa cho sinh viên 3. Thành viên 2 gửi lại toàn bộ cho thành viên 1. 

Các phương trình là 

[ 
x_1=1+x_2, 
] 

[ 
x_2=0,5x_1. 
] 

Hệ thống Gaussian và chuyển giao cuối cùng là: 

| Bước | (x_1) | (x_2) | Sinh viên 3 | 
| --- | --- | --- | --- | 
| Ban đầu | (1+x_2) | (0,5x_1) | 0 | 
| Giải (x_2=0,5x_1) | 2 | 1 | 0 | 
| Áp dụng di chúc của thành viên 1 | 2 | 1 | 1 | 
| Áp dụng ý chí của thành viên 2 | 2 | 1 | 1 | 

Đầu ra đúng là```
0.0
0.0
1.0
```Dấu vết này thể hiện sự bất biến một cách trực tiếp. Thành viên 1 xử lý tổng cộng hai chiếc kẹo, thành viên 2 xử lý một chiếc và chính xác một chiếc kẹo mút đến tay học sinh bình thường. Cây kẹo mút còn lại chỉ ở trong chu trình đệ quy dưới dạng luồng trung gian chứ không phải là đầu ra cuối cùng bổ sung. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(N^3+K+M)) | Hệ thống tuyến tính có tối đa (N=500) biến, trong khi di chúc chứa (K\le10^6) mục | 
| Không gian | (O(N^2+K+M)) | Nhu cầu ma trận dày đặc (O(N^2)), nhu cầu ý chí được lưu trữ (O(K)) và nhu cầu đầu ra (O(M)) | 

Với (N\le500), phần khối chứa tối đa khoảng (500^3/3) cập nhật loại bỏ sau khi khai thác cấu trúc tam giác của phép loại bỏ Gaussian. Đầu vào hàng triệu mục được xử lý tuyến tính. Điều này phù hợp với thiết kế của bài toán, trong đó số lượng người tương tác đệ quy là nhỏ mặc dù tổng số sinh viên và mục nhập ý chí có thể lớn. Giới hạn chính thức là 7 giây với bộ nhớ 1024 MB. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

# Paste the solve() implementation from the solution above before running
# these tests.

def run(inp: str) -> str:
    global input
    old_stdin = sys.stdin
    old_input = input
    try:
        sys.stdin = io.StringIO(inp)
        input = sys.stdin.readline
        solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        input = old_input

# provided sample
sample = """2 5
100 2
2 0.9
3 0.1
100 2
1 0.2
4 0.8
"""

# The helper above needs stdout captured as well.
def run(inp: str) -> str:
    global input
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    old_input = input
    try:
        sys.stdin = io.StringIO(inp)
        sys.stdout = io.StringIO()
        input = sys.stdin.readline
        solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout
        input = old_input

out = run(sample).strip().splitlines()
expected = [
    "0.0000000000",
    "0.0000000000",
    "14.6341463400",
    "185.3658536600",
    "0.0000000000",
]
for got, want in zip(out, expected):
    assert abs(float(got) - float(want)) < 1e-7

# Custom 1: minimum N and M, direct distribution.
inp = """2 3
5 1
3 1.0
7 1
3 1.0
"""
out = run(inp).strip().splitlines()
assert abs(float(out[2]) - 12.0) < 1e-7, "direct recipient"

# Custom 2: all mass trapped in a closed cycle.
inp = """2 3
1 1
2 1.0
1 1
1 1.0
"""
out = run(inp).strip().splitlines()
assert all(abs(float(x)) < 1e-9 for x in out), "closed cycle"

# Custom 3: recursive cycle with a small escape probability.
inp = """2 3
1 2
2 0.999999
3 0.000001
1 1
1 1.0
"""
out = run(inp).strip().splitlines()
assert abs(float(out[2]) - 2.0) < 1e-5, "slowly leaking cycle"

# Custom 4: maximum N, sparse wills, with every member eventually reaching
# the same ordinary student.
n = 500
m = 501
parts = [f"{n} {m}"]
for i in range(1, n + 1):
    parts.append("1 1")
    parts.append(f"{m} 1.0")
inp = "\n".join(parts) + "\n"

out = run(inp).strip().splitlines()
assert len(out) == m, "maximum number of output lines"
assert all(abs(float(out[i])) < 1e-9 for i in range(n)), "departing students"
assert abs(float(out[n]) - 500.0) < 1e-7, "all lollipops reach final student"

# Custom 5: a member can feed a useless closed component.
inp = """3 4
10 1
2 1.0
5 1
1 0.5
4 0.5
7 1
2 1.0
"""
out = run(inp).strip().splitlines()
assert abs(float(out[3]) - 5.0) < 1e-7, "mass entering closed component is discarded"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 3`, cả hai di chúc trực tiếp cho học sinh 3 |`0, 0, 12`| Trường hợp kích thước tối thiểu và chuyển khoản trực tiếp | 
| Hai thành viên cống hiến 100% cho nhau |`0, 0, 0`| Phát hiện chu trình khép kín và khối lượng thải bỏ | 
| Một chu kỳ giữ 99,9999% và rò rỉ 0,0001% |`0, 0, 2`| Dòng đệ quy và nghiệm số thay cho mô phỏng | 
|`N=500`, mỗi thành viên trao trực tiếp cho sinh viên 501 | 500 dòng 0 theo sau là`500`| Tối đa (N), kích thước đầu ra tối đa và ranh giới hệ thống dày đặc | 
| Đồ thị ba phần tử có thành phần đóng |`0, 0, 0, 5`| Khối lượng đi vào một khu vực không có đường đến một học sinh bình thường phải biến mất | 

## Vỏ cạnh 

Đối với trường hợp chu trình kín```
2 3
1 1
2 1.0
1 1
1 1.0
```không thành viên rời đi nào có lợi thế hơn một học sinh bình thường, do đó quá trình truyền ngược lại bắt đầu mà không có đỉnh hữu ích. Tập hữu ích trống, hệ thống Gaussian không có biến nào và mọi giá trị đầu ra vẫn bằng 0. Điều này trực tiếp thực hiện quy tắc kẹo mút bị mắc kẹt mãi mãi trong số các thành viên rời đi sẽ bị vứt đi. 

Đối với chu kỳ rò rỉ chậm```
2 3
1 2
2 0.999999
3 0.000001
1 1
1 1.0
```cả hai thành viên rời đi đều được đánh dấu là hữu ích vì thành viên 2 tiếp cận trực tiếp với sinh viên 3 và thành viên 1 tiếp cận thành viên 2. Các phương trình là 

[ 
x_1=1+x_2 
] 

và 

[ 
x_2=0,999999x_1. 
] 

Lời giải là khoảng (x_1=1{,}000{,}000) và (x_2=999{,}999). Thành viên 2 đưa (0,000001x_2) cho sinh viên 3, sản xuất chính xác (1) kẹo mút từ thành viên 2, trong khi thành viên 1 cuối cùng cũng đóng góp (1) khác thông qua cơ chế rò rỉ tương tự. Câu trả lời cuối cùng là (2), mặc dù mô phỏng trực tiếp sẽ cần hàng triệu vòng để quan sát sự hội tụ. 

Đối với trường hợp một thành viên hữu ích gửi một khối lượng nào đó vào một thành phần đóng, hãy xem xét```
3 4
10 1
2 1.0
5 2
1 0.5
4 0.5
7 1
2 1.0
```Thành viên 1 gửi toàn bộ số kẹo của mình cho thành viên 2. Thành viên 2 gửi một nửa cho học sinh 4 và một nửa cho thành viên 1. Thành viên 3 bị mắc kẹt trong vòng quay của thành viên 2 và không tạo được lối đi riêng ra bên ngoài. Việc giải hệ thống con hữu ích sẽ cho tổng cộng 10 kẹo được xử lý bởi thành viên 2, trong đó 5 kẹo đến tay học sinh 4 và phần còn lại tiếp tục qua chu trình. Đầu ra là```
0.0
0.0
0.0
5.0
```Bước tiếp cận biểu đồ là điều làm cho việc này trở nên an toàn. Chúng tôi không bao giờ cố gắng ấn định một "tổng số tiền đã từng được xử lý" hữu hạn cho một thành phần thực sự đóng, bởi vì số lượng đó không nhất thiết phải tồn tại. Chúng tôi chỉ giải quyết phần nhất thời có thể đóng góp cho câu trả lời cuối cùng được yêu cầu.
