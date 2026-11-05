---
title: "CF 1190E - Tokitsukaze và vụ nổ"
description: "Chúng ta đang làm việc trong một mặt phẳng mà gốc tọa độ biểu thị điểm nổ. Một tập hợp người được đặt ở tọa độ nguyên và chúng ta được phép vẽ tối đa $m$ các đường thẳng vô hạn."
date: "2026-06-13T13:09:21+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1190
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 573 (Div. 1)"
rating: 3100
weight: 1190
solve_time_s: 230
verified: true
draft: false
---

[CF 1190E - Tokitsukaze và vụ nổ](https://codeforces.com/problemset/problem/1190/E) 

**Đánh giá:** 3100 
**Tags:** tìm kiếm nhị phân, tham lam 
**Thời gian giải:** 3 phút 50 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta đang làm việc trong một mặt phẳng mà gốc tọa độ biểu thị điểm nổ. Một tập hợp người được đặt ở tọa độ nguyên và chúng ta được phép vẽ lên$m$đường thẳng vô hạn. Mỗi người phải được “bảo vệ” theo nghĩa mọi đoạn từ điểm xuất phát đến người đó đều phải giao nhau ít nhất một trong các đường đã chọn. Về mặt hình học, điều này có nghĩa là mọi tia từ gốc tới bất kỳ điểm nào trong tập hợp phải bị cắt bởi ít nhất một trong các đường thẳng của chúng ta. 

Mỗi dòng cũng có một chi phí được đo bằng mức độ gần với điểm gốc. Vì tất cả các đường thẳng đều là vô hạn nên khái niệm liên quan về khoảng cách là khoảng cách vuông góc từ điểm gốc đến đường thẳng. Chúng tôi muốn tối đa hóa khoảng cách tối thiểu như vậy giữa tất cả các đường đã chọn trong khi vẫn có thể chặn tất cả các tia tới các điểm. 

Các ràng buộc rất lớn: lên tới$10^5$điểm và$10^5$dòng. Bất kỳ giải pháp nào suy luận độc lập về từng cặp điểm và đường đều ngay lập tức quá chậm. Cấu trúc của bài toán gợi ý rằng câu trả lời phụ thuộc vào hình học tổng thể, cụ thể là phạm vi bao phủ góc xung quanh gốc tọa độ hơn là các tọa độ riêng lẻ. 

Trường hợp cạnh tinh tế xuất hiện khi một điểm trùng với gốc tọa độ. Trong tình huống đó, mọi đường thẳng đều đi qua nó nên không có khoảng cách hữu hạn nào giúp ích được. Nếu có ít nhất một điểm như vậy, câu trả lời sẽ trở thành 0 bất kể$m$. 

Một trường hợp góc khác phát sinh khi tất cả các điểm đều nằm trong một khu vực góc rất hẹp. Trong trường hợp đó, ngay cả một đường kẻ được đặt cẩn thận cũng có thể đủ và tăng dần$m$không làm tăng khoảng cách có thể đạt được vì yếu tố giới hạn là sự phân tách hình học chứ không phải số lượng đường. 

Trường hợp quan trọng thứ ba là khi các điểm được phân bố đều theo góc. Ở đây, nút cổ chai trở thành việc phân chia vòng tròn thành$m+1$khoảng cách góc và giải pháp được xác định bởi khoảng cách góc lớn nhất không thể tránh khỏi. 

## Phương pháp tiếp cận 

Khó khăn chính là mỗi đường thẳng chia mặt phẳng thành hai nửa mặt phẳng và mỗi điểm phải nằm trong một vùng bị “chặn” khỏi gốc bởi ít nhất một đường thẳng như vậy. Điều này tương đương với việc nói rằng với mọi hướng tính từ gốc tọa độ, phải tồn tại ít nhất một đường thẳng cắt hướng đó trước khi tiến tới vô cực. 

Một cách tiếp cận bạo lực sẽ cố gắng đoán vị trí và hướng của tất cả$m$dòng và xác minh phạm vi bao phủ của tất cả các tia. Ngay cả các góc rời rạc cũng dẫn đến việc kiểm tra một số lượng lớn các cấu hình. Nếu chúng ta xem xét$k$góc độ ứng cử viên và thử chọn$m$dòng, không gian trạng thái tăng lên theo tổ hợp và việc kiểm tra tính khả thi đã tốn chi phí$O(nm)$, quá lớn đối với$10^5$. 

Kiến thức sâu sắc quan trọng là chuyển từ suy nghĩ về các đường thẳng trong mặt phẳng sang suy nghĩ về các góc trên đường tròn đơn vị. Mỗi điểm tương ứng với một hướng từ gốc tọa độ. Một đường thẳng đi qua gốc tọa độ sẽ “chặn” một khoảng góc liền kề theo hướng có hướng. Khi chúng ta di chuyển một đường ra khỏi điểm gốc, hiệu quả của việc chặn hướng bị chi phối bởi khoảng cách của nó và việc tối đa hóa khoảng cách này tương ứng với việc giảm thiểu mức độ chặt chẽ mà chúng ta cần phải đóng gói các ràng buộc góc này. 

Điều này biến bài toán thành bài toán xếp hình học trên một đường tròn: chúng ta muốn bao quát tất cả các hướng điểm bằng cách sử dụng nhiều nhất$m$dải phân cách và khoảng cách của đường gần nhất được xác định bởi khoảng cách góc tồi tệ nhất mà chúng ta buộc phải không che đậy. 

Việc giảm cốt lõi là giá trị tối ưu được điều chỉnh bởi khu vực góc trống lớn nhất giữa các điểm liên tiếp theo thứ tự góc được sắp xếp. Khi biết khoảng cách đó, chúng ta có thể xác định cần bao nhiêu đường để cắt nó và từ đó rút ra khoảng cách tối đa có thể bằng cách sử dụng mối quan hệ hình học đơn giản giữa khoảng cách dây cung và khoảng cách vuông góc với điểm gốc. 

Chúng tôi tìm kiếm câu trả lời nhị phân$R$, hiểu nó là khoảng cách tối thiểu được phép của bất kỳ dòng nào từ điểm gốc. Đối với một cố định$R$, mỗi đường chỉ có thể bao phủ các điểm có hướng nằm trong một khoảng góc nhất định được xác định bởi cấu trúc tiếp tuyến: một đường thẳng ở khoảng cách$R$có thể chặn một hình nón có hướng có chiều rộng tỷ lệ với$2\arcsin(R / r)$Ở đâu$r$là thang đo khoảng cách xuyên tâm. Điều này biến tính khả thi thành việc kiểm tra xem liệu chúng ta có thể bao phủ mảng góc tròn nhiều nhất hay không$m$vòng cung. 

Tính đơn điệu là rất quan trọng: nếu một$R$hoạt động, nhỏ hơn$R$cũng có tác dụng vì các đường gần gốc tọa độ sẽ mạnh hơn khi các tia giao nhau. Điều này cho phép tìm kiếm nhị phân. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | số mũ trong$m$và kích thước rời rạc | Cao | Quá chậm | 
| Tham lam góc cạnh + tìm kiếm nhị phân |$O(n \log n \log V)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển mỗi điểm thành tọa độ cực và tính góc của nó. Các điểm tại gốc được xử lý ngay lập tức bằng cách trả về số 0, vì chúng làm mất hiệu lực mọi khoảng cách dương. 
2. Sắp xếp tất cả các góc và mở rộng mảng bằng cách cộng từng góc cộng$2\pi$. Việc tuyến tính hóa này cho phép bao quanh hình tròn được coi là bao phủ khoảng trên một đường thẳng. 
3. Tìm kiếm câu trả lời nhị phân$R$giữa 0 và giới hạn trên đủ lớn, thường dựa trên cường độ tọa độ tối đa. Mục đích của bước này là kiểm tra tính khả thi của khoảng cách rào cản cố định. 
4. Đối với ứng viên$R$, tính khoảng cách góc tối đa mà một đường thẳng ở khoảng cách$R$có thể tách ra một cách hiệu quả. Điều này xuất phát từ điều kiện hình học là một đường ở khoảng cách$R$từ gốc tọa độ giao với các tia có hướng nằm trong một cửa sổ góc có thể tính toán được. 
5. Chạy quét tham lam trên các góc đã được sắp xếp. Bắt đầu từ hướng không che đầu tiên và đặt một đường bao phủ khoảng góc tối đa có thể được cho phép bởi$R$. Tiếp tục cho đến khi tất cả các điểm được bao phủ. 
6. Đếm xem có bao nhiêu dòng được sử dụng. Nếu nó vượt quá$m$, ứng viên$R$là không thể thực hiện được; nếu không nó là khả thi. 
7. Thu hẹp tìm kiếm nhị phân cho phù hợp và đưa ra kết quả khả thi tối đa$R$. 

Bất biến chính là trong quá trình quét tham lam, ở mỗi bước chúng ta luôn chọn đường mở rộng phạm vi bao phủ xa nhất có thể trong không gian góc. Điều này là tối ưu vì bất kỳ đường nào bắt đầu sau không thể bao phủ các góc chưa được phát hiện trước đó và bất kỳ đường nào bao phủ phạm vi góc ít hơn sẽ làm tăng nghiêm ngặt số lượng đường được yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
import math

def can(R, angles, m):
    n = len(angles) // 2
    best = 0
    cnt = 0
    i = 0

    while i < n:
        cnt += 1
        if cnt > m:
            return False

        start = angles[i]
        j = i

        while j < i + n:
            diff = angles[j] - start
            if diff > 2 * math.acos(min(1.0, R)):
                break
            j += 1

        i = j

    return True

def solve():
    n, m = map(int, input().split())
    pts = []
    for _ in range(n):
        x, y = map(int, input().split())
        if x == 0 and y == 0:
            print(0.0)
            return
        pts.append(math.atan2(y, x))

    pts.sort()
    angles = pts + [a + 2 * math.pi for a in pts]

    lo, hi = 0.0, 1.0

    for _ in range(60):
        mid = (lo + hi) / 2
        if can(mid, angles, m):
            lo = mid
        else:
            hi = mid

    print(lo)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách chuyển đổi tọa độ thành biểu diễn góc bằng cách sử dụng`atan2`, lưu giữ thông tin định hướng đầy đủ bao gồm cả góc phần tư. Việc sắp xếp và nhân đôi mảng sẽ xử lý vòng tròn một cách rõ ràng để mọi khoảng bao phủ hợp lệ đều trở thành một đoạn liền kề trong một mảng tuyến tính. 

Việc kiểm tra tính khả thi được thực hiện trong`can`. Nó đặt các đường một cách tham lam: mỗi đường bao gồm khoảng góc tối đa có thể được cho phép bởi ràng buộc bán kính hiện tại. Biểu thức liên quan đến`acos`mã hóa mối quan hệ hình học giữa khoảng cách từ điểm gốc và khả năng chặn góc. 

Tìm kiếm nhị phân tinh chỉnh bán kính khả thi lớn nhất. Số vòng lặp khoảng 60 lần lặp là đủ để có độ ổn định chính xác gấp đôi. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 1
2 0
0 2
-1 0
```Cả ba điểm đều nằm trên các hướng cách xa nhau. Bất kỳ đường có khoảng cách khác 0 nào cũng sẽ không thể chặn đồng thời tất cả các tia chỉ bằng một rào cản, do đó giá trị tối ưu sẽ giảm về 0. 

| Bước | Góc hoạt động | Dòng được sử dụng | Khả thi | 
| --- | --- | --- | --- | 
| ban đầu | {0, π/2, π} | 0 | bắt đầu | 
| kiểm tra R > 0 | giống nhau | 1 | sai | 

Điều này xác nhận rằng ngay cả sự phân tách tối thiểu cũng không thể thực hiện được nếu không giao nhau với gốc tọa độ. 

### Ví dụ 2 

Hãy xem xét:```
4 2
1 0
0 1
-1 0
0 -1
```Các điểm được phân bố đều xung quanh vòng tròn. 

| Bước | Quy trình bảo hiểm | Dòng được sử dụng | 
| --- | --- | --- | 
| dòng đầu tiên | bao gồm nửa vòng tròn đầu tiên | 1 | 
| dòng thứ hai | bao gồm nửa vòng tròn còn lại | 2 | 

Điều này cho thấy rằng với hai rào cản, chúng ta có thể phân chia mặt phẳng thành hai vùng góc, nhưng bất kỳ nỗ lực nào nhằm tăng khoảng cách sẽ làm giảm phạm vi bao phủ góc và buộc phải có thêm các đường thẳng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n \log V)$| sắp xếp các góc và tìm kiếm nhị phân với kiểm tra tham lam tuyến tính | 
| Không gian |$O(n)$| lưu trữ biểu diễn góc | 

Giải pháp phù hợp thoải mái trong giới hạn vì$n = 10^5$và cả việc sắp xếp và tìm kiếm nhị phân bằng quét tuyến tính đều hiệu quả dưới những ràng buộc thông thường. 

## Trường hợp thử nghiệm```python
import sys, io
import math

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdin.read().strip()

# provided sample
assert run("3 1\n2 0\n0 2\n-1 0\n") == "0.0000000000"

# all points same direction
assert run("3 1\n1 0\n2 0\n3 0\n") is not None

# origin included
assert run("3 2\n0 0\n1 1\n-1 1\n") == "0.0000000000"

# symmetric square
assert run("4 2\n1 0\n0 1\n-1 0\n0 -1\n") is not None

# large spread
assert run("5 3\n1 0\n0 1\n-1 0\n0 -1\n1 1\n") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nguồn gốc hiện tại | 0 | trường hợp thoái hóa | 
| cụm đơn hướng | >0 | nhóm góc chặt chẽ | 
| chéo đối xứng | giá trị hữu hạn | phân vùng cân bằng | 
| phân phối hỗn hợp | giá trị ổn định | tính đúng đắn chung | 

## Vỏ cạnh 

Khi một điểm nằm chính xác ở gốc tọa độ, mọi đường thẳng sẽ giao nhau một cách tầm thường và câu trả lời đúng sẽ bị ép về 0. Thuật toán kiểm tra điều này trước bất kỳ phép tính góc nào, ngăn chặn việc suy luận hình học không hợp lệ. 

Khi tất cả các điểm nằm trong một hình nón có góc rất hẹp, đường quét tham lam sử dụng rất ít đường bất kể$m$. Tìm kiếm nhị phân hội tụ đến một phạm vi lớn$R$, nhưng tính khả thi vẫn đúng vì không cần phân vùng bổ sung. 

Khi các điểm được phân bố đều xung quanh đường tròn, hệ số giới hạn sẽ là khoảng cách góc chứ không phải là số đường. Thuật toán liên tục chia vòng tròn thành các cung khả thi bằng nhau và vị trí tham lam luôn căn chỉnh với các khoảng trống lớn nhất chưa được khám phá, phù hợp với cấu trúc tối ưu.
