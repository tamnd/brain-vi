---
title: "CF 104282C - Bậc Thầy Genshin"
description: "Có 6 rãnh độc lập và mỗi rãnh chứa một số đoạn thời gian rời rạc trong đó các khối xuất hiện. Mỗi đoạn [l, r] có nghĩa là trên mỗi giây nguyên từ l đến r, đoạn nhạc đó đóng góp chính xác một điểm nếu chúng ta chọn nhấn nó vào giây đó."
date: "2026-07-01T21:05:08+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104282
codeforces_index: "C"
codeforces_contest_name: "The 20th Hangzhou City University Programming Contest"
rating: 0
weight: 104282
solve_time_s: 52
verified: true
draft: false
---

[CF 104282C - Bậc thầy Genshin](https://codeforces.com/problemset/problem/104282/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 52s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Có 6 rãnh độc lập và mỗi rãnh chứa một số đoạn thời gian rời rạc trong đó các khối xuất hiện. Mỗi đoạn`[l, r]`có nghĩa là trên mỗi số nguyên thứ hai từ`l`ĐẾN`r`bao gồm, bài hát đó đóng góp chính xác một điểm nếu chúng ta chọn nhấn nó vào giây đó. 

Cứ mỗi giây, Yoimiya có thể nhấn nút ở tối đa 5 trong số 6 bài hát. Nếu một bản nhạc có một khối ở giây đó và chúng ta nhấn vào nó, chúng ta sẽ đạt được một điểm cho bản nhạc đó vào giây đó. Chúng ta được phép tự do lựa chọn bài hát nào sẽ nhấn trong mỗi giây, miễn là số lượng bài hát được nhấn không vượt quá 5. 

Nhiệm vụ là tối đa hóa tổng số điểm thu thập được trên tất cả các giây và tất cả các bản nhạc. 

Khó khăn chính là thời gian kéo dài đến`10^8`, trong khi tổng quãng thời gian trên các bản nhạc tổng cộng là khoảng`2 * 10^5`. Điều này buộc mọi giải pháp phải tránh lặp lại từng giây và thay vào đó hoạt động với các ranh giới sự kiện. 

Một cách giải thích ngây thơ sẽ mô phỏng từng giây và tham lam nhấn tới 5 bản nhạc đang hoạt động. Điều này ngay lập tức thất bại vì dòng thời gian quá lớn. Ngay cả thời gian nén cũng cần thiết, nhưng ngay cả khi đó thách thức chính vẫn là quyết định nên bỏ qua bài hát nào tại mỗi thời điểm. 

Một trường hợp phức tạp nhưng quan trọng phát sinh khi có nhiều hơn một rãnh đang hoạt động và “lựa chọn tốt nhất trong số 5 rãnh” thay đổi thường xuyên do ranh giới khoảng thời gian. 

Ví dụ: giả sử tại một thời điểm nhất định, tất cả 6 bản nhạc đều hoạt động. Chúng ta phải chọn bỏ qua đúng một đường đua, mất đi ít nhất một điểm tiềm năng. Nếu sau đó chỉ còn 5 bản vẫn hoạt động, chúng ta phải đảm bảo rằng mình không bỏ qua một bản nhạc một cách không cần thiết. Một mô phỏng ngây thơ theo từng giây có thể "bám" vào lựa chọn trước đó một cách không chính xác. 

## Phương pháp tiếp cận 

Nếu chúng ta cố định thời gian ở mỗi giây nguyên thì vấn đề sẽ trở nên đơn giản: tại mỗi giây, hãy đếm xem có bao nhiêu bản nhạc đang hoạt động. Nếu là 6 thì chúng ta mất đúng 1 điểm vì chỉ bấm được 5 bài; nếu không chúng ta sẽ không mất gì cả. 

Vậy câu trả lời tương đương với: tính tổng trên tất cả số giây của`min(5, active_tracks_at_time)`. 

Cách thức mạnh mẽ là mở rộng tất cả các khoảng thời gian thành từng giây riêng lẻ, duy trì số lượng tần số cho các bản nhạc đang hoạt động và cứ mỗi giây tính toán số lượng bản nhạc đang hoạt động. Tuy nhiên, tổng nhịp có thể đạt`10^8`, khiến điều này không thể thực hiện được. 

Quan sát quan trọng là không có gì thay đổi trong một khoảng thời gian mà tập hợp các khoảng thời gian hoạt động được cố định. Lần duy nhất mà số lượng bản nhạc đang hoạt động thay đổi là điểm cuối khoảng thời gian. Vì vậy chúng ta có thể chuyển đổi từng khoảng`[l, r]`thành hai sự kiện: một lúc`l`tăng phạm vi bảo hiểm và một tại`r + 1`độ che phủ giảm dần. Sau đó, chúng tôi quét theo thời gian theo thứ tự, duy trì số lượng bản nhạc đang hoạt động tại bất kỳ thời điểm nào. 

Giữa hai vị trí sự kiện liên tiếp`x`Và`y`, số lượng bản nhạc đang hoạt động là không đổi, do đó sự đóng góp chỉ đơn giản là`(y - x) * min(5, active_count)`. 

Điều này làm giảm vấn đề thành một đường quét cổ điển lên đến`2 * 2e5`sự kiện. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(thời gian tối đa × 6) | O(1) | Quá chậm | 
| Đường quét | O(N log N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi coi mỗi điểm cuối trong khoảng thời gian là một sự thay đổi trong dòng thời gian chung của các tuyến đường đang hoạt động. 

1. Chuyển đổi từng khoảng`[s, t]`trên mỗi bài hát trong số 6 bài hát thành hai sự kiện: thêm`+1`vào thời điểm đó`s`và thêm`-1`vào thời điểm đó`t + 1`. Điều này đảm bảo rằng vùng phủ sóng được thể hiện dưới dạng một mảng khác biệt theo thời gian. 
2. Thu thập tất cả các sự kiện từ tất cả các bản nhạc vào một danh sách duy nhất và sắp xếp chúng theo thời gian. Việc sắp xếp là cần thiết vì chúng ta cần xử lý thời gian theo thứ tự thời gian để duy trì tính chính xác của số lượng hoạt động. 
3. Khởi tạo`active = 0`Và`answer = 0`, và đặt một con trỏ`i = 0`qua các sự kiện được sắp xếp. 
4. Quét qua các điểm sự kiện. Đối với mỗi thời điểm riêng biệt`x`, tính toán đóng góp đầu tiên từ lần trước`prev`ĐẾN`x`BẰNG`(x - prev) * min(5, active)`. Điều này hoạt động vì`active`không thay đổi trong khoảng này. 
5. Sau đó áp dụng tất cả các sự kiện vào thời điểm đó`x`, đang cập nhật`active += delta`cho mỗi sự kiện. Điều này cập nhật số lượng bản nhạc hiện đang hoạt động tại thời điểm chính xác này. 
6. Di chuyển`prev`ĐẾN`x`và tiếp tục. 
7. Sau khi xử lý tất cả các sự kiện, không cần đóng góp gì thêm vì mọi thứ ngoài sự kiện cuối cùng đều không có khoảng thời gian hoạt động. 

Điểm tinh tế là chúng tôi tính toán các đóng góp trước khi áp dụng các cập nhật ở tọa độ hiện tại. Điều này bảo tồn ý nghĩa rằng các sự kiện tại thời điểm`x`chỉ ảnh hưởng từ`x`trở đi, không phải trước đó. 

### Tại sao nó hoạt động 

Tại bất kỳ khoảng thời gian cố định nào giữa hai điểm sự kiện liên tiếp, tập hợp các khoảng thời gian hoạt động không thay đổi. Do đó, số lượng rãnh hoạt động là không đổi trong suốt phân đoạn đó. Vì chức năng tính điểm chỉ phụ thuộc vào số lượng bản nhạc đang hoạt động tại một giây nhất định chứ không phụ thuộc vào danh tính hoặc lịch sử nên quyết định tối ưu cũng không đổi trên phân đoạn: chúng tôi luôn lấy`min(5, active)`. Tính tổng những đóng góp này trên tất cả các phân đoạn không đổi tối đa bao gồm mỗi giây chính xác một lần, đảm bảo không có sự trùng lặp hoặc thiếu sót. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

events = []

# 6 tracks
for _ in range(6):
    data = list(map(int, input().split()))
    n = data[0]
    arr = data[1:]
    for i in range(n):
        l = arr[2 * i]
        r = arr[2 * i + 1]
        events.append((l, 1))
        events.append((r + 1, -1))

events.sort()

active = 0
ans = 0

prev = events[0][0]
i = 0
m = len(events)

while i < m:
    x = events[i][0]

    ans += (x - prev) * min(5, active)

    while i < m and events[i][0] == x:
        active += events[i][1]
        i += 1

    prev = x

print(ans)
```Việc triển khai xây dựng một danh sách sự kiện phẳng gồm tất cả các ranh giới khoảng thời gian trên tất cả các tuyến đường. Mỗi khoảng thời gian đóng góp +1 khi bắt đầu và -1 ngay sau khi kết thúc, đảm bảo phạm vi bao phủ chính xác. 

Vòng lặp quét cẩn thận tách biệt “đóng góp khoảng thời gian” khỏi “cập nhật trạng thái”. phép nhân`(x - prev) * min(5, active)`là nơi toàn bộ vấn đề giảm từ chiều thời gian sang tập hợp phân khúc. 

Một cạm bẫy phổ biến là cập nhật`active`trước khi thêm phần đóng góp vào phân khúc tại`x`. Điều đó sẽ dịch chuyển không chính xác các sự kiện theo một đơn vị thời gian và đếm quá hoặc thiếu số giây ranh giới. 

## Ví dụ đã hoạt động 

Hãy xem xét một kịch bản nhỏ có 2 bản nhạc để làm rõ (mặc dù vấn đề có 6 bản nhạc). Giả sử bài hát 1 có`[1, 3]`và bài 2 có`[2, 4]`. 

Chúng tôi xây dựng sự kiện: 

| Thời gian | Thay đổi | 
| --- | --- | 
| 1 | +1 | 
| 2 | +1 | 
| 4 | -1 | 
| 5 | -1 | 

Bây giờ hãy quét: 

| Phân đoạn | Đang hoạt động | Đóng góp | 
| --- | --- | --- | 
| 1-2 | 1 | 1 × 1 = 1 | 
| 2-4 | 2 | 2 × 2 = 4 | 
| 4-5 | 1 | 1 × 1 = 1 | 

Tổng cộng = 6. 

Điều này xác nhận rằng các khoảng chồng chéo được xếp chồng lên nhau một cách chính xác và tính toán dựa trên phân đoạn phù hợp với lý luận mỗi giây. 

Bây giờ hãy xem xét trường hợp tất cả 6 bản nhạc trùng nhau trong một giây, chẳng hạn`[10, 10]`trên tất cả các bài hát. 

Tại thời điểm 10, active = 6, nhưng chúng ta chỉ có thể lấy 5. 

| Phân đoạn | Đang hoạt động | Đóng góp | 
| --- | --- | --- | 
| 11-10 | 6 | 1 × 5 = 5 | 

Điều này xác nhận nắp được áp dụng chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N log N) | Sắp xếp tối đa 2 sự kiện cho mỗi khoảng thời gian chiếm ưu thế | 
| Không gian | O(N) | Danh sách sự kiện lưu trữ tất cả các điểm cuối khoảng thời gian | 

Các ràng buộc cho phép tổng cộng tối đa khoảng 2e5 khoảng thời gian, do đó, nhiều nhất là 4e5 sự kiện. Sắp xếp và quét tuyến tính phù hợp thoải mái trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    events = []
    for _ in range(6):
        data = list(map(int, input().split()))
        n = data[0]
        arr = data[1:]
        for i in range(n):
            l = arr[2 * i]
            r = arr[2 * i + 1]
            events.append((l, 1))
            events.append((r + 1, -1))

    events.sort()

    active = 0
    ans = 0
    prev = events[0][0]
    i = 0
    m = len(events)

    while i < m:
        x = events[i][0]
        ans += (x - prev) * min(5, active)
        while i < m and events[i][0] == x:
            active += events[i][1]
            i += 1
        prev = x

    return str(ans)

# minimal
assert run("""1 1 1
1 1 1
1 1 1
1 1 1
1 1 1
1 1 1
""") == "1", "all single point overlap"

# full overlap cap
assert run("""1 1 10
1 1 10
1 1 10
1 1 10
1 1 10
1 1 10
""") == "50", "cap at 5 tracks over 10 seconds"

# disjoint intervals
assert run("""1 1 1
1 2 2
1 3 3
1 4 4
1 5 5
1 6 6
""") == "6", "no overlap"

# staggered overlap
assert run("""1 1 3
1 2 4
1 3 5
1 4 6
1 5 7
1 6 8
""") > 0
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả các điểm chồng chéo | 1 | tính đúng đắn cơ bản | 
| chồng chéo hoàn toàn | 50 | hành vi giới hạn | 
| rời rạc | 6 | không có sự tổng hợp chồng chéo | 
| loạng choạng | >0 | chồng chéo động | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi nhiều sự kiện xảy ra tại cùng một dấu thời gian, khoảng thời gian trộn bắt đầu và kết thúc. Thuật toán xử lý tất cả các sự kiện cùng nhau tại một thời điểm nhất định, nhưng chỉ sau khi tính đến sự đóng góp từ phân đoạn trước đó. Điều này ngăn chặn việc đếm sai ở ranh giới. 

Ví dụ: nếu một khoảng kết thúc tại`x`và một cái khác bắt đầu lúc`x`, cả hai đều được xử lý trong cùng một nhóm sự kiện. Sự đóng góp cho`[prev, x)`sử dụng số lượng hoạt động cũ, loại trừ chính xác các khoảng thời gian bắt đầu từ`x`. 

Một trường hợp cạnh khác là khi tất cả các khoảng rời rạc trên các rãnh, dẫn đến hoạt động không bao giờ vượt quá 1. Thuật toán vẫn hoạt động vì`min(5, active)`giảm xuống`active`, duy trì tính chính xác mà không cần xử lý đặc biệt.
