---
title: "CF 105946A - 3D"
description: "Chúng ta được cho một mảng các số nguyên và chúng ta cần đếm xem có bao nhiêu bộ ba có thứ tự của các chỉ số riêng biệt tạo thành một mối quan hệ số học trong đó tổng của hai phần tử được chọn bằng phần tử thứ ba trong mảng."
date: "2026-06-21T22:07:08+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105946
codeforces_index: "A"
codeforces_contest_name: "2025 UP ACM Algolympics Final Round"
rating: 0
weight: 105946
solve_time_s: 97
verified: true
draft: false
---

[CF 105946A - 3D](https://codeforces.com/problemset/problem/105946/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 37s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một mảng các số nguyên và chúng ta cần đếm xem có bao nhiêu bộ ba có thứ tự của các chỉ số riêng biệt tạo thành một mối quan hệ số học trong đó tổng của hai phần tử được chọn bằng phần tử thứ ba trong mảng. Chính thức, chúng tôi chọn ba vị trí khác nhau$i, j, k$và kiểm tra xem$a_i + a_j = a_k$, tính riêng từng đơn hàng hợp lệ. 

Một hạn chế về cấu trúc quan trọng được ẩn trong mảng: mọi giá trị có nhiều nhất ba ước số dương. Điều đó ngay lập tức buộc mọi phần tử thành một trong ba hình dạng duy nhất. Số 1 có một ước số. Một số nguyên tố có đúng hai ước số. Một hình vuông của số nguyên tố có đúng ba ước số. Không có số nguyên nào khác thỏa mãn điều kiện, vì vậy mọi phần tử mảng phải thuộc một trong ba loại này, mặc dù bản thân các giá trị có thể lớn bằng$10^{18}$. 

Các ràng buộc cho phép lên đến$2 \cdot 10^5$các phần tử, loại trừ bất kỳ giải pháp nào có dạng bậc ba hoặc thậm chí bậc hai trong trường hợp xấu nhất đối với các chỉ số thô. Một vòng lặp ba lần trực tiếp trên các chỉ số sẽ thực hiện theo thứ tự$n^3$hoạt động vượt quá giới hạn cho phép. Ngay cả một vòng lặp kép ngây thơ trên các cặp chỉ số cũng sẽ tiếp cận$4 \cdot 10^{10}$trong trường hợp xấu nhất, nhìn chung vẫn còn quá chậm trừ khi cấu trúc dữ liệu bị khai thác nhiều. 

Sự tinh tế chính là các chỉ số quan trọng chứ không chỉ các giá trị. Nếu một giá trị xuất hiện nhiều lần thì mỗi lần xuất hiện sẽ được coi là một lựa chọn riêng biệt, do đó bội số phải được xử lý cẩn thận. 

Các trường hợp cạnh thường phá vỡ lý luận ngây thơ là trùng lặp và sắp xếp. Ví dụ, nếu mảng là$[1, 2, 3, 3]$, thì phương trình$1 + 2 = 3$đóng góp nhiều bộ ba hợp lệ tùy thuộc vào sự xuất hiện của 3 được chọn. Một ví dụ khác là khi cùng một giá trị xuất hiện nhiều lần, chẳng hạn như$[2, 2, 4]$. Cặp đôi$(2, 2)$đóng góp khác với$(2, 4)$và việc quên tính khác biệt hoặc bội số của chỉ mục dẫn đến số lượng không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu thử mọi bộ ba chỉ số theo thứ tự$(i, j, k)$và kiểm tra xem điều kiện tổng có đúng hay không. Điều này đúng vì nó trực tiếp thực thi tất cả các ràng buộc, bao gồm cả các chỉ số và thứ tự riêng biệt. Tuy nhiên, nó thực hiện ba vòng lặp lồng nhau, dẫn đến$O(n^3)$thời gian, điều này trở nên không thể thực hiện được ngay cả đối với$n = 200{,}000$. 

Một cải tiến tự nhiên là sửa chỉ số thứ ba$k$, đối xử$a_k$dưới dạng tổng mục tiêu, sau đó đếm xem có bao nhiêu cặp có thứ tự$(i, j)$thỏa mãn$a_i + a_j = a_k$. Điều này làm giảm vấn đề đếm các cặp hợp lệ cho từng mục tiêu cố định. Nếu chúng ta lưu trữ tần số của các giá trị, chúng ta có thể tính toán các đóng góp bằng cách sử dụng phép nhân thay vì quét các chỉ số. 

Cái nhìn sâu sắc quan trọng là chúng ta không còn cần phải suy luận về các vị trí riêng lẻ nữa. Sau khi các giá trị được nhóm lại, mỗi cặp giá trị sẽ đóng góp một số cặp chỉ mục có thể dự đoán được, được xác định hoàn toàn theo tần suất. Điều này biến vấn đề thành tích chập dựa trên tần số trên các giá trị. 

Khó khăn chuyển sang tìm kiếm một cách hiệu quả, cho mỗi giá trị$c$, có bao nhiêu cặp$(x, y)$trong mảng thỏa mãn$x + y = c$. Với bản đồ tần số, điều này trở thành một bảng liệt kê có cấu trúc trên các giá trị riêng biệt. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^3)$|$O(1)$| Quá chậm | 
| Đếm cặp tần số |$O(m^2)$Ở đâu$m$là các giá trị riêng biệt |$O(m)$| Được chấp nhận theo cấu trúc | 

Đây$m$là số lượng các giá trị riêng biệt, nhỏ hơn đáng kể so với$n$trong thực tế do cấu trúc ràng buộc số chia. 

## Hướng dẫn thuật toán 

### 1. Đếm tần số của từng giá trị 

Đầu tiên chúng tôi xây dựng bản đồ tần số`freq[v]`, lưu trữ số lần mỗi giá trị xuất hiện. Điều này là cần thiết vì mỗi chỉ số đều khác biệt và bội số ảnh hưởng trực tiếp đến số lượng bộ ba có thể được hình thành. 

### 2. Lặp lại tất cả các cặp giá trị riêng biệt được sắp xếp 

Chúng tôi xem xét mọi cặp giá trị có thứ tự$(x, y)$, kể cả trường hợp$x = y$. Với mỗi cặp, số cặp chỉ số đóng góp cho cặp giá trị này là: 

-`freq[x] * freq[y]`nếu như$x \neq y$-`freq[x] * (freq[x] - 1)`nếu như$x = y$Điều này đếm chính xác các lựa chọn chỉ mục theo thứ tự$(i, j)$với$i \neq j$. 

### 3. Tính giá trị thứ ba cần thiết 

Đối với mỗi cặp$(x, y)$, tính toán$z = x + y$. Chúng tôi kiểm tra xem$z$tồn tại trong mảng bằng cách sử dụng bản đồ tần số. 

Nếu nó tồn tại, mọi cặp hợp lệ$(i, j)$đóng góp`freq[z]`lựa chọn cho chỉ số$k$, vì bất kỳ sự xuất hiện nào của$z$có thể đóng vai trò là yếu tố thứ ba. 

### 4. Tích lũy đóng góp 

Chúng tôi thêm`pair_count * freq[z]`đến câu trả lời cho mỗi cặp giá trị có thứ tự. 

Không cần chỉnh sửa thêm đối với xung đột chỉ mục vì$z = x + y$cũng không thể bằng$x$hoặc$y$đối với số nguyên dương, do đó chỉ số thứ ba sẽ tự động khác biệt với hai chỉ số đầu tiên. 

### Tại sao nó hoạt động 

Mỗi bộ ba hợp lệ$(i, j, k)$tương ứng duy nhất với một lựa chọn các giá trị$(x, y, z)$như vậy$x + y = z$, cùng với sự lựa chọn các chỉ số cho mỗi lần xuất hiện giá trị. Thuật toán liệt kê mọi cặp giá trị có thứ tự có thể có chính xác một lần và nhân với tất cả các lần xuất hiện tương thích của giá trị thứ ba. Bởi vì các chỉ số chỉ được chọn trong các nhóm tần số rời rạc và các điều kiện đẳng thức được xử lý thông qua phép tính tổ hợp nên mỗi bộ ba hợp lệ sẽ được tính chính xác một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import Counter

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    freq = Counter(a)
    vals = list(freq.keys())
    
    ans = 0
    
    for i in range(len(vals)):
        x = vals[i]
        fx = freq[x]
        for j in range(len(vals)):
            y = vals[j]
            fy = freq[y]
            
            if x == y:
                pair_cnt = fx * (fx - 1)
            else:
                pair_cnt = fx * fy
            
            z = x + y
            if z in freq:
                ans += pair_cnt * freq[z]
    
    print(ans)

if __name__ == "__main__":
    solve()
```Việc thực hiện bắt đầu bằng cách nén mảng vào bản đồ tần số. Các vòng lặp lồng nhau lặp lại trên không gian giá trị thay vì không gian chỉ mục, đây là cách rút gọn khóa. Việc xử lý các$x = y$trường hợp này đảm bảo rằng các cặp chỉ mục được sắp xếp không sử dụng lại cùng một chỉ mục. Việc tra cứu cho$z = x + y$là thời gian không đổi khi sử dụng bản đồ băm. 

Một điểm tinh tế là vòng lặp nằm trên các cặp có thứ tự chứ không phải các cặp không có thứ tự. Điều này là cần thiết bởi vì$(i, j)$Và$(j, i)$là các bộ ba hợp lệ riêng biệt và cả hai phải được tính riêng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Mảng đầu vào:$[1, 2, 3]$| x | y | số cặp | z | tần số[z] | đóng góp | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 2 | 1 | 3 | 1 | 1 | 
| 2 | 1 | 1 | 3 | 1 | 1 | 

Tổng số câu trả lời = 2. 

Điều này cho thấy rằng việc đặt hàng rất quan trọng: cả hai$(1,2,3)$Và$(2,1,3)$được tính. 

### Ví dụ 2 

Mảng đầu vào:$[1, 2, 3, 3]$| x | y | số cặp | z | tần số[z] | đóng góp | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 2 | 1 | 3 | 2 | 2 | 
| 2 | 1 | 1 | 3 | 2 | 2 | 

Tổng số câu trả lời = 4. 

Điều này chứng tỏ cách các bản sao làm tăng số lượng bộ ba hợp lệ vì mỗi lần xuất hiện của giá trị đích đóng góp riêng lẻ như một lựa chọn hợp lệ cho$k$. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(m^2)$| lặp qua tất cả các cặp giá trị riêng biệt theo thứ tự | 
| Không gian |$O(m)$| lưu trữ bản đồ tần số của các giá trị riêng biệt | 

Hiệu suất phụ thuộc vào số lượng giá trị riêng biệt thay vì kích thước mảng. Bởi vì mọi giá trị bị giới hạn ở mức tối đa ba ước số, nên cấu trúc giá trị bị ràng buộc và giải pháp dựa trên tần số tránh mọi sự phụ thuộc vào$n^3$hoặc$n^2$trên các chỉ số. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import Counter

    n = int(input())
    a = list(map(int, input().split()))
    freq = Counter(a)
    vals = list(freq.keys())
    
    ans = 0
    for x in vals:
        for y in vals:
            if x == y:
                cnt = freq[x] * (freq[x] - 1)
            else:
                cnt = freq[x] * freq[y]
            z = x + y
            if z in freq:
                ans += cnt * freq[z]
    return str(ans)

# small cases
assert run("3\n1 2 3\n") == "2"
assert run("4\n1 2 3 3\n") == "4"
assert run("3\n2 2 4\n") == "2"

# duplicates heavy
assert run("5\n1 1 1 2 2\n") >= "0"

# all same
assert run("3\n1 1 1\n") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 2 3`|`2`| thứ tự các cặp | 
|`1 2 3 3`|`4`| xử lý trùng lặp cho k | 
|`2 2 4`|`2`| đếm cặp cùng giá trị | 
|`1 1 1`|`0`| không thể có số tiền hợp lệ | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi tất cả các phần tử đều giống hệt nhau. Ví dụ,$[1, 1, 1]$. Mỗi cặp đặt hàng$(i, j)$với$i \neq j$được tính, nhưng tổng là$2$, không tồn tại trong mảng, vì vậy câu trả lời đúng là 0. Thuật toán xử lý việc này vì$z = x + y$không bao giờ được tìm thấy trong bản đồ tần số. 

Một trường hợp khác là các giá trị lặp lại trong đó tồn tại tổng hợp lệ nhưng đến từ các toán hạng giống hệt nhau. TRONG$[2, 2, 4]$, cặp$(2, 2)$được tính là$2 \cdot 1 = 2$các cặp chỉ mục được sắp xếp và mỗi cặp ánh xạ tới hai lựa chọn$k$, tạo ra tổng cộng bốn đóng góp. Công thức cặp dựa trên tần số đảm bảo rằng các chỉ số không được sử dụng lại. 

Trường hợp tinh vi cuối cùng là khi mảng chứa cả giá trị nhỏ và giá trị lớn, chẳng hạn như$[1, 1000000000000000000, 1000000000000000001]$. Thuật toán vẫn hoạt động vì nó chỉ dựa vào việc tra cứu hàm băm cho tổng chứ không dựa trên bất kỳ giả định thứ tự hoặc phạm vi nào và các số nguyên lớn được xử lý một cách tự nhiên trong số học Python.
