---
title: "CF 155A - I_love_\\%tên người dùng\\%"
description: "Chúng tôi được cung cấp điểm thi theo trình tự thời gian của một lập trình viên. Một màn trình diễn được gọi là \"tuyệt vời\" khi điểm hiện tại lớn hơn mọi điểm trước đó hoặc nhỏ hơn mọi điểm trước đó."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force"]
categories: ["algorithms"]
codeforces_contest: 155
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 109 (Div. 2)"
rating: 800
weight: 155
solve_time_s: 264
verified: false
draft: false
---

[CF 155A - I_love_\\%username\\%](https://codeforces.com/problemset/problem/155/A) 

**Đánh giá:** 800 
**Tags:** vũ lực 
**Thời gian giải:** 4 phút 24s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp điểm thi theo trình tự thời gian của một lập trình viên. Một màn trình diễn được gọi là "tuyệt vời" khi điểm hiện tại lớn hơn mọi điểm trước đó hoặc nhỏ hơn mọi điểm trước đó. 

Cuộc thi đầu tiên không bao giờ được tính vì không có cuộc thi nào trước đó để so sánh. 

Nhiệm vụ là xử lý trình tự và đếm xem người lập trình đặt ra điểm cá nhân tốt nhất hoặc điểm tệ nhất cá nhân mới bao nhiêu lần. 

Ví dụ, hãy xem xét:```
100 50 200 150 200
```Điểm đầu tiên khởi tạo cả bản ghi tốt nhất và tệ nhất. 

Điểm số thứ hai,`50`, thấp hơn mọi điểm trước đó, vì vậy nó phá kỷ lục tệ nhất. 

Điểm thứ ba,`200`, cao hơn mọi điểm trước đó, vì vậy nó phá kỷ lục tốt nhất. 

Các số điểm còn lại đều không phá được kỷ lục nào. 

Câu trả lời là`2`. 

Những hạn chế là rất nhỏ. Số lượng cuộc thi nhiều nhất`1000`, vì vậy ngay cả một`O(n²)`giải pháp sẽ trôi qua thoải mái trong thời hạn. Tuy nhiên, vấn đề này có một giải pháp tuyến tính rất rõ ràng, quét mảng một lần trong khi chỉ duy trì hai giá trị. 

Phần khó khăn là xử lý sự bình đẳng một cách chính xác. Điểm chỉ được tính nếu nó lớn hơn mức tối đa hiện tại hoặc nhỏ hơn mức tối thiểu hiện tại. 

Hãy xem xét đầu vào này:```
4
10 10 10 10
```Câu trả lời đúng là`0`. 

Việc thực hiện bất cẩn bằng cách sử dụng`>=`hoặc`<=`sẽ tính không chính xác các giá trị lặp lại là thành tích phá kỷ lục. 

Một trường hợp khó phát hiện khác là khi các bản ghi luân phiên lặp đi lặp lại:```
5
100 50 200 25 300
```Câu trả lời đúng là`4`. 

Mỗi điểm mới phá vỡ mức tối thiểu hoặc tối đa hiện tại và cả hai bản ghi phải được cập nhật ngay sau khi đếm sự kiện. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực trực tiếp sẽ kiểm tra mọi cuộc thi so với tất cả các cuộc thi trước đó. 

Dành cho cuộc thi`i`, chúng tôi có thể quét các cuộc thi`0`bởi vì`i-1`và xác định xem điểm hiện tại lớn hơn tất cả các điểm trước đó hay nhỏ hơn tất cả các điểm trước đó. 

Điều này có hiệu quả vì định nghĩa về một màn trình diễn xuất sắc chỉ phụ thuộc vào các cuộc thi trước đó. Logic rất đơn giản, nhưng mỗi vị trí có thể so sánh với tối đa`n-1`các yếu tố trước đó, dẫn đến`O(n²)`sự phức tạp về mặt thời gian. 

Với`n = 1000`, điều này vẫn chạy dễ dàng trong giới hạn. Trong trường hợp xấu nhất, chúng tôi thực hiện khoảng một triệu so sánh, điều này không đáng kể đối với phần cứng hiện đại. 

Cách tiếp cận nhanh hơn xuất phát từ việc quan sát rằng chúng ta không thực sự cần tất cả các điểm số trước đó. Tại bất kỳ thời điểm nào, chỉ có hai giá trị quan trọng:```
current maximum score
current minimum score
```Nếu điểm tiếp theo lớn hơn điểm tối đa hiện tại, nó sẽ phá vỡ kỷ lục hiệu suất tốt nhất. 

Nếu nó nhỏ hơn mức tối thiểu hiện tại, nó sẽ phá vỡ kỷ lục hiệu suất kém nhất. 

Sau khi xử lý điểm, chúng ta cập nhật bản ghi tương ứng. 

Điều này làm giảm vấn đề chỉ bằng một lần quét từ trái sang phải với bộ nhớ bổ sung liên tục. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Đã chấp nhận | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số bài thi và danh sách điểm. 
2. Khởi tạo cả hai`best`Và`worst`với điểm số đầu tiên. 

Cuộc thi đầu tiên xác lập những kỷ lục ban đầu và không bao giờ được coi là đáng kinh ngạc. 
3. Khởi tạo`answer = 0`. 
4. Lặp lại điểm số bắt đầu từ cuộc thi thứ hai. 
5. Đối với từng điểm: 

Nếu số điểm thực sự lớn hơn`best`, tăng`answer`và cập nhật`best`. 

Nếu điểm thực sự nhỏ hơn`worst`, tăng`answer`và cập nhật`worst`. 
6. Sau khi xử lý tất cả điểm, in`answer`. 

### Tại sao nó hoạt động 

Ở mỗi bước,`best`lưu trữ số điểm lớn nhất được thấy cho đến nay và`worst`lưu trữ số điểm nhỏ nhất được thấy cho đến nay. 

Khi chúng tôi xử lý điểm mới, chỉ có ba khả năng:```
score > best
score < worst
worst <= score <= best
```Trường hợp đầu tiên có nghĩa là lập trình viên đã đạt được điểm cao nhất mới. Trường hợp thứ hai có nghĩa là điểm thấp nhất mới. Trường hợp thứ ba có nghĩa là không có kỷ lục nào bị phá vỡ. 

Bởi vì thuật toán cập nhật các bản ghi ngay sau khi đếm một bản ghi mới nên tính bất biến vẫn đúng trong suốt quá trình quét. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    scores = list(map(int, input().split()))

    best = scores[0]
    worst = scores[0]
    answer = 0

    for score in scores[1:]:
        if score > best:
            answer += 1
            best = score
        elif score < worst:
            answer += 1
            worst = score

    print(answer)

solve()
```Giải pháp bắt đầu bằng cách đọc dữ liệu đầu vào và khởi tạo cả hai giá trị bản ghi bằng điểm thi đầu tiên. 

Vòng lặp bắt đầu từ phần tử thứ hai vì cuộc thi đầu tiên không thể phá vỡ bất kỳ kỷ lục nào trước đó. 

các`if`câu lệnh kiểm tra xem điểm hiện tại có vượt quá điểm tốt nhất được thấy cho đến nay hay không. Nếu đúng như vậy, chúng tôi sẽ tính hiệu suất đáng kinh ngạc và cập nhật ở mức tối đa. 

các`elif`xử lý theo hướng ngược lại. sử dụng`elif`thay vì độc lập thứ hai`if`hợp lý hơn vì một số không thể đồng thời lớn hơn mức tối đa hiện tại và nhỏ hơn mức tối thiểu hiện tại. 

Sự so sánh chặt chẽ là cần thiết. Điểm bằng nhau không được tính là thành tích phá kỷ lục. 

Giải pháp chỉ sử dụng bộ nhớ bổ sung không đổi vì nó chỉ lưu trữ ba số nguyên ngoài mảng đầu vào. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
100 50 200 150 200
```| Cuộc thi | Điểm | Tốt Nhất Trước | Tệ nhất trước đây | Tuyệt vời? | Tốt Nhất Sau | Sau tệ nhất | Đếm | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | 100 | - | - | Không | 100 | 100 | 0 | 
| 2 | 50 | 100 | 100 | Có | 100 | 50 | 1 | 
| 3 | 200 | 100 | 50 | Có | 200 | 50 | 2 | 
| 4 | 150 | 200 | 50 | Không | 200 | 50 | 2 | 
| 5 | 200 | 200 | 50 | Không | 200 | 50 | 2 | 

Dấu vết này cho thấy cách thuật toán duy trì hoạt động tối thiểu và tối đa. Giá trị bằng nhau, như cuối cùng`200`, không tính vì sự so sánh rất nghiêm ngặt. 

### Ví dụ 2 

đầu vào:```
10
1 2 3 4 5 4 3 2 1 0
```| Cuộc thi | Điểm | Tốt Nhất Trước | Tệ nhất trước đây | Tuyệt vời? | Tốt Nhất Sau | Sau tệ nhất | Đếm | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | 1 | - | - | Không | 1 | 1 | 0 | 
| 2 | 2 | 1 | 1 | Có | 2 | 1 | 1 | 
| 3 | 3 | 2 | 1 | Có | 3 | 1 | 2 | 
| 4 | 4 | 3 | 1 | Có | 4 | 1 | 3 | 
| 5 | 5 | 4 | 1 | Có | 5 | 1 | 4 | 
| 6 | 4 | 5 | 1 | Không | 5 | 1 | 4 | 
| 7 | 3 | 5 | 1 | Không | 5 | 1 | 4 | 
| 8 | 2 | 5 | 1 | Không | 5 | 1 | 4 | 
| 9 | 1 | 5 | 1 | Không | 5 | 1 | 4 | 
| 10 | 0 | 5 | 1 | Có | 5 | 0 | 5 | 

Ví dụ này chứng minh rằng các kỷ lục đang chạy tiếp tục có vai trò quan trọng trong toàn bộ lịch sử chứ không chỉ các cuộc thi liền kề. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi điểm được xử lý một lần | 
| Không gian | O(1) | Chỉ có một vài biến được lưu trữ | 

Với nhiều nhất`1000`các cuộc thi, giải pháp tuyến tính dễ dàng đủ nhanh. Việc sử dụng bộ nhớ không đáng kể vì thuật toán chỉ theo dõi điểm tốt nhất hiện tại, điểm kém nhất hiện tại và bộ đếm câu trả lời. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    n = int(input())
    scores = list(map(int, input().split()))

    best = scores[0]
    worst = scores[0]
    answer = 0

    for score in scores[1:]:
        if score > best:
            answer += 1
            best = score
        elif score < worst:
            answer += 1
            worst = score

    print(answer)

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    output = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return output

# provided samples
assert run("5\n100 50 200 150 200\n") == "2\n", "sample 1"

assert run("10\n1 2 3 4 5 4 3 2 1 0\n") == "5\n", "sample 2"

# minimum size
assert run("1\n100\n") == "0\n", "single contest"

# all equal values
assert run("4\n10 10 10 10\n") == "0\n", "equal scores are not amazing"

# alternating new records
assert run("5\n100 50 200 25 300\n") == "4\n", "multiple alternating records"

# decreasing sequence
assert run("5\n9 8 7 6 5\n") == "4\n", "every contest breaks worst record"

# off-by-one style check
assert run("3\n5 10 10\n") == "1\n", "equal maximum should not count twice"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 100`|`0`| Cuộc thi đầu tiên không bao giờ được tính | 
|`10 10 10 10`|`0`| Điểm bằng nhau bị bỏ qua | 
|`100 50 200 25 300`|`4`| Hồ sơ có thể luân phiên nhiều lần | 
|`9 8 7 6 5`|`4`| Mọi điểm thấp hơn đều phá vỡ mức tối thiểu | 
|`5 10 10`|`1`| Mức tối đa bằng nhau không được tính lại | 

## Vỏ cạnh 

Hãy xem xét trường hợp tất cả các điểm đều giống hệt nhau:```
4
10 10 10 10
```Thuật toán bắt đầu bằng:```
best = 10
worst = 10
answer = 0
```Mọi điểm còn lại đều bằng cả hai bản ghi nên không có điều kiện nào được kích hoạt. 

Câu trả lời cuối cùng vẫn còn`0`, điều này đúng vì không có sự cải thiện hay suy giảm nghiêm trọng nào xảy ra. 

Bây giờ hãy xem xét các giá trị tối đa lặp lại:```
3
5 10 10
```Sau khi xử lý điểm thứ hai:```
best = 10
answer = 1
```Điểm thứ ba bằng`best`, nhưng không vượt quá nó. Vì sự so sánh là`>`còn hơn là`>=`, thuật toán sẽ tránh đếm lại một cách chính xác. 

Cuối cùng, hãy xem xét thay đổi bản ghi luân phiên:```
5
100 50 200 25 300
```Trình tự các bản ghi trở thành:```
100 -> new minimum 50 -> new maximum 200 -> new minimum 25 -> new maximum 300
```Mỗi điểm mới vượt quá một trong các ranh giới hiện tại, vì vậy câu trả lời sẽ tăng lên mỗi lần. Thuật toán xử lý việc này một cách tự nhiên vì nó cập nhật các bản ghi đang chạy ngay sau mỗi màn trình diễn tuyệt vời.
