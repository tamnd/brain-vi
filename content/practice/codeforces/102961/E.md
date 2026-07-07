---
title: "CF 102961E - Khách hàng nhà hàng"
description: "Chúng ta được cung cấp một lịch trình thời gian về những lần khách hàng ghé thăm một nhà hàng, tại đó mỗi khách hàng sẽ xuất hiện vào một thời điểm nào đó và rời đi vào một thời điểm nào đó sau đó. Mỗi khách hàng đóng góp một khoảng thời gian liên tục trong thời gian họ có mặt bên trong nhà hàng."
date: "2026-07-04T06:50:47+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102961
codeforces_index: "E"
codeforces_contest_name: "CSES Problem Set: Sorting and Searching"
rating: 0
weight: 102961
solve_time_s: 46
verified: true
draft: false
---

[CF 102961E - Khách hàng nhà hàng](https://codeforces.com/problemset/problem/102961/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một lịch trình thời gian về những lần khách hàng ghé thăm một nhà hàng, tại đó mỗi khách hàng sẽ xuất hiện vào một thời điểm nào đó và rời đi vào một thời điểm nào đó sau đó. Mỗi khách hàng đóng góp một khoảng thời gian liên tục trong thời gian họ có mặt bên trong nhà hàng. 

Nhiệm vụ là xác định số lượng khách hàng tối đa có mặt đồng thời bên trong vào bất kỳ thời điểm nào. 

Một cách hữu ích để điều chỉnh lại đầu vào là mỗi khách hàng trở thành một phân khúc trên trục số. Sau đó, vấn đề sẽ yêu cầu độ sâu chồng chéo tối đa giữa tất cả các phân đoạn. 

Các ràng buộc (thường lên tới khoảng 2×10^5 khoảng trong họ bài toán này) ngụ ý rằng phương pháp O(n²) ngay lập tức không khả thi. Bất kỳ giải pháp nào so sánh mọi khoảng thời gian với mọi khoảng thời gian khác sẽ thực hiện theo thứ tự 10^10 thao tác trong trường hợp xấu nhất, vượt xa giới hạn thời gian thông thường. Điều này thúc đẩy chúng ta hướng tới các phương pháp O(n log n) hoặc O(n), là tiêu chuẩn cho các bài toán đếm theo đường quét hoặc dựa trên sắp xếp. 

Một trường hợp phức tạp xuất phát từ cách xử lý các sự kiện đồng thời. Hãy xem xét hai khách hàng: một khách hàng rời đi lúc 5 và một khách hàng khác đến lúc 5. Nếu chúng ta coi cả hai là chồng chéo không chính xác, chúng ta có thể tính quá mức. Ví dụ: 

đầu vào:```
1
1 5
5 10
```Tùy theo cách hiểu, câu trả lời đúng thường là 1, vì khách hàng đầu tiên không còn ở bên trong khi khách hàng thứ hai đến. Một cách tiếp cận đơn giản tăng dần trước khi xử lý các chuyến khởi hành ở cùng một dấu thời gian sẽ xuất ra không chính xác 2. 

Sự mơ hồ này buộc chúng ta phải xác định cẩn thận thứ tự ở những thời điểm bằng nhau, thay vì chỉ phân loại theo thời gian. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ mô phỏng từng thời điểm mà những thay đổi xảy ra. Đối với mỗi khoảng, người ta có thể lặp qua tất cả các khoảng khác và đếm xem có bao nhiêu khoảng trùng lặp với nó. Điều này có tác dụng vì việc kiểm tra chồng chéo rất đơn giản nhưng nó lặp lại những so sánh giống nhau nhiều lần. 

Đối với n khoảng thời gian, điều này dẫn đến khoảng n lần kiểm tra trên mỗi khoảng thời gian, tạo ra tổng số thao tác là O(n2). Với n khoảng 200.000, điều này hoàn toàn không thể sử dụng được. 

Quan sát quan trọng là chúng ta không thực sự quan tâm đến mối quan hệ giữa các cặp khoảng tùy ý. Chúng tôi chỉ quan tâm đến số lượng khoảng thời gian hoạt động thay đổi như thế nào theo thời gian. Thay vì hỏi “có bao nhiêu khoảng chồng lên khoảng này”, chúng tôi hỏi “số lượng hoạt động tăng lên như thế nào khi thời gian trôi qua”. 

Điều này biến vấn đề thành một nhiệm vụ xử lý sự kiện. Mỗi khoảng đóng góp chính xác hai sự kiện: một sự kiện vào và một sự kiện thoát. Nếu chúng ta sắp xếp tất cả các sự kiện theo thời gian và quét qua chúng, duy trì một bộ đếm đang hoạt động, chúng ta có thể theo dõi số lượng khách hàng đang hoạt động tại mọi thời điểm. Câu trả lời đơn giản là giá trị tối đa của bộ đếm này. 

Khó khăn duy nhất còn lại là xử lý các sự kiện xảy ra cùng lúc. Chúng tôi giải quyết vấn đề này bằng cách đảm bảo rằng các chuyến khởi hành được xử lý trước khi các chuyến đến ở các mốc thời gian bằng nhau, sao cho khách hàng rời đi vào thời điểm t không trùng với khách hàng đến vào thời điểm t. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Sweep Line (sắp xếp sự kiện) | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển đổi mỗi khoảng thời gian của khách hàng thành hai sự kiện: một sự kiện thể hiện sự xuất hiện tại thời điểm l và một sự kiện thể hiện sự khởi hành tại thời điểm r. Việc cải cách này rất hữu ích vì nó thay thế lý luận chồng chéo phạm vi bằng những thay đổi trạng thái rời rạc. 
2. Mã hóa số lượt đến khi thay đổi +1 về số lượng khách hàng đang hoạt động và số lần khởi hành khi thay đổi -1. Điều này cho phép chúng tôi duy trì một biến đang chạy thay vì tính toán lại các phần chồng chéo. 
3. Sắp xếp tất cả các sự kiện theo thời gian. Khi hai sự kiện có cùng dấu thời gian, hãy đặt sự kiện khởi hành trước sự kiện đến. Thứ tự này ngăn chặn sự chồng chéo nhân tạo tại các điểm ranh giới chính xác. 
4. Quét qua các sự kiện được sắp xếp từ trái sang phải, duy trì số lượng khách hàng đang hoạt động. 
5. Tại mỗi sự kiện, hãy cập nhật bộ đếm bằng giá trị delta của nó và theo dõi giá trị tối đa được quan sát cho đến nay. 
6. Trả về giá trị bộ đếm tối đa sau khi xử lý tất cả các sự kiện. 

Tại sao nó hoạt động: tính bất biến của đường quét là sau khi xử lý tất cả các sự kiện cho đến thời điểm t, bộ đếm đang chạy chính xác bằng số khoảng chứa t theo quy ước biên đã chọn. Bởi vì mỗi khoảng thời gian đóng góp chính xác một +1 và một -1, đồng thời vì thứ tự đảm bảo xử lý chính xác các dấu thời gian bằng nhau nên không có khoảng thời gian nào bị tính hai lần hoặc bị bỏ sót. Do đó, mức tối đa trên tất cả các trạng thái quét phải bằng mức chồng lấp đồng thời tối đa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    events = []

    for _ in range(n):
        l, r = map(int, input().split())
        events.append((l, 1))
        events.append((r, -1))

    events.sort(key=lambda x: (x[0], x[1]))

    cur = 0
    best = 0

    for t, delta in events:
        cur += delta
        if cur > best:
            best = cur

    print(best)

if __name__ == "__main__":
    solve()
```Cốt lõi của việc thực hiện là chuyển đổi sự kiện. Mỗi khoảng sẽ trở thành hai mục và chúng tôi không bao giờ lưu trữ rõ ràng các khoảng đó sau thời điểm đó. Bước sắp xếp thực thi đúng thứ tự thời gian và quy tắc ràng buộc được nhúng trong`(time, delta)`đảm bảo rằng các sự kiện -1 được xử lý trước các sự kiện +1 tại các thời điểm giống hệt nhau. 

Bản thân việc quét là một lần vượt qua. Biến`cur`đại diện cho số lượng khách hàng hiện đang ở bên trong, trong khi`best`theo dõi mức tối đa được nhìn thấy. Thứ tự cập nhật quan trọng: chúng tôi áp dụng sự kiện trước, sau đó xem xét liệu trạng thái mới có phải là ứng cử viên cho câu trả lời hay không. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
1 4
2 5
7 9
```Sự kiện sau khi chuyển đổi: 

| Bước | Sự kiện | Số lượng hoạt động | 
| --- | --- | --- | 
| Bắt đầu | - | 0 | 
| 1 | (1, +1) | 1 | 
| 2 | (2, +1) | 2 | 
| 3 | (4, -1) | 1 | 
| 4 | (5, -1) | 0 | 
| 5 | (7, +1) | 1 | 
| 6 | (9, -1) | 0 | 

Số lượng hoạt động tối đa là 2. 

Điều này cho thấy sự chồng chéo tích lũy một cách tự nhiên như thế nào mà không cần so sánh rõ ràng các khoảng thời gian. 

### Ví dụ 2 

đầu vào:```
2
1 5
5 10
```Sự kiện: 

| Bước | Sự kiện | Số lượng hoạt động | 
| --- | --- | --- | 
| Bắt đầu | - | 0 | 
| 1 | (1, +1) | 1 | 
| 2 | (5, -1) | 0 | 
| 3 | (5, +1) | 1 | 
| 4 | (10, -1) | 0 | 

Số lượng hoạt động tối đa là 1. 

Dấu vết này chứng minh tại sao việc sắp xếp các chuyến khởi hành trước khi đến vào các khoảng thời gian bằng nhau là điều cần thiết. Không có nó, trạng thái trung gian tại thời điểm 5 sẽ trở thành 2 một cách sai lầm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp sự kiện 2n chiếm ưu thế, quét là tuyến tính | 
| Không gian | O(n) | Lưu trữ hai sự kiện mỗi khoảng thời gian | 

Giải pháp thoải mái phù hợp với các ràng buộc điển hình trong khoảng thời gian lên tới 200.000. Việc sắp xếp 400.000 sự kiện nằm trong giới hạn và việc quét tuyến tính duy nhất đảm bảo thời gian chạy có thể dự đoán được. 

## Trường hợp thử nghiệm```python
import sys, io

def solve_input(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

def solve():
    import sys
    input = sys.stdin.readline
    n = int(input())
    events = []
    for _ in range(n):
        l, r = map(int, input().split())
        events.append((l, 1))
        events.append((r, -1))
    events.sort(key=lambda x: (x[0], x[1]))

    cur = 0
    best = 0
    for _, d in events:
        cur += d
        if cur > best:
            best = cur
    print(best)

# provided samples
assert solve_input("3\n1 4\n2 5\n7 9\n") == "2"
assert solve_input("2\n1 5\n5 10\n") == "1"

# custom cases
assert solve_input("1\n10 20\n") == "1", "single interval"
assert solve_input("3\n1 10\n1 10\n1 10\n") == "3", "all overlap"
assert solve_input("3\n1 2\n3 4\n5 6\n") == "1", "disjoint"
assert solve_input("2\n1 2\n2 3\n") == "1", "boundary overlap handling"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| khoảng đơn | 1 | trường hợp kích thước tối thiểu | 
| khoảng giống hệt nhau | 3 | xếp chồng lên nhau đầy đủ | 
| khoảng rời rạc | 1 | không tích lũy chồng chéo | 
| chạm ranh giới | 1 | xử lý cà vạt đúng cách | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là khi nhiều khoảng chia sẻ cùng một điểm cuối. Nếu tất cả khách hàng đến lúc 1 và rời đi lúc 10, thuật toán phải cộng dồn tất cả khách đến trước khi khởi hành, đạt cực đại bằng n. Việc sắp xếp sự kiện đảm bảo điều này vì các lượt đến được coi là +1 và chỉ được xử lý sau -1 khi thời gian khác nhau, trong khi thứ tự thời gian bằng nhau giữ cho cấu trúc dự định nhất quán. 

Một trường hợp cạnh khác là một khoảng đơn. Thuật toán tạo ra chính xác 1 vì nó đưa ra chính xác một +1 và một -1, đồng thời quá trình quét không bao giờ vượt quá số lượng một. 

Trường hợp tinh tế cuối cùng là các điểm cuối xen kẽ như (1,2), (2,3), (3,4). Câu trả lời đúng là 1 và quá trình quét duy trì điều này vì mọi -1 tại thời điểm t đều được xử lý trước hoặc cùng với +1 cùng lúc, ngăn chặn việc xếp chồng nhân tạo ở các ranh giới.
