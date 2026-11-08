---
title: "CF 1433B - Một Giá Sách Khác"
description: "Chúng ta được cấp một giá sách được biểu diễn dưới dạng mảng nhị phân. Giá trị 1 có nghĩa là có một cuốn sách ở vị trí đó, trong khi 0 có nghĩa là vị trí trống."
date: "2026-06-11T04:56:48+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1433
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 677 (Div. 3)"
rating: 800
weight: 1433
solve_time_s: 90
verified: true
draft: false
---

[CF 1433B - Yet Another Bookshelf](https://codeforces.com/problemset/problem/1433/B) 

**Đánh giá:** 800 
**Tags:** tham lam, thực hiện 
**Thời gian giải:** 1 phút 30s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một giá sách được biểu diễn dưới dạng mảng nhị phân. Một giá trị của`1`có nghĩa là một cuốn sách có mặt ở vị trí đó, trong khi`0`có nghĩa là vị trí trống. 

Một bước di chuyển cho phép chúng ta lấy toàn bộ khối sách liền kề và chuyển khối đó sang một vị trí sang trái hoặc phải vào một khoảng trống liền kề. Mục tiêu là làm cho tất cả các cuốn sách chiếm một phân đoạn liên tiếp và không có vị trí trống nào giữa chúng. 

Quan sát quan trọng là sách không bao giờ cần phải di chuyển ra ngoài khu vực giữa cuốn sách hiện có ngoài cùng bên trái và ngoài cùng bên phải. Các vị trí trống bên ngoài phạm vi đó không quan trọng vì khoảng trống chỉ quan trọng khi chúng tách biệt các cuốn sách. 

Những hạn chế là rất nhỏ. Có tối đa 200 test case và mỗi giá sách có chiều dài tối đa là 50. Ngay cả một$O(n^2)$giải pháp sẽ dễ dàng đủ nhanh. Điều này có nghĩa là thách thức không phải là hiệu suất mà là nhận ra sự đơn giản hóa cơ bản. 

Một số trường hợp đặc biệt rất dễ bị hiểu lầm. 

Coi như:```
1 0 0 0 1
```Câu trả lời là`3`, không`1`. Có ba vị trí trống giữa hai cuốn sách và mỗi vị trí trống cuối cùng phải biến mất khỏi chúng. 

Coi như:```
1 1 0 0 1
```Câu trả lời là`2`. 

Một lỗi phổ biến là đếm các nhóm số 0 thay vì các số 0 riêng lẻ. Ở đây có một khoảng trống nhưng lại chứa hai vị trí trống nên cần phải di chuyển hai lần. 

Coi như:```
0 0 1 1 1 0 0
```Câu trả lời là`0`. 

Có số không ở cả hai bên, nhưng không có số không ở giữa các cuốn sách. Việc triển khai bất cẩn đếm tất cả các số 0 sẽ trả về không chính xác`4`. 

Coi như:```
1
```Câu trả lời là`0`. 

Một cuốn sách đã là một phân khúc liền kề. 

## Phương pháp tiếp cận 

Một cách mạnh mẽ để suy nghĩ về vấn đề là mô phỏng các chuyển động. Chúng ta có thể liên tục chọn các phân đoạn và dịch chuyển chúng cho đến khi tất cả các cuốn sách đều liên tiếp. Vì có thể có nhiều chuỗi di chuyển khác nhau nên việc tìm kiếm hoàn chỉnh sẽ phải khám phá nhiều trạng thái của giá sách. 

Mặc dù$n \le 50$, cách tiếp cận không gian trạng thái như vậy là không cần thiết. Số lượng cấu hình giá sách có thể tăng theo cấp số nhân, khiến nó trở thành một giải pháp kém hấp dẫn. 

Sự đột phá đến từ việc xem xét những gì một động thái thực sự đạt được. 

Giả sử có số 0 giữa hai cuốn sách:```
1 0 1
```Để xóa bỏ khoảng trống này, một trong các nhóm sách phải di chuyển qua vị trí trống đó. Bất kể bên nào di chuyển, cần chính xác một bước cho số 0 đó. 

Bây giờ hãy xem xét:```
1 0 0 1
```Có hai vị trí trống giữa các cuốn sách. Một nước đi có thể loại bỏ tối đa một trong các vị trí đó khỏi bên trong. Cuối cùng cả hai số 0 đều phải biến mất, nên câu trả lời là`2`. 

Mô hình này khái quát hóa. Vị trí duy nhất quan trọng là vị trí giữa cuốn sách ngoài cùng bên trái và cuốn sách ngoài cùng bên phải. Mỗi số 0 bên trong khoảng đó đại diện cho một đơn vị công việc không thể tránh khỏi và mọi số 0 như vậy có thể bị loại bỏ chỉ bằng một nước đi. 

Kết quả là số bước đi tối thiểu chỉ đơn giản là số số 0 giữa bước đầu tiên`1`và cuối cùng`1`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm và không cần thiết | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tìm vị trí của cuốn đầu tiên, nghĩa là lần xuất hiện đầu tiên của`1`. 
2. Tìm vị trí của cuốn sách cuối cùng, nghĩa là lần xuất hiện cuối cùng của`1`. 
3. Kiểm tra mọi vị trí giữa hai chỉ số này. 
4. Đếm xem có bao nhiêu vị trí`0`. 
5. Xuất ra số đếm. 

Lý do bước 4 có hiệu quả là vì mỗi số 0 bên trong đại diện cho một khoảng cách ngăn cách các cuốn sách. Mỗi vị trí khoảng trống như vậy cuối cùng phải được lấp đầy bằng cách di chuyển các cuốn sách ngang qua nó và một bước di chuyển có thể loại bỏ chính xác một số 0 bên trong. 

### Tại sao nó hoạt động 

Đặt cuốn sách đầu tiên ở vị trí`L`và cuốn sách cuối cùng ở vị trí`R`. 

Bất kỳ cuốn sách nào ngoài khoảng thời gian`[L, R]`không tồn tại theo định nghĩa. Để làm cho tất cả các cuốn sách liên tiếp nhau, mọi vị trí bên trong`[L, R]`hiện có chứa`0`cuối cùng phải chứa một cuốn sách. 

Mỗi lần di chuyển sẽ dịch chuyển một khối liền kề đi một vị trí và có thể giảm số lượng vị trí trống bên trong`[L, R]`nhiều nhất là một. Do đó, cần phải di chuyển ít nhất bằng số 0 bên trong. 

Ngược lại, với mọi số 0 bên trong, chúng ta có thể di chuyển một khối liền kề về phía nó và lấp đầy nó chỉ bằng một bước di chuyển. Việc lặp lại quá trình này sẽ loại bỏ tất cả các số 0 bên trong bằng cách sử dụng chính xác số lần di chuyển đó. 

Vì giới hạn dưới và giới hạn trên giống hệt nhau nên số bước di chuyển tối thiểu bằng số 0 giữa cuốn đầu tiên và cuốn cuối cùng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        left = a.index(1)
        right = n - 1 - a[::-1].index(1)

        ans = 0
        for i in range(left, right + 1):
            if a[i] == 0:
                ans += 1

        print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp đầu tiên là xác định vị trí các cuốn sách ngoài cùng bên trái và ngoài cùng bên phải. Những vị trí đó xác định khoảng thời gian duy nhất quan trọng. 

Vòng lặp sau đó đếm số 0 trong khoảng đó. Các số 0 nằm ngoài khoảng bị bỏ qua vì chúng không phân tách bất kỳ cuốn sách nào. 

Một chi tiết tinh tế là tính toán lần xuất hiện cuối cùng của`1`. Python không cung cấp thao tác chỉ mục ngược tích hợp, do đó mã sẽ đảo ngược mảng và tìm mảng đầu tiên`1`ở đó. Chuyển đổi vị trí đó trở lại chỉ mục ban đầu sẽ mang lại vị trí của cuốn sách ngoài cùng bên phải. 

Một chi tiết khác là quá trình quét bao gồm cả hai điểm cuối. Vì các điểm cuối đó được đảm bảo chứa sách nên việc bao gồm chúng sẽ không ảnh hưởng đến số lượng và tránh sai sót từng cái một. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
0 0 1 0 1 0 1
```| Biến | Giá trị | 
| --- | --- | 
| trái | 2 | 
| đúng | 6 | 

Các vị trí được khảo sát: 

| Chỉ mục | Giá trị | Số không | 
| --- | --- | --- | 
| 2 | 1 | 0 | 
| 3 | 0 | 1 | 
| 4 | 1 | 1 | 
| 5 | 0 | 2 | 
| 6 | 1 | 2 | 

Câu trả lời cuối cùng:`2`. 

Ví dụ này cho thấy rằng chỉ có số 0 giữa cuốn sách đầu tiên và cuốn sách cuối cùng mới quan trọng. Các số 0 đứng đầu ở vị trí 0 và 1 không liên quan. 

### Ví dụ 2 

đầu vào:```
1 1 0 1 1
```| Biến | Giá trị | 
| --- | --- | 
| trái | 0 | 
| đúng | 4 | 

Các vị trí được khảo sát: 

| Chỉ mục | Giá trị | Số không | 
| --- | --- | --- | 
| 0 | 1 | 0 | 
| 1 | 1 | 0 | 
| 2 | 0 | 1 | 
| 3 | 1 | 1 | 
| 4 | 1 | 1 | 

Câu trả lời cuối cùng:`1`. 

Ví dụ này chứng minh rằng một số 0 bên trong tương ứng với chính xác một bước di chuyển cần thiết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một lần quét để tìm điểm cuối và một lần quét để đếm số không | 
| Không gian | O(1) | Chỉ có một vài biến được sử dụng | 

Với$n \le 50$, thời gian chạy rất nhỏ. Thậm chí trên tất cả 200 trường hợp thử nghiệm, tổng công việc chỉ có vài nghìn thao tác, nằm trong giới hạn thoải mái. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    def input():
        return sys.stdin.readline()

    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        left = a.index(1)
        right = n - 1 - a[::-1].index(1)

        out.append(str(sum(1 for i in range(left, right + 1) if a[i] == 0)))

    return "\n".join(out)

# provided samples
assert run(
"""5
7
0 0 1 0 1 0 1
3
1 0 0
5
1 1 0 0 1
6
1 0 0 0 0 1
5
1 1 0 1 1
"""
) == "2\n0\n2\n4\n1", "sample 1"

# minimum size
assert run(
"""1
1
1
"""
) == "0", "single book"

# books already consecutive
assert run(
"""1
7
0 0 1 1 1 0 0
"""
) == "0", "no internal gaps"

# large internal gap
assert run(
"""1
5
1 0 0 0 1
"""
) == "3", "count every interior zero"

# alternating pattern
assert run(
"""1
7
1 0 1 0 1 0 1
"""
) == "3", "multiple separated gaps"

print("All tests passed")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`|`0`| Ví dụ kích thước tối thiểu | 
|`0 0 1 1 1 0 0`|`0`| Bỏ qua các số 0 ngoài phạm vi hoạt động | 
|`1 0 0 0 1`|`3`| Đếm các số 0 riêng lẻ, không phải nhóm khoảng cách | 
|`1 0 1 0 1 0 1`|`3`| Nhiều khoảng trống độc lập | 

## Vỏ cạnh 

Hãy xem xét:```
1
5
1 0 0 0 1
```Cuốn sách đầu tiên ở chỉ số 0 và cuốn sách cuối cùng ở chỉ số 4. Bên trong khoảng đó có ba số không. Thuật toán đếm cả ba và trả về`3`. Giải pháp đếm các phân đoạn khoảng cách thay vì các ô khoảng cách sẽ trả về không chính xác`1`. 

Coi như:```
1
7
0 0 1 1 1 0 0
```Cuốn sách đầu tiên ở chỉ số 2 và cuốn sách cuối cùng ở chỉ số 4. Thuật toán chỉ kiểm tra các vị trí từ 2 đến 4 và không tìm thấy số 0. Câu trả lời là`0`. Điều này bỏ qua các vị trí trống bên ngoài phạm vi sách một cách chính xác. 

Coi như:```
1
3
1 0 0
```Cuốn sách đầu tiên và cuốn sách cuối cùng có cùng vị trí. Khoảng này chỉ chứa một ô, đó là một cuốn sách. Số số 0 là`0`, vậy câu trả lời là`0`. Một cuốn sách đã tạo thành một phân đoạn liền kề. 

Coi như:```
1
5
1 1 0 0 1
```Khoảng liên quan là toàn bộ mảng. Hai số 0 xuất hiện giữa các cuốn sách ngoài cùng nên thuật toán trả về`2`. Điều này phù hợp với số lần di chuyển tối thiểu cần thiết để loại bỏ cả hai khoảng trống.
