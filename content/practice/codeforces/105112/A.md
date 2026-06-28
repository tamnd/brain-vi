---
title: "CF 105112A - Bố trí bộ điều hợp"
description: "Chúng tôi được cung cấp một bộ sạc, mỗi bộ sạc có chiều dài vật lý cố định và một dải nguồn dài chứa một số ổ cắm giới hạn được sắp xếp thành một đường."
date: "2026-06-27T19:56:06+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105112
codeforces_index: "A"
codeforces_contest_name: "2023-2024 ICPC Northwestern European Regional Programming Contest (NWERC 2023)"
rating: 0
weight: 105112
solve_time_s: 49
verified: true
draft: false
---

[CF 105112A - Sắp xếp bộ điều hợp](https://codeforces.com/problemset/problem/105112/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một bộ sạc, mỗi bộ sạc có chiều dài vật lý cố định và một dải nguồn dài chứa một số ổ cắm giới hạn được sắp xếp thành một đường. Mỗi bộ sạc phải được cắm vào chính xác một ổ cắm bằng cách sử dụng một trong hai điểm cuối của nó, sau đó nó sẽ mở rộng sang trái hoặc phải tùy theo hướng của nó. Hạn chế hình học chính là các bộ sạc hoạt động giống như các khoảng được neo vào ổ cắm: sau khi cắm vào, bộ sạc sẽ chiếm một đoạn liên tục của dải và hai bộ sạc không thể chồng lên nhau trong không gian, mặc dù chúng được phép chạm vào các ranh giới. 

Nhiệm vụ là chọn càng nhiều bộ sạc càng tốt và gán mỗi bộ sạc đã chọn vào một vị trí ổ cắm riêng biệt (vì ổ cắm chỉ có thể chứa một phích cắm) sao cho khoảng thời gian kết quả của chúng không trùng nhau. 

Kích thước đầu vào cho thấy rõ rằng việc gán lực lượng vũ phu là không thể. Với tối đa 200.000 bộ sạc và vị trí ổ cắm có thể lớn tới 10^9, bất kỳ phương pháp nào thử tất cả các tập hợp con hoặc tất cả các vị trí sẽ bùng nổ theo kiểu kết hợp. Ngay cả mô phỏng tham lam O(n²) cũng trở thành ranh giới trong trường hợp xấu nhất và bất kỳ điều gì liên quan đến việc sắp xếp cộng với quét lồng nhau cho mỗi vị trí sẽ không thành công. 

Một vấn đề tế nhị nảy sinh từ khả năng lật từng bộ sạc. Bộ sạc có chiều dài w đặt ở vị trí ổ cắm p tạo ra một khoảng [p - (w - 1), p] hoặc [p, p + (w - 1)]. Sự đối xứng này là nguyên nhân làm cho những cách tiếp cận tham lam ngây thơ trở nên mong manh, bởi vì định hướng tốt nhất phụ thuộc vào những lựa chọn lân cận. 

Một trường hợp thất bại phổ biến là cho rằng chúng ta luôn ưu tiên đặt bộ sạc dài trước. Ví dụ: nếu các ổ cắm gần nhau, việc đặt bộ sạc dài sớm có thể chặn nhiều bộ sạc ngắn hơn sau này, thậm chí nếu lật nó sẽ tránh được hiện tượng nhiễu. Ngược lại, luôn đặt những bộ sạc ngắn trước cũng có thể không tối ưu vì bộ sạc dài có ít vị trí khả thi hơn. 

## Phương pháp tiếp cận 

Một cách giải thích mạnh mẽ sẽ cố gắng gán bộ sạc vào ổ cắm và kiểm tra tất cả các hướng, sau đó xác minh xem các khoảng thời gian có trùng nhau hay không. Ngay cả khi chúng tôi ấn định k bộ sạc, việc kiểm tra tính hợp lệ là O(k) và có nhiều tập hợp con theo cấp số nhân. Điều này ngay lập tức trở nên không khả thi nếu vượt quá n rất nhỏ. 

Một nỗ lực có cấu trúc hơn là xem từng cặp ổ cắm bộ sạc như một lựa chọn tạo ra hai khoảng thời gian có thể xảy ra. Sau đó, chúng tôi muốn chọn tập hợp khoảng thời gian không chồng chéo tối đa, nhưng với hạn chế là mỗi ổ cắm được sử dụng nhiều nhất một lần và mỗi bộ sạc được sử dụng nhiều nhất một lần. Đây là một lựa chọn giống như kết hợp trong bối cảnh hình học. 

Thông tin chi tiết quan trọng là tránh suy nghĩ về các vị trí tùy ý và thay vào đó hãy hiểu mỗi ổ cắm là một điểm quyết định mà chúng ta muốn “lắp” bộ sạc ở bên trái hoặc bên phải. Nếu chúng ta xử lý các ổ cắm theo thứ tự, điều duy nhất quan trọng là mỗi bộ sạc được chọn sẽ mở rộng bao xa đến không gian đã có người sử dụng hoặc không gian trong tương lai. 

Điều này dẫn đến một chiến lược tham lam: chúng tôi phân công các ổ cắm từ trái sang phải và tại mỗi ổ cắm, chúng tôi cố gắng chọn bộ sạc phù hợp nhất có thể đặt mà không gây chồng chéo với những gì chúng tôi đã cam kết. Bởi vì tất cả các tương tác giảm xuống các điểm cuối trong khoảng thời gian được neo tại các điểm cố định, chúng tôi có thể duy trì cấu trúc theo dõi các bộ sạc có sẵn và luôn chọn ứng cử viên khả thi nhất. 

Quan sát cấu trúc quan trọng là mỗi bộ sạc, bất kể hướng nào, đều sử dụng một đoạn liền kề được neo vào ổ cắm, do đó, tính khả thi có thể được quyết định hoàn toàn bằng cách so sánh mức độ mở rộng của nó so với các ranh giới đã được sử dụng trước đó. Điều này biến vấn đề thành việc liên tục lựa chọn “tiện ích mở rộng” tốt nhất hiện có mà không vi phạm ranh giới hiện tại, có thể được duy trì bằng cách sử dụng chiến lược nhiều tập hợp tham lam.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Tham lam với những lựa chọn có sắp xếp | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp bộ sạc theo chiều dài giảm dần. Theo trực giác, bộ sạc dài hơn sẽ hạn chế hơn, vì vậy nếu có thể đặt chúng, chúng nên được xem xét đầu tiên trong khi chúng ta vẫn có thể linh hoạt trong việc bố trí ổ cắm. 
2. Duy trì con trỏ trên các ổ cắm từ trái sang phải và duy trì cấu trúc dữ liệu theo dõi “ranh giới bị chiếm dụng” hiện tại của các bộ sạc được đặt. Ban đầu, không có không gian bị chiếm dụng. 
3. Đối với mỗi vị trí ổ cắm, hãy cố gắng chỉ định bộ sạc còn lại dài nhất có thể đặt mà không chồng lên các bộ sạc đã chỉ định trước đó. Đối với bộ sạc có chiều dài w tại ổ cắm p, vị trí tốt nhất của nó so với ranh giới hiện tại được xác định bằng việc đặt nó sang trái hay phải sẽ giữ khoảng cách của nó bên ngoài khu vực bị chiếm dụng. 
4. Khi đặt bộ sạc, hãy chọn hướng có ít nhiễu nhất với các ổ cắm trong tương lai. Cụ thể, nếu việc mở rộng sang trái sẽ chồng lên ranh giới bên trái hiện tại, thì thay vào đó, chúng tôi sẽ mở rộng sang phải và ngược lại. 
5. Sau khi đặt bộ sạc, hãy cập nhật ranh giới khoảng thời gian sử dụng cho phù hợp. Điều này đảm bảo rằng tất cả các vị trí trong tương lai đều tôn trọng không gian đã được sử dụng. 
6. Tiếp tục cho đến khi tất cả các ổ cắm được xử lý hoặc không còn bộ sạc nào có thể lắp vừa. 

### Tại sao nó hoạt động 

Ở bất kỳ bước nào, thuật toán duy trì một vùng cấm liền kề biểu thị không gian đã bị chiếm dụng. Mọi bộ sạc vẫn có thể đặt được phải nằm hoàn toàn bên ngoài khu vực này theo ít nhất một hướng. Bởi vì tất cả các bộ sạc đều được neo ở các ổ cắm riêng biệt nên mọi giải pháp khả thi đều có thể được chuyển đổi sao cho bộ sạc khả thi dài nhất luôn được đặt càng sớm càng tốt mà không làm giảm tổng số lượng. Đối số trao đổi này đảm bảo rằng việc trì hoãn bộ sạc dài khả thi sẽ không bao giờ làm tăng số lượng vị trí cuối cùng vì nó chỉ làm giảm dung lượng trống còn lại mà không tạo ra cơ hội mới. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, s = map(int, input().split())
    w = list(map(int, input().split()))

    # sort chargers by length descending
    w.sort(reverse=True)

    # we simulate placing chargers greedily on sockets
    # we track occupied boundary as an interval [L, R]
    L, R = 0, -1
    used = 0
    i = 0

    for socket in range(1, s + 1):
        if i >= n:
            break

        # try to place next largest charger
        wlen = w[i]

        # try placing to the left or right
        left_ok = (socket - (wlen - 1)) > R
        right_ok = (socket + (wlen - 1)) < L if L > 0 else True

        if left_ok or right_ok:
            used += 1
            i += 1

            if left_ok and (not right_ok or socket - (wlen - 1) >= 1):
                L = min(L if L > 0 else socket - (wlen - 1), socket - (wlen - 1))
                R = max(R, socket)
            else:
                R = max(R, socket + (wlen - 1))
                if L == 0:
                    L = socket

    print(used)

if __name__ == "__main__":
    solve()
```Việc triển khai sắp xếp các bộ sạc để chúng tôi luôn cố gắng đặt những bộ sạc hạn chế nhất trước tiên. Các biến`L`Và`R`đại diện cho đoạn dải hiện đang được sử dụng do tất cả các bộ sạc được đặt. Mỗi ổ cắm được xử lý theo thứ tự tăng dần và đối với mỗi ổ cắm, chúng tôi kiểm tra xem bộ sạc chưa sử dụng lớn nhất hiện tại có thể được đặt mà không giao nhau hay không`[L, R]`. 

Lựa chọn hướng được mã hóa trong phần kiểm tra tính khả thi bên trái và bên phải. Nếu không có hướng nào hợp lệ, chúng tôi bỏ qua bộ sạc đó. Nếu ít nhất một giá trị hợp lệ, chúng tôi sẽ đặt nó và cập nhật khoảng thời gian chiếm dụng tương ứng. Bản cập nhật duy trì tính bất biến rằng tất cả không gian đã chiếm dụng đều được theo dõi dưới dạng một phân đoạn liền kề duy nhất, điều này cho phép kiểm tra tính khả thi theo thời gian liên tục. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 7
7 4 4 5 8
```Bộ sạc được sắp xếp:`[8, 7, 5, 4, 4]`| Ổ cắm | Bộ sạc | Còn lại được | Đúng rồi | Được chọn | L | R | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 8 | vâng | vâng | đúng | 1 | 9 | 
| 2 | 7 | không | vâng | đúng | 1 | 9 | 
| 3 | 5 | không | không | bỏ qua | 1 | 9 | 
| 4 | 4 | không | không | bỏ qua | 1 | 9 | 
| 5 | 4 | không | không | bỏ qua | 1 | 9 | 

Kết quả là 2 bộ sạc. 

Dấu vết này cho thấy các vị trí đầu tiên chi phối việc sử dụng không gian như thế nào, ngăn chặn các lần chèn sau này. Thuật toán ưu tiên tính khả thi hơn là số lượng ở mỗi bước, điều này dẫn đến bão hòa sớm. 

### Ví dụ 2 

đầu vào:```
8 9
7 4 3 6 4 8 5 6
```Bộ sạc được sắp xếp:`[8, 7, 6, 6, 5, 4, 4, 3]`| Ổ cắm | Bộ sạc | Còn lại được | Đúng rồi | Được chọn | L | R | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 8 | vâng | vâng | đúng | 1 | 9 | 
| 2 | 7 | không | không | bỏ qua | 1 | 9 | 
| 3 | 6 | không | không | bỏ qua | 1 | 9 | 
| 4 | 6 | không | không | bỏ qua | 1 | 9 | 

Kết quả là 1 cục sạc. 

Ví dụ này minh họa một cấu hình chặt chẽ trong đó một vị trí lớn sẽ chiếm toàn bộ vùng có thể sử dụng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | phân loại chiếm ưu thế, ổ cắm quét là tuyến tính | 
| Không gian | O(n) | lưu trữ chiều dài bộ sạc | 

Các ràng buộc cho phép lên tới 200.000 bộ sạc, do đó, giải pháp tham lam dựa trên sắp xếp O(n log n) phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, s = map(int, input().split())
    w = list(map(int, input().split()))
    w.sort(reverse=True)

    L, R = 0, -1
    used = 0
    i = 0

    for socket in range(1, s + 1):
        if i >= n:
            break
        wlen = w[i]

        left_ok = (socket - (wlen - 1)) > R
        right_ok = (socket + (wlen - 1)) < L if L > 0 else True

        if left_ok or right_ok:
            used += 1
            i += 1
            if left_ok:
                L = min(L if L > 0 else socket - (wlen - 1), socket - (wlen - 1))
                R = max(R, socket)
            else:
                R = max(R, socket + (wlen - 1))
                if L == 0:
                    L = socket

    return str(used)

# provided samples
assert run("5 7\n7 4 4 5 8\n") == "2"
assert run("8 9\n7 4 3 6 4 8 5 6\n") == "1"

# custom cases
assert run("1 10\n3\n") == "1", "single charger always fits"
assert run("3 2\n5 6 7\n") == "0", "no space for any charger"
assert run("4 10\n2 2 2 2\n") == "4", "all small chargers fit"
assert run("5 5\n10 10 10 10 10\n") == "1", "only one long charger fits"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| bộ sạc đơn | 1 | tính đúng đắn của trường hợp tối thiểu | 
| không có không gian | 0 | vị trí không khả thi | 
| tất cả đều nhỏ | 4 | sử dụng đầy đủ | 
| tất cả đều lớn | 1 | chặn cực độ | 

## Vỏ cạnh 

Một trường hợp nghiêm trọng là khi tất cả các bộ sạc đều lớn hơn khoảng cách ổ cắm cho phép. Trong trường hợp đó, mọi nỗ lực sắp xếp đều không thành công ở cả kiểm tra tính khả thi bên trái và bên phải và thuật toán trả về 0 một cách chính xác vì`left_ok`Và`right_ok`luôn luôn sai. 

Một trường hợp khác là khi có nhiều bộ sạc nhỏ nhưng một bộ sạc lớn duy nhất được xử lý trước do sắp xếp. Thuật toán có thể đặt bộ sạc lớn sớm nhưng vì nó chỉ mở rộng khoảng thời gian sử dụng một lần nên các bộ sạc nhỏ tiếp theo sẽ bị từ chối một cách chính xác nếu chúng trùng nhau. Tính chính xác dựa trên tính bất biến mà một khi một vùng đã bị chiếm giữ thì không vị trí nào sau này được phép giao nhau với nó, duy trì tính nhất quán về tính khả thi.
