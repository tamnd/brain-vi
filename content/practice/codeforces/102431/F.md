---
title: "CF 102431F - Phà"
description: "Có ba hòn đảo A, B và C và chiếc phà buộc phải di chuyển theo chu kỳ theo thứ tự A, B, C, A, v.v. Mỗi du khách đều xuất phát tại A và có điểm đến cố định là B hoặc C. Một du khách cũng có giới hạn say sóng t."
date: "2026-08-09T12:27:53+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102431
codeforces_index: "F"
codeforces_contest_name: "2019 China Collegiate Programming Contest Final (CCPC-Final 2019)"
rating: 0
weight: 102431
solve_time_s: 418
verified: true
draft: false
---

[CF 102431F - Phà](https://codeforces.com/problemset/problem/102431/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 6 phút 58 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Có ba hòn đảo A, B và C và chiếc phà buộc phải di chuyển theo chu kỳ theo thứ tự A, B, C, A, v.v. Mỗi du khách đều xuất phát tại A và có điểm đến cố định là B hoặc C. Một du khách cũng có giới hạn say sóng`t`. Khi có nhiều người trên phà thì thời gian di chuyển của cạnh tiếp theo là lớn nhất`t`trong số đó. Phà có thể chở tối đa ba người. 

Các thủy thủ là nguồn lực bổ sung quan trọng. Một thủy thủ có`t = 1`, không có điểm đến và có thể ở lại trên phà suốt chặng đường. Vì phà không thể khởi hành mà không có ai trên tàu nên chúng ta có thể cử một thủy thủ vào mỗi chuyến đi. Thủy thủ có thể tiếp tục đi từ B đến C rồi từ C quay lại A ngay cả khi du khách đã rời thuyền. 

Đầu vào chứa tối đa`n = 50000`khách truy cập cho mỗi trường hợp thử nghiệm, với tối đa 10 trường hợp thử nghiệm. Mỗi du khách chỉ đóng góp một điểm đến và một giá trị từ 1 đến 1000. Hệ quả quan trọng của việc`n`là chúng ta không thể khám phá các tập hợp con, hoán vị hoặc các cặp tùy ý một cách rõ ràng. Thậm chí một`O(n^2)`phương pháp này vốn không được mong muốn trong Python đối với các trường hợp lớn nhất, vì vậy giải pháp cần khai thác cấu trúc rất nhỏ của tải trọng phà. 

Một chuyến phà có thể chở một thủy thủ và nhiều nhất là hai du khách. Giả sử hai du khách có giá trị`x <= y`. Nếu cả hai đều muốn B thì họ khởi hành ở B, vậy thời gian chuyến đi là`y`,`1`, Và`1`. Chi phí chu kỳ`y + 2`. Nếu có ít nhất một du khách muốn C, thì du khách đó vẫn ở trên tàu từ A đến B đến C. Cả hai chặng đầu tiên đều mất`y`, trong khi thủy thủ đi chặng cuối từ C đến A một mình. Chi phí chu kỳ`2y + 1`. 

Do đó, vấn đề ban đầu trở thành vấn đề ghép đôi. Mỗi cặp khách truy cập được gửi cùng nhau theo một chu kỳ từ A đến B đến C đến A và chi phí của nó là`y + 2`đối với cặp B-B, 

hoặc`2y + 1`với mỗi cặp chứa ít nhất một C, 

ở đâu`y`cái lớn hơn`t`trong cặp đó. 

Nếu như`n`thật kỳ lạ, một du khách phải đi du lịch một mình. Chúng ta có thể tránh trường hợp đặc biệt bằng cách thêm một khách truy cập B nhân tạo với`t = 1`. Việc ghép nối khách truy cập giả này với khách truy cập B thực sẽ tốn chi phí`t + 2`, chính xác là chi phí để gửi riêng khách truy cập B đó. Ghép nối nó với chi phí của khách truy cập C`2t + 1`, chính xác là chi phí gửi riêng khách truy cập C đó. Sau khi thêm hình nộm, số lượng người luôn là số chẵn. 

Một số trường hợp nhỏ bộc lộ sai sót trong mô hình. Vì```
1
1
1 5
```câu trả lời là`7`, không`15`. Du khách đến B vào thời gian 5, sau đó thủy thủ một mình đưa chặng B đến C và C đến A trong thời gian mỗi chặng 1. Một giải pháp giả định rằng du khách phải ở lại trên tàu cho đến khi A đánh giá quá cao câu trả lời. 

Vì```
1
1
2 5
```câu trả lời là`11`. Du khách ở lại trên tàu qua B, do đó cả A đến B và B đến C đều mất thời gian 5, tiếp theo là thời gian 1 của thủy thủ quay trở lại A. 

cho```
1
3
1 1
1 2
1 3
```câu trả lời là`8`. Sự sắp xếp tốt nhất sẽ kết nối du khách với`t = 2`Và`t = 3`, tính chi phí`5`, trong khi`t = 1`du khách đi du lịch một mình một cách hiệu quả, tốn kém`3`. Đơn giản chỉ cần giả định rằng khách truy cập lớn nhất phải là người độc thân sẽ đưa ra câu trả lời sai. 

Một trường hợp tinh tế hơn là mẫu đầu tiên. Việc ghép nối khách truy cập chỉ theo điểm đến của họ sẽ mang lại chi phí`16`, nhưng tối ưu là`14`. Các cặp tối ưu là B1-C1, B2-B2 và B3-C3, với chi phí`3`,`4`, Và`7`. Điều này cho thấy các nhóm điểm đến không thể được tối ưu hóa một cách độc lập. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực trực tiếp sẽ coi mọi sự phân chia có thể thành tải trọng của phà là một sự lựa chọn. Vì một xe đạp chở tối đa hai du khách sau khi đặt trước một chỗ cho thủy thủ nên về cơ bản đây là bài toán ghép đôi với chi phí tối thiểu. Thậm chí`n`, số cặp hoàn chỉnh là`(n - 1)!!`, cái nào cho`n = 50000`là sản phẩm`49999 * 49997 * ... * 1`. Đánh giá toàn diện những cặp đôi đó là vô vọng. 

Cách tiếp cận bạo lực là đúng vì mọi lịch trình phà khả thi đều có thể được chia thành các chu kỳ bắt đầu từ A và kết thúc tại A, và mỗi chu kỳ chứa tối đa hai du khách. Khi đã biết được số lượng khách truy cập được chỉ định cho một chu kỳ, chi phí của nó được xác định hoàn toàn bởi điểm đến và mức tối đa của họ.`t`. 

Quan sát giúp giải quyết vấn đề là chỉ những loại khách truy cập hiện chưa được so sánh mới quan trọng trong khi chúng tôi xử lý số lượng khách truy cập ngày càng tăng.`t`. Ở bất kỳ thời điểm nào, không bao giờ có lý do gì để giữ chân hai vị khách B không đối thủ. Nếu hai khách truy cập như vậy đã được nhìn thấy, việc ghép đôi họ bây giờ không đắt hơn việc trì hoãn họ, bởi vì mỗi khách truy cập trong tương lai đều có số lượng bằng hoặc lớn hơn.`t`. Lập luận tương tự áp dụng cho hai khách truy cập C chưa từng có. 

Do đó, trong khi quét khách truy cập theo thứ tự được sắp xếp, có thể có tối đa một khách truy cập B chưa từng có và nhiều nhất một khách truy cập C chưa từng có. Điều đó chỉ đưa ra bốn trạng thái có thể xảy ra: không có loại nào đang chờ, chỉ có B đang đợi, chỉ có C đang chờ hoặc một trong mỗi loại đang chờ. 

Khi khách truy cập hiện tại là B, nó có thể không được so sánh nếu không có B đang đợi hoặc ghép nối với khách đang chờ. Nếu khách đang đợi là B thì giá của cặp này là`t + 2`. Nếu khách đang chờ là C, cặp giá này`2t + 1`. Sự chuyển đổi của C hiện tại là tương tự nhau, ngoại trừ mọi cặp chứa C đều có chi phí`2t + 1`. 

Khách truy cập B giả xử lý một số lẻ khách truy cập thực, vì vậy trạng thái cuối cùng phải luôn không chứa khách truy cập nào chưa từng có. Điều này biến toàn bộ quá trình tối ưu hóa thành một chương trình động bốn trạng thái sau khi sắp xếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((n - 1)!!) | O(n) | Quá chậm | 
| DP bốn bang | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tách mỗi khách truy cập thành một cặp`(t, destination)`và sắp xếp tất cả khách truy cập theo`t`. Nếu như`n`thật kỳ lạ, thêm một khách truy cập giả`(1, B)`. Việc sắp xếp là cần thiết vì bất cứ khi nào khách truy cập hiện tại được ghép nối với một khách truy cập chưa từng có trước đó, thì`t`là mức tối đa của cặp. 
2. Duy trì bốn trạng thái DP. Tình trạng`0`có nghĩa là không có khách truy cập chưa từng có. Tình trạng`1`có nghĩa là một khách truy cập B là không thể so sánh được. Tình trạng`2`có nghĩa là một khách truy cập C là chưa từng có. Tình trạng`3`có nghĩa là một khách B và một khách C là không thể so sánh được. Mỗi tiểu bang lưu trữ chi phí tối thiểu của tất cả các cặp được xử lý trong khi không sử dụng chính xác những khách truy cập được chỉ định. 
3. Khi xử lý khách truy cập B có giá trị`t`, tình trạng`0`có thể khiến khách truy cập này chưa từng có, tạo ra trạng thái`1`không có chi phí ngay lập tức. Tình trạng`1`phải ghép đôi hai khách B, tính phí`t + 2`, và trở về trạng thái`0`. Từ tiểu bang`2`, chúng ta có thể để khách truy cập B chờ đợi, tạo ra trạng thái`3`, hoặc ghép B với C để có`2t + 1`và trở về trạng thái`0`. 
4. Khi trạng thái`3`chứa cả B và C và khách truy cập hiện tại là B, B hiện tại phải được ghép nối với một trong hai khách đang chờ. Ghép nối với chi phí B`t + 2`và để C chờ. Ghép nối với chi phí C`2t + 1`và để B chờ đợi. Hai lựa chọn thay thế này đều cần thiết vì sự lựa chọn có thể ảnh hưởng đến những khách truy cập sau này. 
5. Xử lý đối xứng khách truy cập C. Nếu nó kết hợp với B hoặc C thì cặp đó chứa C, do đó giá của nó luôn là`2t + 1`. Nếu không có C nào đang đợi thì C hiện tại có thể không được so sánh. 
6. Sau tất cả các du khách, hãy ghi trạng thái`0`. Vì đầu vào có kích thước lẻ đã nhận được một khách truy cập B giả nên mọi khách truy cập thực đều có thể được ghép nối và hình nộm đại diện cho một chu kỳ đơn lẻ có thể có. Bất kỳ trạng thái nào chứa khách truy cập chưa từng có đều không hợp lệ. 

### Tại sao nó hoạt động 

Điều bất biến là sau khi xử lý lượng truy cập đến trạng thái hiện tại`t`, giá trị DP của mỗi trạng thái là chi phí tối thiểu trong số tất cả các cặp khách truy cập được xử lý đó để lại chính xác các loại khách truy cập được mô tả bởi trạng thái đó không khớp. Hai khách truy cập cùng loại chưa từng có không bao giờ cần phải cùng tồn tại, bởi vì việc ghép nối họ ngay lập tức sử dụng mức tối đa hiện tại của họ.`t`, trong khi trì hoãn việc ghép đôi của họ chỉ có thể thay thế chi phí đó bằng một cặp có mức tối đa ít nhất bằng. Do đó, bốn trạng thái chứa tất cả thông tin có thể ảnh hưởng đến sự tiếp tục tối ưu. 

Mọi hành động có thể thực hiện được đối với khách truy cập hiện tại đều được thể hiện bằng một quá trình chuyển đổi. Việc để nó không khớp sẽ tạo ra một loại khách đang chờ, trong khi việc ghép nối nó với các loại khách đang chờ duy nhất có thể sẽ áp dụng chính xác chi phí chu trình phà tương ứng. Vì hình nộm làm cho tổng số người là số chẵn nên một lịch trình hoàn chỉnh tối ưu tương ứng với một đường đi kết thúc ở trạng thái`0`và mọi đường đi kết thúc ở trạng thái`0`mô tả một tập hợp hợp lệ các chu kỳ phà. Do đó, giá trị DP tối thiểu chính xác là tổng thời gian ngắn nhất có thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

INF = 10**30

def solve_case(visitors):
    if len(visitors) & 1:
        # A dummy B visitor with t = 1 represents a possible singleton.
        visitors.append((1, 1))

    visitors.sort(key=lambda x: x[0])

    # State:
    # 0 -> no unmatched visitor
    # 1 -> one unmatched B
    # 2 -> one unmatched C
    # 3 -> one unmatched B and one unmatched C
    dp = [0, INF, INF, INF]

    for t, w in visitors:
        ndp = [INF, INF, INF, INF]

        if w == 1:
            # Current visitor wants B.

            # State 0: leave current B unmatched.
            ndp[1] = min(ndp[1], dp[0])

            # State 1: pair current B with waiting B.
            ndp[0] = min(ndp[0], dp[1] + t + 2)

            # State 2: either leave current B, or pair B with C.
            ndp[3] = min(ndp[3], dp[2])
            ndp[0] = min(ndp[0], dp[2] + 2 * t + 1)

            # State 3: pair current B with either waiting B or waiting C.
            ndp[2] = min(ndp[2], dp[3] + t + 2)
            ndp[1] = min(ndp[1], dp[3] + 2 * t + 1)

        else:
            # Current visitor wants C.

            # State 0: leave current C unmatched.
            ndp[2] = min(ndp[2], dp[0])

            # State 1: either leave current C, or pair it with B.
            ndp[3] = min(ndp[3], dp[1])
            ndp[0] = min(ndp[0], dp[1] + 2 * t + 1)

            # State 2: pair current C with waiting C.
            ndp[0] = min(ndp[0], dp[2] + 2 * t + 1)

            # State 3: pair current C with either waiting B or waiting C.
            ndp[1] = min(ndp[1], dp[3] + 2 * t + 1)
            ndp[2] = min(ndp[2], dp[3] + 2 * t + 1)

        dp = ndp

    return dp[0]

def main():
    T = int(input())
    for case_id in range(1, T + 1):
        n = int(input())
        visitors = [tuple(map(int, input().split())) for _ in range(n)]
        # Store as (t, destination) for convenient processing.
        visitors = [(t, w) for w, t in visitors]

        answer = solve_case(visitors)
        print(f"Case #{case_id}: {answer}")

if __name__ == "__main__":
    main()
```Đầu vào đầu tiên được chuyển đổi từ`(destination, t)`vào trong`(t, destination)`, bởi vì DP xử lý mọi người trong việc tăng giới hạn say sóng. Việc sắp xếp sau đó đảm bảo rằng khách truy cập hiện tại luôn ở mức tối đa-`t`thành viên của bất kỳ cặp nào được hình thành với một vị khách chưa từng có trước đó. 

Điều kì lạ-`n`trường hợp được xử lý trước khi sắp xếp bằng cách thêm`(1, 1)`, đại diện cho một khách truy cập B giả. Hình nộm này không phải là người thật và chỉ là một thiết bị mô hình hóa. Nếu nó kết hợp với một khách truy cập B thực sự có giá trị`t`, giá của cặp`t + 2`, đó chính xác là chi phí mà du khách đó đi cùng một thủy thủ. Nếu nó kết hợp với khách truy cập C thì chi phí là`2t + 1`, một lần nữa phù hợp với chuyến đi đơn C. 

Bốn mục DP được đặt lại cho mỗi khách truy cập. Các công thức chuyển tiếp mã hóa trực tiếp tuyến phà. Giá một cặp B-B`t + 2`, trong khi mỗi cặp chứa C có giá`2t + 1`. Nhà nước`3`là trạng thái duy nhất mà khách truy cập hiện tại có hai đối tác chờ khác nhau, vì vậy cả hai quá trình chuyển đổi đều phải được giữ lại. 

Không có vấn đề tràn số nguyên trong Python. Câu trả lời tối đa chỉ theo thứ tự`n * max(t)`, Nhưng`INF`có chủ ý lớn hơn nhiều nên các trạng thái không thể truy cập không bao giờ can thiệp vào các giá trị hợp lệ. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Sau khi phân loại, du khách được```
B1, C1, B2, B2, B3, C3
```Bốn trạng thái DP được viết là`[none, B, C, BC]`. 

| Khách truy cập đã xử lý | không | B | C | BC | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | 0 | INF | INF | INF | 
| B1 | INF | 0 | INF | INF | 
| C1 | 3 | INF | INF | 0 | 
| B2 | INF | 3 | 4 | INF | 
| B2 | 7 | 9 | INF | 4 | 
| B3 | 14 | 7 | 9 | INF | 
| C3 | 14 | INF | 14 | 7 | 

Câu trả lời cuối cùng là trạng thái`none = 14`. Một cặp tối ưu là B1-C1, B2-B2 và B3-C3. Chi phí của họ là`3`,`4`, Và`7`, cho`14`. 

Dấu vết cũng chứng minh tại sao việc ghép đôi du khách chỉ theo điểm đến là không đủ. Giải pháp tối ưu có chủ ý sử dụng các cặp đích chéo để kết hợp đúng`t`các giá trị. 

### Mẫu 2 

Khách truy cập thực sự là B5 và vì có một khách truy cập nên thuật toán sẽ thêm B1 giả. 

| Khách truy cập đã xử lý | không | B | C | BC | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | 0 | INF | INF | INF | 
| Giả B1 | INF | 0 | INF | INF | 
| B5 thật | 7 | INF | INF | INF | 

Người giả và khách thật tạo thành cặp B-B tính giá`5 + 2 = 7`. Về mặt vật lý, điều này thể hiện du khách đi từ A đến B trong thời gian 5, tiếp theo là thủy thủ đi từ B đến C và C đến A trong thời gian 1. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Việc sắp xếp chiếm ưu thế trong DP bốn trạng thái, đó là O(n) | 
| Không gian | O(n) | Danh sách khách truy cập yêu cầu bộ nhớ O(n), trong khi bản thân DP sử dụng bốn giá trị | 

Vì`n <= 50000`, việc sắp xếp rất dễ thực hiện và lập trình động chỉ thực hiện một số thao tác không đổi cho mỗi khách truy cập. Điểm đến và`t`giới hạn không yêu cầu cấu trúc dữ liệu bổ sung, do đó việc sử dụng bộ nhớ vẫn tuyến tính theo số lượng khách truy cập. 

## Trường hợp thử nghiệm```python
import io
import sys

def solve_case(visitors):
    INF = 10**30

    if len(visitors) & 1:
        visitors.append((1, 1))

    visitors.sort()

    dp = [0, INF, INF, INF]

    for t, w in visitors:
        ndp = [INF, INF, INF, INF]

        if w == 1:
            ndp[1] = min(ndp[1], dp[0])

            ndp[0] = min(ndp[0], dp[1] + t + 2)

            ndp[3] = min(ndp[3], dp[2])
            ndp[0] = min(ndp[0], dp[2] + 2 * t + 1)

            ndp[2] = min(ndp[2], dp[3] + t + 2)
            ndp[1] = min(ndp[1], dp[3] + 2 * t + 1)

        else:
            ndp[2] = min(ndp[2], dp[0])

            ndp[3] = min(ndp[3], dp[1])
            ndp[0] = min(ndp[0], dp[1] + 2 * t + 1)

            ndp[0] = min(ndp[0], dp[2] + 2 * t + 1)

            ndp[1] = min(ndp[1], dp[3] + 2 * t + 1)
            ndp[2] = min(ndp[2], dp[3] + 2 * t + 1)

        dp = ndp

    return dp[0]

def run(inp: str) -> str:
    data = io.StringIO(inp)

    T = int(data.readline())
    out = []

    for case_id in range(1, T + 1):
        n = int(data.readline())
        visitors = []

        for _ in range(n):
            w, t = map(int, data.readline().split())
            visitors.append((t, w))

        out.append(f"Case #{case_id}: {solve_case(visitors)}")

    return "\n".join(out) + "\n"

sample_input = """\
2
6
1 1
1 2
1 3
1 2
2 3
2 1
1
1 5
"""

sample_output = """\
Case #1: 14
Case #2: 7
"""

assert run(sample_input) == sample_output, "provided samples"

assert run("""\
1
1
1 1
""") == "Case #1: 3\n", "minimum-size B case"

assert run("""\
1
1
2 1
""") == "Case #1: 3\n", "minimum-size C case"

assert run("""\
1
4
1 1
1 1
2 1
2 1
""") == "Case #1: 6\n", "all equal values"

assert run("""\
1
3
1 1
1 2
1 3
""") == "Case #1: 8\n", "odd number of B visitors"

assert run("""\
1
3
2 1
2 2
2 3
""") == "Case #1: 10\n", "odd number of C visitors"

assert run("""\
1
1
2 1000
""") == "Case #1: 2001\n", "maximum t boundary"

max_case = "1\n50000\n" + "\n".join(
    "1 1000" for _ in range(50000)
) + "\n"

assert run(max_case) == "Case #1: 25050000\n", "maximum-size input"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1 / 1 1`|`Case #1: 3`| Đầu vào kích thước tối thiểu và B singleton | 
|`1 / 1 / 2 1`|`Case #1: 3`| Đầu vào có kích thước tối thiểu và C singleton | 
| Bốn du khách với`t = 1`|`Case #1: 6`| Giá trị bình đẳng và điểm đến hỗn hợp | 
| B du khách với`t = 1,2,3`|`Case #1: 8`| Số lẻ và khách truy cập giả | 
| C du khách với`t = 1,2,3`|`Case #1: 10`| Số lượng C lẻ và chi phí cụ thể của C | 
| Một khách truy cập C với`t = 1000`|`Case #1: 2001`| Tối đa`t`ranh giới | 
| 50000 B khách truy cập với`t = 1000`|`Case #1: 25050000`| Kích thước đầu vào tối đa và câu trả lời lớn | 

## Vỏ cạnh 

Trường hợp khách B duy nhất được xử lý bởi hình nộm. Vì```
1
1
1 5
```thuật toán chèn B1, sắp xếp B1 và ​​B5 và ghép chúng thành`5 + 2 = 7`. Điều này tương ứng chính xác với tuyến đường vật lý A đến B trong thời gian 5, B đến C trong thời gian 1 và C đến A trong thời gian 1. 

Trường hợp khách truy cập C duy nhất hoạt động theo cách tương tự. Vì```
1
1
2 5
```giả B1 được ghép nối với C5. Vì cặp chứa C nên giá của nó là`2 * 5 + 1 = 11`. Du khách vẫn ở trên tàu qua B, đến C sau hai chặng thời gian-5, và sau đó thủy thủ trở về một mình. 

Một số lẻ khách truy cập có thể tạo ra một người không phải là khách truy cập lớn nhất. Vì```
1
3
1 1
1 2
1 3
```giả B1 được thêm vào. Sau khi sắp xếp, các cặp tối ưu là B1-dummy và B2-B3. Chi phí của họ là`3`Và`5`, cho`8`. Điều này nắm bắt các triển khai mù quáng để lại khách truy cập cuối cùng hoặc lớn nhất. 

Các điểm đến hỗn hợp có thể khiến việc ghép nối nhiều điểm đến trở nên cần thiết. Trong mẫu đầu tiên, việc ghép nối khách truy cập B với nhau và khách truy cập C với nhau sẽ tốn kém`16`. Thay vào đó, DP tìm B1-C1 cho`3`, B2-B2 cho`4`và B3-C3 cho`7`, tổng cộng`14`. Bốn trạng thái này đủ để ghi nhớ chính xác lựa chọn ghép nối chéo tạo ra sự cải thiện này. 

Cuối cùng, tối đa`t = 1000`là an toàn vì mọi chuyển đổi chỉ sử dụng phép cộng và nhân số nguyên với hai. Đối với 50000 khách truy cập, câu trả lời vẫn nằm trong phạm vi số nguyên thông thường và số học số nguyên của Python sẽ loại bỏ mọi lo ngại về tràn.
