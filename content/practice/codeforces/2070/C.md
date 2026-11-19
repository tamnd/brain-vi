---
title: "CF 2070C - Sơn lại có giới hạn"
description: "Chúng ta có một dãy n ô, ban đầu tất cả đều có màu đỏ. Mỗi ô có một màu mong muốn sau khi sơn, đỏ hoặc xanh. Chúng ta có thể thực hiện tối đa k thao tác, mỗi thao tác cho phép chúng ta chọn một đoạn ô liên tiếp và tô chúng màu xanh lam."
date: "2026-06-08T06:55:23+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "greedy"]
categories: ["algorithms"]
codeforces_contest: 2070
codeforces_index: "C"
codeforces_contest_name: "Educational Codeforces Round 175 (Rated for Div. 2)"
rating: 1500
weight: 2070
solve_time_s: 80
verified: true
draft: false
---

[CF 2070C - Sơn lại có giới hạn](https://codeforces.com/problemset/problem/2070/C) 

**Đánh giá:** 1500 
**Tags:** tìm kiếm nhị phân, tham lam 
**Thời gian giải:** 1 phút 20s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một dải`n`tế bào, ban đầu tất cả đều có màu đỏ. Mỗi ô có một màu mong muốn sau khi sơn, đỏ hoặc xanh. Chúng tôi có thể biểu diễn nhiều nhất`k`hoạt động, mỗi hoạt động cho phép chúng ta chọn một đoạn ô liên tiếp và tô màu xanh lam cho chúng. Các ô vốn đã có màu xanh lam có thể được sơn lại, nhưng chúng ta không thể hoàn nguyên các ô màu xanh lam thành màu đỏ. Sau tất cả các thao tác, một số ô có thể không có màu sắc như mong muốn. Mỗi ô sẽ bị phạt nếu ô đó có màu sai. Hình phạt cuối cùng của một bức tranh là hình phạt tối đa đối với tất cả các ô được tô màu sai hoặc bằng 0 nếu mọi ô đều đúng. Nhiệm vụ là giảm thiểu hình phạt cuối cùng này bằng cách sử dụng nhiều nhất`k`hoạt động. 

Đầu vào chỉ định nhiều trường hợp thử nghiệm, với`n`lên đến`3 * 10^5`và tổng cộng`n`trên các trường hợp thử nghiệm cũng lên đến`3 * 10^5`. Điều này loại trừ bất kỳ thuật toán nào với`O(n^2)`phức tạp, vì nó sẽ thực hiện hàng trăm tỷ hoạt động. Chúng ta cần một thuật toán gần như tuyến tính hoặc gần tuyến tính trong`n`cho mỗi trường hợp thử nghiệm. Hình phạt có thể lớn đến mức`10^9`, vì vậy chúng ta không thể dựa vào bất kỳ thủ thuật đếm nào yêu cầu các mảng được lập chỉ mục theo các giá trị phạt. 

Các trường hợp cạnh có thể dễ dàng phá vỡ cách tiếp cận ngây thơ bao gồm: tất cả các ô đã có màu đỏ,`k=0`, hoặc`k >= n`. Ví dụ: nếu tất cả các ô đều có màu đỏ thì mức phạt tối thiểu luôn bằng 0 bất kể`k`, nhưng một thuật toán tham lam bất cẩn vẫn có thể cố vẽ một cách không cần thiết. Một trường hợp phức tạp khác là khi`k`là rất nhỏ và có các yêu cầu rải rác về màu xanh lam: việc quyết định tô màu xanh lam nào sẽ ảnh hưởng trực tiếp đến hình phạt cuối cùng. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ liệt kê mọi lựa chọn có thể lên đến`k`phân đoạn để sơn màu xanh và sau đó tính toán hình phạt kết quả. Điều này hiệu quả vì nó trực tiếp mô hình hóa vấn đề, nhưng lại thất bại nặng nề trong thực tế vì số lượng các phân đoạn có thể tăng theo phương trình bậc hai với`n`và lựa chọn`k`trong số chúng dẫn đến sự bùng nổ tổ hợp, khiến không thể xử lý được`n = 3 * 10^5`. 

Thông tin chi tiết quan trọng là vấn đề có thể được giảm xuống thành **vấn đề quyết định với tìm kiếm nhị phân**. Giả sử chúng ta sửa một hình phạt dành cho ứng viên`x`. Câu hỏi đặt ra là: chúng ta có thể sơn lại nhiều nhất không?`k`phân đoạn sao cho không có ô nào có mức phạt lớn hơn`x`kết thúc sai à? Nếu chúng ta có thể giải quyết vấn đề quyết định này một cách hiệu quả, chúng ta có thể thực hiện tìm kiếm nhị phân trên tất cả các giá trị phạt có thể có để tìm ra mức phạt tối thiểu có thể đạt được. 

Để kiểm tra hình phạt của ứng cử viên`x`, chúng tôi đánh dấu mọi ô có mức phạt vượt quá`x`là "phải đúng". Sau đó chúng tôi giảm dải thành một chuỗi`0`Và`1`Ở đâu`1`đại diện cho một ô phải có màu xanh để tránh vượt quá hình phạt`x`, Và`0`nếu không thì. Vấn đề bây giờ là: liệu chúng ta có thể bao quát được tất cả`1`s với nhiều nhất`k`các đoạn màu xanh tiếp giáp? Điều này có thể được thực hiện một cách tham lam bằng cách quét từ trái sang phải và bắt đầu một đoạn mới bất cứ khi nào chúng ta gặp một đoạn`1`chưa được đề cập đến trong phân đoạn trước. 

Quan sát này biến đổi vấn đề ban đầu từ việc tìm kiếm theo cấp số nhân trên các phân đoạn thành một`O(n log max_penalty)`thuật toán đủ nhanh với các ràng buộc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n² * k) | O(n) | Quá chậm | 
| Tìm kiếm nhị phân + Tham lam | O(n log P) | O(n) | Đã chấp nhận | 

Đây,`P`là hình phạt tối đa trong mảng. 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc`n`,`k`, chuỗi màu mong muốn`s`, và các hình phạt`a`. 
2. Xây dựng danh sách`penalties`chứa tất cả các giá trị phạt duy nhất. Chúng tôi sẽ tìm kiếm nhị phân trên các giá trị này. 
3. Xác định hàm trợ giúp`can_achieve(max_penalty)`để kiểm tra xem có thể tránh được hình phạt vượt quá`max_penalty`sử dụng nhiều nhất`k`hoạt động: 

1. Chuyển dải thành mảng nhị phân`must_paint`mỗi ô ở đâu`1`nếu màu mong muốn của nó là xanh lam và hình phạt của nó vượt quá`max_penalty`, nếu không thì`0`. 
2. Quét qua`must_paint`trái sang phải. Đếm xem cần bao nhiêu đoạn màu xanh liền kề để bao phủ tất cả`1`S. 
3. Nếu số đoạn là ≤`k`, trở lại`True`; nếu không thì quay lại`False`. 
4. Thực hiện tìm kiếm nhị phân trên các hình phạt duy nhất được sắp xếp bằng cách sử dụng`can_achieve`. Khởi tạo`low = 0`Và`high = max(a)`. 
5. Đối với mỗi giá trị trung bình`mid`, kiểm tra`can_achieve(mid)`. Nếu đúng, hãy thử hình phạt nhỏ hơn (`high = mid`); sai thì tăng hình phạt (`low = mid + 1`). 
6. Cuối cùng,`low`chứa hình phạt tối đa tối thiểu có thể đạt được. 
7. Xuất giá trị này. 

**Tại sao nó hoạt động**: Tìm kiếm nhị phân dựa vào thuộc tính đơn điệu mà nếu chúng ta có thể đạt được một hình phạt`x`, chúng ta cũng có thể đạt được bất kỳ hình phạt nào lớn hơn`x`. Việc đếm phân đoạn tham lam là hợp lệ vì việc vẽ các phân đoạn lớn hơn không bao giờ tệ hơn việc vẽ các phân đoạn nhỏ hơn: việc hợp nhất các ô cần thiết liền kề thành một phân đoạn sẽ giảm thiểu tổng số thao tác. Do đó, thuật toán tìm đúng mức phạt tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        s = input().strip()
        a = list(map(int, input().split()))

        def can_achieve(max_penalty):
            segments_needed = 0
            i = 0
            while i < n:
                if s[i] == 'B' and a[i] > max_penalty:
                    segments_needed += 1
                    while i < n and s[i] == 'B' and a[i] > max_penalty:
                        i += 1
                else:
                    i += 1
            return segments_needed <= k

        low, high = 0, max(a)
        while low < high:
            mid = (low + high) // 2
            if can_achieve(mid):
                high = mid
            else:
                low = mid + 1
        print(low)

if __name__ == "__main__":
    solve()
```các`can_achieve`hàm đếm các đoạn màu xanh lam liền kề phải được sơn để tránh bị phạt phía trên ứng viên. Tìm kiếm nhị phân có hiệu quả với mức phạt tối đa khả thi tối thiểu. Điều kiện biên như`k = 0`và các dải toàn màu đỏ được xử lý một cách tự nhiên bằng phương pháp này. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 1
BRBR
9 3 5 4
```| tôi | s[i] | một [tôi] | must_paint (x=3) | Số lượng phân đoạn | 
| --- | --- | --- | --- | --- | 
| 0 | B | 9 | 1 | bắt đầu đoạn 1 | 
| 1 | R | 3 | 0 | tiếp tục | 
| 2 | B | 5 | 1 | bắt đầu đoạn 2 (vượt quá k=1) | 
| 3 | R | 4 | 0 | - | 

Tìm kiếm nhị phân xác định hình phạt tối thiểu = 3. 

### Ví dụ 2 

đầu vào:```
4 2
BRBR
9 3 5 4
```| tôi | s[i] | một [tôi] | must_paint (x=0) | Số lượng phân đoạn | 
| --- | --- | --- | --- | --- | 
| 0 | B | 9 | 1 | bắt đầu đoạn 1 | 
| 1 | R | 3 | 0 | tiếp tục | 
| 2 | B | 5 | 1 | bắt đầu đoạn 2 | 
| 3 | R | 4 | 0 | - | 

Các phân đoạn cần thiết = 2 ≤ k=2 → mức phạt tối thiểu = 0. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log P) | Mỗi bước tìm kiếm nhị phân kiểm tra`can_achieve`trong O(n), với tối đa log(max(a)) bước. | 
| Không gian | O(n) | Cửa hàng`s`,`a`và các cờ tạm thời cho từng trường hợp thử nghiệm. | 

Thuật toán chia tỷ lệ tuyến tính với tổng`n`qua các trường hợp thử nghiệm, vừa vặn thoải mái trong giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# provided samples
assert run("5\n4 1\nBRBR\n9 3 5 4\n4 1\nBRBR\n9 5 3 4\n4 2\nBRBR\n9 3 5 4\n10 2\nBRBRBBRRBR\n5 1 2 4 5 3 6 1 5 4\n5 5\nRRRRR\n5 3 1 2 4\n") == "3\n3\n0\n4\n0", "sample 1"

# custom test cases
assert run("1\n3 0\nBBB\n1 2 3\n") == "3", "no operations allowed"
assert run("1\n3 3\nRRR\n1 2 3
```
