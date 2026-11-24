---
title: "CF 81C - Điểm trung bình"
description: "Chúng ta được cho một chuỗi điểm, nhưng chủ đề của mỗi điểm đã bị mất. Chỉ có hai môn và chúng tôi biết chính xác mỗi môn có bao nhiêu điểm. Nếu môn 1 phải nhận điểm thì môn 2 tự động nhận điểm b=n - a."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "math", "sortings"]
categories: ["algorithms"]
codeforces_contest: 81
codeforces_index: "C"
codeforces_contest_name: "Yandex.Algorithm Open 2011: Qualification 1"
rating: 1700
weight: 81
solve_time_s: 211
verified: true
draft: false
---

[CF 81C - Điểm trung bình](https://codeforces.com/problemset/problem/81/C) 

**Đánh giá:** 1700 
**Tags:** tham lam, toán học, phân loại 
**Thời gian giải:** 3 phút 31s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một chuỗi điểm, nhưng chủ đề của mỗi điểm đã bị mất. Chỉ có hai môn và chúng tôi biết chính xác mỗi môn có bao nhiêu điểm. Nếu đối tượng 1 phải nhận`a`điểm thì môn 2 tự động nhận`b = n - a`dấu vết. 

Nhiệm vụ của chúng ta là chấm từng điểm cho một trong hai môn sao cho tổng điểm trung bình của hai môn càng lớn càng tốt. Trong số tất cả các bài tập tối ưu, chúng ta phải xuất ra chuỗi nhãn chủ đề nhỏ nhất về mặt từ điển. 

Số trung bình là:$$\frac{\text{sum of subject 1 marks}}{a} + \frac{\text{sum of subject 2 marks}}{b}$$Bản thân các vết này rất nhỏ, chỉ từ 1 đến 5, nhưng số lượng vết có thể lên tới$10^5$. Điều đó ngay lập tức loại trừ bất kỳ tìm kiếm theo cấp số nhân nào trên các bài tập. Thậm chí$O(n^2)$sẽ có rủi ro ở quy mô này trong Python. Chúng ta cần một cái gì đó gần gũi$O(n \log n)$, thường gợi ý việc sắp xếp hoặc xử lý tham lam. 

Phần khó khăn là việc tối đa hóa tổng điểm trung bình không phải là yêu cầu duy nhất. Nếu một số phép gán đạt được cùng một giá trị lớn nhất, chúng ta phải chọn phép gán nhỏ nhất về mặt từ điển. Một giải pháp chỉ tối ưu hóa mức trung bình mà bỏ qua sự ràng buộc có thể âm thầm thất bại. 

Hãy xem xét ví dụ này:```
n = 4
a = 2
b = 2
marks = [5, 1, 5, 1]
```Nếu chúng ta đặt cả hai điểm 5 vào cùng một chủ đề thì điểm trung bình sẽ là:```
(5 + 5) / 2 + (1 + 1) / 2 = 5 + 1 = 6
```Nếu chúng ta chia đều chúng:```
(5 + 1) / 2 + (5 + 1) / 2 = 3 + 3 = 6
```Cả hai đều tối ưu. Nhưng nhiệm vụ nhỏ nhất về mặt từ điển sẽ ưu tiên các vị trí trước đó được gắn nhãn`1`. Một chiến lược tham lam bất cẩn chỉ sắp xếp theo giá trị có thể tạo ra thứ tự sai. 

Một trường hợp tế nhị khác xuất hiện khi một chủ đề nhỏ hơn nhiều so với chủ đề kia:```
n = 5
a = 1
b = 4
marks = [1, 2, 3, 4, 5]
```Chiến lược tối ưu là đặt dấu lớn nhất vào mẫu số nhỏ hơn, vì mỗi điểm ở đó đóng góp nhiều hơn vào biểu thức cuối cùng:```
5 / 1 + (1 + 2 + 3 + 4) / 4 = 5 + 2.5 = 7.5
```Trực giác “cân bằng các chủ đề” ngây thơ sẽ thất bại nặng nề ở đây. 

Một cạm bẫy nữa đến từ điểm bằng nhau. Giả định:```
n = 5
a = 2
b = 3
marks = [4, 4, 4, 4, 4]
```Mỗi bài tập đều có số điểm chính xác như nhau. Toàn bộ vấn đề được rút gọn thành việc tạo ra chuỗi hợp lệ nhỏ nhất về mặt từ điển. Câu trả lời đúng là:```
1 1 2 2 2
```bởi vì chúng tôi muốn những vị trí sớm nhất có thể được giao cho chủ đề 1. 

## Phương pháp tiếp cận 

Giải pháp brute-force rất đơn giản về mặt khái niệm. Chúng tôi thử mọi lựa chọn có thể trong đó`a`vị trí thuộc môn 1, tính hai điểm trung bình và giữ nguyên bài tập tốt nhất. Vì có:$$\binom{n}{a}$$những nhiệm vụ có thể, điều này trở nên hoàn toàn không khả thi ngay cả đối với những người vừa phải`n`. Ví dụ, với`n = 40`Và`a = 20`, số lượng khả năng đã vượt quá$10^{11}$. Với`n = 10^5`, tìm kiếm toàn diện là không thể. 

Quan sát chính xuất phát từ việc viết lại hàm mục tiêu. Giả sử một dấu hiệu`x`được giao cho chủ đề 1. Đóng góp của nó vào tổng số điểm là:$$\frac{x}{a}$$Nếu điểm tương tự được gán cho chủ đề 2, phần đóng góp của nó sẽ trở thành:$$\frac{x}{b}$$Vì vậy, mỗi nhãn hiệu đều đóng góp một cách độc lập`x/a`hoặc`x/b`. 

Điều đó có nghĩa là vấn đề thực sự đang đặt ra: 

“Điểm nào sẽ nhận được hệ số lớn hơn?” 

Nếu như`a < b`, sau đó:$$\frac{1}{a} > \frac{1}{b}$$vì vậy điểm lớn hơn nên chuyển sang chủ đề 1. Nếu`b < a`, thì điểm lớn hơn sẽ chuyển sang chủ đề 2. 

Điều này biến vấn đề thành một vấn đề sắp xếp tham lam. Nhóm nhỏ hơn sẽ nhận được số điểm lớn nhất. 

Sau đó, yêu cầu về từ điển trở thành thách thức còn lại. Giả sử chủ đề 1 là nhóm nhỏ hơn và sẽ nhận được điểm cao nhất. Trong số các điểm bằng nhau, việc đưa ra chỉ số sớm hơn cho chủ đề 1 sẽ tạo ra câu trả lời nhỏ hơn về mặt từ điển. Vì vậy, khi các giá trị hòa nhau, chúng ta nên ưu tiên các chỉ số nhỏ hơn. 

Nếu chủ thể 2 là nhóm nhỏ hơn thì điều ngược lại xảy ra. Vì chúng tôi muốn các vị trí trước đó trở thành`1`bất cứ khi nào có thể, chúng ta nên trì hoãn việc xếp hạng cho môn 2. Vì vậy, trong số điểm bằng nhau, các chỉ số về sau nên dành cho môn 2. 

Quy tắc ràng buộc duy nhất đó là toàn bộ sự tinh vi của vấn đề. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(\binom{n}{a} \cdot n)$|$O(n)$| Quá chậm | 
| Tham lam tối ưu + Sắp xếp |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các nhãn hiệu cùng với chỉ số ban đầu của chúng. 
2. Xác định chủ đề nào có kích thước nhỏ hơn. 

Môn học nhỏ hơn có hệ số lớn hơn trong hàm mục tiêu nên sẽ nhận được điểm lớn nhất. 
3. Sắp xếp các điểm theo giá trị theo thứ tự giảm dần. 

Điều này đặt những điểm có giá trị nhất lên hàng đầu, đó chính xác là những gì mà sự lựa chọn tham lam cần. 
4. Xử lý các giá trị bằng nhau một cách cẩn thận theo thứ tự từ điển. 

Nếu chủ đề 1 là nhóm nhỏ hơn thì giữa các giá trị bằng nhau chúng ta sắp xếp các chỉ số theo thứ tự tăng dần. Các vị trí trước đó trở thành`1`, làm cho chuỗi cuối cùng nhỏ hơn về mặt từ điển. 

Nếu chủ đề 2 là nhóm nhỏ hơn thì giữa các giá trị bằng nhau chúng ta sắp xếp các chỉ số theo thứ tự giảm dần. Điều này làm trì hoãn việc gán`2`đến các vị trí trước đó, một lần nữa giảm thiểu trình tự cuối cùng về mặt từ điển. 
5. Lấy cái đầu tiên`min(a, b)`các phần tử theo thứ tự được sắp xếp và gán chúng cho chủ đề nhỏ hơn. 

Tất cả các vị trí còn lại thuộc về chủ đề lớn hơn. 
6. Xây dựng dãy câu trả lời cuối cùng theo thứ tự ban đầu và in ra. 

### Tại sao nó hoạt động 

Hàm mục tiêu là tuyến tính:$$\sum \frac{x_i}{a} + \sum \frac{y_j}{b}$$Mỗi điểm độc lập đạt được hệ số`1/a`hoặc`1/b`. Hệ số lớn hơn luôn thuộc về kích thước chủ thể nhỏ hơn. Vì vậy mọi giải pháp tối ưu đều phải đặt điểm lớn nhất vào chủ đề nhỏ hơn. 

Sau khi các giá trị được cố định, các điểm bằng nhau sẽ đóng góp như nhau bất kể vị trí. Sự tự do duy nhất còn lại là trật tự từ điển. Việc gán các chỉ số trước đó cho chủ đề 1 sẽ giảm thiểu trình tự, bởi vì`1 < 2`. Các quy tắc ràng buộc ở trên đạt được chính xác điều đó trong khi vẫn duy trì tính tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a, b = map(int, input().split())
    t = list(map(int, input().split()))

    arr = []

    if a <= b:
        # Subject 1 is smaller
        for i, x in enumerate(t):
            arr.append((-x, i))

        arr.sort()

        ans = [2] * n

        for j in range(a):
            _, idx = arr[j]
            ans[idx] = 1

    else:
        # Subject 2 is smaller
        for i, x in enumerate(t):
            arr.append((-x, -i))

        arr.sort()

        ans = [1] * n

        for j in range(b):
            _, neg_idx = arr[j]
            idx = -neg_idx
            ans[idx] = 2

    print(*ans)

solve()
```Giải pháp bắt đầu bằng cách ghép từng dấu với vị trí ban đầu của nó. Chúng ta không thể mất các chỉ số vì đầu ra phải tuân theo thứ tự ban đầu. 

Hai nhánh tương ứng với chủ đề nào nhỏ hơn. Nếu môn 1 nhỏ hơn, chúng ta muốn nó được điểm lớn nhất. Sắp xếp theo`(-value, index)`đưa ra các giá trị giảm dần và các chỉ số tăng dần cho các mối quan hệ. Điều đó tự động tạo ra bài tập nhỏ nhất về mặt từ điển. 

Nhánh thứ hai tinh tế hơn một chút. Khi chủ đề 2 nhỏ hơn, chúng tôi vẫn muốn điểm lớn nhất ở đó, nhưng bây giờ việc gán vị trí sớm cho chủ đề 2 sẽ làm ảnh hưởng đến thứ tự từ điển vì`2`lớn hơn`1`. Sắp xếp theo`(-value, -index)`làm cho các vị trí bằng nhau sau này được chọn trước. 

Mảng câu trả lời được khởi tạo hoàn toàn với nhãn chủ đề lớn hơn. Sau đó chúng ta ghi đè chính xác số lượng vị trí cần thiết cho chủ đề nhỏ hơn. 

Việc sử dụng các giá trị âm trong quá trình sắp xếp sẽ tránh việc viết các bộ so sánh tùy chỉnh và giúp quá trình triển khai trở nên ngắn gọn và nhanh chóng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
3 2
4 4 5 4 4
```Từ`b = 2`nhỏ hơn thì môn 2 sẽ nhận được điểm lớn nhất. 

Thứ tự sắp xếp: 

| Giá trị | Chỉ mục | 

|---|---|---| 

| 5 | 2 | 

| 4 | 4 | 

| 4 | 3 | 

| 4 | 1 | 

| 4 | 0 | 

Chúng tôi sử dụng chỉ số giảm dần giữa các giá trị bằng nhau vì chủ đề 2 là chỉ số nhỏ hơn. 

Hai vị trí đầu tiên thuộc về chủ đề 2. 

| Vị trí | Chủ đề được giao | 
| --- | --- | 
| 2 | 2 | 
| 4 | 2 | 

Tất cả những người khác trở thành chủ đề 1. 

Câu trả lời cuối cùng:```
1 1 2 1 2
```Dấu vết này thể hiện sự ràng buộc về mặt từ điển. Trong số 4 bằng nhau, chỉ số sau`4`được chọn trước các chỉ số trước đó, giữ phần đầu của câu trả lời càng nhỏ càng tốt. 

### Ví dụ 2 

đầu vào:```
5
1 4
1 2 3 4 5
```Môn 1 nhỏ hơn nên sẽ nhận được điểm lớn nhất. 

Thứ tự sắp xếp: 

| Giá trị | Chỉ mục | 
| --- | --- | 
| 5 | 4 | 
| 4 | 3 | 
| 3 | 2 | 
| 2 | 1 | 
| 1 | 0 | 

Phần tử đầu tiên thuộc chủ đề 1. 

| Vị trí | Chủ đề được giao | 
| --- | --- | 
| 4 | 1 | 

Tất cả những người khác trở thành chủ thể 2. 

Câu trả lời cuối cùng:```
2 2 2 2 1
```Giá trị lớn nhất có hệ số lớn hơn`1/a = 1`, tối đa hóa tổng số điểm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Thống trị bằng cách sắp xếp các nhãn hiệu | 
| Không gian |$O(n)$| Mảng chỉ số và câu trả lời cuối cùng | 

Với$n \le 10^5$, việc sắp xếp có thể dễ dàng đủ nhanh trong giới hạn 2 giây. Việc sử dụng bộ nhớ là tuyến tính và vừa vặn thoải mái trong 256 MB. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    def solve():
        n = int(input())
        a, b = map(int, input().split())
        t = list(map(int, input().split()))

        arr = []

        if a <= b:
            for i, x in enumerate(t):
                arr.append((-x, i))

            arr.sort()

            ans = [2] * n

            for j in range(a):
                _, idx = arr[j]
                ans[idx] = 1

        else:
            for i, x in enumerate(t):
                arr.append((-x, -i))

            arr.sort()

            ans = [1] * n

            for j in range(b):
                _, neg_idx = arr[j]
                idx = -neg_idx
                ans[idx] = 2

        return " ".join(map(str, ans))

    return solve()

# provided sample
assert run(
    "5\n3 2\n4 4 5 4 4\n"
) == "1 1 2 1 2", "sample 1"

# minimum size
assert run(
    "2\n1 1\n5 1\n"
) == "1 2", "minimum case"

# all equal values
assert run(
    "5\n2 3\n4 4 4 4 4\n"
) == "1 1 2 2 2", "lexicographic tie-breaking"

# smaller second group
assert run(
    "5\n4 1\n1 2 3 4 5\n"
) == "1 1 1 1 2", "largest value goes to smaller group"

# equal top values with lexicographic issue
assert run(
    "4\n2 2\n5 1 5 1\n"
) == "1 2 1 2", "correct tie handling"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 / 1 1 / 5 1`|`1 2`| Đầu vào hợp lệ tối thiểu | 
| Tất cả các điểm bằng nhau |`1 1 2 2 2`| Thứ tự từ điển giữa các giải pháp tương đương | 
|`a = 4, b = 1`|`1 1 1 1 2`| Nhóm nhỏ hơn đạt điểm cao nhất | 
| Hai giá trị tối đa bằng nhau |`1 2 1 2`| Phá vỡ chính xác với các giá trị bằng nhau | 

## Vỏ cạnh 

Hãy xem xét trường hợp tất cả các nhãn hiệu giống hệt nhau:```
5
2 3
4 4 4 4 4
```Mọi phép gán đều tạo ra tổng trung bình như nhau vì tất cả các giá trị đều bằng nhau. Thuật toán đi vào nhánh`a <= b`, sắp xếp theo`(value descending, index ascending)`và gán hai vị trí đầu tiên cho chủ đề 1. Kết quả đầu ra trở thành:```
1 1 2 2 2
```Đây là phép gán hợp lệ nhỏ nhất về mặt từ điển vì các vị trí sớm nhất có thể nhận được nhãn`1`. 

Bây giờ hãy xem xét sự mất cân bằng ngược lại:```
5
1 4
1 2 3 4 5
```Môn 1 có kích thước 1 nên hệ số của nó lớn nhất. Thuật toán gán giá trị tối đa duy nhất`5`sang chủ đề 1. Bất kỳ lựa chọn nào khác sẽ làm giảm giá trị khách quan. Bài tập được tạo ra là:```
2 2 2 2 1
```Cuối cùng, hãy kiểm tra trường hợp ràng buộc có giá trị bằng nhau tinh tế:```
4
2 2
5 1 5 1
```Cả hai môn đều có quy mô bằng nhau, vì vậy việc tối đa hóa điểm số không quan tâm 5 sẽ đi về đâu. Câu trả lời nhỏ nhất về mặt từ điển nên gán 5 câu trước cho chủ đề 1:```
1 2 1 2
```Bởi vì việc sắp xếp sử dụng các chỉ số tăng dần cho các giá trị bằng nhau khi chủ đề 1 được chọn nên thuật toán sẽ tự động tạo ra thứ tự chính xác.
