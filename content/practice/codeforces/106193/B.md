---
title: "CF 106193B - Hộp đóng bìa"
description: "Chúng ta được cung cấp một số công-ten-nơ vận chuyển ứng viên, mỗi công-ten-nơ là một hộp hình chữ nhật được mô tả bằng ba chiều dài cạnh. Hộp lưu niệm cũng phải là hộp hình chữ nhật và phải được đặt bên trong mỗi hộp đựng nhất định."
date: "2026-06-19T18:39:46+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106193
codeforces_index: "B"
codeforces_contest_name: "2025-2026 ICPC NERC (NEERC), North-Western Russia Regional Contest (Northern Subregionals)"
rating: 0
weight: 106193
solve_time_s: 48
verified: true
draft: false
---

[CF 106193B - Hộp đóng khung](https://codeforces.com/problemset/problem/106193/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 48s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một số công-ten-nơ vận chuyển ứng viên, mỗi công-ten-nơ là một hộp hình chữ nhật được mô tả bằng ba chiều dài cạnh. Hộp lưu niệm cũng phải là hộp hình chữ nhật và phải được đặt bên trong mỗi hộp đựng nhất định. Điểm tự do chính là hộp lưu niệm có thể được xoay tùy ý, miễn là các cạnh của nó vẫn thẳng hàng với trục của hộp đựng sau khi xoay. Điều này có nghĩa là chúng tôi không so sánh trực tiếp các kích thước cố định mà hỏi xem liệu một hộp 3D có thể được định hướng khác nhau bên trong mỗi vùng chứa hay không. 

Nhiệm vụ là chọn kích thước hộp lưu niệm sao cho vừa với tất cả các hộp đựng cùng một lúc và trong số tất cả các hộp khả thi như vậy, chúng tôi muốn có thể tích tối đa có thể. 

Các ràng buộc đủ nhỏ để bất kỳ giải pháp nào liên quan đến việc kiểm tra từng cặp trên 1000 hộp đều có thể chấp nhận được, vì tổng số phép so sánh sẽ vào khoảng vài triệu phép tính. Bất kỳ khối nào trong một tham số lớn hơn đều không thể thực hiện được, nhưng ở đây cấu trúc đủ đơn giản để chúng ta có thể mong đợi một giải pháp O(n) hoặc O(n log n). 

Một điểm tinh tế là phép quay sẽ loại bỏ mọi khái niệm về khớp tọa độ cố định. Một hộp có các cạnh (a, b, c) xếp vừa vào một hộp đựng (x, y, z) khi và chỉ khi sau khi sắp xếp cả hai bộ ba, mỗi kích thước của hộp lưu niệm không vượt quá kích thước tương ứng của hộp đựng. Điều này tránh được những sai lầm trong đó người ta có thể giả định không chính xác thứ tự cố định của các bên. 

Một trường hợp thất bại phổ biến phát sinh khi bỏ qua việc xoay. Ví dụ: nếu chúng ta có một vùng chứa (2, 6, 4) và một vùng chứa khác (6, 2, 4), một cách tiếp cận đơn giản có thể xử lý các ràng buộc theo thứ nguyên và kết luận các giới hạn không nhất quán. Trong thực tế, cả hai đều mô tả cùng một tập hợp ràng buộc sau khi được sắp xếp. 

Một trường hợp khác là khi một chiều lớn hơn nhiều nhưng chiều khác nhỏ hơn trên các hộp khác nhau, khiến cho việc lấy trực tiếp cực tiểu trên mỗi tọa độ mà không cần sắp xếp trở nên hấp dẫn. Điều đó không thành công vì sự tương ứng tọa độ không nhất quán giữa các hộp. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng đoán kích thước hộp lưu niệm và kiểm tra xem nó có vừa với mọi hộp đựng theo một số vòng quay hay không. Ngay cả khi chúng tôi giới hạn bản thân ở các thứ nguyên ứng cử viên bắt nguồn từ các giá trị đầu vào, chúng tôi vẫn cần xem xét các hoán vị cho từng hộp, dẫn đến hành vi theo cấp số nhân nếu được thực hiện một cách đơn giản trên nhiều hộp. 

Quan sát quan trọng là việc xoay làm cho mỗi hộp bị ràng buộc hoàn toàn về các kích thước được sắp xếp. Sau khi mỗi hộp được sắp xếp theo (a ≤ b ≤ c), điều kiện “hộp lưu niệm vừa vặn” sẽ trở thành ba giới hạn trên độc lập về kích thước được sắp xếp của hộp lưu niệm. Để tối đa hóa khối lượng theo nhiều giới hạn trên, chúng tôi muốn đẩy từng thứ nguyên càng lớn càng tốt trong khi vẫn hợp lệ cho tất cả các hộp. 

Điều này biến vấn đề thành tính toán trên tất cả các hộp, giới hạn trên tối thiểu có thể có cho thứ nguyên nhỏ nhất, thứ nguyên ở giữa và thứ nguyên lớn nhất sau khi sắp xếp từng hộp. Ba cực tiểu này trực tiếp xác định hộp lưu niệm được sắp xếp lớn nhất có thể thực hiện được. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | hàm mũ | O(1) | Quá chậm | 
| Sắp xếp + Tổng hợp tối thiểu | O(n log 1) = O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

## 1. Chuẩn hóa từng container 

Đối với mỗi hộp đầu vào, hãy sắp xếp ba chiều của nó để chúng ta luôn coi nó là (x ≤ y ≤ z). Bước này loại bỏ sự mơ hồ do xoay vòng và đảm bảo so sánh nhất quán. 

## 2. Theo dõi các ràng buộc toàn cục 

Duy trì ba biến thể hiện giới hạn trên tốt nhất có thể cho kích thước hộp lưu niệm: min_x, min_y, min_z, tất cả đều được khởi tạo ở mức vô cùng. Đối với mỗi hộp, hãy cập nhật các giá trị này bằng cách lấy giá trị tối thiểu với thứ nguyên được sắp xếp tương ứng. 

Điều này hiệu quả vì mỗi thùng chứa đều hạn chế kích thước của từng kích thước được đặt hàng của hộp lưu niệm một cách độc lập.

##3. Thi công hộp lưu niệm tối ưu 

Sau khi xử lý tất cả các container, hộp lưu niệm hợp lệ lớn nhất có kích thước (min_x, min_y, min_z). Khối lượng của nó là tích của ba giá trị này. 

Chúng ta không cần phải thử sắp xếp lại giữa các vùng chứa nữa vì việc sắp xếp đã căn chỉnh tất cả các ràng buộc vào một hệ thống có thể so sánh được. 

## Tại sao nó hoạt động 

Việc sắp xếp từng hộp làm giảm khả năng tự do xoay thành dạng chuẩn. Bất kỳ vị trí hợp lệ nào đều tương ứng với việc căn chỉnh hộp lưu niệm đã sắp xếp với một số hoán vị của hộp đựng, vốn đã được hấp thụ trong quá trình phân loại. Do đó, cách duy nhất để vi phạm tính khả thi là vượt quá giới hạn sắp xếp của ít nhất một vùng chứa ở một chiều nào đó. Việc lấy cực tiểu trên tất cả các vùng chứa sẽ bảo toàn chính xác giao điểm của tất cả các vùng ràng buộc hợp lệ trong không gian được sắp xếp và tích của các giới hạn này mang lại khối lượng tối đa có thể do mỗi thứ nguyên được tối đa hóa độc lập dưới các ràng buộc đơn điệu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
min_x = min_y = min_z = 10**18

for _ in range(n):
    a, b, c = map(int, input().split())
    a, b, c = sorted((a, b, c))
    min_x = min(min_x, a)
    min_y = min(min_y, b)
    min_z = min(min_z, c)

print(min_x * min_y * min_z)
```Giải pháp đọc từng hộp và ngay lập tức chuẩn hóa nó bằng cách sắp xếp các kích thước của nó. Điều này rất cần thiết vì nó mã hóa quyền tự do xoay thành một thứ tự cố định. Ba cực tiểu đang chạy lưu trữ ràng buộc chặt chẽ nhất trên tất cả các hộp cho từng thứ nguyên vị trí trong biểu diễn được sắp xếp. Phép nhân cuối cùng tính thể tích của hộp lớn nhất thỏa mãn đồng thời tất cả các ràng buộc. 

Một lỗi triển khai phổ biến là quên sắp xếp từng bộ ba. Nếu không sắp xếp, việc lấy cực tiểu theo tọa độ sẽ trộn lẫn các trục không tương thích và tạo ra hộp không hợp lệ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
6 5 6
2 10 10
3 8 4
```Chúng tôi xử lý từng hộp từng bước. 

| Hộp | Đã sắp xếp | phút_x | phút_y | phút_z | 
| --- | --- | --- | --- | --- | 
| (6,5,6) | (5,6,6) | 5 | 6 | 6 | 
| (2,10,10) | (2,10,10) | 2 | 6 | 6 | 
| (3,8,4) | (3,4,8) | 2 | 4 | 6 | 

Kích thước cuối cùng là (2, 4, 6), cho tập 48. 

Dấu vết này cho thấy cách các ràng buộc tích lũy độc lập dọc theo các kích thước được sắp xếp và cách hộp nhỏ nhất trong tất cả các ràng buộc xác định hình dạng cuối cùng. 

### Ví dụ 2 

đầu vào:```
2
1 100 100
50 2 80
```| Hộp | Đã sắp xếp | phút_x | phút_y | phút_z | 
| --- | --- | --- | --- | --- | 
| (1.100.100) | (1.100.100) | 1 | 100 | 100 | 
| (50,2,80) | (2,50,80) | 1 | 50 | 80 | 

Kết quả cuối cùng là (1, 50, 80), tập 4000. 

Ví dụ này nêu bật cách một chiều thứ nhất rất nhỏ trong một hộp có thể chiếm ưu thế trong câu trả lời cuối cùng ngay cả khi các chiều khác lớn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi hộp được sắp xếp theo thời gian không đổi và được xử lý một lần | 
| Không gian | O(1) | Chỉ có ba biến theo dõi được duy trì | 

Thuật toán tuyến tính về số lượng loại hộp, dễ dàng nằm trong giới hạn n lên tới 1000. Việc sử dụng bộ nhớ không đổi ngoại trừ bộ nhớ đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import prod

    n = int(input())
    min_x = min_y = min_z = 10**18

    for _ in range(n):
        a, b, c = map(int, input().split())
        a, b, c = sorted((a, b, c))
        min_x = min(min_x, a)
        min_y = min(min_y, b)
        min_z = min(min_z, c)

    return str(min_x * min_y * min_z)

# provided sample
assert run("3\n6 5 6\n2 10 10\n3 8 4\n") == "48"

# minimum case
assert run("1\n1 1 1\n") == "1"

# all equal boxes
assert run("2\n3 3 3\n3 3 3\n") == "27"

# one tight dimension dominates
assert run("2\n1 100 100\n50 2 80\n") == "4000"

# permutation stress
assert run("3\n2 3 4\n4 2 3\n3 4 2\n") == "24"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| khối đơn vị | 1 | ranh giới tối thiểu | 
| hình khối giống hệt nhau | 27 | ổn định dưới sự trùng lặp | 
| kích thước lớn/nhỏ hỗn hợp | 4000 | thống trị bởi hạn chế nhỏ nhất | 
| chỉ hoán vị | 24 | tính đúng đắn khi xoay | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi ràng buộc chặt chẽ nhất cho mỗi chiều đến từ các hộp khác nhau. Ví dụ: một hộp có thể buộc một cạnh tối thiểu rất nhỏ, một hộp khác ràng buộc cạnh giữa và một hộp khác ràng buộc cạnh lớn nhất. 

đầu vào:```
3
1 100 100
100 2 100
100 100 3
```Xử lý: 

Bộ hộp đầu tiên (1.100.100) (min_x, min_y, min_z) = (1.100.100). 

Hộp thứ hai (2.100.100 được sắp xếp từ 100,2.100) cập nhật lên (1,2.100). 

Hộp thứ ba (3.100.100 được sắp xếp từ 100.100,3) cập nhật lên (1,2,3). 

Đầu ra cuối cùng là 6. 

Thuật toán xử lý chính xác điều này vì mỗi thứ nguyên được theo dõi độc lập trong không gian được sắp xếp, do đó, các ràng buộc từ các hộp khác nhau sẽ kết hợp một cách tự nhiên mà không yêu cầu bất kỳ quyết định căn chỉnh hộp chéo nào.
