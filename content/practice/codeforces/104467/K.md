---
title: "CF 104467K - Karaoke"
description: "Chúng tôi bắt đầu với T giây trước khi hệ thống karaoke khóa. Các bài hát phải được phát liên tục từ thời điểm 0 nên không bao giờ có thời gian nhàn rỗi. Có sẵn ba bài hát thông thường với độ dài A, B và C và mỗi bài có thể được chọn bất kỳ số lần nào."
date: "2026-06-30T13:10:28+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104467
codeforces_index: "K"
codeforces_contest_name: "La Salle-Pui Ching Programming Challenge \u57f9\u6b63\u5587\u6c99\u7de8\u7a0b\u6311\u6230\u8cfd 2022"
rating: 0
weight: 104467
solve_time_s: 73
verified: true
draft: false
---

[CF 104467K - Karaoke](https://codeforces.com/problemset/problem/104467/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 13s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi bắt đầu với`T`giây trước khi hệ thống karaoke khóa. Bài hát phải được phát liên tục theo thời gian`0`, nên không bao giờ có thời gian nhàn rỗi. Có sẵn ba bài hát thông thường, có độ dài`A`,`B`, Và`C`và mỗi trong số chúng có thể được chọn nhiều lần. Ngoài ra còn có một bài hát đặc biệt, Những bài hát vàng, có độ dài cố định ở`768`giây. 

Việc khóa xảy ra chính xác vào thời điểm`T`. Một bài hát mới vẫn có thể được bắt đầu bất cứ lúc nào trước khi khóa, nhưng khi đồng hồ điểm`T`, không thể chọn thêm bài hát nào nữa. Vì bài hát đang phát được phép kết thúc nên bài hát cuối cùng thường phải là Bài hát vàng vì nó dài hơn nhiều so với những bài khác. 

Nhiệm vụ là tối đa hóa tổng thời gian hát. Điều đó có nghĩa là chúng tôi muốn sắp xếp các bài hát thông thường trước Bài hát vàng sao cho thời gian bắt đầu của Bài hát vàng càng muộn càng tốt nhưng vẫn ít hơn một cách nghiêm ngặt.`T`. 

Giá trị lớn nhất của`T`chỉ là`18000`, trong khi mỗi bài hát bình thường nằm giữa`180`Và`300`dài vài giây. Điều này ngay lập tức loại trừ việc thử mọi chuỗi bài hát có thể có, vì số lượng chuỗi tăng theo cấp số nhân với số lượng bài hát được chơi. Mặt khác, giới hạn thời gian đủ nhỏ để bất kỳ thuật toán nào tỷ lệ thuận với`T`đủ nhanh một cách dễ dàng. 

Một sai lầm dễ mắc phải là quên rằng Những Bài Hát Vàng có thể được bắt đầu vào một thời điểm nào đó.`0`. Coi như```
3 200 190 180
```Không có bài hát bình thường nào phù hợp trước ổ khóa. Câu trả lời đúng là`768`, vì Bài hát vàng có thể được bắt đầu ngay lập tức. 

Một trường hợp tế nhị khác là khi thời gian còn lại không thể được điền chính xác. Ví dụ,```
400 180 200 300
```Tiền tố thông thường tốt nhất kéo dài`200`giây. Những bài hát vàng bắt đầu vào lúc`200`, mang lại tổng cộng`968`. Chờ đợi đến lúc`399`là không thể vì thời gian nhàn rỗi bị cấm. 

Nguồn lỗi thứ ba là do quy tắc khóa. Giả định```
600 300 200 180
```Hai`300`-Bài hát thứ hai kết thúc đúng lúc`600`. Khi đó không thể bắt đầu Bài hát vàng vì màn hình đã bị khóa. Sự lựa chọn hợp lệ tốt nhất là`200`-bài hát thứ hai theo sau là hai`180`-Bài hát thứ hai, đạt đến thời gian`560`, sau đó bắt đầu Những bài hát vàng. Câu trả lời là`1328`, không`1368`. 

## Phương pháp tiếp cận 

Tìm kiếm brute-force trực tiếp tạo ra mọi chuỗi có thể có của các bài hát thông thường có tổng thời lượng ở mức dưới`T`. Bất cứ khi nào không thể thêm thêm bài hát nào nữa, thời lượng hiện tại được coi là thời điểm bắt đầu của Bài hát vàng. Vì mỗi vị trí đều có ba lựa chọn nên số dãy tăng dần theo`3^k`, Ở đâu`k`là số lượng bài hát thông thường được chơi. Với độ dài bài hát khoảng`180`giây,`k`có thể đạt được khoảng`100`, làm cho việc tìm kiếm hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là chỉ có thời gian tích lũy mới quan trọng. Thứ tự của các bài hát dẫn đến cùng một khoảng thời gian đã trôi qua không ảnh hưởng gì đến các lựa chọn trong tương lai. Điều này có nghĩa là vấn đề thực sự đang hỏi thời gian nào dưới đây`T`có thể truy cập được bằng cách thêm liên tục`A`,`B`, hoặc`C`. 

Điều đó ngay lập tức gợi ý lập trình động theo thời gian đã trôi qua. Cho phép`dp[t]`cho biết liệu có thể chi tiêu chính xác hay không`t`giây chỉ sử dụng các bài hát thông thường. Chúng tôi bắt đầu với`dp[0] = True`. Bất cứ khi nào có thể truy cập được một thời điểm, việc thêm bất kỳ độ dài nào trong số ba bài hát sẽ tạo ra một thời gian có thể truy cập khác, miễn là thời lượng đó vẫn thấp hơn`T`. 

Sau khi tính toán tất cả thời gian có thể truy cập, chúng ta chỉ cần tìm giá trị lớn nhất có thể truy cập nhỏ hơn`T`. Những bài hát vàng bắt đầu từ đó, và câu trả lời là thời gian cộng thêm`768`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Đệ quy theo cấp số nhân | Quá chậm | 
| Tối ưu | O(T) | O(T) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc bốn số nguyên`T`,`A`,`B`, Và`C`. 
2. Tạo một mảng boolean`dp`chiều dài`T`. chỉ mục`i`đại diện cho dù chính xác`i`giây của bài hát thông thường có thể được phát trước khi khóa. 
3. Đặt`dp[0] = True`bởi vì luôn luôn có thể sử dụng các bài hát không thông thường. 
4. Quét mọi lúc có thể từ`0`ĐẾN`T - 1`. Bất cứ khi nào`dp[i]`là đúng, hãy thử thêm từng độ dài trong ba độ dài bài hát thông thường. Nếu như`i + song < T`, đánh dấu thời gian mới đó là có thể truy cập được. 
5. Sau khi xử lý tất cả các trạng thái có thể truy cập, hãy quét mảng một lần nữa và ghi lại thời gian có thể truy cập lớn nhất. 
6. Xuất ra cộng thêm thời gian có thể đạt được tối đa`768`, vì Bài hát vàng bắt đầu ngay sau các bài hát thông thường và luôn được phát hoàn chỉnh. 

### Tại sao nó hoạt động 

Bất biến quy hoạch động là sau khi xử lý một thời gian có thể truy cập`x`, bất cứ khi nào có thể đạt được bằng cách kéo dài lịch trình đó với một bài hát thông thường bổ sung cũng được đánh dấu là có thể truy cập được. Vì mọi trình tự đều bắt đầu vào thời điểm`0`và mỗi chuỗi được xây dựng bằng cách liên tục nối thêm một bài hát, mỗi thời gian trôi qua hợp lệ bên dưới`T`cuối cùng cũng được phát hiện. 

Ngược lại, mọi trạng thái được đánh dấu đều tương ứng với một chuỗi bài hát thực tế vì các trạng thái chỉ được tạo bằng cách thêm một độ dài bài hát hợp pháp vào trạng thái đã hợp lệ. Thời gian có thể tiếp cận lớn nhất chính xác là thời điểm muộn nhất có thể khi Golden Songs có thể bắt đầu hợp pháp, do đó, việc thêm thời lượng cố định sẽ tạo ra tổng thời gian hát tối đa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

T, A, B, C = map(int, input().split())

dp = [False] * T
dp[0] = True

songs = (A, B, C)

for t in range(T):
    if not dp[t]:
        continue
    for s in songs:
        nt = t + s
        if nt < T:
            dp[nt] = True

best = 0
for t in range(T):
    if dp[t]:
        best = t

print(best + 768)
```Mảng lập trình động lưu trữ chính xác thời gian đã trôi qua có thể truy cập được trước khi khóa. Vì mỗi lần chuyển đổi đều tăng thời gian ít nhất`180`giây, không thể thực hiện các chu kỳ và chỉ cần quét từ trái sang phải đơn giản là đủ. 

Việc so sánh sử dụng`nt < T`, không`<= T`. Bắt đầu bài hát vàng chính xác vào thời điểm`T`bị cấm vì màn hình khóa ngay lúc đó. Đây là lỗi thường gặp nhất trong vấn đề này. 

Lần quét cuối cùng chỉ giữ giá trị lớn nhất có thể truy cập được. Nếu không có bài hát bình thường nào phù hợp cả, chỉ`dp[0]`là đúng, nên câu trả lời tự nhiên trở thành`768`. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào```
601 180 210 300
```| Thời gian có thể truy cập hiện tại | Lần mới đạt | 
| --- | --- | 
| 0 | 180, 210, 300 | 
| 180 | 360, 390, 480 | 
| 210 | 420, 510 | 
| 300 | 600 | 
| ... | ... | 

Thời gian có thể truy cập lớn nhất bên dưới`601`là`600`. 

| Thời gian có thể tiếp cận tốt nhất | Tổng số câu trả lời | 
| --- | --- | 
| 600 | 1368 | 

Ví dụ này cho thấy việc lấp đầy gần như toàn bộ thời gian có sẵn trước khi bắt đầu Bài hát vàng là tối ưu. 

### Mẫu 2 

đầu vào```
3 200 190 180
```| Thời gian có thể truy cập hiện tại | Lần mới đạt | 
| --- | --- | 
| 0 | Không có | 

Chỉ có thời gian`0`có thể truy cập được. 

| Thời gian có thể tiếp cận tốt nhất | Tổng số câu trả lời | 
| --- | --- | 
| 0 | 768 | 

Điều này xác nhận rằng Bài hát vàng có thể được phát ngay lập tức khi không có bài hát thông thường nào phù hợp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(T) | Mỗi lần có thể truy cập sẽ thử ba lần chuyển đổi | 
| Không gian | O(T) | Mảng khả năng tiếp cận Boolean | 

Với`T ≤ 18000`, thuật toán chỉ thực hiện vài chục nghìn thao tác, nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def solve():
    input = sys.stdin.readline

    T, A, B, C = map(int, input().split())

    dp = [False] * T
    dp[0] = True

    for t in range(T):
        if not dp[t]:
            continue
        for s in (A, B, C):
            if t + s < T:
                dp[t + s] = True

    best = 0
    for t in range(T):
        if dp[t]:
            best = t

    print(best + 768)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    old = sys.stdout
    sys.stdout = out
    solve()
    sys.stdout = old
    return out.getvalue().strip()

# provided samples
assert run("601 180 210 300\n") == "1368", "sample 1"
assert run("3 200 190 180\n") == "768", "sample 2"

# custom cases
assert run("180 180 180 180\n") == "768", "cannot start ordinary song at T"
assert run("181 180 180 180\n") == "948", "ordinary song fits once"
assert run("540 180 180 180\n") == "1128", "exactly reaches T, not allowed"
assert run("18000 300 300 300\n") == "18768", "maximum input"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`180 180 180 180`|`768`| Ranh giới nơi những bài hát bình thường hoàn toàn bình đẳng`T`| 
|`181 180 180 180`|`948`| Giá trị nhỏ nhất cho phép một bài hát thông thường | 
|`540 180 180 180`|`1128`| Đến chính xác tại`T`không được sử dụng | 
|`18000 300 300 300`|`18768`| Hạn chế tối đa | 

## Vỏ cạnh 

Hãy xem xét```
3 200 190 180
```Bảng lập trình động chỉ đánh dấu`0`có thể truy cập được vì mọi bài hát thông thường đều vượt quá thời gian có sẵn. Thuật toán trả về`0 + 768 = 768`, nhận ra chính xác rằng Bài hát vàng phải được bắt đầu ngay lập tức. 

Bây giờ hãy xem xét```
400 180 200 300
```Thời gian có thể tiếp cận là`0`,`180`,`200`,`300`, Và`380`. Thời gian`400`không thể truy cập được vì nó không hoàn toàn nhỏ hơn`T`. Thuật toán chọn`380`, sản xuất`1148`, hoặc nếu chỉ`200`có thể truy cập được theo các độ dài bài hát khác nhau, thay vào đó, nó sẽ chọn chính xác điều đó. Không bao giờ có tình trạng chờ đợi không hoạt động vì mọi trạng thái có thể truy cập đều tương ứng với quá trình phát lại không bị gián đoạn. 

Cuối cùng, hãy xem xét```
600 300 200 180
```Các dấu mảng lập trình động`560`càng có thể truy cập được, trong khi`600`bị bỏ qua vì quá trình chuyển đổi sử dụng điều kiện`next < T`. Thuật toán bắt đầu Bài hát vàng vào lúc`560`và trả về`1328`, khớp chính xác với quy tắc khóa.
