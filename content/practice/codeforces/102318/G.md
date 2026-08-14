---
title: "CF 102318G - Xác suất trò chơi điện tử"
description: "Trò chơi có chứa một số loại vật phẩm. Đối với mỗi loại, chúng tôi biết cần có bao nhiêu bản sao và xác suất lấy được loại đó trong một lần thử. Chúng tôi cũng biết tổng số lần thử có sẵn."
date: "2026-08-14T00:02:52+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102318
codeforces_index: "G"
codeforces_contest_name: "UCF Locals 2017"
rating: 0
weight: 102318
solve_time_s: 579
verified: true
draft: false
---

[CF 102318G - Xác suất trò chơi điện tử](https://codeforces.com/problemset/problem/102318/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 9 phút 39 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Trò chơi có chứa một số loại vật phẩm. Đối với mỗi loại, chúng tôi biết cần có bao nhiêu bản sao và xác suất lấy được loại đó trong một lần thử. Chúng tôi cũng biết tổng số lần thử có sẵn. Mọi nỗ lực đều được chỉ định cho bất kỳ loại mục nào hiện đang cần thiết và một lần thử không thành công sẽ khiến chúng tôi phải làm việc trên cùng một mục. Nhiệm vụ là tính xác suất để tất cả các vật phẩm cần thiết được thu thập trước khi hết giới hạn số lần thử. 

Một cách hữu ích để suy nghĩ về quy trình là mở rộng từng loại mục thành các bản sao được yêu cầu riêng lẻ. Nếu loại đầu tiên cần ba lần với xác suất (p=0,4), chúng ta có thể coi đó là ba mục tiêu liên tiếp có xác suất thành công là (0,4). Sau khi mục tiêu đầu tiên thành công, mục tiêu thứ hai sẽ hoạt động, v.v. Tuyên bố ban đầu cung cấp tối đa 50 loại mục, mỗi loại yêu cầu tối đa 50 bản sao, do đó có tối đa 2500 thành công được yêu cầu. Số lần thử có thể lên tới 10000. Các giới hạn này loại trừ bất kỳ số mũ nào về số lần thử hoặc các mục bắt buộc, trong khi chương trình động (O(2500\cdot10000)) có khoảng 25 triệu chuyển đổi trạng thái trong trường hợp thử nghiệm đơn lẻ lớn nhất. Đánh giá vấn đề chính thức mô tả công thức lập trình động tương tự này. 

Đầu vào bắt đầu bằng số lượng trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm đưa ra số loại mục, theo sau là số lượng yêu cầu và xác suất thành công cho mỗi loại và cuối cùng là tổng giới hạn lần thử. Đầu ra của mỗi trường hợp kiểm thử là xác suất hoàn thành mọi mục được yêu cầu, được in đến ba chữ số thập phân. Tài liệu UCF ban đầu chỉ định (1\le g\le50), (0\le c\le50), (0\le p\le1) và (0\le a\le10000). 

Một số trường hợp ranh giới rất dễ bị xử lý sai. Nếu không có mục nào được yêu cầu thì câu trả lời luôn là 1. Ví dụ: dữ liệu đầu vào```
1
1
0 0.5
10
```có đầu ra```
1.000
```bởi vì người chơi đã có mọi thứ. Một DP giả sử có ít nhất một mục bắt buộc có thể trả về 0 không chính xác. 

Xác suất thành công bằng không cũng có vấn đề. Vì```
1
1
1 0.0
10
```đầu ra là```
0.000
```bởi vì vật phẩm yêu cầu không bao giờ có thể có được. Một công thức chia cho xác suất thành công hoặc giả định mọi mục tiêu cuối cùng đều thành công, có thể thất bại trong trường hợp này. 

Giới hạn số lần thử có thể bằng số lần thành công chính xác cần thiết. Vì```
1
2
1 0.5
1 0.5
2
```đầu ra là```
0.250
```bởi vì cả hai lần thử đều phải thành công, cho kết quả (0,5\cdot0,5). Một lỗi thường gặp là coi việc đạt được mục tiêu cuối cùng sau lần thử cuối cùng là không thể, mặc dù việc hoàn thành mục tiêu đó trong lần thử đó là hợp lệ. 

Cuối cùng, xác suất thành công bằng 1 không nên được coi là chuyển đổi dấu phẩy động thông thường với nhánh thất bại có ý nghĩa. Vì```
1
2
1 1.0
1 1.0
2
```đầu ra là```
1.000
```vì cả hai mục yêu cầu đều được đảm bảo. DP xử lý việc này một cách tự nhiên, nhưng việc đặt trạng thái cuối cùng trong vỏ đặc biệt không chính xác có thể làm mất xác suất đó. 

## Phương pháp tiếp cận 

Cách tiếp cận đệ quy trực tiếp tuân theo quy trình trò chơi thực tế. Xác định (f(i,j)) là xác suất mà sau (i) lần thử chính xác (j) các mục yêu cầu đã được thu thập. Ở mọi nỗ lực đều có hai khả năng. Có thể đạt được mục tiêu hiện tại, tiến từ (j-1) lên (j) hoặc nỗ lực có thể thất bại, khiến số lượng vật phẩm thu thập được không thay đổi. Sự lặp lại này là đúng vì hai sự kiện đó loại trừ lẫn nhau và bao gồm mọi kết quả có thể xảy ra của lần thử tiếp theo. 

Vấn đề với việc triển khai đệ quy đó là công việc lặp đi lặp lại. Trạng thái tương tự có thể đạt được thông qua nhiều chuỗi thành công và thất bại khác nhau. Nếu không có sự ghi nhớ, đệ quy sẽ phân nhánh ở mỗi lần thử, tạo ra nhiều đường dẫn theo cấp số nhân. Với 10000 lần thử, thậm chí (2^{10000}) chuỗi kết quả có thể xảy ra về mặt khái niệm có liên quan đến nhau, điều này hoàn toàn không khả thi. 

Việc ghi nhớ khắc phục vấn đề bài toán con lặp lại, nhưng số trạng thái riêng biệt vẫn là (O(aT)), trong đó (T) là tổng số mục bắt buộc. Phiên bản lập trình động tính toán mọi trạng thái một lần và do đó là (O(aT)). 

Quan sát quan trọng là thứ tự thử các loại mục không ảnh hưởng đến phân bố xác suất của kết quả cuối cùng. Mỗi lần thử có xác suất thành công độc lập cố định cho vật phẩm hiện đang được thu thập. Do đó, chúng ta có thể giả vờ rằng trình phát luôn làm việc trên các bản sao được yêu cầu theo một thứ tự cố định. Trạng thái quan trọng không phải là toàn bộ lịch sử mà chỉ là số lượng bản sao cần thiết đã được lấy. Khi biết con số đó, chúng tôi biết chính xác xác suất áp dụng cho lần thử thành công tiếp theo. 

Lực lượng vũ phu hoạt động vì mọi chuỗi thành công và thất bại hoàn chỉnh đều tương ứng với một lịch sử trò chơi có thể xảy ra, nhưng thất bại vì nó liệt kê các lịch sử đó một cách riêng lẻ. Quan sát cho thấy các lịch sử có cùng số lượng mục tiêu đã hoàn thành có hành vi giống hệt nhau trong tương lai cho phép chúng tôi hợp nhất chúng thành một trạng thái DP. Đánh giá chính thức trình bày chính xác sự tái diễn này bằng cách sử dụng danh sách làm phẳng các nhiệm vụ bắt buộc. 

Đặt (p_j) là xác suất thành công của bản sao bắt buộc thứ (j), sử dụng chỉ mục dựa trên số 0. Đặt (dp[j]) là xác suất mà chính xác (j) bản sao đã được hoàn thành sau những lần thử được xử lý cho đến nay. Với (0<j<T), 

[ 
mới[j]=dp[j](1-p_j)+dp[j-1]p_{j-1}. 
] 

Thuật ngữ đầu tiên có nghĩa là chúng tôi đã ở (j) và thất bại trong lần thử tiếp theo. Thứ hai có nghĩa là chúng ta đã ở (j-1) và đã thành công với mục tiêu dẫn đến (j). Đối với (j=T), không có mục tiêu tiếp theo, vì vậy tất cả xác suất đã ở (T) vẫn ở đó, trong khi (dp[T-1]p_{T-1}) có thể chuyển sang trạng thái hoàn thành. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(2^a)) | (O(a)) độ sâu đệ quy | Quá chậm | 
| Đệ quy được ghi nhớ | (O(aT)) | (O(aT)) | Đúng nhưng lớn không cần thiết | 
| DP tối ưu | (O(aT)) | (O(T)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Mở rộng số lượng mục thành một mảng xác suất duy nhất. Nếu một loại mục yêu cầu (c) bản sao và có xác suất (p), hãy thêm (p) chính xác (c) lần. Mảng kết quả có độ dài (T), tổng số bản sao được yêu cầu. Điều này chuyển đổi đầu vào được nhóm ban đầu thành một chuỗi các mục tiêu trong đó xác suất chỉ phụ thuộc vào số lượng mục tiêu đã được hoàn thành. 
2. Tạo mảng DP một chiều với các mục (T+1). Ban đầu`dp[0] = 1`, bởi vì trước khi thực hiện bất kỳ nỗ lực nào, không có bản sao cần thiết nào đã được thu thập một cách chắc chắn. Mọi trạng thái khác đều có xác suất bằng không. 
3. Xử lý từng lần thử một. Sau (i) lần thử, chỉ các trạng thái từ 0 đến (\min(i,T)) mới có thể có xác suất khác 0, vì một lần thử có thể tăng số lượng mục tiêu đã hoàn thành lên tối đa một. Việc hạn chế vòng lặp trong phạm vi này sẽ tránh được những công việc không cần thiết trong những lần lặp đầu tiên. 
4. Cập nhật các trạng thái theo thứ tự giảm dần. Đối với trạng thái chưa hoàn thành (j), xác suất mới của nó là xác suất cũ để duy trì ở (j) sau một thất bại cộng với xác suất cũ ở (j-1) sau đó là thành công: 

[ 
dp[j]\leftarrow dp[j](1-p_j)+dp[j-1]p_{j-1}. 
] 

Thứ tự giảm dần là cần thiết vì`dp[j-1]`vẫn phải đại diện cho lần thử trước đó. Nếu mảng được cập nhật từ trái sang phải,`dp[j-1]`sẽ chứa thông tin từ lần thử hiện tại và sẽ được tính không chính xác. 

1. Cập nhật riêng trạng thái 0. Không có trạng thái trước đó có thể nhập số 0, vì vậy nó đơn giản trở thành 

[ 
dp[0]\leftarrow dp[0](1-p_0). 
] 

1. Cập nhật trạng thái hoàn thành (T) khi nó tồn tại. Khi tất cả các vật phẩm cần thiết đã được thu thập, những lần thử sau không thể khiến kết quả trở nên tồi tệ hơn. Do đó, xác suất hoàn thành được chuyển tiếp không thay đổi, trong khi thành công từ trạng thái (T-1) được thêm vào đó. 
2. Sau tất cả (a) lần thử, xuất ra`dp[T]`. Đây chính xác là xác suất mà mọi bản sao cần thiết đã được thu thập trong số lần thử cho phép. 

### Tại sao nó hoạt động 

Điều bất biến là sau khi xử lý chính xác (i) lần thử,`dp[j]`bằng với xác suất mà chính xác (j) bản sao cần thiết đã được thu thập và mục tiêu tiếp theo là bản sao thứ (j). Mọi cách để đạt đến trạng thái (j) sau lần thử tiếp theo có đúng một trong hai dạng: chúng ta đã ở (j) và thất bại, hoặc chúng ta đã ở (j-1) và đã thành công. Quá trình chuyển đổi bao gồm cả hai khả năng với xác suất chính xác của chúng và không có khả năng nào khác. Trạng thái (T) đang hấp thụ vì việc hoàn thành tất cả các yêu cầu là vĩnh viễn. Vì bất biến giữ nguyên ban đầu và mọi chuyển đổi đều bảo toàn nó,`dp[T]`sau tất cả các lần thử là xác suất mong muốn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve_case(g, items, attempts):
    probs = []

    for count, p in items:
        probs.extend([p] * count)

    total = len(probs)

    if total == 0:
        return 1.0

    # dp[j] = probability of having completed exactly j targets.
    dp = [0.0] * (total + 1)
    dp[0] = 1.0

    for used in range(1, attempts + 1):
        hi = min(used, total)

        # State total is special: once completed, it stays completed.
        if hi == total:
            dp[total] += dp[total - 1] * probs[total - 1]
            start = total - 1
        else:
            start = hi

        # Descending order keeps dp[j - 1] from the previous attempt.
        for j in range(start, 0, -1):
            dp[j] = dp[j] * (1.0 - probs[j]) + dp[j - 1] * probs[j - 1]

        dp[0] *= 1.0 - probs[0]

    return dp[total]

def solve(data):
    it = iter(data.split())
    cases = int(next(it))
    out = []

    for _ in range(cases):
        g = int(next(it))
        items = []

        for _ in range(g):
            count = int(next(it))
            p = float(next(it))
            items.append((count, p))

        attempts = int(next(it))

        answer = solve_case(g, items, attempts)
        out.append(f"{answer:.3f}")

    return "\n".join(out)

def main():
    data = sys.stdin.buffer.read()
    sys.stdout.write(solve(data.decode()))

if __name__ == "__main__":
    main()
```Phần đầu tiên của`solve_case`xây dựng mảng xác suất phẳng. Loại yêu cầu năm bản sẽ đóng góp năm mục liên tiếp với xác suất như nhau. Việc nhận dạng chính xác các bản sao là không liên quan, bởi vì tất cả các bản sao của một loại đều có xác suất thành công độc lập giống hệt nhau. 

Mảng DP có thêm một vị trí cho`total`, đại diện cho trạng thái nơi mọi yêu cầu đã được đáp ứng. Trạng thái ban đầu chứa xác suất bằng 1 tại các mục tiêu đã hoàn thành bằng 0. 

Vòng lặp chính thể hiện các lần thử thay vì các mục bắt buộc. Điều này phù hợp trực tiếp với sự tái phát. Nhiều nhất`used`mục tiêu có thể được hoàn thành sau`used`số lần thử, do đó giới hạn trên`hi`ngăn cản việc lặp lại sớm việc quét các trạng thái không thể truy cập được. 

Trạng thái hoàn thành được xử lý trước khi cập nhật giảm dần thông thường. Khi`hi == total`, một thành công từ`total - 1`đi vào trạng thái hoàn thành. Xác suất hiện có ở`dp[total]`phải ở đó vì những nỗ lực bổ sung không hoàn tác được các yêu cầu đã hoàn thành. 

Các trạng thái còn lại được cập nhật từ phải sang trái. Thứ tự này là chi tiết triển khai trung tâm. Ví dụ: khi tính giá trị mới của`dp[3]`,`dp[2]`vẫn phải mô tả sự phân bố trước lần thử hiện tại. Cập nhật xuống đảm bảo tài sản đó. 

Trình phân tích cú pháp đầu vào sử dụng`sys.stdin.buffer.read()`thay vì liên tục gọi`input()`. Sự cố có thể yêu cầu hàng triệu lần chuyển đổi DP, vì vậy việc giảm chi phí đầu vào phía Python là một biện pháp phòng ngừa hợp lý. Người được yêu cầu`input = sys.stdin.readline`giao diện vẫn tương thích với cấu trúc giải pháp, mặc dù trình phân tích cú pháp thực tế sử dụng đầu vào hàng loạt được lưu vào bộ đệm vì nó nhanh hơn. 

Số nguyên Python không tràn và giá trị DP là xác suất dấu phẩy động. Lỗi dấu phẩy động là vô hại ở ba chữ số thập phân bắt buộc đối với phép tính dự kiến. Định dạng cuối cùng làm tròn kết quả đến chính xác ba chữ số sau dấu thập phân. 

## Ví dụ đã hoạt động 

Báo cáo vấn đề được cung cấp không chứa đầu vào mẫu hoặc đầu ra mẫu, vì vậy các dấu vết sau đây sử dụng hai trường hợp được xây dựng nhỏ. 

Hãy xem xét trường hợp đầu tiên:```
1
1
1 0.5
2
```Có một mục bắt buộc, xác suất thành công của nó là (0,5) và có hai lần thử. 

| Cố gắng |`dp[0]`|`dp[1]`| 
| --- | --- | --- | 
| Ban đầu | 1.000 | 0,000 | 
| 1 | 0,500 | 0,500 | 
| 2 | 0,250 | 0,750 | 

Sau lần thử đầu tiên, có 50% khả năng vật phẩm vẫn bị thiếu và 50% khả năng vật phẩm đó đã được thu thập. Trong lần thử thứ hai, trường hợp bị thiếu thành công với xác suất (0,5), đóng góp một trường hợp khác (0,25) vào trạng thái hoàn thành. Câu trả lời cuối cùng là`0.750`. 

Dấu vết này thể hiện bản chất hấp thụ của trạng thái hoàn thành. Một lần`dp[1]`nhận được xác suất, những lần thử sau không thể loại bỏ nó. 

Bây giờ hãy xem xét hai loại mặt hàng khác nhau:```
1
2
1 0.5
1 1.0
2
```Mục bắt buộc đầu tiên sẽ thành công với xác suất (0,5), trong khi mục thứ hai được đảm bảo sau khi có được mục đầu tiên. 

| Cố gắng |`dp[0]`|`dp[1]`|`dp[2]`| 
| --- | --- | --- | --- | 
| Ban đầu | 1.000 | 0,000 | 0,000 | 
| 1 | 0,500 | 0,500 | 0,000 | 
| 2 | 0,250 | 0,250 | 0,500 | 

Sau lần thử đầu tiên, vật phẩm đầu tiên đã nhận được hoặc không. Ở lần thử thứ hai, chỉ trạng thái có một mục đã hoàn thành mới có thể đạt đến trạng thái cuối cùng và mục tiêu tiếp theo của nó có xác suất là 1. Câu trả lời là`0.500`. 

Ví dụ này cho thấy tại sao phân phối nhị thức đơn lẻ là không đủ. Xác suất thành công thay đổi sau khi hoàn thành mục tiêu đầu tiên, vì vậy DP phải nhớ số lượng mục tiêu đã được thu thập. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(aT)) | Tối đa (T) trạng thái DP được xử lý cho mỗi (a) lần thử | 
| Không gian | (O(T)) | Thuật toán lưu trữ một xác suất cho mỗi số mục tiêu có thể hoàn thành | 

Ở đây (T\le50\cdot50=2500) và (a\le10000), do đó trường hợp thử nghiệm đơn lớn nhất thực hiện tối đa khoảng 25 triệu cập nhật trạng thái. Việc sử dụng bộ nhớ chỉ là vài nghìn giá trị dấu phẩy động. Giới hạn cuộc thi chính thức là 3 giây và 256 MB. 

## Trường hợp thử nghiệm```python
import io
import sys

def solve_case(g, items, attempts):
    probs = []

    for count, p in items:
        probs.extend([p] * count)

    total = len(probs)

    if total == 0:
        return 1.0

    dp = [0.0] * (total + 1)
    dp[0] = 1.0

    for used in range(1, attempts + 1):
        hi = min(used, total)

        if hi == total:
            dp[total] += dp[total - 1] * probs[total - 1]
            start = total - 1
        else:
            start = hi

        for j in range(start, 0, -1):
            dp[j] = (
                dp[j] * (1.0 - probs[j])
                + dp[j - 1] * probs[j - 1]
            )

        dp[0] *= 1.0 - probs[0]

    return dp[total]

def solution(inp: str) -> str:
    it = iter(inp.split())
    cases = int(next(it))
    ans = []

    for _ in range(cases):
        g = int(next(it))
        items = []

        for _ in range(g):
            c = int(next(it))
            p = float(next(it))
            items.append((c, p))

        a = int(next(it))
        ans.append(f"{solve_case(g, items, a):.3f}")

    return "\n".join(ans)

def run(inp: str) -> str:
    return solution(inp)

# The supplied statement contains no sample cases, so these are
# constructed verification cases.

assert run(
    """1
1
1 0.5
1
"""
) == "0.500", "one attempt"

assert run(
    """1
1
1 0.5
2
"""
) == "0.750", "two attempts"

assert run(
    """1
2
1 0.5
1 0.5
2
"""
) == "0.250", "both successes required"

assert run(
    """1
1
0 0.7
0
"""
) == "1.000", "zero required items"

assert run(
    """1
1
1 0.0
10000
"""
) == "0.000", "impossible item"

assert run(
    """1
2
1 1.0
1 1.0
2
"""
) == "1.000", "guaranteed items"

assert run(
    """1
50
50 0.5
50 0.5
50 0.5
50 0.5
50 0.5
50 0.5
50 0.5
50 0.5
50 0.5
50 0.5
50 0.5
50 0.5
50 0.5
50 0.5
50 0.5
50 0.5
50 0.5
50 0.5
50 0.5
50 0.5
50 0.5
50 0.5
50 0.5
50 0.5
50 0.5
50 0.5
50 0.5
50 0.5
50 0.5
50 0.5
50 0.5
50 0.5
50 0.5
50 0.5
50 0.5
50 0.5
50 0.5
50 0.5
50 0.5
50 0.5
50 0.5
50 0.5
50 0.5
50 0.5
50 0.5
50 0.5
50 0.5
50 0.5
50 0.5
50 0.5
50 0.5
10000
"""
) == "0.000", "maximum-size case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1 / 1 0.5 / 1`|`0.500`| Trường hợp hữu ích tối thiểu và chuyển đổi đầu tiên | 
|`1 / 1 / 1 0.5 / 2`|`0.750`| Nỗ lực lặp đi lặp lại và hoàn thành hấp dẫn | 
|`1 / 2 / 1 0.5 / 1 0.5 / 2`|`0.250`| Nhiều mục tiêu và ranh giới nỗ lực chính xác | 
|`1 / 1 / 0 0.7 / 0`|`1.000`| Không cần bản sao và không cần thử | 
|`1 / 1 / 1 0.0 / 10000`|`0.000`| Mục tiêu bất khả thi | 
|`1 / 2 / 1 1.0 / 1 1.0 / 2`|`1.000`| Đảm bảo thành công | 
| 50 loại, mỗi loại 50 bản và 10000 lần thử |`0.000`| Kích thước tối đa và hành vi số | 

## Vỏ cạnh 

Đối với các mục không bắt buộc, mảng xác suất phẳng sẽ trống. Thuật toán trả về 1 ngay lập tức vì điều kiện hoàn thành đã được thỏa mãn. Để có đầu vào chính xác```
1
1
0 0.5
10
```

`total`bằng 0, vậy câu trả lời là`1.000`. Nếu không có trường hợp đặc biệt này, việc truy cập`probs[0]`sẽ không hợp lệ và vòng lặp DP chung sẽ không thể hiện chính xác yêu cầu trống. 

Đối với một mục tiêu không thể, hãy xem xét```
1
1
1 0.0
3
```Trạng thái ban đầu là`dp[0] = 1`. Mỗi lần thử sẽ nhân nó với (1-0=1), trong khi không có xác suất nào xảy ra`dp[1]`. Sau cả ba lần thử,`dp[1]`vẫn bằng 0, tạo ra`0.000`. 

Đối với một mục tiêu được đảm bảo, hãy xem xét```
1
1
1 1.0
1
```Lần thử đầu tiên có xác suất thất bại bằng 0. Do đó, sự chuyển đổi từ trạng thái 0 sang trạng thái 1 góp phần`1.0`, cho`1.000`. Thuật toán không cần một nhánh đặc biệt cho xác suất một. 

Trường hợp ranh giới chính xác```
1
2
1 0.5
1 0.5
2
```đòi hỏi hai lần thành công trong đúng hai lần thử. Sau lần thử thứ nhất, xác suất trạng thái là (0,5,0,5,0). Trong lần thử thứ hai, con đường duy nhất vào trạng thái hai là xác suất (0,5) hiện có ở trạng thái một, theo sau là thành công (0,5) khác, cho kết quả (0,25). Đầu ra là`0.250`. Điều này phát hiện các hoạt động triển khai vô tình yêu cầu thêm một nỗ lực để xử lý mục tiêu cuối cùng. 

Hộp có kích thước tối đa chứa 2500 bản sao bắt buộc và cho phép 10000 lần thử. DP vẫn chỉ lưu trữ 2501 trạng thái và liên tục cập nhật tiền tố có thể truy cập. Với xác suất thành công (0,5) cho mỗi bản sao, việc thu thập 2500 thành công trong 10000 lần thử là rất khó xảy ra nên kết quả được làm tròn chính xác là`0.000`. Trường hợp này rất hữu ích để kiểm tra cả giới hạn trạng thái và hành vi thời gian chạy.
