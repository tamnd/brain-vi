---
title: "CF 1639B - Truy tìm kho báu"
description: "Nhiệm vụ này là một vấn đề khám phá đồ thị tương tác. Chúng ta bắt đầu từ một đỉnh đã biết của đồ thị vô hướng liên thông. Mỗi đỉnh đều chứa một kho báu và lần đầu tiên chúng ta ghé thăm một đỉnh, chúng ta sẽ tự động đánh dấu nó bằng một lá cờ."
date: "2026-06-10T04:23:58+07:00"
tags: ["codeforces", "competitive-programming", "graphs", "interactive"]
categories: ["algorithms"]
codeforces_contest: 1639
codeforces_index: "B"
codeforces_contest_name: "Pinely Treasure Hunt Contest"
rating: 0
weight: 1639
solve_time_s: 74
verified: true
draft: false
---

[CF 1639B - Truy tìm kho báu](https://codeforces.com/problemset/problem/1639/B) 

**Đánh giá:** - 
**Thẻ:** đồ thị, tương tác 
**Thời gian giải:** 1 phút 14s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ này là một vấn đề khám phá đồ thị tương tác. Chúng ta bắt đầu từ một đỉnh đã biết của đồ thị vô hướng liên thông. Mỗi đỉnh đều chứa một kho báu và lần đầu tiên chúng ta ghé thăm một đỉnh, chúng ta sẽ tự động đánh dấu nó bằng một lá cờ. Từ vị trí hiện tại, chúng ta chỉ thấy, đối với mỗi đỉnh lân cận, bậc của nó và liệu nó đã chứa cờ hay chưa. Chúng tôi không biết nhãn hiệu thực sự của hàng xóm. 

Một điều phức tạp hơn nữa là thứ tự của các lân cận được chọn ngẫu nhiên một cách độc lập mỗi khi chúng ta đi vào một đỉnh. Ngay cả khi chúng ta quay lại cùng một vị trí, cạnh đầu tiên trong danh sách có thể tương ứng với một cạnh khác so với trước đó. Bản thân đồ thị không bao giờ thay đổi nhưng không có cách đánh số ổn định các cạnh đi ra. 

Mục tiêu không phải là xây dựng lại biểu đồ. Chúng ta chỉ cần thăm mỗi đỉnh ít nhất một lần trong khi sử dụng một số bước di chuyển khá nhỏ. 

Biểu đồ chứa tối đa 300 đỉnh và mỗi độ nhiều nhất là 50. Những giới hạn nhỏ như vậy có nghĩa là bất kỳ chiến lược nào có vài chục nghìn bước di chuyển đều có thể chấp nhận được. Vì tài liệu cuộc thi có sẵn các biểu đồ hoàn chỉnh nên thí sinh có thể điều chỉnh chiến lược của mình cho phù hợp với từng bài kiểm tra. Tuy nhiên, ý tưởng dự định đơn giản hơn nhiều: thiết kế một bước đi ngẫu nhiên ưu tiên những lãnh thổ chưa được khám phá. 

Cạm bẫy nguy hiểm nhất là giả định rằng một cạnh có thể được ghi nhớ theo vị trí của nó trong danh sách lân cận. 

Hãy xem xét một con đường```
1 - 2 - 3
```Giả sử chúng ta đang ở đỉnh 2 và trình tương tác hiện đang báo cáo```
degree 1, unflagged
degree 1, flagged
```Sau đó, sau khi quay lại đỉnh 2, thứ tự có thể trở thành```
degree 1, flagged
degree 1, unflagged
```Việc chọn "hàng xóm số 1" cả hai lần không tương ứng với việc đi qua cùng một cạnh. 

Một sai lầm khác là cố gắng tái tạo lại các đỉnh chỉ bằng các mẫu bậc. Hai hàng xóm khác nhau có thể có cấp độ giống hệt nhau và trạng thái cờ giống hệt nhau. 

Ví dụ```
    2
    |
1 - 3 - 4
```Đỉnh 1 và 4 đều có bậc 1. Khi đứng ở vị trí 3 thì không thể phân biệt được. Bất kỳ nỗ lực xác định nào để gán danh tính đều có thể dễ dàng trở nên không nhất quán. 

Cách tiếp cận đúng sẽ tránh được việc xác định hoàn toàn các hàng xóm. 

## Phương pháp tiếp cận 

Chiến lược đơn giản nhất là bước đi ngẫu nhiên hoàn toàn thống nhất. Bất cứ khi nào chúng ta đến một đỉnh, chúng ta chọn ngẫu nhiên một trong những đỉnh lân cận của nó. 

Phương pháp này đúng vì một biểu đồ được kết nối cuối cùng sẽ được bao phủ bởi một bước đi ngẫu nhiên với xác suất 1. Thật không may, thời gian bao phủ dự kiến ​​của một số biểu đồ là lớn. Ví dụ: đồ thị hình kẹo mút là những ví dụ nổi tiếng trong đó các bước đi ngẫu nhiên thuần túy yêu cầu khoảng$O(n^3)$chuyển động trong sự mong đợi. Ngay cả khi chỉ có 300 đỉnh, điều này có thể trở nên tốn kém không cần thiết. 

Quan sát quan trọng là bộ tương tác cho chúng ta biết đỉnh lân cận nào đã được truy cập. Vì mục tiêu của chúng ta là khám phá các đỉnh mới nên chúng ta phải luôn di chuyển đến một láng giềng không gắn cờ bất cứ khi nào một láng giềng như vậy tồn tại. 

Giả sử đỉnh hiện tại có một số đỉnh lân cận và ít nhất một trong số chúng chưa được ghé thăm. Đi đến một hàng xóm được gắn cờ không thể ngay lập tức tăng số lượng các đỉnh được phát hiện, trong khi việc đi đến một hàng xóm không được gắn cờ chắc chắn sẽ làm được. Vì vậy, việc ưu tiên những người hàng xóm chưa ghé thăm luôn có lợi. 

Cuối cùng, chúng ta đạt đến những vùng mà mọi đỉnh liền kề đều đã được nhìn thấy. Lúc đó chúng ta cần một số cơ chế để trốn thoát và tiếp tục khám phá. Chọn ngẫu nhiên trong số tất cả hàng xóm hoạt động tốt. Điều này tạo ra một bước đi ngẫu nhiên có sai lệch, mở rộng mạnh mẽ bất cứ khi nào có thể và mặt khác sẽ đi lang thang cho đến khi nó đạt đến đỉnh biên một lần nữa. 

Chiến lược này không yêu cầu phải xây dựng lại biểu đồ hoặc ghi nhớ các đặc điểm nhận dạng của cạnh. Nó chỉ sử dụng thông tin có sẵn ở bước hiện tại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force đi bộ ngẫu nhiên | Dự kiến ​​O(n³) di chuyển | O(1) | Được chấp nhận nhưng không hiệu quả | 
| Thích hàng xóm chưa ghé thăm | Dự kiến ​​nhỏ hơn nhiều, khoảng O(m) đến O(n²) trong thực tế | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Bắt đầu tại đỉnh đã cho. Nó ngay lập tức được coi là đã ghé thăm. 
2. Bất cứ khi nào trình tương tác báo cáo đỉnh hiện tại, hãy kiểm tra tất cả các mô tả lân cận. 
3. Thu thập các chỉ số tương ứng với các lân cận có cờ bằng 0. Chúng đại diện cho các đỉnh chưa bao giờ được ghé thăm. 
4. Nếu tập hợp này không trống, hãy chọn một trong các chỉ số đó. Việc thăm một đỉnh như vậy sẽ làm tăng số đỉnh được phát hiện thêm một. 
5. Nếu mỗi người hàng xóm đều đã có cờ, hãy chọn một người hàng xóm ngẫu nhiên trong số tất cả các lựa chọn có sẵn. Điều này cho phép cuộc đi bộ rời khỏi ngõ cụt và cuối cùng đến được những khu vực chưa được khám phá một lần nữa. 
6. Tiếp tục cho đến khi đầu ra tương tác`"AC"`. 

### Tại sao nó hoạt động 

Biểu đồ được kết nối. Bất cứ khi nào tồn tại một đỉnh lân cận chưa được thăm, thuật toán sẽ ngay lập tức phát hiện ra một đỉnh mới. Khi vùng lân cận cục bộ đã cạn kiệt, quá trình này sẽ trở thành bước đi ngẫu nhiên trên các đỉnh đã được phát hiện. 

Một bước đi ngẫu nhiên trên một đồ thị liên thông sẽ đến mọi đỉnh với xác suất là 1. Do đó, cuối cùng nó sẽ đến một đỉnh liền kề với một đỉnh chưa được khám phá. Tại thời điểm đó, thuật toán ngay lập tức mở rộng sang đỉnh đó. Lặp lại lập luận này cho thấy rằng mọi đỉnh cuối cùng đều được thăm. 

## Giải pháp Python 

Vì đây là sự cố tương tác nên mã sẽ liên lạc với người tương tác sau mỗi lần di chuyển.```python
import sys
import random

input = sys.stdin.readline

rng = random.Random()

t = int(input())

for _ in range(t):
    n, m, start, base_move_count = map(int, input().split())

    for _ in range(m):
        input()

    while True:
        parts = input().split()

        if parts[0] == "AC" or parts[0] == "F":
            break

        d = int(parts[1])

        unvisited = []
        all_neighbors = []

        ptr = 2
        for i in range(d):
            deg = int(parts[ptr])
            flag = int(parts[ptr + 1])
            ptr += 2

            all_neighbors.append(i + 1)
            if flag == 0:
                unvisited.append(i + 1)

        if unvisited:
            move = rng.choice(unvisited)
        else:
            move = rng.choice(all_neighbors)

        print(move)
        sys.stdout.flush()
```Mô tả biểu đồ được đưa ra trước khi bắt đầu tương tác, nhưng chiến lược này không cần đến nó. Những dòng đó chỉ đơn giản là đọc và loại bỏ. 

Đối với mỗi đỉnh được báo cáo, mã sẽ quét các mô tả lân cận. Biến`unvisited`lưu trữ các chỉ số có cờ bằng 0. Nếu danh sách này không trống thì một trong các phần tử của nó được chọn ngẫu nhiên một cách thống nhất. Ngược lại, một người hàng xóm ngẫu nhiên trong số tất cả những người hàng xóm sẽ được chọn. 

Các chỉ số lân cận dựa trên một vì bộ tương tác mong đợi các giá trị từ 1 đến`d`. Việc quên chuyển đổi này là nguyên nhân phổ biến dẫn đến câu trả lời sai. 

Một chi tiết tinh tế khác là xả đầu ra sau mỗi lần di chuyển. Nếu không xóa, người tương tác sẽ không bao giờ nhận được câu trả lời và giải pháp sẽ bị treo. 

## Ví dụ đã hoạt động 

Bởi vì sự tương tác thực sự là ngẫu nhiên nên việc minh họa chiến lược trên các tình huống cố định sẽ dễ dàng hơn. 

### Ví dụ 1 

Hãy xem xét```
1 - 2 - 3
```Giả sử chúng ta bắt đầu ở đỉnh 1. 

| Đỉnh hiện tại | Cờ hàng xóm | Bộ ứng cử viên | Nước đi được chọn | 
| --- | --- | --- | --- | 
| 1 | [0] | [2] | 2 | 
| 2 | [1,0] | [3] | 3 | 
| 3 | [1] | [2] | 2 | 

Hai nước đi đầu tiên luôn khám phá ra một đỉnh mới. Sau khi đạt đến đỉnh 3, tất cả kho báu đã được thu thập. 

Ví dụ này cho thấy thuật toán mở rộng một cách tham lam bất cứ khi nào có thể. 

### Ví dụ 2 

Hãy xem xét một đồ thị ngôi sao.```
    2
    |
3 - 1 - 4
    |
    5
```Bắt đầu từ đỉnh 1. 

| Đỉnh hiện tại | Cờ hàng xóm | Bộ ứng cử viên | Nước đi được chọn | 
| --- | --- | --- | --- | 
| 1 | [0,0,0,0] | cả bốn lá | một chiếc lá | 
| Lá | [1] | không | trung tâm | 
| 1 | [1,0,0,0] | ba chiếc lá chưa thăm | một chiếc lá | 
| Lá | [1] | không | trung tâm | 
| 1 | [1,1,0,0] | hai chiếc lá chưa thăm | một chiếc lá | 

Mỗi lần quay trở lại trung tâm sẽ để lộ những chiếc lá chưa được thăm dò còn lại và quá trình này tiếp tục cho đến khi khám phá hết tất cả những chiếc lá. 

Ví dụ này cho thấy tại sao cần phải di chuyển ngẫu nhiên khi đi vào ngõ cụt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(tổng số nước đi) | Mỗi bước di chuyển xử lý một mô tả đỉnh | 
| Không gian | O(50) | Bằng cấp không bao giờ vượt quá 50 | 

Bản thân biểu đồ chứa tối đa 300 đỉnh và độ tối đa là 50. Xử lý một bước tương tác là thời gian không đổi. Dung lượng bộ nhớ sử dụng không đáng kể so với giới hạn 256 MB. 

## Trường hợp thử nghiệm 

Các vấn đề tương tác không thể được kiểm tra bằng các cặp đầu vào-đầu ra thông thường vì trọng tài phản ứng với các bước đi của chúng ta. Tuy nhiên, chúng ta có thể kiểm tra logic quyết định cục bộ.```
def choose(flags):
    unvisited = [i + 1 for i, x in enumerate(flags) if x == 0]
    if unvisited:
        return unvisited
    return list(range(1, len(flags) + 1))

# one unvisited neighbor
assert choose([1, 0, 1]) == [2]

# all neighbors already visited
assert choose([1, 1, 1]) == [1, 2, 3]

# all neighbors unvisited
assert choose([0, 0, 0]) == [1, 2, 3]

# degree one vertex
assert choose([1]) == [1]

# mixed configuration
assert choose([0, 1, 0, 1]) == [1, 3]
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| [1,0,1] | [2] | Ưu tiên cho những người hàng xóm chưa được khám phá | 
| [1,1,1] | [1,2,3] | Dự phòng bước đi ngẫu nhiên | 
| [0,0,0] | [1,2,3] | Nhiều lựa chọn ứng viên | 
| [1] | [1] | Độ một đỉnh | 
| [0,1,0,1] | [1,3] | Lọc đúng | 

## Vỏ cạnh 

Xét một đường đi có hai đỉnh.```
1 - 2
```Bắt đầu từ đỉnh 1, hàng xóm duy nhất không được thăm, do đó thuật toán ngay lập tức chuyển sang đỉnh 2 và kết thúc. Các đỉnh bậc một không gây ra khó khăn gì đặc biệt. 

Một trường hợp thú vị khác là một chu trình```
1 - 2 - 3 - 4 - 1
```Giả sử chúng ta đã thăm các đỉnh 1, 2 và 3. Tại đỉnh 2, cả hai đỉnh lân cận đều được gắn cờ, do đó thuật toán thực hiện một bước di chuyển ngẫu nhiên. Cuối cùng, nó đạt tới đỉnh 3, nơi vẫn còn một đỉnh lân cận chưa được ghé thăm là 4. Khi đạt đến đỉnh biên như vậy, thuật toán sẽ mở rộng trở lại. 

Cuối cùng, xét các đỉnh đối xứng có bậc bằng nhau. Ví dụ,```
    2
    |
1 - 3 - 4
```Đỉnh 1 và 4 đều có bậc 1. Thuật toán không bao giờ cố gắng phân biệt chúng. Nó chỉ sử dụng trạng thái cờ của chúng, vì vậy thứ tự thay đổi của các hàng xóm không thể làm mất hiệu lực bất kỳ thông tin được lưu trữ nào. Điều này tránh được khó khăn chính của vấn đề.
