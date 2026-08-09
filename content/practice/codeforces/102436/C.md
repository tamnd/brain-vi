---
title: "CF 102436C - Kế hoạch sơn"
description: "Chúng ta có (n) đoạn trên trục số. Các điểm cuối ban đầu đã bị mất theo cặp. Chúng tôi chỉ biết tất cả (2n) tọa độ điểm cuối, được sắp xếp thành một mảng (x1 < x2 < dots < x{2n}) và chúng tôi biết rằng phần kết của các đoạn ban đầu có tổng chiều dài chính xác (k)."
date: "2026-08-09T00:10:50+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102436
codeforces_index: "C"
codeforces_contest_name: "Innopolis Open 2019-2020, qualification, contest 1"
rating: 0
weight: 102436
solve_time_s: 132
verified: true
draft: false
---

[CF 102436C - Kế hoạch sơn](https://codeforces.com/problemset/problem/102436/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 12s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có (n) đoạn trên trục số. Các điểm cuối ban đầu đã bị mất theo cặp. Chúng tôi chỉ biết tất cả (2n) tọa độ điểm cuối, được sắp xếp thành một mảng (x_1 < x_2 < \dots < x_{2n}) và chúng tôi biết rằng phần kết của các phân đoạn ban đầu có tổng chiều dài chính xác (k). 

Chúng ta phải tái cấu trúc bất kỳ cặp tọa độ (2n) nào thành (n) đoạn có phần hợp có độ dài (k). Đầu ra sử dụng các chỉ số của mảng điểm cuối được sắp xếp chứ không phải chính tọa độ. Nếu không có cặp nào tạo ra độ dài kết hợp được yêu cầu, chúng tôi sẽ in`No`. Nếu không chúng tôi in`Yes`theo sau là các cặp chỉ số (n). Tuyên bố ban đầu đảm bảo rằng tất cả các tọa độ đều khác biệt. 

Các ràng buộc là chìa khóa cho giải pháp dự định. Có thể có tới 7000 phân đoạn, do đó có 14000 điểm cuối. Độ dài liên kết mục tiêu (k) tối đa là 30000. Một DP có trạng thái chỉ phụ thuộc vào vị trí hiện tại và độ dài lên tới (k) là hợp lý, trong khi bất kỳ số mũ nào trong (n) ngay lập tức là không thể. Giải pháp dự định sử dụng cấu trúc của các điểm cuối được sắp xếp để biến quá trình tái thiết thành DP kiểu ba lô và kích thước trạng thái lớn được xử lý bằng các tập hợp bit. 

Trường hợp cạnh đầu tiên là mục tiêu nhỏ hơn liên kết tối thiểu có thể có. Ví dụ,```
2 1
0 1 2 3
```Bốn điểm cuối có thể được ghép thành ([0,1]) và ([2,3]), tạo ra độ dài kết hợp (2). Mọi ghép nối khác đều tạo ra một liên kết có độ dài ít nhất là (2), vì vậy đầu ra đúng là`No`. Việc triển khai bất cẩn chỉ kiểm tra xem (k) có thể được hình thành dưới dạng chênh lệch tùy ý giữa các tọa độ hay không có thể chấp nhận nó một cách không chính xác. 

Trường hợp cạnh thứ hai chính xác là độ dài hợp tối thiểu. Vì```
2 2
0 1 2 3
```ghép nối liền kề ([0,1]), ([2,3]) cho độ dài kết hợp (2), vì vậy câu trả lời là`Yes`. Một giải pháp giả định rằng một số khoảng phải giao nhau có thể bỏ lỡ cấu hình hoàn toàn rời rạc này. 

Trường hợp cạnh thứ ba là một cấu hình lồng nhau hoàn toàn. Vì```
2 3
0 1 2 3
```các đoạn ([0,3]) và ([1,2]) có độ dài hợp (3). Các điểm cuối phải được ghép thành ((1,4)) và ((2,3)). Việc ghép nối các điểm cuối liền kề sẽ cho độ dài (2), do đó, chỉ lấy số lượng ghép nối tối thiểu là không đủ. 

Trường hợp cạnh thứ tư là (k=0). Vì tất cả các tọa độ đều khác nhau nên mỗi đoạn đều có độ dài dương, do đó, phần hợp của một đoạn chẵn có độ dài dương. Như vậy```
1 0
0 1
```phải sản xuất`No`. Việc triển khai coi số 0 là trạng thái DP trống mà không kiểm tra xem mọi điểm cuối cuối cùng có phải được sử dụng hay không sẽ mắc lỗi này. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp nhất là thử mọi khả năng ghép nối các điểm cuối (2n). Có ((2n-1)!!) các cặp khác nhau. Đối với mỗi cặp, chúng ta có thể sắp xếp các khoảng kết quả theo điểm cuối bên trái của chúng và tính độ dài liên kết của chúng theo (O(n)). Tổng công là (O(n(2n-1)!!)). Thậm chí (n=20) đã vượt xa giới hạn thực tế, trong khi ràng buộc thực tế là (n=7000). Lực lượng vũ phu là chính xác vì nó kiểm tra mọi khả năng tái tạo theo đúng nghĩa đen, nhưng không gian tìm kiếm của nó phát triển quá nhanh. 

Có một cách mạnh mẽ hơn nhiều để xem xét các điểm cuối được sắp xếp. 

Giả sử chúng ta ghép tạm thời mọi cặp liền kề: 

[ 
(x_1,x_2),(x_3,x_4),\ldots,(x_{2n-1},x_{2n}). 
] 

Các khoảng này rời rạc và mang lại sự kết hợp nhỏ nhất có thể cho các điểm cuối này. Gọi độ dài của chúng 

[ 
a_i=x_{2i+2}-x_{2i+1} 
] 

sử dụng lập chỉ mục dựa trên số không. 

Bây giờ hãy xem xét hai cặp liên tiếp. Thay vì giữ 

[ 
(x_{2i},x_{2i+1}),\quad(x_{2i+2},x_{2i+3}), 
] 

chúng ta có thể hợp nhất liên kết của chúng thành một thành phần được kết nối bằng cách ghép bốn điểm cuối theo thứ tự lồng nhau: 

[ 
(x_{2i},x_{2i+3}),\quad(x_{2i+1},x_{2i+2}). 
] 

Sự kết hợp khi đó chỉ đơn giản là khoảng cách từ điểm cuối đầu tiên đến điểm cuối cuối cùng. Số tiền đóng góp thêm khi cặp tiếp theo được hấp thụ là 

[ 
b_i=x_{2i+1}-x_{2i-1} 
] 

với (i>0). 

Điều này mang lại một sự trình bày đặc biệt thuận tiện. Mọi thành phần được kết nối của liên kết cuối cùng đều chứa một khối tọa độ điểm cuối liên tiếp. Vì mỗi điểm cuối được sử dụng chính xác một lần nên một thành phần chứa (2m) điểm cuối có thể được biểu diễn bằng cách lồng các điểm cuối đó. Do đó, toàn bộ quá trình tái thiết có thể được xem như việc phân chia (n) cặp điểm cuối liền kề thành các khối liên tiếp. 

Đối với một khối bao gồm các chỉ số cặp (l) đến (r), liên kết của nó là 

[ 
[x_{2l},x_{2r+1}], 
] 

chiều dài của nó là 

a_l+b_{l+1}+b_{l+2}+\cdots+b_r. 
] 

Vì vậy, khi quét các chỉ số cặp từ trái sang phải, có đúng hai lựa chọn. Chúng ta có thể bắt đầu một thành phần mới, thanh toán (a_i) hoặc mở rộng thành phần hiện tại, thanh toán (b_i). 

Đây bây giờ là một vấn đề kiểu ba lô 0/1. Tại vị trí (i), DP ghi lại tổng chiều dài có thể tiếp cận được. Dung lượng mục tiêu là (k), tối đa là 30000. Giải pháp được xuất bản sử dụng bộ bit C++ vì lý do chính xác này. 

Khó khăn cuối cùng là việc tái thiết. Một DP boolean bình thường sẽ cần nhớ phần trước cho mỗi cặp vị trí và độ dài, đó là bộ nhớ (O(nk)). Thay vào đó, chúng tôi giữ một bitset cho mỗi vị trí cho biết trạng thái có thể truy cập nào đã sử dụng`extend`chuyển tiếp. Trong Python, một số nguyên có độ chính xác tùy ý hoạt động như một tập bit nhỏ gọn, do đó một số nguyên có thể biểu thị tất cả các trạng thái (k+1) cùng một lúc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n(2n-1)!!)) | (O(n)) | Quá chậm | 
| DP thông thường | (O(nk)) | (O(nk)) để tái thiết | Python quá chậm, tốn nhiều bộ nhớ | 
| Bitset DP | (O(nk/W)) thao tác từ | (O(nk/W)) để tái thiết | Đã chấp nhận | 

Ở đây (W) là số bit được xử lý bởi một thao tác bitset cấp từ máy. Trong Python, ý tưởng tương tự được thực hiện bởi các số nguyên có độ chính xác tùy ý, có các phép dịch và phép toán theo bit xử lý đồng thời nhiều trạng thái DP. 

## Hướng dẫn thuật toán 

1. Tính độ dài (a_i=x_{2i+1}-x_{2i}) của cặp liền kề cơ bản tại vị trí (i). Đồng thời tính toán (b_i=x_{2i+1}-x_{2i-1}) cho mọi (i>0). Bắt đầu một thành phần với (i) chi phí (a_i), đồng thời mở rộng thành phần đó thông qua (i) chi phí (b_i). 
2. Biểu thị các trạng thái DP có thể truy cập dưới dạng một số nguyên`dp`. (Các) bit được đặt chính xác khi có thể đạt được tổng độ dài kết hợp sau khi xử lý tiền tố hiện tại. Ban đầu chỉ có thể truy cập được độ dài bằng 0, vì vậy`dp = 1`. 
3. Tại vị trí (i), dịch chuyển`dp`để lại bởi (a_i) biểu thị việc bắt đầu một thành phần mới tại (i). Dịch chuyển nó sang trái bằng (b_i) thể hiện việc mở rộng thành phần từ cặp trước đó sang (i). Sự kết hợp của hai bitset này mang lại trạng thái có thể truy cập mới. 
4. Lưu trữ các trạng thái mới có thể truy cập đến từ`b_i`chuyển tiếp. Nếu cả hai quá trình chuyển đổi có thể đạt đến cùng một trạng thái, hãy ưu tiên`a_i`chuyển tiếp. Việc phá vỡ ràng buộc tùy ý này rất hữu ích vì nó có nghĩa là một bit được lưu trữ là đủ để xác định phần trước trong quá trình tái cấu trúc. 
5. Sau khi tất cả (n) vị trí đã được xử lý, hãy kiểm tra bit (k). Nếu nó không được đặt thì không tồn tại phân vùng hợp lệ của chuỗi điểm cuối, vì vậy hãy in`No`. 
6. Nếu bit (k) được đặt, hãy đi lùi qua DP. Tại vị trí (i), kiểm tra xem dữ liệu được lưu trữ có`b_i`bit được đặt cho độ dài mục tiêu hiện tại. Nếu đúng như vậy thì thành phần hiện tại đã được mở rộng, vì vậy hãy trừ (b_i). Nếu không nó đã được bắt đầu ở đây, vì vậy hãy trừ (a_i). 
7. Các lựa chọn được khôi phục sẽ chia (n) cặp điểm cuối liền kề thành các khối liên tiếp. Đối với mỗi khối ([l,r]), hãy ghép các điểm cuối của nó theo thứ tự lồng nhau. Đoạn ngoài cùng sử dụng các vị trí (2l) và (2r+1), các đoạn tiếp theo sử dụng (2l+1) và (2r), v.v. 
8. Chuyển đổi các vị trí điểm cuối dựa trên 0 thành các chỉ số dựa trên một yêu cầu và in các phân đoạn (n) kết quả. 

### Tại sao nó hoạt động 

Bất biến trung tâm là mọi sự kết hợp có thể có của các phân đoạn ban đầu có thể được biểu diễn bằng cách sử dụng các thành phần được kết nối liên tiếp của chuỗi điểm cuối được sắp xếp. Trong một thành phần, việc lồng các điểm cuối sẽ tạo ra khoảng cách chính xác của thành phần đó, trong khi các thành phần khác vẫn rời rạc. Do đó, việc tái cấu trúc hợp lệ tương đương với việc phân chia (n) cặp điểm cuối liền kề thành các khối liên tiếp. 

Đối với một khối bắt đầu tại (l) và kết thúc tại (r), độ dài hợp của nó chính xác là (a_l+\sum_{i=l+1}^{r}b_i). DP xem xét chính xác hai khả năng ở mọi vị trí: bắt đầu một khối mới hoặc mở rộng khối trước đó. Do đó, mọi đường dẫn DP tương ứng với một phân vùng khối hợp lệ và mọi phân vùng khối hợp lệ tương ứng với một đường dẫn DP. Nếu bit (k) có thể truy cập được thì các lựa chọn được xây dựng lại sẽ tạo ra sự kết hợp chính xác của (k); nếu không thể truy cập được thì không có bản dựng lại hợp lệ nào tồn tại. 

## Giải pháp Python```python
import sys

input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    x = list(map(int, input().split()))

    # a[i] is the length of the basic adjacent pair.
    a = [x[2 * i + 1] - x[2 * i] for i in range(n)]

    # b[i] is the cost of extending the current component
    # from pair i-1 through pair i.
    b = [0] * n
    for i in range(1, n):
        b[i] = x[2 * i + 1] - x[2 * i - 1]

    # Bit s of dp means that total length s is reachable.
    dp = 1

    # For each i, bit s of extend[i] means that state s
    # was reached by taking the b[i] transition.
    extend = [0] * n

    mask = (1 << (k + 1)) - 1

    for i in range(n):
        from_start = dp << a[i]

        if i == 0:
            from_extend = 0
            chosen_extend = 0
        else:
            from_extend = dp << b[i]

            # If both transitions reach the same state, prefer
            # starting a new component.
            chosen_extend = from_extend & ~from_start

        dp = (from_start | from_extend) & mask
        extend[i] = chosen_extend & mask

    if not ((dp >> k) & 1):
        print("No")
        return

    # Reconstruct whether each position extends the previous block.
    used_extend = [False] * n
    cur = k

    for i in range(n - 1, -1, -1):
        if i > 0 and ((extend[i] >> cur) & 1):
            used_extend[i] = True
            cur -= b[i]
        else:
            cur -= a[i]

    # Convert the sequence of choices into consecutive blocks.
    blocks = []
    start = 0

    for i in range(1, n):
        if not used_extend[i]:
            blocks.append((start, i - 1))
            start = i

    blocks.append((start, n - 1))

    # Construct nested pairs inside every block.
    answer = []

    for l, r in blocks:
        while l <= r:
            # Zero-based endpoint positions:
            # 2*l and 2*r+1.
            answer.append((2 * l + 1, 2 * r + 2))
            l += 1
            r -= 1

    print("Yes")
    for u, v in answer:
        print(u, v)

if __name__ == "__main__":
    solve()
```Phần đầu tiên xây dựng`a`Và`b`. giá trị`a[i]`là chi phí để biến cặp (i) thành một thành phần được kết nối riêng biệt. giá trị`b[i]`là chi phí của việc gắn cặp (i) vào thành phần ngay trước nó. 

biểu hiện`dp << a[i]`dịch chuyển mọi tổng số có thể đạt được bằng chi phí khởi động một thành phần. Tương tự như vậy,`dp << b[i]`đại diện cho việc mở rộng thành phần hiện tại. Bởi vì tất cả các chi phí đều dương, các trạng thái lớn hơn (k) không bao giờ có thể trở nên hữu ích sau này, vì vậy mặt nạ sẽ loại bỏ chúng một cách an toàn. 

biểu hiện`from_extend & ~from_start`chỉ ghi lại trạng thái phần mở rộng nào là tiền thân hợp lệ nhưng việc bắt đầu một thành phần mới thì không. Điều này là đủ để tái thiết vì khi cả hai lựa chọn đều có thể thực hiện được thì việc triển khai sẽ cố tình chọn quá trình chuyển đổi bắt đầu. 

Việc xây dựng lại ngược sẽ loại bỏ chính xác quá trình chuyển đổi đã tạo ra trạng thái hiện tại. giá trị`cur`do đó di chuyển qua các trạng thái trước đó hợp lệ cho đến khi nó đạt đến số 0. 

Việc xây dựng khối sử dụng các vị trí điểm cuối dựa trên số 0 trong nội bộ. Đối với khối từ cặp (l) đến cặp (r), đoạn ngoài cùng kết nối các vị trí điểm cuối (2l) và (2r+1). Thêm một vào cả hai vị trí sẽ cung cấp các chỉ số dựa trên một yêu cầu. Di chuyển vào trong sẽ tạo ra tất cả các phân đoạn lồng nhau còn lại. 

Không thể tràn số nguyên trong Python. Trong C++, các ràng buộc ban đầu cũng phù hợp thoải mái với các số nguyên 32 bit thông thường cho tọa độ và độ dài mục tiêu. Chi phí bộ nhớ chính của việc triển khai Python là`extend`mảng, chứa (n) tập hợp bit có độ chính xác tùy ý có tối đa (k+1) bit. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là```
4 9
0 1 3 5 8 9 10 12
```Độ dài cặp liền kề là 

[ 
a=[1,2,1,2], 
] 

và chi phí mở rộng là 

[ 
b=[0,4,4,3]. 
] 

DP phát triển như sau. 

| Vị trí | Chi phí ban đầu (a_i) | Chi phí gia hạn (b_i) | Tổng số có thể tiếp cận sau vị trí | 
| --- | --- | --- | --- | 
| 0 | 1 | không có sẵn | {1} | 
| 1 | 2 | 4 | {3, 5} | 
| 2 | 1 | 4 | {4, 6, 7, 9} | 
| 3 | 2 | 3 | {6, 7, 8, 9, 10, 11, 12} | 

Mục tiêu (9) có thể đạt được. Một công trình xây dựng lại hợp lệ là`start, start, extend, start`, cho các khối ([0,0]), ([1,2]) và ([3,3]). 

Khối lồng nhau tương ứng ([1,2]) sử dụng các chỉ số điểm cuối (3,6) và (4,5). Vì vậy, một câu trả lời hợp lệ là```
Yes
1 2
3 6
4 5
7 8
```Chúng đại diện cho các khoảng ([0,1]), ([3,9]), ([5,8]) và ([10,12]), có liên kết có độ dài (9). Mẫu chính thức sử dụng thứ tự hợp lệ khác của cùng phân khúc. 

Dấu vết thể hiện sự bất biến quan trọng: việc chọn`extend`nối các cặp cơ bản lân cận thành một thành phần được kết nối, trong khi chọn`start`đóng thành phần trước đó và bắt đầu thành phần khác. 

### Mẫu 2 

Đầu vào là```
3 2
1 2 3 4 5 6
```đây 

[ 
a=[1,1,1] 
] 

và 

[ 
b=[0,2,2]. 
] 

DP phát triển như sau. 

| Vị trí | Chi phí ban đầu (a_i) | Chi phí gia hạn (b_i) | Tổng số có thể tiếp cận sau vị trí | 
| --- | --- | --- | --- | 
| 0 | 1 | không có sẵn | {1} | 
| 1 | 1 | 2 | {2, 3} | 
| 2 | 1 | 2 | {3, 4} | 

Mục tiêu (2) biến mất sau lần chuyển đổi thứ hai và không thể truy cập được sau khi cả ba cặp được xử lý. Do đó, đầu ra chính xác là```
No
```Ví dụ này cho thấy tại sao chỉ kiểm tra phép kết tối thiểu có thể là không đủ. Mức tối thiểu là (3) cho ba đoạn liền kề rời nhau, trong khi một số tổng trung gian có thể không thực hiện được. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(nk/W)) hoạt động bit | Mỗi vị trí trong số (n) thực hiện một số lần dịch chuyển và thao tác theo bit không đổi trên các bit (k+1). | 
| Không gian | (O(nk/W)) | Một bitset được lưu trữ cho mọi vị trí để xây dựng lại các chuyển đổi đã chọn. | 

Ở đây (n\le7000) và (k\le30000), do đó, mỗi bit được lưu trữ chỉ chứa khoảng 30001 bit, khoảng 3,75 KB trước chi phí số nguyên Python. Toàn bộ lịch sử của phiên bản tiền nhiệm có dung lượng khoảng vài chục megabyte, thấp hơn giới hạn bộ nhớ 512 MB. DP song song bit tránh được các bản cập nhật trạng thái cấp Python riêng lẻ khoảng (7000\times30000) mà một DP vòng lặp lồng nhau thông thường sẽ yêu cầu. Các ràng buộc và giới hạn được đưa ra trong tuyên bố ban đầu. 

## Trường hợp thử nghiệm 

Bộ dây thử nghiệm bên dưới kiểm tra các mẫu chính thức và một số vỏ kết cấu. Vì câu trả lời mang tính xây dựng nên nó không so sánh danh sách phân khúc chính xác. Thay vào đó, nó xác minh rằng chương trình in đúng`Yes`hoặc`No`, sử dụng mọi điểm cuối chính xác một lần, giữ cho mọi phân đoạn được định hướng từ trái sang phải và tạo ra độ dài kết hợp được yêu cầu.```python
import sys
import io

def solve_instance(n, k, x):
    a = [x[2 * i + 1] - x[2 * i] for i in range(n)]

    b = [0] * n
    for i in range(1, n):
        b[i] = x[2 * i + 1] - x[2 * i - 1]

    dp = 1
    extend = [0] * n
    mask = (1 << (k + 1)) - 1

    for i in range(n):
        from_start = dp << a[i]

        if i == 0:
            from_extend = 0
            chosen_extend = 0
        else:
            from_extend = dp << b[i]
            chosen_extend = from_extend & ~from_start

        dp = (from_start | from_extend) & mask
        extend[i] = chosen_extend & mask

    if not ((dp >> k) & 1):
        return "No\n"

    used_extend = [False] * n
    cur = k

    for i in range(n - 1, -1, -1):
        if i > 0 and ((extend[i] >> cur) & 1):
            used_extend[i] = True
            cur -= b[i]
        else:
            cur -= a[i]

    blocks = []
    start = 0

    for i in range(1, n):
        if not used_extend[i]:
            blocks.append((start, i - 1))
            start = i

    blocks.append((start, n - 1))

    answer = []

    for l, r in blocks:
        while l <= r:
            answer.append((2 * l + 1, 2 * r + 2))
            l += 1
            r -= 1

    out = ["Yes"]
    out.extend(f"{u} {v}" for u, v in answer)
    return "\n".join(out) + "\n"

def run(inp: str) -> str:
    data = list(map(int, inp.split()))
    n, k = data[0], data[1]
    x = data[2:2 + 2 * n]
    return solve_instance(n, k, x)

def verify(inp: str, out: str):
    data = list(map(int, inp.split()))
    n, k = data[0], data[1]
    x = data[2:2 + 2 * n]

    lines = out.strip().splitlines()

    if lines[0] == "No":
        return

    assert lines[0] == "Yes"
    assert len(lines) == n + 1

    used = [False] * (2 * n)
    intervals = []

    for line in lines[1:]:
        u, v = map(int, line.split())
        assert 1 <= u <= 2 * n
        assert 1 <= v <= 2 * n
        assert u < v

        u -= 1
        v -= 1

        assert not used[u]
        assert not used[v]

        used[u] = True
        used[v] = True
        intervals.append((x[u], x[v]))

    assert all(used)

    intervals.sort()

    total = 0
    left, right = intervals[0]

    for l, r in intervals[1:]:
        if l > right:
            total += right - left
            left, right = l, r
        else:
            right = max(right, r)

    total += right - left

    assert total == k

# Official sample 1
sample1 = """\
4 9
0 1 3 5 8 9 10 12
"""
out = run(sample1)
verify(sample1, out)
assert out.splitlines()[0] == "Yes"

# Official sample 2
sample2 = """\
3 2
1 2 3 4 5 6
"""
out = run(sample2)
assert out.strip() == "No"

# Minimum-size input
case_min = """\
1 4
0 4
"""
out = run(case_min)
verify(case_min, out)
assert out.splitlines()[0] == "Yes"

# Minimum possible union, all adjacent gaps equal
case_equal = """\
4 4
0 1 2 3 4 5 6 7
"""
out = run(case_equal)
verify(case_equal, out)
assert out.splitlines()[0] == "Yes"

# Nested configuration, catches block construction
case_nested = """\
2 3
0 1 2 3
"""
out = run(case_nested)
verify(case_nested, out)
assert out.splitlines()[0] == "Yes"

# Impossible value below the minimum
case_too_small = """\
2 1
0 1 2 3
"""
out = run(case_too_small)
assert out.strip() == "No"

# Maximum-size input from the official constraints
n = 7000
x = list(range(2 * n))
case_max = f"{n} {n}\n" + " ".join(map(str, x)) + "\n"
out = run(case_max)
verify(case_max, out)
assert out.splitlines()[0] == "Yes"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 4 / 0 4`|`Yes`| Tối thiểu (n), phân đoạn đơn | 
|`4 4 / 0 1 2 3 4 5 6 7`|`Yes`| Khoảng cách liền kề bằng nhau và liên kết tối thiểu | 
|`2 3 / 0 1 2 3`|`Yes`| Xây dựng thành phần lồng nhau | 
|`2 1 / 0 1 2 3`|`No`| Nhắm mục tiêu dưới mức liên minh tối thiểu có thể | 
|`7000 7000 / 0 1 ... 13999`|`Yes`| Tối đa (n), số lượng vị trí DP tối đa | 

## Vỏ cạnh 

Đối với mục tiêu nhỏ hơn liên kết tối thiểu, DP đương nhiên sẽ từ chối phiên bản đó. Coi như```
2 1
0 1 2 3
```Chi phí cơ bản liền kề là (a=[1,1]), trong khi chi phí mở rộng là (b_1=2). Tổng số duy nhất có thể truy cập là (1), (2) và (3), do đó bit (1) không được đặt sau khi xử lý cả hai cặp. Thuật toán in`No`. 

Để có mục tiêu chính xác bằng liên kết tối thiểu, hãy xem xét```
2 2
0 1 2 3
```DP có thể chọn`start`hai lần, cho (a_0+a_1=1+1=2). Các khối được xây dựng lại là ([0,0]) và ([1,1]), tạo ra các phân đoạn có chỉ số`(1,2)`Và`(3,4)`. Sự kết hợp của họ có chiều dài (2). 

Đối với cấu hình lồng nhau, hãy xem xét```
2 3
0 1 2 3
```DP có thể chọn`start`cho cặp đầu tiên và`extend`cho cặp thứ hai. Chi phí trở nên 

[ 
a_0+b_1=1+2=3. 
] 

Khối kết quả là ([0,1]). Cấu trúc lồng nhau của nó tạo ra`(1,4)`Và`(2,3)`, tương ứng với ([0,3]) và ([1,2]). Hợp của chúng chính xác là ([0,3]), có độ dài (3). 

Với (k=0), hãy xem xét```
1 0
0 1
```Chi phí chuyển tiếp khả dụng duy nhất (1), do đó, trạng thái duy nhất có thể truy cập là độ dài (1), không phải độ dài (0) sau khi tất cả các điểm cuối đã được xử lý. Do đó, thuật toán in`No`. Thực tế là trạng thái DP ban đầu chứa số 0 không có nghĩa là cấu trúc trống là một câu trả lời hợp lệ, bởi vì mọi điểm cuối phải được sử dụng chính xác trong một phân đoạn. 

Cuối cùng, trường hợp kích thước tối đa nhấn mạnh lý do sử dụng bitset. Với (n=7000), một vòng lặp Python thông thường sẽ kiểm tra tới (7000\cdot30001) hoặc khoảng 210 triệu trạng thái DP. Việc biểu diễn bitset xử lý đồng thời tất cả các độ dài có thể bằng cách sử dụng các phép dịch số nguyên và các phép toán theo bit, đây là sự tối ưu hóa trung tâm làm cho ràng buộc đầy đủ trở nên thực tế. Bài toán ban đầu đặt chính xác các giá trị lớn nhất này cho (n) và (k). 

Nếu bạn muốn, tôi cũng có thể biến nó thành một bài xã luận kiểu Codeforces nhỏ gọn hơn hoặc cung cấp phiên bản C++17 phù hợp hơn với giải pháp bitset chính thức.
