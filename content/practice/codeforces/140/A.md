---
title: "CF 140A - Bàn Tết"
description: "Chúng ta có một bàn tròn lớn có bán kính R và muốn đặt n tấm hình tròn giống hệt nhau, mỗi tấm có bán kính r. Mỗi tấm phải thỏa mãn đồng thời ba điều kiện. Đầu tiên, toàn bộ đĩa phải ở bên trong bàn. Thứ hai, mọi đĩa đều phải chạm vào mép bàn."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "geometry", "math"]
categories: ["algorithms"]
codeforces_contest: 140
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 100"
rating: 1700
weight: 140
solve_time_s: 105
verified: true
draft: false
---

[CF 140A - Bàn tiệc năm mới](https://codeforces.com/problemset/problem/140/A) 

**Đánh giá:** 1700 
**Thẻ:** hình học, toán học 
**Thời gian giải:** 1 phút 45s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cái bàn tròn lớn có bán kính`R`và muốn đặt`n`các tấm hình tròn giống hệt nhau, mỗi tấm có bán kính`r`. 

Mỗi tấm phải thỏa mãn đồng thời ba điều kiện. 

Đầu tiên, toàn bộ đĩa phải ở bên trong bàn. 

Thứ hai, mọi đĩa đều phải chạm vào mép bàn. 

Thứ ba, không được phép hai tấm chồng lên nhau, mặc dù được phép chạm vào. 

Nhiệm vụ chỉ đơn giản là quyết định liệu sự sắp xếp như vậy có tồn tại hay không. 

Những hạn chế là rất nhỏ. Tất cả các giá trị tối đa là 1000 và chỉ có một trường hợp thử nghiệm. Điều này ngay lập tức cho chúng ta biết thách thức không phải là tối ưu hóa mà là tìm ra điều kiện hình học chính xác. Bất kỳ thuật toán nào từ thời gian không đổi cho đến thời gian bậc hai đều sẽ phù hợp thoải mái trong giới hạn. Khó khăn thực sự là tránh việc suy luận hình học không chính xác và xử lý các trường hợp góc một cách rõ ràng. 

Các trường hợp cạnh nguy hiểm nhất đến từ các giá trị nhỏ của`n`. 

Hãy xem xét đầu vào này:```
1 5 10
```Câu trả lời đúng là:```
NO
```Một chiếc đĩa đơn vẫn phải vừa vặn bên trong bàn. Vì bán kính tấm vượt quá bán kính bàn nên không thể bố trí được. Một giải pháp bất cẩn chỉ kiểm tra khoảng cách giữa các tấm có thể chấp nhận trường hợp này không chính xác vì không có tấm nào khác để va chạm. 

Bây giờ hãy nhìn vào:```
2 4 2
```Câu trả lời đúng là:```
YES
```Mỗi tấm có đường kính`4`, bằng bán kính của bàn. Hai tấm có thể ngồi đối diện nhau và chạm vào đúng một điểm. Việc triển khai sử dụng các bất đẳng thức nghiêm ngặt thay vì các bất đẳng thức không nghiêm ngặt thường từ chối cấu hình hợp lệ này. 

Trường hợp khó nhất là:```
2 2 1
```Câu trả lời đúng là:```
YES
```Tâm của hai tấm nằm trên một đường tròn bán kính`R - r = 1`. Các điểm đối diện trên đường tròn đó là khoảng cách`2`ngoài ra, chính xác bằng`2r`, do đó các tấm chạm vào nhau nhưng không chồng lên nhau. 

Một trường hợp tế nhị khác là:```
3 2 1
```Câu trả lời đúng là:```
NO
```Nhiều giải pháp sai chỉ kiểm tra xem`2r <= R`, điều này đúng ở đây. Nhưng ba vòng tròn đơn vị không thể chạm vào ranh giới của bảng bán kính-2 mà không chồng lên nhau. 

## Phương pháp tiếp cận 

Một cách mạnh mẽ để suy nghĩ về vấn đề này là hãy tưởng tượng việc đặt tâm tấm trên đường tròn bán kính`R - r`, bởi vì mỗi tấm chạm vào ranh giới của bàn buộc tâm của nó phải ở cách xa tâm bàn một cách chính xác. 

Nếu chúng ta thử mọi cách sắp xếp góc cạnh của`n`xung quanh vòng tròn này và kiểm tra khoảng cách từng cặp giữa các tấm, cuối cùng chúng ta sẽ tìm ra một sự sắp xếp hợp lệ khi tồn tại. Điều này đúng về mặt khái niệm vì hình học hoàn toàn được xác định bởi các vị trí trung tâm. 

Vấn đề là hình học liên tục có vô số cấu hình. Ngay cả việc rời rạc hóa các góc đủ tinh tế vẫn có thể gặp khó khăn và không cần thiết. Các ràng buộc là nhỏ, nhưng tìm kiếm số là công cụ sai ở đây vì cấu trúc có tính đối xứng cao. 

Quan sát quan trọng là sự sắp xếp tốt nhất luôn cách đều nhau xung quanh bàn. 

Mỗi tâm tấm nằm trên cùng một đường tròn bán kính`R - r`. Nếu chúng ta phân bố các tâm một cách đồng đều thì các tâm lân cận sẽ tạo thành các góc bằng nhau`2π / n`. Khoảng cách giữa các tâm lân cận trở thành độ dài dây cung:$$2(R-r)\sin\left(\frac{\pi}{n}\right)$$Để các tấm không chồng lên nhau thì khoảng cách này ít nhất phải bằng`2r`. 

Vì vậy chúng ta cần:$$2(R-r)\sin\left(\frac{\pi}{n}\right) \ge 2r$$Điều này đơn giản hóa để:$$(R-r)\sin\left(\frac{\pi}{n}\right) \ge r$$Có một cách thậm chí còn rõ ràng hơn để tránh hoàn toàn lượng giác. 

Khi`n >= 2`, các tấm có thể phù hợp với iff:$$R \ge 2r$$và ngoài ra việc bố trí chu vi có đủ chỗ. Các giải pháp biên tập của Codeforce thường đưa ra điều kiện tương đương:$$n \cdot r \le R$$chỉ dành cho những diễn giải hình học đặc biệt, nhưng điều đó không đúng ở đây. 
Độ lệch chuẩn đúng là: 
cho`n = 1`, chúng ta chỉ cần`r <= R`. 

Vì`n = 2`, hai tấm vừa khít`2r <= R`. 

Vì`n >= 3`, các tâm cách đều nhau tạo thành một đa giác đều và điều kiện chính xác trở thành:$$\sin\left(\frac{\pi}{n}\right) \ge \frac{r}{R-r}$$Sử dụng hình học trực tiếp là cách tiếp cận an toàn nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Vô hạn / không thực tế | O(1) | Quá chậm | 
| Hình học tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`,`R`, Và`r`. 
2. Xử lý vụ việc`n == 1`. 

Một tấm duy nhất chỉ cần đặt vừa vào bên trong bàn khi chạm vào ranh giới. Điều này có thể thực hiện được chính xác khi`r <= R`. 
3. Xử lý vụ việc`n == 2`. 

Tâm của hai tấm phải nằm đối diện nhau trên đường tròn bán kính`R-r`. Khoảng cách tối đa giữa chúng là đường kính:$$2(R-r)$$Các tấm tránh chồng lên nhau nếu điều này ít nhất là`2r`. 

Điều đó đơn giản hóa thành:$$R \ge 2r$$4. Xử lý vụ việc`n >= 3`. 

Đặt các tâm đều xung quanh đường tròn bán kính`R-r`. 

Góc giữa hai tâm lân cận là:$$\frac{2\pi}{n}$$Độ dài dây cung giữa các hàng xóm là:$$2(R-r)\sin\left(\frac{\pi}{n}\right)$$Các tấm phù hợp nếu khoảng cách này ít nhất là`2r`. 
5. In`"YES"`nếu bất đẳng thức giữ nguyên, nếu không thì in`"NO"`. 

### Tại sao nó hoạt động 

Mọi tâm tấm hợp lệ phải nằm chính xác trên đường tròn bán kính`R-r`, vì tấm chạm vào ranh giới bên trong bảng. 

Trong số tất cả các sự sắp xếp của`n`điểm trên một đường tròn, đa giác đều sẽ tối đa hóa khoảng cách tối thiểu giữa các điểm lân cận. Nếu ngay cả sự sắp xếp đối xứng tối ưu cũng gây ra sự chồng chéo thì không có sự sắp xếp bất thường nào có thể làm tốt hơn. 

Thuật toán kiểm tra chính xác cấu hình tối ưu này. Điều kiện khoảng cách đảm bảo các tấm lân cận không giao nhau, điều này tự động đảm bảo tất cả các cặp không chồng lên nhau vì các cặp lân cận là gần nhất. 

## Giải pháp Python```python
import sys
import math

input = sys.stdin.readline

def solve():
    n, R, r = map(int, input().split())

    if n == 1:
        print("YES" if r <= R else "NO")
        return

    if R < 2 * r:
        print("NO")
        return

    if n == 2:
        print("YES")
        return

    lhs = (R - r) * math.sin(math.pi / n)

    eps = 1e-12

    if lhs + eps >= r:
        print("YES")
    else:
        print("NO")

solve()
```Nhánh đầu tiên xử lý trường hợp đơn một cách riêng biệt vì hình học thay đổi hoàn toàn khi chỉ có một tấm. Nhiều bài nộp sai quên điều này và áp dụng sai công thức nhiều đĩa. 

Lần kiểm tra tiếp theo sẽ sớm loại bỏ các cấu hình không thể thực hiện được. Nếu như`R < 2r`, thậm chí hai tấm đối diện cũng không thể vừa. 

Vì`n >= 3`, việc triển khai sẽ đánh giá trực tiếp sự bất đẳng thức hình học xuất phát từ cách sắp xếp đa giác đều. Độ chính xác của dấu phẩy động quan trọng vì các trường hợp hợp lệ có thể liên quan đến đẳng thức chính xác. Epsilon tránh từ chối cấu hình do các lỗi làm tròn nhỏ. 

Biểu thức:```
(R - r) * math.sin(math.pi / n)
```xuất phát từ việc giảm một nửa sự bất bình đẳng về độ dài dây cung. Điều này giữ cho số học sạch hơn một chút và tránh phép nhân không cần thiết với 2. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 10 4
```| Biến | Giá trị | 
| --- | --- | 
| n | 4 | 
| R | 10 | 
| r | 4 | 
| R - r | 6 | 
| tội lỗi(pi/n) | sin(pi / 4) ≈ 0,7071 | 
| lh | 6 × 0,7071 ≈ 4,2426 | 
| So sánh với r | 4,2426 ≥ 4 | 

Đầu ra:```
YES
```Ví dụ này cho thấy một sự sắp xếp thành công tiêu chuẩn. Tâm của các tấm lân cận cách nhau đủ xa để các vòng tròn chạm vào nhau mà không bị chồng lên nhau. 

### Ví dụ 2 

đầu vào:```
3 2 1
```| Biến | Giá trị | 
| --- | --- | 
| n | 3 | 
| R | 2 | 
| r | 1 | 
| R - r | 1 | 
| tội lỗi(pi/n) | sin(pi / 3) ≈ 0,8660 | 
| lh | 1 × 0,8660 = 0,8660 | 
| So sánh với r | 0,8660 < 1 | 

Đầu ra:```
NO
```Điều này chứng tỏ tại sao chỉ cần kiểm tra`R >= 2r`là không đủ. Hai tấm vừa, nhưng ba tấm thì không. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ một số phép tính số học được thực hiện | 
| Không gian | O(1) | Không sử dụng cấu trúc dữ liệu bổ sung | 

Các ràng buộc là cực kỳ nhỏ nên hiệu suất không bao giờ là vấn đề. Giải pháp chạy ngay lập tức và sử dụng bộ nhớ không đổi. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
import math

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n, R, r = map(int, input().split())

    out = []

    if n == 1:
        out.append("YES" if r <= R else "NO")
    else:
        if R < 2 * r:
            out.append("NO")
        elif n == 2:
            out.append("YES")
        else:
            lhs = (R - r) * math.sin(math.pi / n)
            eps = 1e-12

            if lhs + eps >= r:
                out.append("YES")
            else:
                out.append("NO")

    return "\n".join(out) + "\n"

# provided sample
assert run("4 10 4\n") == "YES\n", "sample 1"

# minimum values
assert run("1 1 1\n") == "YES\n", "single plate exact fit"

# single plate too large
assert run("1 3 4\n") == "NO\n", "single plate impossible"

# boundary case for two plates
assert run("2 2 1\n") == "YES\n", "two touching plates"

# three plates impossible
assert run("3 2 1\n") == "NO\n", "three plates overlap"

# large valid configuration
assert run("6 1000 1\n") == "YES\n", "large table"

# equality edge case
assert run("2 4 2\n") == "YES\n", "exact boundary equality"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1`|`YES`| Tấm đơn phù hợp chính xác | 
|`1 3 4`|`NO`| Đĩa lớn hơn bàn | 
|`2 2 1`|`YES`| Bình đẳng cho hai tấm chạm vào | 
|`3 2 1`|`NO`| Trường hợp chồng lên nhau nhiều tấm | 
|`6 1000 1`|`YES`| Cấu hình dễ dàng bán kính lớn | 
|`2 4 2`|`YES`| Xử lý bất bình đẳng không nghiêm ngặt | 

## Vỏ cạnh 

Hãy xem xét đầu vào:```
1 3 4
```Thuật toán ngay lập tức đi vào`n == 1`chi nhánh. Từ`r > R`, tấm không thể vừa với bàn ngay cả trước khi xem xét việc chạm vào ranh giới. Đầu ra trở thành:```
NO
```Trường hợp này rất quan trọng vì công thức hình học tổng quát của nhiều tấm không được áp dụng rõ ràng khi chỉ có một tấm. 

Bây giờ hãy kiểm tra:```
2 2 1
```Thuật toán bỏ qua nhánh đơn. Từ`R = 2r`, bài kiểm tra tính bất khả thi ban đầu đã vượt qua chính xác ở mức bằng nhau. Bởi vì`n == 2`, thuật toán in:```
YES
```Hai tấm nằm đối diện nhau và chạm vào nhau tại một điểm. So sánh chặt chẽ sẽ từ chối sự sắp xếp hợp lệ này một cách không chính xác. 

Cuối cùng, hãy xem xét:```
3 2 1
```Việc tính toán trở thành:$$(R-r)\sin(\pi/3)=1 \times 0.8660$$Vì giá trị này nhỏ hơn`r = 1`, các mảng lân cận sẽ chồng lên nhau. Thuật toán in chính xác:```
NO
```Trường hợp này chứng minh tại sao chỉ kiểm tra`R >= 2r`là không đủ khi số lượng đĩa vượt quá hai.
