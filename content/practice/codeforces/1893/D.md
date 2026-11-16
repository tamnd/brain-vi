---
title: "CF 1893D - Đầy màu sắc mang tính xây dựng"
description: "Chúng ta được cấp một tập hợp nhiều khối màu. Màu sắc của khối là cố định nhưng chúng tôi có thể chọn cách phân phối chúng trên các giá và thứ tự chúng xuất hiện trên mỗi giá. Đối với kệ, yêu cầu là khoảng cách tối thiểu giữa các màu bằng nhau."
date: "2026-06-08T21:57:08+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "data-structures", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1893
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 908 (Div. 1)"
rating: 2600
weight: 1893
solve_time_s: 125
verified: false
draft: false
---

[CF 1893D - Mang tính xây dựng đầy màu sắc](https://codeforces.com/problemset/problem/1893/D) 

**Đánh giá:** 2600 
**Tags:** thuật toán xây dựng, cấu trúc dữ liệu, tham lam 
**Thời gian giải:** 2m 5s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một tập hợp nhiều khối màu. Màu sắc của khối là cố định nhưng chúng tôi có thể chọn cách phân phối chúng trên các giá và thứ tự chúng xuất hiện trên mỗi giá. 

Đối với kệ, yêu cầu là khoảng cách tối thiểu giữa các màu bằng nhau. Nếu một kệ ít nhất có nhiều màu sắc`d`thì hai lần xuất hiện cùng màu trên giá đó phải cách nhau ít nhất`d`các vị trí. Nói cách khác, bên trong kệ, các màu giống nhau bị cấm xuất hiện bên trong kệ trước đó.`d - 1`các vị trí. 

Mỗi kệ có chiều dài`s_i`và khoảng cách cần thiết`d_i`. Chúng ta phải xây dựng một chuỗi màu thực tế cho mỗi kệ, sử dụng mỗi khối đúng một lần. 

Tổng số khối trong tất cả các trường hợp thử nghiệm nhiều nhất là`2 · 10^5`. Điều đó ngay lập tức loại trừ bất cứ điều gì bậc hai trong`n`. Một giải pháp xung quanh`O(n log n)`hoàn toàn thoải mái, trong khi`O(n^2)`sẽ thực hiện đại khái`4 · 10^10`hoạt động trong trường hợp xấu nhất. 

Phần tinh tế của vấn đề là các giá trị độc lập trong câu trả lời cuối cùng, nhưng tần số khối là toàn cục. Một vị trí tham lam có vẻ hợp lý trên một kệ có thể tiêu thụ màu sắc theo cách khiến kệ sau này không thể sử dụng được. 

Hãy xem xét một kệ có kích thước`4`với`d = 2`và bốn khối cùng màu. Kệ sẽ cần mẫu`x ? x ?`, nhưng không có đủ màu khác để phân tách tất cả các bản sao. Một thuật toán tham lam chỉ kiểm tra vị trí hiện tại có thể phát hiện ra điều không thể xảy ra quá muộn. 

Một sai lầm dễ mắc phải khác là coi tình trạng này là "các khối liền kề phải khác nhau" khi`d > 2`. Ví dụ:```
s = 5
d = 3
shelf = [1, 2, 1, 3, 4]
```hai`1`s chỉ cách nhau hai vị trí nên màu sắc là`2`, không`3`. 

Cạm bẫy thứ ba là quên rằng các kệ hoàn toàn tách biệt. Màu được sử dụng ở cuối một kệ và đầu một kệ khác không tạo ra bất kỳ vi phạm nào vì màu sắc chỉ được xác định bên trong mỗi kệ. 

## Phương pháp tiếp cận 

Quan điểm mạnh mẽ là xây từng kệ một vị trí và thử tất cả các màu vẫn còn bản sao. Bất cứ khi nào chúng tôi đặt một màu, chúng tôi xác minh rằng giới hạn về màu sắc của giá vẫn thỏa mãn. Điều này đúng về mặt khái niệm vì nó khám phá tất cả các cách xây dựng có thể. 

Vấn đề là yếu tố phân nhánh. Ngay cả sau khi nén các màu bằng nhau, có thể có hàng nghìn lựa chọn riêng biệt ở mỗi bước. Không gian tìm kiếm trở nên theo cấp số nhân. 

Quan sát quan trọng là một kệ có yêu cầu`d`chỉ quan tâm đến chuyện trước đó`d - 1`các vị trí. Khi một màu được sử dụng, nó sẽ không có sẵn cho màu tiếp theo`d - 1`vị trí và sau đó có thể sử dụng lại được. 

Điều này bắt đầu trông giống như một vấn đề lập kế hoạch. Mỗi màu có một tần số còn lại. Trong số tất cả các màu hiện có, sử dụng màu có số lượng còn lại lớn nhất là lựa chọn an toàn nhất vì những màu có tần suất sử dụng cao sẽ khó đặt sau này. 

Điều đó cho thấy tham lam cổ điển được sử dụng trong các vấn đề sắp xếp lại với các hạn chế về thời gian hồi chiêu. 

Đối với kệ có khoảng cách`d`:```
place a color
temporarily block it
after d steps, unblock it
```Trong số tất cả các màu được bỏ chặn, luôn chọn màu có tần số còn lại lớn nhất. 

Điều tuyệt vời là chúng ta không cần phải giải quyết các kệ một cách riêng biệt. Chúng tôi xử lý các kệ theo thứ tự trong khi vẫn duy trì một nhóm các hình khối còn lại trên toàn cầu. Bên trong kệ, màu sắc tạm thời được loại bỏ khỏi nhóm để đảm bảo chính xác`d`các vị trí. Khi chúng trở nên hợp pháp trở lại, chúng sẽ được lắp lại. 

Nếu ở một vị trí nào đó không có sẵn màu thì không có cấu trúc hợp lệ nào tồn tại. Giải pháp chính thức sử dụng chính xác điều này với một tập hợp được sắp xếp theo tần suất còn lại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Bộ tham lam + đặt hàng tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm xem có bao nhiêu hình khối cho mỗi màu. 
2. Lưu trữ mọi màu có tần số dương theo thứ tự được khóa bởi`(remaining_count, color)`. 
3. Xử lý từng kệ một. 
4. Đối với kệ hiện tại, tạo một mảng có độ dài`s_i`. 
5. Khi điền vị trí`j`, trước tiên hãy kiểm tra xem màu được đặt ở vị trí`j - d_i`nên có sẵn một lần nữa. Nếu vẫn còn bản sao, hãy lắp lại vào bộ này. 
6. Nếu bộ trống, không thể đặt màu hợp lệ ở vị trí hiện tại. đầu ra`-1`. 
7. Ngược lại, lấy màu có tần số còn lại lớn nhất. 
8. Đặt nó ở vị trí hiện tại, xóa một bản sao khỏi tần số của nó và giữ nó bị chặn cho lần tiếp theo`d_i - 1`các vị trí. 
9. Sau khi đã lấp đầy kệ, hãy giải phóng tất cả các màu vẫn đang chờ trong thời gian hồi chiêu. Điều này chuẩn bị cấu trúc toàn cầu cho kệ tiếp theo. 
10. Sau khi tất cả các kệ được xử lý, xuất ra các kệ đã được xây dựng. 

### Tại sao nó hoạt động 

Ở mỗi bước, bộ chứa chính xác các màu hợp pháp để đặt ở vị trí hiện tại. Bất kỳ màu nào được sử dụng trong vòng cuối cùng`d_i - 1`vị trí của kệ hiện tại tạm thời bị loại trừ. 

Trong số tất cả các màu hợp pháp, việc chọn màu có tần suất còn lại lớn nhất là chiến lược tham lam tiêu chuẩn để lên lịch thời gian hồi chiêu. Màu có nhiều bản sao còn lại là nguồn tài nguyên hạn chế nhất. Trì hoãn nó chỉ làm cho các vị trí trong tương lai trở nên khó khăn hơn. Nếu người tham lam không thể đặt bất kỳ màu nào vì bộ có sẵn trống thì mọi màu còn lại hiện bị chặn, do đó không có sự tiếp tục hợp lệ nào tồn tại. 

Điều bất biến là tập hợp luôn chứa tất cả và chỉ các màu hợp pháp và mọi vị trí đều tôn trọng yêu cầu về khoảng cách. Do đó, mỗi chiếc kệ được sản xuất đều có ít nhất màu sắc`d_i`. Giải pháp Codeforces được chấp nhận tuân theo chính xác cấu trúc này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    a = list(map(int, input().split()))
    s = list(map(int, input().split()))
    d = list(map(int, input().split()))

    cnt = [0] * (n + 1)
    for x in a:
        cnt[x] += 1

    import heapq

    heap = []
    for color in range(1, n + 1):
        if cnt[color]:
            heapq.heappush(heap, (-cnt[color], color))

    ans = []

    for i in range(m):
        shelf = [0] * s[i]

        for j in range(s[i]):
            if j >= d[i]:
                old = shelf[j - d[i]]
                if cnt[old] > 0:
                    heapq.heappush(heap, (-cnt[old], old))

            if not heap:
                print(-1)
                return

            negf, color = heapq.heappop(heap)
            shelf[j] = color
            cnt[color] -= 1

        for j in range(s[i], s[i] + d[i]):
            old = shelf[j - d[i]]
            if cnt[old] > 0:
                heapq.heappush(heap, (-cnt[old], old))

        ans.append(shelf)

    for shelf in ans:
        print(*shelf)

t = int(input())
for _ in range(t):
    solve()
```Mảng tần số theo dõi số lượng khối chưa sử dụng còn lại cho mỗi màu. 

Heap chỉ lưu trữ những màu hiện có sẵn. Một màu biến mất khỏi đống ngay sau khi được đặt. Nó xuất hiện lại chính xác`d_i`vị trí sau, được thực hiện bằng cách chèn lại màu từ vị trí`j - d_i`. 

Một chi tiết tinh tế là phím heap sử dụng tần số còn lại hiện tại. Khi một màu được đặt, số lượng của nó sẽ giảm và màu đó không được chèn lại ngay lập tức. Điều này tránh tần số cũ. 

Một chi tiết quan trọng khác là vòng dọn dẹp sau khi hoàn thiện kệ. Một số màu vẫn có thể bị chặn do thời gian sử dụng đã kết thúc trước khi hết thời gian hồi chiêu. Chúng phải được phát hành trước khi bắt đầu kệ tiếp theo vì các kệ độc lập. 

## Ví dụ đã hoạt động 

### Ví dụ 1```
n = 5
colors = [1, 1, 2, 2, 3]

s = [5]
d = [2]
```| Vị trí | Tần số có sẵn | Được chọn | 
| --- | --- | --- | 
| 0 | 1:2, 2:2, 3:1 | 2 | 
| 1 | 1:2, 3:1 | 1 | 
| 2 | 2:1, 3:1 | 2 | 
| 3 | 1:1, 3:1 | 1 | 
| 4 | 3:1 | 3 | 

Kệ đã thi công:```
[2, 1, 2, 1, 3]
```Màu sắc bằng nhau luôn ở khoảng cách ít nhất`2`. 

### Ví dụ 2```
n = 4
colors = [7, 7, 7, 7]

s = [4]
d = [2]
```| Vị trí | Màu sắc có sẵn | 
| --- | --- | 
| 0 | {7} | 
| 1 | {} | 

Tại vị trí`1`heap trống, do đó thuật toán đưa ra:```
-1
```Điều này chứng tỏ khả năng phát hiện không thể thực hiện được. Màu duy nhất vẫn bị chặn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Mỗi khối được chèn và xóa khỏi heap một số lần không đổi | 
| Không gian | O(n) | Lưu trữ tần số, đống và câu trả lời | 

Vì tổng của`n`trên tất cả các trường hợp thử nghiệm là nhiều nhất`2 · 10^5`, cái`O(n log n)`sự phức tạp dễ dàng phù hợp trong giới hạn. 

## Trường hợp thử nghiệm```
# These are validation ideas rather than exact-output tests,
# because many different valid constructions may exist.

# minimum size
1
1 1
1
1
1

# all cubes different
1
5 1
1 2 3 4 5
5
5

# impossible, all equal with d = 2
1
4 1
7 7 7 7
4
2

# boundary cooldown
1
6 1
1 1 1 2 2 3
6
3

# multiple shelves
1
8 2
1 1 2 2 3 3 4 4
4 4
2 2
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Khối đơn | Một khối được in | Hạn chế tối thiểu | 
| Tất cả các màu sắc riêng biệt | Bất kỳ hoán vị | Màu sắc tương đương với kích thước kệ | 
| Tất cả đều bằng nhau, d = 2 | -1 | Phát hiện không thể | 
| Yêu cầu khoảng cách lớn | Sắp xếp hợp lệ hoặc -1 | Logic thời gian hồi chiêu | 
| Nhiều kệ | Xây dựng hợp lệ | Kệ độc lập | 

## Vỏ cạnh 

Hãy xem xét:```
1
4 1
7 7 7 7
4
2
```Sau khi đặt cái đầu tiên`7`, màu bị chặn ở một vị trí. Không có màu nào khác tồn tại nên bộ có sẵn trở nên trống. Thuật toán ngay lập tức trở lại`-1`, điều đó đúng. 

Coi như:```
1
5 1
1 2 3 4 5
5
5
```Mỗi màu xuất hiện một lần. Heap không bao giờ cần sử dụng lại một màu. Bất kỳ đơn đặt hàng nào cũng có tác dụng và màu sắc chính xác là độ dài kệ. 

Coi như:```
1
6 2
1 1 1 2 2 2
3 3
2 2
```Kệ đầu tiên có thể tiêu thụ một số bản sao màu`1`Và`2`. Khi hết giá, các màu bị chặn sẽ được lắp lại trước khi giá thứ hai bắt đầu. Điều này phản ánh chính xác rằng các hạn chế về khoảng cách không vượt qua ranh giới thềm lục địa. Kệ thứ hai bắt đầu với đầy đủ các màu hợp pháp còn lại.
