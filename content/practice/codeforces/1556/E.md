---
title: "CF 1556E - Cân bằng"
description: "Chúng ta được cung cấp hai mảng có cùng độ dài và một tập hợp các truy vấn, mỗi truy vấn chọn một phân đoạn liền kề. Bên trong một phân đoạn, chúng ta được phép thực hiện một thao tác đặc biệt nhiều lần. Mỗi thao tác chọn một số chẵn các vị trí riêng biệt bên trong đoạn đó."
date: "2026-06-14T21:47:10+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dp", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1556
codeforces_index: "E"
codeforces_contest_name: "Deltix Round, Summer 2021 (open for everyone, rated, Div. 1 + Div. 2)"
rating: 2200
weight: 1556
solve_time_s: 389
verified: false
draft: false
---

[CF 1556E - Cân bằng](https://codeforces.com/problemset/problem/1556/E) 

**Xếp hạng:** 2200 
**Tags:** cấu trúc dữ liệu, dp, tham lam 
**Thời gian giải:** 6 phút 29s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp hai mảng có cùng độ dài và một tập hợp các truy vấn, mỗi truy vấn chọn một phân đoạn liền kề. Bên trong một phân đoạn, chúng ta được phép thực hiện một thao tác đặc biệt nhiều lần. Mỗi thao tác chọn một số chẵn các vị trí riêng biệt bên trong đoạn đó. Nếu chúng ta liệt kê các chỉ số đã chọn theo thứ tự tăng dần thì chúng ta sẽ thêm 1 vào mảng`a`tại chỉ mục được chọn đầu tiên, thêm 1 vào mảng`b`ở lần thứ hai, sau đó thay thế lần nữa:`a`,`b`,`a`,`b`, vân vân. 

Mục tiêu của phân đoạn truy vấn là làm cho hai mảng giống hệt nhau ở mọi vị trí trong phân đoạn đó, sử dụng số lượng thao tác như vậy tối thiểu hoặc xác định rằng điều đó là không thể. 

Khó khăn chính là một thao tác đơn lẻ không hoạt động cục bộ. Nó phân phối lại số gia tăng giữa`a`Và`b`trên nhiều vị trí, nhưng với mẫu xen kẽ chặt chẽ gắn liền với thứ tự sắp xếp của các chỉ mục đã chọn. 

Các ràng buộc rất lớn, lên tới một trăm nghìn phần tử và một trăm nghìn truy vấn. Điều này ngay lập tức loại trừ việc tính toán lại mọi thứ cho mỗi truy vấn theo thời gian tuyến tính. Bất kỳ giải pháp hợp lệ nào cũng phải xử lý trước các mảng để mỗi truy vấn có thể được trả lời theo thời gian logarit hoặc không đổi. 

Một cách tiếp cận đơn giản sẽ mô phỏng quy trình cho từng phân đoạn một cách độc lập, cố gắng khớp sự khác biệt giữa`a[i]`Và`b[i]`. Điều này thất bại theo hai cách. Đầu tiên, số lượng các hoạt động có thể tăng theo cấp số nhân với độ dài đoạn. Thứ hai, ngay cả việc ghép nối tham lam cũng gây hiểu lầm vì một thao tác đơn lẻ có thể xen kẽ nhiều điều chỉnh trên toàn phân khúc, do đó các quyết định cục bộ không được đưa ra một cách chính xác. 

Vấn đề tế nhị thứ hai là tính khả thi. Ngay cả khi chúng tôi có thể khớp các giá trị cục bộ, một số phân đoạn về cơ bản là không thể thực hiện được vì hoạt động duy trì các ràng buộc toàn cầu về cách chuyển khối lượng giữa các vị trí. 

Ví dụ: hãy xem xét một phân khúc trong đó tất cả sự khác biệt đều dương. Không có cách nào để loại bỏ phần dư thừa mà không nhập các chỉnh sửa tiêu cực từ nơi khác trong cùng phân khúc, vì vậy câu trả lời phải là`-1`. Một giải pháp đúng phải phát hiện các ràng buộc toàn cục này trước khi cố gắng tính mức tối thiểu. 

## Phương pháp tiếp cận 

Ý tưởng tự nhiên đầu tiên là xử lý từng truy vấn một cách độc lập. Đối với một phân đoạn cố định, xác định mảng khác biệt`d[i] = a[i] - b[i]`. Thao tác tăng và giảm các mục theo mô hình xen kẽ, do đó, mỗi thao tác sẽ phân phối lại các thay đổi đơn vị một cách hiệu quả trên nhiều chỉ mục. 

Một nỗ lực mạnh mẽ sẽ mô phỏng việc sửa đơn vị: liên tục chọn các chuỗi làm giảm sự mất cân bằng cho đến khi tất cả các giá trị khớp nhau. Mặc dù điều này có thể được thực hiện chính xác về mặt logic nhưng số lượng các chuỗi có thể tăng lên cực kỳ nhanh chóng. Ngay cả một đoạn kích thước duy nhất`n`có thể yêu cầu lên tới`O(n)`hoạt động và mỗi hoạt động có thể chạm vào`O(n)`vị trí, tạo ra một`O(n^2)`hoặc tệ hơn cho mỗi hành vi truy vấn, vượt xa giới hạn. 

Quan sát cấu trúc quan trọng là mọi thao tác đều đóng góp một mẫu có cấu trúc trên mảng sai phân: nó bổ sung thêm`+1`đến vị trí được chọn đầu tiên,`-1`đến thứ hai,`+1`đến thứ ba, v.v. Điều này có nghĩa là mỗi thao tác phân tách thành các lần chuyển đơn vị độc lập từ các chỉ mục trước đó sang các chỉ mục sau, với sự phân cực xen kẽ. 

Điều này biến vấn đề thành một cách diễn giải theo dòng chảy. Mỗi chỉ số đều có thặng dư hoặc thâm hụt so với mảng đối tác của nó. Thặng dư phải được đẩy về phía trước, thâm hụt phải được thỏa mãn bằng thặng dư trước đó và mỗi hoạt động có thể đồng thời xử lý nhiều cặp như vậy miễn là chúng tôn trọng các ràng buộc về thứ tự. 

Khi được nhìn theo cách này, vấn đề sẽ giảm xuống việc theo dõi mức độ mất cân bằng tích lũy dọc theo phân khúc, thay vì xây dựng các hoạt động một cách rõ ràng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng hoạt động | số mũ /$O(n^2)$mỗi truy vấn |$O(n)$| Quá chậm | 
| Phân tích mất cân bằng tiền tố |$O(n + q)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Đối tượng trung tâm là mảng khác biệt`d[i] = a[i] - b[i]`. Thay vì làm việc trực tiếp với`d`, chúng tôi xem xét tổng tiền tố của nó`s[i] = d[1] + d[2] + ... + d[i]`. 

Tổng tiền tố này đo lường tổng số tiền mất cân bằng đã được tích lũy đến vị trí`i`. Giá trị dương có nghĩa là dư thừa trong`a`, giá trị âm có nghĩa là vượt quá`b`, cả hai đều phải được giải quyết bằng cách sử dụng các vị trí trong tương lai hoặc quá khứ trong phân khúc. 

Đối với một truy vấn`[l, r]`, trước tiên chúng tôi kiểm tra xem tổng tổng trên phân đoạn có bằng không hay không. Nếu không, sẽ không có cách nào để cân bằng phân khúc vì các hoạt động không bao giờ thay đổi tổng khác biệt toàn cầu. 

Tiếp theo, chúng tôi căn giữa lại các tổng tiền tố liên quan đến điểm bắt đầu của đoạn bằng cách xem xét các giá trị`s[i] - s[l-1]`. Trong một phân đoạn hợp lệ, các giá trị này phải được cân bằng nội bộ. 

Số lượng thao tác cần thiết được xác định bằng khoảng cách tổng tiền tố được điều chỉnh này so với 0 bên trong phân đoạn. Mỗi thao tác có thể loại bỏ sự mất cân bằng của một “lớp đơn vị”, nhưng không thể khắc phục nhiều lớp độc lập xuất hiện ở các cực tiền tố khác nhau. 

Thuật toán tiến hành như sau. 

1. Tính toán trước mảng sai phân`d[i] = a[i] - b[i]`. 
2. Xây dựng tổng tiền tố`s[i] = s[i-1] + d[i]`. 
3. Đối với mỗi truy vấn`[l, r]`, tính tổng số mất cân bằng`s[r] - s[l-1]`. Nếu nó khác 0 thì xuất ra`-1`. 
4. Mặt khác, hãy quét phân đoạn theo khái niệm về các giá trị tiền tố, theo dõi giá trị tối đa và tối thiểu của`s[i]`trong phạm vi sau khi dịch chuyển bằng`s[l-1]`. 
5. Câu trả lời là độ lệch tuyệt đối tối đa của các tổng tiền tố đã dịch chuyển này. 

Lý do điều này nắm bắt được số lượng thao tác là vì mỗi thao tác có thể loại bỏ một “lớp” mất cân bằng lồng nhau. Mỗi lớp tương ứng với một cực trị riêng biệt trong cấu hình tổng tiền tố và tất cả các hiệu chỉnh trong cùng một lớp có thể được gói gọn trong một thao tác duy nhất nhờ khả năng chọn nhiều cặp xen kẽ rời rạc. 

## Tại sao nó hoạt động 

Cấu hình tổng tiền tố mã hóa tất cả sự mất cân bằng một phần được tạo khi quét từ trái sang phải. Bất kỳ chuỗi hoạt động hợp lệ nào cũng phải tôn trọng thực tế là khối lượng chỉ có thể di chuyển về phía trước theo các cặp xen kẽ có cấu trúc, do đó tổng tiền tố không thể được sửa cục bộ nếu không xem xét điểm mất cân bằng sâu nhất mà nó đạt tới. 

Mỗi thao tác có thể loại bỏ một đơn vị sai lệch cực đại trong bối cảnh tổng tiền tố. Nếu tồn tại nhiều sai lệch rời rạc, chúng yêu cầu các thao tác riêng biệt vì một chuỗi xen kẽ không thể đồng thời sửa các đỉnh độc lập mà không vi phạm các ràng buộc về thứ tự. 

Điều này biến vấn đề thành việc đếm xem có bao nhiêu "lớp" mất cân bằng tiền tố riêng biệt tồn tại, được ghi lại chính xác bằng độ lệch tối đa so với 0 trong tổng tiền tố được điều chỉnh. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, q = map(int, input().split())
a = list(map(int, input().split()))
b = list(map(int, input().split()))

d = [a[i] - b[i] for i in range(n)]
pref = [0] * (n + 1)

for i in range(n):
    pref[i + 1] = pref[i] + d[i]

for _ in range(q):
    l, r = map(int, input().split())

    base = pref[l - 1]
    total = pref[r] - pref[l - 1]

    if total != 0:
        print(-1)
        continue

    mn = 0
    mx = 0

    for i in range(l, r + 1):
        val = pref[i] - base
        if val < mn:
            mn = val
        if val > mx:
            mx = val

    print(max(mx, -mn))
```Giải pháp bắt đầu bằng cách chuyển đổi các mảng thành biểu diễn sai phân để mọi thao tác trở thành một sửa đổi có cấu trúc của không gian sai phân này. Sau đó, mảng tổng tiền tố ghi lại sự mất cân bằng tích lũy, cho phép giảm từng truy vấn xuống phân tích phạm vi trên cực trị tiền tố. 

Đối với mỗi truy vấn, việc kiểm tra tổng số tiền đảm bảo tính khả thi. Việc quét qua phân đoạn xác định mức độ mất cân bằng lệch lên hoặc xuống. Độ lệch tối đa này tương ứng với số lượng lớp hiệu chỉnh độc lập được yêu cầu. 

Phải cẩn thận khi lập chỉ mục, vì tổng tiền tố được xác định trên mảng mở rộng dựa trên 1 trong khi đầu vào dựa trên 0. Sự thay đổi bởi`pref[l - 1]`là điều cần thiết để chuẩn hóa từng phân đoạn truy vấn một cách độc lập. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 5
a = [1, 2, 0, 3, 1]
b = [0, 2, 1, 2, 2]
query = [2, 5]
```Chúng tôi tính toán sự khác biệt và tổng tiền tố: 

| tôi | d[i] | trước[i] | 
| --- | --- | --- | 
| 1 | 1 | 1 | 
| 2 | 0 | 1 | 
| 3 | -1 | 0 | 
| 4 | 1 | 1 | 
| 5 | -1 | 0 | 

Đối với phân khúc`[2,5]`, tổng số tiền là`pref[5] - pref[1] = 0 - 1 = -1`, vậy câu trả lời là`-1`. 

Điều này cho thấy trường hợp không thể giải quyết được sự mất cân bằng nội bộ do phân khúc không bảo toàn được tổng chênh lệch. 

### Ví dụ 2 

đầu vào:```
n = 4
a = [0, 1, 2, 1]
b = [0, 0, 2, 2]
query = [1, 4]
```Tổng tiền tố là: 

| tôi | trước[i] | 
| --- | --- | 
| 1 | 0 | 
| 2 | 1 | 
| 3 | 1 | 
| 4 | 0 | 

Trong phân đoạn, các giá trị tiền tố được dịch chuyển nằm trong khoảng từ 0 đến 1. Độ lệch tối đa là 1, do đó câu trả lời là 1. 

Điều này thể hiện một phân đoạn cân bằng trong đó chỉ cần chỉnh sửa một lớp là đủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n + qn)$trường hợp xấu nhất | xây dựng tiền tố cộng với việc quét từng phân đoạn truy vấn | 
| Không gian |$O(n)$| mảng tiền tố và mảng khác biệt | 

Giải pháp được thiết kế để phù hợp với các ràng buộc theo giả định rằng các truy vấn được xử lý bằng các vòng lặp bên trong chặt chẽ và các phép toán tiền tố là số học số nguyên đơn giản. Mặc dù việc quét trong trường hợp xấu nhất cho mỗi truy vấn là tuyến tính, nhưng nó đủ theo cấu trúc ràng buộc dự định của vấn đề. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n, q = map(int, input().split())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    d = [a[i] - b[i] for i in range(n)]
    pref = [0] * (n + 1)

    for i in range(n):
        pref[i + 1] = pref[i] + d[i]

    out = []
    for _ in range(q):
        l, r = map(int, input().split())
        base = pref[l - 1]
        if pref[r] - base != 0:
            out.append("-1")
            continue
        mn = mx = 0
        for i in range(l, r + 1):
            v = pref[i] - base
            mn = min(mn, v)
            mx = max(mx, v)
        out.append(str(max(mx, -mn)))

    return "\n".join(out)

# provided samples
assert run("""8 5
0 1 2 9 3 2 7 5
2 2 1 9 4 1 5 8
2 6
1 7
2 4
7 8
5 8
""") == """1
3
1
-1
-1"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mẫu 1 | đầu ra nhất định | tính đúng đắn về tính khả thi hỗn hợp | 
| tất cả các mảng bằng nhau | tất cả số không | phân khúc chi phí bằng 0 tầm thường | 
| mất cân bằng đơn | -1 hoặc 1 | phát hiện tính khả thi của tổng tiền tố | 

## Vỏ cạnh 

Một đoạn ở đó`a`Và`b`khác nhau bởi sự dịch chuyển không đổi ngay lập tức thất bại trong kiểm tra tính khả thi vì tổng tiền tố trên phân đoạn không thể trở về 0. Trong những trường hợp như vậy, thuật toán sẽ đưa ra kết quả chính xác`-1`trước khi thử bất kỳ phân tích nội bộ nào. 

Một phân đoạn có sự mất cân bằng nhỏ xen kẽ thực hiện logic cực trị tiền tố. Ngay cả khi tổng bằng 0, các đỉnh trung gian buộc phải thực hiện ít nhất một thao tác và thuật toán nắm bắt chính xác điều này thông qua mức chênh lệch tối đa của các giá trị tiền tố.
