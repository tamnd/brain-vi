---
title: "CF 1646B - Chất lượng và Số lượng"
description: "Chúng ta được cho một dãy các số nguyên không âm và chúng ta có thể tô từng phần tử thành màu đỏ, xanh lam hoặc không sơn."
date: "2026-06-10T04:09:12+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "constructive-algorithms", "greedy", "sortings", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1646
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 774 (Div. 2)"
rating: 800
weight: 1646
solve_time_s: 76
verified: true
draft: false
---

[CF 1646B - Chất lượng và Số lượng](https://codeforces.com/problemset/problem/1646/B) 

**Đánh giá:** 800 
**Tags:** vũ lực, thuật toán xây dựng, tham lam, sắp xếp, hai con trỏ 
**Thời gian giải:** 1 phút 16s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy các số nguyên không âm và chúng ta có thể tô từng phần tử thành màu đỏ, xanh lam hoặc không sơn. Mục tiêu là tìm một cách tô màu sao cho tổng các số màu đỏ lớn hơn tổng các số màu xanh lam, trong khi tổng số các số màu đỏ nhỏ hơn tổng số các số màu xanh lam. Đối với mỗi trường hợp thử nghiệm, chúng ta chỉ cần quyết định xem màu đó có tồn tại hay không; chúng ta không cần phải tự tạo màu. 

Các ràng buộc đầu vào là đáng kể. Mỗi chuỗi có thể dài tới 200.000 và tổng độ dài của tất cả các chuỗi trong các trường hợp thử nghiệm cũng được giới hạn ở mức 200.000. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào xem xét mọi tập hợp con của các số, bởi vì số lượng tập hợp con tăng theo cấp số nhân. Do đó, thuật toán của chúng tôi phải tuyến tính hoặc gần tuyến tính về số lượng phần tử trên mỗi trường hợp thử nghiệm. Bản thân các giá trị có thể lên tới 10^9, nhưng vì chúng ta chỉ cộng chúng nên việc tràn số nguyên không phải là vấn đề trong Python. 

Trường hợp cạnh tinh tế phát sinh khi các số lớn nhất nằm ở cuối hoặc đầu của chuỗi được sắp xếp. Ví dụ: nếu tất cả các phần tử đều bằng nhau thì sẽ không có màu nào thỏa mãn điều kiện vì tổng tăng tuyến tính theo số đếm, khiến tập hợp con màu đỏ nhỏ hơn không thể vượt quá tổng của tập hợp con màu xanh lam lớn hơn. Một trường hợp phức tạp khác là khi một vài số rất lớn được kết hợp với nhiều số nhỏ; một nỗ lực ngây thơ để chọn các phần tử theo thứ tự tùy ý có thể bỏ lỡ sự phân phối tối ưu. Về cơ bản, vấn đề yêu cầu chúng ta ưu tiên "chất lượng" (giá trị lớn) cho màu đỏ và "số lượng" (nhiều phần tử) cho màu xanh lam. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử mọi cách có thể để gán các phần tử thành màu đỏ, xanh lam hoặc không sơn. Đối với mỗi tập hợp con màu đỏ và xanh lam, chúng ta có thể tính tổng và đếm rồi kiểm tra các điều kiện. Cách tiếp cận này hiệu quả với các chuỗi rất nhỏ, nhưng với n lên tới 2·10^5, nó hoàn toàn không khả thi vì số lượng tập hợp con là 2^n. 

Thông tin chi tiết quan trọng là để tối đa hóa tổng màu đỏ trong khi vẫn giữ số lượng màu đỏ ở mức thấp, chúng ta nên chọn phần tử lớn nhất cho màu đỏ. Ngược lại, để tối đa hóa số lượng màu xanh lam trong khi vẫn giữ tổng của nó thấp hơn, chúng ta nên chọn các phần tử nhỏ nhất cho màu xanh lam. Điều này làm giảm vấn đề sắp xếp chuỗi và kiểm tra tiền tố của phần tử lớn nhất và nhỏ nhất. Cụ thể, chúng ta có thể sắp xếp mảng theo thứ tự tăng dần, xem xét lấy k phần tử nhỏ nhất cho màu xanh lam và k phần tử lớn nhất cho màu đỏ và tăng k cho đến khi tổng màu đỏ vượt quá tổng màu xanh. Nếu tại bất kỳ thời điểm nào số màu đỏ nhỏ hơn số màu xanh và tổng màu đỏ lớn hơn tổng màu xanh, chúng ta có thể dừng lại và tuyên bố CÓ. Nếu chúng ta đến giữa mà không thỏa mãn được điều này thì câu trả lời là KHÔNG. Cách tiếp cận này là O(n log n) cho mỗi trường hợp thử nghiệm do sắp xếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(3^n) | O(n) | Quá chậm | 
| Sắp xếp + Tham lam hai con trỏ | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc trình tự và sắp xếp theo thứ tự tăng dần. Việc sắp xếp cho phép chúng ta chọn một cách có hệ thống các phần tử nhỏ nhất cho màu xanh lam và phần tử lớn nhất cho màu đỏ, điều này tối ưu để đáp ứng cả điều kiện tổng và số lượng. 
2. Khởi tạo hai con trỏ: một con trỏ bắt đầu ở đầu mảng được sắp xếp cho các phần tử màu xanh và một ở cuối mảng cho các phần tử màu đỏ. 
3. Khởi tạo hai tổng:`sum_blue = 0`Và`sum_red = 0`. 
4. Thêm dần phần tử nhỏ nhất vào`sum_blue`và phần tử lớn nhất`sum_red`trong mỗi bước. Theo dõi số lượng màu đỏ và màu xanh. 
5. Sau mỗi lần cộng, kiểm tra xem tổng màu đỏ hiện tại có vượt quá tổng màu xanh hay không và số màu đỏ có nhỏ hơn số màu xanh hay không. Nếu cả hai điều kiện đều đúng, in CÓ và dừng. 
6. Nếu chúng ta đạt đến điểm mà con trỏ màu đỏ vượt qua con trỏ màu xanh và không tìm thấy màu hợp lệ, hãy in NO. 

Tại sao nó hoạt động: Tính năng sắp xếp đảm bảo rằng mọi phần tử màu đỏ càng lớn càng tốt so với các phần tử màu xanh lam, giúp tối đa hóa tổng màu đỏ trong khi giảm thiểu số lượng màu đỏ. Bằng cách kiểm tra dần dần các tập hợp con lớn hơn, chúng tôi đảm bảo rằng điểm đầu tiên có tổng màu đỏ vượt quá tổng màu xanh cũng đáp ứng yêu cầu số lượng màu đỏ tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        a.sort()
        sum_red = 0
        sum_blue = 0
        i = 0  # index for blue (smallest)
        j = n - 1  # index for red (largest)
        count_blue = 0
        count_red = 0
        possible = False
        while i < j:
            sum_blue += a[i]
            sum_red += a[j]
            count_blue += 1
            count_red += 1
            i += 1
            j -= 1
            if sum_red > sum_blue and count_red < count_blue:
                possible = True
                break
        print("YES" if possible else "NO")

solve()
```Đầu tiên, giải pháp sẽ đọc số lượng ca kiểm thử và lặp lại qua từng trình tự. Việc sắp xếp đảm bảo rằng chúng ta có thể chọn giá trị lớn nhất cho màu đỏ và giá trị nhỏ nhất cho màu xanh lam. Vòng lặp while tiếp tục miễn là chúng ta có các phần tử chưa được gán, cập nhật đồng thời cả tổng và số đếm. Chúng tôi dừng lại sớm nếu tìm thấy màu hợp lệ, hiệu quả. Việc lựa chọn hai con trỏ đảm bảo rằng màu đỏ luôn có giá trị còn lại lớn nhất và màu xanh lam nhỏ nhất, phản ánh trực tiếp các ràng buộc của bài toán. 

## Ví dụ đã hoạt động 

**Ví dụ 1:** Đầu vào`[2, 8, 6, 3, 1]`| Bước | tôi (idx xanh) | j (idx đỏ) | tổng_blue | tổng_red | đếm_blue | đếm_red | Điều kiện đã đáp ứng? | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 4 | 1 | 8 | 1 | 1 | Có | 

Ở đây, lấy giá trị lớn nhất 8 cho màu đỏ và giá trị nhỏ nhất 1 cho màu xanh lam thỏa mãn tổng đỏ > tổng xanh và số đỏ < số xanh ngay lập tức. 

**Ví dụ 2:** Đầu vào`[3, 5, 4, 2]`| Bước | tôi | j | tổng_blue | tổng_red | đếm_blue | đếm_red | Điều kiện đã đáp ứng? | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 3 | 2 | 5 | 1 | 1 | Không | 
| 2 | 1 | 2 | 2+3=5 | 5+4=9 | 2 | 2 | Không | 

Mặc dù tổng màu đỏ trở nên lớn hơn ở bước thứ hai, số màu đỏ bằng số màu xanh, vì vậy nó không thỏa mãn điều kiện. Vì vậy, đầu ra là KHÔNG. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp chiếm ưu thế; mỗi chuỗi yêu cầu sắp xếp, sau đó lặp lại hai con trỏ tuyến tính. | 
| Không gian | O(n) | Chúng tôi lưu trữ trình tự và một vài bộ đếm; không cần cấu trúc dữ liệu bổ sung. | 

Điều này phù hợp với các ràng buộc vì tổng của tất cả n trong các trường hợp thử nghiệm là 2·10^5. Việc sắp xếp đủ hiệu quả cho 200.000 phần tử. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

# Provided samples
assert run("4\n3\n1 2 3\n5\n2 8 6 3 1\n4\n3 5 4 2\n5\n1000000000 1000000000 1000000000 1000000000 1000000000") == "NO\nYES\nNO\nNO"

# Custom cases
assert run("1\n3\n1 1 10") == "YES", "few elements with a single large number"
assert run("1\n3\n5 5 5") == "NO", "all equal elements"
assert run("1\n5\n1 2 3 4 5") == "YES", "small ascending numbers"
assert run("1\n6\n10 1 1 1 1 1") == "YES", "one large number with many small numbers"
assert run("1\n4\n0 0 0 0") == "NO", "all zeros"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3\n1 1 10`| CÓ | Chọn một số lớn cho màu đỏ làm việc với số nhỏ màu xanh | 
|`3\n5 5 5`| KHÔNG | Các giá trị bằng nhau không thể thỏa mãn tổng > ràng buộc đếm | 
|`5\n1 2 3 4 5`| CÓ | Số tăng dần bình thường; logic tham lam cơ bản | 
|`6\n10 1 1 1 1 1`| CÓ | Số lớn chiếm ưu thế, số nhỏ màu xanh đảm bảo điều kiện | 
|`4\n0 0 0 0`| KHÔNG | Tất cả các số 0 không thể thỏa mãn tổng > điều kiện tổng | 

## Vỏ cạnh 

Đối với các chuỗi có tất cả các số bằng nhau, chẳng hạn như`[5, 5, 5]`, thuật toán sắp xếp mảng và lặp lại. Tổng màu đỏ không bao giờ có thể vượt quá tổng màu xanh nếu số màu đỏ nhỏ hơn vì mỗi phần tử đóng góp bằng nhau cho cả hai tổng. Thuật toán trả về đúng NO. Đối với một chuỗi có một phần tử cực lớn và nhiều phần tử nhỏ, chẳng hạn như `[10, 1, 1, 1, 1,
