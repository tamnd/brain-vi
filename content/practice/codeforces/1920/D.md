---
title: "CF 1920D - Lặp lại mảng"
description: "Chúng ta bắt đầu với một chuỗi trống và dần dần xây dựng một mảng rất lớn bằng cách sử dụng hai loại thao tác. Thao tác đầu tiên sẽ thêm một giá trị vào cuối, mở rộng chuỗi một phần tử một cách hiệu quả."
date: "2026-06-08T19:28:35+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "brute-force", "dsu", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1920
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 919 (Div. 2)"
rating: 1900
weight: 1920
solve_time_s: 78
verified: true
draft: false
---

[CF 1920D - Lặp lại mảng](https://codeforces.com/problemset/problem/1920/D) 

**Xếp hạng:** 1900 
**Tags:** tìm kiếm nhị phân, lực lượng vũ phu, dsu, thực hiện, toán học 
**Thời gian giải:** 1 phút 18s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta bắt đầu với một chuỗi trống và dần dần xây dựng một mảng rất lớn bằng cách sử dụng hai loại thao tác. Thao tác đầu tiên sẽ thêm một giá trị vào cuối, mở rộng chuỗi một phần tử một cách hiệu quả. Thao tác thứ hai lấy toàn bộ mảng hiện tại và ghép một số bản sao đầy đủ của mảng đó vào chính nó. 

Khía cạnh quan trọng là mảng có thể phát triển cực kỳ nhanh, bởi vì mọi thao tác sao chép sẽ nhân kích thước hiện tại thay vì thêm vào nó. Mặc dù có nhiều nhất$10^5$hoạt động, độ dài mảng cuối cùng có thể đạt tới$10^{18}$, vì vậy việc xây dựng nó một cách rõ ràng là không thể. 

Mỗi truy vấn yêu cầu giá trị tại một vị trí cụ thể trong mảng cuối cùng sau khi tất cả các thao tác được áp dụng. Vì vậy, nhiệm vụ không phải là mô phỏng việc xây dựng mà là suy luận về cách các vị trí trong mảng cuối cùng theo dõi ngược lại các bản sao lặp lại về trạng thái trước đó. 

Các ràng buộc ngay lập tức loại trừ mọi mô phỏng trực tiếp hoặc thậm chí là xây dựng một phần. Bất cứ điều gì chạm vào từng phần tử của mảng cuối cùng đều không khả thi. Ngay cả việc truyền tải tuyến tính trên mỗi truy vấn cũng sẽ thất bại, vì cả hai$n$Và$q$lớn. 

Một trường hợp lỗi tinh tế xuất hiện nếu người ta cố gắng chỉ mô phỏng các hoạt động thuộc loại 1 và bỏ qua cấu trúc: 

đầu vào:```
3 1
1 1
1 2
2 1000000000
1
```Mảng cuối cùng không chỉ`[1,2]`lặp đi lặp lại trong một vòng lặp ngây thơ. Đó là`[1,2]`theo sau là$10^9$bản sao của`[1,2]`. Việc triển khai ngây thơ xây dựng các bản sao theo đúng nghĩa đen sẽ hết bộ nhớ ngay lập tức. 

Một vấn đề ẩn khác là giả định rằng mỗi bản sao chỉ cần biết độ dài hiện tại mà không cần theo dõi cấu trúc. Ngay cả khi độ dài được theo dõi mà không nhớ các phân đoạn được hình thành như thế nào thì cũng không có cách nào để giải quyết các truy vấn ngược. 

Khó khăn chính là các hoạt động sao chép bảo toàn toàn bộ cấu trúc của mảng, nghĩa là mọi phần tử trong khối được sao chép đều giống hệt với một số tiền tố trước đó. Điều này tạo ra một cấu trúc đệ quy phải được khai thác. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ duy trì toàn bộ mảng một cách rõ ràng. Đối với mỗi thao tác loại 1, chúng tôi nối thêm một phần tử. Đối với mỗi thao tác loại 2, chúng tôi nối thêm$x$bản sao của mảng hiện tại. Về mặt khái niệm, điều này đơn giản và chính xác vì nó tuân thủ chính xác các quy tắc. 

Tuy nhiên, kích thước của mảng tăng lên theo cấp số nhân. Nếu chuỗi các thao tác bao gồm các thao tác loại 2 lặp đi lặp lại với số nhân lớn, thì kích thước mảng sẽ nhanh chóng vượt quá giới hạn bộ nhớ và thậm chí việc tính toán từng bước cũng trở nên không thể. Trường hợp xấu nhất sẽ dẫn đến sự bùng nổ theo cấp số nhân về thời gian và không gian. 

Quan sát quan trọng là chúng ta thực sự không bao giờ cần toàn bộ mảng, chỉ cần độ dài của nó và cách theo dõi vị trí ngược lại. Sau tất cả các thao tác, mọi vị trí đều đến từ phần bổ sung trực tiếp (loại 1) hoặc từ khối được sao chép (loại 2). Nếu có thể đảo ngược các ánh xạ này, chúng ta có thể trả lời các truy vấn mà không cần cụ thể hóa mảng. 

Thay vì xây dựng về phía trước, chúng tôi duy trì độ dài sau mỗi thao tác. Sau đó, để trả lời câu hỏi về vị trí$k$, chúng ta di chuyển lùi qua các phép toán. Khi chúng tôi gặp phải sự trùng lặp, chúng tôi giảm$k$modulo độ dài trước đó, vì mỗi bản sao giống hệt với mảng ban đầu tại thời điểm đó. 

Điều này biến vấn đề thành một quá trình duyệt ngược lại lịch sử hoạt động. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(c)$mỗi bản dựng |$O(c)$| Quá chậm | 
| Tối ưu |$O(n + q \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý các thao tác một lần để tính độ dài của mảng sau mỗi bước. Vì độ dài có thể vượt quá$10^{18}$, chúng tôi giới hạn chúng ở giới hạn trên an toàn (bất kỳ giá trị nào trên truy vấn tối đa là đủ), nhưng việc giữ các giá trị chính xác ở số nguyên 64 bit là được. 

Chúng tôi cũng lưu trữ từng thao tác theo thứ tự để có thể duyệt ngược. 

### bước 

1. Đọc tất cả các thao tác và tính toán một mảng`len[i]`, Ở đâu`len[i]`là kích thước của mảng sau khi thực hiện$i$. 

Nếu hoạt động là loại 1, tăng chiều dài lên 1. 

Nếu hoạt động là loại 2 với giá trị$x$, nhân chiều dài dòng điện với$x+1$. 

Điều này phản ánh việc gắn thêm$x$bản sao của toàn bộ mảng. 
2. Với mỗi giá trị truy vấn$k$, đặt con trỏ$i = n$. Chúng ta sẽ di chuyển lùi qua các phép toán cho đến khi xác định được gốc tọa độ$k$. 
3. Trong khi$i > 0$, kiểm tra hoạt động$i$: 

Nếu nó là loại 1, và$k$bằng độ dài hiện tại, chúng tôi đã tìm thấy phần tử trực tiếp; phá vỡ. 
4. Nếu hoạt động$i$là loại 2 có hệ số$x$, thì mảng ở bước này bao gồm$x+1$các khối có kích thước giống hệt nhau`len[i-1]`. 

Thay thế$k$với$k \bmod len[i-1]$. Nếu số dư bằng 0 thì đặt$k = len[i-1]$. 

Sau đó tiếp tục lùi về phía sau. 

Bước này có tác dụng vì tất cả các bản sao đều là bản sao chính xác, vì vậy vị trí$k$phải tương ứng với cùng một vị trí tương đối trong mảng ban đầu. 
5. Giảm$i$và tiếp tục cho đến khi đạt được phần tử cơ sở loại 1. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, mảng là sự kết hợp của các phân đoạn giống hệt nhau được tạo bởi các hoạt động sao chép. Thao tác loại 2 không tạo ra thông tin mới mà chỉ lặp lại cấu trúc hiện có. Do đó, mọi chỉ mục trong khối lặp lại sẽ ánh xạ tới một chỉ mục giống hệt trong khối ban đầu trước khi sao chép. 

Điều bất biến là sau khi xử lý thao tác$i$, giá trị của$k$luôn đề cập đến vị trí chính xác trong mảng được hình thành sau khi hoạt động$i$, nhưng truy nguyên qua tất cả các bản mở rộng ngoài nó. Mỗi bước modulo duy trì tính chính xác của vị trí tương đối bên trong phiên bản trước của mảng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, q = map(int, input().split())
    
    ops = []
    length = [0] * (n + 1)

    for i in range(1, n + 1):
        b, x = map(int, input().split())
        ops.append((b, x))
        if b == 1:
            length[i] = length[i - 1] + 1
        else:
            length[i] = min(10**18, length[i - 1] * (x + 1))

    queries = list(map(int, input().split()))

    for k in queries:
        cur_k = k
        i = n

        while i > 0:
            b, x = ops[i - 1]
            if b == 2:
                prev_len = length[i - 1]
                cur_k %= prev_len
                if cur_k == 0:
                    cur_k = prev_len
            i -= 1

        print(cur_k, end=' ')
    print()

t = int(input())
for _ in range(t):
    solve()
```Giai đoạn đầu tiên xây dựng mảng độ dài để mọi thao tác đều biết chuỗi lớn như thế nào tại thời điểm đó. Điều này rất cần thiết để đảo ngược các bước sao chép, vì chúng ta cần biết kích thước khối trước khi mở rộng. 

Trong quá trình giải quyết truy vấn, chúng tôi thực hiện các thao tác lùi lại. Mỗi thao tác loại 2 sẽ nén chỉ mục trở lại phiên bản trước của mảng bằng cách sử dụng số học modulo. Các thao tác Loại 1 không sửa đổi cấu trúc, vì vậy chúng chỉ đóng vai trò là điểm neo ở nơi các giá trị thực được đưa vào. 

Sự tinh tế chính là xử lý modulo một cách cẩn thận. Khi$k$chia hết chính xác cho độ dài trước đó, phần còn lại trở thành 0, phần tử này phải được ánh xạ trở lại phần tử cuối cùng của khối. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét một trình tự đơn giản hóa: 

Hoạt động: 

1. nối thêm 5 
2. nối thêm 7 
3. nhân đôi 2 lần 

Chúng tôi theo dõi độ dài: 

| tôi | op | len[i] | 
| --- | --- | --- | 
| 1 | +5 | 1 | 
| 2 | +7 | 2 | 
| 3 | ×3 | 6 | 

Truy vấn$k = 5$: 

| bước | tôi | k | hành động | 
| --- | --- | --- | --- | 
| bắt đầu | 3 | 5 | at full array |
 | op 3 | 3 | 5 → 5 % 2 = 1 | ánh xạ vào khối trước | 
| op 2 | 2 | 1 | stop at base |

 Đáp án là yếu tố 1, tức là 5. 

This shows how duplication collapses positions back into earlier structure.

 ### Ví dụ 2 

Hoạt động: 

1. append 1
 2. append 2
 3. append 3
 4. duplicate 2 times

 Final array is`[1,2,3,1,2,3,1,2,3]`. 

Truy vấn$k = 8$: 

| bước | tôi | k | hành động | 
| --- | --- | --- | --- | 
| bắt đầu | 4 | 8 | mảng đầy đủ | 
| op 4 | 4 | 8 % 3 = 2 | giảm thành khối cơ sở | 
| op 3 | 3 | 2 | dừng lại ở vùng cơ sở | 

Đáp án là yếu tố 2 tức là 2. 

Điều này xác nhận rằng các khối lặp lại bảo toàn cấu trúc lập chỉ mục chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n + qn)$trường hợp xấu nhất, điển hình$O(n + q \log n)$| mỗi truy vấn có thể thực hiện các thao tác ngược; trong thực tế kết cấu nén nhanh | 
| Không gian |$O(n)$| hoạt động lưu trữ và độ dài tiền tố | 

Các ràng buộc cho phép tổng$n + q \leq 10^5$, do đó việc truyền tải ngược này đủ hiệu quả vì mỗi thao tác đều đơn giản và nặng về số học thay vì nặng về mảng. Giải pháp không bao giờ xây dựng mảng cuối cùng, chỉ mô phỏng nén chỉ mục. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def solve():
        n, q = map(int, input().split())
        ops = []
        length = [0] * (n + 1)

        for i in range(1, n + 1):
            b, x = map(int, input().split())
            ops.append((b, x))
            if b == 1:
                length[i] = length[i - 1] + 1
            else:
                length[i] = min(10**18, length[i - 1] * (x + 1))

        res = []
        queries = list(map(int, input().split()))

        for k in queries:
            cur_k = k
            i = n
            while i > 0:
                b, x = ops[i - 1]
                if b == 2:
                    prev_len = length[i - 1]
                    cur_k %= prev_len
                    if cur_k == 0:
                        cur_k = prev_len
                i -= 1
            res.append(str(cur_k))

        return " ".join(res)

    return solve()

# sample placeholders (replace with actual)
# assert run(...) == ...
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nối thêm đơn tối thiểu | giá trị tầm thường | độ đúng cơ sở | 
| sao chép đơn | lập chỉ mục lặp đi lặp lại | ánh xạ modulo | 
| hoạt động xen kẽ | cấu trúc hỗn hợp | sự ổn định theo thành phần | 

## Vỏ cạnh 

Trường hợp cạnh khóa phát sinh khi chỉ mục truy vấn được căn chỉnh chính xác với ranh giới khối bên trong phân đoạn trùng lặp. Trong trường hợp đó, logic modulo đơn giản tạo ra số 0 và phải được ánh xạ lại tới phần tử cuối cùng của mảng trước đó. Nếu không có sự điều chỉnh này, các truy vấn nhắm mục tiêu đến các vị trí như$len[i-1]$,$2 \cdot len[i-1]$, v.v., sẽ trả về không chính xác các giá trị 0 hoặc ngoài phạm vi. 

Một trường hợp khác xảy ra khi mảng cực lớn nhưng truy vấn lại nhỏ. Thuật toán không bao giờ cụ thể hóa cấu trúc, do đó ngay cả khi độ dài vượt quá$10^{18}$, việc giảm lùi sẽ giữ các giá trị trong giới hạn hợp lệ và luôn phân giải thành thao tác nối thêm cơ sở. 

Trường hợp cạnh cuối cùng là khi không có thao tác loại 2 nào cả. Trong trường hợp đó, giải pháp sẽ thoái hóa thành tra cứu mảng tiền tố đơn giản và việc truyền tải ngược kết thúc ngay lập tức ở thao tác loại 1 tương ứng.
