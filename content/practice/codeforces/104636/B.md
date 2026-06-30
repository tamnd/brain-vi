---
title: "CF 104636B - Vlad và quán cà phê"
description: "Chúng ta được cung cấp một chuỗi các lần ghé thăm quán cà phê, trong đó mỗi con số đại diện cho chỉ số của quán cà phê mà Vlad đã ghé thăm tại thời điểm đó. Các quán cà phê có thể lặp lại, nghĩa là Vlad có thể ghé thăm cùng một quán cà phê nhiều lần và chúng tôi chỉ quan tâm đến thứ tự ghé thăm."
date: "2026-06-29T17:05:28+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104636
codeforces_index: "B"
codeforces_contest_name: "\u041c\u0438\u0441\u0438\u0441 2023 \u043e\u0441\u0435\u043d\u044c - \u043c\u0430\u0441\u0441\u0438\u0432\u044b, \u0441\u0442\u0440\u043e\u043a\u0438"
rating: 0
weight: 104636
solve_time_s: 108
verified: true
draft: false
---

[CF 104636B - Vlad và quán cà phê](https://codeforces.com/problemset/problem/104636/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 48s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các lần ghé thăm quán cà phê, trong đó mỗi con số đại diện cho chỉ số của quán cà phê mà Vlad đã ghé thăm tại thời điểm đó. Các quán cà phê có thể lặp lại, nghĩa là Vlad có thể ghé thăm cùng một quán cà phê nhiều lần và chúng tôi chỉ quan tâm đến thứ tự ghé thăm. 

Nhiệm vụ là xác định quán cà phê có lượt ghé thăm gần đây nhất là sớm nhất trong số tất cả các quán cà phê xuất hiện trong chuỗi. Nói một cách đơn giản hơn, đối với mỗi quán cà phê riêng biệt, chúng tôi xem xét thời điểm nó được ghé thăm lần cuối và chúng tôi muốn quán cà phê có lần xuất hiện gần đây nhất trong quá khứ. 

Các ràng buộc cho phép tối đa 200.000 lượt truy cập, do đó, bất kỳ giải pháp nào quét liên tục mảng cho từng quán cà phê riêng biệt sẽ quá chậm. Cách tiếp cận bậc hai sẽ liên quan đến việc quét toàn bộ mảng trên mỗi quán cà phê, có thể giảm xuống còn khoảng 4 × 10^10 hoạt động trong trường hợp xấu nhất, điều này không khả thi trong 2 giây. Điều này ngay lập tức đẩy chúng ta tới một giải pháp tuyến tính hoặc gần tuyến tính. 

Một trường hợp phức tạp phát sinh khi nhiều quán cà phê chỉ xuất hiện một lần. Trong trường hợp đó, câu trả lời là quán cà phê có lần xuất hiện sớm nhất trong mảng, vì lần ghé thăm cuối cùng của nó cũng là lần xuất hiện đó. Một trường hợp đặc biệt khác xảy ra khi tất cả các giá trị đều giống hệt nhau, trong đó câu trả lời là quán cà phê đó một cách tầm thường. 

Một lỗi phổ biến là cố gắng theo dõi lần xuất hiện đầu tiên thay vì lần xuất hiện cuối cùng. Ví dụ: theo trình tự như 2 1 2 3, quán cà phê 1 xuất hiện một lần nhưng không nhất thiết phải liên quan, trong khi quán cà phê 2 xuất hiện hai lần và lần xuất hiện cuối cùng của nó là muộn hơn. Câu trả lời đúng chỉ phụ thuộc vào vị trí cuối cùng chứ không phải lần xuất hiện đầu tiên. 

## Phương pháp tiếp cận 

Giải pháp brute-force sẽ xem xét từng quán cà phê riêng biệt, quét toàn bộ mảng và ghi lại vị trí cuối cùng nơi nó xuất hiện. Sau khi tính toán tất cả các lần xuất hiện cuối cùng, chúng tôi chọn mức tối thiểu trong số đó. Mặc dù đúng nhưng phương pháp này lặp lại quá trình quét toàn bộ cho từng quán cà phê riêng biệt. Nếu có k quán cà phê riêng biệt, độ phức tạp về thời gian trong trường hợp xấu nhất sẽ trở thành O(nk), suy biến thành O(n^2) khi tất cả các giá trị là khác biệt. 

Quan sát quan trọng là chúng ta không cần phải tính toán lại nhiều lần các lần xuất hiện cuối cùng. Mỗi lần chúng ta nhìn thấy một quán cà phê trong mảng, chúng ta có thể ghi đè lên vị trí đã lưu trữ của nó. Khi kết thúc quá trình quét, giá trị được lưu trữ cho mỗi quán cà phê chính xác là chỉ số xuất hiện cuối cùng của nó. Điều này làm giảm vấn đề xuống còn một lần duyệt qua mảng, sau đó là một bước lựa chọn đơn giản trên các vị trí đã ghi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(n) | Quá chậm | 
| Tối ưu (theo dõi lần xuất hiện cuối cùng) | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo từ điển hoặc mảng`last`ánh xạ từng chỉ mục quán cà phê đến vị trí gần đây nhất của nó trong chuỗi. Chúng tôi khởi tạo nó trống vì chưa có quán cà phê nào được nhìn thấy. 
2. Lặp lại trình tự truy cập từ trái sang phải. Đối với mỗi vị trí`i`, cửa hàng`last[a[i]] = i`. Điều này đảm bảo rằng mỗi khi chúng tôi nhìn thấy một quán cà phê, chúng tôi sẽ ghi đè lên bản ghi trước đó của nó, vì vậy chỉ còn lại vị trí gần đây nhất. 
3. Sau khi xử lý toàn bộ chuỗi, giờ đây chúng tôi có chỉ số xuất hiện cuối cùng cho mỗi quán cà phê xuất hiện ít nhất một lần. 
4. Quét qua tất cả các quán cà phê được ghi lại và chọn quán cà phê có chỉ mục được lưu trữ nhỏ nhất. Điều này tương ứng với quán cà phê có lần ghé thăm gần đây nhất trong quá khứ. 
5. Xuất ra chỉ số của quán cà phê đó. 

Lý do chúng tôi quét tất cả các quán cà phê được ghi ở cuối thay vì duy trì mức chạy tối thiểu là vì chúng tôi chỉ biết vị trí cuối cùng sau khi xử lý toàn bộ chuỗi. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào trong quá trình lặp,`last[x]`chính xác là vị trí gần đây nhất của quán cà phê`x`trong số các tiền tố được xử lý cho đến nay. Khi vòng lặp kết thúc, giá trị này sẽ trở thành giá trị thực sự xuất hiện cuối cùng trong toàn bộ mảng. Vì câu trả lời cuối cùng chỉ phụ thuộc vào những lần xuất hiện cuối cùng này nên việc chọn chỉ số tối thiểu trong số chúng sẽ xác định chính xác quán cà phê có lượt ghé thăm gần đây nhất sớm nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
a = list(map(int, input().split()))

last = {}

for i, x in enumerate(a):
    last[x] = i

ans = None
best_pos = 10**18

for x, pos in last.items():
    if pos < best_pos:
        best_pos = pos
        ans = x

print(ans)
```Lần đầu tiên đi qua mảng sẽ xây dựng ánh xạ từ quán cà phê đến chỉ mục được nhìn thấy lần cuối của nó. Chỉ số liệt kê`i`đương nhiên đại diện cho thời gian ghé thăm. Mỗi bài tập sẽ ghi đè lên các vị trí trước đó, điều này rất cần thiết để đảm bảo tính chính xác. 

Vòng lặp thứ hai so sánh các vị trí cuối cùng được lưu trữ. Chúng tôi theo dõi cả vị trí tốt nhất và quán cà phê tương ứng. Việc sử dụng giá trị ban đầu lớn sẽ đảm bảo lần so sánh đầu tiên luôn thành công. 

Một cạm bẫy phổ biến là việc sử dụng lập chỉ mục dựa trên 1 và 0 một cách không nhất quán. Ở đây, chúng tôi sử dụng các chỉ mục dựa trên 0 trong nội bộ, nhưng vì chúng tôi chỉ so sánh thứ tự tương đối nên câu trả lời cuối cùng vẫn đúng bất kể cơ sở lập chỉ mục. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
5
1 3 2 1 2
```| tôi | quán cà phê | bước cuối cùng | 
| --- | --- | --- | 
| 0 | 1 | {1:0} | 
| 1 | 3 | {1:0, 3:1} | 
| 2 | 2 | {1:0, 3:1, 2:2} | 
| 3 | 1 | {1:3, 3:1, 2:2} | 
| 4 | 2 | {1:3, 3:1, 2:4} | 

Lần xuất hiện cuối cùng cuối cùng là 1 → 3, 3 → 1, 2 → 4. Tối thiểu là vị trí 1, tương ứng với quán cà phê 3. 

Đầu ra:```
3
```Dấu vết này cho thấy rằng chỉ có ghi đè cuối cùng mới quan trọng. Những lần xuất hiện trước đó của quán cà phê 1 và 2 sẽ không còn liên quan khi lượt truy cập cuối cùng của họ được cập nhật. 

### Mẫu 2 

đầu vào:```
6
2 1 2 2 4 1
```| tôi | quán cà phê | bước cuối cùng | 
| --- | --- | --- | 
| 0 | 2 | {2:0} | 
| 1 | 1 | {2:0, 1:1} | 
| 2 | 2 | {2:2, 1:1} | 
| 3 | 2 | {2:3, 1:1} | 
| 4 | 4 | {2:3, 1:1, 4:4} | 
| 5 | 1 | {2:3, 1:5, 4:4} | 

Vị trí cuối cùng cuối cùng là 2 → 3, 1 → 5, 4 → 4. Tối thiểu là 3, tương ứng với quán cà phê 2. 

Đầu ra:```
2
```Ví dụ này nêu bật việc ghi đè lặp đi lặp lại. Cafe 2 xuất hiện nhiều lần, nhưng chỉ lần xuất hiện cuối cùng mới quyết định điểm số của nó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một lần ghi lại lần xuất hiện cuối cùng và một lần ghi lại các khóa riêng biệt | 
| Không gian | O(n) | Cửa hàng có nhiều nhất một mục cho mỗi quán cà phê riêng biệt | 

Giải pháp này phù hợp một cách thoải mái trong các ràng buộc vì 200.000 thao tác là không đáng kể đối với một đường truyền tuyến tính trong Python và các thao tác từ điển được khấu hao theo thời gian không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))

    last = {}
    for i, x in enumerate(a):
        last[x] = i

    ans = None
    best_pos = 10**18
    for x, pos in last.items():
        if pos < best_pos:
            best_pos = pos
            ans = x

    return str(ans)

# provided samples
assert run("5\n1 3 2 1 2\n") == "3"
assert run("6\n2 1 2 2 4 1\n") == "2"

# minimum size
assert run("1\n7\n") == "7"

# all equal
assert run("5\n9 9 9 9 9\n") == "9"

# strictly increasing distinct
assert run("4\n1 2 3 4\n") == "1"

# last is earliest candidate
assert run("5\n5 4 3 2 1\n") == "5"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | cùng một phần tử | trường hợp ranh giới tối thiểu | 
| tất cả đều bình đẳng | giá trị đó | lặp đi lặp lại ghi đè chính xác | 
| trình tự tăng dần | phần tử đầu tiên | lần xuất hiện cuối cùng sớm nhất | 
| dãy giảm dần | phần tử cuối cùng | đảo ngược thứ tự chính xác | 

## Vỏ cạnh 

### Chuyến thăm duy nhất 

đầu vào:```
1
10
```Thuật toán lưu trữ`last[10] = 0`. Không có mục nào khác tồn tại, vì vậy mức tối thiểu là 10. Kết quả đầu ra là chính xác vì theo định nghĩa, quán cà phê duy nhất vừa là đầu tiên vừa là cuối cùng. 

### Tất cả các quán cà phê đều giống hệt nhau 

đầu vào:```
5
3 3 3 3 3
```Mỗi lần lặp lại ghi đè`last[3]`, nhưng giá trị vẫn là 3 với vị trí cuối cùng là 4. Vì chỉ có một khóa trong từ điển nên nó sẽ tự động được chọn. Điều này xác nhận rằng việc ghi đè lặp đi lặp lại không ảnh hưởng đến tính chính xác. 

### Tất cả các quán cà phê khác biệt 

đầu vào:```
4
8 1 6 2
```Bản đồ cuối cùng cuối cùng là`{8:0, 1:1, 6:2, 2:3}`. Chỉ số tối thiểu là 0 nên quán cà phê 8 được chọn. Điều này cho thấy giải pháp rút gọn chính xác để tìm ra sự xuất hiện sớm nhất khi không tồn tại sự lặp lại.
