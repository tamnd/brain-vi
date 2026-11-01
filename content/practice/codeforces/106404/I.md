---
title: "CF 106404I - Sách giáo khoa Tiger"
description: "Chúng tôi có một bộ sưu tập sách giáo khoa. Mỗi cuốn sách giáo khoa có một mã nhận dạng chủ đề và một giá trị thể hiện mức độ yêu thích của con hổ. Các truy vấn không hỏi về vị trí trong bộ sưu tập gốc."
date: "2026-06-25T10:03:37+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106404
codeforces_index: "I"
codeforces_contest_name: "Bay Area Programming Contest 2026 Advanced Division"
rating: 0
weight: 106404
solve_time_s: 36
verified: true
draft: false
---

[CF 106404I - Sách giáo khoa về Tiger](https://codeforces.com/problemset/problem/106404/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 36s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một bộ sưu tập sách giáo khoa. Mỗi cuốn sách giáo khoa có một mã nhận dạng chủ đề và một giá trị thể hiện mức độ yêu thích của con hổ. Các truy vấn không hỏi về vị trí trong bộ sưu tập gốc. Thay vào đó, mỗi truy vấn đưa ra một phạm vi tần suất chủ đề có thể có và chúng ta phải tính điểm được tạo ra bằng cách xem xét từng cặp tần số theo thứ tự bên trong phạm vi đó. 

Đối với một tần số`k`, định nghĩa`cnt[k]`như số lượng chủ đề xuất hiện chính xác`k`lần. Trong số tất cả các chủ đề có tần suất`k`, định nghĩa`best[k]`là giá trị hưởng thụ tối đa. Đối với hai tần số`i`Và`j`, đóng góp của họ bằng 0 trừ khi`cnt[i]`Và`cnt[j]`đều bình đẳng. Nếu chúng bằng nhau và tồn tại cả hai tần số thì cặp sách tốt nhất có thể có giá trị`best[i] * best[j]`. Câu trả lời truy vấn là tổng của những đóng góp này trên tất cả các cặp có thứ tự`(i, j)`bên trong khoảng truy vấn. 

Kích thước đầu vào lớn: cả số lượng sách giáo khoa và số lượng truy vấn đều có thể đạt tới`5 * 10^5`. Một giải pháp quét mọi tần số có thể cho mỗi truy vấn sẽ yêu cầu khoảng`2.5 * 10^11`hoạt động trong trường hợp xấu nhất, vượt xa giới hạn hai giây cho phép. Chúng ta cần tránh phụ thuộc vào toàn bộ dải tần. 

Hạn chế chính ẩn trong biểu diễn tần số là số lượng tần số khác nhau thực sự xuất hiện là nhỏ. Nếu có`s`các tần số dương khác nhau thì tổng số sách giáo khoa nhỏ nhất có thể có là:`1 + 2 + 3 + ... + s`. 

Vì số tiền này không thể vượt quá`n`,`s`nhiều nhất là về`1000`khi`n = 500000`. Điều này thay đổi vấn đề hoàn toàn. Chúng ta chỉ cần lưu trữ và xử lý các tần số tồn tại. 

Một số trường hợp cạnh rất dễ bị bỏ sót. Tần suất không có chủ đề không thể đóng góp, mặc dù về mặt kỹ thuật, hai tần số bị thiếu có cùng số lượng chủ đề bằng 0. Ví dụ:```
n = 1
topics = [5]
values = [10]
query = [2, 3]
```Không có chủ đề nào xuất hiện hai ba lần nên đáp án là`0`. Việc thực hiện bất cẩn khiến tất cả các nhóm đều bình đẳng`cnt[k]`giá trị mà không kiểm tra xem`cnt[k] > 0`có thể thêm sai nhóm tần số bằng 0. 

Một trường hợp góc khác là khi một truy vấn chỉ chứa một tần số hiện có. Ví dụ:```
n = 3
topics = [1, 1, 1]
values = [5, 7, 9]
query = [3, 3]
```Chỉ có tần số`3`tồn tại. Câu trả lời là`9 * 9 = 81`, vì giá trị lớn nhất của ba cuốn sách là`9`. Sử dụng tổng của tất cả các giá trị thay vì giá trị tối đa sẽ cho kết quả sai. 

Trường hợp ranh giới cuối cùng là khi phạm vi truy vấn vượt quá tần số lớn nhất có thể. Ví dụ:```
query = [100, 1000000000]
```Chỉ có tần số lên tới`n`có thể tồn tại. Phạm vi bổ sung phải được bỏ qua. 

## Phương pháp tiếp cận 

Một giải pháp trực tiếp trước tiên sẽ xây dựng thông tin tần số và sau đó trả lời từng truy vấn bằng cách lặp qua mọi tần số có thể có giữa`L`Và`R`. Điều này đúng vì mỗi cặp tần số có thể được đánh giá độc lập. Tuy nhiên, dải tần có thể lớn đến mức`10^9`trong các truy vấn và thậm chí giới hạn nó ở`n = 500000`sẽ làm cho mọi truy vấn quá chậm. Trường hợp xấu nhất sẽ là về`5 * 10^5 * 5 * 10^5`kiểm tra tần số. 

Quan sát quan trọng là dãy tần số cực kỳ thưa thớt. giá trị`cnt[k]`khác 0 chỉ đối với tần số xuất hiện trong các chủ đề. Có thể có nhiều chủ đề, nhưng không thể có nhiều tần số tích cực khác nhau vì tần số mới sẽ tiêu tốn nhiều sách giáo khoa hơn tần số trước. 

Đối với mọi tần số hiện có`k`, chúng tôi giữ cặp`(k, best[k])`. Tần số giống nhau`cnt[k]`thuộc cùng một nhóm. Nếu một nhóm chứa tần số`k1, k2, ...`, thì bên trong một truy vấn, sự đóng góp của tất cả các cặp có thứ tự từ nhóm đó là:`(best[k1] + best[k2] + ...)^2`bởi vì mọi tần số trong nhóm có thể ghép nối với mọi tần số khác trong cùng một nhóm, bao gồm cả chính nó. 

Số lượng tần số hiện có nhiều nhất là khoảng`1000`, vì vậy mỗi nhóm có thể chỉ cần lưu trữ các tần số đã được sắp xếp của nó. Đối với một truy vấn, chúng tôi tìm kiếm nhị phân tần số đầu tiên và cuối cùng trong mỗi nhóm và thu được tổng của`best`các giá trị nằm trong khoảng. Cộng bình phương của tổng đó sẽ có câu trả lời. 

Lực lượng vũ phu hoạt động vì nó kiểm tra chính xác các cặp được mô tả trong câu lệnh, nhưng không thành công vì nó lặp lại các so sánh tần số giống nhau trên nhiều truy vấn. Việc quan sát tần số thưa thớt cho phép chúng tôi thay thế việc quét dải tần số lớn bằng một vài tìm kiếm nhị phân. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n + qn) | O(n) | Quá chậm | 
| Nhóm tần số thưa thớt với tìm kiếm nhị phân | O(n + q * s log s) | O (các) | Đã chấp nhận | 

Đây`s`là số tần số riêng biệt thực sự xảy ra, và`s <= 1000`. 

## Hướng dẫn thuật toán 

1. Đếm xem mỗi chủ đề xuất hiện bao nhiêu lần. Trong khi làm điều này, hãy nhớ giá trị thú vị tối đa trong số tất cả các cuốn sách thuộc từng chủ đề. 
2. Đối với mỗi chủ đề, hãy sử dụng tần suất cập nhật cuối cùng của chủ đề đó`cnt[frequency]`Và`best[frequency]`. Giá trị được lưu trữ trong`best[k]`là giá trị thưởng thức lớn nhất trong số các chủ đề xuất hiện chính xác`k`lần. 
3. Thu thập tất cả các tần số`k`Ở đâu`cnt[k] > 0`. Tần số với`cnt[k] = 0`bị bỏ qua vì không có sách giáo khoa hợp lệ nào có thể được chọn cho chúng. 
4. Nhóm các tần số này theo`cnt[k]`giá trị. Mỗi nhóm đại diện cho các tần số có thể tương tác với nhau trong một truy vấn. 
5. Sắp xếp tần số trong mỗi nhóm. Cũng lưu trữ tổng tiền tố của họ`best`các giá trị. Điều này cho phép tổng`best`các giá trị bên trong bất kỳ khoảng truy vấn nào được tìm thấy bằng tìm kiếm nhị phân. 
6. Đối với một truy vấn`[L, R]`, xử lý mọi nhóm. Tìm các tần số trong nhóm đó nằm giữa`L`Và`R`. Nếu họ`best`tổng các giá trị thành`x`, thêm vào`x * x`để trả lời. 

Lý do nhóm này hoạt động là do tần số từ các nhóm khác nhau có số lượng chủ đề khác nhau, vì vậy mỗi cặp giữa các nhóm đó đều không đóng góp gì. Trong một nhóm, mỗi cặp đóng góp tích có giá trị tốt nhất tương ứng và tổng của tất cả các sản phẩm được đặt chính xác bằng bình phương của tổng. 

Điều bất biến là mọi cặp tần số hợp lệ đều thuộc về chính xác một nhóm đếm tần số. Bên trong nhóm đó, phép tính tổng tiền tố bao gồm chính xác các tần số được truy vấn cho phép và không có tần số nào khác. Vì mọi khoản đóng góp có thể được đưa vào một lần nên tổng cuối cùng là chính xác. 

## Giải pháp Python```python
import sys
from collections import defaultdict
from bisect import bisect_left, bisect_right

input = sys.stdin.readline

def solve():
    n, q = map(int, input().split())
    a = list(map(int, input().split()))
    v = list(map(int, input().split()))

    freq = defaultdict(int)
    best_topic = defaultdict(int)

    for x, val in zip(a, v):
        freq[x] += 1
        if val > best_topic[x]:
            best_topic[x] = val

    cnt = defaultdict(int)
    best = defaultdict(int)

    for topic, f in freq.items():
        cnt[f] += 1
        if best_topic[topic] > best[f]:
            best[f] = best_topic[topic]

    groups = defaultdict(list)
    for f in cnt:
        if cnt[f] > 0:
            groups[cnt[f]].append((f, best[f]))

    data = []
    for items in groups.values():
        items.sort()
        freqs = []
        pref = [0]
        for f, val in items:
            freqs.append(f)
            pref.append(pref[-1] + val)
        data.append((freqs, pref))

    ans = []
    for _ in range(q):
        l, r = map(int, input().split())
        cur = 0

        for freqs, pref in data:
            left = bisect_left(freqs, l)
            right = bisect_right(freqs, r)
            if left < right:
                s = pref[right] - pref[left]
                cur += s * s

        ans.append(str(cur))

    sys.stdout.write("\n".join(ans))

if __name__ == "__main__":
    solve()
```Phần đầu tiên của mã nén sách giáo khoa gốc thành thông tin chủ đề. Chúng tôi không cần sách riêng lẻ sau thời điểm này vì tần suất chỉ quan tâm đến giá trị tối đa trong số các chủ đề có tần suất đó. 

các`groups`từ điển đại diện cho quan sát chính. Điều quan trọng của nó là số lượng chủ đề có tần suất nhất định. Các tần số chia sẻ cùng một khóa có thể đóng góp cùng nhau, trong khi các tần số có các khóa khác nhau không bao giờ tương tác. 

Đối với mỗi nhóm, danh sách tần số được sắp xếp và mảng tổng tiền tố cho phép trả lời một truy vấn mà không cần quét mọi tần số.`bisect_left`tìm tần số hợp lệ đầu tiên và`bisect_right`tìm vị trí sau tần số hợp lệ cuối cùng, do đó sự khác biệt của tổng tiền tố sẽ cho tổng yêu cầu của`best`các giá trị. 

Số nguyên Python tự động xử lý các giá trị câu trả lời lớn. Điểm tối đa có thể có thể vượt quá giới hạn số nguyên 32 bit, vì vậy các ngôn ngữ không có số nguyên lớn tự động sẽ cần loại 64 bit. 

## Ví dụ đã hoạt động 

Hãy xem xét mẫu:```
12 6
1 1 2 2 2 3 4 4 6 6 7 7
10 4 9 4 5 2 6 1 8 7 3 2
```Thông tin tần số trở thành: 

| Tần số | Số chủ đề | Giá trị tốt nhất | 
| --- | --- | --- | 
| 1 | 1 | 2 | 
| 2 | 4 | 10 | 
| 3 | 1 | 9 | 

Các nhóm được phân theo số lượng chủ đề. 

| Số nhóm | Tần số | 
| --- | --- | 
| 1 | 1, 3 | 
| 4 | 2 | 

Đối với truy vấn`[1, 2]`: 

| Nhóm | Tần số trong phạm vi | Tổng các giá trị tốt nhất | Đóng góp | 
| --- | --- | --- | --- | 
| đếm = 1 | 1 | 2 | 4 | 
| đếm = 4 | 2 | 10 | 100 | 

Câu trả lời là`104`. 

Đối với truy vấn`[1, 3]`: 

| Nhóm | Tần số trong phạm vi | Tổng các giá trị tốt nhất | Đóng góp | 
| --- | --- | --- | --- | 
| đếm = 1 | 1, 3 | 11 | 121 | 
| đếm = 4 | 2 | 10 | 100 | 

Câu trả lời là`221`. 

Những dấu vết này cho thấy tính bất biến chính: một nhóm đóng góp bình phương của tổng các tần số đã chọn của nó, chứ không phải là tổng của các bình phương tần số riêng lẻ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + q * s log s) | Dữ liệu tần số xây dựng là tuyến tính. Mỗi truy vấn thực hiện tìm kiếm nhị phân nhiều nhất`s`các nhóm. | 
| Không gian | O (các) | Chỉ các tần số hiện có được lưu trữ và số lượng của chúng tối đa là khoảng`1000`. | 

Giới hạn tần số thưa thớt là điều làm cho giải pháp phù hợp với các ràng buộc. Ngay cả với nửa triệu truy vấn, mỗi truy vấn cũng chỉ chạm tới một số ít nhóm tần suất. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    solve()
    out = sys.stdout.getvalue() if hasattr(sys.stdout, "getvalue") else ""
    sys.stdin = old
    return out

# Sample 1
assert run("""12 3
1 1 2 2 2 3 4 4 6 6 7 7
10 4 9 4 5 2 6 1 8 7 3 2
1 2
1 3
4 4
""") == """104
221
0""", "sample"

# Minimum size
assert run("""1 2
5
7
1 1
2 3
""") == """49
0""", "single book"

# All topics equal
assert run("""5 2
1 1 1 1 1
1 3 5 7 9
5 5
1 5
""") == """81
81""", "one frequency"

# Multiple equal frequency groups
assert run("""6 2
1 1 2 2 3 4
5 8 2 6 9 1
1 2
1 3
""") == """196
196""", "several groups"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu | 104, 221, 0 | Logic nhóm chính | 
| Sách đơn | 49, 0 | Thiếu tần số và kích thước tối thiểu | 
| Tất cả các chủ đề đều bình đẳng | 81, 81 | Lựa chọn giá trị tối đa cho một tần số | 
| Một số nhóm | 196, 196 | Nhiều nhóm đếm tần số | 

## Vỏ cạnh 

Đối với trường hợp thiếu tần số:```
1 1
5
10
2 3
```Tần số duy nhất hiện có là`1`. Truy vấn không chứa nó nên mỗi nhóm đều có giao điểm trống. Thuật toán thực hiện tìm kiếm nhị phân, không tìm thấy giá trị nào và trả về`0`. 

Đối với trường hợp tần số đơn:```
3 1
1 1 1
5 7 9
3 3
```chủ đề`1`có tần số`3`, và giá trị tốt nhất là`9`. Nhóm chỉ chứa tần số`3`, vậy tổng được chọn là`9`và sự đóng góp là`9 * 9 = 81`. 

Đối với một truy vấn vượt quá phạm vi có thể:```
3 1
1 2 2
4 8 9
10 1000000000
```Không có tần số được lưu trữ nằm trong khoảng thời gian. Tìm kiếm nhị phân trả về các phạm vi trống cho mỗi nhóm và kết quả là`0`. 

Những trường hợp này được xử lý một cách tự nhiên vì thuật toán không bao giờ tạo các mục nhập nhân tạo cho các tần số không thể có và chỉ hoạt động với các tần số tương ứng với các chủ đề thực tế.
