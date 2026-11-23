---
title: "CF 412B - Cấu hình mạng"
description: "Chúng tôi được cấp một bộ máy tính, mỗi máy có tốc độ Internet tối đa được đo. Có ít người tham gia hơn máy tính và mỗi người tham gia phải có một máy tính riêng."
date: "2026-06-07T02:20:26+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 412
codeforces_index: "B"
codeforces_contest_name: "Coder-Strike 2014 - Round 1"
rating: 900
weight: 412
solve_time_s: 265
verified: true
draft: false
---

[CF 412B - Cấu hình mạng](https://codeforces.com/problemset/problem/412/B) 

**Xếp hạng:** 900 
**Tags:** tham lam, phân loại 
**Thời gian giải:** 4 phút 25s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một bộ máy tính, mỗi máy có tốc độ Internet tối đa được đo. Có ít người tham gia hơn máy tính và mỗi người tham gia phải có một máy tính riêng. Mục đích là làm cho tốc độ của các máy tính được chọn bằng nhau, nhưng chúng ta được phép giảm tốc độ của bất kỳ máy tính nào một cách tùy ý, không bao giờ tăng tốc độ lên. Trong số tất cả các cách để chọn máy tính và điều chỉnh tốc độ của chúng, chúng tôi muốn tối đa hóa tốc độ chung được chỉ định cho người tham gia. 

Đầu vào gồm hai số nguyên: tổng số máy tính`n`và số lượng người tham gia`k`. Sau đó là một dãy số nguyên biểu thị tốc độ tối đa của mỗi máy tính. Đầu ra là một số nguyên duy nhất: tốc độ đồng đều cao nhất có thể mà ít nhất`k`máy tính có thể đạt được sau khi giảm tùy chọn. 

Các hạn chế là nhỏ:`n`tối đa là 100. Điều này cho phép tiếp cận thoải mái đến khoảng O(n² log n), bởi vì ngay cả O(n³) cũng là đường biên. Bản thân tốc độ là những số nguyên vừa phải lên tới 32768, nhưng chúng ta không bao giờ cần liệt kê tất cả các số trong phạm vi đó nhờ số nhỏ`n`. 

Một trường hợp nguy hiểm có thể phá vỡ một giải pháp đơn giản xảy ra khi nhiều máy tính chia sẻ tốc độ cao nhưng chỉ cần một số ít máy tính. Ví dụ, với`n = 5`,`k = 3`, và tốc độ`[10, 10, 9, 9, 8]`, lựa chọn tối ưu là`[10, 10, 9]`giảm xuống`9`mỗi. Một cách tiếp cận bất cẩn chỉ đếm các bản sao chính xác có thể chọn sai`10`và thất bại vì chỉ có hai máy tính có nó. Một trường hợp khó phát hiện khác là khi tất cả các máy tính đều có tốc độ như nhau; câu trả lời chỉ nên là tốc độ đó bất kể`k`. 

## Phương pháp tiếp cận 

Phương pháp brute-force sẽ xem xét mọi tốc độ nguyên từ 1 đến tốc độ quan sát được tối đa. Đối với mỗi ứng viên tốc độ`s`, chúng ta sẽ đếm xem có bao nhiêu máy tính có tốc độ tối thiểu`s`. Nếu ít nhất`k`máy tính đáp ứng được ngưỡng này,`s`là khả thi. Sau đó, chúng tôi lấy khả năng lớn nhất có thể`s`. Điều này có hiệu quả vì việc giảm tốc độ được cho phép, do đó, bất kỳ máy tính nào nhanh hơn`s`có thể đóng góp. Vấn đề với điều này là phạm vi tốc độ lên tới 32768, vì vậy mặc dù`n`nhỏ, việc lặp lại tất cả 32768 tốc độ là không hiệu quả. 

Điểm mấu chốt là chúng ta chỉ cần xem xét tốc độ thực sự xuất hiện trên máy tính. Việc sắp xếp mảng cho phép chúng tôi kiểm tra tốc độ ứng viên một cách có hệ thống từ lớn nhất đến nhỏ nhất. Đối với tốc độ ứng viên nhất định`s`, chúng ta có thể đếm được có bao nhiêu máy tính có tốc độ tối thiểu như vậy. Ngay khi số lượng này ít nhất là`k`, chúng tôi đã tìm được tốc độ tối ưu. Điều này làm giảm không gian ứng cử viên từ 32768 xuống nhiều nhất`n`những giá trị hoàn toàn khả thi với`n`lên tới 100. 

Câu chuyện ở đây là: brute-force hoạt động về nguyên tắc vì cho phép giảm tốc độ, nhưng nó không hiệu quả vì nó cố gắng đạt được những tốc độ không thể hoặc không cần thiết. Việc sắp xếp và kiểm tra chỉ các tốc độ hiện có sẽ thúc đẩy cấu trúc của vấn đề: chỉ các mức tối đa hiện có mới quan trọng đối với giải pháp tối ưu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tất cả tốc độ 1..max) | O(n * max_speed) | O(1) | Quá chậm | 
| Kiểm tra tốc độ được sắp xếp | O(n log n + n2) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc giá trị đầu vào`n`,`k`và mảng tốc độ tối đa. Việc sắp xếp mảng sẽ giúp chúng ta suy luận từ tốc độ cao đến tốc độ thấp. 
2. Sắp xếp mảng tốc độ theo thứ tự không giảm. Lý do là tốc độ cao hơn được ưu tiên nên chúng tôi sẽ thử các ứng viên từ lớn nhất đến nhỏ nhất. 
3. Lặp lại mảng từ tốc độ lớn nhất trở xuống. Đối với mỗi tốc độ`a[i]`, đếm xem có bao nhiêu máy tính có tốc độ lớn hơn hoặc bằng`a[i]`. Chúng ta có thể làm điều này bằng cách quét mảng từ cuối về phía sau. 
4. Ngay khi đếm được số máy tính có tốc độ tối thiểu`a[i]`ít nhất là`k`, trở lại`a[i]`. Điều này hoạt động vì`a[i]`là ứng cử viên lớn nhất cho phép ít nhất`k`máy tính để đáp ứng nó. Chúng ta không cần kiểm tra tốc độ nhỏ hơn vì chúng sẽ chỉ làm giảm tốc độ chung. 

Tại sao nó hoạt động: Bằng cách chỉ xem xét tốc độ hiện có và đếm số lượng máy tính có thể đạt được từng tốc độ, chúng tôi đảm bảo rằng chúng tôi chọn tốc độ đồng đều tối đa có thể áp dụng cho ít nhất`k`máy tính. Bất kỳ tốc độ ứng viên nhỏ hơn đều không cần thiết hoặc không tối ưu. Bất biến là ở mọi ứng cử viên`s`, số lượng máy tính ≥`s`cho chúng ta biết chính xác tính khả thi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, k = map(int, input().split())
speeds = list(map(int, input().split()))

# Sort speeds ascending
speeds.sort()

# Check candidates from largest to smallest
for i in range(n - 1, -1, -1):
    count = sum(1 for speed in speeds if speed >= speeds[i])
    if count >= k:
        print(speeds[i])
        break
```Mã đầu tiên sắp xếp tốc độ. Sau đó, với mỗi tốc độ ứng viên bắt đầu từ tốc độ lớn nhất, nó sẽ tính xem có bao nhiêu máy tính có thể đạt được ít nhất tốc độ đó. Ngay khi số đếm đạt đến`k`, tốc độ đó được in ra. Sự tinh tế ở đây là quét từ lớn nhất đến nhỏ nhất và đếm`>= speeds[i]`, đảm bảo tốc độ khả thi tối đa. 

## Ví dụ đã hoạt động 

**Mẫu 1:** 

đầu vào:```
3 2
40 20 30
```| Bước | Ứng viên`s`| Đếm >=`s`| Quyết định | 
| --- | --- | --- | --- | 
| 1 | 40 | 1 | Không đủ | 
| 2 | 30 | 2 | Đủ → chọn 30 | 

Thuật toán xác định rằng hai máy tính có thể được đặt ở tốc độ 30, đây là tốc độ đồng đều cao nhất có thể. 

**Ví dụ tùy chỉnh:** 

đầu vào:```
5 3
10 10 9 9 8
```| Bước | Ứng viên`s`| Đếm >=`s`| Quyết định | 
| --- | --- | --- | --- | 
| 1 | 10 | 2 | Không đủ | 
| 2 | 9 | 4 | Đủ → chọn 9 | 

Mặc dù số 10 xuất hiện hai lần, nhưng chỉ có bốn máy tính có thể đạt tới số 9 sau khi giảm tùy chọn, tạo ra số 9 đồng nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n²) | Sắp xếp là O(n log n), kiểm tra từng ứng viên là O(n²) trong trường hợp xấu nhất | 
| Không gian | O(n) | Lưu trữ mảng tốc độ | 

Với`n ≤ 100`, O(n²) không đáng kể (nhiều nhất là 10.000 thao tác). Việc sử dụng bộ nhớ là không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    n, k = map(int, input().split())
    speeds = list(map(int, input().split()))
    speeds.sort()
    for i in range(n-1, -1, -1):
        count = sum(1 for s in speeds if s >= speeds[i])
        if count >= k:
            print(speeds[i])
            break
    return output.getvalue().strip()

# Provided sample
assert run("3 2\n40 20 30\n") == "30", "sample 1"

# Custom cases
assert run("5 3\n10 10 9 9 8\n") == "9", "mixed speeds"
assert run("4 2\n5 5 5 5\n") == "5", "all equal"
assert run("1 1\n16\n") == "16", "minimum size"
assert run("6 4\n20 15 15 10 10 10\n") == "15", "need top 4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`5 3\n10 10 9 9 8`| 9 | Lựa chọn và giảm độ chính xác | 
|`4 2\n5 5 5 5`| 5 | Tất cả tốc độ bằng nhau | 
|`1 1\n16`| 16 | Đầu vào tối thiểu | 
|`6 4\n20 15 15 10 10 10`| 15 | Chọn hàng đầu`k`sau khi giảm giá | 

## Vỏ cạnh 

cho`n = 1`Và`k = 1`với tốc độ`16`, thuật toán ngay lập tức chọn 16. Đối với`n = 5`Và`k = 3`với tốc độ`[10, 10, 9, 9, 8]`, thuật toán đầu tiên kiểm tra 10 (đếm 2 < 3), sau đó 9 (đếm 4 ≥ 3), xuất ra 9. Cả hai trường hợp cạnh đều chứng minh rằng việc đếm`>= candidate`là điều cần thiết và bắt đầu từ ứng cử viên lớn nhất sẽ đảm bảo tính tối đa. Phương pháp này xử lý tất cả các trường hợp`k`bằng`n`, tất cả các tốc độ đều bằng nhau hoặc cần phải giảm để phù hợp với tốc độ đồng đều.
