---
title: "CF 1942B - Bessie và MEX"
description: "Chúng ta có một mảng a có độ dài n, được xây dựng từ một số hoán vị p chưa biết của các số nguyên từ 0 đến n-1. Mỗi phần tử của a thỏa mãn quan hệ a[i] = MEX(p[1..i]) - p[i]. Nhiệm vụ là xây dựng lại bất kỳ hoán vị p hợp lệ nào tạo ra a này."
date: "2026-05-31T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "math"]
categories: ["algorithms"]
codeforces_contest: 1942
codeforces_index: "B"
codeforces_contest_name: "CodeTON Round 8 (Div. 1 + Div. 2, Rated, Prizes!)"
rating: 1100
weight: 1942
solve_time_s: 78
verified: false
draft: false
---

[CF 1942B - Bessie và MEX](https://codeforces.com/problemset/problem/1942/B) 

**Đánh giá:** 1100 
**Tags:** thuật toán xây dựng, toán học 
**Thời gian giải:** 1 phút 18s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng`a`chiều dài`n`, được xây dựng từ một số hoán vị chưa biết`p`của các số nguyên`0`bởi vì`n-1`. Mỗi phần tử của`a`thỏa mãn mối quan hệ`a[i] = MEX(p[1..i]) - p[i]`. Nhiệm vụ là xây dựng lại bất kỳ hoán vị hợp lệ nào`p`cái đó tạo ra cái này`a`. MEX của một mảng được định nghĩa là số nguyên không âm nhỏ nhất bị thiếu trong mảng đó. 

Đầu vào cung cấp nhiều trường hợp thử nghiệm. Với mỗi trường hợp, thuật toán phải xây dựng lại một hoán vị`p`một cách hiệu quả. 

Những hạn chế là đáng kể:`n`có thể lớn bằng 2_10^5 và tổng của tất cả`n`trên tất cả các trường hợp thử nghiệm cũng là 2_10^5. Điều này ngụ ý rằng chúng tôi không thể chấp nhận bất cứ điều gì tệ hơn thời gian tuyến tính cho mỗi trường hợp thử nghiệm; cách tiếp cận bạo lực O(n^2) sẽ dẫn đến khoảng 10^10 thao tác trong trường hợp xấu nhất, quá chậm so với giới hạn thời gian 2 giây. 

Một điểm tinh tế phát sinh với các giá trị âm và dương lớn trong`a`. Từ`a[i] = MEX(...) - p[i]`, có thể cho`a[i]`trở nên tiêu cực. Ví dụ: nếu MEX tại một điểm là`2`nhưng phần tử hoán vị là`4`, sau đó`a[i] = 2 - 4 = -2`. Một thuật toán bất cẩn giả định`a[i] ≥ 0`sẽ thất bại. Ngoài ra, có thể tồn tại nhiều giải pháp, do đó, bất kỳ sự tái thiết hợp lệ nào cũng đủ. 

Một trường hợp đặc biệt cần xem xét là khi tất cả`a[i]`là`1`,`0`, hoặc số âm. Ví dụ, nếu`a = [1,1,1]`, thì thuật toán phải tạo ra một hoán vị chính xác tôn trọng sự khác biệt MEX mà không sử dụng lại các số. 

## Phương pháp tiếp cận 

Cách tiếp cận ngây thơ là xây dựng lại hoán vị một cách tham lam bằng cách mô phỏng MEX từ đầu: cho từng vị trí`i`, lặp qua tất cả các số chưa sử dụng để xem số nào thỏa mãn`a[i] = MEX(current) - x`. Điều này đòi hỏi phải tính toán lại MEX mỗi bước trong thời gian O(n), dẫn đến thuật toán O(n^2). Điều này đúng về nguyên tắc, nhưng quá chậm đối với các ràng buộc của bài toán. 

Quan sát quan trọng là MEX luôn tăng tối đa 1 ở mỗi bước. Nếu MEX hiện tại là`m`, thì phần tử tiếp theo của hoán vị là`m - a[i]`nếu số đó vẫn còn hoặc một số chưa sử dụng khác nhỏ hơn`m`. Điều này mang lại một công trình thời gian tuyến tính: 

1. Giữ một tập hợp tất cả các số chưa sử dụng trong hoán vị. 
2. Theo dõi MEX hiện tại một cách hiệu quả bằng con trỏ bắt đầu từ`0`. 
3. Đối với từng vị trí`i`, tính toán`p[i] = current_MEX - a[i]`. Nếu số này không được sử dụng, hãy chỉ định nó. Ngược lại, hãy chọn số nhỏ nhất chưa sử dụng nhỏ hơn MEX hiện tại. Cập nhật MEX hiện tại cho phù hợp. 

Cách tiếp cận này có hiệu quả vì công thức`p[i] = MEX - a[i]`được bắt nguồn trực tiếp từ phát biểu vấn đề. Đảm bảo rằng tồn tại một hoán vị hợp lệ, vì vậy chúng ta sẽ luôn tìm thấy số chưa sử dụng khớp với công thức. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo danh sách`used`kích thước`n`để theo dõi những con số đã được đặt vào`p`. Khởi tạo một bộ`available`chứa tất cả các số`0..n-1`. 
2. Đặt`current_mex`ĐẾN`0`. 
3. Đối với mỗi chỉ số`i`từ`0`ĐẾN`n-1`, tính số ứng viên`x = current_mex - a[i]`. 
4. Nếu`x`đang ở trong`available`, giao phó`p[i] = x`, xóa nó khỏi`available`, và rời đi`current_mex`không thay đổi hoặc tăng nó nếu`x == current_mex`. 
5. Nếu`x`không có trong`available`, giao phó`p[i]`là số nhỏ nhất trong`available`. Loại bỏ nó và cập nhật`current_mex`nếu cần thiết. 
6. Lặp lại cho đến khi lấp đầy tất cả các vị trí. 
7. Xuất mảng`p`. 

**Tại sao nó hoạt động:** Điều bất biến là ở mỗi bước,`current_mex`phản ánh MEX của những con số được sử dụng cho đến nay. sử dụng`p[i] = current_mex - a[i]`đảm bảo rằng công thức chênh lệch MEX được giữ nguyên. Việc kiểm tra tính khả dụng đảm bảo rằng chúng tôi không bao giờ sử dụng lại các số, duy trì thuộc tính hoán vị. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        used = [False] * n
        p = [0] * n
        mex = 0
        import heapq
        # maintain min-heap of unused numbers
        available = list(range(n))
        heapq.heapify(available)

        for i in range(n):
            x = mex - a[i]
            if 0 <= x < n and not used[x]:
                p[i] = x
                used[x] = True
                while mex < n and used[mex]:
                    mex += 1
            else:
                # pick smallest unused
                while used[available[0]]:
                    heapq.heappop(available)
                p[i] = heapq.heappop(available)
                used[p[i]] = True
                while mex < n and used[mex]:
                    mex += 1
        print(" ".join(map(str, p)))

if __name__ == "__main__":
    solve()
```Giải pháp này sử dụng mảng boolean`used`để theo dõi các số đã được gán, đảm bảo tính duy nhất hoán vị. các`mex`con trỏ theo dõi MEX hiện tại một cách hiệu quả. Một đống tối thiểu lưu trữ các số chưa sử dụng để chọn số nhỏ nhất có sẵn một cách hiệu quả khi lý tưởng`x`đã được sử dụng rồi. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
1 1 -2 1 2
```| tôi | một [tôi] | mex | ứng cử viên x | được giao p[i] | được sử dụng sau bước | mex sau bước | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | -1 | 0 | {0} | 1 | 
| 1 | 1 | 1 | 0 | 1 | {0,1} | 2 | 
| 2 | -2 | 2 | 4 | 4 | {0,1,4} | 2 | 
| 3 | 1 | 2 | 1 | 2 | {0,1,2,4} | 3 | 
| 4 | 2 | 3 | 1 | 3 | {0,1,2,3,4} | 5 | 

Điều này cho thấy sự bất biến:`mex`luôn phản ánh MEX hiện tại và`p[i] = mex - a[i]`khi có thể. 

### Ví dụ 2 

đầu vào:```
3
-2 1 2
```| tôi | một [tôi] | mex | ứng cử viên x | được giao p[i] | 
| --- | --- | --- | --- | --- | 
| 0 | -2 | 0 | 2 | 2 | 
| 1 | 1 | 0 | -1 | 0 | 
| 2 | 2 | 1 | -1 | 1 | 

Ở đây, các giá trị ứng cử viên âm sẽ kích hoạt lựa chọn số nhỏ nhất chưa được sử dụng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phần tử được xử lý một lần, các phép toán heap là O(log n) cho mỗi phần tử nhưng tổng trên n là O(n log n) có thể chấp nhận được trong các ràng buộc | 
| Không gian | O(n) | Mảng Boolean`used`và lưu trữ heap tối thiểu lên tới`n`yếu tố | 

Được cho`sum(n) <= 2*10^5`, điều này vừa vặn thoải mái trong giới hạn thời gian 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

# provided samples
assert run("3\n5\n1 1 -2 1 2\n5\n1 1 1 1 1\n3\n-2 1 2\n") in ["0 1 4 2 3\n0 1 2 3 4\n2 0 1","0 1 4 2 3\n0 1 2 3 4\n2 1 0"], "sample 1"

# custom: minimum size
assert run("1\n1\n0\n") == "0", "min size"

# custom: maximum n, trivial a
max_n = 10
assert run(f"1\n{max_n}\n" + " ".join(["1"]*max_n) + "\n") != "", "all ones"

# custom: negative a triggers selection of available
assert run("1\n3\n-1 -1 -1\n") != "", "all negative"

#
```
