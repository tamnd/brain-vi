---
title: "CF 1530D - Ông già Noel bí mật"
description: "Chúng ta được yêu cầu xây dựng một bài tập giống như hoán vị cho một nhóm người. Mỗi người phải tặng quà cho đúng một người khác và không ai được phép tặng quà cho mình."
date: "2026-06-10T16:54:46+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "flows", "graphs", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1530
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 733 (Div. 1 + Div. 2, based on VK Cup 2021 - Elimination (Engine))"
rating: 1600
weight: 1530
solve_time_s: 158
verified: false
draft: false
---

[CF 1530D - Ông già Noel bí mật](https://codeforces.com/problemset/problem/1530/D) 

**Đánh giá:** 1600 
**Tags:** thuật toán xây dựng, dòng chảy, đồ thị, tham lam, toán học 
**Thời gian giải:** 2 phút 38 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xây dựng một bài tập giống như hoán vị cho một nhóm người. Mỗi người phải tặng quà cho đúng một người khác và không ai được phép tặng quà cho mình. Điều này có nghĩa là chúng ta đang xây dựng một hoán vị có kích thước`n`với hạn chế là không có vị trí nào ánh xạ tới chính nó. 

Ngoài ra, mỗi người có một người nhận ưa thích. Nếu người`i`tặng quà cho`a_i`, mong muốn đó được coi là thỏa mãn. Mục tiêu là xây dựng bất kỳ hoán vị giống như loạn trí hợp lệ nào nhằm tối đa hóa số lượng vị trí phù hợp với mục tiêu ưa thích của chúng. 

Vì vậy, nhiệm vụ không chỉ là xây dựng bất kỳ hoán vị hợp lệ nào mà còn là một hoán vị phù hợp với mảng đã cho`a`càng nhiều càng tốt trong khi vẫn đảm bảo mọi giá trị được sử dụng chính xác một lần và không tồn tại điểm cố định. 

Ràng buộc`n ≤ 2⋅10^5`trên tất cả các trường hợp thử nghiệm ngay lập tức buộc phải có giải pháp tuyến tính hoặc gần tuyến tính. Bất kỳ cách tiếp cận nào xem xét nhiều hoán vị, cố gắng khớp các tập hợp con một cách triệt để hoặc thực hiện khớp lặp lại bằng quay lui sẽ thất bại vì ngay cả`O(n log n)`mỗi trường hợp thử nghiệm có thể nằm ở ranh giới nếu được thực hiện với các hằng số nặng, trong khi`O(n^2)`là hoàn toàn không thể. 

Ràng buộc cấu trúc chính là đầu ra phải là một hoán vị. Điều này có nghĩa là mọi giá trị được sử dụng chính xác một lần, vì vậy ngay cả khi nhiều người muốn có cùng một mục tiêu thì chỉ một trong số họ có thể đạt được mục tiêu đó. Sự kết hợp toàn cầu này là nguyên nhân khiến việc phân công địa phương tham lam trở nên không an toàn. 

Một trường hợp thất bại tinh tế xuất hiện khi nhiều chỉ số trỏ đến cùng một`a_i`. Ví dụ, nếu tất cả`a_i = 1`, khi đó chỉ có thể gán một người vào 1 và những người còn lại phải được sắp xếp lại theo một chu kỳ. Một cách tiếp cận tham lam ngây thơ chỉ định`b_i = a_i`bất cứ khi nào có thể sẽ phá vỡ tính tiêm. 

Một tình huống rắc rối khác là khi việc làm theo mong muốn tạo ra một điểm cố định. Nếu chúng ta gán`b_i = a_i`thật tham lam, có thể một số lực lượng phân công cuối cùng còn sót lại`b_j = j`, điều đó bị cấm. Một ví dụ nhỏ là`a = [2, 1]`. Nếu chúng ta tham lam gán cả hai, chúng ta sẽ nhận được một chu trình hợp lệ, nhưng trong các trường hợp hỗn hợp lớn hơn, việc cố định các lựa chọn sớm một cách tham lam có thể khiến các vị trí cuối cùng rơi vào vòng tự lặp. 

Khó khăn cốt lõi là chúng tôi muốn tối đa hóa các kết quả khớp với một mảng nhất định theo ràng buộc hoán vị với các điểm cố định bị cấm. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ thử tất cả các hoán vị của`b`thỏa mãn`b_i ≠ i`rồi đếm xem có bao nhiêu vị trí trùng khớp`a_i`. Đây là một bài toán gán tiêu chuẩn có ràng buộc. Số lượng hoán vị hợp lệ là theo thứ tự biến dạng, đại khái là`n! / e`, vượt xa mọi giới hạn tính toán ngay cả đối với`n = 10^5`. Ngay cả đối với`n = 10`, điều này trở nên không thể thực hiện được. 

Một lực lượng vũ phu có cấu trúc hơn sẽ coi nó như một vấn đề chuyển nhượng và sử dụng kết hợp lưỡng cực tối đa giữa các nút bên trái`i`và các nút bên phải`j`, các cạnh cấm`i -> i`và tối đa hóa các kết quả phù hợp trong đó`j = a_i`có sự ưu tiên. Điều này trở thành một vấn đề kết hợp có trọng số với`n`các đỉnh ở mỗi bên và`O(n^2)`các cạnh. Giải pháp dòng chảy hoặc kiểu Hungary về mặt lý thuyết là đúng nhưng quá chậm để`n = 2⋅10^5`. 

Quan sát quan trọng là cấu trúc không khớp tùy ý. Mỗi nút chỉ có một cạnh “ưu tiên” mang lại giá trị 1 và tất cả các cạnh khác đều tương đương (giá trị 0). Vì vậy, chúng tôi đang tối đa hóa số lần chúng tôi có thể sử dụng các cạnh ưa thích trong khi vẫn tạo ra hoán vị. 

Điều này làm giảm vấn đề giải quyết xung đột bên trong các nhóm có sở thích giống hệt nhau và chỉ khắc phục các trường hợp việc lấy tất cả các cạnh ưu tiên sẽ phá vỡ tính tiêm nhiễm hoặc tạo ra một điểm cố định. 

Thực tế mang tính cấu trúc quan trọng là xung đột chỉ xảy ra trong các nhóm có cùng đẳng cấp.`a_i`. Giữa các nhóm, các nhiệm vụ là độc lập ngoại trừ tính sẵn có của các mục tiêu trên toàn cầu. Điều này cho thấy trước tiên chúng ta có thể chỉ định tất cả các cạnh ưu tiên “an toàn” và sau đó sửa chữa cấu trúc còn lại bằng cách sử dụng các vị trí còn sót lại, thường bằng cách tuần hoàn chúng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Hoán vị Brute Force | Ồ (n!) | O(n) | Quá chậm | 
| Kết hợp / Lưu lượng tối đa | O(n^2 √n) hoặc tệ hơn | O(n^2) | Quá chậm | 
| Tham lam với việc sửa chữa xe đạp | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tách các chỉ số theo mục tiêu ưa thích của chúng. Với mỗi giá trị`x`, chúng tôi duy trì danh sách các chỉ số muốn`x`. Sau đó chúng tôi cố gắng gán`x`đến một trong những chỉ số này. 

1. Nhóm tất cả các chỉ số theo giá trị mong muốn`a_i`. Điều này xác định tất cả các đối thủ cạnh tranh cho từng mục tiêu. 
2. Với mỗi giá trị`x`, xem xét tất cả các chỉ số muốn`x`. Nếu như`x`bản thân nó nằm trong số đó, chúng tôi cẩn thận vì việc chỉ định`b_x = x`bị cấm. 
3. Đối với từng nhóm chỉ số mong muốn`x`, chúng ta có thể gán`x`một trong số đó, nhưng không nhất thiết là tất cả. 
4. Chúng tôi duy trì một tập hợp các mục tiêu chưa sử dụng. Ban đầu mọi số`1..n`có sẵn. 
5. Chúng tôi lặp qua các chỉ số và cố gắng gán`b_i = a_i`chỉ nếu`a_i`vẫn chưa được sử dụng và`a_i ≠ i`. Nếu cả hai đều giữ, chúng tôi sẽ tạm gán nó và đánh dấu`a_i`như đã sử dụng. 
6. Sau giai đoạn tham lam này, một số vị trí vẫn chưa được chỉ định và một số giá trị vẫn chưa được sử dụng. Chúng tôi thu thập cả hai danh sách. 
7. Bây giờ chúng ta khớp các vị trí còn lại với các giá trị còn lại một cách tùy ý, nhưng phải tránh các điểm cố định. Nếu tại bất kỳ vị trí điểm nào`i`sẽ nhận được giá trị`i`, we swap it with another value.
 8. Một cách sửa tuyến tính đơn giản là xoay các giá trị còn sót lại theo một vị trí, đảm bảo không có điểm cố định miễn là có ít nhất hai phần tử. 

Lý do điều này có tác dụng là vì giai đoạn tham lam chỉ cam kết thực hiện các trận đấu an toàn khi mục tiêu chưa bị bắt. This ensures injectivity. Bất kỳ chỉ số chưa từng có nào còn lại tạo thành một bài toán con trong đó không thể gán ưu tiên mà không có xung đột, do đó tất cả cấu trúc còn lại có thể được sắp xếp lại một cách tự do. 

## Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, mỗi giá trị được chỉ định nhiều nhất một lần vì chúng tôi chỉ sử dụng mục tiêu khi nó vẫn chưa được sử dụng. Như vậy`b`vẫn là một hoán vị bất biến trong suốt quá trình xây dựng. 

Mỗi lần chúng tôi phân công`b_i = a_i`, chúng tôi tối đa hóa số lượng mong muốn được thỏa mãn cục bộ mà không chặn các nhiệm vụ trong tương lai vốn là điều không thể thực hiện được vì mọi nỗ lực sau này để sử dụng cùng một giá trị sẽ không hợp lệ. 

Các vị trí còn lại tạo thành một tập hợp trong đó không còn phép gán ưu tiên an toàn nào, nghĩa là mọi phép gán còn lại phải là hoán vị của các giá trị còn lại. Dịch chuyển theo chu kỳ trên tập dư này sẽ tránh được các điểm cố định khi xây dựng, vì việc dịch chuyển đảm bảo mọi chỉ mục ánh xạ tới một chỉ mục khác nhau. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        used = [False] * (n + 1)
        b = [-1] * n

        free_pos = []

        for i in range(n):
            x = a[i]
```
