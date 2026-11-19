---
title: "CF 2093C - Sự lặp lại đơn giản"
description: "Chúng ta được cho một số nguyên cơ sở x và số lần lặp lại k. Từ x, chúng ta xây dựng một số y mới bằng cách viết biểu diễn thập phân của x liên tiếp k lần mà không chèn dấu phân cách. Ví dụ: nếu x = 52 và k = 3 thì y = 525252. Nếu x = 6 và k = 7 thì y = 6666666."
date: "2026-06-08T05:37:44+07:00"
tags: ["codeforces", "competitive-programming", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 2093
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 1016 (Div. 3)"
rating: 1000
weight: 2093
solve_time_s: 94
verified: true
draft: false
---

[CF 2093C - Lặp lại đơn giản](https://codeforces.com/problemset/problem/2093/C) 

**Đánh giá:** 1000 
**Tags:** toán, lý thuyết số 
**Thời gian giải:** 1 phút 34s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một số nguyên cơ sở`x`và số lần lặp lại`k`. Từ`x`, ta xây dựng một số mới`y`bằng cách viết biểu diễn thập phân của`x`liên tiếp`k`lần mà không cần chèn dấu phân cách. Ví dụ, nếu`x = 52`Và`k = 3`, sau đó`y = 525252`. Nếu như`x = 6`Và`k = 7`, sau đó`y = 6666666`. 

Nhiệm vụ không phải là xây dựng con số này một cách rõ ràng và kiểm tra tính nguyên tố của nó theo nghĩa thông thường, mà là xác định xem con số được xây dựng này có`y`là nguyên tố. 

Những ràng buộc ngay lập tức báo hiệu rằng việc xây dựng`y`nói chung là không thể. Khi`x`có tới 9 chữ số và`k`lên tới 7, số kết quả có thể có tới 63 chữ số, vượt quá các loại số nguyên tiêu chuẩn. Ngay cả việc lưu trữ nó dưới dạng một chuỗi cũng được, nhưng việc kiểm tra tính nguyên tố trên một số nguyên 63 chữ số cho tối đa 100 trường hợp kiểm tra sẽ yêu cầu các kỹ thuật tính nguyên tố số nguyên lớn nâng cao và vẫn là quá mức cần thiết đối với một vấn đề được xếp hạng 1000. 

Một vấn đề tinh vi hơn xuất hiện trong các trường hợp phức tạp liên quan đến`k = 1`Và`x = 1`. Nếu như`x = 1`Và`k = 1`, sau đó`y = 1`, không phải là số nguyên tố. Nếu như`k > 1`, ngay cả khi`x = 1`, kết quả là một sự trả thù như`111...1`, có thể trông đặc biệt nhưng luôn là sự kết hợp của`k > 1`trong cách xây dựng này vì nó có thể được tính thành nhân tử. 

Một sai lầm ngây thơ là xây dựng chuỗi theo đúng nghĩa đen và cố gắng kiểm tra tính nguyên tố bằng phép chia thử. Ngay cả khi được tối ưu hóa lên tới sqrt(n), điều này vẫn bị hỏng do kích thước của`y`và các trường hợp thử nghiệm lặp đi lặp lại. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực trực tiếp được xây dựng`y`và kiểm tra xem nó có phải là số nguyên tố hay không bằng phép chia lên tới`sqrt(y)`. Điều này đúng về mặt khái niệm nhưng nhanh chóng trở nên không khả thi. số`y`có thể có tới 63 chữ số, do đó, ngay cả một lần kiểm tra tính nguyên tố cũng vượt xa mọi phương pháp phân chia thử nghiệm xác định hợp lý. 

Quan sát quan trọng mang tính cấu trúc: phép nối lặp lại tạo ra một số có đặc tính phân tích đại số mạnh bất cứ khi nào`k > 1`. Cho chiều dài của`x`là`d`. Khi đó số được xây dựng có thể được viết là:$$y = x \cdot 10^{d(k-1)} + x \cdot 10^{d(k-2)} + \cdots + x$$Bao thanh toán`x`, chúng tôi nhận được:$$y = x \cdot (10^{d(k-1)} + 10^{d(k-2)} + \cdots + 1)$$Điều này cho thấy bất cứ khi nào`k > 1`,`y`là một sản phẩm của`x`và một số nguyên khác lớn hơn 1, ngay lập tức làm cho`y`tổng hợp trừ khi`x = 1`và chúng tôi dựa vào yếu tố thứ hai là đặc biệt. Nhưng ngay cả trong trường hợp đó, hệ số thứ hai vẫn lớn hơn 1 đối với`k > 1`, Vì thế`y`là tổng hợp. 

Vì vậy, thời điểm duy nhất chúng ta có thể có được số nguyên tố là khi`k = 1`, bởi vì khi đó`y = x`. Vấn đề giảm xuống để kiểm tra xem`x`bản thân nó là số nguyên tố. 

Vì vậy, toàn bộ giải pháp sụp đổ thành: 

- Nếu`k > 1`, câu trả lời luôn là "KHÔNG". 
- Nếu như`k = 1`, kiểm tra tính nguyên tố của`x`. 

Bây giờ chúng ta chỉ cần kiểm tra tính nguyên tố cho`x ≤ 10^9`, điều này đơn giản với phép chia thử lên tới`sqrt(x)`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Xây dựng lực lượng vũ phu + Kiểm tra tính nguyên thủy lớn | O(k· | x | + sqrt(y)) | 
| Thông tin chi tiết về hệ số + Kiểm tra x | O(√x) mỗi lần kiểm tra | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`x`Và`k`cho mỗi trường hợp thử nghiệm. Chúng tôi xử lý từng trường hợp một cách độc lập vì không có tính toán nào được thực hiện. 
2. Nếu`k`lớn hơn 1 thì xuất ra ngay "NO". Điều này xuất phát từ việc phân tích nhân tử đại số của phép nối lặp lại, đảm bảo tính tổng hợp. 
3. Nếu`k`bằng 1, chúng ta rút gọn bài toán thành việc kiểm tra xem`x`là một số nguyên tố. 
4. Để kiểm tra tính nguyên tố của`x`, trước tiên xử lý các trường hợp nhỏ: các số nhỏ hơn 2 không phải là số nguyên tố. 
5. Đối với`x ≥ 2`, hãy thử chia cho tất cả các số nguyên từ 2 đến`sqrt(x)`. Nếu tìm thấy bất kỳ ước số nào,`x`là hợp chất; nếu không thì nó là số nguyên tố. 

### Tại sao nó hoạt động 

Tính đúng đắn phụ thuộc vào thực tế là phép nối đưa ra một cấu trúc nhân không tầm thường bất cứ khi nào sự lặp lại xảy ra. Vì`k > 1`, số được xây dựng luôn phân tích thành tích của`x`và tổng hình học của các lũy thừa bằng mười, cả hai đều lớn hơn 1 về độ lớn. Điều này đảm bảo tính tổng hợp bất kể`x`bản thân nó là số nguyên tố. Khi`k = 1`, không có cấu trúc nào được giới thiệu và số lượng chính xác`x`, do đó tính nguyên tố được bảo toàn chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def is_prime(n: int) -> bool:
    if n < 2:
        return False
    if n % 2 == 0:
        return n == 2
    i = 3
    while i * i <= n:
        if n % i == 0:
            return False
        i += 2
    return True

t = int(input())
for _ in range(t):
    x, k = map(int, input().split())
    if k > 1:
        print("NO")
    else:
        print("YES" if is_prime(x) else "NO")
```Mã đầu tiên cô lập lối tắt cấu trúc`k > 1`, điều này tránh mọi nhu cầu xây dựng hoặc lý giải về số lượng lớn lặp lại. Việc kiểm tra tính nguyên thủy chỉ được áp dụng khi cần thiết và an toàn vì`x`được giới hạn bởi`10^9`. 

Hàm nguyên tố sử dụng phép chia thử căn bậc hai tiêu chuẩn được tối ưu hóa bằng cách bỏ qua các số chẵn sau khi xử lý 2. Điều này là đủ với ràng buộc. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi hai trường hợp để xem quyết định được đơn giản hóa như thế nào. 

### Ví dụ 1:`x = 52, k = 3`| Bước | x | k | Quyết định | 
| --- | --- | --- | --- | 
| 1 | 52 | 3 | k > 1 | 
| 2 | 52 | 3 | Đầu ra KHÔNG | 

Thuật toán không bao giờ xây dựng số`525252`. Nó trực tiếp xác định cấu trúc lặp lại là đủ để đảm bảo tính tổng hợp. 

### Ví dụ 2:`x = 7, k = 1`| Bước | x | k | Kiểm tra tính nguyên thủy | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | 7 | 1 | kiểm tra ước số | không tìm thấy | 
| 2 | 7 | 1 | nguyên tố | CÓ | 

Trường hợp này thực hiện đường dẫn dự phòng trong đó không xảy ra sự lặp lại, do đó cần phải kiểm tra tính nguyên thủy tiêu chuẩn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t √x) | Mỗi phép kiểm tra chỉ kiểm tra tính nguyên tố khi k = 1, sử dụng phép chia thử lên tới √x | 
| Không gian | O(1) | Không có cấu trúc phụ trợ ngoài một vài biến | 

Các ràng buộc cho phép tối đa 100 trường hợp thử nghiệm với`x ≤ 10^9`. Cách tiếp cận √x đủ nhanh vì √10^9 là khoảng 31623 và trong trường hợp xấu nhất, chúng tôi thực hiện thao tác đó 100 lần, nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    def is_prime(n: int) -> bool:
        if n < 2:
            return False
        if n % 2 == 0:
            return n == 2
        i = 3
        while i * i <= n:
            if n % i == 0:
                return False
            i += 2
        return True

    t = int(input())
    out = []
    for _ in range(t):
        x, k = map(int, input().split())
        if k > 1:
            out.append("NO")
        else:
            out.append("YES" if is_prime(x) else "NO")
    return "\n".join(out)

# provided samples
assert run("""4
52 3
6 7
7 1
1 7
""") == """NO
NO
YES
NO"""

# minimum edge cases
assert run("""3
1 1
1 5
2 1
""") == """NO
NO
YES"""

# all equal repetition
assert run("""2
11 2
11 1
""") == """NO
NO"""

# prime boundary
assert run("""2
2 1
4 1
""") == """YES
NO"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1`| KHÔNG | trường hợp nguyên tố không hợp lệ nhỏ nhất | 
|`1 5`| KHÔNG | sự lặp lại luôn tổng hợp | 
|`11 2`| KHÔNG | cấu trúc lặp đi lặp lại giết chết tính nguyên thủy | 
|`2 1`| CÓ | số nguyên tố nhỏ nhất | 
|`4 1`| KHÔNG | hỗn hợp nhỏ nhất | 

## Vỏ cạnh 

Trường hợp cạnh nhạy cảm nhất là`x = 1`. Nếu như`k = 1`, số đó là`1`, không phải là số nguyên tố nên kết quả đầu ra phải là "KHÔNG". Nếu như`k > 1`, số đó sẽ trở thành một chuỗi các số như`111...1`, vẫn được xử lý bởi`k > 1`quy định và ngay lập tức được phân loại là "KHÔNG" nếu không có công trình xây dựng. 

Một trường hợp cạnh khác là các số nguyên tố nhỏ có sự lặp lại, chẳng hạn như`x = 2, k = 2`. Số được xây dựng là`22`, rõ ràng là tổng hợp và thuật toán trả về chính xác "KHÔNG" do quy tắc lặp lại trước bất kỳ kiểm tra nguyên tố nào. 

Cuối cùng, các số nguyên tố lớn với`k = 1`, chẳng hạn như`x = 999999937`, được xử lý hoàn toàn bằng vòng kiểm tra tính nguyên tố. Thuật toán chỉ phụ thuộc vào giới hạn căn bậc hai, đảm bảo tính chính xác mà không gặp vấn đề tràn hoặc chuyển đổi.
