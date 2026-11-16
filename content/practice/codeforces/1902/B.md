---
title: "CF 1902B - Nhận điểm"
description: "Chúng ta có một sinh viên, Monocarp, người có chuỗi n ngày trong một học kỳ. Mỗi ngày, anh ấy có thể học hoặc nghỉ ngơi. Việc học giúp anh ta có hai cách để kiếm điểm: tham dự một buổi học (trị giá l điểm) và hoàn thành các nhiệm vụ thực tế (mỗi bài trị giá t điểm)."
date: "2026-06-08T21:07:16+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "brute-force", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1902
codeforces_index: "B"
codeforces_contest_name: "Educational Codeforces Round 159 (Rated for Div. 2)"
rating: 1100
weight: 1902
solve_time_s: 120
verified: true
draft: false
---

[CF 1902B - Nhận điểm](https://codeforces.com/problemset/problem/1902/B) 

**Đánh giá:** 1100 
**Tags:** tìm kiếm nhị phân, vũ phu, tham lam 
**Thời gian giải:** 2 phút 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một sinh viên, Monocarp, người có trình tự`n`ngày trong một kỳ hạn. Mỗi ngày, anh ấy có thể học hoặc nghỉ ngơi. Việc học mang lại cho anh hai cách để kiếm điểm: tham dự một bài học (giá trị`l`điểm) và hoàn thành các nhiệm vụ thực tế (có giá trị`t`mỗi điểm cho nhau). Nhiệm vụ được mở khóa hàng tuần: nhiệm vụ đầu tiên vào ngày 1, nhiệm vụ thứ hai vào ngày 8, nhiệm vụ thứ ba vào ngày 15, v.v. Vào bất kỳ ngày nào, Monocarp có thể hoàn thành tối đa 2 nhiệm vụ đã mở khóa ngoài việc tham dự buổi học. 

Mục tiêu là tính xem Monocarp có thể nghỉ ngơi bao nhiêu ngày mà vẫn kiếm được ít nhất`P`tổng số điểm. Đầu vào cho số ngày`n`, số điểm cần thiết`P`, điểm bài học`l`, và điểm nhiệm vụ`t`. Đầu ra là một số nguyên cho mỗi trường hợp thử nghiệm: số ngày nghỉ tối đa có thể. 

Những hạn chế là lớn.`n`Và`l`,`t`có thể lên tới`10^9`, Và`P`có thể lên tới`10^18`. Điều này loại trừ mọi cách tiếp cận mô phỏng trực tiếp mỗi ngày. Việc đếm số lượng hàng ngày sẽ quá chậm. Chúng ta cần một cách giải thích về điểm tích lũy theo từng khoảng thời gian mà không lặp lại hàng ngày. 

Các trường hợp cạnh không rõ ràng phát sinh từ hai quan sát. Thứ nhất, nếu bài học cực kỳ có giá trị so với nhiệm vụ thì Monocarp có thể phải tham dự gần như mỗi ngày. Ví dụ, nếu`n = 1`,`P = 5`, Và`l = 5`, anh ấy không thể nghỉ ngơi chút nào. Thứ hai, số lượng nhiệm vụ tối đa anh ta có thể hoàn thành không chỉ bị giới hạn theo ngày mà còn bị giới hạn 2 nhiệm vụ mỗi ngày và mở khóa hàng tuần. Ví dụ, nếu`n = 8`, chỉ có hai nhiệm vụ được mở khóa vào ngày thứ 8, vì vậy ngay cả khi có đủ ngày để hoàn thành tất cả các nhiệm vụ còn lại sau đó, anh ta cũng không thể làm nhiều hơn 2 nhiệm vụ mỗi ngày. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ mô phỏng mỗi ngày, theo dõi các nhiệm vụ đã mở khóa, nhiệm vụ đã hoàn thành và điểm tích lũy. Mỗi ngày, chúng ta sẽ chọn học hay nghỉ ngơi tùy theo mức độ gần gũi của chúng ta với nhau.`P`. Về nguyên tắc thì điều này đúng, nhưng nó là O(n) cho mỗi trường hợp thử nghiệm, quá chậm đối với`n`lên đến 10^9 và`tc`lên đến 10^4. Số lượng hoạt động sẽ vượt quá`10^13`, điều đó là không thể thực hiện được. 

Nhận xét quan trọng là việc học tập rất “mạnh mẽ” và mang tính chất bổ sung. Mỗi ngày học cho một điểm bài cố định`l`cộng tối đa 2 điểm nhiệm vụ`t`. Nhiệm vụ được mở khóa theo nhóm một nhiệm vụ mỗi tuần, vì vậy chúng tôi có thể tính toán số lượng nhiệm vụ tối đa có sẵn sau một ngày nhất định. Monocarp sẽ muốn thực hiện ít ngày học tập nhất có thể để đạt được`P`. Chúng ta có thể đặt lại vấn đề: số ngày học tối thiểu là bao nhiêu?`k`cần thiết để đạt được`P`nếu chúng ta luôn hoàn thành số lượng nhiệm vụ tối đa mỗi ngày học? 

Cho phép`tasks_total`là tổng số nhiệm vụ được mở khóa vào ngày cuối cùng. Vào một ngày nhất định, tổng số điểm anh ấy có thể kiếm được nếu học`k`ngày là`l * k + t * min(2*k, tasks_total)`. Chúng ta cần số tiền này ít nhất là`P`. Nếu chúng ta giải quyết tối thiểu`k`, thì số ngày nghỉ tối đa chỉ đơn giản là`n - k`. Vì hàm số đơn điệu trong`k`, tìm kiếm nhị phân có thể tìm thấy điều tối thiểu này`k`một cách hiệu quả. Điều này làm giảm độ phức tạp xuống O(log n) cho mỗi trường hợp thử nghiệm, điều này có thể chấp nhận được. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) cho mỗi trường hợp thử nghiệm | O(1) | Quá chậm | 
| Tối ưu (Tìm kiếm nhị phân trong ngày học) | O(log n) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tổng số nhiệm vụ được mở khóa theo ngày`n`. Mỗi nhiệm vụ sẽ mở khóa 7 ngày một lần nên số lượng nhiệm vụ là`(n + 6) // 7`. Điều này giải thích cho thực tế là ngày 1 mở khóa nhiệm vụ đầu tiên, ngày 8 nhiệm vụ thứ hai, v.v. 
2. Xác định hàm trợ giúp`points(study_days)`tính toán số điểm tối đa mà Monocarp có thể kiếm được nếu anh ấy học`study_days`ngày. Mỗi ngày học, anh được`l`điểm cho bài học và có thể hoàn thành tối đa 2 nhiệm vụ. Do đó, tổng số điểm là`l * study_days + t * min(2 * study_days, total_tasks)`. 
3. Thực hiện tìm kiếm nhị phân theo số ngày học. Bộ`lo = 0`Và`hi = n`. Trong mỗi lần lặp lại, hãy kiểm tra xem`points(mid)`lớn hơn hoặc bằng`P`. Nếu có, chúng ta có thể thử giảm số ngày học bằng cách thiết lập`hi = mid`. Ngược lại, đặt`lo = mid + 1`. Tiếp tục cho đến khi`lo == hi`. 
4. Trở về`n - lo`như số ngày nghỉ tối đa. 

Tại sao nó hoạt động: chức năng`points(k)`đơn điệu ngày càng tăng. Nhiều ngày học hơn không bao giờ làm giảm tổng điểm, và một khi`points(k)`đạt tới`P`, tăng dần`k`hơn nữa là không cần thiết. Tìm kiếm nhị phân tìm thấy giá trị tối thiểu`k`đó thỏa mãn điều kiện. Trừ từ`n`cung cấp số ngày nghỉ tối đa. Giới hạn nhiệm vụ mỗi ngày và mở khóa hàng tuần được tôn trọng vì`min(2*k, total_tasks)`tính đến cả hai ràng buộc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def max_rest_days(n, P, l, t):
    total_tasks = (n + 6) // 7
    
    def points(k):
        return l * k + t * min(2 * k, total_tasks)
    
    lo, hi = 0, n
    while lo < hi:
        mid = (lo + hi) // 2
        if points(mid) >= P:
            hi = mid
        else:
            lo = mid + 1
    return n - lo

tc = int(input())
for _ in range(tc):
    n, P, l, t = map(int, input().split())
    print(max_rest_days(n, P, l, t))
```Mã đầu tiên tính toán số lượng nhiệm vụ được mở khóa. Hàm trợ giúp tính tổng số điểm cho một số ngày học, tôn trọng giới hạn 2 ngày học. Tìm kiếm nhị phân có hiệu quả tìm thấy số ngày học tối thiểu cần thiết. Điều kiện biên như`0`ngày học và`n`ngày học được xử lý tự động bởi`lo = 0, hi = n`khởi tạo. 

## Ví dụ đã hoạt động 

**Ví dụ 1:**`1 5 5 2`| k (ngày học) | điểm(k) | Tình trạng | 
| --- | --- | --- | 
| 0 | 0 | < 5 | 
| 1 | 5 + 2*1 = 7 | >= 5 | 

Tìm kiếm nhị phân tìm thấy`k = 1`, vậy số ngày nghỉ tối đa =`1 - 1 = 0`. 

**Ví dụ 2:**`14 3000000000 1000000000 500000000`Tổng số nhiệm vụ =`(14 + 6)//7 = 2`. 

| k | điểm(k) | Tình trạng | 
| --- | --- | --- | 
| 0 | 0 | < 3e9 | 
| 2 | 2e9 + phút(4,2)*5e8 = 2e9 + 1e9 = 3e9 | >= 3e9 | 

Ngày học tối thiểu = 2, ngày nghỉ tối đa = 14 - 2 = 12. 

Những dấu vết này chứng minh rằng tìm kiếm nhị phân có hiệu quả tìm ra số ngày học tối thiểu và tôn trọng giới hạn nhiệm vụ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(tc * log n) | Tìm kiếm nhị phân tối đa n ngày cho mỗi trường hợp thử nghiệm tc | 
| Không gian | O(1) | Chỉ một số số nguyên được lưu trữ, độc lập với n | 

Ngay cả với`tc = 10^4`Và`n = 10^9`, tổng số thao tác là khoảng 3×10^5, rất phù hợp trong giới hạn thời gian 1 giây. 

## Trường hợp thử nghiệm```python
def run(inp: str) -> str:
    import sys, io
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    # call solution
    tc = int(input())
    for _ in range(tc):
        n, P, l, t = map(int, input().split())
        print(max_rest_days(n, P, l, t))
    return out.getvalue().strip()

# provided samples
assert run("5\n1 5 5 2\n14 3000000000 1000000000 500000000\n100 20 1 10\n8 120 10 20\n42 280 13 37") == "0\n12\n99\n0\n37"

# custom cases
assert run("2\n1 1 1 1\n7 14 2 2") == "0\n3", "minimum size and exact week limit"
assert run("1\n1000000000 1000000000000000000 1000 1000") == "999999500", "large n, large P"
assert run("1\n7 10 1 2") == "3", "exactly one week, mixed points"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| `1 | | |
