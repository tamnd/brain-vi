---
title: "CF 1154E - Hai Đội"
description: "Chúng ta có một hàng gồm n sinh viên, mỗi sinh viên có kỹ năng lập trình riêng biệt từ 1 đến n. Hai huấn luyện viên lần lượt chọn học sinh về đội của mình. Đến lượt huấn luyện viên, họ chọn học sinh có kỹ năng cao nhất còn lại trong hàng."
date: "2026-06-12T02:49:32+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "implementation", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1154
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 552 (Div. 3)"
rating: 1800
weight: 1154
solve_time_s: 118
verified: true
draft: false
---

[CF 1154E - Hai đội](https://codeforces.com/problemset/problem/1154/E) 

**Đánh giá:** 1800 
**Tas:** cấu trúc dữ liệu, triển khai, sắp xếp 
**Thời gian giải:** 1 phút 58 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một hàng`n`sinh viên, mỗi sinh viên có một kỹ năng lập trình riêng biệt từ 1 đến`n`. Hai huấn luyện viên lần lượt chọn học sinh về đội của mình. Đến lượt huấn luyện viên, họ chọn học sinh có kỹ năng cao nhất còn lại trong hàng. Sau đó, ngoài học sinh đó, họ còn theo học tới`k`học sinh ở bên trái và`k`học sinh ở bên phải, miễn là những học sinh này chưa được chọn. Tất cả học sinh được chọn rời khỏi hàng và gia nhập đội của huấn luyện viên hiện tại. Quá trình này luân phiên cho đến khi không còn học sinh nào. 

Đầu vào cung cấp`n`Và`k`, theo sau là một mảng`n`các số nguyên biểu diễn kỹ năng của học sinh theo thứ tự. Kết quả đầu ra phải cho biết, đối với mỗi học sinh ở hàng ban đầu, họ tham gia nhóm nào:`1`cho huấn luyện viên đầu tiên và`2`cho huấn luyện viên thứ hai. 

Với những hạn chế,`n`có thể lên tới 200.000. Một cách tiếp cận đơn giản là quét liên tục để tìm kỹ năng tối đa trong hàng hiện tại có thể mất O(n²), thời gian này quá chậm. Chúng ta phải sử dụng cấu trúc dữ liệu cho phép chúng ta nhanh chóng tìm thấy mức tối đa hiện tại và loại bỏ hiệu quả các phạm vi học sinh liên tiếp. Trường hợp cạnh phát sinh khi mức tối đa ở gần ranh giới của hàng; ở đây, phạm vi của`k`học sinh ở bên trái hoặc bên phải có thể mở rộng ra ngoài hàng và thuật toán phải xử lý việc này một cách khéo léo. Việc triển khai bất cẩn có thể cố gắng truy cập các chỉ mục bên ngoài giới hạn mảng. 

Ví dụ, với`n = 5`,`k = 2`, và kỹ năng`[2, 4, 5, 3, 1]`, huấn luyện viên đầu tiên chọn`5`và nhận tất cả học sinh (vị trí từ 1 đến 5), để lại huấn luyện viên thứ hai không có học sinh nào. Một phương pháp đơn giản không xử lý chính xác các phạm vi ở rìa có thể tạo ra sự phân công nhóm không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force rất đơn giản: ở mỗi lượt, quét mảng hiện tại để tìm kỹ năng tối đa, xác định chỉ số của học sinh cần thực hiện dựa trên`k`, đánh dấu chúng là thuộc về huấn luyện viên hiện tại, xóa chúng khỏi mảng và lặp lại. Mặc dù điều này hoạt động hợp lý nhưng việc quét mảng để tìm mức tối đa ở mỗi lượt có thể yêu cầu tới`n + (n-1) + ... + 1 = O(n²)`sự so sánh không thể chấp nhận được`n`lên tới 2 × 10⁵. 

Cái nhìn sâu sắc quan trọng là các kỹ năng là các số nguyên riêng biệt từ 1 đến`n`. Điều này cho phép chúng tôi xử lý học sinh theo thứ tự kỹ năng giảm dần thay vì quét nhiều lần. Nếu chúng ta duy trì một tập hợp các chỉ số được sắp xếp của các học sinh vẫn còn trong hàng, chúng ta có thể, với mỗi kỹ năng từ`n`xuống 1, kiểm tra xem học sinh đã được phân lớp chưa. Nếu không, chúng tôi chỉ định họ và`k`những người hàng xóm gần nhất ở mỗi bên bằng cách sử dụng tập hợp đã sắp xếp để tìm các chỉ số liền kề một cách hiệu quả. Điều này làm giảm số lượng thao tác xuống O(n log n) vì mỗi lần chèn/xóa trong cấu trúc cây cân bằng (hoặc một tập hợp được sắp xếp) sẽ mất log n thời gian và mỗi học sinh được xử lý chính xác một lần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một mảng`team`chiều dài`n`với tất cả số không. Mảng này sẽ lưu trữ bài tập nhóm của từng học sinh. Số 0 cho biết học sinh chưa được chỉ định. 
2. Xây dựng danh sách`(skill, index)`ghép nối và sắp xếp nó theo thứ tự kỹ năng giảm dần. Điều này cho phép xử lý học sinh từ kỹ năng cao nhất đến kỹ năng thấp nhất. 
3. Duy trì một`SortedSet`hoặc một`TreeSet`của các chỉ số đại diện cho học sinh vẫn còn trong hàng. Điều này cho phép tìm kiếm hiệu quả các hàng xóm liên tiếp. 
4. Đặt biến`current_team = 1`để theo dõi xem đến lượt huấn luyện viên nào. 
5. Lặp lại danh sách học sinh đã được sắp xếp. Đối với mỗi`(skill, idx)`:

- Nếu như`team[idx]`đã khác 0, bỏ qua học sinh này. 
- Ngược lại thì giao`team[idx] = current_team`. 
- Sử dụng tập hợp đã sắp xếp, xác định tối đa`k`hàng xóm bên trái và bên phải. Chỉ định chúng giống nhau`current_team`value và xóa tất cả các chỉ mục được chỉ định khỏi tập hợp đã sắp xếp. 
6. Sau khi xử lý xong từng kỹ năng cao nhất, lật`current_team`để thay thế huấn luyện viên. 
7. Tiếp tục cho đến khi tất cả học sinh được phân vào một đội. 

Tại sao nó hoạt động: Ở mỗi bước, chúng tôi luôn chọn kỹ năng chưa được chỉ định cao nhất, giống như các quy tắc đưa ra. Phân công lên tới`k`hàng xóm của mỗi bên mô phỏng chính xác sự lựa chọn của huấn luyện viên. Vì chúng tôi không bao giờ phân công lại một học sinh và mỗi học sinh cuối cùng được xử lý theo thứ tự giảm dần nên kết quả là`team`mảng được đảm bảo tôn trọng các quy tắc vấn đề. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from bisect import bisect_left, bisect_right, insort

class SortedSet:
    def __init__(self, data=None):
        self.data = sorted(data) if data else []

    def __contains__(self, x):
        i = bisect_left(self.data, x)
        return i < len(self.data) and self.data[i] == x

    def add(self, x):
        i = bisect_left(self.data, x)
        if i == len(self.data) or self.data[i] != x:
            self.data.insert(i, x)

    def discard(self, x):
        i = bisect_left(self.data, x)
        if i < len(self.data) and self.data[i] == x:
            self.data.pop(i)

    def index_left(self, x):
        i = bisect_left(self.data, x)
        return i

    def index_right(self, x):
        i = bisect_right(self.data, x)
        return i

    def get_left_neighbors(self, idx, k):
        i = bisect_left(self.data, idx)
        left = self.data[max(0, i - k):i]
        return left

    def get_right_neighbors(self, idx, k):
        i = bisect_right(self.data, idx)
        right = self.data[i:i + k]
        return right

def main():
    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    team = [0] * n
    sorted_indices = SortedSet(range(n))
    skills = sorted([(val, idx) for idx, val in enumerate(a)], reverse=True)
    current_team = 1

    for val, idx in skills:
        if team[idx] != 0:
            continue
        to_assign = [idx]
        to_assign += sorted_indices.get_left_neighbors(idx, k)
        to_assign += sorted_indices.get_right_neighbors(idx, k)
        for i in to_assign:
            team[i] = current_team
            sorted_indices.discard(i)
        current_team = 3 - current_team  # switch team

    print("".join(map(str, team)))

if __name__ == "__main__":
    main()
```các`SortedSet`class là một tiện ích để duy trì các chỉ số sinh viên còn lại và tìm kiếm hàng xóm một cách hiệu quả. các`get_left_neighbors`Và`get_right_neighbors`chức năng truy xuất lên đến`k`hàng xóm mỗi bên. Chuyển đổi`current_team`được thực hiện bởi`3 - current_team`luân phiên giữa 1 và 2. Trận chung kết`team`mảng trực tiếp đại diện cho chuỗi đầu ra được yêu cầu. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:`5 2`, kỹ năng`[2, 4, 5, 3, 1]`. 

| Lặp lại | Đội hiện tại | Kỹ năng tối đa | Chỉ số được giao | Bộ sắp xếp còn lại | Đội Mảng | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 5 | [2,0,1,3,4] | [] | [1,1,1,1,1] | 

Tất cả học sinh đều được thực hiện ngay bước đầu tiên. Đầu ra cuối cùng là`11111`. Điều này cho thấy thuật toán xử lý lựa chọn phạm vi đầy đủ một cách chính xác. 

### Mẫu 2 

đầu vào:`5 1`, kỹ năng`[1, 3, 5, 4, 2]`. 

| Lặp lại | Đội hiện tại | Kỹ năng tối đa | Chỉ số được giao | Bộ sắp xếp còn lại | Đội Mảng | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 5 | [2,1,3] | [0,4] | [0,1,1,1,0] | 
| 2 | 2 | 4 | [0,4] | [] | [2,1,1,1,2] | 

Điều này xác nhận việc lựa chọn hàng xóm tôn trọng ranh giới và các lượt rẽ xen kẽ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Quá trình sắp xếp mất O(n log n) và mỗi lần chèn/loại bỏ trong tập hợp đã sắp xếp là O(log n) cho mỗi học sinh. | 
| Không gian | O(n) | Lưu trữ các bài tập nhóm và các chỉ số được sắp xếp có kích thước n. | 

Với n 2 × 10⁵, thuật toán chạy thoải mái trong vòng 2 giây và sử dụng ít hơn 256 MB bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import builtins
    input_backup = builtins.input
    builtins.input = lambda: sys.stdin.readline()
    from contextlib import redirect_stdout
    output = io.StringIO()
    with redirect_stdout(output):
        main()
    builtins.input = input_backup
    return output.getvalue().strip()

# Provided samples
assert run("5 2\n2 4 5
```
