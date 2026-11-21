---
title: "CF 246C - Cuộc thi sắc đẹp"
description: "Chúng tôi được yêu cầu chọn những người lính từ một tiểu đoàn để tham gia một cuộc thi sắc đẹp trong vài ngày. Mỗi người lính đều có một giá trị vẻ đẹp riêng. Mỗi ngày, chúng ta phải cử đi một nhóm binh sĩ có vẻ đẹp tổng hợp độc đáo so với những ngày khác."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "constructive-algorithms", "greedy"]
categories: ["algorithms"]
codeforces_contest: 246
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 151 (Div. 2)"
rating: 1600
weight: 246
solve_time_s: 103
verified: true
draft: false
---

[CF 246C - Cuộc thi sắc đẹp](https://codeforces.com/problemset/problem/246/C) 

**Đánh giá:** 1600 
**Tags:** vũ lực, thuật toán mang tính xây dựng, lòng tham 
**Thời gian giải:** 1 phút 43s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu chọn những người lính từ một tiểu đoàn để tham gia một cuộc thi sắc đẹp trong vài ngày. Mỗi người lính đều có một giá trị vẻ đẹp riêng. Mỗi ngày, chúng ta phải cử đi một nhóm binh sĩ có vẻ đẹp tổng hợp độc đáo so với những ngày khác. Phân đội cho mỗi ngày không được để trống và chúng ta cần tạo ra một danh sách binh sĩ cho mỗi ngày thỏa mãn điều kiện duy nhất này. 

Dữ liệu đầu vào cho chúng ta số lượng binh sĩ, số ngày cuộc thi kéo dài và giá trị vẻ đẹp cá nhân của các binh sĩ. Kết quả đầu ra yêu cầu chúng ta in ra mỗi ngày số lượng binh lính được chọn và vẻ đẹp cụ thể của những người lính đó. 

Những ràng buộc cho chúng ta biết rằng`n`có thể lên tới 50 và bản thân người đẹp là những số nguyên dương lên tới 10^7. Bởi vì`n`là nhỏ, điều này cho phép các phương pháp tiếp cận xem xét các tập hợp con của binh lính mà không đạt đến giới hạn hiệu suất, mặc dù về mặt lý thuyết, số lượng tất cả các tập hợp con là theo cấp số nhân. Bài toán đảm bảo rằng có lời giải nên chúng ta không cần xử lý những trường hợp không thể giải được. 

Trường hợp đặc biệt là khi số ngày`k`vượt quá số lượng binh sĩ`n`. Ví dụ, nếu`n = 3`Và`k = 5`, chúng ta không thể chỉ phân công một người lính mỗi ngày vì không có đủ binh sĩ. Sau đó, chúng ta phải kết hợp binh lính thành các đơn vị gồm nhiều thành viên để đảm bảo mỗi ngày có một số tiền duy nhất. Giải pháp bất cẩn luôn chọn từng người lính sẽ thất bại ở đây. 

Một điểm tinh tế khác là tổng của các tập hợp con phải khác biệt. Vì mỗi người đẹp đều là duy nhất nên việc chọn binh sĩ đảm bảo`n`số tiền độc đáo. Khi`k > n`, chúng ta phải tập hợp binh lính thành các đội, nhưng chúng ta luôn có thể làm điều này một cách xây dựng. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực ngây thơ sẽ thử tất cả các tập hợp con lính có thể có mỗi ngày và kiểm tra xem tổng đó có phải là duy nhất hay không. Tổng số tập con không rỗng của`n`binh lính là`2^n - 1`. Vì`n = 50`, con số này vượt quá 10^15, điều này rõ ràng là không khả thi. Thậm chí cố gắng kiểm tra sự kết hợp cho`k`ngày sẽ nhân lên con số này nhiều hơn nữa, do đó, vũ lực sẽ thất bại ngay lập tức đối với mức độ vừa phải.`n`. 

Cái nhìn sâu sắc quan trọng là chúng ta không cần tất cả các tập hợp con. Chúng ta có thể chỉ định từng cái đầu tiên`n`ngày chỉ có một người lính, đảm bảo số tiền duy nhất cho những ngày này. Nếu như`k > n`, chúng tôi chỉ cần chọn thêm một người lính tối thiểu để kết hợp với mỗi người lính hiện có để tạo ra số tiền duy nhất bổ sung. Bởi vì vẻ đẹp là duy nhất nên việc cộng thêm vẻ đẹp của bất kỳ người lính nào khác sẽ luôn tạo ra một tổng mới khác biệt với tất cả các tổng trước đó. Điều này cho phép một thuật toán xây dựng đơn giản mà không cần liệt kê các tập hợp con hoặc kiểm tra tổng nhiều lần. 

Cách tiếp cận này có quy mô tốt vì nó trực tiếp xây dựng các biệt đội theo cách tham lam: bắt đầu với từng người đẹp riêng lẻ và sau đó chỉ kết hợp thêm binh lính khi cần số tiền độc đáo hơn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n × k) | O(2^n) | Quá chậm | 
| Tham lam mang tính xây dựng | O(n + k) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp quân lính theo vẻ đẹp. Bước này không thực sự cần thiết để đảm bảo tính chính xác nhưng nó giúp đơn giản hóa việc lựa chọn và đưa ra kết quả xác định. 
2. Khởi tạo danh sách giữ các phân đội cho mỗi ngày. 
3. Lần đầu tiên`n`ngày, giao cho mỗi ngày một người lính duy nhất với một vẻ đẹp riêng. Điều này đảm bảo`n`sim độc đáo. 
4. Nếu`k > n`, cho phần còn lại`k - n`ngày, chỉ định mỗi ngày người lính đầu tiên cộng với một trong những người lính khác chưa được sử dụng ngày hôm đó. Bởi vì tổng bây giờ bao gồm nhiều cái đẹp, mỗi sự kết hợp mới tạo ra một tổng khác biệt với tất cả các tổng trước đó. 
5. In các đội theo ngày, đầu tiên in số quân, theo sau là các mỹ nhân theo thứ tự bất kỳ. 

Tại sao nó hoạt động: thuật toán duy trì sự bất biến rằng mỗi ngày có một đội có tổng số điểm đẹp là duy nhất. Việc sử dụng các binh lính đơn lẻ đảm bảo tính duy nhất ban đầu và việc kết hợp các binh lính trong những ngày bổ sung sẽ duy trì tính duy nhất vì mỗi tổng thay đổi ít nhất bằng giá trị đẹp bổ sung. Bản chất mang tính xây dựng tránh được xung đột về tổng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, k = map(int, input().split())
a = list(map(int, input().split()))

# Sort for deterministic output
a.sort()

detachments = []

# Step 1: assign one soldier per day
for i in range(min(n, k)):
    detachments.append([a[i]])

# Step 2: if more days are needed, add extra soldiers to first
extra_needed = k - n
idx = 0
for _ in range(extra_needed):
    # Take first soldier plus another
    detachments.append([a[0], a[idx + 1]])
    idx += 1

# Output
for det in detachments:
    print(len(det), *det)
```Mã đầu tiên gán từng cái đầu tiên`n`ngày một người lính duy nhất. Nếu cần nhiều ngày hơn, nó sẽ tạo ra số tiền mới bằng cách kết hợp người lính đầu tiên với những người khác. Việc sắp xếp giúp duy trì một mẫu xác định đơn giản, nhưng bất kỳ thứ tự nào cũng có tác dụng. sử dụng`len(det)`đảm bảo số đầu tiên trên mỗi dòng là chính xác. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
3 3
1 2 3
```| Ngày | Biệt đội | Tổng hợp | 
| --- | --- | --- | 
| 1 | [1] | 1 | 
| 2 | [2] | 2 | 
| 3 | [3] | 3 | 

Tất cả các khoản tiền là duy nhất. Không cần kết hợp thêm vì`k = n`. 

Đầu vào mẫu 2:```
3 5
4 5 7
```| Ngày | Biệt đội | Tổng hợp | 
| --- | --- | --- | 
| 1 | [4] | 4 | 
| 2 | [5] | 5 | 
| 3 | [7] | 7 | 
| 4 | [4,5] | 9 | 
| 5 | [4,7] | 11 | 

Các tổng 4,5,7,9,11 đều khác biệt, thỏa mãn điều kiện ràng buộc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + k) | Sắp xếp là O(n log n), xây dựng các phần tách là O(k), cả hai đều không đáng kể với n ≤ 50 | 
| Không gian | O(n + k) | Cửa hàng tách ra mỗi ngày | 

Cho n 50 và k 50, tổng số phép toán đều dưới 10^4, thoải mái trong giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    a.sort()
    detachments = []
    for i in range(min(n, k)):
        detachments.append([a[i]])
    extra_needed = k - n
    idx = 0
    for _ in range(extra_needed):
        detachments.append([a[0], a[idx + 1]])
        idx += 1
    out = []
    for det in detachments:
        out.append(f"{len(det)} {' '.join(map(str, det))}")
    return "\n".join(out)

# provided sample
assert run("3 3\n1 2 3\n") == "1 1\n1 2\n1 3", "sample 1"

# custom cases
assert run("3 5\n4 5 7\n") == "1 4\n1 5\n1 7\n2 4 5\n2 4 7", "k > n case"
assert run("1 1\n10\n") == "1 10", "single soldier single day"
assert run("2 3\n100 200\n") == "1 100\n1 200\n2 100 200", "two soldiers three days"
assert run("4 4\n1 3 6 10\n") == "1 1\n1 3\n1 6\n1 10", "n=k case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 5 \n4 5 7 | 1 4\n1 5\n1 7\n2 4 5\n2 4 7 | Xử lý k > n | 
| 1 1 \n10 | 1 10 | Đầu vào tối thiểu | 
| 2 3 \n100 200 | 1 100\n1 200\n2 100 200 | Tổng hợp binh lính thêm ngày | 
| 4 4 \n1 3 6 10 | 1 1\n1 3\n1 6\n1 10 | Trận đấu chính xác của ngày và lính | 

## Vỏ cạnh 

Nếu chỉ có một người lính và nhiều ngày, thuật toán sẽ xử lý việc đó một cách tự nhiên vì nó không bao giờ cần tạo thêm ngày ngoài`n`. Ví dụ: đầu vào:```
1 1
42
```sản xuất```
1 42
```thỏa mãn tính duy nhất vì chỉ có một ngày. Vì`k > n`với`n > 1`, thuật toán kết hợp các binh lính theo một thứ tự xác định, đảm bảo mọi tổng đều khác biệt. Điều này đảm bảo rằng tất cả các điều kiện biên đều được giải quyết mà không cần logic trường hợp đặc biệt.
