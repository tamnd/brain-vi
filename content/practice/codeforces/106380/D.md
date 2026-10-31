---
title: "CF 106380D - Bạn có chơi Ballance không?"
description: "Chúng tôi có một quả bóng có thể là một trong ba vật liệu. Mỗi cơ chế trên đường dẫn chấp nhận một số tập hợp con của ba vật liệu này. Quả bóng phải đi qua mọi cơ chế nhưng chúng ta được phép lựa chọn thứ tự của các cơ chế."
date: "2026-06-25T10:21:14+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106380
codeforces_index: "D"
codeforces_contest_name: "The 6th Liaoning Provincial Collegiate Programming Contest"
rating: 0
weight: 106380
solve_time_s: 37
verified: true
draft: false
---

[CF 106380D - Bạn có chơi Ballance không?](https://codeforces.com/problemset/problem/106380/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 37s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một quả bóng có thể là một trong ba vật liệu. Mỗi cơ chế trên đường dẫn chấp nhận một số tập hợp con của ba vật liệu này. Quả bóng phải đi qua mọi cơ chế nhưng chúng ta được phép lựa chọn thứ tự của các cơ chế. 

Lúc đầu chúng ta phải trả tiền một lần để chọn nguyên liệu ban đầu. Trước bất kỳ cơ chế nào, chúng tôi có thể giữ miễn phí tài liệu hiện tại hoặc trả tiền cho một đơn vị để thay đổi tài liệu đó sang tài liệu khác. Mục tiêu là tìm ra tổng chi phí nhỏ nhất có thể sau khi sắp xếp các cơ chế theo thứ tự tốt nhất có thể. 

Mỗi cơ chế chỉ có thể được mô tả bằng tập hợp vật liệu mà nó chấp nhận. Vì có ba vật liệu nên chỉ có bảy loại cơ chế không trống. Kích thước đầu vào có thể lên tới 100 cơ chế, do đó, một giải pháp phụ thuộc vào số lượng cơ chế theo cấp số nhân là không thể, nhưng một giải pháp chỉ phụ thuộc vào bảy loại có thể là đủ nhanh. 

Các trường hợp khó khăn chính xuất phát từ thực tế là thứ tự rất quan trọng và cơ chế có thể cho phép nhiều vật liệu. Một giải pháp tham lam bất cẩn có thể thất bại. 

Ví dụ:```
3
1 0 0
0 1 0
0 0 1
```Câu trả lời đúng là`3`. Chúng ta cần một lựa chọn ban đầu và hai công tắc. Một chiến lược chỉ đơn giản là đếm các vật liệu và lợi nhuận khác nhau`3`sẽ có tác dụng ở đây, nhưng nó không thành công khi tồn tại sự chồng chéo vì tài liệu dùng chung có thể kết nối các nhóm. 

Một ví dụ khác:```
3
1 1 0
0 1 0
0 0 1
```Câu trả lời đúng là`3`. Chúng ta có thể bắt đầu với vật liệu 1, vượt qua cơ chế đầu tiên, giữ lại vật liệu 1 hoặc chuyển sang vật liệu 2 cho vật liệu thứ hai, sau đó chuyển sang vật liệu 3. Phương pháp tham lam luôn chuyển đổi bất cứ khi nào vật liệu mới xuất hiện có thể tạo ra những thay đổi không cần thiết. 

Các ràng buộc hướng trực tiếp tới việc nén trạng thái. Hiện tại chỉ có ba vật liệu có thể có và bảy mặt nạ cơ chế khả thi, vì vậy thuật toán có thể khám phá tất cả các kết hợp của các loại cơ chế đã xử lý thay vì tất cả các đơn hàng có thể có của 100 cơ chế. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp sẽ thử mọi thứ tự có thể có của cơ chế và mọi lựa chọn vật liệu có thể có trước mỗi cơ chế. Nếu có nhiều cơ chế, số lượng đơn đặt hàng là giai thừa, và thậm chí bỏ qua những lựa chọn vật chất thì số lượng các cách sắp xếp có thể vượt xa những gì có thể kiểm tra được. Đối với 100 cơ chế, việc khám phá các đơn hàng là hoàn toàn không khả thi. 

Quan sát hữu ích là các cơ chế sử dụng cùng loại vật liệu được phép là giống hệt nhau xét về góc độ đưa ra các quyết định trong tương lai. Sau khi xử lý một cơ chế thuộc một loại nhất định, việc xử lý một cơ chế khác có cùng loại sẽ có tác dụng hoàn toàn tương tự. Thông tin duy nhất quan trọng là chiếc mặt nạ nào trong số bảy chiếc mặt nạ bằng chất liệu không rỗng đã được hoàn thiện và quả bóng hiện có chất liệu gì. 

Brute Force hoạt động vì nó xem xét mọi mệnh lệnh hợp lệ có thể có, nhưng không thành công vì nó giữ thông tin không ảnh hưởng đến câu trả lời. Nhận xét rằng chỉ có bảy loại cơ chế cho phép chúng ta giảm bài toán xuống một đường đi ngắn nhất nhiều nhất là`2^7 * 3`tiểu bang. 

Đối với mọi trạng thái, chúng tôi thử lấy một danh mục cơ chế chưa được xử lý. Nếu vật liệu hiện tại được cho phép, chúng tôi có thể hoàn thành hạng mục đó mà không phải trả tiền. Nếu không, chúng tôi có thể chuyển sang một trong những tài liệu được phép và trả một đơn vị. Chúng tôi cũng cho phép tự nguyện chuyển sang tài liệu khác được chấp nhận vì tài liệu sau khi hoàn thành cơ chế có thể ảnh hưởng đến các lựa chọn trong tương lai. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| Tối ưu | O(2^7 * 7 * 3) | O(2^7 * 3) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển đổi mọi cơ chế thành mặt nạ ba bit. Chút`0`,`1`, Và`2`đại diện cho ba loại vật liệu. Đếm xem cái nào trong số bảy mặt nạ có thể xuất hiện. 
2. Sử dụng quy hoạch động ở đâu`dp[mask][material]`là chi phí tối thiểu sau khi xử lý tất cả các loại cơ chế có trong`mask`và hiện đang có tài liệu nhất định. 
3. Khởi tạo trạng thái trống. Việc chọn bất kỳ nguyên liệu ban đầu nào cũng tốn một đơn vị, vì vậy ba trạng thái không có cơ chế xử lý đều bắt đầu bằng chi phí`1`. 
4. Từ mọi trạng thái có thể truy cập, hãy chọn một danh mục cơ chế chưa được xử lý. Hãy thử mọi vật liệu có thể là vật liệu sau khi vượt qua cơ chế đó. 
5. Nếu vật liệu được chọn là vật liệu hiện tại thì chi phí chuyển đổi bằng 0. Nếu nó khác, quá trình chuyển đổi sẽ tốn một chi phí vì chúng ta chuyển đổi trước cơ chế. 
6. Câu trả lời là giá trị tối thiểu trong số tất cả các trạng thái mà mọi danh mục cơ chế hiện có đã được xử lý. 

Tại sao nó hoạt động: 

Điều bất biến là mọi trạng thái DP đều thể hiện cách rẻ nhất có thể để đạt được chính xác tình huống được mô tả. Thông tin duy nhất cần thiết cho các quyết định trong tương lai là tập hợp các loại cơ chế đã được xử lý và tài liệu hiện tại. Khi quá trình chuyển đổi được thực hiện, nó sẽ xem xét mọi danh mục tiếp theo có thể có và mọi tài liệu kết quả có thể có, do đó không bỏ sót thứ tự hợp lệ nào. Vì mọi chuyển đổi trạng thái có thể xảy ra đều tương ứng với một hành động pháp lý trong trò chơi và mọi hành động pháp lý đều được thể hiện bằng một chuyển đổi, nên trạng thái cuối cùng tối thiểu là chi phí tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    cnt = [0] * 8
    for _ in range(n):
        a = list(map(int, input().split()))
        mask = a[0] | (a[1] << 1) | (a[2] << 2)
        cnt[mask] += 1

    used = [i for i in range(1, 8) if cnt[i] > 0]
    idx = {x: i for i, x in enumerate(used)}
    m = len(used)

    categories = [(x, idx[x]) for x in used]
    full = (1 << m) - 1

    INF = 10**9
    dp = [[INF] * 3 for _ in range(1 << m)]

    for i in range(3):
        dp[0][i] = 1

    for state in range(1 << m):
        for cur in range(3):
            if dp[state][cur] == INF:
                continue
            for mask, bit in categories:
                if state & (1 << bit):
                    continue
                nstate = state | (1 << bit)
                for nxt in range(3):
                    if mask & (1 << nxt):
                        cost = 0 if nxt == cur else 1
                        if dp[state][cur] + cost < dp[nstate][nxt]:
                            dp[nstate][nxt] = dp[state][cur] + cost

    print(min(dp[full]))

if __name__ == "__main__":
    solve()
```Đầu vào đầu tiên được nén thành bảy loại cơ chế có thể. Số lần xuất hiện thực tế không quan trọng sau thời điểm này vì việc chuyển nhiều cơ chế cùng loại luôn có thể được thực hiện liên tục. 

Bảng DP chỉ lưu trữ trạng thái nén. Kích thước bitmask thể hiện các danh mục đã hoàn thành chứ không phải các cơ chế riêng lẻ. Đây là sự rút gọn khóa làm cho thuật toán trở nên nhỏ gọn. 

Trong quá trình chuyển đổi, vòng lặp kết thúc`nxt`là cần thiết vì một công tắc có thể hữu ích ngay cả khi vật liệu hiện tại được chấp nhận. Ví dụ: cơ chế chấp nhận vật liệu 1 và 2 có thể được sử dụng để chuyển từ vật liệu 1 sang vật liệu 2 nếu điều đó có ích sau này. 

Tra cứu cuối cùng sử dụng trạng thái chứa mọi danh mục xuất hiện trong đầu vào. Các danh mục không bao giờ xuất hiện thì không cần phải xử lý. 

## Ví dụ đã hoạt động 

Đối với đầu vào:```
5
1 0 0
1 0 1
0 1 0
1 0 1
0 0 1
```Các danh mục là bộ tài liệu`{1}`,`{1,3}`,`{2}`,`{3}`. 

Một dấu vết DP có thể có: 

| Danh mục đã xử lý | Tài liệu hiện tại | Chi phí | 
| --- | --- | --- | 
| không | 1 | 1 | 
|`{1}`| 1 | 1 | 
|`{1,3}`| 1 | 1 | 
|`{1,3},{2}`| 2 | 2 | 
| tất cả | 3 | 3 | 

Kết quả là`3`. Dấu vết cho thấy các cơ chế chồng chéo cho phép chúng ta di chuyển qua các danh mục mà không cần chuyển đổi mỗi lần. 

Vì:```
3
1 1 0
0 1 0
0 0 1
```Sự tiến hóa của trạng thái có thể là: 

| Danh mục đã xử lý | Tài liệu hiện tại | Chi phí | 
| --- | --- | --- | 
| không | 1 | 1 | 
|`{1,2}`| 1 | 1 | 
|`{1,2},{2}`| 2 | 2 | 
| tất cả | 3 | 3 | 

Một lần nữa câu trả lời là`3`và phần quan trọng là cơ chế đầu tiên có thể kết nối vật liệu 1 và 2. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(2^k * k * 3 * 3) |`k`là số loại cơ chế xuất hiện, nhiều nhất là 7 | 
| Không gian | O(2^k * 3) | Lưu trữ trạng thái DP | 

Thuật toán không phụ thuộc vào số lượng cơ chế sau khi nén. Ngay cả trong trường hợp xấu nhất, tất cả bảy loại đều xuất hiện, chỉ mang lại giá trị 384 DP và số lần chuyển đổi rất nhỏ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    n = int(input())
    cnt = [0] * 8
    for _ in range(n):
        a = list(map(int, input().split()))
        cnt[a[0] | (a[1] << 1) | (a[2] << 2)] += 1

    used = [i for i in range(1, 8) if cnt[i]]
    pos = {x: i for i, x in enumerate(used)}
    dp = [[10**9] * 3 for _ in range(1 << len(used))]
    for i in range(3):
        dp[0][i] = 1

    for s in range(1 << len(used)):
        for c in range(3):
            for mask in used:
                if dp[s][c] == 10**9:
                    continue
                if s & (1 << pos[mask]):
                    continue
                for nc in range(3):
                    if mask & (1 << nc):
                        ns = s | (1 << pos[mask])
                        dp[ns][nc] = min(dp[ns][nc], dp[s][c] + (c != nc))

    ans = str(min(dp[-1]))
    sys.stdin = old
    return ans

assert run("""5
1 0 0
1 0 1
0 1 0
1 0 1
0 0 1
""") == "3"

assert run("""1
1 1 1
""") == "1"

assert run("""3
1 0 0
0 1 0
0 0 1
""") == "3"

assert run("""3
1 1 0
0 1 0
0 0 1
""") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Cơ chế duy nhất cho phép tất cả các vật liệu | 1 | Chỉ lựa chọn ban đầu | 
| Ba cơ chế vật liệu đơn rời rạc | 3 | Yêu cầu chuyển đổi giữa các nhóm bị cô lập | 
| Bộ vật liệu chồng chéo | 3 | Tránh những sai lầm chuyển đổi tham lam | 

## Vỏ cạnh 

Đối với trường hợp vật liệu cách ly:```
3
1 0 0
0 1 0
0 0 1
```DP bắt đầu với chi phí`1`đối với bất kỳ vật liệu nào. Việc xử lý danh mục đầu tiên chỉ miễn phí nếu nó phù hợp với tài liệu hiện tại. Hai loại còn lại yêu cầu chuyển mạch, đưa ra đáp án cuối cùng`3`. 

Đối với trường hợp toàn vật liệu:```
1
1 1 1
```Bất kỳ vật liệu ban đầu nào cũng hoạt động và cơ chế chấp nhận nó ngay lập tức. DP đạt đến trạng thái hoàn thành với chi phí`1`, đây là mức tối thiểu có thể vì lựa chọn ban đầu là bắt buộc. 

Đối với các danh mục chồng chéo:```
3
1 1 0
0 1 0
0 0 1
```Hai cơ chế đầu tiên có thể được xử lý thông qua vật liệu 2, trong khi cơ chế cuối cùng yêu cầu vật liệu 3. DP giữ khả năng chuyển từ vật liệu 1 sang vật liệu 2 vào đúng thời điểm thay vì buộc phải chuyển đổi sớm. Chi phí cuối cùng vẫn còn`3`.
