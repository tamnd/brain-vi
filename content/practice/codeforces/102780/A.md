---
title: "CF 102780A - Trà xanh"
description: "Tôi sẽ cung cấp bài xã luận dưới dạng một tài liệu độc lập hoàn chỉnh. Chỉnh sửa Nhiệm vụ là chọn bao nhiêu thìa nước nóng và nước lạnh cho vào cốc sao cho hỗn hợp thu được có nhiệt độ chính xác là 80 độ."
date: "2026-07-28T03:32:19+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102780
codeforces_index: "A"
codeforces_contest_name: "ICPC Central Russia Regional Contest (CRRC 19)"
rating: 0
weight: 102780
solve_time_s: 106
verified: true
draft: false
---

[CF 102780A - Trà xanh](https://codeforces.com/problemset/problem/102780/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 46s 
**Đã xác minh:** có 

## Giải pháp 
Tôi sẽ cung cấp bài xã luận dưới dạng một tài liệu độc lập hoàn chỉnh. 

Chỉnh sửa 

#Hiểu vấn đề 

Nhiệm vụ là chọn bao nhiêu thìa nước nóng và nước lạnh cho vào cốc sao cho hỗn hợp thu được có nhiệt độ chính xác là 80 độ. Nước nóng có nhiệt độ`t1`, nước lạnh có nhiệt độ`t2`, và đầu ra cần thiết là cặp tập`v1`Và`v2`. Trong số tất cả các cặp có thể có, chúng ta cần cặp có tổng số thìa nhỏ nhất. 

Quy tắc trộn nói rằng tổng nhiệt lượng sau khi trộn phải bằng giá trị trung bình có trọng số của hai nhiệt độ. Đối với vấn đề này, điều đó có nghĩa là:`t1 * v1 + t2 * v2 = 80 * (v1 + v2)`Các ràng buộc cho chúng ta biết rằng nhiệt độ nước nóng không bao giờ dưới 80 và nhiệt độ nước lạnh luôn dưới 80. Các giá trị đủ nhỏ để thậm chí kiểm tra nhiều kết hợp cũng phù hợp, nhưng phương trình có đủ cấu trúc để giải trực tiếp trong thời gian không đổi. Không cần mô phỏng hoặc tìm kiếm. 

Các trường hợp khó khăn chính xuất phát từ nhiệt độ khiến một mặt của hỗn hợp trở nên không cần thiết hoặc làm cho tỷ lệ trở nên rất đơn giản. Nếu như`t1`Đã 80 rồi, chỉ riêng nước nóng đã đạt nhiệt độ cần thiết. Đối với đầu vào`80 20`, đầu ra đúng là`1 0`. Một giải pháp bất cẩn chỉ dựa trên công thức tỉ số tổng quát có thể chia cho 0 vì`t1 - 80`trở thành số không. 

Một trường hợp khác là khi chênh lệch nhiệt độ không phải là nguyên tố cùng nhau. Đối với đầu vào`100 20`, sự khác biệt là`20`Và`60`. Tỷ lệ thô là`v1 : v2 = 60 : 20`, nhưng cặp hợp lệ nhỏ nhất là`3 : 1`. Một giải pháp không làm giảm phân số sẽ tạo ra các giá trị lớn không cần thiết. 

Trường hợp thứ ba là khi hai chênh lệch nhiệt độ đã có ước số chung lớn hơn một nhưng một bên gần đạt mục tiêu. Đối với đầu vào`81 79`, tỉ số cần tìm là`1 : 1`, vậy câu trả lời là`1 1`. Chỉ tìm kiếm từ một phía hoặc sử dụng phép tính dấu phẩy động có thể gây ra lỗi làm tròn ở đây. 

# Phương pháp tiếp cận 

Một cách tiếp cận đơn giản là thử đếm từng cặp thìa có thể. Chúng ta có thể lặp lại tất cả`v1`Và`v2`các giá trị từ 0 đến 1000, kiểm tra xem phương trình có thỏa mãn không và giữ cặp có tổng nhỏ nhất. Điều này đúng vì mọi câu trả lời được phép đều được xem xét. Trường hợp xấu nhất kiểm tra khoảng một triệu cặp, vì có khoảng`1001 * 1001`sự kết hợp. 

Cách tiếp cận bạo lực có tác dụng với các giới hạn đã cho nhưng nó che giấu cấu trúc toán học của bài toán. Nếu số lượng thìa được phép lớn hơn nhiều, số lượng kết hợp sẽ tăng theo phương trình bậc hai và nhanh chóng trở nên không thực tế. 

Quan sát hữu ích đến từ việc sắp xếp lại phương trình nhiệt độ:`t1 * v1 + t2 * v2 = 80 * v1 + 80 * v2`Các điều khoản di chuyển mang lại:`(t1 - 80) * v1 = (80 - t2) * v2`Phía bên trái mô tả nước nóng làm tăng nhiệt độ lên trên 80 bao nhiêu, và phía bên phải mô tả nước lạnh hạ nhiệt độ xuống dưới 80 bao nhiêu. Hai tác động phải cân bằng chính xác. 

Điều này đưa ra một tỷ lệ trực tiếp giữa hai tập. Lượng nước nóng phải tỷ lệ thuận với`80 - t2`, trong khi lượng nước lạnh phải tỷ lệ thuận với`t1 - 80`. Giảm tỷ lệ này bằng ước số chung lớn nhất sẽ cho nghiệm số nguyên nhỏ nhất có thể. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(1000²) | O(1) | Được chấp nhận cho các giới hạn nhất định | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

#Hướng dẫn thuật toán 

1. Đọc nhiệt độ nước nóng và lạnh. 
2. Kiểm tra xem nhiệt độ nước nóng có chính xác là 80 hay không. Trong tình huống này, việc thêm bất kỳ lượng nước lạnh nào sẽ chỉ làm tăng nhiệt độ ra xa mục tiêu, vì vậy một thìa nước nóng và không thìa nước lạnh là câu trả lời hợp lệ nhỏ nhất. 
3. Tính hai hiệu nhiệt độ:`hot_difference = t1 - 80`Và`cold_difference = 80 - t2`. Các giá trị này biểu thị mức độ thay đổi nhiệt độ của từng loại nước so với mục tiêu. 
4. Tìm ước chung lớn nhất của hai hiệu. Chia cả hai giá trị cho ước số này sẽ làm giảm tỷ lệ trong khi vẫn giữ phương trình cân bằng đúng. 
5. Đầu ra`cold_difference / gcd`như lượng nước nóng và`hot_difference / gcd`như lượng nước lạnh. Các giá trị được đặt theo cách này vì cần nhiều nước lạnh hơn khi nước lạnh thấp hơn nhiều so với 80 và cần nhiều nước nóng hơn khi nước nóng cao hơn nhiều so với 80. 

Tại sao nó hoạt động: Mọi hỗn hợp hợp lệ phải thỏa mãn phương trình`(t1 - 80) * v1 = (80 - t2) * v2`. Điều này có nghĩa là hai thể tích phải có cùng tỷ lệ chênh lệch nhiệt độ đối diện. Thuật toán xây dựng chính xác tỷ lệ đó và chia cho ước số chung lớn nhất, tạo ra cặp số nguyên dương nhỏ nhất thỏa mãn phương trình. Vì bất kỳ cặp hợp lệ nào khác sẽ là bội số của cặp rút gọn này nên không thể tồn tại tổng khối lượng nhỏ hơn. 

#Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t1, t2 = map(int, input().split())

    if t1 == 80:
        print(1, 0)
        return

    a = t1 - 80
    b = 80 - t2

    g = __import__("math").gcd(a, b)

    print(b // g, a // g)

if __name__ == "__main__":
    solve()
```Phần đầu tiên xử lý trường hợp đặc biệt khi nước nóng đã ở nhiệt độ mục tiêu. Công thức chung giả định rằng nước nóng đóng góp một lượng nhiệt dương trên 80, vì vậy trường hợp này cần xử lý riêng. 

Các biến`a`Và`b`lưu trữ hai độ lệch so với nhiệt độ mục tiêu. Phương trình nói rằng thể tích nước nóng được kết nối với độ lệch của nước lạnh và thể tích nước lạnh được kết nối với độ lệch của nước nóng, đó là lý do tại sao đầu ra cuối cùng sử dụng chúng theo thứ tự ngược lại. 

Ước chung lớn nhất được sử dụng để giảm tỷ lệ. Nếu không có sự rút gọn này, đáp án vẫn thỏa mãn phương trình nhưng sẽ không có tổng số thìa tối thiểu có thể có. 

Tất cả các phép tính đều sử dụng số nguyên nên không có vấn đề gì về độ chính xác của dấu phẩy động. Các giá trị cũng thấp hơn nhiều so với giới hạn số nguyên của Python, vì vậy việc tràn dữ liệu không phải là vấn đề đáng lo ngại. 

# Ví dụ đã hoạt động 

Đối với mẫu đầu tiên, đầu vào là:```
100 20
```Việc thực hiện có thể được theo dõi như sau. 

| Bước | t1 | t2 | hot_difference | lạnh_khác biệt | gcd | Đầu ra | 
| --- | --- | --- | --- | --- | --- | --- | 
| Ban đầu | 100 | 20 | 20 | 60 | | | 
| Sau gcd | 100 | 20 | 20 | 60 | 20 | 3 1 | 

Nước nóng cao hơn mục tiêu 20 độ và nước lạnh thấp hơn mục tiêu 60 độ. Nước nóng cần xuất hiện theo tỷ lệ chênh lệch của nước lạnh, tạo ra`60 : 20`, làm giảm đến`3 : 1`. 

Đối với mẫu thứ hai, đầu vào là:```
100 30
```Việc thực hiện là: 

| Bước | t1 | t2 | hot_difference | lạnh_khác biệt | gcd | Đầu ra | 
| --- | --- | --- | --- | --- | --- | --- | 
| Ban đầu | 100 | 30 | 20 | 50 | | | 
| Sau gcd | 100 | 30 | 20 | 50 | 10 | 5 2 | 

Nước nóng làm tăng nhiệt độ lên 20 độ mỗi thìa và nước lạnh giảm nhiệt độ xuống 50 độ mỗi thìa. Năm thìa nước nóng cân bằng hai thìa nước lạnh. 

# Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ các phép tính số học và một phép tính ước chung lớn nhất được thực hiện. | 
| Không gian | O(1) | Thuật toán chỉ lưu trữ một vài biến số nguyên. | 

Giải pháp thời gian không đổi dễ dàng phù hợp với giới hạn thời gian và giới hạn bộ nhớ vì nó tránh được việc liệt kê số lượng thìa có thể có. 

# Trường hợp thử nghiệm```python
import sys
import io
import math

def solve_data(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)

    t1, t2 = map(int, sys.stdin.readline().split())

    if t1 == 80:
        ans = "1 0"
    else:
        a = t1 - 80
        b = 80 - t2
        g = math.gcd(a, b)
        ans = f"{b // g} {a // g}"

    sys.stdin = old_stdin
    return ans

assert solve_data("100 20\n") == "3 1", "sample 1"
assert solve_data("100 30\n") == "5 2", "sample 2"

assert solve_data("80 0\n") == "1 0", "target hot water boundary"
assert solve_data("100 79\n") == "1 20", "small cold difference"
assert solve_data("81 79\n") == "1 1", "equal temperature differences"
assert solve_data("100 0\n") == "4 1", "maximum temperature range"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`80 0`|`1 0`| Kiểm tra trường hợp đặc biệt khi nước nóng đã có sẵn ở mục tiêu. | 
|`100 79`|`1 20`| Kiểm tra trường hợp cần một lượng lớn nước lạnh. | 
|`81 79`|`1 1`| Kiểm tra tỷ lệ cân bằng không đặc biệt nhỏ nhất. | 
|`100 0`|`4 1`| Kiểm tra chênh lệch nhiệt độ lớn nhất có thể. | 

# Vỏ cạnh 

Đối với đầu vào`80 20`, thuật toán ngay lập tức phát hiện nhiệt độ nước nóng bằng nhiệt độ mong muốn. Nó xuất ra`1 0`, vì chỉ một thìa nước nóng thôi đã tạo ra hỗn hợp 80 độ. Việc triển khai chỉ theo công thức có thể thất bại ở đây vì chênh lệch nhiệt độ nóng bằng 0. 

Đối với đầu vào`100 20`, thuật toán tính toán`hot_difference = 20`Và`cold_difference = 60`. Ước chung lớn nhất là 20 nên tỉ số rút gọn là`60 / 20 = 3`Và`20 / 20 = 1`. Đầu ra là`3 1`, sử dụng số lượng thìa tối thiểu. 

Đối với đầu vào`81 79`, thuật toán tính toán`hot_difference = 1`Và`cold_difference = 1`. Ước chung lớn nhất của chúng là 1 nên không cần rút gọn. Đầu ra`1 1`cho thấy rằng khoảng cách bằng nhau đến mục tiêu đòi hỏi lượng nước bằng nhau. 

Bài xã luận này cũng có thể được điều chỉnh thành một lời giải thích ngắn gọn hơn theo phong cách Codeforces hoặc được mở rộng thành một phiên bản dựa trên bằng chứng chính thức hơn.
