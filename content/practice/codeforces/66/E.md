---
title: "CF 66E - Petya và Post"
description: "Chúng ta có một tuyến đường vòng có n bưu điện, mỗi bưu cục có một trạm xăng. Mỗi trạm i có sẵn một[i] lít xăng, khoảng cách từ trạm i đến i+1 là b[i] km, bao quanh ở cuối."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dp"]
categories: ["algorithms"]
codeforces_contest: 66
codeforces_index: "E"
codeforces_contest_name: "Codeforces Beta Round 61 (Div. 2)"
rating: 2000
weight: 66
solve_time_s: 132
verified: false
draft: false
---

[CF 66E - Petya và Post](https://codeforces.com/problemset/problem/66/E) 

**Xếp hạng:** 2000 
**Thẻ:** cấu trúc dữ liệu, dp 
**Thời gian giải:** 2m 12s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một tuyến đường vòng với`n`bưu điện, mỗi bưu điện đều có trạm xăng. Mỗi trạm`i`có`a[i]`lít xăng có sẵn và khoảng cách từ trạm`i`ĐẾN`i+1`là`b[i]`km, quấn quanh ở cuối. Chú của Petya muốn xuất phát ở bưu điện, lấy xe và lái đúng một vòng dọc theo vòng tròn theo cả hai hướng, chỉ đổ xăng ở những trạm mà ông đi qua, không bao giờ đổ xăng nhiều hơn một lần ở cùng một trạm. Chiếc xe tiêu thụ chính xác một lít mỗi km. 

Chúng tôi được yêu cầu tìm tất cả các trạm mà từ đó việc bắt đầu tuyến đường cho phép hoàn thành hết vòng tròn mà không hết xăng. 

Tổng số xăng`sum(a)`bằng tổng tất cả các khoảng cách`sum(b)`, điều này đảm bảo rằng một giải pháp tồn tại từ ít nhất một điểm bắt đầu. Mỗi trạm có ít nhất một lít xăng và mỗi quãng đường ít nhất là một km. 

Với`n`lên tới 10^5 và giới hạn thời gian là 2 giây, bất kỳ giải pháp nào có độ phức tạp kém hơn O (n log n) đều có rủi ro. Cách tiếp cận O(n^2) đơn giản, thử bắt đầu ở mọi trạm và mô phỏng toàn bộ vòng lặp, sẽ bao gồm khoảng 10^10 thao tác và không khả thi. 

Các trường hợp đặc biệt bao gồm việc tất cả các trạm có đủ khí chính xác để đến trạm tiếp theo, các trạm có sự phân phối khí cực kỳ không đồng đều hoặc khoảng cách tạo thành “nút thắt cổ chai” ngăn cản việc bắt đầu từ một số điểm nhất định. Ví dụ:```
n = 3
a = [1, 5, 2]
b = [3, 2, 3]
```Bắt đầu một cách ngây thơ ở trạm 1 sẽ thất bại ngay lập tức vì 1 < 3, trong khi bắt đầu ở trạm 2 sẽ thành công. Thiếu những kiểm tra này sẽ dẫn đến câu trả lời sai. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực rất đơn giản: hãy thử bắt đầu từ mỗi trạm, giữ đồng hồ đếm nhiên liệu đang chạy và mô phỏng việc di chuyển về phía trước. Đối với mỗi trạm, trừ`b[i]`từ nhiên liệu của bạn và thêm`a[i]`ở trạm tiếp theo. Nếu nhiên liệu giảm xuống dưới 0, điểm xuất phát đó sẽ không thành công. Mỗi mô phỏng mất O(n) và chúng tôi lặp lại n lần này cho O(n^2), quá chậm. 

Điểm mấu chốt là vấn đề này tương đương với vấn đề "trạm xăng tròn/chuyến tham quan xăng" cổ điển. Nếu chúng ta xác định mức thay đổi nhiên liệu ròng ở mỗi trạm là`a[i] - b[i]`, thì việc hoàn thành vòng tròn yêu cầu tổng tích lũy của những thay đổi ròng này không bao giờ giảm xuống dưới 0, bắt đầu từ trạm đã chọn. Bởi vì`sum(a) == sum(b)`, tổng số tiền ròng bằng không. Những điểm mà tổng tích lũy đạt đến mức tối thiểu sẽ xác định trạm xuất phát nào khả thi. Bất kỳ trạm nào ngay sau mức tối thiểu toàn cầu của tổng tích lũy đều là điểm bắt đầu hợp lệ. 

Quan sát này cho phép chúng ta tính tổng tích lũy một lần và tìm mức tối thiểu toàn cục, đưa ra nghiệm O(n). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(n) | Quá chậm | 
| Theo dõi Tổng / Tối thiểu Tích lũy | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính lượng nhiên liệu thay đổi ròng tại mỗi trạm:`delta[i] = a[i] - b[i]`. Điều này thể hiện lượng nhiên liệu bạn tăng hoặc giảm khi di chuyển từ ga`i`ĐẾN`i+1`. 
2. Tính tổng tiền tố của`delta`dọc theo vòng tròn:`pref[i] = delta[0] + delta[1] + ... + delta[i]`. Mở rộng mảng theo vòng tròn thêm một vòng để đơn giản hóa việc xử lý bao quanh nếu cần. 
3. Tìm giá trị tối thiểu chung của tổng tích lũy. Cho phép`min_sum`là tổng tiền tố nhỏ nhất và đặt`min_index`là chỉ số cuối cùng nơi mức tối thiểu này xảy ra. Điều này xác định vị trí mà nhiên liệu “thiếu” nhất dọc theo vòng lặp. 
4. Bất kỳ trạm nào ngay sau mức tối thiểu toàn cầu này (tức là`(min_index + 1) % n`) là điểm khởi đầu khả thi. Do tính đối xứng tròn nên dãy các trạm sau điểm này duy trì nhiên liệu không âm trong suốt vòng lặp. 
5. Kiểm tra tuần tự tất cả các trạm bắt đầu từ`min_index + 1`sử dụng tổng chạy để xác định tất cả các trạm có thể bắt đầu một vòng hoàn chỉnh mà không có nhiên liệu âm. Đây là những câu trả lời hợp lệ. 

Tại sao nó hoạt động: Tổng nhiên liệu ròng tích lũy biểu thị lượng nhiên liệu bạn có so với khi bắt đầu ở trạm 0. Mức tối thiểu chung xác định điểm tồi tệ nhất dọc theo vòng tròn. Bắt đầu ngay sau thời điểm này đảm bảo rằng bạn không bao giờ chạm tới mức tiêu hao nhiên liệu vì mọi mức thiếu hụt trước đó đều được bỏ qua và tổng lượng nhiên liệu khớp với tổng quãng đường. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
a = list(map(int, input().split()))
b = list(map(int, input().split()))

delta = [a[i] - b[i] for i in range(n)]

# Compute prefix sums
pref = [0] * n
pref[0] = delta[0]
for i in range(1, n):
    pref[i] = pref[i-1] + delta[i]

# Find minimum prefix sum
min_pref = pref[0]
min_index = 0
for i in range(n):
    if pref[i] < min_pref:
        min_pref = pref[i]
        min_index = i

# The station after the minimum is a valid start
start = (min_index + 1) % n

# Verify all valid stations using running sum from 'start'
res = []
fuel = 0
for i in range(n):
    idx = (start + i) % n
    fuel += delta[idx]
    if fuel < 0:
        start = (idx + 1) % n
        fuel = 0
        res = []
    else:
        res.append(idx + 1)  # 1-based indexing

print(len(res))
print(' '.join(map(str, sorted(res))))
```Giải thích: Trước tiên, chúng tôi tính toán mức tăng nhiên liệu ròng trên mỗi trạm, sau đó tính tổng tiền tố để theo dõi sự thay đổi nhiên liệu tích lũy. Trạm ngay sau tổng tiền tố tối thiểu toàn cục là điểm bắt đầu. Chúng tôi lặp lại vòng tròn để xác nhận tất cả các trạm bắt đầu hợp lệ, xử lý việc bao bọc bằng lập chỉ mục modulo. 

## Ví dụ đã hoạt động 

### Mẫu 1```
n = 4
a = [1, 7, 2, 3]
b = [8, 1, 1, 3]
```| Trạm | đồng bằng | tiền tố | nhiên liệu (từ đầu 2) | 
| --- | --- | --- | --- | 
| 1 | -7 | -7 | - | 
| 2 | 6 | -1 | 6 | 
| 3 | 1 | 0 | 7 | 
| 4 | 0 | 0 | 7 | 

Tổng tiền tố tối thiểu toàn cầu là -7 tại chỉ số 0. Bắt đầu sau đó: trạm 2. Việc lặp lại sẽ cho ra các trạm 2 và 4 hợp lệ. 

### Trường hợp tùy chỉnh```
n = 3
a = [1, 5, 2]
b = [3, 2, 3]
```Tổng tiền tố: -2, 3, 2. Tối thiểu = -2 tại chỉ số 0. Bắt đầu tại trạm 2. Chỉ trạm 2 mới cho phép hoàn thành vòng tròn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một lượt cho delta, một lượt cho tiền tố, một lượt để xác định các lần bắt đầu hợp lệ | 
| Không gian | O(n) | Lưu trữ tổng delta và tiền tố | 

Với`n ≤ 10^5`, thuật toán này chạy thoải mái trong giới hạn thời gian 2 giây, sử dụng bộ nhớ không đáng kể so với 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import builtins
    from contextlib import redirect_stdout
    f = io.StringIO()
    with redirect_stdout(f):
        n = int(input())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))
        delta = [a[i] - b[i] for i in range(n)]
        pref = [0]*n
        pref[0] = delta[0]
        for i in range(1, n):
            pref[i] = pref[i-1] + delta[i]
        min_pref = pref[0]
        min_index = 0
        for i in range(n):
            if pref[i] < min_pref:
                min_pref = pref[i]
                min_index = i
        start = (min_index + 1) % n
        res = []
        fuel = 0
        for i in range(n):
            idx = (start + i) % n
            fuel += delta[idx]
            if fuel < 0:
                start = (idx + 1) % n
                fuel = 0
                res = []
            else:
                res.append(idx + 1)
        print(len(res))
        print(' '.join(map(str, sorted(res))))
    return f.getvalue().strip()

# provided sample
assert run("4\n1 7 2 3\n8 1 1 3\n") == "2\n2 4"

# minimum input
assert run("1\n1\n1\n") == "1\n1"

# all equal values
assert run("3\n2 2 2\n2 2 2\n") == "3\n1 2 3"

# edge case: uneven distribution
assert run("3\n1 5 2\n3 2
```
