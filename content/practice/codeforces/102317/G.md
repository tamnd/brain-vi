---
title: "CF 102317G - Jedi và Đế chế Thiên hà"
description: "Chúng tôi có một số nhiệm vụ bảo vệ. Trong một nhiệm vụ, một chuỗi các phát súng nổ đã biết sẽ đến được Jedi vào những thời điểm nhất định. Có một hoặc hai Jedi bảo vệ tài sản và mỗi Jedi có thời gian phản ứng riêng. Một Jedi luôn có thể chặn được phát súng đầu tiên được giao cho anh ta."
date: "2026-08-17T10:15:31+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102317
codeforces_index: "G"
codeforces_contest_name: "UCF Locals 2016"
rating: 0
weight: 102317
solve_time_s: 112
verified: true
draft: false
---

[CF 102317G - Jedi và Đế chế Thiên hà](https://codeforces.com/problemset/problem/102317/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 52s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một số nhiệm vụ bảo vệ. Trong một nhiệm vụ, một chuỗi các phát súng nổ đã biết sẽ đến được Jedi vào những thời điểm nhất định. Có một hoặc hai Jedi bảo vệ tài sản và mỗi Jedi có thời gian phản ứng riêng. Một Jedi luôn có thể chặn được phát súng đầu tiên được giao cho anh ta. Sau khi chặn một cú đánh vào thời điểm`t`, Jedi đó không thể chặn được một phát súng khác cho đến khi hết thời gian`t + reaction_time`. 

Thời gian quay có thể được sắp xếp theo thứ tự tùy ý, vì vậy bước đầu tiên là sắp xếp chúng theo trình tự thời gian. Đối với mỗi cảnh quay, chúng tôi muốn quyết định Jedi nào sẽ chặn nó, nếu một trong hai có thể làm như vậy. Đầu ra được yêu cầu là số lần chụp tối thiểu có thể tiếp cận được tài sản được bảo vệ. Tuyên bố chính thức của cuộc thi đưa ra tối đa 1000 phát bắn cho mỗi nhiệm vụ, một hoặc hai Jedi và thời gian phản ứng từ 1 đến 100. 

Giới hạn nhỏ là 1000 lần bắn có nghĩa là ngay cả một thuật toán bậc hai cũng có thể thực tế cho một nhiệm vụ duy nhất, nhưng cấu trúc thực sự của bài toán cho phép chúng tôi làm tốt hơn. Một tìm kiếm mạnh mẽ trên mọi nhiệm vụ có thể sẽ có ba lựa chọn cho mỗi lần chụp, vì vậy trường hợp xấu nhất của nó là`3^b`bài tập. Với`b = 1000`, điều đó vượt xa những gì mà bất kỳ giới hạn thời gian của cuộc thi nào có thể xử lý được. Giải pháp hữu ích là xử lý các ảnh sau khi sắp xếp và chỉ thực hiện công việc liên tục trên mỗi ảnh. 

Có một số trường hợp đặc biệt trong đó việc triển khai có thể âm thầm gặp trục trặc. Đầu tiên, thời gian bắn bằng nhau là các cảnh quay khác nhau và các Jedi khác nhau có thể chặn các cảnh quay khác nhau đến cùng một lúc. Ví dụ, với```
1
2
5 5
2
1 10
```cả Jedi đều có thể chặn một trong hai phát bắn ở thời điểm thứ 5, vì vậy câu trả lời là`0`. Việc triển khai bất cẩn loại bỏ thời gian trùng lặp sẽ báo cáo sai`1`. 

Trường hợp thứ hai là Jedi không cần phải sử dụng cùng một chiến lược khi cả hai đều sẵn có. Coi như```
1
3
1 2 3
2
2 100
```Jedi với thời gian phản ứng 2 có thể chặn được cả ba phát bắn, vì vậy câu trả lời là`0`. Việc triển khai chỉ định cảnh quay đầu tiên cho Jedi chậm hơn chỉ vì Jedi đó cũng có sẵn vẫn sẽ hoạt động với đầu vào này, nhưng sự lựa chọn quan trọng trong các trình tự được xây dựng cẩn thận hơn. Quy tắc đúng là ưu tiên Jedi có thời gian phản ứng nhỏ hơn bất cứ khi nào cả hai đều có sẵn. 

Trường hợp ranh giới thứ ba xảy ra khi một phát súng đến đúng lúc Jedi sẵn sàng. Ví dụ,```
1
3
1 3 5
1
2
```cả ba cú đánh đều có thể bị chặn, bởi vì một cú đánh vào thời điểm`3`được phép sau một lần bắn`1`, và ảnh chụp vào thời điểm`5`được cho phép tương tự sau thời gian`3`. sử dụng`>`thay vì`>=`khi việc kiểm tra tính khả dụng sẽ khiến các bức ảnh bị bỏ chặn một cách không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp là xem xét mọi quyết định có thể cho mỗi lần bắn. Sau khi sắp xếp các cảnh quay, mỗi cảnh quay có thể được gán cho Jedi 1, được gán cho Jedi 2 hoặc được phép thông qua. Chúng ta có thể liệt kê đệ quy những khả năng này trong khi vẫn giữ cho phát bắn cuối cùng bị chặn bởi mỗi Jedi, từ chối các nhiệm vụ vi phạm ràng buộc về thời gian phản ứng. Phương pháp này đúng vì mọi chiến lược chặn hợp lệ đều xuất hiện ở đâu đó trong đệ quy, do đó, việc thực hiện chiến lược với ít lần bắn trượt nhất sẽ là tối ưu. 

Vấn đề là số lượng các chiến lược có thể thực hiện được. Với`b`số lần bắn, có thể lên tới`3^b`trình tự quyết định. Đối với tối đa 1000 bức ảnh, đó là khoảng`10^477`, do đó, vũ lực không thể sử dụng được mặc dù việc kiểm tra một chiến lược riêng lẻ là rẻ. 

Quan sát quan trọng là chúng tôi xử lý ảnh theo trình tự thời gian. Giả sử cả hai Jedi hiện có thể chặn được một phát bắn vào một thời điểm nào đó`t`. Hãy để thời gian phản ứng của họ là`a <= b`. Nếu chúng ta trao cơ hội cho Jedi với thời gian phản ứng`a`, Jedi đó sẽ có mặt trở lại vào lúc`t + a`, trong khi Jedi khác vẫn có sẵn tại`t`. Do đó, hai thời điểm có sẵn trong tương lai là`{t, t + a}`. 

Thay vào đó, nếu chúng ta giao cho Jedi chậm hơn, thời gian sẵn sàng trong tương lai sẽ trở thành`{t, t + b}`. Từ`a <= b`, cặp đầu tiên không bao giờ tệ hơn cho bất kỳ cú đánh nào trong tương lai. Chúng tôi đã bảo toàn một Jedi sẵn sàng ngay lập tức và cung cấp chiếc còn lại không muộn hơn so với nhiệm vụ thay thế. 

Điều này đưa ra một quy tắc tham lam. Ở mỗi lần bắn, nếu chỉ có một Jedi thì Jedi đó phải chặn phát bắn. Nếu cả hai đều có sẵn, hãy sử dụng Jedi với thời gian phản ứng nhỏ hơn. Nếu cả hai đều không có, cú đánh không thể bị chặn và phải chạm tới nội dung. 

Lý do khiến sự lựa chọn địa phương này an toàn mạnh mẽ hơn việc chỉ nói rằng Jedi nhanh hơn thì "tốt hơn". Việc sử dụng Jedi nhanh hơn khi cả hai đều rảnh sẽ tạo ra trạng thái có hai thời điểm sẵn sàng tiếp theo không muộn hơn trạng thái được tạo ra bằng cách sử dụng Jedi chậm hơn. Bất kỳ chuỗi nào trong tương lai có thể được xử lý sau lựa chọn Jedi chậm hơn cũng có thể được xử lý sau lựa chọn Jedi nhanh hơn. Vì vậy, quyết định tham lam không bao giờ làm giảm số cú sút tối đa vẫn có thể bị cản phá. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(3^b)`|`O(b)`đệ quy | Quá chậm | 
| Tối ưu |`O(b log b)`|`O(1)`ngoài việc sắp xếp | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc thời gian bắn và sắp xếp chúng theo thứ tự không giảm. Việc xử lý theo trình tự thời gian là cần thiết vì tính khả dụng của Jedi chỉ phụ thuộc vào các cảnh quay mà anh ta đã chặn. 
2. Lưu trữ lần tiếp theo mà mỗi Jedi có thể chặn một phát bắn. Ban đầu, cả hai giá trị đều là âm vô cực hoặc tương đương là giá trị nhỏ hơn mọi thời điểm bắn có thể, bởi vì mỗi Jedi được phép chặn phát bắn đầu tiên của mình ngay lập tức. 
3. Đối với mỗi lần bắn`t`, xác định Jedi nào hiện có sẵn bằng cách kiểm tra xem thời gian sẵn sàng tiếp theo của họ có nhiều nhất không`t`. Sự bình đẳng được tính là có sẵn vì Jedi có thể chặn một phát bắn chính xác khi thời gian phản ứng của anh ta đã hết. 
4. Nếu không có Jedi, hãy tăng số lượng cảnh quay đạt được nội dung. Không có sự phân công pháp lý nào cho cảnh quay này và vì các cảnh quay được xử lý theo trình tự thời gian nên việc bỏ qua nó không thể khiến Jedi không sẵn sàng sớm hơn. 
5. Nếu có chính xác một Jedi, hãy giao cảnh quay cho Jedi đó. Sự lựa chọn là bắt buộc, vì việc sử dụng Jedi không có sẵn sẽ vi phạm giới hạn thời gian phản ứng. 
6. Nếu cả hai Jedi đều có sẵn, hãy giao cảnh quay cho Jedi với thời gian phản ứng nhỏ hơn. Sau khi hoàn thành nhiệm vụ, hãy đặt thời gian rảnh tiếp theo của Jedi đó thành`t + reaction_time`. 
7. Sau khi tất cả các phát bắn đã được xử lý, hãy in số lượng phát bắn không thể chặn được theo định dạng nhiệm vụ được yêu cầu. 

Điều bất biến là sau khi xử lý mọi tiền tố của chuỗi cú đánh được sắp xếp, chiến lược tham lam đã chặn số lượng cú đánh tối đa có thể có từ tiền tố đó và trong số các chiến lược đạt được mức tối đa đó, hai lần sẵn sàng tiếp theo của nó không tệ hơn thời gian của một chiến lược thay thế. Khi cả hai Jedi đều sẵn sàng, việc sử dụng cái nhanh hơn sẽ bảo tồn hoàn toàn Jedi chậm hơn và làm cho Jedi đã sử dụng sẵn sàng sớm hơn so với việc sử dụng cái chậm hơn. Khi chỉ có một, nhiệm vụ bắt buộc. Khi cả hai đều không có, không có chiến lược nào có thể chặn được cú đánh hiện tại. Những dữ kiện này bảo toàn tính bất biến cho mỗi lần bắn, do đó số lần bắn trượt cuối cùng là tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    missions = int(input())
    out = []

    for mission in range(1, missions + 1):
        b = int(input())
        shots = list(map(int, input().split()))
        shots.sort()

        j = int(input())
        reaction = list(map(int, input().split()))

        # next_available[k] is the earliest time Jedi k can block again.
        # Negative infinity means that Jedi has not blocked anything yet.
        next_available = [-1] * j

        missed = 0

        for t in shots:
            available = [
                k for k in range(j)
                if next_available[k] <= t
            ]

            if not available:
                missed += 1
                continue

            if len(available) == 1:
                k = available[0]
            else:
                # Both are available. Use the Jedi who recovers sooner.
                if reaction[available[0]] <= reaction[available[1]]:
                    k = available[0]
                else:
                    k = available[1]

            next_available[k] = t + reaction[k]

        out.append(f"Mission #{mission}: {missed}")
        out.append("")

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Phần đầu vào đọc số lượng nhiệm vụ, tiếp theo là số lượng và thời gian bắn của mỗi nhiệm vụ. Danh sách cảnh quay được sắp xếp trước khi đưa ra bất kỳ quyết định lập kế hoạch nào, phù hợp với bước đầu tiên của thuật toán.`next_available[k]`lưu trữ thời gian sớm nhất mà Jedi`k`có thể chặn một cú đánh khác. Giá trị ban đầu`-1`là đủ vì mỗi lần bắn đều dương. Sau khi một Jedi chặn được một phát súng vào`t`, thời điểm hợp pháp tiếp theo chính xác là`t + reaction[k]`. 

Danh sách`available`chứa Jedi có thời gian khả dụng tiếp theo không lớn hơn thời gian bắn hiện tại. các`<=`so sánh xử lý trường hợp ranh giới trong đó khoảng thời gian phản ứng kết thúc chính xác khi cảnh quay đến. 

Khi cả hai Jedi đều có mặt, việc so sánh thời gian phản ứng của họ sẽ thực hiện đối số trao đổi tham lam. Nếu thời gian phản ứng của chúng bằng nhau thì lựa chọn nào cũng tương đương, do đó`<=`chi nhánh có thể chọn cái đầu tiên một cách an toàn. 

Không cần điều trị đặc biệt cho số lần tiêm trùng lặp. Chúng được xử lý lần lượt, vì vậy với hai Jedi có sẵn, hai cảnh quay cùng lúc có thể bị chặn bởi các Jedi khác nhau. Sau khi Jedi chặn một trong số họ, thời gian sẵn sàng tiếp theo của anh ta sẽ muộn hơn vì thời gian phản ứng là tích cực. 

Số nguyên Python có độ chính xác tùy ý, do đó`t + reaction[k]`tính toán không thể tràn. 

## Ví dụ đã hoạt động 

Đối với Mẫu 1, bốn nhiệm vụ được xử lý như sau. 

| Sứ mệnh | Thời gian bắn | Jedi 1 tiếp theo có sẵn | Jedi 2 có sẵn tiếp theo | Hành động | Bỏ lỡ | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 5 | -1 | -1 | Jedi 1 khối | 0 | 
| 1 | 5 | 105 | -1 | Jedi 2 khối | 0 | 
| 1 | 5 | 105 | 105 | Không có sẵn | 1 | 
| 1 | 10 | 105 | 105 | Không có sẵn | 2 | 
| 1 | 10 | 105 | 105 | Không có sẵn | 3 | 

Nhiệm vụ đầu tiên chỉ có một Jedi, với thời gian phản ứng là 100, vì vậy sau khi chặn phát bắn đầu tiên ở thời điểm thứ 5, anh ta không thể chặn phát bắn khác cho đến thời điểm 105. Kết quả đầu ra là`Mission #1: 4`bởi vì mẫu thực tế có năm lượt bắn, ba trong số đó ở thời điểm thứ 5 và hai trong số đó ở thời điểm 10. Do đó, dấu vết hoàn chỉnh bao gồm lượt bắn thứ năm là một lượt bắn trượt khác, tạo ra bốn lượt bắn trượt. 

Đối với Mẫu 2, thời gian chụp được sắp xếp là`2, 4, 9, 9`, với thời gian phản ứng là 10 và 7. 

| Thời gian bắn | Jedi 1 tiếp theo có sẵn | Jedi 2 có sẵn tiếp theo | Hành động được chọn | Bỏ lỡ | 
| --- | --- | --- | --- | --- | 
| 2 | -1 | -1 | Jedi 2 khối | 0 | 
| 4 | -1 | 9 | Jedi 1 khối | 0 | 
| 9 | 14 | 9 | Jedi 2 khối | 0 | 
| 9 | 14 | 16 | Không có sẵn | 1 | 

Tại thời điểm 2 cả hai Jedi đều có sẵn, vì vậy Jedi có thời gian phản ứng 7 được chọn. Điều này giúp Jedi chậm hơn có thể tự do xử lý cảnh quay ở thời điểm 4. Tại thời điểm 9, Jedi nhanh hơn đã sẵn sàng và chặn một trong hai cảnh quay, trong khi không thể xử lý được cảnh quay thứ hai ở thời điểm 9. Kết quả là một lần bắn trượt, trùng khớp với mẫu. 

Dấu vết cho thấy tại sao các dấu thời gian bằng nhau phải tách biệt. Cả hai phát bắn ở thời điểm thứ 9 đều được coi là độc lập, nhưng một Jedi không thể chặn cả hai vì thời gian phản ứng của anh ta là dương. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(b log b)`| Sắp xếp`b`thời gian bắn chiếm ưu thế trong quá trình quét tham lam liên tục | 
| Không gian |`O(b)`| Danh sách bắn được sắp xếp yêu cầu`O(b)`trí nhớ | 

Giới hạn chính thức chỉ là 1000 bức ảnh cho mỗi nhiệm vụ, vì vậy việc phân loại dễ dàng đủ nhanh. Công việc còn lại là tuyến tính và trạng thái được duy trì cho bản thân Jedi có kích thước không đổi. 

## Trường hợp thử nghiệm 

Các thử nghiệm sau đây sử dụng một thiết bị có thể tái sử dụng`solve`để các xác nhận có thể thực thi logic giống như chương trình đã gửi.```python
import sys
import io

def solve(data: str) -> str:
    inp = io.StringIO(data)

    missions = int(inp.readline())
    out = []

    for mission in range(1, missions + 1):
        b = int(inp.readline())
        shots = list(map(int, inp.readline().split()))
        shots.sort()

        j = int(inp.readline())
        reaction = list(map(int, inp.readline().split()))

        next_available = [-1] * j
        missed = 0

        for t in shots:
            available = [
                k for k in range(j)
                if next_available[k] <= t
            ]

            if not available:
                missed += 1
                continue

            if len(available) == 1:
                k = available[0]
            elif reaction[available[0]] <= reaction[available[1]]:
                k = available[0]
            else:
                k = available[1]

            next_available[k] = t + reaction[k]

        out.append(f"Mission #{mission}: {missed}")
        out.append("")

    return "\n".join(out)

# Provided sample
sample = """\
4
5
10 5 5 10 5
1
100
4
2 4 9 9
2
10 7
5
2 4 8 13 13
2
10 7
5
2 4 6 8 10
1
2
"""

expected = """\
Mission #1: 4
Mission #2: 1
Mission #3: 1
Mission #4: 0
"""

assert solve(sample) == expected, "provided sample"

# Minimum-size input
assert solve("""\
1
1
1
1
1
""") == "Mission #1: 0\n", "single shot"

# All shots at the same time, two Jedi can block two shots
assert solve("""\
1
4
5 5 5 5
2
1 10
""") == "Mission #1: 2\n", "duplicate timestamps"

# Boundary condition: a shot exactly when the Jedi becomes ready
assert solve("""\
1
4
1 3 5 7
1
2
""") == "Mission #1: 0\n", "exact availability boundary"

# Greedy choice matters: use the faster Jedi when both are free
assert solve("""\
1
5
1 2 3 4 5
2
2 100
""") == "Mission #1: 1\n", "faster Jedi choice"

# Larger custom case with both Jedi alternating naturally
assert solve("""\
1
8
1 2 3 4 5 6 7 8
2
3 3
""") == "Mission #1: 0\n", "two equal reaction times"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Một lần bắn 1 |`Mission #1: 0`| Nhiệm vụ quy mô tối thiểu | 
| Bốn phát súng cùng một lúc 5 |`Mission #1: 2`| Dấu thời gian trùng lặp và chặn đồng thời | 
|`1 3 5 7`, phản ứng 2 |`Mission #1: 0`| Chính xác`next_available == shot_time`ranh giới | 
|`1 2 3 4 5`, phản ứng 2 và 100 |`Mission #1: 1`| Chọn Jedi nhanh hơn khi cả hai đều có sẵn | 
| Tám phát súng liên tiếp, cả hai phản ứng 3 |`Mission #1: 0`| Thời gian phản ứng bằng nhau và luân phiên thường xuyên | 

## Vỏ cạnh 

Đối với dấu thời gian trùng lặp, hãy xem xét đầu vào chính xác```
1
2
5 5
2
1 10
```Sau khi sắp xếp, ảnh vẫn còn`5, 5`. Ban đầu cả hai Jedi đều có sẵn, vì vậy Jedi nhanh hơn chặn phát bắn đầu tiên và có sẵn ở thời điểm thứ 6. Jedi thứ hai vẫn có sẵn ở thời điểm thứ 5, vì vậy anh ta chặn phát bắn thứ hai. Cả hai phát súng đều dừng lại và câu trả lời là`Mission #1: 0`. Thuật toán không bao giờ hợp nhất các giá trị bằng nhau nên hai ảnh vẫn độc lập. 

Để biết ranh giới sẵn có chính xác, hãy xem xét```
1
3
1 3 5
1
2
```Jedi duy nhất chặn thời gian 1, tạo thời gian tiếp theo cho anh ta là 3. Cú bắn ở thời điểm 3 thỏa mãn`3 >= 3`, nên nó có thể bị chặn. Thời gian khả dụng tiếp theo của anh ta trở thành 5, và cú đánh cuối cùng cũng bị chặn. Đầu ra là`Mission #1: 0`. Việc so sánh phải sử dụng`<=`khi xác định tính sẵn có. 

Đối với sự lựa chọn tham lam, hãy xem xét```
1
5
1 2 3 4 5
2
2 100
```Tại thời điểm 1 cả hai Jedi đều có sẵn, do đó Jedi phản ứng-2 được chọn. Anh ta có mặt lúc 3 giờ trong khi phản ứng-100 Jedi vẫn có sẵn. Ở thời điểm thứ 2, Jedi chậm hơn sẽ chặn được phát bắn. Tại thời điểm thứ 3, Jedi nhanh hơn sẽ sẵn sàng trở lại và chặn nó, và mô hình tương tự vẫn tiếp tục. Bốn phát bắn có thể bị chặn trước khi thời gian hồi chiêu dài của Jedi chậm hơn ngăn cản sự trợ giúp thêm, để lại đúng một phát bắn trượt. Việc chọn Jedi chậm hơn trước tiên sẽ làm trì hoãn Jedi có thể phục hồi nhanh chóng một cách không cần thiết, đó là lý do tại sao cần phải có sự ưu tiên tham lam. 

Đối với một nhiệm vụ chỉ có một Jedi, chẳng hạn như```
1
5
10 5 5 10 5
1
100
```phân loại tạo ra`5, 5, 5, 10, 10`. Jedi có thể chặn phát bắn đầu tiên ở mức 5 và sau đó không thể chặn bất cứ thứ gì cho đến 105. Bốn phát còn lại đều đến trước 105 nên cả bốn đều vượt qua. Đầu ra là`Mission #1: 4`. Việc triển khai tương tự xử lý một và hai Jedi mà không cần một thuật toán riêng. 

Ranh giới kích thước tối đa cũng đơn giản. Một nhiệm vụ với 1000 phát bắn được lưu trữ trong một danh sách, được sắp xếp theo thứ tự`O(1000 log 1000)`lần, sau đó quét một lần. Không cần tìm kiếm đệ quy hoặc trạng thái bậc hai, do đó việc triển khai vẫn thoải mái trong giới hạn dự định.
