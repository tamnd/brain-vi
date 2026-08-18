---
title: "CF 102262E - \u041a\u0440\u0438\u0442\u0438\u0447\u0435\u0441\u043a\u0430\u044f \u0443\u044f\u0437\u0432\u0438\u043c\u043e\u0441\u0442\u044c"
description: "Mỗi cụm là một công việc cập nhật không thể chia nhỏ. Cụm i chứa các máy chủ xi, do đó việc xử lý phải mất chính xác xi đơn vị thời gian. Khoảng thời gian cho phép của nó bắt đầu tại ai và kết thúc tại ai + xi."
date: "2026-08-17T20:19:58+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102262
codeforces_index: "E"
codeforces_contest_name: "\u0427\u0435\u043c\u043f\u0438\u043e\u043d\u0430\u0442 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e - \u0444\u0438\u043d\u0430\u043b (\u042f\u043d\u0434\u0435\u043a\u0441)"
rating: 0
weight: 102262
solve_time_s: 83
verified: true
draft: false
---

[CF 102262E - \u041a\u0440\u0438\u0442\u0438\u0447\u0435\u0441\u043a\u0430\u044f \u0443\u044f\u0437\u0432\u0438\u043c\u043e\u0441\u0442\u044c](https://codeforces.com/problemset/problem/102262/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 23s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Mỗi cụm là một công việc cập nhật không thể chia nhỏ. Cụm`i`chứa`x_i`máy chủ, do đó việc xử lý cần chính xác`x_i`đơn vị thời gian. Khoảng thời gian cho phép của nó bắt đầu lúc`a_i`và kết thúc tại`a_i + x_i`. Vì khoảng thời gian được phép có độ dài chính xác bằng thời gian xử lý được yêu cầu nên thực tế chỉ có một lịch trình khả thi cho cụm đó: nó chiếm toàn bộ khoảng thời gian`[a_i, a_i + x_i]`. 

Chúng tôi có thể chọn bất kỳ tập hợp con nào của cụm, nhưng khoảng thời gian của chúng không được trùng nhau vì mỗi lần chỉ có thể cập nhật một cụm. Nếu hai khoảng thời gian đã chọn gặp nhau ở điểm cuối thì điều đó hợp lệ: một bản cập nhật có thể kết thúc vào thời điểm đó`t`và việc tiếp theo có thể bắt đầu vào lúc nào đó`t`. 

Giá trị của việc chọn cụm`i`là`x_i`, bởi vì tất cả`x_i`các máy chủ trong cụm đó sẽ được cập nhật. Do đó, nhiệm vụ là chọn một tập hợp trọng số tối đa gồm các khoảng không chồng lấp và xuất ra cả tổng trọng số của nó và các chỉ số cụm dựa trên 0 tương ứng. 

Với`n`lên tới`10^5`, việc thử mọi tập hợp con sẽ cần tới`2^100000`khả năng, điều đó hoàn toàn không thể xảy ra. Ngay cả một thuật toán bậc hai cũng thực hiện xung quanh`10^10`hoạt động trong trường hợp xấu nhất, vượt xa những gì giới hạn một giây có thể xử lý. Chúng tôi cần một`O(n log n)`hoặc giải pháp hiệu quả tương tự. 

Có một số trường hợp ranh giới có thể khiến việc triển khai hợp lý trở nên sai lầm. Đầu tiên, khoảng thời gian chạm vào phải được coi là tương thích. Ví dụ,```
21 23 2
```đưa ra khoảng thời gian`[1,3]`Và`[3,5]`. Cả hai đều có thể được chọn, vì vậy câu trả lời là`4`với chỉ số`0 1`. Tìm kiếm trước đó bằng cách sử dụng`< a_i`thay vì`<= a_i`sẽ từ chối cụm thứ hai một cách không chính xác. 

Thứ hai, khoảng thời gian riêng lẻ dài nhất không nhất thiết là câu trả lời tốt nhất. Vì```
31 44 26 3
```các khoảng là`[1,5]`,`[4,6]`, Và`[6,9]`. Chọn cụm`1`Và`2`cho`2 + 3 = 5`, điều này tốt hơn việc chọn cụm`0`có giá trị`4`. Do đó, một chiến lược tham lam chỉ chiếm cụm dài nhất có sẵn có thể thất bại. 

Thứ ba, câu trả lời không thể chứa nhiều hơn một cụm ngay cả khi tồn tại nhiều cụm. Vì```
31 51 51 5
```cả ba khoảng đều giống nhau nên chỉ có thể xử lý một khoảng. Mức tối đa chính xác là`5`, không`15`. 

Cuối cùng, tất cả các giá trị thời gian và câu trả lời có thể đạt tới khoảng`10^14`khi tổng hợp lại`10^5`cụm. Các số nguyên Python tự động xử lý việc này, nhưng việc triển khai C++ sẽ cần`long long`. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp xem xét mọi tập hợp con của cụm. Đối với mỗi tập hợp con, chúng tôi có thể sắp xếp hoặc kiểm tra các khoảng thời gian đã chọn của nó, kiểm tra xem chúng có trùng nhau hay không và tính toán số lượng máy chủ được cập nhật. Điều này đúng vì mọi tập hợp cụm có thể đều được kiểm tra, do đó cuối cùng phải tìm ra tập hợp khả thi nhất. 

Vấn đề là số lượng tập hợp con. Với`n = 10^5`, có`2^100000`tập hợp con. Ngay cả trước khi kiểm tra xem những tập hợp con đó có khả thi hay không, thì về mặt thiên văn, nó quá lớn. 

Một công thức lập trình động lực mạnh mẽ hơn sẽ sắp xếp các khoảng thời gian theo thời gian kết thúc của chúng. Đối với mỗi khoảng thời gian, chúng ta có thể xem xét từng khoảng thời gian trước đó để tìm khoảng thời gian tương thích cuối cùng. Điều này mang lại sự lặp lại lập kế hoạch khoảng thời gian có trọng số quen thuộc, nhưng việc tìm kiếm khoảng thời gian trước đó bằng cách quét tất cả các khoảng thời gian trước đó sẽ mất nhiều thời gian.`O(n^2)`thời gian. Tại`n = 10^5`, đó là về`5 * 10^9`người tiền nhiệm kiểm tra trong trường hợp xấu nhất, vẫn còn quá chậm. 

Điều quan trọng là sau khi sắp xếp các khoảng theo điểm cuối bên phải của chúng, thông tin duy nhất chúng ta cần từ các khoảng trước đó là câu trả lời tốt nhất có khoảng cuối cùng kết thúc không muộn hơn thời điểm bắt đầu của khoảng hiện tại. Vì tất cả các điểm cuối bên phải trước đó đã được sắp xếp nên có thể tìm thấy điểm trước đó bằng tìm kiếm nhị phân. 

Hãy để một khoảng được biểu diễn dưới dạng`(start, end, weight, index)`, Ở đâu`start = a_i`

`end = a_i + x_i`

`weight = x_i`. 

Sau khi sắp xếp theo`end`, định nghĩa`dp[i]`là số lượng máy chủ tối đa có thể được cập nhật bằng cách sử dụng máy chủ đầu tiên`i`khoảng được sắp xếp. Trong khoảng thời gian tiếp theo, có đúng hai khả năng. Chúng ta hoặc bỏ qua nó, giữ`dp[i]`hoặc lấy nó và kết hợp trọng lượng của nó với giải pháp tốt nhất kết thúc tại hoặc trước khi nó bắt đầu. Nếu như`p`là số khoảng thời gian có điểm kết thúc nhiều nhất là điểm bắt đầu hiện tại, độ lặp lại là`dp[i + 1] = max(dp[i], dp[p] + x_i)`. 

Tìm kiếm nhị phân tìm thấy`p`TRONG`O(log n)`, giảm thuật toán hoàn chỉnh thành`O(n log n)`. 

Lập trình động tương tự cũng cho phép chúng ta xây dựng lại các cụm đã chọn. Bất cứ khi nào lấy khoảng hiện tại cho một giá trị hoàn toàn tốt hơn, chúng ta nhớ rằng khoảng này đã được chọn và quay trở lại`p`. Nếu không, chúng ta chuyển sang khoảng thời gian trước đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(2^n n)`|`O(n)`| Quá chậm | 
| DP bậc hai |`O(n^2)`|`O(n)`| Quá chậm | 
| Khoảng trọng số tối ưu DP |`O(n log n)`|`O(n)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc từng cụm và xây dựng khoảng thời gian cập nhật cố định của nó. Đối với cụm`i`, cửa hàng`(a_i, a_i + x_i, x_i, i)`. Chỉ mục gốc được giữ lại vì đầu ra phải sử dụng cách đánh số cụm từ đầu vào. 
2. Sắp xếp tất cả các khoảng theo thời gian kết thúc của chúng. Thứ tự này làm cho mọi phần trước của một khoảng xuất hiện trước nó và quan trọng hơn là làm cho tất cả thời gian kết thúc đều có sẵn cho tìm kiếm nhị phân. 
3. Tạo một mảng`ends`chứa thời gian kết thúc được sắp xếp. Đối với khoảng thời gian hiện tại bắt đầu`s`, sử dụng`bisect_right(ends, s)`để tìm xem có bao nhiêu khoảng thời gian kết thúc tại hoặc trước`s`. Gọi số này`p`. Việc sử dụng`bisect_right`là cố ý vì một khoảng kết thúc chính xác tại`s`không trùng lặp với khoảng thời gian hiện tại. 
4. Duy trì`dp`, Ở đâu`dp[k]`là số lượng máy chủ cập nhật tối đa có thể đạt được từ lần đầu tiên`k`khoảng được sắp xếp. Ban đầu`dp[0] = 0`, bởi vì chọn không có gì cập nhật nên không có máy chủ. 
5. Xử lý các khoảng từ trái sang phải. Đối với khoảng thời gian hiện tại, một tùy chọn là bỏ qua nó, đưa ra`dp[i]`. Lựa chọn khác là lấy nó, cho`dp[p] + x_i`. Lưu trữ giá trị lớn hơn trong`dp[i + 1]`. 
6. Bên cạnh`dp`, lưu trữ một quyết định cho từng vị trí. Nếu lấy khoảng thời gian hiện tại tốt hơn, hãy ghi lại rằng khoảng thời gian đã được chọn và ghi nhớ`p`. Nếu bỏ qua thì ít nhất cũng tốt, hãy ghi lại rằng khoảng thời gian hiện tại đã bị bỏ qua. Việc chọn bỏ qua khi cả hai giá trị bằng nhau sẽ thuận tiện vì nó mang lại sự tái cấu trúc xác định đơn giản, trong khi vẫn giữ được câu trả lời tối ưu. 
7. Bắt đầu từ trạng thái DP cuối cùng và xây dựng lại các cụm đã chọn về phía sau. Nếu khoảng thời gian hiện tại đã được chọn, hãy thêm chỉ mục ban đầu của nó và chuyển đến`p`. Nếu không thì chuyển sang trạng thái DP trước đó. Đảo ngược các chỉ số đã thu thập trước khi in chúng. 

Sau khi sắp xếp, bất biến là`dp[i]`luôn thể hiện câu trả lời tối ưu bằng cách sử dụng chính xác câu trả lời đầu tiên`i`khoảng thời gian theo thứ tự thời gian kết thúc. Bất kỳ giải pháp tối ưu nào đều loại trừ khoảng thời gian hiện tại, trong trường hợp đó nó được biểu thị bằng`dp[i]`, hoặc bao gồm nó, trong trường hợp đó, mọi khoảng thời gian đã chọn khác phải kết thúc không muộn hơn thời điểm bắt đầu của nó và tiền tố tốt nhất như vậy chính xác là`dp[p]`. Hai trường hợp này bao gồm mọi giải pháp khả thi, do đó phép truy toán không thể bỏ sót một phương án tối ưu. 

## Giải pháp Python```python
Pythonimport sysfrom bisect import bisect_right
input = sys.stdin.readline

def solve():    n = int(input())
    intervals = []    for idx in range(n):        a, x = map(int, input().split())        intervals.append((a, a + x, x, idx))
    intervals.sort(key=lambda item: item[1])
    ends = [item[1] for item in intervals]
    dp = [0] * (n + 1)    take = [False] * n    prev = [0] * n
    for i, (start, end, weight, idx) in enumerate(intervals):        p = bisect_right(ends, start, 0, i)
        skip_value = dp[i]        take_value = dp[p] + weight
        if take_value > skip_value:            dp[i + 1] = take_value            take[i] = True            prev[i] = p        else:            dp[i + 1] = skip_value
    answer = []    pos = n
    while pos > 0:        i = pos - 1        if take[i]:            answer.append(intervals[i][3])            pos = prev[i]        else:            pos -= 1
    answer.reverse()
    print(dp[n])    print(*answer)

if __name__ == "__main__":    solve()
```Vòng lặp đầu vào chuyển đổi mọi cụm thành khoảng thời gian thực sự phải được chiếm giữ. biểu thức`a + x`là an toàn vì số nguyên Python có độ chính xác tùy ý và giá trị tối đa có thể chỉ nằm trong khoảng`2 * 10^9`cho một điểm cuối. 

Sau khi sắp xếp,`ends[i]`là thời điểm kết thúc của`i`-khoảng thứ. Cuộc gọi đến`bisect_right(ends, start, 0, i)`chỉ tìm kiếm trong các khoảng thời gian đã được DP xử lý. Hạn chế này rất hữu ích vì khoảng thời gian sau`i`không thể là tiền thân của khoảng`i`. Nó cũng xử lý các điểm cuối bằng nhau một cách chính xác. 

Mảng DP có thêm một phần tử.`dp[0]`đại diện cho việc chọn từ một tiền tố trống và khoảng thời gian ở vị trí được sắp xếp`i`sản xuất nhà nước`dp[i + 1]`. Việc lập chỉ mục này làm cho người tiền nhiệm`p`có thể sử dụng trực tiếp như một vị trí DP. 

Các mảng tái thiết xứng đáng được chú ý đặc biệt.`prev[i]`có ý nghĩa khi khoảng`i`được chọn và cho chúng tôi biết chính xác trạng thái DP nào đã được sử dụng trước đó. Chúng tôi chuyển sang trạng thái đó thay vì chỉ giảm đi một. Khi khoảng thời gian bị bỏ qua, chúng tôi chuyển từ`pos`ĐẾN`pos - 1`. 

Sự so sánh chặt chẽ`take_value > skip_value`không bắt buộc đối với tính chính xác của giá trị, nhưng nó đưa ra lựa chọn mang tính quyết định khi cả hai phương án đều tốt như nhau. Tuyên bố cho phép bất kỳ tập hợp tối ưu nào, vì vậy lựa chọn nào cũng hợp lệ. 

Các chỉ số cuối cùng bị đảo ngược vì quá trình tái thiết tuân theo lịch trình ngược lại. Thứ tự của chúng trong đầu ra thực tế không bị hạn chế, nhưng việc đảo ngược chúng giúp kiểm tra kết quả dễ dàng hơn và giữ chúng theo thứ tự như các khoảng đã chọn. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Mẫu đầu tiên, được xây dựng lại từ định dạng câu lệnh, là```
41 44 118 512 5
```Các khoảng là`[1,5]`,`[4,15]`,`[8,13]`, Và`[12,17]`. Sau khi sắp xếp theo thời gian kết thúc, chúng đã xuất hiện theo thứ tự này. 

|`i`| Khoảng thời gian | Bắt đầu | Cân nặng |`p`| Bỏ qua | Đi |`dp[i+1]`| 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 0 |`[1,5]`| 1 | 4 | 0 | 0 | 4 | 4 | 
| 1 |`[4,15]`| 4 | 11 | 0 | 4 | 11 | 11 | 
| 2 |`[8,13]`| 8 | 4? | 1 | 11 | 9 | 11 | 
| 3 |`[12,17]`| 12 | 5 | 1 | 11 | 9 | 11 | 

Cụm thứ ba của mẫu được hiển thị có`x = 5`, vậy khoảng của nó là`[8,13]`. Do đó giá trị nhận thực tế của nó là`dp[1] + 5 = 9`, và kết luận của bảng không thay đổi. Câu trả lời tối ưu là cụm`1`, cho`11`máy chủ được cập nhật. 

### Mẫu 2 

Mẫu thứ hai là```
41 44 118 312 5
```Các khoảng là`[1,5]`,`[4,15]`,`[8,11]`, Và`[12,17]`. 

|`i`| Khoảng thời gian | Bắt đầu | Cân nặng |`p`| Bỏ qua | Đi |`dp[i+1]`| 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 0 |`[1,5]`| 1 | 4 | 0 | 0 | 4 | 4 | 
| 1 |`[8,11]`| 8 | 3 | 1 | 4 | 7 | 7 | 
| 2 |`[4,15]`| 4 | 11 | 0 | 7 | 11 | 11 | 
| 3 |`[12,17]`| 12 | 5 | 2 | 11 | 12 | 12 | 

Ở đây khoảng`[8,11]`có thể được theo sau bởi`[12,17]`bởi vì`11 <= 12`. Tổng kết quả là`4 + 3 + 5 = 12`, sử dụng cụm`0`,`2`, Và`3`. Ví dụ này chứng minh cụ thể tại sao đẳng thức trong điều kiện trước đó phải được chấp nhận. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(n log n)`| Sắp xếp mất`O(n log n)`, và mỗi`n`khoảng thực hiện một tìm kiếm nhị phân | 
| Không gian |`O(n)`| Các khoảng, giá trị DP, thông tin trước đó và mảng tái tạo đều yêu cầu không gian tuyến tính | 

Vì`n = 10^5`, sắp xếp và đại khái`10^5`tìm kiếm nhị phân nằm trong phạm vi dự định. Thuật toán tránh tìm kiếm bậc hai trước đó và chỉ sử dụng bộ nhớ bổ sung tuyến tính, do đó, nó phù hợp thoải mái trong giới hạn bộ nhớ đã nêu và phù hợp với giới hạn lập trình cạnh tranh một giây. 

## Trường hợp thử nghiệm 

Vì có thể tồn tại nhiều bộ tối ưu và câu lệnh cho phép các chỉ mục của chúng theo thứ tự tùy ý nên trình trợ giúp kiểm tra bên dưới xác thực giá trị được trả về và bộ đã chọn thay vì yêu cầu một thứ tự cụ thể.```python
Pythonimport sysimport iofrom bisect import bisect_right

def solve_io(data: str) -> str:    it = iter(data.split())    n = int(next(it))
    intervals = []    for idx in range(n):        a = int(next(it))        x = int(next(it))        intervals.append((a, a + x, x, idx))
    intervals.sort(key=lambda item: item[1])    ends = [item[1] for item in intervals]
    dp = [0] * (n + 1)    take = [False] * n    prev = [0] * n
    for i, (start, end, weight, idx) in enumerate(intervals):        p = bisect_right(ends, start, 0, i)
        if dp[p] + weight > dp[i]:            dp[i + 1] = dp[p] + weight            take[i] = True            prev[i] = p        else:            dp[i + 1] = dp[i]
    selected = []    pos = n
    while pos:        i = pos - 1        if take[i]:            selected.append(intervals[i][3])            pos = prev[i]        else:            pos -= 1
    selected.reverse()
    return str(dp[n]) + "\n" + " ".join(map(str, selected)) + "\n"

def run(inp: str) -> str:    return solve_io(inp)

def parse_output(out: str):    lines = out.strip().splitlines()    value = int(lines[0])    indices = list(map(int, lines[1].split())) if len(lines) > 1 and lines[1] else []    return value, indices

def check(inp: str, expected_value: int):    out = run(inp)    value, indices = parse_output(out)
    assert value == expected_value
    data = list(map(int, inp.split()))    n = data[0]    clusters = []    p = 1
    for i in range(n):        a = data[p]        x = data[p + 1]        p += 2        clusters.append((a, x))
    assert len(indices) == len(set(indices))
    intervals = []    total = 0
    for idx in indices:        a, x = clusters[idx]        intervals.append((a, a + x))        total += x
    intervals.sort()
    for i in range(1, len(intervals)):        assert intervals[i - 1][1] <= intervals[i][0]
    assert total == value

# Provided sample 1.assert parse_output(run(    """41 44 118 512 5"""))[0] == 11
# Provided sample 2.assert parse_output(run(    """41 44 118 312 5"""))[0] == 12
# Minimum-size input.check(    """17 3""",    3)
# All intervals are identical, so only one cluster can be chosen.check(    """51 21 21 21 21 2""",    2)
# Touching intervals must be accepted.check(    """31 23 25 2""",    6)
# A long interval is worse than several compatible shorter intervals.check(    """41 62 24 26 2""",    6)
# Large-value stress case.n = 100000large_input = str(n) + "\n" + "\n".join(    f"{2 * i + 1} 1" for i in range(n)) + "\n"check(large_input, n)
print("All tests passed.")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 7 3`|`3`| Đầu vào có kích thước tối thiểu và tái tạo lại một khoảng thời gian | 
| Năm bản sao của`1 2`|`2`| Chồng chéo các khoảng giống hệt nhau và tránh việc vô tình đếm hai lần | 
|`1 2`,`3 2`,`5 2`|`6`| Điều kiện biên nơi các khoảng liên tiếp chạm nhau | 
|`1 6`,`2 2`,`4 2`,`6 2`|`6`| DP chọn một số công việc tương thích thay vì công việc dài nhất | 
|`100000`khoảng thời gian`(2i+1, 1)`|`100000`| Kích thước đầu vào tối đa, sắp xếp, tìm kiếm nhị phân và trạng thái DP lớn | 

## Vỏ cạnh 

Đối với khoảng thời gian chạm, hãy xem xét```
31 23 25 2
```Các khoảng là`[1,3]`,`[3,5]`, Và`[5,7]`. Đối với khoảng thứ hai,`bisect_right`bao gồm khoảng đầu tiên vì kết thúc của nó chính xác`3`, Vì thế`p = 1`. Đối với quãng thứ ba, cả hai quãng trước đó đều có thể là các quãng trước, cho`p = 2`. DP đạt`6`, chọn cả ba cụm. Một điều kiện nghiêm ngặt trước đó sẽ làm giảm kết quả một cách không chính xác. 

Đối với các khoảng giống nhau, hãy xem xét```
31 51 51 5
```Tất cả các khoảng kết thúc tại`6`và sau khi khoảng đầu tiên được chọn, mọi khoảng khác có`p = 0`bởi vì không có kết thúc nào trước hoặc vào lúc bắt đầu`1`. Do đó DP giữ giá trị`5`thay vì thêm một khoảng chồng chéo khác. Đầu ra chứa chính xác một chỉ mục và báo cáo`5`. 

Trong một khoảng thời gian dài so với một số khoảng thời gian ngắn hơn, hãy xem xét```
41 62 24 26 2
```Cụm đầu tiên chiếm`[1,7]`và mang lại giá trị`6`. Ba người còn lại chiếm`[2,4]`,`[4,6]`, Và`[6,8]`, vì vậy tất cả chúng đều có thể được chọn và cũng cho giá trị tổng`6`. DP có thể chọn cách sắp xếp tối ưu tùy thuộc vào khả năng xử lý ràng buộc của mình. Vì nó ưu tiên bỏ qua khi các giá trị bằng nhau nên nó giữ giải pháp của cụm đầu tiên ở trạng thái liên quan nhưng giá trị được báo cáo vẫn chính xác. 

Đối với một cụm bắt đầu chính xác khi một cụm khác kết thúc, hãy xem xét```
210 414 100
```Các khoảng là`[10,14]`Và`[14,114]`. Cụm thứ hai là cụm kế thừa hợp lệ vì lần cập nhật đầu tiên kết thúc vào lúc`14`, chính xác khi phần thứ hai bắt đầu. Việc tìm kiếm nhị phân sử dụng`bisect_right`, do đó khoảng đầu tiên được đưa vào làm khoảng trước đó. Thuật toán trả về`104`, thể hiện quy ước điểm cuối mà không cần dựa vào thời gian dấu phẩy động. 

Đối với kích thước đầu vào tối đa, bài kiểm tra căng thẳng được tạo có chứa`100000`khoảng thời gian có độ dài đơn vị không chồng chéo. Mọi khoảng thời gian đều có thể được chọn, vì vậy câu trả lời là`100000`. Thuật toán thực hiện một sắp xếp và một tìm kiếm nhị phân trên mỗi khoảng, thay vì so sánh từng cặp, đây chính xác là điểm khác biệt giúp giải pháp có thể mở rộng.
