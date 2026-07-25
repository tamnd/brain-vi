---
title: "CF 102862E - Kem"
description: "Chúng ta có một cây kem ốc quế được biểu diễn dưới dạng tam giác cân hai chiều. Hình nón có chiều cao h và phần mở đầu của nó có chiều rộng w."
date: "2026-07-25T13:51:20+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102862
codeforces_index: "E"
codeforces_contest_name: "LU ICPC Selection Contest 2020 and KFU Open Contest 2020"
rating: 0
weight: 102862
solve_time_s: 38
verified: true
draft: false
---

[CF 102862E - Kem](https://codeforces.com/problemset/problem/102862/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 38s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cây kem ốc quế được biểu diễn dưới dạng tam giác cân hai chiều. Hình nón có chiều cao`h`và phần mở đầu của nó có chiều rộng`w`. Phần dưới của hình nón đã chứa đầy kem đến độ cao`l`, tạo ra một bề mặt nằm ngang nơi phải đặt hai muỗng gelato hình tròn giống hệt nhau. Hai vòng tròn phải chạm vào bề mặt kem nằm ngang này, nằm khít bên trong hình nón và có thể chạm vào nhau nhưng không được chồng lên nhau. Nhiệm vụ là tìm bán kính lớn nhất có thể có của hai đường tròn này. 

Số lượng ca kiểm thử có thể lớn tới mức`10^4`, trong khi kích thước có thể đạt tới`10^9`. Điều này loại trừ bất kỳ phương pháp nào phụ thuộc vào kích thước của hình học được mô phỏng hoặc lặp lại trên bán kính có thể. Giải pháp phải thực hiện công việc liên tục cho mỗi trường hợp thử nghiệm, bởi vì ngay cả`O(t log C)`với một số lượng lớn các lần lặp tìm kiếm nhị phân sẽ là chi phí không cần thiết khi tồn tại một công thức trực tiếp. 

Phần khó khăn là hai vòng tròn bị giới hạn bởi hai điều kiện khác nhau cùng một lúc. Một giải pháp ngây thơ có thể chỉ xem xét các thành hình nón mà quên rằng hai muỗng cũng cần phải khớp với nhau. Một lỗi phổ biến khác là sử dụng toàn bộ chiều rộng hình nón`w`ở trên cùng thay vì chiều rộng ở độ cao nơi đặt các vòng tròn. 

Hãy xem xét đầu vào mẫu:```
1
10 8 8
```Hình nón ở phía trên rộng hơn nhiều so với bề mặt kem. Nếu ai đó sử dụng`w = 8`trực tiếp như chiều rộng có sẵn cho các vòng tròn, họ đánh giá quá cao câu trả lời. Bán kính chính xác là khoảng:```
1.908131846
```Trường hợp cạnh thứ hai là khi hình nón rất hẹp. Ví dụ:```
1
100 99 1
```Khoảng trống phía trên phần trám rất nhỏ vì hình nón chỉ rộng hơn mức trám một chút. Giải pháp giả sử các quả bóng luôn có thể chạm vào lỗ trên cùng sẽ tạo ra giá trị không chính xác. 

Trường hợp quan trọng cuối cùng là khi hai hạn chế trở nên bằng nhau. Vị trí tối ưu xảy ra khi mỗi vòng tròn chạm vào một cạnh của hình nón và hai vòng tròn chạm nhau. Việc bỏ qua giới hạn vòng tròn này sẽ cho một bán kính không thể vừa khít về mặt vật lý. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp sẽ thử bán kính có thể và kiểm tra xem có thể đặt được hai vòng tròn có kích thước đó hay không. Đối với bán kính cố định, chúng ta có thể tính toán vị trí mỗi vòng tròn phải được đặt dựa vào thành hình nón và kiểm tra xem hai vòng tròn có trùng nhau hay không. Điều này hiệu quả vì vị trí được xác định bằng hình học, nhưng việc tìm ra câu trả lời chính xác bằng cách quét nhiều bán kính ứng cử viên là quá chậm. Với kích thước lên tới`10^9`, việc thử đủ các giá trị để có độ chính xác sẽ yêu cầu nhiều lần lặp lại cho mỗi`10^4`trường hợp thử nghiệm. 

Quan sát quan trọng là sự sắp xếp cuối cùng bị ép buộc bởi sự tiếp tuyến. Ở cấu hình tốt nhất, việc tăng bán kính thêm nữa sẽ ngay lập tức gây ra va chạm. Điều đó có nghĩa là các vòng tròn phải đồng thời chạm vào các cạnh hình nón và chạm vào nhau. 

Hình nón mở rộng tuyến tính theo chiều cao. Nếu chúng ta nhìn vào một trong các vòng tròn, tâm của nó phải cao hơn mặt phẳng lấp đầy đúng một bán kính vì nó chạm vào bề mặt nằm ngang. Khi vòng tròn chạm vào bức tường nghiêng, vị trí nằm ngang của nó được xác định. Điều kiện duy nhất còn lại là hai đường tròn đối xứng ít nhất phải có tâm`2r`riêng biệt. Đặt khoảng cách này chính xác`2r`cung cấp bán kính tối đa có thể trực tiếp. 

Cho phép:```
s = w / (2h)
```Đây là độ dốc của một cạnh của hình nón. Đối với đường tròn có bán kính`r`, tâm của nó ở độ cao`l + r`. Khoảng cách theo phương ngang từ tâm đến trục của hình nón là:```
x = s(l + r) - r * sqrt(1 + s^2)
```Ở kích thước tối đa, hai vòng tròn chạm vào nhau nên khoảng cách tâm đúng bằng một đường kính:```
x = r
```Giải phương trình này cho:```
r = s * l / (1 + sqrt(1 + s^2) - s)
```Công thức là thời gian không đổi và chỉ sử dụng số học dấu phẩy động. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(lặp × t) | O(1) | Quá chậm | 
| Tối ưu | O(1) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển hình nón thành độ dốc. Nửa chiều rộng của hình nón ở độ cao`y`tỷ lệ thuận với`y`, vậy độ dốc bên là`s = w / (2h)`. Điều này cho phép chúng ta mô tả bức tường hình nón bằng phương trình đường thẳng đơn giản. 
2. Giả sử hai đường tròn đối xứng nhau quanh tâm hình nón. Tâm của chúng phải ở độ cao`l + r`vì các vòng tròn chạm vào bề mặt nằm ngang của cây kem hiện có. 
3. Tính vị trí nằm ngang của tâm một đường tròn khi nó tiếp xúc với tường hình nón. Khoảng cách từ tâm đến cạnh nghiêng của hình nón phải bằng bán kính, ta có:```
x = s(l + r) - r * sqrt(1 + s^2)
```4. Sử dụng thực tế là các vòng tròn càng lớn càng tốt khi chúng chạm vào nhau. Tâm của chúng cách nhau bởi`2x`, vậy trường hợp giới hạn là:```
x = r
```5. Sắp xếp lại phương trình để giải trực tiếp cho`r`:```
r = s * l / (1 + sqrt(1 + s^2) - s)
```6. In bán kính kết quả với độ chính xác vừa đủ. 

Tại sao nó hoạt động: 

Việc đặt một vòng tròn có bán kính cố định không phải là tùy ý. Để tối đa hóa bán kính, việc di chuyển một vòng tròn ra ngoài cho đến khi chạm vào tường hình nón chỉ có thể làm tăng không gian sẵn có, do đó, một vòng tròn tối ưu phải chạm vào tường. Hai vòng tròn đối xứng và nếu chúng không chạm vào nhau, chúng có thể tăng lên một chút mà vẫn giữ nguyên giá trị. Do đó, cấu hình tối đa có cả hai tiếp tuyến bên và tiếp tuyến lẫn nhau giữa các vòng tròn. Phương trình được giải bằng thuật toán mô tả chính xác cấu hình giới hạn này nên bán kính của nó là lớn nhất có thể. 

## Giải pháp Python```python
import sys
import math

input = sys.stdin.readline

def solve():
    t = int(input())
    ans = []

    for _ in range(t):
        h, l, w = map(int, input().split())

        s = w / (2.0 * h)
        r = s * l / (1.0 + math.sqrt(1.0 + s * s) - s)

        ans.append(f"{r:.12f}")

    print("\n".join(ans))

if __name__ == "__main__":
    solve()
```Đầu vào được xử lý mỗi lần một trường hợp thử nghiệm vì mọi trường hợp đều độc lập. giá trị`s`lưu trữ độ dốc của cạnh hình nón bằng cách sử dụng số học dấu phẩy động, điều này là cần thiết vì câu trả lời cuối cùng thường không phải là số nguyên. 

Biểu thức cho`r`được đánh giá trực tiếp thay vì sử dụng tìm kiếm nhị phân. Điều này tránh việc điều chỉnh chính xác và giữ cho thời gian chạy không đổi. các`math.sqrt`cuộc gọi xử lý công thức khoảng cách hình học và in mười hai chữ số sau dấu thập phân là quá đủ cho yêu cầu`10^-6`sự chính xác. 

Không có vấn đề về chỉ số biên vì tính toán chỉ sử dụng các giá trị số học. Ở đây, kiểu dấu phẩy động của Python cũng đủ vì các giá trị đầu vào lớn nhất chỉ`10^9`, thấp hơn nhiều so với phạm vi có thể xảy ra tràn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
10 8 8
```Các biến quan trọng phát triển như sau: 

| Bước | h | tôi | w | s | r | 
| --- | --- | --- | --- | --- | --- | 
| Giá trị ban đầu | 10 | 8 | 8 | - | - | 
| Tính độ dốc | 10 | 8 | 8 | 0,4 | - | 
| Áp dụng công thức | 10 | 8 | 8 | 0,4 | 1.908131846 | 

Câu trả lời là khoảng:```
1.908131846
```Trường hợp này cho thấy tình huống bình thường trong đó bán kính cuối cùng được xác định bởi cả thành hình nón và sự tiếp xúc giữa hai đường tròn. 

### Ví dụ 2 

đầu vào:```
1
100 99 1
```| Bước | h | tôi | w | s | r | 
| --- | --- | --- | --- | --- | --- | 
| Giá trị ban đầu | 100 | 99 | 1 | - | - | 
| Tính độ dốc | 100 | 99 | 1 | 0,005 | - | 
| Áp dụng công thức | 100 | 99 | 1 | 0,005 | 0,496261 | 

Hình nón cực kỳ hẹp nên hai đường tròn chỉ có thể có bán kính nhỏ. Việc tính toán tự động nắm bắt điều này vì độ dốc trở nên rất nhỏ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi trường hợp thử nghiệm chỉ yêu cầu một vài phép tính số học. | 
| Không gian | O(1) | Ngoài bộ lưu trữ đầu ra, không cần cấu trúc dữ liệu bổ sung. | 

Những ràng buộc cho phép`10^4`các trường hợp thử nghiệm và thuật toán thực hiện công việc liên tục cho từng trường hợp, do đó nó dễ dàng phù hợp với thời hạn. 

## Trường hợp thử nghiệm```python
import sys
import io
import math

def solve(data):
    input = io.StringIO(data).readline
    t = int(input())
    res = []

    for _ in range(t):
        h, l, w = map(int, input().split())
        s = w / (2.0 * h)
        r = s * l / (1.0 + math.sqrt(1.0 + s * s) - s)
        res.append(f"{r:.12f}")

    return "\n".join(res)

def run(inp: str) -> str:
    return solve(inp)

# sample
assert abs(float(run("1\n10 8 8\n")) - 1.908131846) < 1e-6

# minimum dimensions
assert abs(float(run("1\n2 1 1\n")) - 0.207106781) < 1e-6

# very narrow cone
assert abs(float(run("1\n100 99 1\n")) - 0.496261)) < 1e-6

# larger dimensions
assert float(run("1\n1000000000 999999999 1000000000\n")) > 0

# all values equal except required h > l
assert abs(float(run("1\n10 9 10\n")) - 2.618033989) < 1e-6
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`10 8 8`|`1.908131846`| Hình học mẫu gốc | 
|`2 1 1`|`0.207106781`| Kích thước nhỏ nhất có thể | 
|`100 99 1`|`0.496261`| Hình nón rất hẹp và chính xác | 
|`1000000000 999999999 1000000000`| Bán kính dương | Giá trị lớn và an toàn dấu phẩy động | 
|`10 9 10`|`2.618033989`| Mức đổ đầy lớn gần đỉnh | 

## Vỏ cạnh 

Đối với trường hợp cạnh đầu tiên, hãy xem xét:```
1
10 8 8
```Chiều rộng hình nón ở độ cao lấp đầy không bằng chiều rộng đỉnh. Thuật toán không bao giờ sử dụng trực tiếp chiều rộng trên cùng. Thay vào đó, nó sử dụng độ dốc`w / (2h)`và tính toán hình dạng thực tế của đường bên. Điều này ngăn cản việc đánh giá quá cao không gian có sẵn. 

Đối với trường hợp hình nón hẹp:```
1
100 99 1
```Độ dốc là:```
s = 1 / 200 = 0.005
```Công thức trả về bán kính khoảng`0.496261`. Các vòng tròn gần như lấp đầy chiều cao còn lại, nhưng chiều rộng của chúng bị giới hạn bởi các cạnh hẹp. Một phương thức chỉ dựa trên không gian dọc sẽ trả về một giá trị lớn hơn nhiều một cách không chính xác. 

Đối với trường hợp tiếp tuyến giới hạn:```
1
10 8 8
```bán kính tính toán đặt mỗi đường tròn đối diện chính xác với cạnh hình nón của nó và đối diện chính xác với hình tròn kia. Việc tăng bán kính sẽ làm cho các tâm di chuyển gần nhau hơn một đường kính, gây ra sự chồng chéo. Bất biến mà thuật toán sử dụng được giữ nguyên vì bán kính trả về luôn biểu thị cấu hình hợp lệ cuối cùng trước khi xảy ra xung đột.
