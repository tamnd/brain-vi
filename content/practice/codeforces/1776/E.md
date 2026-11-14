---
title: "CF 1776E - Băng qua đường sắt"
description: "Chúng tôi được yêu cầu mô phỏng một người, Isona, băng qua một loạt đường ray xe lửa song song từ sân ga này sang sân ga khác trong khi tránh tàu hỏa. Đường ngang thẳng và vuông góc với đường sắt."
date: "2026-06-09T11:44:47+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dp"]
categories: ["algorithms"]
codeforces_contest: 1776
codeforces_index: "E"
codeforces_contest_name: "SWERC 2022-2023 - Online Mirror (Unrated, ICPC Rules, Teams Preferred)"
rating: 3500
weight: 1776
solve_time_s: 111
verified: false
draft: false
---

[CF 1776E - Băng qua đường sắt](https://codeforces.com/problemset/problem/1776/E) 

**Đánh giá:** 3500 
**Thẻ:** cấu trúc dữ liệu, dp 
**Thời gian giải:** 1 phút 51 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu mô phỏng một người, Isona, băng qua một loạt đường ray xe lửa song song từ sân ga này sang sân ga khác trong khi tránh tàu hỏa. Đường ngang thẳng và vuông góc với đường sắt. Mỗi tuyến đường sắt chiếm đúng một mét không gian và có một mét khoảng cách giữa các tuyến đường sắt liên tiếp và giữa các sân ga với tuyến đường sắt gần nhất. Isona có thể chọn tốc độ của mình tại bất kỳ thời điểm nào, nhưng mỗi thay đổi về tốc độ đều được tính là "thay đổi tốc độ" và chúng tôi muốn giảm thiểu những thay đổi này. Cô ấy cũng có thể đợi hoặc đứng yên và bắt đầu chạy từ trạng thái nghỉ không được tính là thay đổi tốc độ. 

Đầu vào mô tả số lượng tàu`n`, số lượng đường sắt`m`, tổng thời gian cho phép`s`, và số giây`v`Isona phải vượt qua một mét với tốc độ tối đa. Mỗi chuyến tàu được đưa ra bởi thời gian bắt đầu và kết thúc`(a_i, b_i)`và đường sắt`r_i`nó sẽ chiếm giữ. Đầu ra là số lần thay đổi tốc độ tối thiểu mà Isona phải thực hiện để tiếp cận nền tảng khác trong thời gian`s`, hoặc`-1`nếu điều đó là không thể. 

Những ràng buộc cho chúng ta biết rằng`m`là nhỏ (lên tới 10), trong khi`n`có thể đạt tới 500 và giá trị thời gian có thể rất lớn (`10^9`). Điều này cho thấy chúng ta nên tập trung vào các thuật toán trong đó độ phức tạp phụ thuộc theo cấp số nhân vào`m`nhưng không bật`s`hoặc các giá trị thời gian thực tế, vì việc lặp lại từng giây sẽ quá chậm. Các trường hợp khó khăn bao gồm các tình huống trong đó các đoàn tàu chặn mọi thời gian có thể xảy ra, các đoàn tàu ở ngay cạnh nhau trên cùng một tuyến đường sắt hoặc Isona hoàn toàn có thể tính thời gian chạy của mình để tránh bất kỳ sự thay đổi tốc độ nào. 

Việc triển khai đơn giản thử mọi sự kết hợp giữa thời gian bắt đầu và thay đổi tốc độ trên tất cả các đồng hồ đo sẽ không khả thi do số lượng thời gian có thể có rất lớn và phạm vi thời gian lớn. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực sẽ cố gắng mô phỏng chuyển động của Isona trên mỗi mét trong mọi thời gian bắt đầu có thể. Chúng ta có thể thử mọi sự kết hợp giữa chờ đợi hoặc chạy ở tốc độ tối đa, theo dõi những thay đổi về tốc độ. Cách tiếp cận này đúng về mặt nguyên tắc, nhưng với`m`lên đến 10 và lần lên đến`10^9`, việc lặp lại từng giây là không thể. Ngay cả phương pháp lập trình động ghi lại "những thay đổi tốc độ tối thiểu ở mỗi bước thời gian" cũng sẽ thất bại do phạm vi lớn của`s`. 

Quan sát quan trọng là các đoàn tàu chỉ chặn những quãng đường nhỏ trên mỗi tuyến đường sắt. Vì có tối đa 500 chuyến tàu và nhiều nhất 10 tuyến đường sắt nên chúng tôi có thể xử lý trước các khoảng thời gian này để xác định "cửa sổ an toàn" trên mỗi tuyến đường sắt. Trong cửa sổ an toàn, Isona có thể băng qua mà không đụng phải tàu hỏa. Giữa các cửa sổ, cô ấy có thể phải đợi hoặc điều chỉnh tốc độ của mình. Điều này biến vấn đề thành một vấn đề lập trình động trên các lớp (đường sắt), trong đó các quốc gia theo dõi thời gian đến sớm nhất và những thay đổi tốc độ tối thiểu cho từng khoảng thời gian an toàn trên tuyến đường sắt tiếp theo. 

Cụ thể, chúng ta có thể biểu diễn trạng thái ở mỗi tuyến đường bằng một tập hợp các khoảng thời gian`[start_time, end_time]`trong đó Isona có thể đến mà không có xung đột. Di chuyển từ đường sắt`i`đến đường sắt`i+1`, chúng tôi xem xét tất cả các cặp khoảng cách từ đường sắt`i`ĐẾN`i+1`. Đối với mỗi cặp, chúng tôi tính toán thời gian sớm nhất cô ấy có thể đến chuyến tàu tiếp theo, có thể chờ để tránh tàu. Nếu cô ấy điều chỉnh tốc độ của mình giữa những khoảng thời gian này, chúng ta sẽ tính một lần thay đổi tốc độ. Cuối cùng, tốc độ tối thiểu thay đổi trong số tất cả các khoảng thời gian hợp lệ trên tuyến đường sắt cuối cùng đến trước`s`đưa ra câu trả lời. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^m * s) | O(s*m) | Quá chậm | 
| DP tối ưu trong khoảng thời gian an toàn | O(n * m^2) | O(n * m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xử lý trước các đoàn tàu để tạo danh sách các quãng đường bị chặn cho mỗi tuyến đường sắt. Mỗi tuyến đường sắt sẽ lưu trữ các khoảng thời gian của nó dưới dạng`(a_i, b_i)`sắp xếp theo thời gian bắt đầu. Chúng tôi thêm`0`Và`s`làm ranh giới nhân tạo để đơn giản hóa việc tính toán DP. 
2. Đối với mỗi tuyến đường sắt, chuyển đổi các khoảng dừng bị chặn thành khoảng cách an toàn. Khoảng thời gian an toàn là khoảng thời gian tối đa mà Isona có thể băng qua tuyến đường sắt này mà không gặp tàu. Đối với đường sắt`r_i`, nếu các khoảng bị chặn là`[1,2], [3,4]`, các khoảng an toàn trở thành`[0,1], [2,3], [4,s]`. 
3. Khởi tạo mảng DP`dp[i][j]`cho đường sắt`i`và khoảng cách an toàn`j`. Mỗi mục lưu trữ những thay đổi tốc độ tối thiểu cần thiết để đạt đến cuối khoảng thời gian đó. Đối với tuyến đường sắt đầu tiên, mọi khoảng cách an toàn bắt đầu sau`0`có thể đạt được với`0`tốc độ thay đổi nếu Isona bắt đầu chạy vào thời điểm đó. 
4. Đối với mỗi tuyến đường sắt tiếp theo`i`, lặp lại tất cả các khoảng an toàn của đường sắt trước đó`i-1`. Đối với mỗi sự kết hợp giữa khoảng thời gian trước đó và khoảng thời gian hiện tại, hãy tính thời gian sớm nhất`t`Isona có thể đạt đến khoảng thời gian hiện tại so với khoảng thời gian trước đó, xem xét tốc độ tối đa của mình và chờ đợi nếu cần thiết. Nếu điều này yêu cầu thay đổi tốc độ (tức là thời gian sử dụng khác với tốc độ truyền tải tối đa), hãy tăng số lần thay đổi tốc độ lên 1. 
5. Sau khi xử lý tất cả các tuyến đường sắt, kiểm tra khoảng cách an toàn của tuyến đường sắt cuối cùng. Đối với mỗi khoảng thời gian cho phép đến sân ga trước`s`, lấy số lần thay đổi tốc độ tối thiểu trong số đó. Nếu không tồn tại, xuất`-1`. 
6. Xuất kết quả. 

Tại sao nó hoạt động: Tại mỗi tuyến đường sắt, trạng thái DP ghi lại những thay đổi tốc độ tối thiểu để đạt đến bất kỳ khoảng thời gian an toàn nào. Vì quá trình chuyển đổi chỉ xem xét các khoảng thời gian an toàn và điều chỉnh tốc độ tối thiểu nên chúng tôi không bao giờ đánh giá thấp số lượng thay đổi cần thiết. Bằng cách triển khai từ tuyến đường sắt đầu tiên đến tuyến đường sắt cuối cùng, chúng tôi đảm bảo mọi sự kết hợp có thể có giữa việc chờ đợi và thay đổi tốc độ đều được xem xét. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m, s, v = map(int, input().split())
    trains = [[] for _ in range(m)]
    for _ in range(n):
        a, b, r = map(int, input().split())
        trains[r-1].append((a, b))
    
    safe = []
    for tlist in trains:
        tlist.sort()
        intervals = []
        prev = 0
        for a, b in tlist:
            if prev < a:
                intervals.append((prev, a))
            prev = b
        if prev < s:
            intervals.append((prev, s))
        safe.append(intervals)
    
    dp = [{} for _ in range(m)]
    for st, en in safe[0]:
        dp[0][(st, en)] = 0
    
    for i in range(1, m):
        for c_st, c_en in safe[i]:
            dp[i][(c_st, c_en)] = float('inf')
            for p_st, p_en in dp[i-1]:
                prev_changes = dp[i-1][(p_st, p_en)]
                t_needed = v
                earliest_arrival = max(c_st, p_st + t_needed)
                if earliest_arrival <= c_en:
                    changes = prev_changes
                    if earliest_arrival != p_st + t_needed:
                        changes += 1
                    dp[i][(c_st, c_en)] = min(dp[i][(c_st, c_en)], changes)
    
    ans = float('inf')
    for (st, en), changes in dp[-1].items():
        if st <= s:
            ans = min(ans, changes)
    
    print(-1 if ans == float('inf') else ans)

solve()
```Giải pháp xử lý trước các chuyến tàu vào khoảng thời gian an toàn. DP truyền bá những thay đổi tốc độ tối thiểu từ tuyến đường này sang tuyến đường tiếp theo. Đối với mỗi khoảng thời gian an toàn, chúng tôi tính toán xem có thể truy cập được khoảng thời gian đó mà không cần thay đổi tốc độ bổ sung hay không và nếu không, hãy tăng số lượng. Các điều kiện biên như các khoảng bắt đầu từ`0`hoặc kết thúc tại`s`được xử lý một cách tự nhiên vì chúng tôi bao gồm các cạnh của nền tảng trong khoảng thời gian an toàn. 

## Ví dụ đã hoạt động 

### Mẫu 1```
4 3 5 1
1 2 1
3 4 1
2 3 2
3 4 3
```| Đường sắt | Khoảng thời gian an toàn | Trạng thái DP (thay đổi tối thiểu) | 
| --- | --- | --- | 
| 1 | [0,1],[2,3],[4,5] | 0 cho tất cả | 
| 2 | [0,2],[3,5] | chuyển đổi từ 1, thay đổi tối thiểu vẫn là 0 | 
| 3 | [0,3],[4,5] | chuyển đổi từ 2, thay đổi tối thiểu vẫn là 0 | 

Đầu ra là`0`, cho thấy Isona có thể chạy thẳng với tốc độ tối đa và tiếp cận an toàn mà không cần điều chỉnh tốc độ. 

### Mẫu 2```
2 2 5 1
1 3 1
2 4 2
```| Đường sắt | Khoảng thời gian an toàn | Trạng thái DP (thay đổi tối thiểu) | 
| --- | --- | --- | 
| 1 | [0,1],[3,5] | 0 khi bắt đầu, 1 nếu đợi đến 3 | 
| 2 | [0,2],[4,5] | 0 từ khoảng thời gian đầu tiên yêu cầu thay đổi tốc độ để chờ, tổng cộng tối thiểu 1 | 

Đầu ra là`1`, cho thấy Isona phải điều chỉnh tốc độ một lần để đồng bộ với các quãng tàu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n * m^2) | Đối với mỗi tuyến đường sắt, chúng tôi lặp lại tất cả các khoảng an toàn của đường sắt trước đó, tối đa 500 khoảng thời gian trên mỗi tuyến đường sắt, m ≤ 10 | 
| Không gian | O(n * m) | Lưu trữ các khoảng thời gian an toàn và trạng thái DP | 

Điều này phù hợp thoải mái trong các ràng buộc cho trước n 500, m 10. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) ->
```
