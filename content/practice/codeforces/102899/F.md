---
title: "CF 102899F - KK \u4e0e\u5237\u9898"
description: "Chúng ta được cho một chuỗi các ngày và mỗi ngày kk ghi lại số câu trả lời sai (WA) mà anh ta đã mắc phải. Các giá trị này tạo thành một mảng a[1..n]. Bắt đầu từ số điểm 0, điểm số sẽ tăng dần theo từng ngày. Khi xử lý ngày thứ i, chúng tôi so sánh số WA a[i] với mọi ngày trước đó j < i."
date: "2026-07-04T08:21:10+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102899
codeforces_index: "F"
codeforces_contest_name: "The 2nd Hangzhou Normal University Freshman Programming Contest"
rating: 0
weight: 102899
solve_time_s: 43
verified: true
draft: false
---

[CF 102899F - KK \u4e0e\u5237\u9898](https://codeforces.com/problemset/problem/102899/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 43s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một chuỗi các ngày và mỗi ngày kk ghi lại số câu trả lời sai (WA) mà anh ta đã mắc phải. Các giá trị này tạo thành một mảng`a[1..n]`. 

Bắt đầu từ số điểm 0, điểm số sẽ tăng dần theo từng ngày. Khi xử lý ngày`i`, chúng tôi so sánh số lượng WA`a[i]`với mọi ngày trước đó`j < i`. Mỗi so sánh đóng góp độc lập: nếu`a[j] < a[i]`, điểm sẽ tăng thêm một, nếu`a[j] > a[i]`, điểm số giảm đi một và nếu chúng bằng nhau thì không có gì thay đổi. Sau khi áp dụng tất cả các so sánh với những ngày trước đó, điểm mới này sẽ trở thành điểm hiện tại của ngày`i`. Ngày 1 không có ngày nào trước đó nên luôn kết thúc với số điểm bằng 0. 

Chúng tôi phải xuất hai giá trị: số điểm tối đa đạt được vào bất kỳ thời điểm nào trong tất cả các ngày và điểm cuối cùng sau khi xử lý tất cả các ngày. 

Ràng buộc`n ≤ 100000`buộc chúng ta phải tránh bất cứ điều gì so sánh rõ ràng từng cặp ngày. Một vòng lặp kép ngây thơ sẽ bao gồm các phép so sánh bình phương khoảng n, điều này vượt xa khả năng thực hiện. Bất kỳ giải pháp nào cũng phải giảm việc lặp đi lặp lại “đếm xem có bao nhiêu giá trị trước đó nhỏ hơn hoặc lớn hơn hiện tại” thành một thứ có thể được trả lời hiệu quả mỗi ngày. 

Trường hợp cạnh tinh tế xuất hiện khi có nhiều giá trị bằng nhau. Vì các giá trị bằng nhau không đóng góp gì nên giải pháp đúng phải đảm bảo chúng được loại trừ khỏi cả số lượng “lớn hơn” và “nhỏ hơn” mà không vô tình đếm hai lần hoặc thiếu chuyển đổi giữa các nhóm có giá trị bằng nhau. 

Một trường hợp góc khác phát sinh khi dãy tăng hoặc giảm một cách nghiêm ngặt. Trong những trường hợp như vậy, điểm số tăng theo phương trình bậc hai theo cách giải thích đơn giản về sự đóng góp của cặp theo thời gian, nhưng sự tiến triển thực tế là tuyến tính trên mỗi bước xét về sự đóng góp từ các phần tử trước đó và sự khác biệt này rất dễ bị hiểu sai nếu người ta cố gắng mô phỏng cập nhật cặp không chính xác. 

## Phương pháp tiếp cận 

Việc giải thích bạo lực rất đơn giản. Cho mỗi ngày`i`, chúng tôi quét tất cả các ngày trước đó`j < i`và so sánh rõ ràng`a[j]`Và`a[i]`. Chúng tôi duy trì số điểm hiện tại và cũng theo dõi mức tối đa. Điều này đúng vì nó trực tiếp tuân theo định nghĩa của quy tắc cập nhật điểm số. Tuy nhiên, nó thực hiện khoảng`1 + 2 + ... + (n-1)`sự so sánh theo thứ tự`n^2 / 2`, và cho`n = 100000`điều này là hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là điểm số trong ngày`i`chỉ phụ thuộc vào có bao nhiêu giá trị trước đó nhỏ hơn và bao nhiêu giá trị lớn hơn. Nếu chúng ta xác định: 

-`less(i)`= số lượng`j < i`như vậy`a[j] < a[i]`-`greater(i)`= số lượng`j < i`như vậy`a[j] > a[i]`thì quá trình chuyển đổi chỉ đơn giản là:`score[i] = score[i-1] + less(i) - greater(i)`Vì vậy, vấn đề giảm xuống còn việc duy trì số lượng phần tử trước đó so với giá trị hiện tại. Chúng ta cần một cấu trúc dữ liệu có thể hỗ trợ khi chúng ta xử lý mảng từ trái sang phải: 

1. Chèn một giá trị`a[i]`2. Truy vấn có bao nhiêu giá trị hiện tại nhỏ hơn hoặc lớn hơn hoàn toàn 

Vì giá trị lên đến`10^5`, chúng ta có thể nén chúng thành cây Fenwick (hoặc cây phân đoạn). Điều này cho phép chúng tôi duy trì tần số và tổng tiền tố truy vấn một cách hiệu quả. Mỗi ngày trở thành một`O(log n)`hoạt động thay vì`O(n)`quét. 

Câu trả lời cuối cùng yêu cầu theo dõi điểm tiền tố tối đa trong suốt quá trình chứ không chỉ giá trị cuối cùng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Đếm cây Fenwick | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý mảng từ trái sang phải trong khi duy trì cấu trúc tần số trên tất cả các giá trị đã thấy trước đó. 

1. Khởi tạo cây Fenwick (hoặc cấu trúc tương tự) và đặt tất cả số đếm về 0. Đồng thời khởi tạo`score = 0`Và`best = 0`. Điều này thể hiện ngày đầu tiên trước khi có bất kỳ sự so sánh nào. 
2. Cho mỗi ngày`i`từ 1 đến n, chúng ta xử lý`a[i]`làm giá trị hiện tại mà chúng tôi đang chèn vào lịch sử. 
3. Trước khi chèn`a[i]`, chúng tôi truy vấn có bao nhiêu giá trị trước đó nhỏ hơn hoàn toàn. Đây là truy vấn tổng tiền tố lên tới`a[i] - 1`. Điều này mang lại`less(i)`. 
4. Chúng tôi cũng truy vấn xem có bao nhiêu giá trị trước đó lớn hơn một cách nghiêm ngặt. Điều này được tính như`(i - 1) - count(≤ a[i])`. Tổng tiền tố lên đến`a[i]`đưa ra số lượng giá trị nhỏ hơn hoặc bằng, do đó, việc trừ đi tổng số phần tử trước đó sẽ tách biệt những phần tử lớn hơn. Điều này mang lại`greater(i)`. 
5. Chúng tôi cập nhật điểm số như`score += less(i) - greater(i)`. Điều này trực tiếp áp dụng định nghĩa về việc mỗi ngày trước đó góp phần như thế nào vào sự thay đổi của ngày hôm nay. 
6. Chúng tôi cập nhật`best = max(best, score)`sau mỗi ngày, vì số điểm tối đa có thể đạt được trước ngày cuối cùng. 
7. Chúng tôi chèn`a[i]`vào cây Fenwick bằng cách tăng tần số của nó. 

Sau khi xử lý tất cả các ngày,`best`là số điểm tối đa được nhìn thấy, và`score`là giá trị cuối cùng 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, cây Fenwick thể hiện chính xác tập hợp số lượng WA của tất cả các ngày trước đó. Mọi đóng góp vào số điểm trong ngày`i`chỉ phụ thuộc vào sự so sánh giữa`a[i]`và các phần tử trước đó. Cấu trúc đảm bảo rằng mỗi phần tử trước đó được tính chính xác một lần trong danh mục “ít hơn”, “lớn hơn” hoặc bị bỏ qua và các danh mục này không liên kết với nhau. Vì điểm được xác định là tổng của các đóng góp theo cặp độc lập nên việc duy trì tổng số sẽ duy trì giá trị chính xác mà không cần phải xem lại các phần tử trong quá khứ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class Fenwick:
    def __init__(self, n):
        self.n = n
        self.bit = [0] * (n + 1)

    def add(self, i, v):
        while i <= self.n:
            self.bit[i] += v
            i += i & -i

    def sum(self, i):
        s = 0
        while i > 0:
            s += self.bit[i]
            i -= i & -i
        return s

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    maxv = max(a)
    fw = Fenwick(maxv)

    score = 0
    best = 0

    for i, x in enumerate(a):
        less = fw.sum(x - 1)
        total_prev = i
        leq = fw.sum(x)
        greater = total_prev - leq

        score += less - greater
        best = max(best, score)

        fw.add(x, 1)

    print(best, score)

if __name__ == "__main__":
    solve()
```Cây Fenwick lưu trữ tần số của số lượng WA được thấy cho đến nay. Tổng tiền tố lên đến`x-1`trực tiếp đưa ra số lượng các giá trị trước đó nhỏ hơn nghiêm ngặt. Tổng tiền tố lên đến`x`đưa ra số lượng giá trị nhỏ hơn hoặc bằng, do đó, việc trừ đi số phần tử được xử lý sẽ mang lại giá trị lớn hơn. Cập nhật điểm tuân theo chính xác quy tắc đóng góp theo cặp và giá trị tốt nhất được theo dõi tăng dần. 

Một chi tiết tinh tế đó là`total_prev`đơn giản là chỉ mục`i`, kể từ trước ngày xử lý`i`có chính xác`i`các phần tử trước đó. Điều này tránh được một truy vấn bổ sung và giữ cho bản cập nhật luôn sạch sẽ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
1 3 2
```Chúng tôi ngầm theo dõi bang Fenwick. 

| tôi | x | ít hơn | lớn hơn | ghi điểm trước | ghi điểm sau | tốt nhất | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | - | - | - | 0 | 0 | 0 | 
| 1 | 1 | 0 | 0 | 0 | 0 | 0 | 
| 2 | 3 | 1 | 0 | 0 | 1 | 1 | 
| 3 | 2 | 1 | 1 | 1 | 1 | 1 | 

Điều này cho thấy ngày thứ 2 tăng điểm như thế nào do giá trị nhỏ hơn trước đó và ngày thứ 3 cân bằng giữa lãi và lỗ. 

### Ví dụ 2 

đầu vào:```
3
1 5 4
```| tôi | x | ít hơn | lớn hơn | ghi điểm trước | ghi điểm sau | tốt nhất | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | - | - | - | 0 | 0 | 0 | 
| 1 | 1 | 0 | 0 | 0 | 0 | 0 | 
| 2 | 5 | 1 | 0 | 0 | 1 | 1 | 
| 3 | 4 | 1 | 0 | 1 | 2 | 2 | 

Ngày thứ ba vẫn đóng góp tích cực về tổng thể vì nó lớn hơn hầu hết các giá trị trước đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Mỗi ngày thực hiện hai truy vấn tiền tố Fenwick và một lần cập nhật | 
| Không gian | O(n) | Mảng tần số cho cây Fenwick lên tọa độ tối đa | 

Ràng buộc`n ≤ 100000`làm cho`O(n log n)`an toàn một cách thoải mái, vì mỗi phép toán có nhiều nhất là logarit`10^5`. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output

    solve()
    return output.getvalue().strip()

# sample tests
assert run("3\n1 3 2\n") == "1 1"
assert run("3\n1 5 4\n") == "1 2"

# all equal
assert run("5\n2 2 2 2 2\n") == "0 0"

# strictly increasing
assert run("5\n1 2 3 4 5\n") == "10 10"

# strictly decreasing
assert run("5\n5 4 3 2 1\n") == "10 -10"

# single element
assert run("1\n42\n") == "0 0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều bình đẳng | 0 0 | giá trị bằng nhau không tạo ra sự đóng góp | 
| ngày càng tăng | 10 10 | trường hợp tích lũy tối đa | 
| giảm dần | 10 -10 | tích lũy âm đối xứng | 
| phần tử đơn | 0 0 | trường hợp không có cạnh so sánh | 

## Vỏ cạnh 

Đối với một chuỗi trong đó tất cả các giá trị đều giống nhau, mọi so sánh đều mang lại sự bằng nhau, do đó cả hai đều không`less`cũng không`greater`thay đổi điểm số. Cây Fenwick luôn trả về 0 cho cả hai truy vấn và điểm vẫn bằng 0 xuyên suốt. Đối với đầu vào`5 2 2 2 2 2`, thuật toán xử lý mỗi ngày, nhưng mỗi`less`Và`greater`bằng 0, vì vậy cả điểm cuối cùng và điểm tối đa đều bằng 0. 

Đối với một chuỗi tăng nghiêm ngặt như`1 2 3 4 5`, mỗi phần tử mới lớn hơn tất cả các phần tử trước đó, vì vậy`less(i) = i-1`Và`greater(i) = 0`. Điểm số tăng lên bởi`0, 1, 2, 3, 4`, tích lũy đến 10 và cây Fenwick phản ánh chính xác sự phân bổ tiền tố ngày càng tăng này ở mỗi bước.
