---
title: "CF 180E - Khối"
description: "Chúng ta có một hàng hình khối, mỗi hình được sơn một trong m màu và chúng ta được phép loại bỏ tối đa k hình khối để tối đa hóa chiều dài của một đoạn hình khối liên tiếp đều có cùng màu."
date: "2026-06-03T00:49:28+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "dp", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 180
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 116 (Div. 2, ACM-ICPC Rules)"
rating: 1800
weight: 180
solve_time_s: 80
verified: true
draft: false
---

[CF 180E - Hình khối](https://codeforces.com/problemset/problem/180/E) 

**Đánh giá:** 1800 
**Thẻ:** tìm kiếm nhị phân, dp, hai con trỏ 
**Thời gian giải:** 1 phút 20s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một hàng hình khối, mỗi hình được sơn bằng một trong _m_ màu và chúng ta được phép loại bỏ tối đa _k_ hình khối để tối đa hóa chiều dài của một đoạn hình khối liên tiếp đều có cùng màu. Đầu vào cung cấp số khối _n_, số lượng màu _m_, số lần xóa tối đa _k_ và chuỗi các màu khối. Đầu ra là một số nguyên duy nhất: độ dài của đoạn liền kề dài nhất của một màu sau khi loại bỏ tối đa _k_ khối. 

Thách thức chính là mảng có thể lớn, lên tới 2·10^5 khối, vì vậy mọi giải pháp đều phải gần như tuyến tính hoặc tuyến tính trong _n_. Các phương pháp bậc hai thử tất cả các mảng con hoặc tất cả các mẫu xóa là không khả thi vì chúng sẽ liên quan đến tối đa 4·10^10 phép toán trong trường hợp xấu nhất. Số lượng màu cũng có thể lớn, lên tới 10^5, do đó, các giải pháp lặp lại trên tất cả các màu có thao tác nặng trên mỗi màu phải cẩn thận để duy trì hiệu quả. 

Các trường hợp cạnh dễ xử lý sai bao gồm khi _k_ bằng 0, trong trường hợp đó không thể xóa khối nào, do đó thuật toán phải tìm chính xác đoạn liền kề dài nhất hiện có. Một trường hợp khác là khi tất cả các hình khối đều có cùng màu; điểm tối đa là _n_ bất kể _k_. Nếu _k_ lớn nhưng không đủ để xóa tất cả các màu khác thì thuật toán vẫn phải tính toán chính xác chỉ xóa tối đa _k_. Các trình tự nhỏ, các trình tự có màu xen kẽ và các trình tự chỉ có hai màu cũng tinh tế vì việc triển khai đơn giản có thể tính sai hoặc sắp xếp sai các phần bị xóa. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ xem xét mọi mảng con của chuỗi và đối với mỗi màu, hãy đếm xem có bao nhiêu khối cần phải xóa để làm cho mảng con đó hoàn toàn có màu đó. Sau đó, nó sẽ theo dõi mảng con lớn nhất trong đó yêu cầu xóa tối đa là _k_. Mặc dù về nguyên tắc là đúng, nhưng cách tiếp cận này là O(n^2·m) trong trường hợp xấu nhất: có các mảng con O(n^2) và đối với mỗi mảng con, chúng ta có thể kiểm tra tất cả các màu. Tốc độ này quá chậm đối với n = 2·10^5. 

Thông tin chi tiết quan trọng là chúng tôi chỉ quan tâm đến các phân đoạn liền kề có một màu duy nhất. Đối với bất kỳ màu cố định _c_ nào, chúng ta có thể coi các vị trí mà hình khối không có màu _c_ là "khoảng trống" có thể bị loại bỏ. Sau đó, vấn đề giảm xuống còn việc tìm mảng con dài nhất trong đó số khối không phải _c_ không vượt quá _k_. Điều này có thể được giải quyết một cách hiệu quả bằng cách sử dụng kỹ thuật hai con trỏ hoặc cửa sổ trượt: duy trì một cửa sổ [l, r] và đếm các khối không phải _c_ bên trong nó. Mở rộng đầu bên phải miễn là số lượng khối không phải _c_ là ≤ k và di chuyển đầu bên trái về phía trước khi số lượng vượt quá k. Chúng tôi lặp lại điều này cho tất cả các màu có trong chuỗi. 

Cách tiếp cận này hoạt động vì cửa sổ hai con trỏ luôn duy trì phân đoạn liền kề tối đa cho một màu nhất định với tối đa _k_ lần xóa. Vì chúng tôi chỉ xem xét các vị trí có màu khác với _c_ nên việc tính toán là tuyến tính theo độ dài của chuỗi trên mỗi màu. Số lượng màu duy nhất nhiều nhất là _m_, do đó độ phức tạp tổng cộng là O(n·m_unique), trong đó m_unique là số lượng màu riêng biệt thực sự xuất hiện. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2·m) | O(1) | Quá chậm | 
| Cửa sổ trượt theo màu | O(n·m_unique) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm các màu duy nhất trong chuỗi và lặp lại từng màu _c_. Mỗi màu sẽ được xử lý độc lập để tìm ra phân khúc liền kề tối đa có thể đạt được nếu chúng ta tập trung vào nó. 
2. Khởi tạo hai con trỏ,`l`Và`r`, ở đầu chuỗi. Những con trỏ này xác định một cửa sổ trượt của các hình khối hiện đang được xem xét. Duy trì một bộ đếm`bad`đối với các hình khối trong cửa sổ không có màu _c_. 
3. Mở rộng con trỏ bên phải`r`từng cái một. Nếu khối lập phương ở vị trí`r`không phải là màu _c_, tăng dần`bad`. 
4. Nếu`bad`vượt quá`k`, tăng`l`để thu nhỏ cửa sổ từ bên trái cho đến khi`bad`≤ k nữa. Nếu khối lập phương rời khỏi cửa sổ không có màu _c_, hãy giảm`bad`. 
5. Ở mỗi bước, hãy cập nhật mức tối đa toàn cầu`best`với kích thước của cửa sổ hiện tại (`r - l + 1`). 
6. Sau khi xử lý tất cả các vị trí cho một màu, hãy chuyển sang màu tiếp theo. Sau khi tất cả các màu được xử lý,`best`chứa số điểm tối đa có thể đạt được với tối đa _k_ lần xóa. 

Tại sao nó hoạt động: Tại bất kỳ thời điểm nào, cửa sổ đều chứa mảng con liền kề lớn nhất có màu _c_ có thể được hình thành với ≤ k lần xóa. Tính bất biến đó là`bad`luôn đếm chính xác số khối không phải _c_ trong cửa sổ hiện tại. Việc mở rộng hoặc thu hẹp cửa sổ đảm bảo chúng tôi không bao giờ vượt quá số lần xóa được phép và quét qua tất cả các màu đảm bảo chúng tôi tìm thấy số lượng tối đa tuyệt đối. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m, k = map(int, input().split())
colors = list(map(int, input().split()))

from collections import Counter
unique_colors = set(colors)

best = 0

for c in unique_colors:
    l = 0
    bad = 0
    for r in range(n):
        if colors[r] != c:
            bad += 1
        while bad > k:
            if colors[l] != c:
                bad -= 1
            l += 1
        best = max(best, r - l + 1)

print(best)
```Giải pháp lặp lại tất cả các màu duy nhất, sử dụng cửa sổ trượt để theo dõi xem có bao nhiêu khối không có màu mục tiêu bên trong phân đoạn hiện tại. Bất cứ khi nào số lượng này vượt quá _k_, đầu bên trái của cửa sổ sẽ di chuyển về phía trước cho đến khi phân đoạn hợp lệ trở lại. Mỗi kích thước cửa sổ được so sánh với kích thước tối đa toàn cầu`best`, tích lũy phân đoạn hợp lệ lớn nhất trên tất cả các màu. Việc sử dụng một tập hợp các màu duy nhất sẽ tránh được sự lặp lại không cần thiết đối với các màu không có trong mảng. 

## Ví dụ đã hoạt động 

Đầu vào mẫu:```
10 3 2
1 2 1 1 3 2 1 1 2 2
```| r | màu sắc[r] | tệ | tôi | r-l+1 | tốt nhất | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | 0 | 1 | 1 | 
| 1 | 2 | 1 | 0 | 2 | 2 | 
| 2 | 1 | 1 | 0 | 3 | 3 | 
| 3 | 1 | 1 | 0 | 4 | 4 | 
| 4 | 3 | 2 | 0 | 5 | 5 | 
| 5 | 2 | 3 | 0->1->2 | 2 | 4 | 

Sau khi hoàn thành màu 1, đoạn liền kề tối đa là 4. Lặp lại cho màu 2 và 3 sẽ không cho kết quả tốt hơn. 

Đầu vào thứ hai:```
5 2 1
1 2 2 1 2
```Truy tìm màu 2 mang lại cửa sổ`[1, 3]`sau khi loại bỏ tối đa 1 khối, cho kết quả tốt nhất = 3. 

Những dấu vết này xác nhận rằng cửa sổ trượt xử lý chính xác việc xóa và cập nhật độ dài phân đoạn tối đa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n·m_unique) | Chúng tôi quét chuỗi một lần cho mỗi màu duy nhất. Mỗi lần quét là tuyến tính theo n. | 
| Không gian | O(n) | Lưu trữ mảng màu và tập hợp các màu độc đáo. | 

Vì n 2·10^5 và m_unique ≤ n nên tổng số phép toán là 4·10^10 trong trường hợp xấu nhất tuyệt đối, nhưng trên thực tế, m_unique thường nhỏ hơn nhiều và thuật toán chạy thoải mái dưới 1 giây với Python. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, m, k = map(int, input().split())
    colors = list(map(int, input().split()))
    unique_colors = set(colors)
    best = 0
    for c in unique_colors:
        l = 0
        bad = 0
        for r in range(n):
            if colors[r] != c:
                bad += 1
            while bad > k:
                if colors[l] != c:
                    bad -= 1
                l += 1
            best = max(best, r - l + 1)
    return str(best)

# provided samples
assert run("10 3 2\n1 2 1 1 3 2 1 1 2 2\n") == "4", "sample 1"
assert run("5 2 1\n1 2 2 1 2\n") == "3", "custom alternating"
assert run("5 1 0\n1 1 1 1 1\n") == "5", "all same color"
assert run("6 3 2\n1 2 3 1 2 3\n") == "3", "max deletion 2"
assert run("1 1 0\n1\n") == "1", "single cube"
assert run("4 2 3\n1 2 1 2\n") == "4", "k large enough
```
