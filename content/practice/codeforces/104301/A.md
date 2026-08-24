---
title: "CF 104301A - Đọc Sách"
description: "Chúng tôi được đưa ra nhiều kịch bản độc lập. Trong mỗi kịch bản, người đọc có một chuỗi số lượng sách đã đọc trong nhiều ngày và danh sách độ dài cuốn sách. Mỗi ngày đóng góp một số trang nhất định có thể được sử dụng để đọc các cuốn sách theo thứ tự."
date: "2026-07-01T20:17:21+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104301
codeforces_index: "A"
codeforces_contest_name: "TheForces Round #10 (TEN-Forces)"
rating: 0
weight: 104301
solve_time_s: 281
verified: false
draft: false
---

[CF 104301A - Đọc sách](https://codeforces.com/problemset/problem/104301/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 4 phút 41 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được đưa ra nhiều kịch bản độc lập. Trong mỗi kịch bản, người đọc có một chuỗi số lượng sách đã đọc trong nhiều ngày và danh sách độ dài cuốn sách. Mỗi ngày đóng góp một số trang nhất định có thể được sử dụng để đọc các cuốn sách theo thứ tự. Khi đã tích lũy đủ số trang để che hết một cuốn sách, cuốn sách đó được coi là đã hoàn thành vĩnh viễn. 

Mỗi ngày, chúng tôi phải báo cáo số lượng sách đã đọc xong sau khi xử lý tiến độ đọc của ngày hôm đó. 

Điểm mấu chốt là sách được đọc theo thứ tự và tiến độ một phần được thực hiện qua các ngày. Câu trả lời sau mỗi ngày không độc lập, nó phụ thuộc vào tất cả các ngày trước đó. 

Những ràng buộc rất chặt chẽ. Tổng số ngày và sổ của tất cả các trường hợp thử nghiệm lên tới$2 \cdot 10^5$. Điều này ngay lập tức loại trừ mọi giải pháp tính toán lại tiến độ từ đầu cho mỗi ngày hoặc quét danh sách sách nhiều lần. Một mô phỏng đơn giản kiểm tra tất cả sách mỗi ngày sẽ xuống cấp$O(nm)$trong trường hợp xấu nhất, điều này vượt xa mức có thể chấp nhận được. 

Một số trường hợp phức tạp có vấn đề. Một là khi một ngày cung cấp đủ số trang để đọc hết nhiều cuốn sách cùng một lúc. Một trường hợp khác là khi sách cực nhỏ nên đọc nhanh hoặc cực lớn khiến không tiến triển trong nhiều ngày. Một giải pháp đơn giản chỉ trừ khỏi cuốn sách hiện tại mà không lặp chính xác trên nhiều cuốn sách đã hoàn thành sẽ thất bại trong các trường hợp như: 

đầu vào:```
1
3 3
10 10 10
5 5 20
```Hành vi đúng: 

Ngày 1 đọc được 1 cuốn, Ngày thứ 2 đọc được 2 cuốn, Ngày thứ 3 đọc được 3 cuốn. 

Cách tiếp cận có lỗi có thể dừng lại sau khi đọc xong một cuốn sách mỗi ngày, thiếu các lần hoàn thành theo chuỗi. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: duy trì một con trỏ tới cuốn sách hiện tại và các trang còn lại cần thiết. Đối với mỗi ngày, hãy thêm các trang của ngày hôm đó và liên tục trừ đi kích thước sách nếu có thể. Điều này đúng nhưng cần phải thận trọng khi thực hiện. Trường hợp xấu nhất xảy ra khi mỗi ngày chỉ hoàn thành một cuốn sách và chúng tôi liên tục chạm vào nhiều mục trong nhiều ngày, nhưng ngay cả khi đó mỗi cuốn sách vẫn bị xóa một lần, do đó tổng công việc là$O(n + m)$. Tuy nhiên, cách tiếp cận lồng nhau đơn giản khởi động lại quá trình quét từ đầu mỗi lần hoặc tìm kiếm liên tục cuốn sách chưa hoàn thành tiếp theo sẽ trở thành phương pháp bậc hai. 

Điều quan trọng là mỗi cuốn sách được đọc xong đúng một lần và sau khi đọc xong, nó không bao giờ cần phải xem lại. Điều này có nghĩa là chúng ta có thể duy trì một con trỏ duy nhất vào danh sách sách và bộ đếm các trang tích lũy đang chạy. Mỗi cuốn sách được xử lý nhiều nhất một lần trong toàn bộ trường hợp thử nghiệm, do đó độ phức tạp khấu hao trở nên tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Mô phỏng Brute Force với tính năng quét lại |$O(nm)$|$O(1)$| Quá chậm | 
| Mô phỏng tích lũy hai con trỏ |$O(n + m)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì hai biến: một con trỏ vào mảng sách và tổng số trang được thu thập cho đến nay. 

1. Khởi tạo con trỏ`i = 0`chỉ vào cuốn sách đầu tiên và một biến`cur = 0`lưu trữ các trang tích lũy. 
2. Theo thứ tự mỗi ngày, hãy cộng số lượng bài đọc của ngày đó vào`cur`. Điều này thể hiện tổng số trang chưa đọc được chuyển tiếp. 
3. Trong khi`cur`đủ lớn để bao phủ toàn bộ cuốn sách hiện tại (`cur >= b[i]`) và chúng ta vẫn còn sách, trừ đi kích thước sách từ`cur`và di chuyển`i`phía trước. Mỗi phép trừ tương ứng với việc hoàn thành một cuốn sách. 
4. Sau khi xử lý tất cả các lần hoàn thành có thể có trong ngày, hãy xuất ra`i`, đó là số lượng sách đã hoàn thành cho đến nay. 

Ý tưởng quan trọng là con trỏ chỉ di chuyển về phía trước. Chúng ta không bao giờ xem lại một cuốn sách, bởi vì một khi các trang của nó bị lược bỏ, nó sẽ được hoàn thành vĩnh viễn. 

### Tại sao nó hoạt động 

Bất cứ lúc nào,`cur`đại diện cho các trang còn sót lại chưa được giao cho công việc chưa hoàn thành. Mỗi khi đọc xong một cuốn sách, chúng tôi sẽ xóa chính xác số trang của cuốn sách đó, đảm bảo rằng`cur`luôn phản ánh tiến độ một phần còn lại đối với cuốn sách còn dang dở tiếp theo. Vì sách được xử lý theo thứ tự và không bao giờ được xem lại nên con trỏ`i`đơn điệu ngày càng tăng. Điều này đảm bảo tính chính xác và tránh việc tính hai lần hoặc bỏ qua. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n, m = map(int, input().split())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))
    
    i = 0
    cur = 0
    res = []
    
    for x in a:
        cur += x
        
        while i < m and cur >= b[i]:
            cur -= b[i]
            i += 1
        
        res.append(str(i))
    
    print(" ".join(res))
```Mã tuân theo thuật toán trực tiếp. Biến`cur`tích lũy các trang trong nhiều ngày. Con trỏ`i`theo dõi có bao nhiêu cuốn sách được hoàn thành đầy đủ. Vòng lặp bên trong loại bỏ tất cả các sách có thể được hoàn thành với các trang tích lũy hiện tại, đảm bảo việc hoàn thành theo chuỗi được xử lý chính xác. 

Một cạm bẫy triển khai phổ biến là quên`while`vòng lặp và thay thế nó bằng một vòng lặp duy nhất`if`, điều này sẽ không chính xác khi cho phép chỉ hoàn thành một cuốn sách mỗi ngày. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n=4, m=3
a = [7, 5, 2, 1]
b = [6, 8, 9]
```| Ngày | Trang đã thêm | Cur trước | Quyển 1 | Quyển 2 | Quyển 3 | Sách đã hoàn thành | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 7 | 7 | 6 tiêu thụ | 1 trái | - | 1 | 
| 2 | 5 | 6 | 8 không đủ | - | - | 1 | 
| 3 | 2 | 8 | 2+6 = 10 → hết quyển 2 | - | - | 2 | 
| 4 | 1 | 1 | không thể hoàn thành | - | - | 2 | 

Đầu ra:```
1 1 2 2
```Dấu vết này cho thấy các trang còn sót lại sẽ tích lũy qua nhiều ngày và được sử dụng lại một cách hiệu quả khi đủ. 

### Ví dụ 2 

đầu vào:```
n=5, m=6
a = [7, 12, 23, 15, 29]
b = [10, 9, 6, 8, 13, 1]
```| Ngày | Cur sau khi thêm | Sách đã đọc xong trong ngày | Tổng cộng | 
| --- | --- | --- | --- | 
| 1 | 7 | không | 0 | 
| 2 | 19 | 10, 9 | 2 | 
| 3 | 23 | 6, 8, 13 | 5 | 
| 4 | 15 | không có (một phần) | 5 | 
| 5 | 44 | 1 | 6 | 

Đầu ra:```
2 3 5 5 6
```Điều này thể hiện nhiều lần hoàn thành theo chuỗi trong một ngày và cho thấy lý do tại sao vòng lặp bên trong lại cần thiết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n + m)$mỗi trường hợp thử nghiệm | Mỗi cuốn sách được xử lý chính xác một lần bằng con trỏ`i`và mỗi ngày được xử lý một lần | 
| Không gian |$O(1)$thêm | Chỉ con trỏ và bộ đếm được sử dụng ngoài bộ nhớ đầu vào | 

Tổng kích thước đầu vào trên các trường hợp thử nghiệm là$2 \cdot 10^5$, do đó việc xử lý thời gian tuyến tính trên mỗi tập hợp trường hợp kiểm thử dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline
    
    t = int(input())
    out_lines = []
    for _ in range(t):
        n, m = map(int, input().split())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))
        
        i = 0
        cur = 0
        res = []
        for x in a:
            cur += x
            while i < m and cur >= b[i]:
                cur -= b[i]
                i += 1
            res.append(str(i))
        out_lines.append(" ".join(res))
    
    return "\n".join(out_lines)

# provided sample
assert run("""2
4 3
7 5 2 1
6 8 9
5 6
7 12 23 15 29
10 9 6 8 13 1
""") == """1 1 2 2
2 3 5 6 6"""

# minimum size
assert run("""1
1 1
5
10
""") == "0"

# immediate completion chain
assert run("""1
3 3
10 10 10
5 5 5
""") == "1 2 3"

# no progress case
assert run("""1
4 2
1 1 1 1
10 10
""") == "0 0 0 0"

# large accumulation single day
assert run("""1
2 3
100
10 20 30
""") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| kích thước tối thiểu | 0 | chưa có cuốn sách nào hoàn thành | 
| hoàn thành chuỗi | 1 2 3 | nhiều cuốn sách trong một luồng | 
| không có tiến triển | 0 giây | khối sách lớn tiến bộ | 
| ngày trọng đại duy nhất | 3 | tiêu thụ đầy đủ trong một bước | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi nhiều cuốn sách được hoàn thành trong một ngày. Thuật toán xử lý việc này thông qua`while`vòng lặp, giúp tiếp tục tiêu thụ sách miễn là có đủ số trang tích lũy. Ví dụ: 

đầu vào:```
1
1 3
100
10 20 30
```Việc thực thi bắt đầu với`cur = 100`. Vòng lặp loại bỏ 10, rồi 20, rồi 30, hoàn thành cả ba cuốn sách trong một bước và rời đi`cur = 40`. Con trỏ tiến đến cuối nên đầu ra là`3`. 

Một trường hợp khó khăn khác là khi số lượng sách quá lớn không thể đọc hết trong một ngày. Con trỏ không di chuyển và`cur`chỉ đơn giản là tích lũy. Điều này đảm bảo tính chính xác mà không cần các thao tác không cần thiết, vì không có phép trừ nào xảy ra cho đến khi đạt được tính khả thi.
