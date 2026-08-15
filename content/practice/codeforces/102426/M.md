---
title: "CF 102426M - \u957f\u5b89\u8857\u7684\u534e\u706f"
description: "Chúng ta có (N) vùng chiếu sáng hình tròn giống hệt nhau. Tâm của chúng nằm trên một đường thẳng, các tâm liên tiếp cách nhau đúng (L) đơn vị và mọi đường tròn đều có bán kính (R). Nhiệm vụ là tính diện tích được bao phủ bởi ít nhất một nguồn sáng."
date: "2026-08-12T19:44:08+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102426
codeforces_index: "M"
codeforces_contest_name: "The 7-th BIT Campus Programming Contest for Junior Grade Group"
rating: 0
weight: 102426
solve_time_s: 72
verified: true
draft: false
---

[CF 102426M - \u957f\u5b89\u8857\u7684\u534e\u706f](https://codeforces.com/problemset/problem/102426/M) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 12s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có (N) vùng chiếu sáng hình tròn giống hệt nhau. Tâm của chúng nằm trên một đường thẳng, các tâm liên tiếp cách nhau đúng (L) đơn vị và mọi đường tròn đều có bán kính (R). Nhiệm vụ là tính diện tích được bao phủ bởi ít nhất một nguồn sáng. 

Các vòng tròn được sắp xếp theo chuỗi một chiều, đây là thuộc tính cấu trúc quan trọng. Mặc dù các vùng được chiếu sáng là hai chiều, nhưng tâm của chúng chỉ có một bậc tự do, do đó mô hình chồng lấp đơn giản hơn nhiều so với các vòng tròn tùy ý. 

Có thể có tối đa (10^5) trường hợp thử nghiệm, trong khi (N), (R) và (L) mỗi trường hợp có thể lớn bằng (10^9). Một thuật toán lặp lại các đèn (N) cho mọi trường hợp thử nghiệm có thể yêu cầu (10^{14}) thao tác trong trường hợp xấu nhất, vượt xa giới hạn thời gian. Chúng ta cần một lượng số học không đổi cho mỗi trường hợp thử nghiệm. Các giá trị lớn của (R) và (L) cũng có nghĩa là các biểu thức hình học trung gian phải được đánh giá cẩn thận bằng số học dấu phẩy động, mặc dù bản thân các số nguyên của Python không bị tràn. 

Một số trường hợp ranh giới rất dễ bị xử lý sai. Ví dụ: nếu không có đèn (N=0,R=10,L=3), vùng được chiếu sáng chính xác là (0), bất kể các thông số khác. Công thức bắt đầu bằng (N\pi R^2) xử lý việc này một cách tự nhiên, nhưng mã giả định ít nhất một vòng tròn có thể bị lỗi. 

Ví dụ: nếu (N=1), (N=1,R=2,L=100), chỉ có một đường tròn, do đó câu trả lời là (4\pi). Khoảng cách giữa các đèn liên tiếp không liên quan vì không có cặp đèn liên tiếp. 

Nếu (L\ge 2R), các đường tròn lân cận không trùng nhau. Ví dụ: (N=2,R=1,L=2) có diện tích là (2\pi). Việc sử dụng công thức chồng chéo mà không xử lý ranh giới một cách cẩn thận có thể đưa ra một đối số căn bậc hai âm nhỏ do số học dấu phẩy động hoặc đếm không chính xác giao điểm có diện tích bằng 0. 

Ở thái cực khác, (L=0) có nghĩa là mọi đèn đều ở cùng một vị trí. Ví dụ: (N=5,R=3,L=0) có diện tích (9\pi), không phải (45\pi). Một công thức chồng lấp đúng phải nhận ra rằng giao điểm của hai đường tròn trùng nhau là toàn bộ đường tròn. 

Cuối cùng, (R=0) làm cho mọi vùng được chiếu sáng trở thành một điểm có diện tích bằng 0. Ví dụ: (N=10,R=0,L=7) có câu trả lời (0). Trường hợp này nên được xử lý trước khi đánh giá một biểu thức chẳng hạn như (L/(2R)), vì nếu không thì phép chia cho 0 sẽ xảy ra. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ xử lý đèn từ trái sang phải và duy trì rõ ràng phần nào của vòng tròn mới đã được chiếu sáng. Về mặt hình học, điều này có thể được thực hiện bằng cách tính toán các giao điểm với các vòng tròn đã được đặt trước đó. Phương pháp này đúng vì có thể thu được diện tích hợp bằng cách thêm phần của mỗi vòng tròn mới chưa được che phủ trước đó. 

Vấn đề là có thể có (N) vòng kết nối và việc triển khai đơn giản có thể so sánh từng vòng kết nối mới với tất cả các vòng kết nối trước đó. Sau đó, một trường hợp thử nghiệm có thể yêu cầu kiểm tra cặp (\Theta(N^2)), đó là các hoạt động (10^{18}) khi (N=10^9), mặc dù bản thân đầu vào giới hạn số lượng trường hợp thử nghiệm thay vì thực hiện việc lặp lại như vậy trong thực tế. Ngay cả thuật toán (O(N)) cho mỗi trường hợp cũng quá đắt khi (10^5) trường hợp chứa (N) lớn. 

Quan sát hữu ích là chúng ta không thực sự cần so sánh một vòng tròn với tất cả các vòng tròn trước đó. Xét ba tâm liên tiếp ở các vị trí (0,L,2L). Lấy một điểm thuộc cả vòng tròn thứ nhất và thứ ba. Vì tâm ở giữa gần với mọi điểm nằm giữa hai tâm ngoài nên điểm đó cũng thuộc đường tròn ở giữa. Tổng quát hơn, khi các vòng tròn được thêm từ trái sang phải, mọi điểm của vòng tròn mới đã được bao phủ bởi bất kỳ vòng tròn nào trước đó cũng được bao phủ bởi vòng tròn ngay trước đó.

Do đó, vòng tròn mới chồng lên toàn bộ liên kết trước đó trong cùng một khu vực giống như nó chồng lên vòng tròn trước đó. Điều này vẫn đúng ngay cả khi nhiều vòng tròn chồng lên nhau cùng một lúc. Sự trùng lặp bậc ba và bậc cao hơn không cần các thuật ngữ loại trừ bao gồm riêng biệt vì chúng đã được tính khi mỗi vòng kết nối mới được thêm vào. 

Như vậy vùng đoàn có dạng rất đơn giản. Bắt đầu với (N) diện tích vòng tròn riêng lẻ, (N\pi R^2) và trừ đi số lần trùng lặp của cặp liền kề (N-1). Khi (L\ge 2R), không có sự trùng lặp và không cần phải trừ đi gì. 

Đối với hai đường tròn bằng nhau có bán kính (R), có tâm cách nhau (L), diện tích phần giao nhau của chúng là (0\le L<2R) là 

## 2R^2\arccos\left(\frac{L}{2R}\right) 

\frac{L}{2}\sqrt{4R^2-L^2}. 
] 

Do đó, với (N>0), 

[ 
S= 
N\pi R^2-(N-1)S_{\text{overlap}}, 
] 

với (S_{\text{overlap}}=0) khi (L\ge 2R). 

Phần quan trọng không chỉ đơn thuần là nhận biết công thức giao đường tròn tiêu chuẩn. Sự đơn giản hóa thực sự là chứng minh rằng mọi vòng tròn mới giao với toàn bộ liên minh trước đó chỉ trong cùng một khu vực mà nó chia sẻ với vòng tròn trước đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(N^2)) cho mỗi trường hợp thử nghiệm | (O(N)) | Quá chậm | 
| Tối ưu | (O(1)) cho mỗi trường hợp thử nghiệm | (O(1)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc (N), (R) và (L). Nếu (N=0) hoặc (R=0), xuất ra (0), vì không có vùng được chiếu sáng vùng dương. 
2. Tính diện tích của một hình tròn là (\pi R^2), sau đó khởi tạo tổng diện tích là (N\pi R^2). Điều này đại diện cho khu vực trước khi loại bỏ sự chồng chéo. 
3. Kiểm tra xem (L<2R). Nếu không, các vòng tròn liên tiếp sẽ rời nhau hoặc tiếp tuyến, do đó diện tích chồng lấp bằng 0 và tổng ban đầu đã là câu trả lời. 
4. Khi (L<2R), hãy tính diện tích giao điểm của hai đường tròn lân cận với 

\frac{L}{2}\sqrt{4R^2-L^2}. 
] 

Số hạng đầu tiên bao gồm hai cung tròn, trong khi số hạng căn bậc hai loại bỏ hai phần hình tam giác giữa dây cung và tâm. 

1. Trừ phần chồng chéo này chính xác (N-1) lần. Có (N-1) cặp liền kề và việc thêm một vòng tròn mới sẽ làm tăng sự kết hợp bằng toàn bộ diện tích của nó trừ đi chính xác phần trùng lặp của nó với vòng tròn trước đó. 
2. In giá trị dấu phẩy động thu được với đủ chữ số, ví dụ sử dụng`:.10f`. Mười chữ số sau dấu thập phân mang lại độ chính xác cao hơn đáng kể so với yêu cầu (10^{-6}). 

Tại sao nó hoạt động: khi vòng tròn thứ (i) được thêm vào, giả sử một điểm trong đó cũng thuộc về vòng tròn trước đó. Tâm của vòng tròn ngay trước đó nằm giữa tâm của vòng tròn mới và tâm trước đó. Đối với bất kỳ điểm nào bên trong cả hai vòng tròn điểm cuối, khoảng cách của nó đến tâm giữa không lớn hơn khoảng cách của nó đến tâm điểm cuối xa hơn. Do đó điểm cũng thuộc đường tròn trước đó. Do đó, giao điểm giữa vòng tròn mới và toàn bộ liên kết hiện có chính xác là giao điểm của nó với vòng tròn trước đó. Diện tích chồng chéo giống nhau sẽ bị trừ một lần cho mỗi một trong số (N-1) phép cộng, do đó, mọi điểm được bao phủ bởi nhiều vòng tròn sẽ được tính chính xác một lần trong liên kết cuối cùng. 

## Giải pháp Python```python
import sys
import math

input = sys.stdin.readline

def solve():
    t = int(input())

    out = []

    for _ in range(t):
        n, r, l = map(int, input().split())

        if n == 0 or r == 0:
            out.append("0.0000000000")
            continue

        circle_area = math.pi * r * r
        area = n * circle_area

        if l < 2 * r:
            x = l / (2.0 * r)

            # Protect acos from a possible tiny floating-point drift.
            x = max(-1.0, min(1.0, x))

            overlap = (
                2.0 * r * r * math.acos(x)
                - 0.5 * l * math.sqrt(4.0 * r * r - l * l)
            )

            area -= (n - 1) * overlap

        out.append(f"{area:.10f}")

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Nhánh đầu tiên xử lý hai trường hợp diện tích bằng 0 trước bất kỳ phép chia hoặc căn bậc hai nào. Điều này là cần thiết vì công thức chồng chéo chứa (L/(2R)). 

ban đầu`area`là tổng diện tích của tất cả (N) hình tròn. Khi (L\ge2R), các vòng tròn rời nhau hoặc tiếp tuyến, do đó độ trùng lặp bằng 0 và giá trị đã đúng. 

Đối với các vòng tròn chồng chéo,`x`là (L/(2R)), cosin của nửa góc chắn dây chung. các`max`Và`min`kẹp bảo vệ`math.acos`từ một giá trị dấu phẩy động như`1.0000000000000002`, mặc dù giá trị toán học chính xác nằm trong khoảng hợp lệ. 

Biểu thức dưới`sqrt`không âm bất cứ khi nào (L<2R). Việc so sánh được thực hiện bằng cách sử dụng các số nguyên ban đầu, do đó không có sự mơ hồ ở ranh giới (L=2R). 

Số nguyên Python có thể biểu thị các tích có giá trị lên tới (10^9) mà không bị tràn. Các tính toán hình học cuối cùng sử dụng`float`, có độ chính xác đủ cho dung sai (10^{-6}) cần thiết. Việc in mười chữ số sau dấu thập phân sẽ tăng thêm sự an toàn chống lại lỗi định dạng. 

## Ví dụ đã hoạt động 

Mẫu được cung cấp đại diện cho một trường hợp thử nghiệm có (N=4), (R=1) và (L=1). Đầu vào như được hiển thị trong câu lệnh đã mất ngắt dòng, do đó nó tương ứng với:```
1
4 1 1
```Trong trường hợp này, các vòng tròn lân cận chồng lên nhau vì (1<2). 

| Bước | (N) | (R) | (L) | Khu vực ban đầu | Cặp chồng lên nhau | Khu vực cuối cùng | 
| --- | --- | --- | --- | --- | --- | --- | 
| Đầu vào | 4 | 1 | 1 | (4\pi) | (1.2283697\ldots) | (8.8812621\ldots) | 

Sự chồng lên nhau của hai đường tròn đơn vị có tâm cách nhau một đơn vị là 

\frac{2\pi}{3}-\frac{\sqrt3}{2}. 
] 

Có ba cặp liền kề nên diện tích hợp là 

# 3\left(\frac{2\pi}{3}-\frac{\sqrt3}{2}\right) 

2\pi+\frac{3\sqrt3}{2} 
\ khoảng 8,881262. 
] 

Điều này chứng tỏ tại sao chỉ cần trừ đi sự chồng chéo theo cặp vẫn đúng mặc dù bốn vòng tròn có thể tham gia vào các khu vực có nhiều vòng tròn chồng lên nhau. Mỗi vòng tròn chỉ được thêm một lần và diện tích bao phủ mới của nó được xác định hoàn toàn bởi vòng tròn trước đó. 

Đối với ví dụ thứ hai, hãy xem xét hai đường tròn tiếp tuyến:```
1
2 3 6
```Ở đây (2R=6=L), vậy các vòng tròn chạm vào đúng một điểm. Một điểm có diện tích bằng 0, nghĩa là không có diện tích để trừ. 

| Bước | (N) | (R) | (L) | Tình trạng | Chồng chéo | Khu vực cuối cùng | 
| --- | --- | --- | --- | --- | --- | --- | 
| Đầu vào | 2 | 3 | 6 | (L\ge2R) | 0 | (18\pi) | 

Câu trả lời là (18\pi\khoảng56,5486677646). Dấu vết này thực hiện ranh giới tiếp tuyến chính xác và cho thấy tại sao nhánh chồng chéo phải sử dụng`L < 2 * R`, không`L <= 2 * R`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(T)) | Mỗi trường hợp thử nghiệm thực hiện một số lượng không đổi các phép toán số học, căn bậc hai và lượng giác. | 
| Không gian | (O(T)) | Các chuỗi đầu ra được lưu trữ trước khi được ghi, trong khi bộ nhớ làm việc cho mỗi trường hợp kiểm thử là (O(1)). | 

Với (T\le10^5), thuật toán chỉ thực hiện một lượng công việc không đổi cho mỗi trường hợp thử nghiệm, do đó nó chia tỷ lệ tuyến tính với kích thước đầu vào. Không có sự phụ thuộc vào (N), điều này rất cần thiết vì (N) có thể lớn bằng (10^9). Số học chỉ sử dụng một vài biến vô hướng và thậm chí việc lưu trữ tất cả các kết quả đầu ra được định dạng cũng đủ nhỏ cho giới hạn bộ nhớ đã nêu. 

## Trường hợp thử nghiệm```python
import sys
import io
import math

def solve():
    input = sys.stdin.readline
    t = int(input())
    out = []

    for _ in range(t):
        n, r, l = map(int, input().split())

        if n == 0 or r == 0:
            out.append("0.0000000000")
            continue

        circle_area = math.pi * r * r
        area = n * circle_area

        if l < 2 * r:
            x = l / (2.0 * r)
            x = max(-1.0, min(1.0, x))

            overlap = (
                2.0 * r * r * math.acos(x)
                - 0.5 * l * math.sqrt(4.0 * r * r - l * l)
            )

            area -= (n - 1) * overlap

        out.append(f"{area:.10f}")

    sys.stdout.write("\n".join(out))

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

def check(inp: str, expected: list[float], eps: float = 1e-6):
    actual = list(map(float, run(inp).split()))
    assert len(actual) == len(expected)

    for a, e in zip(actual, expected):
        assert abs(a - e) <= eps * max(1.0, abs(e)), (a, e)

# Provided sample.
check(
    "1\n4 1 1\n",
    [2 * math.pi + 3 * math.sqrt(3) / 2]
)

# Minimum-size input: no lamps.
check(
    "1\n0 1000000000 1000000000\n",
    [0.0]
)

# One circle: L is irrelevant.
check(
    "1\n1 7 0\n",
    [49 * math.pi]
)

# Coincident circles: all N circles occupy exactly the same region.
check(
    "1\n5 3 0\n",
    [9 * math.pi]
)

# Tangency boundary: L = 2R, so there is no positive-area overlap.
check(
    "1\n2 3 6\n",
    [18 * math.pi]
)

# Completely separated circles.
check(
    "1\n4 2 5\n",
    [16 * math.pi]
)

# Several strongly overlapping circles.
check(
    "1\n10 10 1\n",
    [
        10 * math.pi * 100
        - 9 * (
            200 * math.acos(1 / 20)
            - 0.5 * math.sqrt(399)
        )
    ]
)
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 0 1000000000 1000000000`|`0`| Không đèn, đoàn trống | 
|`1 / 1 7 0`| (49\pi) | Vòng tròn đơn và khoảng cách không liên quan | 
|`1 / 5 3 0`| (9\pi) | Tất cả các vòng tròn trùng nhau | 
|`1 / 2 3 6`| (18\pi) | Tiếp tuyến chính xác tại (L=2R) | 
|`1 / 4 2 5`| (16\pi) | Vòng tròn hoàn toàn rời rạc | 
|`1 / 10 10 1`| Giá trị công thức | Sự chồng chéo mạnh mẽ và phép trừ lặp đi lặp lại | 

Các bài kiểm tra so sánh kết quả dấu phẩy động bằng số thay vì so sánh các chuỗi được định dạng. Đây là cách chính xác để kiểm tra một nghiệm hình học vì các phép tính tương đương về mặt toán học có thể khác nhau một vài đơn vị ở một số chữ số thập phân cuối cùng. 

## Vỏ cạnh 

Với (N=0), hãy xem xét```
1
0 1000000000 1000000000
```Thuật toán nhập ngay điều kiện đầu tiên và trả về`0.0000000000`. Không có vòng tròn nào tồn tại nên không có vùng được chiếu sáng. Việc triển khai bất cẩn giả sử (N\ge1) vẫn có thể trả về biểu thức khác 0 nếu nó sử dụng bán kính mà không kiểm tra số lượng đèn. 

Với (N=1), hãy xem xét```
1
1 2 100
```Thuật toán tính toán một diện tích hình tròn, (4\pi) và không bao giờ cần công thức chồng lấp vì (N-1=0). Giá trị khoảng cách không mô tả bất kỳ cặp đèn thực tế nào trong trường hợp này. Sản lượng là khoảng`12.5663706144`. 

Đối với các vòng tròn trùng nhau, hãy xem xét```
1
5 3 0
```Công thức chồng chéo cho 

[ 
2\cdot9\arccos(0)-0=9\pi. 
] 

Tổng ban đầu là (5\cdot9\pi) và bốn phần trùng lặp của (9\pi) bị loại bỏ: 

[ 
45\pi-4(9\pi)=9\pi. 
] 

Do đó, thuật toán xử lý chính xác tất cả năm đèn như một đĩa được chiếu sáng. Trường hợp này cũng xác nhận rằng phương pháp này xử lý mức độ trùng lặp cực đại có thể xảy ra mà không yêu cầu một công thức đặc biệt ngoài biểu thức giao nhau chung. 

Đối với đường tròn tiếp tuyến, hãy xem xét```
1
2 3 6
```Vì (L=2R), nhánh chồng chéo bị bỏ qua. Câu trả lời là 

[ 
2\pi\cdot3^2=18\pi. 
] 

Các vòng tròn chỉ chia sẻ một điểm biên có diện tích bằng 0. Sử dụng một cách nghiêm ngặt`<`so sánh tránh việc đánh giá căn bậc hai ở mức 0 một cách không cần thiết và làm cho ý nghĩa hình học của ranh giới trở nên rõ ràng. 

Đối với các vòng tròn riêng biệt, hãy xem xét```
1
4 2 5
```Ở đây (2R=4<L=5), do đó, mọi vòng tròn đều tách rời khỏi vòng tròn tiếp theo và do đó với mọi vòng tròn khác. Câu trả lời đơn giản là 

[ 
4\cdot\pi\cdot2^2=16\pi. 
] 

Thuật toán không bao giờ gọi`acos`hoặc`sqrt`trong nhánh này, do đó không có rủi ro khi đánh giá công thức giao bên ngoài miền hình học của nó. 

Đối với nhiều vòng tròn chồng chéo nhiều, hãy xem xét (N=10,R=10,L=1). Mỗi vòng tròn mới được thêm vào sẽ chồng lên liên kết trước đó, nhưng số lượng cần loại bỏ vẫn chính xác là sự chồng chéo của hai vòng tròn. Thuật toán thực hiện phép trừ đó chín lần thay vì cố gắng liệt kê các giao điểm bậc ba, bậc bốn hoặc bậc cao hơn. Đây là bất biến trung tâm của nghiệm và là yếu tố làm giảm toàn bộ bài toán hình học thành công (O(1)) cho mỗi trường hợp thử nghiệm.
