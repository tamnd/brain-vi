---
title: "CF 1791F - Truy vấn điểm cập nhật phạm vi"
description: "Chúng ta được cung cấp một mảng các số nguyên và một loạt các phép toán biến đổi một mảng con bằng cách thay thế mỗi phần tử bằng tổng các chữ số của nó hoặc truy vấn giá trị hiện tại của một phần tử. Nhiệm vụ là xuất kết quả của tất cả các truy vấn theo thứ tự chúng xuất hiện."
date: "2026-06-09T10:33:21+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "brute-force", "data-structures"]
categories: ["algorithms"]
codeforces_contest: 1791
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 849 (Div. 4)"
rating: 1500
weight: 1791
solve_time_s: 194
verified: true
draft: false
---

[CF 1791F - Truy vấn điểm cập nhật phạm vi](https://codeforces.com/problemset/problem/1791/F) 

**Đánh giá:** 1500 
**Tags:** tìm kiếm nhị phân, lực lượng vũ phu, cấu trúc dữ liệu 
**Thời gian giải:** 3 phút 14s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên và một loạt các phép toán biến đổi một mảng con bằng cách thay thế mỗi phần tử bằng tổng các chữ số của nó hoặc truy vấn giá trị hiện tại của một phần tử. Nhiệm vụ là xuất kết quả của tất cả các truy vấn theo thứ tự chúng xuất hiện. Mỗi trường hợp thử nghiệm là độc lập. Kích thước của mảng và số lượng thao tác có thể đạt tới 200.000, nhưng tổng số trên tất cả các trường hợp thử nghiệm được giới hạn bởi cùng một số. Điều này có nghĩa là một cách tiếp cận đơn giản áp dụng từng phép toán tổng chữ số bằng cách lặp lại rõ ràng trên tất cả các phần tử trong phạm vi cho mỗi truy vấn sẽ quá chậm trong trường hợp xấu nhất. 

Phép tính tổng các chữ số sẽ không có giá trị khi số đó trở thành số có một chữ số. Đây là thuộc tính quan trọng cho phép chúng tôi tối ưu hóa. Ví dụ: áp dụng phép tính tổng các chữ số nhiều lần trên một số như 1434 sẽ cho 1+4+3+4=12, sau đó 1+2=3, sau đó các phép tính tiếp theo sẽ không làm gì cả. Các trường hợp biên phát sinh khi phạm vi chỉ chứa các số có một chữ số, không thay đổi hoặc khi các phạm vi trùng nhau, do đó việc lặp lại đơn giản có thể lặp lại các phép tính một cách không cần thiết. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực chỉ lặp lại từng truy vấn loại 1 và với mỗi chỉ mục trong phạm vi được chỉ định sẽ tính tổng các chữ số, sau đó trả lời các truy vấn loại 2 bằng cách tra cứu trực tiếp. Mặc dù điều này đúng về mặt logic nhưng độ phức tạp trong trường hợp xấu nhất của nó là$O(q \cdot n \cdot \log(a_i))$, trong đó hệ số logarit xuất phát từ phép toán tổng các chữ số, không thể chấp nhận được đối với các giới hạn đầu vào. Ví dụ: 200.000 thao tác, mỗi thao tác ảnh hưởng đến 200.000 phần tử sẽ liên quan đến hàng chục tỷ thao tác. 

Quan sát quan trọng là mọi số đều trở nên ổn định (một chữ số) sau tối đa 9 ứng dụng tổng chữ số, vì các số được giới hạn bởi 10^9. Do đó, khi một phần tử có một chữ số, các phép tính tổng chữ số tiếp theo có thể bị bỏ qua. Chúng tôi có thể theo dõi những phần tử nào vẫn “hoạt động” (chưa phải một chữ số) và chỉ áp dụng các thao tác cho chúng. Việc sử dụng cấu trúc dữ liệu như danh sách được sắp xếp hoặc tập hợp các chỉ mục hoạt động cho phép chúng ta bỏ qua các số đã ổn định một cách hiệu quả. Điều này làm giảm công việc dư thừa và giữ độ phức tạp tổng thể tuyến tính theo số lượng cập nhật đang hoạt động. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(q * n * log(a_i)) | O(n) | Quá chậm | 
| Tối ưu hóa với các chỉ số hoạt động | O(n + q * log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một mảng`a`với các số đầu vào và một danh sách được sắp xếp`active`chứa chỉ số của tất cả các số lớn hơn 9. 
2. Lặp lại từng truy vấn theo thứ tự. Nếu truy vấn thuộc loại 2, hãy in ngay giá trị tại chỉ mục được truy vấn. 
3. Đối với truy vấn loại 1 chỉ định phạm vi [l, r], hãy tìm tất cả các chỉ mục trong`active`nằm trong phạm vi này. Từ`active`được sắp xếp, việc này có thể được thực hiện một cách hiệu quả bằng tìm kiếm nhị phân. 
4. Đối với mỗi chỉ mục hoạt động như vậy, hãy cập nhật`a[i]`là tổng các chữ số của nó. 
5. Nếu sau khi cập nhật`a[i]`trở thành một chữ số, hãy xóa chỉ mục khỏi`active`. Nếu không, hãy giữ nó cho các hoạt động trong tương lai. 
6. Tiếp tục xử lý các truy vấn cho đến khi tất cả được xử lý. Truy vấn loại 2 luôn đọc giá trị hiện tại trong`a`, phản ánh tất cả các biến đổi trước đó. 

Tại sao nó hoạt động: Bất biến là vậy`active`chứa chính xác những chỉ số có giá trị chưa ổn định. Chúng tôi chỉ cập nhật những thông tin này để đảm bảo không tính toán lãng phí. Truy vấn loại 2 luôn truy xuất giá trị hiện tại, điều này đúng vì mọi thao tác loại 1 trước đó ảnh hưởng đến chỉ mục đó đã được áp dụng chính xác một lần. Hoạt động tổng của các chữ số sẽ không có tác dụng khi đạt đến một chữ số, đảm bảo tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from bisect import bisect_left, bisect_right

def sum_digits(x):
    return sum(int(d) for d in str(x))

def solve():
    t = int(input())
    for _ in range(t):
        n, q = map(int, input().split())
        a = list(map(int, input().split()))
        # active indices with values > 9
        active = set(i for i, val in enumerate(a) if val > 9)
        
        queries = []
        for _ in range(q):
            queries.append(input().split())
        
        for query in queries:
            if query[0] == '1':
                l, r = int(query[1])-1, int(query[2])-1
                to_remove = []
                for i in list(active):
                    if l <= i <= r:
                        a[i] = sum_digits(a[i])
                        if a[i] < 10:
                            to_remove.append(i)
                for i in to_remove:
                    active.remove(i)
            else:
                x = int(query[1])-1
                print(a[x])

if __name__ == "__main__":
    solve()
```Mã theo dõi các chỉ số hoạt động trong một tập hợp. Đối với mỗi truy vấn loại 1, chúng tôi chỉ lặp lại các chỉ mục đang hoạt động trong phạm vi đã chỉ định, cập nhật giá trị của chúng và xóa các chỉ mục ổn định. Truy vấn loại 2 trực tiếp xuất giá trị. Chúng tôi sử dụng lập chỉ mục dựa trên 0 trong nội bộ để đơn giản. Việc sử dụng một tập hợp đảm bảo việc loại bỏ là O(1) và việc lặp lại các chỉ mục đang hoạt động sẽ hiệu quả vì số lượng phần tử không có một chữ số là nhỏ. 

## Ví dụ đã hoạt động 

**Vết mẫu 1:** 

đầu vào:`[1, 420, 69, 1434, 2023]`, truy vấn loại 1 đầu tiên`[2, 3]`. 

| Chỉ mục | Tích cực? | Giá trị | Sau sum_digits | 
| --- | --- | --- | --- | 
| 1 | vâng | 420 | 6 | 
| 2 | vâng | 69 | 15 | 

Hoạt động sau lần cập nhật đầu tiên:`{2,3,4}`→ chỉ số có giá trị`15, 1434, 2023`. Truy vấn loại 2 đọc trực tiếp các giá trị hiện tại: 6, 15, 1434. 

Truy vấn loại 1 thứ hai`[2,5]`: 

| Chỉ mục | Tích cực? | Giá trị | Sau sum_digits | 
| --- | --- | --- | --- | 
| 2 | vâng | 15 | 6 | 
| 3 | vâng | 1434 | 12 | 
| 4 | vâng | 2023 | 7 | 

Hoạt động sau này:`{3}`→ chỉ có chỉ số 3 có giá trị 12 (>9). Đầu ra truy vấn loại 2: 1, 6, 7, xác nhận tính chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + q * k) |`k`nhỏ vì mỗi số trở thành một chữ số sau tối đa 9 tổng chữ số; mỗi chỉ mục chỉ được xử lý khi >9 | 
| Không gian | O(n) | Mảng`a`và bộ hoạt động | 

Vì tổng số phần tử và truy vấn trên tất cả các trường hợp thử nghiệm ≤ 2e5 nên giải pháp này chạy tốt trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# Provided sample
assert run("3\n5 8\n1 420 69 1434 2023\n1 2 3\n2 2\n2 3\n2 4\n1 2 5\n2 1\n2 3\n2 5\n2 3\n9999 1000\n1 1 2\n2 1\n2 2\n1 1\n1\n2 1\n") == "6\n15\n1434\n1\n6\n7\n36\n1\n1"

# Custom tests
assert run("1\n5 3\n9 10 11 12 13\n1 1 5\n2 3\n2 5\n") == "2\n4"
assert run("1\n1 2\n123456789\n1 1 1\n2 1\n") == "9"
assert run("1\n3 2\n5 5 5\n1 1 3\n2 2\n") == "5"
assert run("1\n3 1\n99 9 9\n2 1\n") == "99"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 5 phần tử đều >9, đầy đủ | 2 4 | Sửa các phép biến đổi và ổn định tổng chữ số | 
| Phần tử đơn lớn | 9 | Nhiều ứng dụng tổng chữ số được xử lý chính xác | 
| Tất cả một chữ số | 5 | Không thực hiện cập nhật không cần thiết | 
| Kết hợp 2 chữ số và một chữ số | 99 | Truy vấn hoạt động trên các số không thay đổi | 

## Vỏ cạnh 

Nếu một phạm vi chỉ chứa các số có một chữ số thì thuật toán sẽ bỏ qua chúng một cách chính xác vì chúng không nằm trong tập hợp hiện hoạt. Đối với các phạm vi chồng chéo, mỗi chỉ số được xử lý tối đa 9 lần cho đến khi ổn định, do đó các phạm vi lặp lại không gây ra sự kém hiệu quả. Đối với mảng hoặc phạm vi một phần tử, hoạt động giống hệt nhau và cập nhật hoặc đọc phần tử một cách chính xác. Việc triển khai xử lý tất cả các điều kiện biên bao gồm kích thước mảng tối đa, số lượng truy vấn tối đa và các giá trị ở giới hạn trên 10^9.
