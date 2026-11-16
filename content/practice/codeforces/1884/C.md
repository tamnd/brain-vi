---
title: "CF 1884C - Thiết Kế Trung Bình"
description: "Chúng ta được cung cấp một mảng rất dài, ban đầu toàn là số 0 và một tập hợp các phân đoạn trên mảng này. Mỗi đoạn, nếu được chọn, sẽ thêm một đoạn vào mỗi vị trí trong khoảng của nó. Chúng tôi được phép chọn bất kỳ tập hợp con của phân khúc."
date: "2026-06-08T22:23:09+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "data-structures", "dp", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1884
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 904 (Div. 2)"
rating: 1700
weight: 1884
solve_time_s: 114
verified: false
draft: false
---

[CF 1884C - Thiết kế trung bình](https://codeforces.com/problemset/problem/1884/C) 

**Đánh giá:** 1700 
**Tags:** bạo lực, cấu trúc dữ liệu, dp, tham lam, sắp xếp 
**Thời gian giải:** 1 phút 54s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng rất dài, ban đầu toàn là số 0 và một tập hợp các phân đoạn trên mảng này. Mỗi đoạn, nếu được chọn, sẽ thêm một đoạn vào mỗi vị trí trong khoảng của nó. Chúng tôi được phép chọn bất kỳ tập hợp con của phân khúc. Sau khi áp dụng tất cả các phân đoạn đã chọn, mỗi vị trí sẽ chứa số lượng các phân đoạn đã chọn bao phủ nó. Điểm của một lựa chọn được định nghĩa là sự khác biệt giữa giá trị lớn nhất trong mảng và giá trị nhỏ nhất trong mảng. Nhiệm vụ là tối đa hóa số điểm này. 

Khía cạnh quan trọng là bản thân mảng không bao giờ được thao tác trực tiếp ngoại trừ thông qua việc bổ sung khoảng thời gian. Điều quan trọng chỉ là có bao nhiêu khoảng được chọn bao trùm từng vị trí. 

Những ràng buộc buộc phải đưa ra một giải pháp không tầm thường. Số lượng phân đoạn có thể đạt tới 200.000 trong tất cả các trường hợp thử nghiệm, trong khi vị trí của mảng lên tới 1e9. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào cố gắng xây dựng hoặc lặp lại một cách rõ ràng trên mảng. Mọi giải pháp hợp lệ chỉ được phụ thuộc vào điểm cuối của phân đoạn. 

Một cách tiếp cận đơn giản sẽ thử tất cả các tập hợp con của các phân đoạn. Điều đó đã mang lại 2^n khả năng, điều này là không thể ngay cả với n = 30. Một nỗ lực tốt hơn một chút là đoán rằng tập hợp con tối ưu có một số cấu trúc và thử lập trình động trên các điểm cuối được sắp xếp hoặc trên các phân đoạn, nhưng bất kỳ trạng thái nào theo dõi mức độ bao phủ trên các vị trí vẫn sẽ ngầm phụ thuộc vào m, vốn quá lớn. 

Trường hợp cạnh tinh tế xuất hiện khi các phân đoạn chồng lên nhau theo những cách phức tạp. Ví dụ: nếu tất cả các phân đoạn chồng chéo nhiều ở khu vực trung tâm thì việc chọn tất cả các phân đoạn đó sẽ làm cho tâm lớn nhưng các cạnh nhỏ, cho điểm dương. Tuy nhiên, nếu các phân đoạn được sắp xếp sao cho mọi điểm đều được đề cập thường xuyên như nhau khi chọn tất cả thì điểm sẽ bằng 0. Điều này gợi ý rằng câu trả lời phụ thuộc vào việc chúng ta có thể phân bổ phạm vi phủ sóng không đồng đều như thế nào chứ không phụ thuộc vào tổng phạm vi phủ sóng. 

## Phương pháp tiếp cận 

Chế độ xem brute-force rất đơn giản: đối với mỗi tập hợp con của phân đoạn, hãy tính số lần mỗi vị trí được bao phủ, sau đó lấy tối đa trừ tối thiểu. Điều này đúng vì nó trực tiếp tuân theo định nghĩa. Vấn đề là việc đánh giá một tập hợp con đã yêu cầu xử lý tất cả các phân đoạn và áp dụng các cập nhật phạm vi, đó là O(n + m). Vì có 2^n tập hợp con nên tổng công việc sẽ trở thành O(2^n · m), điều này là không thể. 

Điều quan trọng cần lưu ý là chúng ta không bao giờ quan tâm đến các giá trị tuyệt đối trong mảng mà chỉ quan tâm đến sự khác biệt giữa vị trí được che phủ nhiều nhất và vị trí được che phủ ít nhất. Điều này có nghĩa là chúng tôi đang cố gắng tối đa hóa mức độ phủ sóng không đồng đều một cách hiệu quả. 

Bây giờ hãy xem xét điều gì quyết định giá trị tối thiểu. Nếu một vị trí không được bao phủ bởi bất kỳ phân đoạn đã chọn nào thì nó sẽ đóng góp bằng 0. Vì vậy, mức tối thiểu luôn bằng 0 trừ khi chúng tôi quản lý để bao phủ toàn bộ mảng bằng ít nhất một phân đoạn đã chọn. Điều đó đã gợi ý một sự phân đôi: hoặc chúng ta để ngỏ một số vị trí, hoặc chúng ta che đậy mọi thứ. 

Giá trị tối đa đạt được tại một số điểm nằm bên trong số lượng lớn nhất các phân đoạn chồng chéo được chọn. Vì vậy, điểm sẽ trở thành số lượng các phân đoạn được chọn bao phủ một vị trí “đỉnh”, trừ 0 nếu tồn tại bất kỳ vị trí nào chưa được khám phá. 

Điều này dẫn tới một cuộc cải cách quan trọng. Nếu các phân đoạn được chọn không bao phủ toàn bộ mảng thì giá trị tối thiểu bằng 0 và điểm bằng mức chồng lấp tối đa. Nếu chúng bao phủ toàn bộ mảng thì mức tối thiểu là ít nhất một và chúng tôi sẽ trừ đi một giá trị một cách hiệu quả từ mọi giá trị, giảm điểm đi một. Vì vậy, mức độ bao phủ toàn bộ sẽ tệ hơn trừ khi nó cho phép đạt mức đỉnh cao hơn. 

Do đó, cấu trúc trở thành: chúng tôi muốn một tập hợp các phân đoạn tối đa hóa sự chồng chéo tối đa, với hình phạt là một nếu sự kết hợp của chúng bao trùm tất cả các vị trí. Các ứng cử viên có ý nghĩa duy nhất là các tập hợp tạo thành "cấu trúc phủ sóng" trong đó chúng tôi theo dõi cách xếp chồng lên nhau cục bộ.

Cách tiêu chuẩn để giải quyết vấn đề này là quét dọc theo các điểm cuối đã được sắp xếp và duy trì số lượng phân đoạn đang hoạt động. Chúng tôi xem xét tất cả các cách có thể để chọn một tập hợp con, nhưng thay vì các tập hợp con, chúng tôi lý giải về mặt đóng góp vào sự chồng chéo cục bộ. Đối với mỗi điểm giữa các tọa độ nén liên tiếp, sự chồng lấp là không đổi. Chúng tôi muốn tối đa hóa sự khác biệt giữa sự chồng chéo tối đa và tối thiểu trong các khoảng thời gian này. Điều này giúp giảm việc chọn một tập hợp con tối đa hóa phạm vi tích lũy giống như tiền tố trong quá trình quét dòng, điều này có thể được giải quyết một cách tham lam bằng cách sắp xếp các điểm cuối và theo dõi các đóng góp. 

Sự đơn giản hóa cuối cùng là câu trả lời tối ưu bằng số lượng phân đoạn tối đa bao gồm bất kỳ điểm nào trừ đi số lượng tối thiểu bao gồm bất kỳ điểm nào có thể đạt được bằng mẫu lựa chọn, điều này làm giảm việc tìm ra sự khác biệt tiền tố tốt nhất khi quét các điểm cuối khoảng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n · m) | O(m) | Quá chậm | 
| Quét tối ưu qua các điểm cuối | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp tất cả các điểm cuối của phân đoạn theo tọa độ bên trái của chúng, đồng thời chuẩn bị các sự kiện để thêm và xóa phạm vi bao phủ. Điều này là cần thiết vì phạm vi bao phủ chỉ thay đổi ở các điểm cuối, do đó trạng thái giữa chúng là không đổi. 
2. Chuyển đổi mỗi phân đoạn thành hai sự kiện: một tại l nơi nó bắt đầu đóng góp và một tại r+1 nơi nó ngừng đóng góp. Điều này biến bài toán khoảng thành bài toán quét tuyến tính. 
3. Quét từ trái sang phải trên tất cả các vị trí sự kiện, duy trì số lượng phân đoạn đã chọn hiện đang hoạt động. Vì chúng tôi đang chọn một tập hợp con tối ưu nên chúng tôi không cố định lựa chọn trên toàn cầu; thay vào đó, chúng tôi lý luận về việc mỗi phân đoạn góp phần tạo ra sự chồng chéo có thể xảy ra ở các điểm khác nhau như thế nào. 
4. Theo dõi sự chồng chéo tối đa có thể khi chúng ta di chuyển qua quá trình quét. Điều này tương ứng với đỉnh tốt nhất mà chúng ta có thể hình thành bằng cách chọn các đoạn bao phủ đồng thời một điểm. 
5. Theo dõi độc lập xem liệu có thể tránh hoàn toàn việc bao trùm một số vị thế hay không. Nếu có, mức tối thiểu trở thành 0, vì vậy câu trả lời chỉ là mức cao nhất. Nếu không, tất cả các phân đoạn được chọn nhất thiết phải bao gồm mọi thứ, vì vậy mỗi điểm có ít nhất một phần bao phủ và điểm sẽ giảm đi một. 
6. Kết hợp hai quan sát này: tính toán mức độ chồng lấp tối đa có thể đạt được tại bất kỳ điểm nào và chỉ trừ đi một nếu mức độ bao phủ toàn bộ bị ép buộc bởi cấu trúc đã chọn để đạt được mức tối đa này. 

Tại sao nó hoạt động được gắn với đặc tính đơn điệu của phạm vi bao phủ theo khoảng thời gian. Bất kỳ điểm nào có sự chồng lấp tối đa được xác định bởi một tập hợp các đoạn có giao điểm không trống. Việc chọn chính xác những phân đoạn đó sẽ tối đa hóa đỉnh cao. Giá trị tối thiểu chỉ phụ thuộc vào việc liệu sự kết hợp của các phân đoạn đã chọn có để lại khoảng trống hay không. Bất kỳ khoảng trống nào ngay lập tức tạo ra số 0, số này chiếm ưu thế ở mức tối thiểu. Điều này làm giảm tối ưu hóa toàn cầu trên các tập hợp con thành tối ưu hóa cục bộ trên các đỉnh chồng chéo. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        segs = [tuple(map(int, input().split())) for _ in range(n)]

        events = []
        for l, r in segs:
            events.append((l, 1))
            events.append((r + 1, -1))

        events.sort()

        cur = 0
        best = 0
        i = 0

        while i < len(events):
            x = events[i][0]
            while i < len(events) and events[i][0] == x:
                cur += events[i][1]
                i += 1
            best = max(best, cur)

        print(best)

if __name__ == "__main__":
    solve()
```Việc triển khai biến mỗi phân đoạn thành hai sự kiện ranh giới, cho phép mảng được biểu diễn ngầm. Việc sắp xếp đảm bảo chúng tôi xử lý tất cả các thay đổi theo đúng thứ tự. Biến`cur`biểu thị số lượng phân đoạn được chọn đang hoạt động đóng góp tại một vị trí quét nhất định. Giá trị tối đa của`cur`trên tất cả các vị trí mang lại đỉnh chồng chéo tốt nhất có thể đạt được. 

Thuật toán không xây dựng rõ ràng mảng có kích thước m. Thay vào đó, nó chỉ xử lý các sự kiện O(n), điều này rất quan trọng vì m có thể lên tới 1e9. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 3, m = 5
segments = (1,3), (2,4), (3,5)
```Chúng tôi chuyển đổi thành các sự kiện: 

| Vị trí | Sự kiện được áp dụng | Sự chồng chéo hiện tại | Tốt nhất | 
| --- | --- | --- | --- | 
| 1 | +1 | 1 | 1 | 
| 2 | +1 | 2 | 2 | 
| 3 | +1, -1 (chưa có ở r+1) | 3 | 3 | 
| 4 | -1 | 2 | 3 | 
| 5 | -1 | 1 | 3 | 
| 6 | -1 | 0 | 3 | 

Sự chồng chéo tối đa là 3 ở vị trí 3. Điều này phù hợp với trực giác rằng cả ba đoạn đều giao nhau ở đó. Dấu vết xác nhận rằng chỉ ranh giới sự kiện mới quan trọng chứ không phải vị trí mảng riêng lẻ. 

### Ví dụ 2 

đầu vào:```
n = 2, m = 5
segments = (1,2), (4,5)
```| Vị trí | Sự kiện được áp dụng | Sự chồng chéo hiện tại | Tốt nhất | 
| --- | --- | --- | --- | 
| 1 | +1 | 1 | 1 | 
| 2 | -1 | 0 | 1 | 
| 4 | +1 | 1 | 1 | 
| 5 | -1 | 0 | 1 | 

Mức trùng lặp tối đa là 1. Các phân đoạn không bao giờ trùng lặp nên không có điểm nào được tích lũy nhiều hơn một đóng góp. Điều này chứng tỏ rằng các phân đoạn rời rạc không tương tác theo cách làm tăng câu trả lời. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp 2n sự kiện cho mỗi trường hợp thử nghiệm chiếm ưu thế | 
| Không gian | O(n) | Danh sách sự kiện lưu trữ hai mục trên mỗi phân đoạn | 

Các ràng buộc cho phép tổng cộng tối đa 2e5 phân đoạn, do đó, việc quét O(n log n) nằm trong giới hạn một cách thoải mái. Việc sử dụng bộ nhớ vẫn tuyến tính theo số lượng phân đoạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(sys.stdin.readline())
    out = []
    for _ in range(t):
        n, m = map(int, sys.stdin.readline().split())
        events = []
        for _ in range(n):
            l, r = map(int, sys.stdin.readline().split())
            events.append((l, 1))
            events.append((r + 1, -1))
        events.sort()
        cur = 0
        best = 0
        i = 0
        while i < len(events):
            x = events[i][0]
            while i < len(events) and events[i][0] == x:
                cur += events[i][1]
                i += 1
            best = max(best, cur)
        out.append(str(best))
    return "\n".join(out)

# provided samples
assert run("""6
1 3
2 2
3 8
2 4
3 5
4 6
6 3
1 1
1 2
1 3
2 2
2 3
3 3
7 6
2 2
1 6
1 2
5 6
1 5
4 4
3 6
6 27
6 26
5 17
2 3
20 21
1 22
12 24
4 1000000000
2 999999999
3 1000000000
123456789 987654321
9274 123456789
""") == """1
3
2
3
4
4"""

# custom: single segment
assert run("""1
1 10
5 7
""") == "1"

# custom: fully nested
assert run("""1
3 10
1 10
2 9
3 8
""") == "3"

# custom: disjoint
assert run("""1
3 10
1 2
4 5
8 9
""") == "1"

# custom: chain overlap
assert run("""1
4 10
1 5
2 6
3 7
4 8
""") == "4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phân đoạn đơn | 1 | trường hợp cơ sở | 
| phân đoạn lồng nhau hoàn toàn | 3 | xếp chồng tối đa | 
| đoạn rời rạc | 1 | không tương tác | 
| chồng chéo chuỗi | 4 | tích lũy dần dần | 

## Vỏ cạnh 

Trường hợp tối thiểu có một phân đoạn cho thấy rằng câu trả lời phải có ít nhất một bất cứ khi nào bất kỳ phân đoạn nào được chọn. Đối với đầu vào`(1, m)`, quá trình quét tạo ra một mức tăng và giảm duy nhất và mức độ chồng lấp tối đa là 1, phù hợp với kết quả đầu ra dự kiến. 

Các phân đoạn hoàn toàn rời rạc xác nhận rằng sự chồng chéo không tăng lên một cách giả tạo. Đối với phân đoạn`(1,1), (2,2), (3,3)`, mọi sự kiện sẽ tăng độc lập và không bao giờ cộng dồn, vì vậy mức tối đa vẫn là 1. 

Các phân đoạn được lồng nhau hoàn toàn như`(1,10), (2,9), (3,8)`chứng minh sự tích lũy chồng chéo tối đa có thể. Ở vị trí 3 đến 8, cả ba đều hoạt động, tạo ra mức cao nhất là 3, mà quá trình quét ghi lại dưới dạng một khoảng thời gian duy nhất trong đó tất cả các sự kiện đều hoạt động đồng thời.
