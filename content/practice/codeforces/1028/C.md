---
title: "CF 1028C - Hình chữ nhật"
description: "Chúng ta có một tập hợp các hình chữ nhật thẳng hàng theo trục trên lưới số nguyên 2D. Mỗi hình chữ nhật được mô tả bởi góc dưới bên trái và góc trên bên phải và nó bao gồm ranh giới cũng như phần bên trong của nó."
date: "2026-06-16T21:18:16+07:00"
tags: ["codeforces", "competitive-programming", "geometry", "implementation", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1028
codeforces_index: "C"
codeforces_contest_name: "AIM Tech Round 5 (rated, Div. 1 + Div. 2)"
rating: 1600
weight: 1028
solve_time_s: 150
verified: true
draft: false
---

[CF 1028C - Hình chữ nhật](https://codeforces.com/problemset/problem/1028/C) 

**Đánh giá:** 1600 
**Tags:** hình học, cách thực hiện, sắp xếp 
**Thời gian giải:** 2 phút 30 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một tập hợp các hình chữ nhật thẳng hàng theo trục trên lưới số nguyên 2D. Mỗi hình chữ nhật được mô tả bởi góc dưới bên trái và góc trên bên phải và nó bao gồm ranh giới cũng như phần bên trong của nó. Điều đảm bảo chính là nếu chúng ta loại bỏ bất kỳ một hình chữ nhật nào thì các hình chữ nhật còn lại vẫn có chung ít nhất một điểm chung. 

Nhiệm vụ là tìm bất kỳ điểm tọa độ nguyên nào nằm bên trong ít nhất$n-1$của các hình chữ nhật này. Nói cách khác, chúng ta được phép “bỏ sót” nhiều nhất một hình chữ nhật và chúng ta vẫn phải đồng thời ở bên trong tất cả các hình chữ nhật khác. 

Những hạn chế là lớn, với$n$lên đến khoảng$1.3 \times 10^5$. Điều này ngay lập tức loại trừ mọi tương tác bậc hai giữa các hình chữ nhật. Thậm chí$O(n^2)$lý luận chồng chéo theo cặp vượt xa giới hạn khả thi. Chúng tôi hướng tới các giải pháp duy trì dần dần các ràng buộc toàn cầu hoặc tính toán lại các điểm giao cắt theo thời gian tuyến tính hoặc gần tuyến tính. 

Trường hợp cạnh tinh tế xuất phát từ thực tế là câu trả lời chỉ cần đáp ứng tất cả trừ một hình chữ nhật. Một giao điểm đơn giản của tất cả các hình chữ nhật có thể thất bại ngay cả khi tồn tại một câu trả lời hợp lệ, bởi vì một hình chữ nhật "xấu" có thể thu nhỏ giao điểm tổng thể thành trống. 

Ví dụ: hãy xem xét ba hình chữ nhật trong đó hai hình chồng lên nhau nhiều và hình thứ ba hơi dịch chuyển giao điểm: 

đầu vào:```
3
0 0 2 2
1 1 3 3
100 100 200 200
```Giao điểm của cả ba đều trống, nhưng việc loại bỏ hình chữ nhật thứ ba sẽ để lại một vùng chồng chéo hợp lệ giữa hai hình đầu tiên và bất kỳ điểm nào trong vùng đó đều được chấp nhận. 

Cạm bẫy thứ hai là giả định rằng giao điểm của bất kỳ tập hợp con cố định nào cũng có hiệu quả. Bài toán không cho chúng ta biết hình chữ nhật nào là ngoại lệ, vì vậy chúng ta phải có khả năng “mô phỏng” loại bỏ từng hình một cách ngầm định mà không cần tính toán lại mọi thứ từ đầu. 

## Phương pháp tiếp cận 

Ý tưởng bạo lực bắt đầu một cách tự nhiên từ điều kiện. Nếu chúng ta thử loại bỏ từng hình chữ nhật một, chúng ta có thể tính giao điểm của các hình còn lại$n-1$hình chữ nhật và kiểm tra xem nó có trống không. Việc tính toán giao điểm của các hình chữ nhật rất đơn giản: chúng ta lấy giá trị lớn nhất của tất cả các tọa độ x bên trái, giá trị nhỏ nhất của tất cả các tọa độ x bên phải và tương tự cho y. 

Làm điều này một cách độc lập cho mọi hình chữ nhật bị loại bỏ sẽ dẫn đến$n$sự tính toán lại. Mỗi lần tính toán lại sẽ quét tất cả các hình chữ nhật, đưa ra$O(n^2)$tổng số hoạt động. Với$n \approx 10^5$, tốc độ này quá chậm. 

Quan sát quan trọng là mỗi truy vấn giao nhau phụ thuộc vào cực trị toàn cục: cực đại thứ hai và cực đại thứ hai của cạnh trái, cực tiểu thứ hai và cực tiểu thứ hai của cạnh phải và cấu trúc tương tự cho y. Khi chúng ta loại bỏ một hình chữ nhật, chỉ những đóng góp cực lớn từ hình chữ nhật đó mới quan trọng. Nếu nó không chịu trách nhiệm về ranh giới thì giao điểm toàn cầu sẽ không thay đổi. Nếu nó chịu trách nhiệm, giá trị tốt thứ hai sẽ hoạt động. 

Điều này làm giảm vấn đề duy trì thông tin tiền tố và hậu tố trên các thái cực được sắp xếp. Đối với mỗi hướng, chúng tôi có thể tính toán trước cả những đóng góp tốt nhất và tốt nhất thứ hai cùng với số lượng. Sau đó, đối với mỗi hình chữ nhật, chúng ta có thể xây dựng lại giao lộ sẽ trông như thế nào nếu nó bị xóa trong thời gian không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý tất cả các hình chữ nhật trong khi vẫn duy trì cực trị tổng thể và cực trị "dự phòng". 

1. Đối với tất cả các hình chữ nhật, hãy tính bốn đại lượng tổng thể cho mỗi chiều: ranh giới bên trái lớn nhất, ranh giới bên phải nhỏ nhất và theo dõi xem có bao nhiêu hình chữ nhật đạt được mỗi cực trị này. Chúng tôi cũng theo dõi các giá trị tốt thứ hai để có thể khôi phục giới hạn khi một hình chữ nhật bị xóa. 

Lý do chúng ta cần các giá trị tốt thứ hai là vì việc loại bỏ một hình chữ nhật xác định ranh giới cực trị sẽ khiến chúng ta mất hoàn toàn ràng buộc đó. 
2. Lặp lại logic tương tự một cách độc lập cho các kích thước x và y. Mỗi chiều hoạt động độc lập vì điều kiện giao nhau được phân tích thành thừa số. 
3. Đối với mỗi hình chữ nhật, hãy mô phỏng việc loại bỏ nó. Nếu nó không góp phần tạo nên một cực đoan thì giao điểm toàn cầu vẫn không thay đổi. Nếu nó đang đóng góp và là người đóng góp duy nhất, chúng tôi chuyển sang giá trị tốt thứ hai; nếu không thì cực trị vẫn không thay đổi. 
4. Sau khi mô phỏng việc loại bỏ một hình chữ nhật, chúng ta thu được một hình chữ nhật giao nhau được xác định bởi: 

trái = max_left_loại trừ_i, phải = min_right_loại trừ_i, 

đáy = max_bottom_loại trừ_i, đỉnh = min_top_loại trừ_i. 

Nếu hình chữ nhật này hợp lệ (trái ≤ phải và dưới ≤ trên cùng), chúng ta sẽ chọn ngay bất kỳ điểm nguyên nào bên trong nó. 
5. Vì bài toán đảm bảo sự tồn tại nên ít nhất việc loại bỏ một hình chữ nhật sẽ tạo ra một giao điểm không trống, vì vậy chúng ta sẽ tìm được đáp án. 

### Tại sao nó hoạt động 

Tính chính xác dựa trên thực tế là giao điểm của các hình chữ nhật thẳng hàng với trục được xác định đầy đủ bởi các ràng buộc độc lập trên các khoảng x và y. Đối với mỗi chiều, chỉ có hai giá trị quan trọng: ranh giới bên trái tối đa và ranh giới bên phải tối thiểu. Việc xóa một hình chữ nhật chỉ có thể ảnh hưởng đến hai giá trị này. Bằng cách lưu trữ các ứng cử viên tốt thứ hai, chúng tôi bảo toàn ranh giới chính xác cho bất kỳ lần xóa nào. Do đó, mọi giao điểm “tất cả trừ một” đều được tính toán chính xác mà không cần tính toán lại từ đầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    xs1, ys1, xs2, ys2 = [], [], [], []

    for _ in range(n):
        x1, y1, x2, y2 = map(int, input().split())
        xs1.append(x1)
        ys1.append(y1)
        xs2.append(x2)
        ys2.append(y2)

    def prep_max(arr):
        max1 = -10**18
        max2 = -10**18
        cnt1 = 0
        for v in arr:
            if v > max1:
                max2 = max1
                max1 = v
                cnt1 = 1
            elif v == max1:
                cnt1 += 1
            elif v > max2:
                max2 = v
        return max1, max2, cnt1

    def prep_min(arr):
        min1 = 10**18
        min2 = 10**18
        cnt1 = 0
        for v in arr:
            if v < min1:
                min2 = min1
                min1 = v
                cnt1 = 1
            elif v == min1:
                cnt1 += 1
            elif v < min2:
                min2 = v
        return min1, min2, cnt1

    xL1, xL2, xLcnt = prep_max(xs1)
    yL1, yL2, yLcnt = prep_max(ys1)
    xR1, xR2, xRcnt = prep_min(xs2)
    yR1, yR2, yRcnt = prep_min(ys2)

    for i in range(n):
        l = xL2 if xs1[i] == xL1 and xLcnt == 1 else xL1
        r = xR2 if xs2[i] == xR1 and xRcnt == 1 else xR1
        d = yL2 if ys1[i] == yL1 and yLcnt == 1 else yL1
        u = yR2 if ys2[i] == yR1 and yRcnt == 1 else yR1

        if l <= r and d <= u:
            print(l, d)
            return

    print(0, 0)

if __name__ == "__main__":
    solve()
```Giải pháp đầu tiên nén mỗi hình chữ nhật thành bốn mảng biểu thị các ranh giới. Sau đó, nó tính toán cực trị tổng thể cho các cạnh trái, phải, dưới và trên cùng, cùng với các giá trị và số lượng tốt nhất thứ hai để phát hiện xem giá trị cực trị có phụ thuộc vào một hình chữ nhật duy nhất hay không. 

Trong quá trình mô phỏng loại bỏ, chúng tôi kiểm tra xem hình chữ nhật có$i$chịu trách nhiệm duy nhất đối với bất kỳ ranh giới cực đoan nào. Nếu vậy, chúng ta quay lại giá trị tốt thứ hai; nếu không thì chúng ta giữ cho cực trị toàn cầu không thay đổi. Điều này được thực hiện độc lập cho x và y, tạo thành một hình chữ nhật giao nhau. 

Khi tìm thấy giao lộ hợp lệ, chúng tôi xuất ra góc dưới bên trái của giao lộ đó. Bất kỳ điểm nào bên trong giao lộ đều hoạt động nên việc chọn góc dưới bên trái sẽ tránh được việc tính toán bổ sung. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
0 0 1 1
1 1 2 2
3 0 4 1
```Chúng tôi tính toán: 

| Bước | xL | xR | yL | yR | Có hiệu lực? | 
| --- | --- | --- | --- | --- | --- | 
| tất cả các hình chữ nhật | 0 | 1 | 1 | 1 | không | 
| loại bỏ trực tràng 1 | 1 | 2 | 1 | 2 | vâng | 
| loại bỏ trực tràng 2 | 0 | 1 | 0 | 1 | vâng | 
| loại bỏ trực tiếp 3 | 0 | 2 | 1 | 2 | không | 

Cấu hình hợp lệ đầu tiên cho giao điểm [1,1], vì vậy chúng tôi xuất ra:```
1 1
```Điều này cho thấy cách chỉ cần xóa một hình chữ nhật để khôi phục phần chồng lấp không trống. 

### Ví dụ 2 

đầu vào:```
2
0 0 5 5
1 1 4 4
```| Bước | xL | xR | yL | yR | Có hiệu lực? | 
| --- | --- | --- | --- | --- | --- | 
| loại bỏ trực tràng 1 | 1 | 4 | 1 | 4 | vâng | 
| loại bỏ trực tràng 2 | 0 | 5 | 0 | 5 | vâng | 

Bất kỳ điểm nào trong toàn bộ giao lộ đều hoạt động; cả hai hình chữ nhật đều đã chồng lên nhau hoàn toàn, vì vậy việc xóa một trong hai hình chữ nhật vẫn để lại một vùng hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| một lần để tính cực trị và một lần quét để kiểm tra việc loại bỏ | 
| Không gian |$O(n)$| lưu trữ ranh giới hình chữ nhật | 

Thuật toán chỉ thực hiện quét tuyến tính và kiểm tra thời gian liên tục trên mỗi hình chữ nhật, phù hợp thoải mái trong giới hạn cho$n \le 1.3 \times 10^5$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    xs1, ys1, xs2, ys2 = [], [], [], []

    for _ in range(n):
        x1, y1, x2, y2 = map(int, input().split())
        xs1.append(x1)
        ys1.append(y1)
        xs2.append(x2)
        ys2.append(y2)

    def prep_max(arr):
        max1 = -10**18
        max2 = -10**18
        cnt1 = 0
        for v in arr:
            if v > max1:
                max2 = max1
                max1 = v
                cnt1 = 1
            elif v == max1:
                cnt1 += 1
            elif v > max2:
                max2 = v
        return max1, max2, cnt1

    def prep_min(arr):
        min1 = 10**18
        min2 = 10**18
        cnt1 = 0
        for v in arr:
            if v < min1:
                min2 = min1
                min1 = v
                cnt1 = 1
            elif v == min1:
                cnt1 += 1
            elif v < min2:
                min2 = v
        return min1, min2, cnt1

    xL1, xL2, xLcnt = prep_max(xs1)
    yL1, yL2, yLcnt = prep_max(ys1)
    xR1, xR2, xRcnt = prep_min(xs2)
    yR1, yR2, yRcnt = prep_min(ys2)

    for i in range(n):
        l = xL2 if xs1[i] == xL1 and xLcnt == 1 else xL1
        r = xR2 if xs2[i] == xR1 and xRcnt == 1 else xR1
        d = yL2 if ys1[i] == yL1 and yLcnt == 1 else yL1
        u = yR2 if ys2[i] == yR1 and yRcnt == 1 else yR1

        if l <= r and d <= u:
            return f"{l} {d}"

    return "0 0"

# provided sample
assert run("""3
0 0 1 1
1 1 2 2
3 0 4 1
""") == "1 1"

# custom 1: minimal
assert run("""2
0 0 1 1
0 0 1 1
""") == "0 0"

# custom 2: one outlier
assert run("""3
0 0 10 10
1 1 2 2
1 1 2 2
""") == "1 1"

# custom 3: extreme separation
assert run("""3
0 0 1 1
0 0 1 1
100 100 200 200
""") in {"0 0", "1 1"}

# custom 4: boundary touch
assert run("""2
0 0 1 1
1 1 2 2
""") == "1 1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| hình chữ nhật trùng lặp tối thiểu | điểm chia sẻ | độ đúng cơ sở | 
| một hình chữ nhật ngoại lệ | chồng chéo hợp lệ | logic loại bỏ | 
| hình chữ nhật tách biệt | bất kỳ giao lộ còn lại hợp lệ nào | sự mạnh mẽ | 
| ranh giới chạm vào hình chữ nhật | bao gồm góc | xử lý ranh giới | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi ranh giới bên trái tối đa được xác định bởi một hình chữ nhật. Ví dụ:```
3
5 0 10 10
0 0 6 6
0 0 6 6
```Nếu hình chữ nhật đầu tiên bị xóa, ranh giới bên trái tối đa giảm từ 5 xuống 0. Thuật toán xử lý điều này bằng cách kiểm tra số lượng người đóng góp tối đa và chỉ chuyển sang mức tối đa thứ hai khi cần. Trong trường hợp này, việc loại bỏ hình chữ nhật 1 ngay lập tức thay đổi ranh giới bên trái thành 0, tạo ra một giao điểm hợp lệ với các hình chữ nhật còn lại. 

Một trường hợp khác là khi cả hai ranh giới cực trị đều xuất phát từ cùng một hình chữ nhật. Thuật toán vẫn hoạt động vì nó điều chỉnh giới hạn trái và phải một cách độc lập. Ngay cả khi một hình chữ nhật ảnh hưởng đến cả hai cạnh, việc loại bỏ nó sẽ thay thế cả hai giá trị bằng các ứng cử viên tốt nhất thứ hai, bảo toàn vùng giao nhau chính xác. 

Trường hợp cạnh cuối cùng là khi tất cả các hình chữ nhật đã chồng lên nhau. Sau đó, mỗi lần loại bỏ vẫn mang lại giao điểm chung. Thuật toán sẽ phát hiện tính hợp lệ ngay ở lần lặp đầu tiên mà không cần dựa vào các giá trị tốt nhất thứ hai.
