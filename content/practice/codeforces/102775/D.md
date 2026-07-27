---
title: "CF 102775D - \u0420\u0430\u0437\u043b\u0438\u0447\u043d\u044b\u0435 \u044d\u043b\u0435\u043c\u0435\u043d\u0442\u044b"
description: "Nhiệm vụ là xem qua một dãy số và đếm xem có bao nhiêu giá trị khác nhau thỏa mãn một điều kiện phạm vi nghiêm ngặt. Một giá trị chỉ được đưa vào khi nó lớn hơn x và nhỏ hơn y. Nếu cùng một số hợp lệ xuất hiện nhiều lần thì nó chỉ đóng góp một lần vào câu trả lời."
date: "2026-07-27T20:37:38+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102775
codeforces_index: "D"
codeforces_contest_name: "ICPC Central Russia Regional Contest (CRRC 20), \u0427\u0435\u043c\u043f\u0438\u043e\u043d\u0430\u0442 \u0426\u0435\u043d\u0442\u0440\u0430\u043b\u044c\u043d\u043e\u0439 \u0420\u043e\u0441\u0441\u0438\u0438, \u043a\u0432\u0430\u043b\u0438\u0444\u0438\u043a\u0430\u0446\u0438\u043e\u043d\u043d\u044b\u0439 \u0440\u0430\u0443\u043d\u0434"
rating: 0
weight: 102775
solve_time_s: 43
verified: true
draft: false
---

[CF 102775D - \u0420\u0430\u0437\u043b\u0438\u0447\u043d\u044b\u0435 \u044d\u043b\u0435\u043c\u0435\u043d\u0442\u044b](https://codeforces.com/problemset/problem/102775/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 43s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ là xem qua một dãy số và đếm xem có bao nhiêu giá trị khác nhau thỏa mãn một điều kiện phạm vi nghiêm ngặt. Một giá trị chỉ được đưa vào khi nó lớn hơn`x`và nhỏ hơn`y`. Nếu cùng một số hợp lệ xuất hiện nhiều lần thì nó chỉ đóng góp một lần vào câu trả lời. 

Đầu vào chứa kích thước của mảng, hai đường viền của phạm vi được phép và chính mảng đó. Đầu ra là một số nguyên biểu thị số lượng giá trị mảng duy nhất nằm hoàn toàn giữa hai đường viền đó. 

Mảng có thể chứa tới 150000 phần tử. Giải pháp kiểm tra từng cặp giá trị hoặc tìm kiếm nhiều lần trong mảng sẽ thực hiện quá nhiều thao tác ở kích thước này. Với khoảng 100000 đến 200000 phần tử, chúng ta thường cần một cách tiếp cận gần với thời gian tuyến tính, vì phép tính bậc hai có thể đạt tới hàng tỷ phép tính và sẽ không vừa với giới hạn một giây thông thường. 

Bản thân các giá trị có thể lớn tới hai tỷ theo cả hai hướng, do đó thuật toán phải so sánh các số nguyên một cách chính xác mà không cần dựa vào các phạm vi cố định nhỏ. Trong Python, điều này được xử lý một cách tự nhiên vì số nguyên có độ chính xác tùy ý. 

Một số trường hợp ranh giới có thể phá vỡ một giải pháp không chính xác. Sai lầm phổ biến đầu tiên là coi biên giới là bao hàm. Ví dụ:```
5
1 5
1 2 3 5 6
```Đầu ra đúng là:```
3
```Các giá trị hợp lệ là`2`,`3`, Và`?`Thực ra vì điều kiện khắt khe nên chỉ`2`Và`3`là hợp lệ, vì vậy đầu ra đúng là:```
2
```Việc thực hiện bất cẩn bằng cách sử dụng`x <= ai <= y`sẽ tính`1`Và`5`và trả lại câu trả lời sai. 

Một vấn đề khác là đếm số lần xuất hiện thay vì các giá trị khác nhau. Ví dụ:```
6
0 10
3 3 3 7 7 11
```Đầu ra đúng là:```
2
```Các số hợp lệ là`3`Và`7`. Đếm mọi lần xuất hiện sẽ tạo ra`5`, không phù hợp với yêu cầu. 

Trường hợp cạnh cuối cùng là khi không có giá trị nào thuộc phạm vi:```
4
5 8
1 2 5 8
```Đầu ra đúng là:```
0
```Các giá trị bằng ranh giới bị loại trừ, vì vậy câu trả lời là 0. 

## Phương pháp tiếp cận 

Một giải pháp trực tiếp sẽ kiểm tra mọi phần tử và duy trì tập hợp các giá trị thỏa mãn điều kiện. Đây vốn là một ý tưởng giải pháp hay vì thao tác cần thiết cho mỗi số rất đơn giản: so sánh nó với hai ranh giới và chèn nó vào nếu nó hợp lệ. Công việc duy nhất là quyết định cách tránh đếm trùng lặp. Một tập hợp tự nhiên đại diện cho tập hợp các giá trị hợp lệ khác nhau. 

Việc giải thích bạo lực chậm hơn sẽ so sánh mọi giá trị với mọi giá trị khác để xác định xem nó có xuất hiện trước đó hay không. Cách tiếp cận đó đúng vì nó có thể phát hiện các bản sao một cách rõ ràng, nhưng đối với`N = 150000`nó sẽ hoạt động khoảng`N * N`, hoặc khoảng 22500000000, so sánh trong trường hợp xấu nhất. Điều này vượt xa giới hạn thời gian cho phép. 

Điều quan trọng là việc phát hiện sự trùng lặp không yêu cầu tìm kiếm qua tất cả các phần tử trước đó. Vấn đề chỉ yêu cầu số lượng giá trị hợp lệ khác nhau và một tập hợp chính xác là cấu trúc dữ liệu lưu trữ các phần tử duy nhất đồng thời hỗ trợ chèn nhanh. Mỗi phần tử mảng có thể được xử lý độc lập. Nếu nó nằm trong phạm vi, nó sẽ được thêm vào tập hợp và các giá trị lặp lại chỉ cần giữ nguyên tập hợp. 

Lực lượng vũ phu hoạt động vì nó có thể xác định các bản sao bằng cách kiểm tra mọi thứ với mọi thứ khác, nhưng không thành công vì số lượng so sánh tăng quá nhanh. Việc quan sát thấy tính duy nhất có thể được xử lý trực tiếp bằng bộ băm sẽ giảm tác vụ xuống còn một lần truyền qua mảng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N2) | O(1) | Quá chậm | 
| Tối ưu | O(N) trung bình | O(K) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc kích thước mảng, đường viền của hai phạm vi và tất cả các giá trị mảng. Thuật toán chỉ cần kiểm tra từng giá trị một lần nên dữ liệu đầu vào có thể được xử lý dưới dạng một chuỗi đơn giản. 
2. Tạo một tập hợp trống sẽ lưu trữ tất cả các giá trị thỏa mãn điều kiện. Tập hợp này được sử dụng vì câu trả lời phụ thuộc vào số lượng giá trị khác nhau tồn tại chứ không phải số lượng vị trí chứa các giá trị đó. 
3. Duyệt từng số trong mảng. Nếu số đó lớn hơn`x`và nhỏ hơn`y`, chèn nó vào bộ. Việc so sánh nghiêm ngặt là cần thiết vì các giá trị bằng một trong hai đường viền phải được bỏ qua. 
4. Sau khi tất cả các số đã được xử lý, hãy xuất kích thước của tập hợp. Mỗi phần tử trong tập hợp biểu thị chính xác một giá trị hợp lệ riêng biệt, vì vậy kích thước của nó là câu trả lời bắt buộc. 

Tại sao nó hoạt động: Trong quá trình truyền tải, tập hợp luôn chứa chính xác các giá trị riêng biệt được thấy cho đến nay thỏa mãn điều kiện phạm vi. Việc thêm một giá trị hợp lệ mới sẽ giữ nguyên thuộc tính này vì các bộ sẽ tự động loại bỏ các giá trị trùng lặp. Việc bỏ qua các giá trị không hợp lệ cũng giữ nguyên thuộc tính vì chúng không bao giờ có thể đóng góp vào câu trả lời cuối cùng. Sau khi toàn bộ mảng đã được kiểm tra, bất biến sẽ áp dụng cho toàn bộ đầu vào, do đó kích thước được đặt chính xác là số phần tử được yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    x, y = map(int, input().split())
    a = list(map(int, input().split()))

    values = set()

    for value in a:
        if x < value < y:
            values.add(value)

    print(len(values))

if __name__ == "__main__":
    solve()
```Chương trình đọc tất cả các giá trị mảng một lần và lưu trữ các giá trị hợp lệ trong`values`. Việc triển khai tập hợp của Python tự động xử lý việc chèn và loại bỏ trùng lặp, khớp với định nghĩa toán học của các phần tử riêng biệt. 

điều kiện`x < value < y`được viết dưới dạng so sánh theo chuỗi, thể hiện trực tiếp yêu cầu về phạm vi nghiêm ngặt. sử dụng`<=`ở đây sẽ gây ra một lỗi bằng cách bao gồm các đường viền. 

Câu trả lời cuối cùng có được với`len(values)`, bởi vì mỗi phần tử được lưu trữ tương ứng với chính xác một số duy nhất. Không cần logic đếm bổ sung và các giá trị lặp lại không bao giờ ảnh hưởng đến kết quả. 

Số nguyên Python an toàn trong giới hạn nhất định, do đó không cần xử lý đặc biệt đối với các giá trị gần hai tỷ. 

## Ví dụ đã hoạt động 

Sử dụng cách giải thích tùy chỉnh đầu tiên của tuyên bố: 

đầu vào:```
5
1 5
1 2 3 5 6
```Dấu vết là: 

| Giá trị hiện tại | Phạm vi bên trong? | Đặt sau khi xử lý | 
| --- | --- | --- | 
| 1 | Không |`{}`| 
| 2 | Có |`{2}`| 
| 3 | Có |`{2, 3}`| 
| 5 | Không |`{2, 3}`| 
| 6 | Không |`{2, 3}`| 

Tập cuối cùng chứa hai giá trị, vì vậy đầu ra là`2`. Dấu vết này cho thấy tại sao đường viền bị loại trừ. 

Ví dụ thứ hai: 

đầu vào:```
6
0 10
3 3 3 7 7 11
```Dấu vết là: 

| Giá trị hiện tại | Phạm vi bên trong? | Đặt sau khi xử lý | 
| --- | --- | --- | 
| 3 | Có |`{3}`| 
| 3 | Có |`{3}`| 
| 3 | Có |`{3}`| 
| 7 | Có |`{3, 7}`| 
| 7 | Có |`{3, 7}`| 
| 11 | Không |`{3, 7}`| 

Các giá trị trùng lặp không làm tăng kích thước đã đặt. Câu trả lời cuối cùng là`2`, điều này chứng tỏ tại sao việc đếm số lần xuất hiện sẽ không chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) trung bình | Mỗi phần tử mảng được kiểm tra một lần và việc chèn tập hợp mất thời gian trung bình không đổi. | 
| Không gian | O(K) | Tập hợp này chỉ lưu trữ các giá trị hợp lệ riêng biệt, trong đó K là số lượng các giá trị đó. | 

Kích thước mảng tối đa là 150000, do đó quét tuyến tính thực hiện một số lượng nhỏ thao tác trên mỗi phần tử và vừa vặn trong giới hạn. Việc sử dụng bộ nhớ cũng bị giới hạn bởi số lượng giá trị riêng biệt có thể xuất hiện trong câu trả lời. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve(data):
    input = io.StringIO(data).readline

    n = int(input())
    x, y = map(int, input().split())
    a = list(map(int, input().split()))

    values = set()
    for value in a:
        if x < value < y:
            values.add(value)

    return str(len(values))

def run(inp: str) -> str:
    return solve(inp)

assert run("""5
1 5
1 2 3 5 6
""") == "2", "sample style 1"

assert run("""6
0 10
3 3 3 7 7 11
""") == "2", "sample style 2"

assert run("""1
0 10
5
""") == "1", "single valid element"

assert run("""5
1 2
1 2 3 4 5
""") == "0", "exclusive boundaries"

assert run("""8
-5 5
-5 -4 -4 0 0 4 5 6
""") == "3", "negative values and duplicates"

assert run("""10
0 100
42 42 42 42 42 42 42 42 42 42
""") == "1", "all equal values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`5 / 1 5 / 1 2 3 5 6`| 2 | Xử lý ranh giới nghiêm ngặt | 
|`6 / 0 10 / 3 3 3 7 7 11`| 2 | Loại bỏ trùng lặp | 
|`1 / 0 10 / 5`| 1 | Đầu vào kích thước tối thiểu | 
|`5 / 1 2 / 1 2 3 4 5`| 0 | Phạm vi hợp lệ trống | 
|`8 / -5 5 / -5 -4 -4 0 0 4 5 6`| 3 | Số âm và giá trị cạnh | 
|`10 / 0 100 / ten copies of 42`| 1 | Tất cả các phần tử đều bằng nhau | 

## Vỏ cạnh 

Đối với trường hợp ranh giới nghiêm ngặt:```
5
1 5
1 2 3 5 6
```Thuật toán kiểm tra từng giá trị.`1`bị từ chối vì nó không lớn hơn`1`.`5`bị từ chối vì nó không nhỏ hơn`5`. Bộ chỉ nhận`2`Và`3`, sản xuất`2`. 

Đối với trường hợp trùng lặp:```
6
0 10
3 3 3 7 7 11
```đầu tiên`3`được chèn vào, trong khi hai cái tiếp theo`3`các giá trị bị tập hợp bỏ qua vì chúng đã tồn tại. Điều tương tự cũng xảy ra đối với`7`. giá trị`11`không kiểm tra phạm vi. Tập kết quả là`{3, 7}`, vậy câu trả lời là`2`. 

Đối với trường hợp không có giá trị hợp lệ:```
4
5 8
1 2 5 8
```Mọi con số đều thất bại trong việc so sánh nghiêm ngặt. Bộ vẫn trống từ đầu đến cuối và thuật toán xuất ra chính xác`0`. 

Đối với trường hợp số âm:```
8
-5 5
-5 -4 -4 0 0 4 5 6
```Các giá trị`-5`Và`5`bị loại trừ vì chúng phù hợp với ranh giới. Các giá trị`-4`,`0`, Và`4`được chèn vào, trong khi`6`là quá lớn. Tập cuối cùng chứa ba phần tử, vì vậy câu trả lời là`3`.
