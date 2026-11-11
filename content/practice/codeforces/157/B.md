---
title: "CF 157B - Vết"
description: "Bài toán trình bày một tình huống trong đó một bức tường được trang trí bằng nhiều vòng tròn đồng tâm, một số vòng tròn được sơn màu đỏ trong khi một số khác có màu xanh lam theo kiểu xen kẽ. Vùng ngoài cùng ngoài vòng tròn lớn nhất luôn có màu xanh lam."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "geometry", "sortings"]
categories: ["algorithms"]
codeforces_contest: 157
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 110 (Div. 2)"
rating: 1000
weight: 157
solve_time_s: 139
verified: true
draft: false
---

[CF 157B - Dấu vết](https://codeforces.com/problemset/problem/157/B) 

**Đánh giá:** 1000 
**Thẻ:** hình học, sắp xếp 
**Thời gian giải:** 2m 19s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Bài toán trình bày một tình huống trong đó một bức tường được trang trí bằng nhiều vòng tròn đồng tâm, một số vòng tròn được sơn màu đỏ trong khi một số khác có màu xanh lam theo kiểu xen kẽ. Vùng ngoài cùng ngoài vòng tròn lớn nhất luôn có màu xanh lam. Nhiệm vụ là tính tổng diện tích của tất cả các vùng sơn đỏ trên bức tường này. Đầu vào cung cấp số vòng tròn,`n`và bán kính riêng của chúng là số nguyên. Mỗi bán kính biểu thị khoảng cách từ tâm chung đến cạnh của một vòng tròn. Đầu ra phải là một số thực biểu thị diện tích tổng hợp của tất cả các vùng màu đỏ, được tính bằng tổng diện tích của vòng tròn màu đỏ bên trong và bất kỳ vòng màu đỏ nào được hình thành giữa các vòng tròn liên tiếp. 

Những ràng buộc chỉ ra rằng`n`tương đối nhỏ, tối đa 100 và mỗi bán kính tối đa là 1000. Điều này có nghĩa là một giải pháp lặp qua các vòng tròn và thực hiện các phép tính số học cơ bản là đủ hiệu quả. Các trường hợp cạnh cần xem xét cẩn thận bao gồm chỉ có một vòng tròn, các vòng tròn được cung cấp theo thứ tự không được sắp xếp hoặc vòng tròn lớn nhất rất nhỏ hoặc rất lớn. Một cách tiếp cận đơn giản nhằm cố gắng mô phỏng toàn bộ bức tranh hoặc sử dụng lưới 2D sẽ phức tạp và chậm chạp một cách không cần thiết, nhưng các phép tính sắp xếp và số học trên các khu vực là đủ. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng mô phỏng bức tường dưới dạng mặt phẳng 2D và đánh dấu từng đơn vị diện tích là màu đỏ hoặc xanh lam. Điều này sẽ liên quan đến việc lặp lại tất cả các điểm trong lưới 2D có kích thước tỷ lệ với bán kính lớn nhất, tính toán khoảng cách từ tâm và tính tổng các khu vực dựa trên màu sắc. Mặc dù đúng về mặt logic nhưng độ phức tạp tính toán lại cực kỳ lớn. Ví dụ: bán kính 1000 sẽ tạo ra một triệu điểm lưới, khiến phương pháp này không hiệu quả. 

Cách tiếp cận tối ưu dựa trên quan sát hình học quan trọng: diện tích của mỗi đoạn hoặc vòng tròn có thể được tính bằng công thức tính diện tích hình tròn,`π * r^2`. Mẫu màu xen kẽ có nghĩa là các vùng màu đỏ là mỗi vòng tròn thứ hai bắt đầu từ vòng tròn trong cùng (hoặc vòng tròn đầu tiên theo thứ tự được sắp xếp). Việc sắp xếp bán kính theo thứ tự giảm dần giúp đơn giản hóa quá trình ghép các vòng tròn bên ngoài và bên trong để tính diện tích vòng. Đối với một vòng màu đỏ giữa hai vòng tròn bán kính`R`Và`r`Ở đâu`R > r`, diện tích là`π * (R^2 - r^2)`. Hình tròn màu đỏ trong cùng có diện tích`π * r^2`vì nó không có vòng tròn bên trong. Tổng các diện tích này sẽ được tổng diện tích sơn đỏ. Phương pháp này làm giảm đáng kể độ phức tạp vì chỉ cần thực hiện các phép tính sắp xếp và số học. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n*r_max^2) | O(r_max^2) | Quá chậm đối với bán kính lớn | 
| Tối ưu | O(n log n) | O(1) | Hiệu quả và được chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc giá trị đầu vào: số vòng tròn`n`và danh sách bán kính. 
2. Sắp xếp bán kính theo thứ tự giảm dần. Điều này đảm bảo rằng vòng tròn ngoài cùng là đầu tiên, đơn giản hóa việc tính diện tích vòng tròn. 
3. Khởi tạo một biến để giữ tổng diện tích màu đỏ,`red_area`, bắt đầu từ số không. 
4. Lặp lại danh sách bán kính đã sắp xếp, xem xét từng vòng tròn thứ hai bắt đầu từ chỉ số 0 (vòng tròn màu đỏ ngoài cùng). Đối với mỗi vòng tròn sơn đỏ: 

một. Nếu đó không phải là hình tròn trong cùng thì hãy tính diện tích của vòng màu đỏ như sau:`π * (R^2 - r^2)`Ở đâu`R`là bán kính hình tròn hiện tại và`r`là bán kính đường tròn bên trong tiếp theo. 

b. Nếu là hình tròn trong cùng thì tính diện tích của nó như sau`π * R^2`. 

c. Thêm diện tích tính toán vào`red_area`. 
5. Sau khi duyệt qua tất cả các vòng tròn màu đỏ, hãy in hoặc quay lại`red_area`. 

Tại sao nó hoạt động: Thuật toán tận dụng đặc tính hình học của các vòng tròn đồng tâm. Bằng cách sắp xếp bán kính, chúng tôi đảm bảo rằng mỗi vùng màu đỏ có thể được biểu diễn chính xác dưới dạng một vòng tròn hoặc một vòng. Mẫu màu xen kẽ được bảo toàn bằng cách lấy mỗi vòng tròn thứ hai. Điều này đảm bảo rằng tất cả các vùng sơn đỏ đều được tính toán chính xác một lần, không bị chồng chéo hoặc bỏ sót. 

## Giải pháp Python```python
import sys
import math
input = sys.stdin.readline

n = int(input())
radii = list(map(int, input().split()))

radii.sort(reverse=True)
red_area = 0.0

for i in range(0, n, 2):
    outer = radii[i]
    inner = radii[i+1] if i+1 < n else 0
    red_area += math.pi * (outer**2 - inner**2)

print(f"{red_area:.10f}")
```Việc triển khai Python trước tiên sẽ đọc số vòng tròn và bán kính của chúng. Sắp xếp theo thứ tự giảm dần đảm bảo ghép nối chính xác các vòng tròn để tính diện tích vòng. Vòng lặp lặp lại trên mỗi vòng tròn thứ hai để chọn các vùng được sơn màu đỏ. các`inner`bán kính bằng 0 nếu không có vòng tròn bên trong, xử lý chính xác vòng tròn trong cùng. Khu vực cuối cùng được in với độ chính xác cao. 

## Ví dụ đã hoạt động 

Đối với đầu vào:```
1
1
```Danh sách bán kính được sắp xếp là`[1]`. Chỉ có một hình tròn nên vùng màu đỏ là`π * 1^2 = π ≈ 3.1415926536`. 

Đối với đầu vào:```
3
1 4 2
```Danh sách bán kính được sắp xếp là`[4, 2, 1]`. Các vùng màu đỏ là các vòng tròn ngoài cùng (chỉ số 0) và trong cùng (chỉ số 2). Vòng màu đỏ đầu tiên là`π * (4^2 - 2^2) = π * (16 - 4) = 12π`. Vòng tròn màu đỏ trong cùng là`π * 1^2 = π`. Tổng diện tích màu đỏ =`13π ≈ 40.8407044967`. 

| Lặp lại | Bên ngoài | Nội tâm | Đã thêm khu vực màu đỏ | Diện tích màu đỏ tích lũy | 
| --- | --- | --- | --- | --- | 
| 0 | 4 | 2 | 12π | 12π | 
| 2 | 1 | 0 | π | 13π | 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp bán kính chiếm ưu thế; lặp qua vòng tròn là O(n) | 
| Không gian | O(1) | Chỉ có một số lượng biến cố định; đầu vào có thể được đọc tại chỗ | 

Sắp xếp có hiệu quả đối với`n ≤ 100`và diện tích tính toán là không đáng kể khi so sánh. Thuật toán phù hợp thoải mái trong giới hạn bộ nhớ và thời gian. 

## Trường hợp thử nghiệm```
import math

# Sample cases
assert math.isclose(run("1\n1\n"), math.pi, rel_tol=1e-10), "Single circle"
assert math.isclose(run("3\n1 4 2\n"), 13*math.pi, rel_tol=1e-10), "Three circles, unsorted input"

# Edge cases
assert math.isclose(run("2\n1000 1\n"), math.pi*(1000**2 - 1**2), rel_tol=1e-10), "Two circles, large difference"
assert math.isclose(run("5\n5 4 3 2 1\n"), math.pi*(5**2 - 4**2 + 3**2 - 2**2 + 1**2), rel_tol=1e-10), "Multiple circles"
assert math.isclose(run("1\n1000\n"), math.pi*1000**2, rel_tol=1e-10), "Single large circle"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 hình tròn bán kính 1 | π | Tính đúng đắn cho vòng tròn đơn | 
| 3 hình tròn bán kính 1,4,2 | 13π | Xử lý chính xác dữ liệu đầu vào chưa được sắp xếp và nhiều vùng màu đỏ | 
| 2 hình tròn bán kính 1000,1 | π*(1000^2-1^2) | Xử lý đúng các giá trị lớn | 
| 5 hình tròn, bán kính 5,4,3,2,1 | π*(5^2-4^2 + 3^2-2^2 + 1^2) | Đúng mẫu xen kẽ cho nhiều vòng tròn | 
| 1 hình tròn bán kính 1000 | π*1000^2 | Xử lý đúng vòng tròn lớn đơn | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là khi chỉ có một vòng tròn. Thuật toán xử lý vấn đề này bằng cách đặt bán kính bên trong về 0, tính toán chính xác diện tích của một vòng tròn có màu đỏ. Một trường hợp khó phát hiện khác là khi các vòng tròn không được sắp xếp theo thứ tự; việc sắp xếp đảm bảo rằng bán kính bên ngoài và bên trong được ghép nối chính xác để tính diện tích vòng. Cuối cùng, khi vòng tròn lớn nhất lớn hơn đáng kể so với vòng tròn tiếp theo, phép trừ bán kính bình phương sẽ tránh được việc tính các diện tích chồng lấp. Đối với tất cả các trường hợp này, thứ tự sắp xếp và logic lựa chọn xen kẽ đảm bảo tính chính xác mà không có bất kỳ lỗi tràn hoặc lỗi tràn nào.
