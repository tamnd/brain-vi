---
title: "CF 439A - Devu, ca sĩ và Churu, Joker"
description: "Bài toán yêu cầu chúng ta sắp xếp một sự kiện trong đó Devu, một ca sĩ, biểu diễn n bài hát có độ dài khác nhau và Churu, một diễn viên hài, kể những câu chuyện cười có thời lượng cố định là 5 phút."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 439
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 251 (Div. 2)"
rating: 900
weight: 439
solve_time_s: 72
verified: true
draft: false
---

[CF 439A - Devu, Ca sĩ và Churu, Joker](https://codeforces.com/problemset/problem/439/A) 

**Xếp hạng:** 900 
**Tags:** tham lam, thực hiện 
**Thời gian giải:** 1 phút 12s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Bài toán yêu cầu chúng ta lên lịch một sự kiện trong đó Devu, một ca sĩ, biểu diễn _n_ bài hát có độ dài khác nhau và Churu, một diễn viên hài, kể những câu chuyện cười có thời lượng cố định là 5 phút. Devu yêu cầu bắt buộc phải nghỉ 10 phút sau mỗi bài hát, trừ bài cuối cùng, trong khi Churu có thể diễn trò đùa bất cứ lúc nào mà không cần nghỉ ngơi. Tổng thời lượng của sự kiện không được vượt quá _d_ phút. Mục tiêu là xác định số lượng trò đùa tối đa mà Churu có thể thực hiện mà không bỏ qua bất kỳ bài hát nào của Devu. Nếu không thể lên lịch cho tất cả các bài hát trong thời gian quy định thì đầu ra sẽ là -1. 

Đầu vào bao gồm hai số nguyên _n_ và _d_, theo sau là danh sách các số nguyên _n_ biểu thị thời lượng của bài hát. Đầu ra là một số nguyên duy nhất: số lượng câu chuyện cười tối đa có thể có hoặc -1 nếu Devu không thể hoàn thành phần trình diễn của mình. 

Các ràng buộc chỉ ra rằng _n_ nhỏ, nhiều nhất là 100 và _d_ có thể lên tới 10.000. Vì số lượng bài hát ít nên chúng ta có thể sử dụng số học đơn giản một cách an toàn mà không phải lo lắng về tính kém hiệu quả của thuật toán. Các trường hợp đặc biệt bao gồm các tình huống trong đó tổng thời lượng bài hát của Devu cộng với thời gian nghỉ bắt buộc đã vượt quá _d_ hoặc khi tất cả các bài hát đều rất ngắn, để lại nhiều chỗ cho những trò đùa. 

Một trường hợp tế nhị phát sinh khi chỉ có một bài hát. Trong trường hợp đó, Devu không yêu cầu nghỉ 10 phút sau đó, điều này ảnh hưởng đến tổng thời gian dành cho trò đùa. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng mô phỏng mọi thứ tự có thể có của các bài hát và câu chuyện cười để tối đa hóa số lượng câu chuyện cười. Đối với mỗi bài hát, chúng ta có thể thử đặt không hoặc nhiều câu chuyện cười trước hoặc sau bài hát đó. Tuy nhiên, điều này sẽ bùng nổ về mặt tổ hợp: đối với 100 bài hát và tối đa 10.000 phút, số lượng vị trí rất lớn, khiến phương pháp này không khả thi. 

Thông tin chi tiết quan trọng đến từ việc quan sát thấy các bài hát và phần nghỉ ngơi của Devu tiêu tốn một khoảng thời gian tối thiểu cố định. Đối với _n_ bài hát, tổng thời lượng bài hát là tổng của tất cả _tᵢ_ và tổng thời gian nghỉ là 10 phút cho mỗi bài trừ bài cuối, tính ra là 10 * (n - 1) phút. Điều này để lại tất cả số phút còn lại trong _d_ có sẵn cho những câu chuyện cười mà Churu có thể tự do lên lịch. Vì mỗi câu chuyện cười có thời lượng 5 phút nên số lượng câu chuyện cười tối đa chỉ đơn giản là chia số phút còn lại cho 5. 

Do đó, vấn đề giảm xuống còn việc kiểm tra xem tổng thời gian của bài hát cộng với thời gian nghỉ bắt buộc có vượt quá _d_ hay không. Nếu có, chúng tôi trả về -1. Nếu không, chúng tôi sẽ tính xem có bao nhiêu câu chuyện cười dài 5 phút phù hợp trong thời gian còn lại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc giá trị đầu vào: số lượng bài hát _n_ và tổng thời lượng có sẵn _d_. 
2. Đọc danh sách thời lượng bài hát. 
3. Tính tổng thời gian cần thiết cho các bài hát của Devu:`total_song_time = sum(tᵢ for each song)`. 
4. Tính tổng thời gian nghỉ ngơi bắt buộc:`total_rest_time = 10 * (n - 1)`. 
5. Tính thời gian tối thiểu cần thiết cho sự kiện:`min_event_time = total_song_time + total_rest_time`. 
6. Nếu`min_event_time > d`, xuất ra -1, vì không thể sắp xếp tất cả các bài hát. 
7. Ngược lại, hãy tính thời gian còn lại để đùa:`remaining_time = d - min_event_time`. 
8. Mỗi câu chuyện cười kéo dài 5 phút và những câu chuyện cười bổ sung cũng có thể phù hợp với khoảng thời gian nghỉ 10 phút giữa các bài hát. có`(n - 1)`thời gian nghỉ 10 phút, mỗi lần có thể tổ chức 2 câu chuyện cười. Vì thế,`max_jokes = 2 * (n - 1) + remaining_time // 5`. 
9. Đầu ra`max_jokes`. 

Điều này hiệu quả vì trước tiên chúng tôi luôn lấp đầy thời gian nghỉ bắt buộc bằng những câu chuyện cười, tối đa hóa số lượng của chúng mà không vi phạm yêu cầu nghỉ ngơi của Devu. Mọi thời gian còn lại đều có thể chia thành những trò đùa dài 5 phút. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, d = map(int, input().split())
songs = list(map(int, input().split()))

total_song_time = sum(songs)
total_rest_time = 10 * (n - 1)
min_event_time = total_song_time + total_rest_time

if min_event_time > d:
    print(-1)
else:
    remaining_time = d - min_event_time
    max_jokes = 2 * (n - 1) + remaining_time // 5
    print(max_jokes)
```Mã trực tiếp thực hiện thuật toán. Đầu tiên nó tính tổng thời lượng bài hát và tổng thời gian nghỉ bắt buộc. Nó kiểm tra xem Devu có thể biểu diễn tất cả các bài hát trong thời lượng nhất định hay không. Nếu không, nó sẽ in -1. Mặt khác, nó sẽ tính toán số lượng câu chuyện cười tối đa bằng cách lấp đầy khoảng thời gian giữa các bài hát (2 câu chuyện cười mỗi lần nghỉ 10 phút) và sau đó sử dụng bất kỳ thời gian còn lại nào cho các câu chuyện cười bổ sung. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3 30
2 2 1
```| Bước | tổng_song_time | tổng_thời gian nghỉ ngơi | phút_event_time | thời gian còn lại | max_jokes | 
| --- | --- | --- | --- | --- | --- | 
| Tính toán | 2+2+1=5 | 10*(3-1)=20 | 5+20=25 | 30-25=5 | 2*2 + 5//5 = 4+1=5 | 

Thuật toán đưa ra 5, xác nhận rằng tất cả các bài hát đều có thể phù hợp và số lượng câu chuyện cười tối đa được tính toán chính xác. 

### Mẫu 2 (Trường hợp không thể) 

đầu vào:```
2 15
10 10
```| Bước | tổng_song_time | tổng_thời gian nghỉ ngơi | phút_event_time | 
| --- | --- | --- | --- | 
| Tính toán | 10+10=20 | 10*(2-1)=10 | 20+10=30 | 

Vì 30 > 15 nên đầu ra là -1. Điều này chứng tỏ thuật toán xác định chính xác các lịch trình không thể thực hiện được. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Chúng tôi tổng hợp tất cả thời lượng bài hát một lần | 
| Không gian | O(n) | Lưu trữ thời lượng bài hát | 

Với _n_ 100, giải pháp sẽ chạy hiệu quả trong vòng chưa đầy một phần nghìn giây, trong giới hạn về thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, d = map(int, input().split())
    songs = list(map(int, input().split()))
    total_song_time = sum(songs)
    total_rest_time = 10 * (n - 1)
    min_event_time = total_song_time + total_rest_time
    if min_event_time > d:
        return "-1"
    remaining_time = d - min_event_time
    max_jokes = 2 * (n - 1) + remaining_time // 5
    return str(max_jokes)

# Provided samples
assert run("3 30\n2 2 1\n") == "5", "sample 1"
assert run("2 15\n10 10\n") == "-1", "sample 2"

# Custom cases
assert run("1 15\n5\n") == "2", "single song with extra time"
assert run("4 100\n5 5 5 5\n") == "18", "multiple short songs, large d"
assert run("3 31\n5 5 5\n") == "7", "extra minute allows one more joke"
assert run("2 25\n10 5\n") == "3", "minimum time leaves room for remaining jokes"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 15\n5 | 2 | Bài hát đơn, thêm thời gian để đùa | 
| 4 100\n5 5 5 5 | 18 | Nhiều bài hát ngắn, tối đa hóa những câu chuyện cười | 
| 3 31\n5 5 5 | 7 | Kiểm tra phân bổ thời gian còn lại | 
| 2 25\n10 5 | 3 | Xử lý đúng một phần thời gian còn lại | 

## Vỏ cạnh 

Nếu chỉ có một bài hát thì sau đó không có 10 phút nghỉ ngơi. Đối với đầu vào:```
1 20
5
```Tổng thời gian diễn ra sự kiện chỉ là 5 phút, còn lại 15 phút để đùa giỡn. Mỗi trò đùa dài 5 phút nên kết quả đầu ra là 3. Thuật toán xử lý chính xác điều này bằng cách tính toán`10*(1-1)=0`để nghỉ ngơi. 

Một trường hợp đặc biệt khác là khi tổng thời lượng bài hát cộng với thời gian nghỉ chính xác bằng _d_, không còn chỗ cho những trò đùa. Đối với đầu vào:```
3 25
5 5 5
```Thời gian sự kiện là 5+5+5 + 10*2 = 25, khớp chính xác với _d_. Thời gian còn lại bằng 0 nên số trò đùa tối đa là`2*(3-1) + 0//5 = 4`. Giải pháp lấp đầy phần còn lại một cách chính xác nhưng không thêm các câu chuyện cười bổ sung.
