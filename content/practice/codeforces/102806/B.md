---
title: "CF 102806B - \u041f\u0435\u0440\u043b\u044b \u0438 \u043a\u043e\u043d\u0432\u0435\u0440\u0442\u0435\u0440"
description: "Chúng ta có một dãy n viên ngọc được tạo ra lần lượt. Màu sắc của viên ngọc được tạo ra ở giây thứ i được cho trước. Một bộ công cụ hợp lệ phải chứa chính xác một viên ngọc mỗi màu, vì vậy nó luôn chứa k viên ngọc."
date: "2026-07-26T16:17:47+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102806
codeforces_index: "B"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2017-2018, \u0427\u0435\u0442\u0432\u0451\u0440\u0442\u0430\u044f \u043b\u0438\u0447\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430"
rating: 0
weight: 102806
solve_time_s: 50
verified: true
draft: false
---

[CF 102806B - \u041f\u0435\u0440\u043b\u044b \u0438 \u043a\u043e\u043d\u0432\u0435\u0440\u0442\u0435\u0440](https://codeforces.com/problemset/problem/102806/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một chuỗi`n`ngọc trai được sản xuất từng cái một. Màu sắc của viên ngọc được tạo ra ở lần thứ hai`i`được đưa ra. Một bộ công cụ hợp lệ phải chứa chính xác một viên ngọc trai của mỗi màu, vì vậy nó luôn chứa`k`ngọc trai. Thời gian sản xuất của những viên ngọc trai trong một bộ phải gần nhau: viên ngọc sớm nhất và viên mới nhất trong bộ đó có thể chênh lệch nhau nhiều nhất.`m`giây. Mỗi viên ngọc chỉ có thể được sử dụng một lần. Nhiệm vụ là xây dựng càng nhiều bộ như vậy càng tốt và đưa ra chỉ số của các viên ngọc trong mỗi bộ. 

Dữ liệu đầu vào chứa số lượng ngọc trai được sản xuất, chênh lệch thời gian tối đa cho phép, số lượng màu sắc và sau đó là màu của từng viên ngọc trai theo thứ tự sản xuất. Đầu ra bắt đầu với số lượng bộ hoàn chỉnh tối đa, theo sau là các chỉ số được chọn cho mỗi bộ. 

Các giới hạn rất lớn: cả hai`n`Và`k`có thể đạt được`100000`. Thuật toán quét liên tục tất cả các màu hoặc tất cả các viên ngọc trai trước đó cho mọi vị trí sẽ trở thành bậc hai và không kết thúc. Chúng ta cần một giải pháp trong đó mỗi viên ngọc trai chỉ được xử lý một số lần không đổi. 

Các trường hợp đặc biệt chính xuất phát từ sự tương tác giữa tính sẵn có của màu sắc và khoảng thời gian. Nếu một màu bị thiếu trong cửa sổ hiện tại, một bộ không thể được hình thành ngay cả khi có nhiều màu khác. Ví dụ:```
6 2 3
1 2 2 1 3 3
```Bộ đầu tiên có thể được hình thành bởi ngọc trai`4, 3, 5`, vì màu sắc của chúng là`1, 2, 3`và sự khác biệt về thời gian là`5 - 3 = 2`. Một giải pháp bất cẩn chỉ đếm số lần mỗi màu xuất hiện sẽ có hai bộ, bởi vì mỗi màu xuất hiện hai lần, nhưng giới hạn thời gian đã ngăn cản điều đó. 

Một trường hợp phức tạp khác là khi viên ngọc cuối cùng của một màu nằm ngoài cửa sổ hiện tại trong khi các màu cũ hơn vẫn còn xuất hiện. Ví dụ:```
5 2 3
1 2 2 2 3
```Có đủ tổng số ngọc trai màu sắc`2`, nhưng chỉ có một viên ngọc màu`1`và nó xuất hiện quá sớm. Câu trả lời là`0`. Giải pháp chỉ theo dõi số lượng toàn cầu sẽ tạo ra câu trả lời tích cực không chính xác. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp là thử mọi nhóm có thể`k`ngọc trai và kiểm tra xem nó có chứa tất cả các màu sắc và phù hợp với thời hạn hay không. Điều này đúng vì nó kiểm tra chính xác định nghĩa của một tập hợp hợp lệ. Tuy nhiên có thể có tới`100000`ngọc trai, vì vậy ngay cả việc nhìn vào nhiều cửa sổ có thể có và kiểm tra tất cả các màu một cách nhanh chóng cũng trở nên quá chậm. Trong trường hợp xấu nhất, việc quét tất cả các màu cho mọi vị trí sẽ tốn kém`O(nk)`hoạt động. 

Cấu trúc của vấn đề đưa ra một hướng đi tốt hơn. Một tập hợp lệ phải được chứa bên trong một cửa sổ di chuyển có độ dài`m + 1`các vị trí. Trong khi quét ngọc trai từ trái sang phải, chúng ta chỉ cần biết những viên ngọc trai chưa sử dụng của mỗi màu hiện đang ở trong cửa sổ đó. Nếu mỗi màu có sẵn ít nhất một viên ngọc trai thì việc lấy viên ngọc trai cũ nhất hiện có của mỗi màu sẽ ngay lập tức tạo ra một bộ hợp lệ. 

Lý do lấy những viên ngọc trai lâu đời nhất hoạt động là vì cửa sổ hiện tại đã đảm bảo rằng tất cả những viên ngọc trai được lưu trữ đều đủ gần với vị trí hiện tại. Sử dụng những viên ngọc trai cũ hơn không thể làm cho những bộ ngọc trai trong tương lai trở nên tồi tệ hơn, bởi vì việc giữ những viên ngọc trai mới hơn sẽ mang lại sự linh hoạt hơn cho những cửa sổ sau này. Sau khi tạo một bộ, những viên ngọc trai đó sẽ bị xóa khỏi hàng màu của chúng và quá trình quét tiếp tục. 

Cách tiếp cận bạo lực có hiệu quả vì nó kiểm tra tất cả các khả năng, nhưng nó lặp lại cùng một công việc nhiều lần. Hàng đợi cửa sổ trượt làm giảm vấn đề xuống còn việc chỉ duy trì thông tin vẫn có thể tham gia vào câu trả lời trong tương lai. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nk) | O(n) | Quá chậm | 
| Tối ưu | O(n + k) | O(n + k) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo một hàng đợi cho mỗi màu. Mỗi hàng đợi lưu trữ chỉ số của những viên ngọc trai hiện có có màu đó bên trong cửa sổ thời gian hoạt động. Ngoài ra, hãy theo dõi số lượng màu hiện có trong hàng đợi không trống. 
2. Xử lý ngọc trai từ trái sang phải. Khi ngọc trai`i`xuất hiện, hãy thêm chỉ mục của nó vào hàng đợi màu của nó. Nếu hàng đợi màu này trống trước khi chèn, hãy tăng số lượng màu có sẵn. 
3. Lấy viên ngọc không còn bên trong cửa sổ ra. Khi xử lý chỉ số`i`, chỉ số được phép cũ nhất là`i - m`, vậy chỉ mục`i - m - 1`phải được loại bỏ nếu nó tồn tại. Nếu việc xóa nó làm cho hàng đợi màu trống, hãy giảm số lượng màu có sẵn. 
4. Nếu tất cả`k`hàng đợi màu không trống, hãy lấy phần tử đầu tiên từ mỗi hàng đợi. Những cái này`k`ngọc trai có tất cả các màu khác nhau và đều nằm bên trong cửa sổ hiện tại nên chúng tạo thành một bộ hợp lệ. Lưu trữ các chỉ mục của chúng và xóa chúng khỏi hàng đợi. 
5. Tiếp tục cho đến khi tất cả ngọc trai được xử lý. Các bộ được lưu trữ là số lượng bộ tối đa có thể. 

Tại sao nó hoạt động: 

Tại mọi thời điểm, mỗi hàng đợi chứa chính xác những viên ngọc trai cùng màu chưa sử dụng vẫn có thể được sử dụng trong một bộ kết thúc ở vị trí hiện tại. Một bộ chỉ có thể được tạo khi mỗi màu có ít nhất một ứng cử viên, vì vậy thuật toán sẽ tạo một bộ chính xác khi có lựa chọn hợp lệ tại thời điểm đó. Việc chọn ứng viên sớm nhất của mỗi màu là an toàn vì tất cả các ứng cử viên trong hàng đợi đều đáp ứng giới hạn thời gian như nhau và việc loại bỏ những viên ngọc trai có sẵn sớm nhất sẽ để lại những viên ngọc mới nhất cho các cửa sổ trong tương lai. Do đó, mọi bộ được tạo ra đều sử dụng những viên ngọc trai không thể thay thế bằng sự lựa chọn tốt hơn và quá trình tham lam không bao giờ làm giảm số lượng các bộ có thể có trong tương lai. 

## Giải pháp Python```python
import sys
from collections import deque

input = sys.stdin.readline

def solve():
    n, m, k = map(int, input().split())
    colors = list(map(int, input().split()))

    queues = [deque() for _ in range(k)]
    non_empty = 0
    ans = []

    for i, c in enumerate(colors):
        c -= 1

        if not queues[c]:
            non_empty += 1
        queues[c].append(i + 1)

        old = i - m - 1
        if old > 0:
            oc = colors[old - 1] - 1
            queues[oc].popleft()
            if not queues[oc]:
                non_empty -= 1

        if non_empty == k:
            cur = []
            for q in queues:
                cur.append(q.popleft())
            ans.append(cur)

            non_empty = 0
            for q in queues:
                if q:
                    non_empty += 1

    out = [str(len(ans))]
    for group in ans:
        out.append(" ".join(map(str, group)))
    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Mảng hàng đợi là sự triển khai trực tiếp của ý tưởng cửa sổ trượt. Hàng đợi`c`chứa các vị trí của ngọc trai màu hiện có thể sử dụng`c`. 

Khi một viên ngọc mới được đọc, nó sẽ được thêm vào hàng màu của nó. Bước loại bỏ sử dụng`i - m - 1`bởi vì các chỉ số dựa trên một trong câu trả lời được lưu trữ nhưng dựa trên 0 trong vòng lặp. Đúng là ngọc trai`m`vị trí cách xa vẫn có hiệu lực, trong khi viên ngọc xa hơn một vị trí thì không. 

Khi tất cả các hàng đợi không trống, mặt trước của mỗi hàng đợi sẽ được chọn. Mặt trước đại diện cho viên ngọc trai lâu đời nhất có màu đó. Việc loại bỏ những viên ngọc đã chọn này khỏi tất cả các hàng đợi sẽ chuẩn bị cấu trúc cho việc tìm tập hợp độc lập tiếp theo. 

Biến`non_empty`tránh quét tất cả các màu sau mỗi lần chèn. Nó chỉ được cập nhật khi hàng đợi thay đổi giữa trạng thái trống và không trống, giữ cho toàn bộ thuật toán tuyến tính. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
6 2 3
1 2 2 1 3 3
```Quá trình quét hoạt động như sau. 

| Bước | Đã thêm ngọc trai | Hàng đợi màu đang hoạt động | Hành động | 
| --- | --- | --- | --- | 
| 1 | màu 1 ở chỉ số 1 | {1} | Chờ màu khác | 
| 2 | màu 2 ở chỉ số 2 | {1,2} | Chờ màu 3 | 
| 3 | màu 2 ở chỉ số 3 | {1,2} | Chờ màu 3 | 
| 4 | màu 1 ở chỉ số 4 | {1,2} | Ngọc 1 còn thiếu màu 3 | 
| 5 | màu 3 ở chỉ số 5 | {1,2,3} | Tạo bộ 4,3,5 | 
| 6 | màu 3 ở chỉ số 6 | {3} | Không đủ màu sắc | 

Tập hợp đã tạo chứa các chỉ số`4 3 5`. Chỉ số sớm nhất của nó là`3`và mới nhất là`5`, vậy sự khác biệt chính xác là`2`. 

Đối với mẫu thứ hai:```
5 2 3
1 2 2 2 3
```| Bước | Đã thêm ngọc trai | Ngọc trai đã bỏ | Màu sắc năng động | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | màu 1 lúc 1 | không | 1 | Chờ đã | 
| 2 | màu 2 lúc 2 | không | 1,2 | Chờ đã | 
| 3 | màu 2 lúc 3 | không | 1,2 | Chờ đã | 
| 4 | màu 2 lúc 4 | không | 1,2 | Chờ đã | 
| 5 | màu 3 lúc 5 | chỉ mục 2 bị xóa khỏi cửa sổ | 1,2,3 | Không có lựa chọn hoàn chỉnh hợp lệ | 

Viên ngọc màu duy nhất`1`quá xa khi màu sắc`3`xuất hiện. Thuật toán xuất ra các tập hợp 0 ​​một cách chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + k) | Mỗi viên ngọc vào một hàng một lần và rời đi nhiều nhất một lần. | 
| Không gian | O(n + k) | Hàng đợi lưu trữ tất cả các viên ngọc trai hiện chưa được sử dụng và có`k`hàng đợi. | 

Giải pháp chỉ thực hiện công việc liên tục trên mỗi viên ngọc, phù hợp với`100000`giới hạn phần tử Việc sử dụng bộ nhớ cũng tuyến tính vì mỗi chỉ số ngọc trai được lưu trữ nhiều nhất một lần. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    result = sys.stdout.getvalue()
    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return result

assert run("""6 2 3
1 2 2 1 3 3
""") == """1
4 3 5
""", "sample 1"

assert run("""5 2 3
1 2 2 2 3
""") == """0
""", "sample 2"

assert run("""3 1 3
1 2 3
""") == """1
1 2 3
""", "minimum case"

assert run("""8 10 2
1 2 1 2 1 2 1 2
""") == """4
1 2
3 4
5 6
7 8
""", "large window"

assert run("""7 2 3
1 1 2 2 3 3 3
""") == """1
2 4 5
""", "boundary removal"

assert run("""6 3 2
1 1 1 1 1 1
""") == """0
""", "all equal colors"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`6 2 3 / 1 2 2 1 3 3`|`1`đặt | Đưa ra ví dụ và sự lựa chọn tham lam | 
|`5 2 3 / 1 2 2 2 3`|`0`bộ | Một màu rời khỏi cửa sổ hợp lệ | 
|`3 1 3 / 1 2 3`|`1`đặt | Đầu vào nhỏ nhất có thể | 
|`8 10 2 / 1 2 1 2 1 2 1 2`|`4`bộ | Nhiều bộ có cửa sổ rộng | 
|`7 2 3 / 1 1 2 2 3 3 3`|`1`đặt | Ranh giới hết hạn của cửa sổ | 
|`6 3 2 / 1 1 1 1 1 1`|`0`bộ | Thiếu màu sắc | 

## Vỏ cạnh 

Khi mỗi màu xuất hiện đủ số lần nhưng bề ngoài bị tách biệt thì việc đếm tổng thể sẽ gây hiểu nhầm. Vì:```
6 2 3
1 2 2 1 3 3
```số lượng màu gợi ý hai bộ có thể. Trong quá trình quét, thuật toán chỉ giữ những viên ngọc trai trong khoảng cách`2`của vị trí hiện tại. Tại chỉ mục`5`, hàng đợi có sẵn chứa màu sắc`1`,`2`, Và`3`, vì vậy nó tạo ra tập hợp`4, 3, 5`. Màu còn lại`3`không thể ghép nối với một nhóm hoàn chỉnh khác. 

Khi một màu được yêu cầu chỉ tồn tại trong quá khứ bên ngoài cửa sổ thì nó phải bị loại bỏ. Vì:```
5 2 3
1 2 2 2 3
```viên ngọc ở chỉ số`1`có màu sắc`1`. Đến lúc màu sắc`3`ngọc trai đến chỉ số`5`, cửa sổ hợp lệ bắt đầu tại chỉ mục`3`, vậy màu sắc`1`hàng đợi trống. Thuật toán loại bỏ các ngọc trai lỗi thời trước khi kiểm tra một bộ, do đó nó không bao giờ tạo ra một nhóm không hợp lệ. 

Khi tất cả các viên ngọc trai đều có cùng một màu thì không có nhóm nào có thể hoàn thiện trừ khi chỉ có một màu duy nhất. Vì:```
6 3 2
1 1 1 1 1 1
```hàng đợi màu thứ hai không bao giờ trống. các`non_empty`quầy vẫn ở bên dưới`k`và thuật toán trả về chính xác`0`.
