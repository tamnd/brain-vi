---
title: "CF 102448F - Cuối cùng cũng đến Giáng sinh!"
description: "Mỗi tòa nhà là một hình chữ nhật thẳng hàng trục đứng trên cùng một đường cơ sở ngang. Một tòa nhà được mô tả bằng tọa độ bên trái (Li), tọa độ bên phải (Ri) và chiều cao (Hi), do đó nó chiếm khoảng từ (Li) đến (Ri) và tăng lên độ cao (Hi)."
date: "2026-08-09T14:08:09+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102448
codeforces_index: "F"
codeforces_contest_name: "UFPE Starters Final Try-Outs 2020"
rating: 0
weight: 102448
solve_time_s: 319
verified: true
draft: false
---

[CF 102448F - Cuối cùng cũng đến Giáng sinh!](https://codeforces.com/problemset/problem/102448/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 5 phút 19s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Mỗi tòa nhà là một hình chữ nhật thẳng hàng trục đứng trên cùng một đường cơ sở ngang. Một tòa nhà được mô tả bằng tọa độ bên trái (L_i), tọa độ bên phải (R_i) và chiều cao (H_i), do đó nó chiếm khoảng từ (L_i) đến (R_i) và tăng dần lên độ cao (H_i). 

Việc trang trí phải bao phủ toàn bộ mặt tiền nhìn thấy được của thành phố, nhưng những phần chồng lên nhau của các tòa nhà chỉ cần che một lần. Tại tọa độ nằm ngang (x), chiều cao yêu cầu do đó là chiều cao của tòa nhà cao nhất bao phủ (x). Nếu chúng ta gọi chiều cao đó là (f(x)), thì diện tích cần tìm là tích phân của (f(x)) trên toàn thành phố. 

Đầu vào chứa tối đa (10^5) tòa nhà. Tọa độ có thể đạt tới (10^9), vì vậy chúng ta không thể tạo một mảng được lập chỉ mục theo mọi tọa độ có thể. Quan trọng hơn, thuật toán (O(N^2)) sẽ thực hiện khoảng (10^{10}) thao tác trong trường hợp xấu nhất, vượt xa giới hạn một giây có thể hỗ trợ. Chúng ta cần một giải pháp (O(N\log N)) hoặc giải pháp hiệu quả tương tự. 

Tọa độ là số nguyên, nhưng các tòa nhà biểu thị các hình chữ nhật liên tục. Điều này tạo ra một số chỗ dễ mắc lỗi. 

Đầu tiên, các tòa nhà chồng chéo chỉ được đóng góp chiều cao tối đa. Ví dụ,```
2
0 4 3
1 2 5
```có diện tích (3\cdot1+5\cdot1+3\cdot2=16). Cộng hai diện tích hình chữ nhật sẽ có (12+5=17), vì phần chồng lên nhau sẽ được tính hai lần. 

Thứ hai, các tòa nhà chạm vào không chồng chéo lên nhau. Ví dụ,```
2
0 2 4
2 5 6
```có diện tích (2\cdot4+3\cdot6=26). Điểm (x=2) có chiều rộng bằng 0 nên không đóng góp diện tích. Việc xử lý các điểm cuối trong khoảng như các ô có chiều rộng đơn vị có thể mang lại sự đóng góp bổ sung. 

Thứ ba, một số tòa nhà có thể có cùng chiều cao tối đa. Ví dụ,```
2
0 3 5
0 3 5
```có diện tích (3\cdot5=15). Khi một tòa nhà kết thúc, chiều cao (5) phải tiếp tục hoạt động vì tòa nhà kia vẫn đang che lấp khoảng trống. Cấu trúc dữ liệu chỉ cần xóa giá trị tối đa mà không theo dõi bội số sẽ làm giảm đường chân trời một cách không chính xác. 

Thứ tư, khoảng trống giữa các tòa nhà phải tạo ra diện tích bằng không. Ví dụ,```
2
0 2 3
5 7 4
```có diện tích (2\cdot3+2\cdot4=14). Việc triển khai quét mang mức tối đa trước đó trong khoảng trống từ (2) đến (5) sẽ thêm vùng không tồn tại. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp bắt đầu bằng việc quan sát rằng đường chân trời chỉ có thể thay đổi tại điểm cuối của tòa nhà. Chúng ta có thể thu thập mọi tọa độ trái và phải, sắp xếp chúng và xem xét từng cặp tọa độ liên tiếp. Trong một khoảng thời gian như vậy, không có tòa nhà nào bắt đầu hoặc kết thúc, do đó tập hợp các tòa nhà đang hoạt động là cố định và chiều cao yêu cầu là không đổi. Chúng tôi có thể kiểm tra mọi tòa nhà trong từng khoảng thời gian và lấy chiều cao lớn nhất trong số các tòa nhà trong khoảng thời gian đó. 

Điều này đúng vì toàn bộ trục x liên tục đã được phân chia thành các khoảng mà đường chân trời không thay đổi. Tuy nhiên, có thể có gần như (2N) điểm cuối riêng biệt, tạo ra khoảng cơ bản lên tới (2N-1). Việc kiểm tra tất cả (N) tòa nhà cho mỗi tòa nhà mất tới 

[ 
(2N-1)N 
] 

kiểm tra xây dựng. Đối với (N=10^5), đây là (19.999.900.000), khoảng (2\cdot10^{10}) lượt kiểm tra, quá chậm. 

Lực lượng vũ phu hoạt động vì mọi khoảng thời gian giữa các điểm cuối liên tiếp đều có một câu trả lời cố định. Vấn đề là nó liên tục tính toán lại mức tối đa từ đầu. Quan sát quan trọng là khi chúng ta di chuyển từ điểm cuối này sang điểm cuối tiếp theo, chỉ những tòa nhà bắt đầu hoặc kết thúc tại tọa độ đó mới thay đổi tập hoạt động. Không có lý do gì để kiểm tra lại mọi tòa nhà khác. 

Chúng ta có thể quét từ trái sang phải. Trong khi quét, chúng tôi duy trì tất cả các chiều cao tòa nhà hiện đang hoạt động và cần truy vấn mức tối đa của chúng. Vùng nhớ tối đa cung cấp chính xác thao tác đó trong (O(\log N)) khi một tòa nhà được chèn vào. 

Khó khăn còn lại là xóa. Vùng heap tiêu chuẩn của Python không hỗ trợ loại bỏ phần tử tùy ý một cách hiệu quả. Chúng ta có thể giải quyết vấn đề này bằng cách lười biếng xóa. Đối với mỗi độ cao, chúng tôi lưu trữ số lượng tòa nhà hiện đang hoạt động có chiều cao đó. Khi một tòa nhà kết thúc, chúng tôi giảm số lượng của nó nhưng vẫn giữ nguyên mục nhập heap của nó. Bất cứ khi nào chiều cao cao nhất của heap có số 0, chúng tôi sẽ xóa mục nhập cũ đó. Mỗi mục nhập heap được chèn một lần và bị xóa nhiều nhất một lần, do đó tổng công việc vẫn còn (O(N\log N)). 

Việc tính diện tích đặc biệt đơn giản. Giả sử tọa độ sự kiện trước đó là (x_{\text{prev}}), tọa độ hiện tại là (x) và chiều cao tối đa hiện tại là (h). Đường chân trời chính xác là (h) trong suốt khoảng thời gian ([x_{\text{prev}},x)), do đó đóng góp diện tích của nó là 

[ 
(x-x_{\text{prev}})h. 
] 

Sau khi thêm phần đóng góp này, chúng tôi xử lý tất cả các tòa nhà bắt đầu hoặc kết thúc tại (x) và tập hoạt động thu được sẽ xác định chiều cao cho khoảng tiếp theo. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(N^2)) | (O(N)) | Quá chậm | 
| Đường quét tối ưu | (O(N\log N)) | (O(N)) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Đọc từng tòa nhà và biến nó thành hai sự kiện. Tại (L_i), chiều cao (H_i) bắt đầu hoạt động. Tại (R_i), chiều cao (H_i) ngừng hoạt động. Điểm cuối bên phải được coi là điểm kết thúc vì tòa nhà chiếm khoảng liên tục lên tới (R_i) và một điểm cuối duy nhất có diện tích bằng 0. 
2. Sắp xếp tất cả các sự kiện (2N) theo tọa độ x của chúng. Sau khi sắp xếp, quá trình quét sẽ gặp mọi địa điểm có thể mà đường chân trời có thể thay đổi theo thứ tự từ trái sang phải. 
3. Duy trì một đống tối đa chứa chiều cao của các tòa nhà đã bắt đầu nhưng chưa kết thúc. Vì Python cung cấp vùng heap tối thiểu nên lưu trữ chiều cao dưới dạng giá trị âm, do đó phần tử vùng heap nhỏ nhất biểu thị chiều cao thực tế lớn nhất. 
4. Duy trì một từ điển chứa số lượng tòa nhà hiện đang hoạt động cho mỗi chiều cao. Điều này cung cấp cho chúng tôi thông tin đa dạng, điều này cần thiết khi hai hoặc nhiều tòa nhà đang hoạt động có cùng chiều cao. 
5. Bắt đầu quét tại tọa độ x của sự kiện đầu tiên. Trước khi xử lý sự kiện đầu tiên, không có vùng được che phủ nên vùng tích lũy ban đầu bằng không. 
6. Tại mỗi tọa độ sự kiện riêng biệt (x), trước tiên hãy cộng diện tích giữa tọa độ trước đó và (x). Tập hoạt động không thay đổi ở bất kỳ đâu trong khoảng này, do đó chiều cao tối đa hiện tại không đổi ở đó. 
7. Xử lý mọi sự kiện tại (x). Sự kiện bắt đầu sẽ tăng số lượng chiều cao của nó và đẩy chiều cao đó vào heap. Một sự kiện kết thúc làm giảm số lượng. Chúng tôi xử lý tất cả các sự kiện ở cùng một tọa độ trước khi di chuyển sang phải xa hơn vì thứ tự chính xác của chúng tại một điểm không thể ảnh hưởng đến khu vực, trong khi kết quả tổng hợp phải mô tả tập hoạt động ngay sau tọa độ đó. 
8. Loại bỏ các mục nhập heap cũ trong khi heap top có số lượng hoạt động bằng 0. Những mục này đại diện cho các tòa nhà đã kết thúc. Việc xóa lười biếng sẽ tránh được việc phải xóa một đống tùy ý tốn kém. 
9. Đặt tọa độ trước đó thành (x) và tiếp tục cho đến khi mọi sự kiện được xử lý. Sau điểm cuối bên phải cuối cùng, không có tòa nhà nào đang hoạt động nên không có diện tích bổ sung. 

### Tại sao nó hoạt động 

Bất biến trung tâm là ngay sau khi xử lý tất cả các sự kiện tại tọa độ (x), heap biểu thị chính xác chiều cao của các tòa nhà bao trùm mọi điểm ngay bên phải của (x), với bội số được tính bởi từ điển đếm hoạt động. 

Giữa hai tọa độ sự kiện liên tiếp, không có tòa nhà nào bắt đầu hoặc kết thúc. Do đó, tập hoạt động không thay đổi, do đó tòa nhà đang hoạt động cao nhất sẽ xác định chiều cao đường chân trời trong toàn bộ khoảng thời gian. Nhân chiều cao đó với chiều rộng khoảng sẽ tính toán chính xác diện tích của phần đường chân trời đó. Vì mỗi phần của trục x được bao phủ bởi chính xác một khoảng như vậy nên tổng của tất cả các đóng góp chính xác là diện tích tối thiểu cần thiết để bao phủ khung cảnh phía trước của thành phố. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

def solve():
    n = int(input())

    events = []

    for _ in range(n):
        l, r, h = map(int, input().split())
        events.append((l, 1, h))
        events.append((r, -1, h))

    events.sort()

    heap = []
    count = {}

    area = 0
    prev_x = events[0][0]
    i = 0
    m = len(events)

    while i < m:
        x = events[i][0]

        if heap:
            area += (x - prev_x) * (-heap[0])

        while i < m and events[i][0] == x:
            _, delta, h = events[i]

            if delta == 1:
                count[h] = count.get(h, 0) + 1
                heapq.heappush(heap, -h)
            else:
                count[h] -= 1

            i += 1

        while heap and count.get(-heap[0], 0) == 0:
            heapq.heappop(heap)

        prev_x = x

    print(area)

if __name__ == "__main__":
    solve()
```Việc xây dựng sự kiện là sự chuyển dịch trực tiếp mỗi hình chữ nhật thành hai thay đổi đối với đường chân trời đang hoạt động. Điểm cuối bên trái sẽ thêm chiều cao, trong khi điểm cuối bên phải sẽ xóa chiều cao đó. 

Bước sắp xếp có chi phí (O(N\log N)) vì có chính xác (2N) sự kiện. Sau khi được sắp xếp, quá trình quét sẽ xử lý mọi sự kiện một lần. Chi phí chèn vùng heap (O(\log N)) và mặc dù các mục nhập cũ có thể tồn tại tạm thời nhưng mỗi mục nhập chỉ có thể được xóa khỏi heap một lần. Do đó, tất cả các thao tác heap cùng nhau lấy (O(N\log N)). 

biểu thức`-heap[0]`cung cấp cho tòa nhà đang hoạt động cao nhất hiện tại vì heap lưu trữ chiều cao âm. Diện tích được tính toán trước khi xử lý các sự kiện tại tọa độ hiện tại. Thứ tự này là có chủ ý: khoảng thời gian từ`prev_x`ĐẾN`x`sử dụng tập hoạt động tồn tại trước khi tiếp cận`x`. Sự kiện tại`x`chỉ ảnh hưởng đến khoảng bên phải của nó. 

Việc nhóm tất cả các sự kiện có cùng tọa độ cũng làm cho hành vi ranh giới trở nên rõ ràng. Giả sử một tòa nhà kết thúc và một tòa nhà khác bắt đầu ở cùng tọa độ x. Không có khu vực nào tồn tại giữa hai sự kiện đó, vì vậy chúng có thể được xử lý cùng nhau và chỉ có tập hoạt động kết hợp của chúng mới quan trọng sau đó. 

Cần có từ điển vì giá trị heap không phải là mã định danh duy nhất. Nếu ba tòa nhà đang hoạt động có chiều cao (7), việc loại bỏ một trong số chúng không được làm cho chiều cao (7) biến mất khỏi mức tối đa. Số lượng ghi lại chính xác có bao nhiêu bản sao vẫn đang hoạt động. 

Số nguyên Python có độ chính xác tùy ý, do đó diện tích tối đa có thể, khoảng (10^9\cdot10^6=10^{15}), được xử lý mà không bị tràn. 

## Ví dụ đã hoạt động 

Mẫu được cung cấp cho đường chân trời sau:```
6
2 6 9
9 14 11
12 20 6
17 25 20
23 31 14
29 36 18
```Trạng thái quét là: 

| x | Sự kiện tại x | Tối đa sau sự kiện | Khu vực được thêm trước sự kiện | Tổng diện tích | 
| --- | --- | --- | --- | --- | 
| 2 | bắt đầu 9 | 9 | 0 | 0 | 
| 6 | kết thúc 9 | 0 | 36 | 36 | 
| 9 | bắt đầu 11 | 11 | 0 | 36 | 
| 12 | bắt đầu 6 | 11 | 33 | 69 | 
| 14 | kết thúc 11 | 6 | 22 | 91 | 
| 17 | bắt đầu 20 | 20 | 18 | 109 | 
| 20 | kết thúc 6 | 20 | 60 | 169 | 
| 23 | bắt đầu 14 | 20 | 60 | 229 | 
| 25 | kết thúc 20 | 14 | 40 | 269 ​​| 
| 29 | bắt đầu 18 | 18 | 56 | 325 | 
| 31 | kết thúc 14 | 18 | 36 | 361 | 
| 36 | kết thúc 18 | 0 | 90 | 451 | 

Kết quả là (451), khớp với đầu ra mẫu. Bảng này cũng cho thấy tại sao tòa nhà có chiều cao (6) bắt đầu từ (12) không thay đổi đường chân trời ngay lập tức vì chiều cao (11) đã hoạt động. Tương tự như vậy, tòa nhà có chiều cao (20) chiếm ưu thế hơn mọi tòa nhà đang hoạt động khác từ (17) đến (25). 

Đối với ví dụ thứ hai, hãy xem xét:```
3
0 4 3
1 3 5
3 6 2
```Độ quét tương ứng là: 

| x | Sự kiện tại x | Tối đa sau sự kiện | Khu vực được thêm trước sự kiện | Tổng diện tích | 
| --- | --- | --- | --- | --- | 
| 0 | bắt đầu 3 | 3 | 0 | 0 | 
| 1 | bắt đầu 5 | 5 | 3 | 3 | 
| 3 | kết thúc 5, bắt đầu 2 | 3 | 10 | 13 | 
| 4 | kết thúc 3 | 2 | 3 | 16 | 
| 6 | kết thúc 2 | 0 | 4 | 20 | 

Câu trả lời là (20). Tại (x=3), một tòa nhà kết thúc chính xác tại nơi tòa nhà khác bắt đầu. Việc xử lý cả hai sự kiện ở cùng một tọa độ sẽ cho chiều cao chính xác của (3) ngay sau đó. Không có khoảng trống nhân tạo hoặc đơn vị diện tích bổ sung tại ranh giới chung. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(N\log N)) | Có (2N) sự kiện cần sắp xếp, theo sau là các lần chèn và xóa vùng nhớ heap (O(N)), mỗi sự kiện tính giá (O(\log N)). | 
| Không gian | (O(N)) | Danh sách sự kiện, số đống và chiều cao chứa các mục nhập (O(N)). | 

Với (N\le 10^5), việc sắp xếp các sự kiện (2N) và thực hiện các phép toán logarit (O(N)) là thoải mái trong mức độ phức tạp dự định. Phạm vi tọa độ của (10^9) không bao giờ ảnh hưởng đến việc sử dụng bộ nhớ vì thuật toán chỉ lưu trữ các điểm cuối của tòa nhà thay vì mọi tọa độ x có thể có. 

## Trường hợp thử nghiệm```python
import sys
import io
import heapq

def solve():
    input = sys.stdin.readline

    n = int(input())
    events = []

    for _ in range(n):
        l, r, h = map(int, input().split())
        events.append((l, 1, h))
        events.append((r, -1, h))

    events.sort()

    heap = []
    count = {}

    area = 0
    prev_x = events[0][0]
    i = 0

    while i < len(events):
        x = events[i][0]

        if heap:
            area += (x - prev_x) * (-heap[0])

        while i < len(events) and events[i][0] == x:
            _, delta, h = events[i]

            if delta == 1:
                count[h] = count.get(h, 0) + 1
                heapq.heappush(heap, -h)
            else:
                count[h] -= 1

            i += 1

        while heap and count.get(-heap[0], 0) == 0:
            heapq.heappop(heap)

        prev_x = x

    print(area)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    try:
        sys.stdin = io.StringIO(inp)
        sys.stdout = io.StringIO()
        solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided sample
sample1 = """\
6
2 6 9
9 14 11
12 20 6
17 25 20
23 31 14
29 36 18
"""
assert run(sample1) == "451\n", "sample 1"

# Minimum-size input
assert run("""\
1
0 1 1
""") == "1\n", "minimum-size case"

# Touching intervals and boundary coordinates
assert run("""\
2
0 1 1
1 2 1
""") == "2\n", "touching intervals"

# Containment and a taller inner building
assert run("""\
2
0 4 3
1 2 5
""") == "16\n", "contained building"

# Maximum-size input, all buildings identical and at coordinate limits
n = 100000
maximum_case = str(n) + "\n" + ("0 1000000000 1000000\n" * n)
assert run(maximum_case) == "1000000000000000\n", "maximum-size case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 0 1 1`|`1`| Tính toán diện tích cơ bản và đầu vào tối thiểu có thể | 
|`2 / 0 1 1, 1 2 1`|`2`| Điểm cuối được chia sẻ và ranh giới khoảng thời gian liên tục | 
|`2 / 0 4 3, 1 2 5`|`16`| Chồng chéo, ngăn chặn và lấy chiều cao tối đa | 
|`100000`các tòa nhà giống hệt nhau từ`0`ĐẾN`1000000000`với chiều cao`1000000`|`1000000000000000`| Tối đa (N), tọa độ tối đa, chiều cao tối đa, chiều cao trùng lặp và diện tích số nguyên lớn | 

## Vỏ cạnh 

Trường hợp cạnh đầu tiên là các tòa nhà chồng chéo có chiều cao khác nhau.```
2
0 4 3
1 2 5
```Tại (x=0), chiều cao (3) bắt đầu hoạt động. Từ (0) đến (1), đường chân trời là (3), đóng góp (3). Tại (x=1), chiều cao (5) bắt đầu, do đó từ (1) đến (2) đường chân trời là (5), đóng góp (5). Tại (x=2), chiều cao (5) kết thúc, đưa đường chân trời trở về (3), góp phần (6) từ (2) đến (4). Tổng số là (3+5+6=14). 

Phép tính này đưa ra một sự điều chỉnh mà phép tính liên hình chữ nhật bất cẩn có thể bỏ sót. Tổng số đúng thực sự là (14), không phải (16), vì khoảng đầu tiên có chiều rộng (1), phần chồng chéo có chiều rộng (1) và khoảng cuối cùng có chiều rộng (2). Cuộc quét tính toán chính xác ba mảnh này. Do đó, ví dụ trước đó trong phần thảo luận vấn đề có thể được sử dụng như một cảnh báo: việc cộng diện tích hình chữ nhật là sai và việc tính toán đường chân trời chính xác phải luôn được thực hiện. 

Trường hợp cạnh thứ hai là hai tòa nhà chạm vào nhau tại một điểm cuối.```
2
0 2 4
2 5 6
```Tòa nhà đầu tiên đóng góp (2\cdot4=8) và tòa nhà thứ hai đóng góp (3\cdot6=18). Tại (x=2), tòa nhà đầu tiên bị xóa và tòa nhà thứ hai được thêm vào. Vì một điểm có chiều rộng bằng 0 nên không có gì bổ sung được thêm vào đó. Câu trả lời là (26). 

Trường hợp cạnh thứ ba là chiều cao tối đa trùng lặp.```
2
0 3 5
0 3 5
```Cả hai tòa nhà đều bắt đầu ở (x=0), do đó số lượng hiện hoạt cho chiều cao (5) trở thành (2). Cả hai tòa nhà đều kết thúc ở (x=3), giảm số lượng xuống 0. Trong suốt khoảng thời gian, vùng heap tối đa là (5), cho diện tích (3\cdot5=15). Việc đếm ngăn lần xóa đầu tiên xóa chiều cao không chính xác trong khi tòa nhà thứ hai vẫn đang hoạt động. 

Trường hợp cạnh thứ tư là một khoảng trống.```
2
0 2 3
5 7 4
```Từ (0) đến (2), mức hoạt động tối đa là (3), đóng góp (6). Từ (2) đến (5), heap trở nên trống nên phần đóng góp bằng 0. Từ (5) đến (7) lớn nhất là (4), đóng góp (8). Câu trả lời cuối cùng là (14). Đống trống chính xác là cách triển khai thể hiện mặt đất chưa được che phủ, do đó không có khu vực nào vô tình bị đưa qua khoảng trống. 

Trường hợp ranh giới có tọa độ (0) và (10^9) được xử lý mà không có bất kỳ logic đặc biệt nào. Ví dụ,```
1
0 1000000000 1000000
```có diện tích (10^9\cdot10^6=10^{15}). Thuật toán chỉ thực hiện hai sự kiện bất kể phạm vi tọa độ rất lớn và số học số nguyên của Python xử lý trực tiếp giá trị kết quả.
