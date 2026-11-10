---
title: "CF 1532D - Thành lập đội"
description: "Chúng ta có số học sinh chẵn, mỗi học sinh có một trình độ kỹ năng lập trình. Mục tiêu là thành lập chính xác các đội $n/2$, mỗi đội có hai học sinh. Một đội chỉ có giá trị nếu cả hai học sinh có cùng trình độ kỹ năng."
date: "2026-06-10T16:37:19+07:00"
tags: ["codeforces", "competitive-programming", "*special", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1532
codeforces_index: "D"
codeforces_contest_name: "Kotlin Heroes: Practice 7"
rating: 0
weight: 1532
solve_time_s: 81
verified: true
draft: false
---

[CF 1532D - Thành lập nhóm](https://codeforces.com/problemset/problem/1532/D) 

**Đánh giá:** - 
**Thẻ:** *đặc biệt, phân loại 
**Thời gian giải:** 1 phút 21s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có số học sinh chẵn, mỗi học sinh có một trình độ kỹ năng lập trình. Mục tiêu là hình thành chính xác$n/2$các đội, mỗi đội có hai học sinh. Một đội chỉ có giá trị nếu cả hai học sinh có cùng trình độ kỹ năng. Để đạt được điều này, học sinh có thể giải quyết các vấn đề, mỗi vấn đề sẽ nâng cao kỹ năng của họ thêm một lần. 

Đầu vào bao gồm$n$(số lượng sinh viên) và một dãy số nguyên biểu thị các kỹ năng. Đầu ra là một số nguyên duy nhất: tổng số bài toán tối thiểu mà học sinh cần giải để mỗi học sinh được ghép thành một nhóm có kỹ năng như nhau. 

Các hạn chế là nhỏ:$2 \le n \le 100$Và$1 \le a_i \le 100$. Với$n$giới hạn ở mức 100, bất kỳ thuật toán nào lên tới$O(n^2 \log n)$hoặc$O(n^2)$sẽ chạy thoải mái trong thời gian giới hạn. Điều này cho thấy rằng ngay cả cách tiếp cận so sánh theo cặp cũng khả thi, nhưng chúng ta nên hướng tới điều gì đó đơn giản và trực tiếp hơn. Các trường hợp khó khăn có thể gặp phải một giải pháp đơn giản bao gồm có nhiều bản sao hoặc tất cả các giá trị duy nhất, trong đó số lượng vấn đề tối thiểu có thể yêu cầu các cặp không rõ ràng. 

Một cách tiếp cận bất cẩn sẽ chỉ đơn giản là ghép cặp học sinh theo thứ tự đầu vào. Ví dụ, với đầu vào`4\n1 3 2 2`, ghép nối`(1,3)`Và`(2,2)`sẽ tốn kém`2`cho cặp đầu tiên, nhưng giải pháp tốt hơn là`(1,2)`Và`(2,3)`chỉ có chi phí`1`. Điều này cho thấy thứ tự ghép nối các vấn đề. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ xem xét mọi khả năng ghép đôi của$n$sinh viên. có$(n-1)!!$cách để tạo thành cặp (giai thừa kép), là giai thừa trong tăng trưởng và không thực tế ngay cả đối với$n=20$. Ở mỗi cặp, chúng tôi sẽ tính toán chi phí để cân bằng các kỹ năng. Cách tiếp cận này đúng về nguyên tắc nhưng không khả thi về mặt tính toán. 

Nhận xét quan trọng là chi phí để cân bằng hai học sinh chỉ phụ thuộc vào sự khác biệt tuyệt đối về kỹ năng của họ. Điều này gợi ý rằng nếu chúng ta sắp xếp học sinh theo kỹ năng, việc ghép các học sinh liền kề sẽ giảm thiểu chi phí. Việc sắp xếp đảm bảo rằng sự khác biệt giữa các học sinh được ghép đôi là nhỏ nhất có thể và bất kỳ việc ghép đôi nào khác sẽ chỉ làm tăng tổng số khác biệt. Sau khi được sắp xếp, chiến lược tối ưu là ghép học sinh thứ nhất với học sinh thứ hai, học sinh thứ ba với học sinh thứ tư, v.v. Điều này làm giảm vấn đề xuống$O(n \log n)$để sắp xếp cộng$O(n)$để tổng hợp sự khác biệt. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((n-1)!!) | O(n) | Quá chậm | 
| Tối ưu (sắp xếp + cặp liền kề) | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc$n$và hàng loạt kỹ năng$a$. Việc sắp xếp yêu cầu cấu trúc dữ liệu cho phép sắp xếp; một danh sách là đủ. 
2. Sắp xếp mảng$a$theo thứ tự không giảm. Điều này đảm bảo rằng sự khác biệt giữa các phần tử liên tiếp được giảm thiểu. 
3. Khởi tạo một biến`total_cost`về không. Điều này sẽ tích lũy số lượng vấn đề cần giải quyết. 
4. Lặp lại mảng theo hai bước, ghép nối$a[i]$với$a[i+1]$. Với mỗi cặp, tính giá thành như sau$a[i+1] - a[i]$và thêm nó vào`total_cost`. Ghép nối theo thứ tự sắp xếp đảm bảo tổng chi phí tối thiểu. 
5. Sau khi xử lý tất cả các cặp, xuất ra`total_cost`. 

Tại sao nó hoạt động: Sau khi sắp xếp, mỗi học sinh được ghép nối với cấp độ kỹ năng gần nhất có thể. Bất kỳ nỗ lực nào để ghép chéo học sinh sẽ thay thế những khác biệt nhỏ bằng những khác biệt lớn hơn, làm tăng tổng số bài toán cần có. Điều bất biến là sau khi sắp xếp, các phần tử liên tiếp tạo thành các cặp sai phân tối thiểu tối ưu toàn cục. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
a = list(map(int, input().split()))
a.sort()

total_cost = 0
for i in range(0, n, 2):
    total_cost += a[i+1] - a[i]

print(total_cost)
```Giải pháp bắt đầu bằng việc đọc dữ liệu đầu vào một cách hiệu quả. Sắp xếp đảm bảo sự khác biệt theo cặp tối thiểu. Vòng lặp lặp lại theo hai bước để tạo thành các cặp chính xác. Máy tính`a[i+1] - a[i]`trực tiếp đưa ra số lượng bài toán cần thiết cho mỗi đội. Các trường hợp khó khăn như trùng lặp hoặc các kỹ năng giống hệt nhau không yêu cầu vấn đề nào mà phép trừ tự động xử lý. 

## Ví dụ đã hoạt động 

**Mẫu 1:** 

đầu vào:`6\n5 10 2 3 14 5`Mảng được sắp xếp:`[2, 3, 5, 5, 10, 14]`| tôi | Cặp | Chi phí | tổng_chi phí | 
| --- | --- | --- | --- | 
| 0 | (2,3) | 1 | 1 | 
| 2 | (5,5) | 0 | 1 | 
| 4 | (10,14) | 4 | 5 |
