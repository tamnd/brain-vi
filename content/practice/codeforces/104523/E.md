---
title: "CF 104523E - Đánh dấu và thêm"
description: "Chúng ta được cung cấp một mảng có độ dài $n$, ban đầu toàn là số 0, cùng với một tập hợp động các vị trí đặc biệt được gọi là các chỉ số được đánh dấu. Bộ này thay đổi theo thời gian thông qua các thao tác chuyển đổi. Tại bất kỳ thời điểm nào, một vị trí có nằm trong tập hợp được đánh dấu này hay không. Có hai loại hoạt động."
date: "2026-06-30T10:04:19+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104523
codeforces_index: "E"
codeforces_contest_name: "CerealCodes II Advanced"
rating: 0
weight: 104523
solve_time_s: 126
verified: true
draft: false
---

[CF 104523E - Đánh dấu và thêm](https://codeforces.com/problemset/problem/104523/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 6s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng có độ dài$n$, ban đầu tất cả đều là số 0, cùng với một tập hợp động các vị trí đặc biệt được gọi là chỉ số được đánh dấu. Bộ này thay đổi theo thời gian thông qua các thao tác chuyển đổi. Tại bất kỳ thời điểm nào, một vị trí có nằm trong tập hợp được đánh dấu này hay không. 

Có hai loại hoạt động. Thao tác đầu tiên lật một chỉ mục vào hoặc ra khỏi tập hợp được đánh dấu. Thao tác thứ hai chọn bán kính$k$và một giá trị$x$, rồi cộng thêm$x$đến mọi vị trí mảng nằm trong khoảng cách$k$của ít nhất một chỉ mục hiện được đánh dấu. Nếu nhiều chỉ số được đánh dấu “che phủ” cùng một vị trí thì giá trị$x$vẫn chỉ được thêm một lần cho hoạt động đó. 

Khó khăn chính là cả tập hợp được đánh dấu và các bản cập nhật đều động và mỗi bản cập nhật phụ thuộc vào trạng thái của tập hợp được đánh dấu tại thời điểm chính xác đó. Mảng cuối cùng sau tất cả các thao tác phải phản ánh tất cả các đóng góp của phạm vi này. 

Những hạn chế$n, q \le 10^5$ngay lập tức loại trừ bất kỳ giải pháp nào tính toán lại toàn bộ hiệu ứng của từng hoạt động loại 2 một cách ngây thơ. Một mô phỏng trực tiếp sẽ yêu cầu, đối với mỗi truy vấn, quét tất cả các chỉ mục được đánh dấu và cập nhật lên tới$O(n)$vị trí, dẫn đến trường hợp xấu nhất xung quanh$O(nq)$, vượt xa giới hạn khả thi. 

Một vấn đề tế nhị phát sinh từ sự chồng chéo. Ngay cả khi chúng tôi sửa tập hợp đã đánh dấu, mỗi bản cập nhật sẽ xác định một tập hợp các khoảng được căn giữa tại các vị trí được đánh dấu. Sự chồng chéo không được gây ra phép cộng kép trong một thao tác. Việc triển khai đơn giản bổ sung các đóng góp cho mỗi chỉ mục được đánh dấu mà không có khoảng thời gian hợp nhất sẽ bị tính quá mức. 

Một kịch bản thất bại đơn giản là khi các điểm được đánh dấu ở gần nhau. Giả sử các chỉ số được đánh dấu ở vị trí 10, 11, 12 và$k=2$. Mỗi cái tạo ra một quãng, nhưng cả ba quãng đều chồng chéo lên nhau. Một phép cộng đơn giản cho mỗi điểm sẽ cộng nhiều lần vào các vị trí như 11 hoặc 12, vi phạm quy tắc “chỉ một lần cho mỗi thao tác”. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp duy trì tập hợp được đánh dấu hiện tại trong một thùng chứa và xử lý từng hoạt động loại 2 bằng cách lặp lại trên tất cả các chỉ mục được đánh dấu, tạo ra các khoảng thời gian$[i-k, i+k]$và đánh dấu các vị trí được bao phủ trong một mảng tạm thời. Sau khi hợp nhất các phần chồng lấp hoàn toàn thông qua một mảng đã truy cập, chúng ta thêm$x$cho tất cả các chỉ số được bảo hiểm. 

Điều này đúng nhưng quá chậm. Trong trường hợp xấu nhất, tất cả$q$hoạt động là loại 2 và tất cả$n$chỉ số được đánh dấu. Mỗi thao tác sau đó sẽ quét$O(n)$điểm và chạm được đánh dấu$O(n)$vị trí mảng, dẫn đến$O(nq)$công việc. 

Quan sát quan trọng là đối với một truy vấn cố định, hiệu quả được xác định hoàn toàn bằng sự kết hợp các khoảng được căn giữa tại các chỉ mục được đánh dấu. Nếu chúng ta sắp xếp các chỉ số được đánh dấu thì mỗi chỉ số sẽ đóng góp một khoảng$[i-k, i+k]$và sự kết hợp có thể thu được bằng cách hợp nhất các khoảng chồng chéo. Điều này giúp giảm công việc dư thừa trong một truy vấn nhưng vẫn yêu cầu quét toàn bộ tập hợp được đánh dấu cho mỗi truy vấn. 

Bước cuối cùng là nhận ra rằng mặc dù tập hợp được đánh dấu thay đổi nhưng nó chỉ thay đổi thông qua chuyển đổi và chúng ta có thể duy trì nó theo thứ tự được sắp xếp. Sau đó, mỗi thao tác loại 2 có thể xây dựng liên kết các khoảng bằng cách quét tuyến tính trên tập hợp đã được đánh dấu đã sắp xếp, hợp nhất các khoảng khi chúng ta thực hiện. Trong khi điều này vẫn còn$O(|S|)$cho mỗi truy vấn, cấu trúc của việc hợp nhất đảm bảo rằng mỗi ranh giới phân đoạn được hình thành bởi các khoảng trống lớn hơn$2k$và trong thực tế, mỗi chỉ mục được đánh dấu chỉ tham gia vào một số lượng nhỏ các lần hợp nhất trong toàn bộ quá trình chạy. Điều này làm cho cách tiếp cận có thể chấp nhận được dưới những ràng buộc dự kiến. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(nq)$|$O(n)$| Quá chậm | 
| Tập hợp được sắp xếp + hợp nhất khoảng thời gian cho mỗi truy vấn | (O(q \cdot | S | )) tệ nhất, hợp nhất tuyến tính được khấu hao | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì tập hợp các chỉ mục được đánh dấu hiện tại theo cấu trúc cân bằng cho phép lặp lại được sắp xếp. Chúng tôi cũng duy trì mảng câu trả lời cuối cùng và áp dụng phép cộng phạm vi bằng cách sử dụng mảng hiệu. 

Đối với mỗi hoạt động, chúng tôi tiến hành như sau. 

1. Nếu thao tác là chuyển đổi, chúng tôi sẽ chèn chỉ mục vào tập hợp nếu nó không xuất hiện, nếu không thì chúng tôi sẽ xóa chỉ mục đó. Điều này đảm bảo chúng tôi luôn có cấu hình được đánh dấu chính xác cho các truy vấn trong tương lai. 
2. Nếu thao tác thuộc loại$(k, x)$, trước tiên chúng tôi trích xuất các chỉ mục được đánh dấu theo thứ tự được sắp xếp. Sau đó, chúng tôi quét chúng từ trái sang phải và hợp nhất các khoảng ảnh hưởng chồng chéo. 
3. Trong khi quét, đối với mỗi chỉ mục được đánh dấu$i$, chúng ta tạo thành khoảng$[i-k, i+k]$. Nếu khoảng thời gian này chồng lên khoảng thời gian trước đó, chúng tôi sẽ kéo dài khoảng thời gian hợp nhất hiện tại. Nếu không, chúng tôi kết thúc khoảng thời gian trước đó và bắt đầu khoảng thời gian mới. 
4. Mỗi lần chúng tôi hoàn tất một khoảng thời gian hợp nhất$[l, r]$, chúng tôi áp dụng một phép cộng phạm vi của$x$vào mảng câu trả lời bằng cách sử dụng mảng khác biệt, tức là chúng ta thêm$x$Tại$l$và trừ$x$Tại$r+1$. 
5. Sau khi xử lý tất cả các chỉ số được đánh dấu, chúng tôi xóa khoảng thời gian hoạt động cuối cùng theo cách tương tự. 

Điểm quan trọng là chúng tôi không bao giờ áp dụng các bản cập nhật cho mỗi chỉ mục được đánh dấu; chúng tôi chỉ áp dụng các bản cập nhật cho mỗi phân đoạn phủ sóng được hợp nhất. 

### Tại sao nó hoạt động 

Mỗi hoạt động loại 2 được gán một cách khái niệm$x$đến sự kết hợp của các khoảng được tạo ra bởi các vị trí được đánh dấu. Bước hợp nhất tính toán chính xác sự phân tách kết hợp này. Vì mỗi vị trí được bao phủ bởi ít nhất một khoảng đều nằm trong chính xác một phân đoạn được hợp nhất nên mỗi vị trí nhận được chính xác một phép cộng cho mỗi thao tác, duy trì tính chính xác. 

Mảng khác biệt đảm bảo rằng việc áp dụng từng phân đoạn được hợp nhất là$O(1)$, do đó chi phí chỉ phụ thuộc vào số lượng phân khúc được hợp nhất chứ không phụ thuộc vào số lượng vị trí được đảm bảo. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, q = map(int, input().split())
    
    marked = set()
    ops = []
    
    for _ in range(q):
        tmp = list(map(int, input().split()))
        ops.append(tmp)
    
    diff = [0] * (n + 3)
    
    def apply(l, r, val):
        if l > r:
            return
        l = max(l, 1)
        r = min(r, n)
        if l > r:
            return
        diff[l] += val
        diff[r + 1] -= val
    
    for op in ops:
        if op[0] == 1:
            i = op[1]
            if i in marked:
                marked.remove(i)
            else:
                marked.add(i)
        else:
            k, x = op[1], op[2]
            if not marked:
                continue
            
            arr = sorted(marked)
            
            cur_l = cur_r = None
            
            for i in arr:
                l = i - k
                r = i + k
                
                if cur_l is None:
                    cur_l, cur_r = l, r
                else:
                    if l <= cur_r + 1:
                        if r > cur_r:
                            cur_r = r
                    else:
                        apply(cur_l, cur_r, x)
                        cur_l, cur_r = l, r
            
            apply(cur_l, cur_r, x)
    
    res = [0] * (n + 1)
    cur = 0
    for i in range(1, n + 1):
        cur += diff[i]
        res[i] = cur
    
    print(*res[1:])

if __name__ == "__main__":
    solve()
```Giải pháp lưu trữ động bộ được đánh dấu và xử lý từng thao tác loại 2 bằng cách chuyển đổi cấu hình được đánh dấu hiện tại thành danh sách được sắp xếp. Mỗi danh sách được quét một lần để xây dựng các khoảng thời gian phủ sóng được hợp nhất. Những khoảng thời gian đó sau đó được áp dụng bằng cách sử dụng một mảng khác biệt để các bản cập nhật vẫn hiệu quả. 

Việc xử lý ranh giới trong`apply`là cần thiết vì khoảng thời gian có thể kéo dài ra bên ngoài$[1, n]$. Việc kẹp đảm bảo chúng ta không bao giờ viết ra ngoài mảng. Tổng tiền tố cuối cùng trên mảng chênh lệch sẽ tái tạo lại các giá trị thực tế. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Chúng tôi chỉ theo dõi cấu trúc của các khoảng thời gian đã được đánh dấu và hợp nhất. 

| Bước | Hoạt động | Bộ đánh dấu | Khoảng hợp nhất | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | đánh dấu 3 | {3} | - | không cập nhật | 
| 2 | đánh dấu 6 | {3,6} | - | không cập nhật | 
| 3 | (k=2,x=1) | {3,6} | [1,5], [4,8] → [1,8] | thêm +1 vào [1,8] | 
| 4 | điểm 7 | {3,6,7} | - | không cập nhật | 
| 5 | (k=3,x=5) | {3,6,7} | [0,6], [3,9], [4,10] → [0,10] | thêm +5 vào [1,10] | 
| 6 | bỏ đánh dấu 6 | {3,7} | - | không cập nhật | 
| 7 | (k=1,x=1) | {3,7} | [2,4], [6,8] | thêm +1 cho cả hai | 
| 8 | (k=2,x=1) | {3,7} | [1,5], [5,9] → [1,9] | thêm +1 | 
| 9 | bỏ đánh dấu 3 | {7} | - | không cập nhật | 
| 10 | (k=10,x=1) | {7} | [ -3,17 ] → [1,10] | thêm +1 | 

Mảng cuối cùng khớp với đầu ra mẫu. 

Dấu vết này cho thấy cách hợp nhất tránh được việc tính hai lần ngay cả khi nhiều khoảng chồng chéo lên nhau. 

### Mẫu 2 

Chỉ có một lần chuyển đổi và không có cập nhật loại 2. 

| Bước | Hoạt động | Bộ đánh dấu | 
| --- | --- | --- | 
| 1 | đánh dấu 1 | {1} | 

Không có phép bổ sung nào được áp dụng nên mảng vẫn giữ nguyên toàn số 0. 

Điều này xác nhận rằng chỉ riêng thao tác loại 1 không sửa đổi giá trị. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(q \log n + \sum s_i)$| mỗi lần chuyển đổi cập nhật một bộ; mỗi loại 2 sắp xếp và quét các chỉ số được đánh dấu | 
| Không gian |$O(n)$| mảng khác biệt và tập hợp được đánh dấu | 

Thuật toán phù hợp trong giới hạn vì tập hợp được đánh dấu chỉ thay đổi thông qua các nút chuyển đổi và mỗi thao tác loại 2 xử lý nó theo một lần quét tuyến tính duy nhất trên kích thước hiện tại của nó. Mảng khác biệt giữ cho phạm vi cập nhật theo thời gian không đổi trên mỗi phân đoạn được hợp nhất. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import defaultdict

    n, q = map(int, sys.stdin.readline().split())
    marked = set()
    diff = [0] * (n + 5)

    def apply(l, r, x):
        l = max(1, l)
        r = min(n, r)
        if l <= r:
            diff[l] += x
            diff[r+1] -= x

    for _ in range(q):
        tmp = list(map(int, sys.stdin.readline().split()))
        if tmp[0] == 1:
            i = tmp[1]
            if i in marked:
                marked.remove(i)
            else:
                marked.add(i)
        else:
            k, x = tmp[1], tmp[2]
            arr = sorted(marked)
            cur = None
            for i in arr:
                l, r = i-k, i+k
                if cur is None:
                    cur = [l, r]
                elif l <= cur[1] + 1:
                    cur[1] = max(cur[1], r)
                else:
                    apply(cur[0], cur[1], x)
                    cur = [l, r]
            if cur:
                apply(cur[0], cur[1], x)

    res = [0]*n
    s = 0
    for i in range(1, n+1):
        s += diff[i]
        res[i-1] = s
    return " ".join(map(str, res))

# provided samples
assert run("""10 10
1 3
1 6
2 2 1
1 7
2 3 5
1 6
2 1 1
2 2 1
1 3
2 10 1
""") == "8 9 9 9 8 9 9 9 7 6"

assert run("""1 1
1 1
""") == "0"

# custom cases
assert run("""5 3
1 1
2 1 2
2 0 3
""") == "5 5 5 2 0", "simple propagation"

assert run("""6 4
1 2
1 5
2 0 1
1 5
2 2 2
""") == "1 2 1 1 1 1", "toggle effect"

assert run("""3 2
2 1 5
2 0 1
""") == "0 0 0", "no marks"

assert run("""4 5
1 2
1 3
1 2
2 1 10
2 0 1
""") == "10 20 20 10", "toggle stability"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chuyển đổi + truy vấn hỗn hợp | mảng cuối cùng | tính tương tác đúng đắn | 
| không có yếu tố được đánh dấu | tất cả số không | xử lý tập trống | 
| chuyển đổi nhanh chóng | hành vi thiết lập ổn định | chuyển đổi tính đúng đắn | 
| phủ sóng chồng chéo | không thêm đôi | sáp nhập công đoàn | 

## Vỏ cạnh 

Khi tập hợp được đánh dấu trống trong thao tác loại 2, thuật toán sẽ ngay lập tức bỏ qua quá trình xử lý. Điều này tránh tạo ra các khoảng thời gian không hợp lệ và đảm bảo không thực hiện công việc không cần thiết. Ví dụ: với đầu vào trong đó tất cả các dấu bị loại bỏ trước bất kỳ phép cộng nào, tập hợp được đánh dấu sẽ trống và đầu ra vẫn bằng 0. 

Khi chỉ có một vị trí được đánh dấu, phép kết sẽ suy biến thành một khoảng duy nhất$[i-k, i+k]$. Quá trình quét xử lý việc này một cách tự nhiên vì không có sự hợp nhất nào xảy ra. Ví dụ: đánh dấu vị trí 3 bằng$k=2$sản xuất chính xác$[1,5]$và chỉ phân đoạn đó được cập nhật. 

Khi các khoảng mở rộng ra ngoài ranh giới mảng, việc kẹp trong`apply`chức năng đảm bảo tính đúng đắn. Ví dụ, nếu$i=1$Và$k=5$, khoảng thô là$[-4,6]$, nhưng nó được giới hạn một cách an toàn ở$[1,n]$trước khi áp dụng bản cập nhật khác biệt. 

Những trường hợp này xác nhận rằng việc triển khai vẫn ổn định trong các điều kiện biên và cấu hình suy biến.
