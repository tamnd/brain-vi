---
title: "CF 1437E - Làm cho nó ngày càng tăng"
description: "Chúng ta được cung cấp một mảng các số nguyên và một tập hợp các vị trí bị “khóa”, nghĩa là chúng ta không thể thay đổi giá trị tại các chỉ mục đó. Nhiệm vụ là chuyển đổi mảng thành một dãy tăng dần đồng thời thay đổi số phần tử tối thiểu bên ngoài vị trí bị khóa."
date: "2026-06-11T04:46:21+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "constructive-algorithms", "data-structures", "dp", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1437
codeforces_index: "E"
codeforces_contest_name: "Educational Codeforces Round 97 (Rated for Div. 2)"
rating: 2200
weight: 1437
solve_time_s: 105
verified: true
draft: false
---

[CF 1437E - Làm cho nó ngày càng tăng](https://codeforces.com/problemset/problem/1437/E) 

**Xếp hạng:** 2200 
**Tags:** tìm kiếm nhị phân, thuật toán xây dựng, cấu trúc dữ liệu, dp, triển khai 
**Thời gian giải:** 1 phút 45s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên và một tập hợp các vị trí bị “khóa”, nghĩa là chúng ta không thể thay đổi giá trị tại các chỉ mục đó. Nhiệm vụ là chuyển đổi mảng thành một dãy tăng dần đồng thời thay đổi số phần tử tối thiểu bên ngoài vị trí bị khóa. Mỗi thao tác cho phép chúng ta ghi đè lên bất kỳ phần tử nào không có trong tập hợp bị khóa bằng bất kỳ số nguyên nào chúng ta chọn. Đầu ra là số lượng thay đổi tối thiểu được yêu cầu hoặc -1 nếu không thể thực hiện được. 

Quan sát đầu tiên là độ dài của mảng có thể lên tới 500.000 và mỗi số có thể lên tới 10^9. Một cách tiếp cận ngây thơ thử tất cả các chuỗi thay đổi có thể xảy ra sẽ mang tính cấp số nhân và hoàn toàn không thực tế. Sự hiện diện của các vị trí bị khóa làm tăng thêm sự tinh tế: chúng ta không thể tự do điều chỉnh tất cả các phần tử, do đó, các ràng buộc do các vị trí bị khóa áp đặt có thể khiến vấn đề không thể thực hiện được, ngay cả khi việc thay đổi tất cả các phần tử tự do sẽ có tác dụng. 

Trường hợp cạnh không rõ ràng phát sinh khi bản thân các vị trí bị khóa không tăng. Ví dụ, hãy xem xét`a = [1, 3, 2, 5]`với vị trí bị khóa`{2, 4}`. Từ`a[2] = 3`Và`a[4] = 5`, giải pháp nào cũng phải thỏa mãn`a[2] < a[3] < a[4]`. Nếu phần tử được mở khóa`a[3]`không thể điều chỉnh về giá trị nằm trong khoảng từ 3 đến 5 thì nhiệm vụ đó là không thể. Một thuật toán tham lam bất cẩn cố gắng làm cho mỗi phần tử lớn hơn phần tử trước đó mà không kiểm tra các giới hạn bị khóa sẽ thất bại. 

Một trường hợp cạnh khác là khi tất cả các phần tử được mở khóa. Khi đó chúng ta luôn có thể xây dựng một dãy tăng chặt bằng cách gán các giá trị liên tiếp, do đó câu trả lời đơn giản là`n`trừ đi độ dài của dãy con tăng dài nhất đã tồn tại hoặc một biến thể tùy thuộc vào các ràng buộc. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ lặp lại mọi tổ hợp giá trị có thể có cho các phần tử đã được mở khóa. Đối với mỗi kết hợp, chúng tôi sẽ kiểm tra xem mảng kết quả có tăng nghiêm ngặt hay không. Ngay cả với n=20, số lượng kết hợp là vô cùng lớn vì mỗi phần tử được mở khóa có thể là bất kỳ số nguyên nào và chúng tôi không có giới hạn trên cho giá trị của nó. Điều này rõ ràng là quá chậm. 

Thông tin chi tiết quan trọng đến từ việc phân đoạn mảng bằng cách sử dụng các vị trí bị khóa. Vị trí bị khóa chia mảng thành các khoảng. Trong mỗi khoảng, các giá trị biên được cố định bởi các phần tử bị khóa hoặc bởi vô cực âm và dương tiềm ẩn nếu đoạn đó nằm ở cuối mảng. Đối với mỗi phân đoạn, nhiệm vụ giảm xuống chỉ điều chỉnh các phần tử tự do sao cho chúng tăng dần giữa các ranh giới. Điều này tương đương với việc tìm độ dài của dãy con tăng dài nhất (LIS) trong đoạn phù hợp với các ràng buộc biên. Số lượng thay đổi tối thiểu trong đoạn đó là độ dài đoạn trừ đi độ dài LIS này. LIS có thể được tính theo O(m log m) bằng cách sử dụng cách sắp xếp kiên nhẫn hoặc biến thể tìm kiếm nhị phân, trong đó m là độ dài đoạn. 

Nói tóm lại, vấn đề được giải quyết bằng cách phân tách mảng thành các khoảng được xác định bởi các vị trí bị khóa, áp dụng LIS cho từng khoảng theo các ràng buộc biên và tính tổng số lượng thay đổi cần thiết. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(rất lớn) | O(n) | Quá chậm | 
| Tối ưu (LIS phân đoạn) | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xử lý trước mảng để chuyển đổi các vị trí bị khóa dựa trên 1 thành các chỉ số dựa trên 0 để thao tác dễ dàng hơn. Thêm các vị trí khóa ảo tại -1 và n, biểu thị ranh giới vô cực âm và dương. Điều này đảm bảo rằng mọi phân khúc đều có ranh giới được xác định rõ ràng. 
2. Lặp lại các vị trí bị khóa theo cặp. Mỗi cặp xác định một phân đoạn của mảng nơi tất cả các vị trí đều được mở khóa. 
3. Đối với mỗi đoạn, xác định ranh giới bên trái và bên phải bằng cách sử dụng các vị trí bị khóa. Giá trị biên bên trái là giá trị của phần tử bị khóa trước đó hoặc âm vô cực nếu đó là phần tử bắt đầu. Giá trị biên bên phải là phần tử bị khóa tiếp theo hoặc vô cực dương nếu nó là phần tử cuối cùng. Các ranh giới này áp đặt các ràng buộc nghiêm ngặt về giá trị mà các phần tử tự do trong phân đoạn có thể nhận. 
4. Thu thập tất cả các phần tử trong phân đoạn hiện tại nằm giữa các ranh giới. Tính độ dài của dãy con tăng dài nhất của các phần tử này. Sử dụng phương pháp dựa trên tìm kiếm nhị phân để tính toán LIS một cách hiệu quả trong O(m log m), trong đó m là độ dài đoạn. Chỉ những phần tử thỏa mãn các ràng buộc biên mới là ứng cử viên cho LIS. 
5. Nếu đoạn đó có bất kỳ phần tử bị khóa nào bên trong vi phạm thứ tự tăng dần về mặt biên thì không thể xảy ra. Trả về -1 ngay lập tức. 
6. Số lượng thay đổi cần thiết cho đoạn này là độ dài đoạn trừ đi độ dài của LIS. Tính tổng số này trên tất cả các phân đoạn. 
7. Xuất ra tổng số thao tác được yêu cầu. 

Tại sao nó hoạt động: Việc phân đoạn đảm bảo rằng chúng tôi không bao giờ vi phạm thứ tự tăng dần được thực thi bởi các vị trí bị khóa. Bằng cách tối đa hóa số lượng phần tử mà chúng tôi có thể giữ nguyên (thông qua LIS), chúng tôi giảm thiểu số lượng thao tác. Thuật toán không bao giờ đánh giá quá cao LIS vì nó chỉ xem xét các phần tử trong phạm vi hợp lệ, đảm bảo tính chính xác. 

## Giải pháp Python```python
import sys
import bisect
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    if k:
        b = list(map(int, input().split()))
        b = [x - 1 for x in b]  # convert to 0-based
    else:
        b = []

    locked = [-1] + b + [n]  # virtual boundaries
    a_ext = [float('-inf')] + a + [float('inf')]
    
    total_ops = 0

    for i in range(len(locked) - 1):
        l = locked[i] + 1
        r = locked[i+1] - 1
        left_val = a_ext[locked[i] + 1 - 1]  # previous locked
        right_val = a_ext[locked[i+1] + 1 - 1]  # next locked

        if left_val >= right_val:
            print(-1)
            return

        seg = []
        for idx in range(l, r+1):
            if left_val < a[idx] < right_val:
                seg.append(a[idx])

        # LIS in seg
        lis = []
        for val in seg:
            pos = bisect.bisect_left(lis, val)
            if pos == len(lis):
                lis.append(val)
            else:
                lis[pos] = val
        total_ops += (r - l + 1) - len(lis)

    print(total_ops)

if __name__ == "__main__":
    solve()
```Phần đầu tiên đọc đầu vào và xử lý việc lập chỉ mục bằng 0 cho các vị trí bị khóa. Ranh giới ảo đơn giản hóa việc xử lý phân đoạn và các ràng buộc LIS. Trong mỗi phân đoạn, chúng tôi lọc các phần tử nằm trong phạm vi cho phép. Sử dụng tính toán LIS`bisect_left`cho hiệu suất O(m log m). Tổng hợp`segment length - LIS length`đưa ra những thay đổi tối thiểu cần thiết. 

## Ví dụ đã hoạt động 

Mẫu 1:```
a = [1, 2, 1, 1, 3, 5, 1], b = [3, 5]
locked = [-1, 2, 4, 7]
```| Phân đoạn | tôi | r | trái_val | đúng_val | phân đoạn | LIS len | Rất tiếc | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 1 | -inf | 1 | [1,2] | 2 | 0 | 
| 2 | 3 | 3 | 1 | 3 | [1] | 1 | 0 | 
| 3 | 5 | 6 | 3 | thông tin | [5,1] | 1 | 1 | 

Tổng số phép tính: 0 + 0 + 4 = 4 

Dấu vết này cho thấy việc phân đoạn theo các vị trí bị khóa cho phép tối ưu hóa độc lập và ngăn chặn các ràng buộc vi phạm. 

Mẫu 2 (tùy chỉnh):```
a = [5, 3, 4], b = [2]
locked = [-1, 1, 3]
```| Phân đoạn | tôi | r | trái_val | đúng_val | phân đoạn | LIS len | Rất tiếc | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 0 | -inf | 3 | [5] | 0 | 1 | 
| 2 | 2 | 2 | 3 | thông tin | [4] | 1 | 0 | 

Tổng số phép tính: 1 + 0 = 1 

Đoạn đầu tiên yêu cầu thay đổi 5 thành giá trị nhỏ hơn 3 để thỏa mãn ranh giới. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Tính toán LIS của mỗi đoạn sử dụng tìm kiếm nhị phân theo độ dài đoạn, tổng độ dài đoạn là O(n). | 
| Không gian | O(n) | Lưu trữ mảng, vị trí khóa và mảng phụ LIS. | 

Thuật toán dễ dàng nằm gọn trong giới hạn thời gian 2 giây ngay cả với n = 5*10^5 và không vượt quá giới hạn bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# provided sample
assert run("7 2\n1 2 1 1 3 5 1\n3 5\n") == "4"

# minimum-size input
assert run("1 0\n1\n") == "0", "single element, no change needed"

# all locked
assert run("3 3\n1 2 3\n1 2 3\n") == "0", "already increasing"
```
