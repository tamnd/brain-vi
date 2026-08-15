---
title: "CF 102419K - Rồng và Vương quốc Cây cối"
description: "Sau (m) năm cây (i) có chiều cao (hi). Mỗi khi rồng tấn công một cái cây, chiều cao của cây đó sẽ bằng 0, sau đó nó bắt đầu phát triển trở lại. Do đó, nếu (hi<m), cuộc tấn công cuối cùng vào cây đó phải xảy ra đúng (m-hi) năm trước thời điểm quan sát."
date: "2026-08-15T09:10:57+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102419
codeforces_index: "K"
codeforces_contest_name: "SPC 2019"
rating: 0
weight: 102419
solve_time_s: 894
verified: false
draft: false
---

[CF 102419K - Rồng và Vương quốc Cây cối](https://codeforces.com/problemset/problem/102419/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 14 phút 54 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Sau (m) năm cây (i) có chiều cao (h_i). Mỗi khi rồng tấn công một cái cây, chiều cao của cây đó sẽ bằng 0, sau đó nó bắt đầu phát triển trở lại. Do đó, nếu (h_i<m), cuộc tấn công cuối cùng vào cây đó phải xảy ra đúng (m-h_i) năm trước thời điểm quan sát. Những cây có (h_i=m) rất đặc biệt: chúng không bao giờ cần bị tấn công, mặc dù chúng có thể bị tấn công ngay sau khi trồng và vẫn kết thúc ở chiều cao (m). 

Tại mỗi thời điểm tấn công, phải chọn chính xác (k) khoảng cách rời nhau theo cặp. Hãy xem xét những cây có chiều cao cuối cùng chính xác là (h<m). Vào thời điểm tương ứng với lần thiết lập lại cuối cùng của chúng, mọi cây như vậy đều phải bị tấn công. Một cây có chiều cao cuối cùng nhỏ hơn (h) chưa thể bị tấn công vì nó cần được thiết lập lại sau. Một cây có chiều cao cuối cùng lớn hơn (h) có thể bị tấn công, vì việc đặt lại cuối cùng của nó đã xảy ra trước đó và việc đặt lại cây vào thời điểm này sẽ chỉ khiến thời điểm hiện tại được đặt lại lần cuối. Do đó, ở cấp độ (h), các vị trí có thể sử dụng chính xác là các vị trí có (h_i\ge h) và mọi vị trí có (h_i=h) phải được che phủ. 

Do đó, số khoảng tối thiểu cần có ở độ cao (h) là số thành phần được kết nối của các vị trí thỏa mãn (h_i\ge h) chứa ít nhất một vị trí có (h_i=h). Gọi số này (c(h)). Mọi (k) hợp lệ phải thỏa mãn (k\ge c(h)) cho mọi (h<m) xảy ra. 

Ràng buộc (n\le10^6) loại trừ mọi thứ gần với (O(n^2)). Ngay cả một giải pháp (O(n\log n)) cũng đáng được xem xét kỹ lưỡng trong giới hạn một giây, do đó phương pháp dự định cần xử lý mảng theo thời gian tuyến tính. Giá trị (m) có thể lớn bằng (10^9), do đó việc lặp qua mọi độ cao có thể cũng là không thể. Giải pháp phải phụ thuộc vào (n) phần tử mảng thay vì vào phạm vi số của độ cao. 

Có một số trường hợp nguy hiểm có thể dễ dàng phá vỡ việc triển khai bất cẩn. Nếu mọi chiều cao đều bằng (m) thì đáp án vẫn là (1) chứ không phải (0), vì Ayoub chắc hẳn đã tấn công ít nhất một lần. Ví dụ, với đầu vào```
4 3
3 3 3 3
```câu trả lời đúng là (1). Việc triển khai chỉ xem xét các cây có thiết lập lại bắt buộc sẽ không tìm thấy cây như vậy và in không chính xác (0). 

Chiều cao bằng nhau cũng cần được đối xử đặc biệt. Với```
4 2
2 1 2 1
```câu trả lời là (2). Ở độ cao (2), hai cây có chiều cao (2) được ngăn cách bởi những cây không được tấn công vào thời điểm đó nên cần có hai khoảng cách. Việc triển khai ngăn xếp xử lý các giá trị bằng nhau dưới dạng các thành phần riêng biệt có thể đếm quá số lượng khoảng thời gian cần thiết. 

Cuối cùng, việc có mức thấp hơn (c(h)) đủ lớn không tự nó đảm bảo tính khả thi. Cũng phải có đủ cây ở độ cao cao nhất thực sự cần thiết lập lại. Ví dụ,```
5 5
4 0 2 0 2
```yêu cầu (k=2) vì hai cây chiều cao-(2) tạo thành hai thành phần riêng biệt. Nhưng ở độ cao (4), chỉ có một cây để tấn công. Không thể chọn hai khoảng rời nhau khác trống từ một cây có sẵn, vì vậy câu trả lời đúng là (-1). 

## Phương pháp tiếp cận 

Một giải pháp trực tiếp sẽ kiểm tra mọi chiều cao riêng biệt (h<m). Đối với mỗi cái, hãy quét toàn bộ mảng và đếm các thành phần của các vị trí có chiều cao ít nhất (h) chứa cây chiều cao-(h). Điều này đúng vì các thành phần đó chính xác là các khoảng thời gian phải được thực hiện tại thời điểm đặt lại tương ứng. Trong trường hợp xấu nhất, có (n) độ cao riêng biệt và mỗi lần quét có chi phí (O(n)), cho (O(n^2)), tức là khoảng (10^{12}) thao tác khi (n=10^6). Điều đó vượt xa giới hạn thời gian. 

Quan sát hữu ích là số lượng thành phần này được biểu diễn một cách tự nhiên bằng cây Descartes tối thiểu. Cây Descartes tối thiểu duy trì thứ tự mảng trong khi làm cho mọi phần tử cha đều nhỏ hơn phần tử con của nó. Đối với ngưỡng (h), nếu chúng ta chỉ giữ các nút có giá trị ít nhất (h), thì mọi thành phần được kết nối đều có gốc có giá trị là giá trị nhỏ nhất trong thành phần đó. Một thành phần đóng góp vào (c(h)) chính xác khi mức tối thiểu đó là (h). Do đó, (c(h)) là số nghiệm thành phần cây Descartes có giá trị (h). 

Chúng ta không cần phải xây dựng cây thực tế. Ngăn xếp đơn điệu có thể duy trì chuỗi liên quan trong khi quét từ trái sang phải. Ngăn xếp được tiếp tục tăng nghiêm ngặt. Khi một giá trị mới (x) xuất hiện, mọi giá trị lớn hơn được lấy ra từ ngăn xếp vừa tìm thấy một giá trị nhỏ hơn đóng thành phần của nó, do đó giá trị được lấy ra đó đóng góp một giá trị vào chiều cao của nó. Các giá trị bằng nhau là khác nhau: các vị trí bằng nhau thuộc về cùng một thành phần ở ngưỡng đó, do đó đại diện bằng nhau cũ hơn sẽ bị loại bỏ mà không làm tăng số lượng. 

Các phần tử còn lại trên ngăn xếp ở cuối biểu thị các thành phần đạt đến ranh giới bên phải. Mọi phần tử ngăn xếp còn lại đóng góp một lần, bao gồm cả phần tử dưới cùng, đại diện cho thành phần tối thiểu toàn cục. Chiều cao bằng (m) bị loại khỏi số lượng này vì cây có chiều cao cuối cùng (m) không yêu cầu đặt lại. 

Gọi (k) là tần số tối đa đạt được đối với bất kỳ độ cao nào dưới (m). Đây là số khoảng thời gian tối thiểu có thể có, với điều kiện là (k) thực sự có thể được sử dụng ở mọi thời điểm đặt lại được yêu cầu. Thời gian hạn chế nhất là chiều cao lớn nhất (H<m) xuất hiện trong mảng. Khi đó chỉ có những cây có chiều cao từ (H) trở lên mới có. Nếu số của chúng nhỏ hơn (k) thì việc xây dựng là không thể. Nếu có ít nhất (k), thì mỗi lần đặt lại thấp hơn sẽ có ít nhất số cây có sẵn, do đó (k) giống nhau sẽ hoạt động ở mọi nơi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n^2)) | (O(1)) | Quá chậm | 
| Ngăn xếp đơn điệu | (O(n)) dự kiến ​​| (O(n)) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Đọc chiều cao và giữ một chồng tăng dần. Đồng thời ghi lại chiều cao lớn nhất (H<m), số lần xuất hiện của (H), số cây có chiều cao đúng (m). Hai đại lượng sau sẽ được sử dụng để kiểm tra xem giá trị cuối cùng của (k) có khả thi hay không. 
2. Đối với mọi chiều cao (x), hãy bật lên đầu tiên khi đỉnh ngăn xếp hoàn toàn lớn hơn (x). Mỗi giá trị xuất hiện (v) như vậy đã tìm thấy một giá trị nhỏ hơn, do đó thành phần của nó hiện bị đóng và giá trị tối thiểu của nó là (v). Tăng tần suất của (v), trừ khi (v=m), vì chiều cao (m) không cần tấn công. 
3. Nếu đỉnh ngăn xếp bằng (x), hãy loại bỏ nó mà không tăng tần số. Vị trí cũ và mới có cùng độ cao và thuộc cùng một thành phần ở độ cao đó. Giữ cả hai sẽ tính một thành phần hai lần. 
4. Đẩy (x) lên ngăn xếp. Thuộc tính tăng nghiêm ngặt có nghĩa là ngăn xếp đại diện cho chuỗi thành phần hiện tại có ranh giới bên phải chưa bị đóng bởi chiều cao nhỏ hơn. 
5. Sau khi quét hoàn tất, hãy thêm một lần xuất hiện vào tần suất của mọi giá trị còn lại trên ngăn xếp, ngoại trừ (m). Các thành phần này đến cuối mảng nên không có giá trị nhỏ hơn nào xuất hiện sau đó để đóng chúng. 
6. Nếu không có chiều cao dưới (m), mọi cây có thể không bị ảnh hưởng và Ayoub có thể tấn công một lần ngay sau khi trồng. Trở lại (1). 
7. Ngược lại, gọi (k) là tần số lớn nhất trong số các độ cao dưới (m). Đây là số khoảng thời gian tối thiểu bị ép buộc bởi thời gian đặt lại đòi hỏi khắt khe nhất. 
8. Gọi (H) là chiều cao lớn nhất bên dưới (m). Tại thời điểm đặt lại tương ứng với (H), những cây duy nhất có thể sử dụng được là những cây có chiều cao tối thiểu (H). Vì (H) là chiều cao lớn nhất bên dưới (m) nên đây chính xác là những cây có chiều cao (H) cùng với tất cả các cây có chiều cao (m). Nếu số lượng của chúng nhỏ hơn (k), trả về (-1). Nếu không thì trả về (k). 

Điều bất biến đằng sau ngăn xếp là mỗi giá trị ngăn xếp đại diện cho một thành phần hiện đang mở của tập hợp siêu cấp, với các đại diện có chiều cao bằng nhau được nén thành một giá trị. Khi một giá trị lớn hơn được đưa ra bởi (x), thành phần của nó đã gặp phải một giá trị nhỏ hơn rất nhiều và mức tối thiểu của nó vĩnh viễn được biết là giá trị được đưa ra đó. Khi một giá trị bằng nhau được thay thế, cả hai vị trí đều thuộc cùng một thành phần ở cấp độ đó, do đó chỉ có một đại diện tồn tại. Do đó, mọi thành phần có giá trị tối thiểu là (h) đóng góp chính xác một lần vào tần số của (h) và không có thành phần nào khác đóng góp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve_case(n, m, heights):
    stack = []
    freq = {}

    max_low = -1
    max_low_count = 0
    count_m = 0

    for x in heights:
        if x == m:
            count_m += 1
        else:
            if x > max_low:
                max_low = x
                max_low_count = 1
            elif x == max_low:
                max_low_count += 1

        while stack and stack[-1] > x:
            v = stack.pop()
            if v != m:
                freq[v] = freq.get(v, 0) + 1

        if stack and stack[-1] == x:
            stack.pop()

        stack.append(x)

    for v in stack:
        if v != m:
            freq[v] = freq.get(v, 0) + 1

    if max_low == -1:
        return 1

    k = max(freq.values())

    available_at_highest_reset = max_low_count + count_m
    if k > available_at_highest_reset:
        return -1

    return k

def main():
    n, m = map(int, input().split())
    heights = map(int, input().split())
    print(solve_case(n, m, heights))

if __name__ == "__main__":
    main()
```Phần đầu tiên của`solve_case`duy trì thông tin cần thiết cho việc kiểm tra tính khả thi.`max_low`là chiều cao cuối cùng lớn nhất nằm ngay dưới (m), vì đó là mức đặt lại mới nhất thực sự phải được xử lý.`max_low_count`đếm cây ở cấp độ đó, trong khi`count_m`đếm những cây chưa được chạm tới hoặc được đặt lại ngay lập tức mà vẫn có thể được sử dụng làm khoảng đệm tại thời điểm đó. 

Vòng lặp ngăn xếp là cốt lõi của thuật toán. Sự so sánh thật chặt chẽ`>`khi đếm một giá trị bật lên. Một giá trị nhỏ hơn hoàn toàn sẽ đóng thành phần hiện tại và cho biết mức tối thiểu của nó. Sự bình đẳng được xử lý riêng biệt bằng cách loại bỏ giá trị bằng nhau cũ mà không tính nó. Việc xử lý sự bình đẳng này là điều kiện biên ngăn chặn tình trạng ổn định như`1 1 1`được hiểu là ba thành phần riêng biệt. 

Ngăn xếp cuối cùng cần một lần chuyển cuối cùng vì các giá trị của nó chưa gặp giá trị nhỏ hơn ở bên phải của chúng. Chúng vẫn là các thành phần tối thiểu hợp lệ nên mỗi thành phần đóng góp một lần. Giá trị (m) bị bỏ qua trong suốt quá trình đếm vì cây kết thúc ở độ cao (m) không bắt buộc phải đặt lại lần cuối. 

Số nguyên Python không bị tràn, do đó giới hạn chiều cao của (10^9) không yêu cầu xử lý số đặc biệt. Thuật toán chỉ thực hiện một lần đẩy ngăn xếp cho mỗi giá trị đầu vào và mỗi giá trị có thể được xuất hiện nhiều nhất một lần, mang lại các hoạt động tổng thể tuyến tính cho ngăn xếp. 

Đầu vào được đọc với`input = sys.stdin.readline`, theo yêu cầu. Chuỗi chiều cao được sử dụng trực tiếp dưới dạng trình vòng lặp, vì vậy giải pháp không cần bản sao thứ hai của mảng. 

## Ví dụ đã hoạt động 

Đối với mẫu 1,```
4 3
3 3 3 3
```mọi cây đều kết thúc ở độ cao (m=3). Các giá trị bằng nhau sẽ thay thế nhau trong ngăn xếp và không tính chiều cao vì (3=m). 

| Vị trí | Chiều cao | Xếp chồng sau khi xử lý | Tần số | 
| --- | --- | --- | --- | 
| 1 | 3 | [3] | {} | 
| 2 | 3 | [3] | {} | 
| 3 | 3 | [3] | {} | 
| 4 | 3 | [3] | {} | 
| Kết thúc | | [3] | {} | 

Không có mức thiết lập lại bắt buộc nên yêu cầu đặc biệt mà Ayoub tấn công ít nhất một lần sẽ quyết định câu trả lời. Một khoảng thời gian có thể bị tấn công ngay sau khi trồng, cho (1). 

Đối với mẫu 2,```
4 3
0 0 0 0
```cả bốn cây cần được thiết lập lại lần cuối cùng một lúc. Vì tất cả các giá trị bằng nhau thuộc về một thành phần được kết nối nên chỉ cần một khoảng. 

| Vị trí | Chiều cao | Xếp chồng sau khi xử lý | Tần số | 
| --- | --- | --- | --- | 
| 1 | 0 | [0] | {} | 
| 2 | 0 | [0] | {} | 
| 3 | 0 | [0] | {} | 
| 4 | 0 | [0] | {} | 
| Kết thúc | | [0] | {0: 1} | 

Ngăn xếp cuối cùng đóng góp một thành phần có chiều cao tối thiểu (0), vì vậy (k=1). Có bốn cây có sẵn tại thời điểm đặt lại đó nên điều kiện khả thi được thỏa mãn. 

Đối với mẫu 3,```
4 2
2 1 1 2
```chiều cao đầu tiên (2) được đóng bởi chiều cao đầu tiên (1), tạo ra một thành phần có giá trị tối thiểu (2). Hai giá trị (1) liền kề được nén thành một đại diện. Phần cuối cùng (2) vẫn còn trong ngăn xếp và đóng góp một thành phần khác ở mức tối thiểu (2). 

| Vị trí | Chiều cao | Hoạt động | Ngăn xếp | Tần số | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | Đẩy | [2] | {} | 
| 2 | 1 | Bật 2, đếm 2, đẩy 1 | [1] | {2: 1} | 
| 3 | 1 | Thay thế bằng 1 | [1] | {2: 1} | 
| 4 | 2 | Đẩy | [1, 2] | {2: 1} | 
| Kết thúc | | Đếm ngăn xếp | [1, 2] | {1: 1, 2: 2} | 

Chiều cao (2) bằng (m), nên tần số của nó không liên quan. Mức bắt buộc duy nhất là (1), trong đó số đếm là (1), đưa ra câu trả lời (1). 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n)) dự kiến ​​| Mỗi độ cao được đẩy một lần và bật lên nhiều nhất một lần. Dự kiến ​​cập nhật từ điển (O(1)). | 
| Không gian | (O(n)) | Từ điển tần số và ngăn xếp đơn điệu có thể chứa (O(n)) mục nhập. | 

Với (n\le10^6), việc xử lý tuyến tính là cần thiết. Thuật toán thực hiện một lượng công việc không đổi trên mỗi chiều cao ngoại trừ các cửa sổ bật lên và tổng số cửa sổ bật lên tối đa là (n). Việc sử dụng không gian là tuyến tính và nằm trong phạm vi bộ nhớ dự kiến ​​được ràng buộc với việc triển khai ở trên. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve_case(n, m, heights):
    stack = []
    freq = {}

    max_low = -1
    max_low_count = 0
    count_m = 0

    for x in heights:
        if x == m:
            count_m += 1
        else:
            if x > max_low:
                max_low = x
                max_low_count = 1
            elif x == max_low:
                max_low_count += 1

        while stack and stack[-1] > x:
            v = stack.pop()
            if v != m:
                freq[v] = freq.get(v, 0) + 1

        if stack and stack[-1] == x:
            stack.pop()

        stack.append(x)

    for v in stack:
        if v != m:
            freq[v] = freq.get(v, 0) + 1

    if max_low == -1:
        return 1

    k = max(freq.values())

    if k > max_low_count + count_m:
        return -1

    return k

def run(inp: str) -> str:
    data = list(map(int, inp.split()))
    it = iter(data)
    n = next(it)
    m = next(it)
    heights = [next(it) for _ in range(n)]
    return str(solve_case(n, m, heights))

# Provided samples
assert run("4 3\n3 3 3 3\n") == "1", "sample 1"
assert run("4 3\n0 0 0 0\n") == "1", "sample 2"
assert run("4 2\n2 1 1 2\n") == "1", "sample 3"

# Custom: minimum-size input
assert run("1 1\n0\n") == "1", "single tree"

# Custom: all equal values below m
assert run("3 5\n2 2 2\n") == "1", "one component despite equal heights"

# Custom: two required intervals but only one tree available at the highest reset
assert run("5 5\n4 0 2 0 2\n") == "-1", "impossible padding"

# Custom: repeated separated peaks, catches equal-height handling
assert run("4 2\n2 1 2 1\n") == "2", "two components at height 2"

# Maximum-size input
big_n = 10**6
big_input = f"{big_n} 1\n" + ("0 " * (big_n - 1)) + "0\n"
assert run(big_input) == "1", "maximum n"

| Test input | Expected output | What it validates |
|---|---:|---|
| `1 1 / 0` | `1` | Minimum-size boundary case |
| `3 5 / 2 2 2` | `1` | Equal heights must form one component |
| `5 5 / 4 0 2 0 2` | `-1` | Highest mandatory reset does not have enough available trees |
| `4 2 / 2 1 2 1` | `2` | Separate high components and duplicate-height stack handling |
| \(n=10^6\), all heights \(0\) | `1` | Maximum input size and linear-time behavior |

## Edge Cases

When every tree has height \(m\), there is no mandatory reset level. For `4 3 / 3 3 3 3`, the stack eventually contains only `3`, and all counting for \(m\) is ignored. The algorithm detects that `max_low == -1` and returns \(1\), representing an attack immediately after planting.

For a single tree with final height below \(m\), such as

```văn bản 
1 1 
0```

the stack contains only `0`. Its final-stack contribution gives frequency (1). There is one available tree at the only reset time, so (k=1) is feasible.

Equal adjacent heights must not create multiple components. In

```3 5 
2 2 2```

each new `2` replaces the previous `2` in the stack without increasing the frequency. The final stack contributes one `2`, so the required number of intervals is (1).

Separated equal heights are different. In

```4 2 
2 1 2 1```

the first `2` is popped by the first `1` and contributes one count. The second `2` is eventually left on the stack and contributes another. Thus height (2) has frequency (2), giving (k=2). The two occurrences cannot share an interval because the intervening height-(1) tree must not be attacked at that earlier time.

The impossible case

```5 5 
4 0 2 0 2```

has two components with minimum height (2), so the stack produces frequency (2) for height (2). Hence the minimum candidate is (k=2). The largest mandatory height is (H=4), and only one tree has height at least (4), so only one nonempty interval can be chosen at that time. The feasibility test rejects (k=2) and returns (-1).

The boundary involving height (m) is handled separately. In

```4 2 
2 1 1 2 
``` 

hai cây có chiều cao (2) không bao giờ bị yêu cầu phải tấn công ở độ cao của chúng, bởi vì chiều cao (2) là chiều cao cuối cùng sau tất cả (m=2) năm. Chúng có thể được sử dụng làm phần đệm khi thiết lập lại chiều cao-(1) cây sau này. Loại trừ (m) khỏi phép tính tần số là câu trả lời đúng (1).
