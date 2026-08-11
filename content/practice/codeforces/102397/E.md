---
title: "CF 102397E - Bashar và vùng đất xấu (Cứng)"
description: "Điều bất biến là trước khi xử lý mỗi điểm cuối bên phải mới, điểm bên trái sẽ trỏ tới ranh giới bên trái nhỏ nhất có thể sau tất cả việc thu nhỏ trước đó."
date: "2026-08-11T15:48:55+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102397
codeforces_index: "E"
codeforces_contest_name: "Asu Coding Cup 4"
rating: 0
weight: 102397
solve_time_s: 91
verified: true
draft: false
---

[CF 102397E - Bashar và vùng đất xấu (Cứng)](https://codeforces.com/problemset/problem/102397/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 31 giây 
**Đã xác minh:** có 

##Giải pháp 
## Tại sao nó hoạt động 

Điều bất biến là trước khi xử lý mỗi điểm cuối bên phải mới,`left`trỏ đến ranh giới bên trái nhỏ nhất có thể sau tất cả sự thu nhỏ trước đó. Bất cứ khi nào cửa sổ`[left, right]`đạt được mục tiêu, thuật toán sẽ loại bỏ các nhà ở bên trái miễn là mục tiêu vẫn hài lòng. Vì vậy, đối với điều đặc biệt này`right`, cửa sổ hợp lệ cuối cùng là cửa sổ hợp lệ ngắn nhất kết thúc tại`right`. Mọi phân đoạn tối ưu có thể có đều có một số điểm cuối phù hợp và khi điểm cuối đó được xử lý, thuật toán sẽ tìm thấy một cửa sổ không dài hơn nó. Do đó, việc lấy mức tối thiểu trên tất cả các điểm cuối bên phải sẽ tạo ra đoạn hợp lệ ngắn nhất trên toàn cầu. Chuyển đổi số lượng nhà từ`k`đến khoảng cách đi bộ`k - 1`đưa ra câu trả lời cần thiết. 

#Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    x, n = map(int, input().split())
    a = list(map(int, input().split()))

    left = 0
    current_sum = 0
    best = n + 1

    for right in range(n):
        current_sum += a[right]

        while current_sum >= x:
            best = min(best, right - left + 1)
            current_sum -= a[left]
            left += 1

    if best == n + 1:
        print(-1)
    else:
        print(best - 1)

if __name__ == "__main__":
    solve()
```Đầu vào được đọc một lần và mảng được lưu trữ để điểm cuối bên trái sau này có thể xóa giá trị tương ứng của nó khỏi tổng chạy. Các biến`left`Và`right`xác định đoạn liền kề hiện tại, trong khi`current_sum`lưu trữ tổng số của nó mà không cần tổng hợp lại toàn bộ phân khúc. 

Vòng lặp bên ngoài thêm mỗi ngôi nhà đúng một lần. Một lần`current_sum >= x`, vòng lặp bên trong cố gắng loại bỏ các ngôi nhà ở bên trái. biểu hiện`right - left + 1`là số lượng nhà hiện có trong phân khúc nên đây là số lượng phải giảm thiểu trong quá trình mở cửa sổ. 

Thứ tự các hoạt động bên trong vòng lặp thu hẹp rất quan trọng. Cửa sổ hiện tại hợp lệ trước đó`a[left]`bị loại bỏ nên độ dài của nó phải được xem xét trước khi thay đổi`left`. Sau khi loại bỏ, cửa sổ có thể trở nên không hợp lệ, trong trường hợp đó vòng lặp sẽ tự động dừng lại. 

Không có vấn đề tràn số nguyên trong Python. Tổng số lớn nhất có thể là`10^5 * 10^5 = 10^10`, cũng có thể được biểu diễn một cách an toàn bằng số nguyên Python. 

Cuối cùng,`best - 1`chuyển đổi số lượng nhà tối thiểu được ghé thăm thành khoảng cách đi bộ. Nếu một ngôi nhà là đủ,`best`là`1`và khoảng cách in là chính xác`0`. 

# Ví dụ đã hoạt động 

## Mẫu 1 

Hãy xem xét`x = 12`Và`a = [1, 3, 4, 5, 2]`. Cửa sổ hữu ích cuối cùng sẽ trở thành`[3, 4, 5]`, trong đó có ba ngôi nhà và có khoảng cách đi bộ là hai ngôi nhà. 

|`right`| Giá trị gia tăng |`current_sum`trước khi thu nhỏ lại |`left`| Độ dài cửa sổ tốt nhất | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 1 | 0 | không tìm thấy | 
| 1 | 3 | 4 | 0 | không tìm thấy | 
| 2 | 4 | 8 | 0 | không tìm thấy | 
| 3 | 5 | 13 | 0 | 3 | 
| 4 | 2 | 11 | 1 | 3 | 

Tại`right = 3`, tổng sẽ bằng 13. Cửa sổ`[1, 3, 4, 5]`hợp lệ thì thuật toán sẽ loại bỏ`1`, rời đi`[3, 4, 5]`với tổng bằng 12. Nó không thể loại bỏ một ngôi nhà khác vì tổng sẽ giảm xuống dưới 12. Do đó, cần có ba ngôi nhà cho điểm cuối này, tương ứng với khoảng cách`3 - 1 = 2`. 

## Mẫu 2 

cho`x = 13`Và`a = [5, 1, 2, 3, 4]`, tổng số tiền là 15, nhưng không có mảng con thích hợp nào đạt tới 13. Bắt buộc phải có toàn bộ mảng. 

|`right`| Giá trị gia tăng |`current_sum`|`left`| Độ dài cửa sổ tốt nhất | 
| --- | --- | --- | --- | --- | 
| 0 | 5 | 5 | 0 | không tìm thấy | 
| 1 | 1 | 6 | 0 | không tìm thấy | 
| 2 | 2 | 8 | 0 | không tìm thấy | 
| 3 | 3 | 11 | 0 | không tìm thấy | 
| 4 | 4 | 15 | 0 | 5 | 

Khi ngôi nhà cuối cùng được thêm vào, dãy hoàn chỉnh sẽ đạt 15. Loại bỏ ngôi nhà đầu tiên sẽ chỉ còn lại 10 ngôi nhà, thấp hơn mục tiêu nên cửa sổ 5 ngôi nhà là tối thiểu. Quãng đường đi bộ của nó là`5 - 1 = 4`. 

# Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(n)`| Con trỏ bên phải di chuyển từ trái sang phải một lần, con trỏ bên trái cũng chỉ di chuyển về phía trước nên mỗi nhà chỉ được thêm và bớt một lần. | 
| Không gian |`O(n)`| Mảng được lưu trong bộ nhớ để điểm cuối bên trái có thể xóa các giá trị của nó. | 

Với`n <= 100000`, đại khái`200000`chuyển động con trỏ là đủ cho phần cửa sổ trượt. Điều này thoải mái trong giới hạn thời gian yêu cầu và lưu trữ`100000`số nguyên nằm trong giới hạn bộ nhớ. 

# Trường hợp thử nghiệm```python
import sys
import io

def solve():
    input = sys.stdin.readline

    x, n = map(int, input().split())
    a = list(map(int, input().split()))

    left = 0
    current_sum = 0
    best = n + 1

    for right in range(n):
        current_sum += a[right]

        while current_sum >= x:
            best = min(best, right - left + 1)
            current_sum -= a[left]
            left += 1

    print(-1 if best == n + 1 else best - 1)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()
    result = sys.stdout.getvalue().strip()

    sys.stdin = old_stdin
    sys.stdout = old_stdout

    return result

# Provided samples, interpreted according to the displayed input.
assert run("12 5\n1 3 4 5 2\n") == "2", "sample 1"
assert run("13 5\n5 1 2 3 4\n") == "4", "sample 2"
assert run("6 5\n1 1 1 1 1\n") == "-1", "sample 3"

# Minimum-size input, one house is enough, so no walking is required.
assert run("7 1\n7\n") == "0", "single house exactly reaches target"

# One house is enough even though other houses exist.
assert run("5 3\n2 5 1\n") == "0", "single-house window"

# Entire array is required.
assert run("10 4\n1 2 3 4\n") == "3", "whole array required"

# All values equal, target reached by the first three houses.
assert run("9 5\n3 3 3 3 3\n") == "2", "equal values"

# Target cannot be reached.
assert run("100 4\n10 20 30 39\n") == "-1", "insufficient total"

# Maximum-size input.
n = 100000
assert run(f"{n} {n}\n" + " ".join(["1"] * n) + "\n") == str(n - 1), \
    "maximum-size all-equal input"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`7 1 / 7`|`0`| Kích thước tối thiểu và khoảng cách đi bộ bằng không | 
|`5 3 / 2 5 1`|`0`| Căn nhà duy nhất đạt chỉ tiêu | 
|`10 4 / 1 2 3 4`|`3`| Toàn bộ mảng là bắt buộc và bắt được`length`so với`distance`lỗi | 
|`9 5 / 3 3 3 3 3`|`2`| Giá trị lặp lại và thu nhỏ chính xác | 
|`100 4 / 10 20 30 39`|`-1`| Tổng số vàng không đủ | 
|`100000 100000 / 1 ... 1`|`99999`| Tối đa`n`và hành vi thời gian tuyến tính | 

# Vỏ cạnh 

Một ngôi nhà duy nhất có thể đáp ứng mục tiêu. Vì`x = 5`,`n = 3`, Và`a = [2, 7, 1]`, cửa sổ đạt đến mục tiêu khi`right = 1`. Thuật toán ghi lại độ dài cửa sổ là`1`sau khi loại bỏ phần trước`2`từ cửa sổ. Nó không thể loại bỏ`7`bởi vì điều đó sẽ làm cho tổng bằng 0, nên`best = 1`và câu trả lời là`best - 1 = 0`. Chi tiết quan trọng là việc đi bộ từ một ngôi nhà đến chính nó không tốn phí. 

Nếu tổng số vàng không đủ, vòng lặp thu hẹp sẽ không bao giờ có thể đưa ra câu trả lời cuối cùng hợp lệ. Vì`x = 10`Và`a = [2, 3, 4]`, số tiền lớn nhất đạt được là 9.`best`vẫn ở giá trị trọng điểm của nó, do đó thuật toán sẽ in`-1`thay vì coi cửa sổ một phần cuối cùng là một giải pháp. 

Khi cần toàn bộ mảng, cửa sổ chỉ tiếp cận mục tiêu ở vị trí cuối cùng. Vì`x = 10`Và`a = [1, 2, 3, 4]`, tổng trở thành 10 tại`right = 3`. Cửa sổ có chiều dài 4 và loại bỏ`1`ngay lập tức giảm tổng xuống 9, vì vậy nó là tối thiểu. Câu trả lời là`4 - 1 = 3`, đại diện cho ba chuyển động giữa bốn nhà. 

Việc chuyển đổi khoảng cách cũng có liên quan khi có nhiều ngôi nhà đạt được mục tiêu. Nếu cửa sổ tối ưu là`[l, r]`, có`r - l + 1`nhà nhưng chỉ`r - l`bước giữa ngôi nhà đầu tiên và ngôi nhà cuối cùng. Việc trả về độ dài cửa sổ trực tiếp sẽ gây ra từng lỗi một trên mỗi câu trả lời hợp lệ, kể cả trường hợp đơn giản nhất khi khoảng cách chính xác bằng 0. 

Tính tích cực của mảng là điều làm cho cửa sổ trượt có giá trị. Nếu cho phép các giá trị âm, việc loại bỏ giá trị ngoài cùng bên trái có thể làm tăng tổng và đối số rút gọn đơn điệu sẽ không còn giữ nguyên nữa. Dưới những ràng buộc đã cho, mỗi ngôi nhà đóng góp một lượng dương, do đó mỗi con trỏ chỉ di chuyển về phía trước và giới hạn tuyến tính được đảm bảo.
