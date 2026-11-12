---
title: "CF 1656C - Bình Đẳng Với Mod"
description: "Chúng ta được cho một mảng các số nguyên không âm. Trong một thao tác, chúng ta chọn một số nguyên $x ge 2$ và đồng thời thay thế mọi phần tử $ai$ bằng $ai bmod x$. Hoạt động có thể được áp dụng bất kỳ số lần nào, kể cả số không."
date: "2026-06-10T03:32:25+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "math", "number-theory", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1656
codeforces_index: "C"
codeforces_contest_name: "CodeTON Round 1 (Div. 1 + Div. 2, Rated, Prizes!)"
rating: 1200
weight: 1656
solve_time_s: 118
verified: true
draft: false
---

[CF 1656C - Tạo sự bình đẳng với Mod](https://codeforces.com/problemset/problem/1656/C) 

**Đánh giá:** 1200 
**Tags:** thuật toán xây dựng, toán học, lý thuyết số, sắp xếp 
**Thời gian giải:** 1 phút 58 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một mảng các số nguyên không âm. Trong một thao tác, chúng ta chọn một số nguyên$x \ge 2$và đồng thời thay thế mọi phần tử$a_i$với$a_i \bmod x$. 

Hoạt động có thể được áp dụng bất kỳ số lần nào, kể cả số không. Mục đích là để xác định liệu có thể đạt đến trạng thái mà mọi phần tử của mảng có cùng giá trị hay không. 

Độ dài mảng có thể lớn bằng$10^5$trong một trường hợp thử nghiệm duy nhất và tổng độ dài của tất cả các trường hợp thử nghiệm tối đa là$2 \cdot 10^5$. Bất kỳ giải pháp nào cố gắng mô phỏng nhiều thao tác sửa đổi có thể có hoặc khám phá các trạng thái của mảng sẽ bị loại trừ ngay lập tức. Chúng ta cần một cái gì đó gần tuyến tính hoặc$O(n \log n)$mỗi trường hợp thử nghiệm. 

Phần thú vị của vấn đề là hiểu cách phép toán modulo thay đổi mối quan hệ giữa các số. Thao tác được áp dụng cho tất cả các phần tử cùng một lúc, vì vậy chúng ta không thể thao tác các giá trị riêng lẻ một cách độc lập. 

Một số trường hợp cạnh rất dễ bị bỏ lỡ. 

Coi như:```
3
1 1 1
```Câu trả lời là`YES`bởi vì tất cả các phần tử đều bằng nhau và chúng tôi được phép thực hiện các thao tác bằng 0. 

Bây giờ hãy xem xét:```
3
0 2 5
```Câu trả lời là`YES`. Lựa chọn$x=2$mang lại:```
0 0 1
```và sau đó chọn$x=2$một lần nữa cho:```
0 0 1
```Điều đó không hoàn thành công việc mà là lựa chọn$x=5$đầu tiên sản xuất:```
0 2 0
```và sau đó$x=2$mang lại:```
0 0 0
```Một quy tắc ngây thơ như "nếu có số 0 thì câu trả lời luôn là KHÔNG" sẽ thất bại. 

Phản ví dụ quan trọng là:```
3
0 1 5
```Câu trả lời là`NO`. 

Số 0 không bao giờ có thể thay đổi bởi vì$0 \bmod x = 0$. giá trị$1$cũng không bao giờ thay đổi bởi vì với mọi$x \ge 2$,```
1 mod x = 1
```Vì vậy, không và một không bao giờ có thể bằng nhau. Bất kỳ cách tiếp cận nào chỉ kiểm tra xem số 0 có tồn tại hay không sẽ bỏ lỡ điều này. 

Một trường hợp quan trọng khác là:```
4
5 9 17 5
```Câu trả lời là`YES`. 

Lựa chọn$x=4$mang lại:```
1 1 1 1
```Mặc dù các giá trị ban đầu khá khác nhau nhưng một mô đun được lựa chọn cẩn thận có thể ngay lập tức làm cho chúng bằng nhau. 

## Phương pháp tiếp cận 

Một tư duy vũ phu sẽ cố gắng tìm kiếm các giá trị có thể có của$x$, áp dụng các thao tác và xem liệu cuối cùng mảng có thể thu gọn thành một giá trị hay không. 

Điều này không thực tế. Những con số lớn như$10^9$, do đó có hàng tỷ mô đun có thể có. Ngay cả việc khám phá một phần rất nhỏ của không gian trạng thái cũng là không thể. Tệ hơn nữa, các chuỗi thao tác khác nhau có thể dẫn đến các mảng khác nhau, tạo ra một cây tìm kiếm khổng lồ. 

Để tiến bộ, chúng ta cần hiểu những đặc tính nào không bao giờ thay đổi. 

Quan sát quan trọng là các số 0 và 1 là đặc biệt. 

Số 0 là vĩnh viễn. Cho dù chúng ta chọn mô đun nào,$$0 \bmod x = 0.$$Một cũng là vĩnh viễn vì mọi mô đun cho phép đều thỏa mãn$x \ge 2$, Vì thế$$1 \bmod x = 1.$$Nếu cả 0 và 1 xuất hiện trong mảng, chúng sẽ khác nhau mãi mãi. Việc tiếp cận một mảng trong đó tất cả các phần tử đều bằng nhau là điều không thể. 

Bây giờ giả sử mảng chứa số 0 nhưng không chứa số 1. 

Cho phép$m$là giá trị dương nhỏ nhất trong mảng. Từ$m \ge 2$, đang chọn$x=m$biến mỗi lần xuất hiện của$m$về 0, trong khi các giá trị lớn hơn trở thành số trong phạm vi$[0,m-1]$. Việc lặp lại ý tưởng này cuối cùng sẽ đẩy tất cả các giá trị dương về 0. Bản thân sự hiện diện của số 0 không phải là vấn đề. Vấn đề duy nhất là có cả 0 và 1 cùng một lúc. 

Nếu không có số 0 nào cả thì sao? 

Cho phép$m$phải là phần tử tối thiểu. Lựa chọn$x=m$làm cho mỗi lần xuất hiện của$m$trở thành số không. Vì trước đó không có số 0 nên mảng mới chứa số 0 nhưng không thể chứa số 0 nếu mảng ban đầu không có cặp giá trị liên tiếp. Việc lặp lại quá trình rút gọn cuối cùng sẽ khiến mọi thứ trở nên bình đẳng. 

Trở ngại còn lại là sự tồn tại của hai giá trị liên tiếp. Nếu mảng chứa các giá trị$k$Và$k+1$, thì sau khi rút gọn đủ, chúng hoạt động chính xác như 0 và 1. Điều này hóa ra là điều duy nhất quan trọng. 

Giải pháp được chấp nhận cực kỳ đơn giản: 

Nếu mảng chứa số 0 thì nó không được chứa số 1. 

Tổng quát hơn, sau khi sắp xếp, nếu bất kỳ hai giá trị liền kề nào khác nhau đúng một thì câu trả lời là`NO`. 

Nếu không thì câu trả lời là`YES`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tìm kiếm trạng thái Brute Force | Hàm mũ | Hàm mũ | Quá chậm | 
| Sắp xếp và kiểm tra các giá trị liên tiếp | O(n log n) | O(1) bổ sung (không bao gồm sắp xếp) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc mảng. 
2. Sắp xếp mảng. 

Việc sắp xếp sắp xếp tất cả các giá trị theo thứ tự tăng dần, giúp dễ dàng phát hiện xem hai giá trị có khác nhau chính xác một hay không. 
3. Kiểm tra xem mảng có chứa số 0 hay không. 
4. Nếu tồn tại số 0, hãy quét mảng đã sắp xếp để tìm số 1. 

Nếu có cả 0 và 1, xuất ra`NO`. 

Số không không bao giờ thay đổi và một không bao giờ thay đổi, nên chúng không bao giờ có thể bằng nhau. 
5. Nếu không tồn tại số 0, hãy quét các phần tử liền kề theo thứ tự được sắp xếp. 

Nếu bất kỳ cặp nào khác nhau đúng một, thì xuất ra`NO`. 

Các giá trị liên tiếp cuối cùng tạo ra sự cản trở tương tự như có 0 và 1. 
6. Nếu cả hai điều kiện trước đó đều không được kích hoạt, thì xuất ra`YES`. 

### Tại sao nó hoạt động 

Bất biến quan trọng là khi giá trị 0 và 1 xuất hiện cùng nhau, chúng không bao giờ có thể hợp nhất được. Số không luôn là số không và một luôn là một. 

Nếu mảng đã chứa số 0 thì mọi mảng trong tương lai cũng sẽ chứa số 0. Vì vậy, sự hiện diện của một người ngay lập tức khiến mục tiêu không thể thực hiện được. 

Khi không có số 0, giá trị tối thiểu luôn có thể giảm xuống 0 bằng cách chọn nó làm mô đun. Nếu hai số khác nhau một thì sự giảm này cuối cùng sẽ tạo ra cả 0 và 1, tạo ra cấu hình không thể thực hiện được. Ngược lại, nếu không tồn tại cặp liên tiếp, việc giảm lặp đi lặp lại có thể đưa tất cả các giá trị về cùng một số mà không bao giờ tạo ra tình huống bị cấm. 

Do đó, vấn đề giảm xuống còn việc phát hiện xem mảng có chứa cặp liên tiếp bị cấm hay không, với cách xử lý đặc biệt chỉ quan trọng khi có số 0. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        
        a.sort()
        
        if a[0] == 0:
            if 1 in a:
                print("NO")
            else:
                print("YES")
        else:
            ok = True
            for i in range(1, n):
                if a[i] - a[i - 1] == 1:
                    ok = False
                    break
            
            print("YES" if ok else "NO")

solve()
```Bước đầu tiên là sắp xếp mảng. Sau khi sắp xếp, mọi giá trị liên tiếp sẽ xuất hiện cạnh nhau, do đó việc kiểm tra sự khác biệt chính xác bằng một giá trị sẽ trở thành quá trình quét tuyến tính đơn giản. 

Giá trị nhỏ nhất rất dễ xác định sau khi sắp xếp. Nếu nó bằng 0, chúng ta chỉ cần biết liệu một số có tồn tại ở bất kỳ đâu trong mảng hay không. sử dụng`1 in a`đủ hiệu quả vì tổng kích thước đầu vào được giới hạn bởi$2 \cdot 10^5$. 

Khi không có số 0, thuật toán sẽ kiểm tra mọi cặp liền kề. Thời điểm tìm thấy sự khác biệt, chúng ta biết câu trả lời là`NO`. 

Không có lo ngại về tràn vì số nguyên Python xử lý các giá trị lớn hơn nhiều so với giới hạn của bài toán. Điểm tinh tế duy nhất là hãy nhớ rằng các bản sao là vô hại. Sự khác biệt bằng 0 không tạo ra bất kỳ trở ngại nào, do đó chỉ có sự khác biệt của đúng một vấn đề. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
2 5 6 8
```Mảng được sắp xếp: 

| Chỉ mục | Giá trị | 
| --- | --- | 
| 0 | 2 | 
| 1 | 5 | 
| 2 | 6 | 
| 3 | 8 | 

Sự khác biệt liền kề: 

| Cặp | Sự khác biệt | 
| --- | --- | 
| 2, 5 | 3 | 
| 5, 6 | 1 | 
| 6, 8 | 2 | 

Tồn tại một cặp liên tiếp nên thuật toán đưa ra`NO`. 

Ví dụ này thể hiện điều kiện giá trị liên tiếp bị cấm. 

### Ví dụ 2 

đầu vào:```
5
4 1 7 0 8
```Mảng được sắp xếp: 

| Chỉ mục | Giá trị | 
| --- | --- | 
| 0 | 0 | 
| 1 | 1 | 
| 2 | 4 | 
| 3 | 7 | 
| 4 | 8 | 

Kiểm tra sự hiện diện: 

| Chứa 0 | Chứa 1 | 
| --- | --- | 
| Có | Có | 

Vì cả 0 và 1 đều có mặt nên câu trả lời là`NO`. 

Dấu vết này thể hiện sự xung đột thường xuyên giữa số không và một. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp chiếm ưu thế trong thời gian chạy | 
| Không gian | O(1) thêm | Ngoài việc lưu trữ mảng và sắp xếp | 

Tổng số phần tử trong tất cả các trường hợp thử nghiệm nhiều nhất là$2 \cdot 10^5$. MỘT$O(n \log n)$giải pháp dễ dàng phù hợp với giới hạn thời gian và mức sử dụng bộ nhớ vẫn thấp hơn nhiều so với mức 256 MB khả dụng. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    t = int(input())
    ans = []

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        a.sort()

        if a[0] == 0:
            ans.append("NO" if 1 in a else "YES")
        else:
            ok = True
            for i in range(1, n):
                if a[i] - a[i - 1] == 1:
                    ok = False
                    break
            ans.append("YES" if ok else "NO")

    sys.stdout.write("\n".join(ans))

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    solve()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out.getvalue().strip()

# provided sample
assert run(
"""4
4
2 5 6 8
3
1 1 1
5
4 1 7 0 8
4
5 9 17 5
"""
) == "\n".join([
    "NO",
    "YES",
    "NO",
    "YES"
]), "sample"

# minimum size
assert run(
"""1
1
0
"""
) == "YES"

# all equal
assert run(
"""1
5
7 7 7 7 7
"""
) == "YES"

# contains 0 and 1
assert run(
"""1
3
0 1 100
"""
) == "NO"

# no zero but consecutive values
assert run(
"""1
4
10 14 15 20
"""
) == "NO"

# large gaps only
assert run(
"""1
5
2 4 8 16 32
"""
) == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`[0]`| CÓ | Kích thước mảng tối thiểu | 
|`[7,7,7,7,7]`| CÓ | Đã có giá trị bằng nhau | 
|`[0,1,100]`| KHÔNG | Xung đột vĩnh viễn 0 và 1 | 
|`[10,14,15,20]`| KHÔNG | Cặp liên tiếp không có số 0 | 
|`[2,4,8,16,32]`| CÓ | Không có cặp bị cấm | 

## Vỏ cạnh 

Hãy xem xét:```
1
3
0 1 5
```Sau khi sắp xếp ta được:```
0 1 5
```Thuật toán nhìn thấy cả 0 và 1 và xuất ra ngay lập tức`NO`. 

Điều này đúng vì số 0 luôn bằng 0 và một luôn bằng một. Không có chuỗi thao tác mod nào có thể làm cho chúng bằng nhau. 

Bây giờ hãy xem xét:```
1
4
2 4 8 16
```Mảng được sắp xếp không thay đổi. Sự khác biệt liền kề là:```
2, 4 -> 2
4, 8 -> 4
8, 16 -> 8
```Không có bằng một, do đó thuật toán đưa ra`YES`. 

Việc thiếu các giá trị liên tiếp có nghĩa là có thể tránh được cấu hình 0 và 1 bị cấm trong khi giảm số lượng. 

Cuối cùng hãy xem xét:```
1
5
0 2 2 10 20
```Mảng chứa số 0 nhưng không chứa một. Đầu ra của thuật toán`YES`. 

Bản sao không thành vấn đề. Giá trị nguy hiểm duy nhất bên cạnh số 0 là một. Vì nó vắng mặt nên tất cả các giá trị dương cuối cùng có thể giảm xuống 0, làm cho toàn bộ mảng bằng nhau.
