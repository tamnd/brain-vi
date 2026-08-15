---
title: "CF 102386E - \u041e\u0442\u043b\u043e\u0436\u0435\u043d\u043d\u044b\u0435 \u043e\u043f\u0435\u0440\u0430\u0446\u0438\u0438"
description: "Chúng ta có một chuỗi (n) ngày. Vào ngày (i), bài tập về nhà cho môn (ai) xuất hiện. Dima có thể dành cả ngày để làm tất cả các bài tập về nhà hiện đang tích lũy cho một môn học hoặc không làm gì cả. Thực hiện một chủ đề sẽ xóa mọi bài tập của chủ đề đó đã nhận được cho đến nay."
date: "2026-08-14T13:28:38+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102386
codeforces_index: "E"
codeforces_contest_name: "\u041a\u0432\u0430\u043b\u0438\u0444\u0438\u043a\u0430\u0446\u0438\u043e\u043d\u043d\u044b\u0439 \u0442\u0443\u0440 \u0423\u0440\u0430\u043b\u044c\u0441\u043a\u043e\u0433\u043e \u0447\u0435\u0442\u0432\u0435\u0440\u0442\u044c\u0444\u0438\u043d\u0430\u043b\u0430 \u0427\u0435\u043c\u043f\u0438\u043e\u043d\u0430\u0442\u0430 \u043c\u0438\u0440\u0430 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e 2019"
rating: 0
weight: 102386
solve_time_s: 173
verified: false
draft: false
---

[CF 102386E - \u041e\u0442\u043b\u043e\u0436\u0435\u043d\u043d\u044b\u0435 \u043e\u043f\u0435\u0440\u0430\u0446\u0438\u0438](https://codeforces.com/problemset/problem/102386/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 53s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một chuỗi (n) ngày. Vào ngày (i), bài tập về nhà cho môn (a_i) xuất hiện. Dima có thể dành cả ngày để làm tất cả các bài tập về nhà hiện đang tích lũy cho một môn học hoặc không làm gì cả. Thực hiện một chủ đề sẽ xóa mọi bài tập của chủ đề đó đã nhận được cho đến nay. 

Mục tiêu là chọn số ngày làm việc nhỏ nhất có thể trong khi đảm bảo rằng mọi nhiệm vụ đều được hoàn thành vào cuối ngày (n). Đầu ra là một mảng có độ dài (n). Số 0 có nghĩa là Dima nghỉ ngơi vào ngày hôm đó, trong khi giá trị dương (c) có nghĩa là cậu ấy đã hoàn thành tất cả bài tập về nhà tích lũy cho môn học (c). 

Điều quan trọng nhất là các bài tập của cùng một chủ đề có thể được hoãn lại cùng nhau. Nếu bài tập cuối cùng của chủ đề (c) đến vào ngày (p), Dima có thể chỉ cần đợi đến ngày (p) và hoàn thành tất cả các bài tập của (c) ở đó. Vì các đối tượng khác nhau có vị trí xuất hiện cuối cùng khác nhau nên các thao tác này không bao giờ xung đột. 

Các ràng buộc làm cho điều này đặc biệt hữu ích. Với cả (n) và (k) đều lớn bằng (10^5), giải pháp (O(nk)) có thể thực hiện tối đa (10^{10}) thao tác, vượt xa giới hạn thời gian lập trình cạnh tranh thông thường cho phép. Giải pháp (O(n)) hoặc (O(n+k)) là mục tiêu thích hợp. 

Có một số trường hợp nghiêm trọng mà việc triển khai bất cẩn có thể dẫn đến xử lý sai. Nếu một chủ đề chỉ xuất hiện một lần thì lần xuất hiện duy nhất của nó phải được coi là ngày xử lý. Ví dụ, với đầu vào```
1 1
1
```câu trả lời phải là```
1
```bởi vì nhiệm vụ duy nhất phải được hoàn thành. 

Nếu cùng một chủ đề xuất hiện nhiều lần thì việc xử lý nó sau mỗi bài tập là không cần thiết. Vì```
4 2
1 1 1 1
```câu trả lời```
0 0 0 1
```là tối ưu vì cả bốn nhiệm vụ đều có thể được hoàn thành cùng nhau vào ngày cuối cùng. 

Một chủ đề không bao giờ xuất hiện sẽ không nhận được một hoạt động. Ví dụ,```
3 5
2 2 2
```có thể được trả lời bằng```
0 0 2
```và các môn (1,3,4,5) không yêu cầu phải làm gì cả. Việc triển khai khởi tạo mọi chủ đề cho một ngày xử lý nào đó mà không kiểm tra xem liệu điều đó có xảy ra hay không sẽ tạo ra lịch trình không hợp lệ. 

Cuối cùng, chủ đề có lần xuất hiện cuối cùng là ngày cuối cùng là hoàn toàn hợp lệ. Vì```
3 2
1 2 2
```chúng ta có thể xuất ra```
0 1 2
```Hoạt động của chủ đề (1) diễn ra vào ngày thứ 2, trong khi chủ đề (2) được xử lý vào ngày thứ 3 sau khi nhiệm vụ cuối cùng của nó đến. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là xử lý từng chủ đề một cách độc lập và tìm kiếm lần xuất hiện cuối cùng của nó trong toàn bộ mảng. Đối với chủ đề (1), quét từ cuối cho đến khi tìm thấy (1), sau đó lặp lại thao tác tìm kiếm tương tự cho chủ đề (2), v.v. Các vị trí kết quả chính xác là những ngày mà chúng ta nên làm việc. 

Cách tiếp cận này đúng vì lần xuất hiện cuối cùng là đủ để hoàn thành mọi bài tập của môn học đó. Nó trở nên quá chậm vì cùng một mảng được quét liên tục. Trong trường hợp xấu nhất, với (n=k=10^5), việc này cần (n\cdot k=10^{10}) kiểm tra mảng. 

Cấu trúc của bài toán giúp chúng ta tránh được mọi tìm kiếm lặp lại. Trong khi quét chuỗi một lần từ trái sang phải, bất cứ khi nào chúng ta nhìn thấy chủ đề (a_i), chúng ta có thể chỉ cần ghi lại rằng lần xuất hiện gần đây nhất đã biết của nó là ngày (i). Sau khi quét, vị trí được lưu trữ cho mỗi đối tượng xuất hiện chính xác là lần xuất hiện cuối cùng của nó. Sau đó, chúng tôi đưa chủ đề đó vào câu trả lời ở vị trí đó và để cách ngày bằng 0. 

Không cần cây phân đoạn hoặc bất kỳ cấu trúc dữ liệu phức tạp nào. Ý tưởng “trì hoãn” được thực hiện một cách đơn giản bằng cách trì hoãn công việc của từng môn học cho đến khi nhiệm vụ cuối cùng được giao. 

Lý do điều này cũng đưa ra số ngày làm việc tối thiểu là vì mỗi chủ đề xuất hiện ít nhất một lần đều phải được xử lý ít nhất một lần. Nếu không thì nhiệm vụ của nó sẽ vẫn còn dang dở. Quá trình xây dựng của chúng tôi sử dụng đúng một ngày làm việc cho mỗi chủ đề xuất hiện, vì vậy nó đạt đến giới hạn dưới không thể tránh khỏi này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(nk)) | (O(k)) | Quá chậm | 
| Tối ưu | (O(n+k)) | (O(n+k)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc (n), (k) và dãy (a). Tạo một mảng`last`được lập chỉ mục theo chủ đề, ban đầu được điền bằng 0. Số 0 nghĩa là chủ thể chưa xuất hiện. 
2. Quét các ngày từ (1) đến (n). Đối với ngày (i), đặt`last[a[i]] = i`. Nếu chủ đề tương tự xuất hiện lại sau đó, vị trí được lưu trữ của nó sẽ bị ghi đè, do đó sau khi quét, nó sẽ chứa lần xuất hiện cuối cùng. 
3. Tạo một mảng câu trả lời gồm (n) số không. Đối với mọi chủ đề (c) từ (1) đến (k), hãy kiểm tra`last[c]`. Nếu nó khác 0 thì điền (c) vào câu trả lời ở vị trí đó. 

Đây là công trình quan trọng. Ở lần xuất hiện cuối cùng của (c), tất cả các bài tập của (c) đã đến, do đó, thực hiện tất cả công việc tích lũy ở đó sẽ hoàn thành mọi bài tập của chủ đề đó cùng một lúc. 
4. In mảng câu trả lời. Mọi vị trí khác 0 thể hiện chính xác lần xuất hiện cuối cùng của một chủ đề, trong khi tất cả các ngày còn lại đều được để trống. 

Điều bất biến là sau khi xử lý (i) ngày đầu tiên,`last[c]`chính xác là vị trí lớn nhất nhiều nhất (i) nơi chủ ngữ (c) đã xuất hiện, hoặc bằng 0 nếu nó chưa xuất hiện. Cuối cùng, mọi chủ đề xuất hiện sẽ được xử lý chính xác một lần vào lần xuất hiện cuối cùng của nó. Vì mỗi chủ đề xuất hiện đều yêu cầu ít nhất một ngày xử lý đối với bất kỳ giải pháp hợp lệ nào nên số ngày làm việc là tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    a = list(map(int, input().split()))

    last = [0] * (k + 1)

    for i, subject in enumerate(a, start=1):
        last[subject] = i

    answer = [0] * n

    for subject in range(1, k + 1):
        pos = last[subject]
        if pos != 0:
            answer[pos - 1] = subject

    print(*answer)

if __name__ == "__main__":
    solve()
```các`last`mảng có một mục nhập cho mọi chủ đề có thể. Việc lập chỉ mục của nó bắt đầu từ (1), khớp với việc đánh số chủ đề trong đầu vào, trong khi vị trí 0 không được sử dụng. 

Việc liệt kê bắt đầu từ một vì vấn đề mô tả ngày bằng cách đánh số dựa trên một. Câu trả lời cuối cùng sử dụng tính năng lập chỉ mục dựa trên số 0 của Python, vì vậy`last[subject] - 1`được sử dụng khi viết vào`answer`. Đây là chuyển đổi lập chỉ mục duy nhất trong thuật toán và tránh trộn lẫn hai quy ước ở nơi khác. 

Khi quét đầu vào, gán`last[subject] = i`cố ý ghi đè lên các vị trí trước đó. Chúng ta không cần những lần xuất hiện sớm hơn vì tất cả bài tập về nhà tích lũy của môn học đó đều có thể được hoàn thành ở lần xuất hiện cuối cùng. 

Vòng lặp thứ hai kiểm tra mọi chủ đề có thể. Đối tượng với`last[subject] == 0`chưa bao giờ xuất hiện nên không có hoạt động nào được tạo ra cho chúng. Điều này ngăn chặn các chủ đề không được sử dụng vô tình xuất hiện trong câu trả lời. 

Không có vấn đề tràn số nguyên trong Python và số ngày được lưu trữ lớn nhất chỉ là (10^5). 

## Ví dụ đã hoạt động 

Đối với Mẫu 1, đầu vào là```
3 3
1 2 3
```Quá trình quét ghi lại vị trí mới nhất của từng đối tượng như sau. 

| Ngày | Chủ đề |`last[1]`|`last[2]`|`last[3]`| 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 0 | 0 | 
| 2 | 2 | 1 | 2 | 0 | 
| 3 | 3 | 1 | 2 | 3 | 

Mỗi môn học có một thời điểm diễn ra cuối cùng khác nhau nên cả ba ngày đều trở thành ngày làm việc. 

| Chủ đề | Vị trí cuối cùng | Vị trí trả lời | 
| --- | --- | --- | 
| 1 | 1 | 1 | 
| 2 | 2 | 2 | 
| 3 | 3 | 3 | 

Câu trả lời kết quả là```
1 2 3
```Mỗi chủ đề xuất hiện đúng một lần nên không có cơ hội trì hoãn bất kỳ công việc nào ngoài nhiệm vụ duy nhất của nó. 

Đối với Mẫu 2, đầu vào là```
4 2
1 1 1 1
```Sự xuất hiện mới nhất của chủ đề (1) tiếp tục tiến về phía trước, trong khi chủ đề (2) không bao giờ xuất hiện. 

| Ngày | Chủ đề |`last[1]`|`last[2]`| 
| --- | --- | --- | --- | 
| 1 | 1 | 1 | 0 | 
| 2 | 1 | 2 | 0 | 
| 3 | 1 | 3 | 0 | 
| 4 | 1 | 4 | 0 | 

Chỉ môn học (1) nhận được bài tập về nhà và bài tập cuối cùng của môn đó sẽ đến vào ngày thứ 4. Sau đó, tất cả bốn bài tập có thể được hoàn thành cùng nhau. 

Câu trả lời là```
0 0 0 1
```Điều này chứng tỏ tại sao chỉ ghi lại lần xuất hiện cuối cùng là đủ. Những lần xuất hiện trước đó không cần ngày xử lý riêng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n+k)) | Đầu vào được quét một lần, sau đó tất cả (k) đối tượng được kiểm tra một lần. | 
| Không gian | (O(n+k)) | Trình tự,`last`và mảng trả lời yêu cầu bộ nhớ tuyến tính. | 

Với (n,k\le 10^5), thuật toán chỉ thực hiện vài trăm nghìn thao tác cơ bản. Nó tránh được một cách thoải mái các thao tác (10^{10}) mà phương pháp tìm kiếm lặp lại có thể yêu cầu. 

## Trường hợp thử nghiệm 

Trình trợ giúp kiểm tra bên dưới sử dụng tương tự`solve`logic nhưng trả về kết quả đầu ra được tạo dưới dạng một chuỗi, cho phép kiểm tra các trường hợp bằng các xác nhận Python. Vì nói chung có thể tồn tại một số lịch trình hợp lệ nên các xác nhận này nhắm đến lịch trình xác định được tạo ra bởi quá trình triển khai này, lịch trình này luôn hoạt động dựa trên lần xuất hiện cuối cùng của mỗi chủ đề.```python
import sys
import io

def solution(data: str) -> str:
    lines = data.strip().split()
    it = iter(lines)

    n = int(next(it))
    k = int(next(it))
    a = [int(next(it)) for _ in range(n)]

    last = [0] * (k + 1)

    for i, subject in enumerate(a, start=1):
        last[subject] = i

    answer = [0] * n

    for subject in range(1, k + 1):
        pos = last[subject]
        if pos:
            answer[pos - 1] = subject

    return " ".join(map(str, answer))

def run(inp: str) -> str:
    return solution(inp)

assert run("""3 3
1 2 3
""") == "1 2 3", "sample 1"

assert run("""4 2
1 1 1 1
""") == "0 0 0 1", "sample 2"

assert run("""1 1
1
""") == "1", "minimum size"

assert run("""3 5
2 2 2
""") == "0 0 2", "unused subjects"

assert run("""5 3
1 2 1 3 2
""") == "0 0 1 3 2", "multiple repeated subjects"

assert run("""6 3
1 2 3 1 2 3
""") == "0 0 0 1 2 3", "all subjects repeat"

n = 100000
a = " ".join(["1"] * n)
expected = " ".join(["0"] * (n - 1) + ["1"])
assert run(f"{n} 100000\n{a}\n") == expected, "maximum size"

print("All tests passed.")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 1`|`1`| Đầu vào tối thiểu và một bài tập duy nhất | 
|`3 5 / 2 2 2`|`0 0 2`| Những chủ đề không bao giờ xảy ra | 
|`5 3 / 1 2 1 3 2`|`0 0 1 3 2`| Một số môn học có vị trí cuối cùng khác nhau | 
|`6 3 / 1 2 3 1 2 3`|`0 0 0 1 2 3`| Lặp đi lặp lại và trì hoãn | 
| (n=100000), tất cả các giá trị bằng`1`| (99999) số 0 theo sau là`1`| Kích thước đầu vào tối đa và trường hợp hoàn toàn bằng nhau | 

## Vỏ cạnh 

Khi một chủ đề xuất hiện đúng một lần thì lần xuất hiện cuối cùng của nó cũng là lần xuất hiện đầu tiên. Vì```
1 1
1
```bộ quét`last[1] = 1`, và câu trả lời trở thành`1`. Không có cách nào để trì hoãn công việc quá ngày 1 nên việc thi công mang tính bắt buộc và tối ưu. 

Khi một chủ đề xuất hiện nhiều lần, những lần xuất hiện trước đó sẽ bị lãng quên một cách có chủ ý. Vì```
4 2
1 1 1 1
```các giá trị được lưu trữ cho chủ đề (1) lần lượt là (1,2,3,4), để lại`last[1] = 4`sau khi quét. Do đó, câu trả lời chỉ chứa một thao tác,`0 0 0 1`. Tất cả bốn bài tập đều đang chờ xử lý vào ngày thứ 4 và được hoàn thành cùng nhau. 

Khi một số chủ đề không bao giờ xảy ra, chúng`last`các mục vẫn bằng không. Vì```
3 5
2 2 2
```chúng tôi kết thúc với`last[1] = 0`,`last[2] = 3`, Và`last[3] = last[4] = last[5] = 0`. Chỉ vị trí 3 nhận được giá trị`2`, sản xuất`0 0 2`. Thuật toán không bao giờ phát minh ra tác phẩm cho một chủ đề vắng mặt. 

Khi sự xuất hiện cuối cùng của một chủ đề là vào ngày cuối cùng, hoạt động vẫn hợp pháp vì tất cả các nhiệm vụ cho chủ đề đó đã đến vào thời điểm đó. Vì```
3 2
1 2 2
```các vị trí cuối cùng là`last[1] = 1`Và`last[2] = 3`, vì vậy đầu ra là`1 0 2`. Chủ đề (1) được hoàn thành trong lần duy nhất, trong khi cả hai bài tập cho chủ đề (2) đều được hoàn thành cùng nhau vào ngày thứ 3. 

Ranh giới giữa hai chủ thể lặp đi lặp lại cũng không gây ra sự xử lý đặc biệt nào. Vì```
6 3
1 2 3 1 2 3
```vị trí cuối cùng là (4,5,6), cho`0 0 0 1 2 3`. Ba bài tập đầu tiên vẫn đang chờ xử lý trong khi Dima chờ đợi, sau đó mỗi môn học được hoàn thành đúng một lần sau bài tập cuối cùng. 

Thuộc tính trung tâm tồn tại trong tất cả các trường hợp này: mọi chủ đề xuất hiện sẽ được xử lý một lần, ở lần xuất hiện cuối cùng và mọi chủ đề không xuất hiện sẽ được xử lý 0 lần. Vì mỗi chủ đề xuất hiện nhất thiết phải yêu cầu ít nhất một ngày làm việc nên không có lịch trình hợp lệ nào có thể sử dụng ít ngày làm việc hơn việc xây dựng này.
