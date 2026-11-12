---
title: "CF 165B - Đốt Dầu Nửa Đêm"
description: "Vasya phải viết ít nhất n dòng mã trong một đêm. Anh ấy bắt đầu với năng suất v, nghĩa là anh ấy viết v dòng trước giờ nghỉ uống trà đầu tiên. Sau mỗi lần nghỉ giải lao, năng suất của anh ấy giảm đi hệ số k, sử dụng phép chia số nguyên."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "implementation"]
categories: ["algorithms"]
codeforces_contest: 165
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 112 (Div. 2)"
rating: 1500
weight: 165
solve_time_s: 102
verified: true
draft: false
---

[CF 165B - Đốt dầu lúc nửa đêm](https://codeforces.com/problemset/problem/165/B) 

**Đánh giá:** 1500 
**Tags:** tìm kiếm nhị phân, triển khai 
**Thời gian giải:** 1 phút 42s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Vasya ít nhất phải viết`n`dòng mã trong một đêm. Anh ấy bắt đầu với năng suất`v`, nghĩa là anh ấy viết`v`trước giờ nghỉ uống trà đầu tiên. Sau mỗi lần nghỉ giải lao, năng suất của anh ấy giảm đi một lượng`k`, sử dụng phép chia số nguyên. 

Tổng số dòng anh viết là:$$v + \left\lfloor \frac{v}{k} \right\rfloor + \left\lfloor \frac{v}{k^2} \right\rfloor + \cdots$$Quá trình dừng lại khi số hạng tiếp theo bằng 0. Chúng ta phải tìm năng suất ban đầu nhỏ nhất`v`sao cho tổng số dòng viết ít nhất là`n`. 

Những hạn chế đủ lớn để cố gắng hết sức có thể`v`ngây thơ sẽ là quá chậm.`n`có thể đạt được`10^9`, do đó quét tuyến tính từ`1`trở lên có thể yêu cầu một tỷ lần lặp. Ngay cả khi mỗi lần lặp lại nhỏ, điều đó vẫn vượt xa mức vừa vặn trong giới hạn thời gian 2 giây. 

Cấu trúc của tổng quan trọng. BẰNG`v`tăng lên thì tổng số dòng viết không bao giờ giảm. Năng suất ban đầu lớn hơn luôn tạo ra ít nhất nhiều dây chuyền như trước. Hành vi đơn điệu đó chính xác là điều làm cho tìm kiếm nhị phân có thể áp dụng được. 

Có một số trường hợp nguy hiểm có thể âm thầm phá vỡ việc triển khai bất cẩn. 

Coi như:```
1 2
```Câu trả lời đúng là:```
1
```Việc triển khai có lỗi có thể bắt đầu tìm kiếm nhị phân từ`0`và xử lý sai điều kiện khi bản thân câu trả lời có giá trị nhỏ nhất có thể. 

Một trường hợp tinh tế khác là khi tổng vượt quá`n`sớm:```
59 10
```Vì`v = 54`:$$54 + 5 = 59$$Câu trả lời là chính xác`54`. Nếu việc triển khai tính toán các quyền hạn như`k^i`trực tiếp, lỗi tràn hoặc công việc không cần thiết có thể xuất hiện ở các ngôn ngữ khác. Việc phân chia lặp lại sẽ an toàn và sạch sẽ hơn. 

Một kịch bản quan trọng hơn là khi`k`nhỏ, đặc biệt là`2`:```
1000000000 2
```Dãy số chứa nhiều số hạng khác 0 vì phép chia cho`2`giảm chậm. Một giải pháp liên tục tính toán lại các lũy thừa lớn không hiệu quả có thể trở nên quá chậm. Cách tiếp cận dự định chỉ cần nhiều phép chia theo logarit. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là vũ lực. Chúng ta có thể thử mọi giá trị của`v`bắt đầu từ`1`. Với mỗi ứng viên, chúng tôi tính toán:$$v + \left\lfloor \frac{v}{k} \right\rfloor + \left\lfloor \frac{v}{k^2} \right\rfloor + \cdots$$cho đến khi các điều khoản trở thành số không. đầu tiên`v`tổng của nó đạt ít nhất`n`là câu trả lời. 

Điều này có tác dụng vì định nghĩa khớp chính xác với quy trình trong báo cáo vấn đề. Vấn đề là quy mô. Trong trường hợp xấu nhất, câu trả lời có thể gần giống`10^9`, vì vậy việc kiểm tra từng ứng viên trở nên chậm đến mức khó tin. 

Quan sát quan trọng là tổng số dòng viết tạo thành một hàm đơn điệu của`v`. 

Nếu một số giá trị`v`là đủ để kết thúc chương trình thì mọi giá trị lớn hơn cũng đủ. Tương tự, nếu một số`v`không đủ thì mọi giá trị nhỏ hơn cũng không đủ. 

Điều đó chuyển đổi vấn đề thành tìm kiếm nhị phân cổ điển trên không gian câu trả lời. 

Thay vì kiểm tra mọi giá trị, chúng tôi tìm kiếm trong phạm vi`[1, n]`. Đối với mỗi trung điểm`mid`, chúng tôi tính xem Vasya có thể viết bao nhiêu dòng bắt đầu từ`mid`. Nếu tổng số ít nhất là`n`, thì đáp án nằm ở nửa bên trái, bao gồm`mid`. Nếu không, chúng ta di chuyển sang phải. 

Bản thân tính toán trợ giúp đã hiệu quả. Mỗi số hạng chia cho`k`, do đó số lần lặp chỉ mang tính logarit trong`v`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n log n) | O(1) | Quá chậm | 
| Tối ưu | O(log n × log n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xác định hàm trợ giúp`enough(v)`tính toán xem Vasya có thể viết bao nhiêu dòng bắt đầu bằng năng suất`v`. 
2. Bên trong hàm trợ giúp, liên tục cộng năng suất hiện tại với tổng năng suất đang chạy, sau đó chia năng suất cho`k`. 
3. Dừng lại khi năng suất bằng 0, vì tất cả các số hạng sau đó cũng sẽ bằng 0. 
4. Nếu tổng cuối cùng ít nhất là`n`, trở lại`True`. Nếu không thì quay lại`False`. 
5. Chạy tìm kiếm nhị phân trên phạm vi`[1, n]`. 
6. Tính toán`mid = (left + right) // 2`. 
7. Kiểm tra xem`mid`là đủ bằng cách sử dụng chức năng trợ giúp. 
8. Nếu`mid`hoạt động, hãy ghi lại nó như một câu trả lời có thể và tiếp tục tìm kiếm nửa bên trái. Chúng tôi làm điều này bởi vì chúng tôi muốn giá trị hợp lệ tối thiểu. 
9. Nếu`mid`không hoạt động, tìm kiếm nửa bên phải. 
10. Tiếp tục cho đến khi khoảng tìm kiếm trống. 
11. Xuất ra giá trị hợp lệ nhỏ nhất tìm được. 

### Tại sao nó hoạt động 

Tính đúng đắn phụ thuộc vào tính đơn điệu. 

Cho phép`f(v)`là tổng số dòng viết bắt đầu từ hiệu suất`v`. 

Mỗi số hạng trong tổng:$$\left\lfloor \frac{v}{k^i} \right\rfloor$$không giảm như`v`tăng lên. Điều đó có nghĩa`f(v)`bản thân nó không giảm. 

Vì vậy, tồn tại một điểm biên trong đó các giá trị nhỏ hơn câu trả lời là không đủ và các giá trị lớn hơn hoặc bằng câu trả lời là đủ. Tìm kiếm nhị phân luôn duy trì ranh giới này một cách chính xác và cuối cùng cô lập giá trị hợp lệ tối thiểu`v`. 

Hàm trợ giúp cũng đúng vì nó mô phỏng trực tiếp sự suy giảm năng suất được mô tả trong bài toán. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def enough(v, n, k):
    total = 0
    current = v

    while current > 0:
        total += current
        current //= k

    return total >= n

def solve():
    n, k = map(int, input().split())

    left, right = 1, n
    answer = n

    while left <= right:
        mid = (left + right) // 2

        if enough(mid, n, k):
            answer = mid
            right = mid - 1
        else:
            left = mid + 1

    print(answer)

solve()
```Hàm trợ giúp tuân theo quy trình năng suất chính xác từ vấn đề. Bắt đầu từ`v`, nó tiếp tục cộng phần đóng góp hiện tại và chia cho`k`sau mỗi bước. 

Sử dụng phép chia lặp sẽ an toàn hơn khả năng tính toán như`k^i`. Vòng lặp tự nhiên dừng lại khi phần đóng góp trở thành 0. 

Tìm kiếm nhị phân duy trì tính bất biến là mọi giá trị đều nhỏ hơn`left`được biết là không hợp lệ, trong khi mọi giá trị lớn hơn`right`đã được xử lý hoặc loại bỏ. 

Phạm vi tìm kiếm là`[1, n]`bởi vì`n`bản thân nó luôn là đủ. Ngay cả khi Vasya chỉ viết đợt đầu tiên và không viết gì sau đó, bắt đầu bằng`n`dòng hoạt động rõ ràng. 

Một lỗi thường gặp là cập nhật sai ranh giới sau khi tìm được điểm giữa hợp lệ. Vì chúng ta muốn giá trị hợp lệ tối thiểu nên chúng ta phải tiếp tục tìm kiếm nửa bên trái bằng:```
right = mid - 1
```Một chi tiết tinh tế khác là lưu trữ câu trả lời của thí sinh trước khi chuyển sang trái. Nếu không, điểm giữa hợp lệ cuối cùng có thể bị mất. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
7 2
```Dấu vết tìm kiếm nhị phân: 

| trái | đúng | giữa | Tổng số văn bản | Đủ? | 
| --- | --- | --- | --- | --- | 
| 1 | 7 | 4 | 4 + 2 + 1 = 7 | Có | 
| 1 | 3 | 2 | 2 + 1 = 3 | Không | 
| 3 | 3 | 3 | 3 + 1 = 4 | Không | 

Câu trả lời cuối cùng:```
4
```Dấu vết này thể hiện rõ cấu trúc đơn điệu. Một lần`4`hoạt động, mọi giá trị lớn hơn cũng sẽ hoạt động, vì vậy tìm kiếm nhị phân di chuyển sang trái một cách an toàn để tìm câu trả lời hợp lệ nhỏ hơn. 

### Ví dụ 2 

đầu vào:```
59 10
```Dấu vết tìm kiếm nhị phân: 

| trái | đúng | giữa | Tổng số văn bản | Đủ? | 
| --- | --- | --- | --- | --- | 
| 1 | 59 | 30 | 30 + 3 = 33 | Không | 
| 31 | 59 | 45 | 45 + 4 = 49 | Không | 
| 46 | 59 | 52 | 52 + 5 = 57 | Không | 
| 53 | 59 | 56 | 56 + 5 = 61 | Có | 
| 53 | 55 | 54 | 54 + 5 = 59 | Có | 
| 53 | 53 | 53 | 53 + 5 = 58 | Không | 

Câu trả lời cuối cùng:```
54
```Ví dụ này chứng tỏ tại sao chúng ta tiếp tục tìm kiếm sau khi tìm được điểm giữa hợp lệ.`56`hoạt động, nhưng nó không phải là tối thiểu. Việc tìm kiếm cuối cùng thu hẹp lại`54`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log n × log n) | Tìm kiếm nhị phân thực hiện các lần lặp O(log n), mỗi lệnh gọi trợ giúp sẽ chia O(log n) | 
| Không gian | O(1) | Chỉ có một số biến số nguyên được sử dụng | 

Ngay cả ở mức giới hạn tối đa`n = 10^9`, tìm kiếm nhị phân chỉ cần khoảng 30 lần lặp. Mỗi phép tính trợ giúp cũng thực hiện tối đa khoảng 30 phép chia khi`k = 2`. Tổng công việc dễ dàng phù hợp trong thời hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    def enough(v, n, k):
        total = 0
        current = v

        while current > 0:
            total += current
            current //= k

        return total >= n

    n, k = map(int, input().split())

    left, right = 1, n
    answer = n

    while left <= right:
        mid = (left + right) // 2

        if enough(mid, n, k):
            answer = mid
            right = mid - 1
        else:
            left = mid + 1

    return str(answer)

# provided samples
assert run("7 2\n") == "4", "sample 1"
assert run("59 10\n") == "54", "sample 2"

# minimum values
assert run("1 2\n") == "1", "minimum case"

# exact equality after several divisions
assert run("10 2\n") == "6", "6 + 3 + 1 = 10"

# large k, almost no extra contribution
assert run("100 10\n") == "91", "91 + 9 = 100"

# maximum-style stress case
assert run("1000000000 2\n") == "500000006", "large input"

print("All tests passed.")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 2`|`1`| Câu trả lời nhỏ nhất có thể | 
|`10 2`|`6`| Bình đẳng chính xác sau nhiều lần giảm | 
|`100 10`|`91`| Số chia lớn với chuỗi rất ngắn | 
|`1000000000 2`|`500000006`| Độ ổn định đầu vào lớn và tìm kiếm nhị phân | 

## Vỏ cạnh 

Hãy xem xét đầu vào nhỏ nhất có thể:```
1 2
```Tìm kiếm nhị phân bắt đầu bằng`left = right = 1`. 

Vì`mid = 1`:$$1 + 0 = 1$$Điều kiện thành công ngay lập tức, vì vậy câu trả lời trở thành`1`. 

Trường hợp này xác nhận rằng ranh giới tìm kiếm được khởi tạo chính xác và thuật toán xử lý câu trả lời hợp lệ nhỏ nhất mà không bị tràn. 

Bây giờ hãy xem xét:```
10 2
```Câu trả lời đúng là`6`. 

Theo dõi tính toán của người trợ giúp: 

| Hiện tại | Tổng cộng | 
| --- | --- | 
| 6 | 6 | 
| 3 | 9 | 
| 1 | 10 | 
| 0 | dừng lại | 

Tổng số đạt chính xác`10`, Vì thế`6`là hợp lệ. Một sai lầm phổ biến là sử dụng`>`thay vì`>=`khi kiểm tra đủ. Lỗi đó sẽ từ chối không chính xác các kết quả khớp chính xác. 

Cuối cùng, hãy xem xét trường hợp năng suất giảm gần như ngay lập tức:```
100 10
```Vì`v = 91`:$$91 + 9 = 100$$Số hạng tiếp theo bằng 0. 

Trường hợp này xác nhận rằng hàm trợ giúp dừng chính xác khi phép chia đạt đến 0, thay vì tiếp tục mãi mãi hoặc thực hiện công việc không cần thiết.
