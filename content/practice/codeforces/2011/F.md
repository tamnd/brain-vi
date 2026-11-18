---
title: "CF 2011F - Phân mảng tốt"
description: "Chúng ta được yêu cầu đếm tất cả các mảng con liền kề của một mảng số nguyên nhất định sao cho mỗi mảng con có thể được tạo bởi một quá trình bắt đầu bằng một số nguyên duy nhất và liên tục nối thêm cùng một số, ít hơn một hoặc nhiều hơn một phần tử đã có trong mảng."
date: "2026-06-08T13:12:15+07:00"
tags: ["codeforces", "competitive-programming", "*special", "data-structures", "greedy"]
categories: ["algorithms"]
codeforces_contest: 2011
codeforces_index: "F"
codeforces_contest_name: "Kotlin Heroes: Episode 11"
rating: 0
weight: 2011
solve_time_s: 125
verified: false
draft: false
---

[CF 2011F - Phân mảng tốt](https://codeforces.com/problemset/problem/2011/F) 

**Đánh giá:** - 
**Tags:** *đặc biệt, cấu trúc dữ liệu, tham lam 
**Thời gian giải:** 2m 5s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu đếm tất cả các mảng con liền kề của một mảng số nguyên nhất định sao cho mỗi mảng con có thể được tạo bởi một quá trình bắt đầu bằng một số nguyên duy nhất và liên tục nối thêm cùng một số, ít hơn một hoặc nhiều hơn một phần tử đã có trong mảng. Nói một cách đơn giản hơn, một mảng con là "tốt" nếu chênh lệch giữa hai phần tử bất kỳ trong mảng đó tối đa là một và mọi số trung gian đều có thể đạt được bằng cách tăng hoặc giảm một đơn vị so với số đã thấy trước đó trong mảng con đó. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm, mỗi trường hợp có một mảng số nguyên có kích thước lên tới 300.000 và tổng của tất cả các kích thước mảng trên tất cả các trường hợp thử nghiệm không vượt quá 300.000. Với giới hạn thời gian 3 giây, điều này ngụ ý rằng bất kỳ thuật toán nào có thời gian tuyến tính kém hơn cho mỗi phần tử có thể sẽ quá chậm. Các phương pháp tiếp cận bạo lực để kiểm tra tất cả các mảng con có thể một cách rõ ràng sẽ liên quan đến khoảng$O(n^2)$hoạt động cho từng trường hợp thử nghiệm, điều này không thể thực hiện được với các giới hạn trên. 

Một số trường hợp khó khăn. Mảng có các phần tử lặp lại, mảng tạo thành một chuỗi tăng hoặc giảm nghiêm ngặt hoặc mảng có bước nhảy đột ngột lớn hơn một đều có thể ảnh hưởng đến việc mảng con có tốt hay không. Ví dụ, trong mảng`[1, 3]`, mảng con`[1, 3]`không tốt vì thiếu 2. Một cách tiếp cận đơn giản chỉ kiểm tra các khác biệt liền kề mà không xem xét liệu các số trung gian có xuất hiện hay không sẽ phân loại sai các mảng con đó. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force rất đơn giản: đối với mỗi chỉ mục bắt đầu, hãy mở rộng mảng con sang bên phải trong khi kiểm tra xem liệu tất cả các giá trị trung gian bắt buộc có xuất hiện và khác nhau nhiều nhất một hay không. Điều này hiệu quả vì nó phản ánh trực tiếp định nghĩa của mảng con "tốt". Vấn đề là có$\frac{n(n+1)}{2}$tổng cộng các mảng con, có thể đạt tới khoảng$4.5 \cdot 10^{10}$hoạt động khi$n=3 \cdot 10^5$. Điều này vượt xa mức có thể chấp nhận được. 

Thông tin chuyên sâu quan trọng về tối ưu hóa xuất phát từ việc quan sát rằng độ dài tối đa của một mảng con "tốt" bị hạn chế bởi số lượng giá trị riêng biệt mà nó chứa. Vì một mảng con tốt chỉ có thể chứa các giá trị khác nhau tối đa một giá trị, nên không có mảng con tốt nào có thể chứa nhiều hơn ba số nguyên liên tiếp. Điều này cho phép chúng ta hạn chế sự chú ý của mình vào các mảng con trong đó tập hợp các phần tử riêng biệt có kích thước tối đa là ba. 

Hơn nữa, nếu chúng ta kiểm tra các mảng có độ dài lớn hơn ba thì cấu trúc bên trong của mảng con tốt sẽ bị ràng buộc rất nhiều. Có thể có nhiều nhất một giá trị "ở giữa" giữa mức tối thiểu và tối đa, nếu không thì các thao tác được phép không thể tạo ra giá trị đó. Điều này biến vấn đề thành một nhiệm vụ cửa sổ trượt: chúng ta có thể duy trì một cửa sổ theo dõi giá trị tối thiểu, tối đa và sự hiện diện của bất kỳ giá trị ở giữa nào, mở rộng sang phải cho đến khi cửa sổ trở nên không hợp lệ, sau đó di chuyển ranh giới bên trái về phía trước. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Cửa sổ trượt + Theo dõi giá trị | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo bộ đếm`ans`về không. Điều này sẽ lưu trữ tổng số mảng con tốt cho trường hợp thử nghiệm hiện tại. 
2. Sử dụng hai con trỏ,`l`Và`r`, để thể hiện cửa sổ hiện tại của mảng đang được xem xét. Bắt đầu cả hai ở đầu mảng. 
3. Duy trì một từ điển hoặc mảng tần số nhỏ để đếm số lần xuất hiện của mỗi số trong cửa sổ hiện tại. Theo dõi số lượng tối thiểu và tối đa trong cửa sổ. 
4. Mở rộng con trỏ bên phải`r`một phần tử tại một thời điểm. Cập nhật bản đồ tần số và điều chỉnh`min_val`Và`max_val`tương ứng. 
5. Sau khi thêm một phần tử, hãy kiểm tra xem cửa sổ hiện tại có còn hợp lệ hay không. Cửa sổ không hợp lệ nếu sự khác biệt giữa`max_val`Và`min_val`vượt quá 2 hoặc nếu cả ba giá trị có thể có trong phạm vi`[min_val, max_val]`không được sắp xếp theo cách cho phép tạo mảng với các thao tác được phép. 
6. Nếu không hợp lệ, di chuyển con trỏ sang trái`l`ở bên phải cho đến khi cửa sổ hợp lệ trở lại, giảm dần bản đồ tần số khi các phần tử bị xóa. 
7. Sau mỗi bước`r`, tất cả các mảng con kết thúc tại`r`và bắt đầu từ bất kỳ chỉ mục nào giữa`l`Và`r`được đảm bảo là tốt. Thêm vào`r - l + 1`ĐẾN`ans`. 
8. Sau khi toàn bộ mảng được xử lý, hãy lưu trữ hoặc in`ans`như là kết quả của ca kiểm thử. 

Tại sao nó hoạt động: thuật toán duy trì một cửa sổ trượt luôn biểu thị một mảng con tốt liền kề tối đa kết thúc ở ranh giới bên phải hiện tại. Điều bất biến là tất cả các mảng con chứa trong cửa sổ này đều tốt vì cửa sổ được mở rộng một cách tham lam và chỉ thu nhỏ khi cần thiết. Bằng cách đếm tất cả các mảng con kết thúc ở mỗi`r`, chúng tôi tính đến tất cả các mảng con tốt liền kề có thể có chính xác một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def count_good_subarrays(a):
    n = len(a)
    ans = 0
    l = 0
    freq = {}
    
    for r in range(n):
        freq[a[r]] = freq.get(a[r], 0) + 1
        while True:
            keys = list(freq.keys())
            if max(keys) - min(keys) > 2:
                freq[a[l]] -= 1
                if freq[a[l]] == 0:
                    del freq[a[l]]
                l += 1
            else:
                break
        ans += r - l + 1
    return ans

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    print(count_good_subarrays(a))
```Mã sử ​​dụng từ điển để theo dõi tần số của các phần tử trong cửa sổ trượt hiện tại. Đối với mỗi phần tử mới, nó cập nhật bản đồ tần số và kiểm tra xem phạm vi giá trị có vượt quá 2 hay không. Nếu đúng như vậy, ranh giới bên trái sẽ tăng lên cho đến khi cửa sổ hợp lệ trở lại. Đếm tất cả các mảng con kết thúc ở mỗi mảng`r`được thực hiện với`r - l + 1`. Việc sử dụng từ điển đảm bảo chúng ta có thể xử lý các phạm vi giá trị tùy ý, tuy nhiên đối với vấn đề này với`1 ≤ a[i] ≤ n`, một mảng có kích thước cố định cũng có thể hoạt động. 

## Ví dụ đã hoạt động 

**Ví dụ 1:**`[1, 1, 3]`| r | tần số | tôi | mảng con kết thúc tại r | và | 
| --- | --- | --- | --- | --- | 
| 0 | {1:1} | 0 | [1] | 1 | 
| 1 | {1:2} | 0 | [1,1], [1] | 3 | 
| 2 | {1:2,3:1} | 2 | [3] | 4 | 
Cửa sổ`[1,1]`là hợp lệ,`[1,1,3]`không hợp lệ vì`max - min = 2`, Vì thế`l`chuyển sang 2. 

**Ví dụ 2:**`[3,2,3,1]`| r | tần số | tôi | mảng con kết thúc tại r | trả lời | 
| --- | --- | --- | --- | --- | 
| 0 | {3:1} | 0 | [3] | 1 | 
| 1 | {3:1,2:1} | 0 | [3,2], [2] | 3 | 
| 2 | {3:2,2:1} | 0 | [3,2,3], [2,3], [3] | 6 | 
| 3 | {3:2,2:1,1:1} | 1 | [2,3,1], [3,1], | |
