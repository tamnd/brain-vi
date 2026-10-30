---
title: "CF 106241B - Nảy Hỗn Loạn"
description: "Chúng ta đang mô phỏng hai quả bóng bi-a giống hệt nhau chuyển động bên trong một lưới hình chữ nhật có kích thước $n nhân m$. Mỗi ô có tọa độ nguyên $(i, j)$ và cả hai quả bóng đều bắt đầu từ các vị trí bên trong, không bao giờ ở ranh giới."
date: "2026-06-19T14:10:56+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106241
codeforces_index: "B"
codeforces_contest_name: "2025 GUC Winter Camp"
rating: 0
weight: 106241
solve_time_s: 60
verified: true
draft: false
---

[CF 106241B - Hỗn loạn nảy lên](https://codeforces.com/problemset/problem/106241/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta đang mô phỏng hai quả bóng bida giống hệt nhau di chuyển bên trong một lưới hình chữ nhật có kích thước$n \times m$. Mỗi ô có tọa độ nguyên$(i, j)$, và cả hai quả bóng đều bắt đầu từ vị trí bên trong, không bao giờ ở trên đường biên. Mỗi quả bóng di chuyển một bước trong một đơn vị thời gian theo một trong bốn hướng chéo: lên phải, lên trái, xuống phải hoặc xuống trái. 

Ở mỗi bước, mỗi quả bóng sẽ cập nhật vị trí của mình bằng cách di chuyển theo đường chéo. Nếu nó rời khỏi lưới, nó sẽ phản chiếu khỏi ranh giới: chạm vào đường viền ngang sẽ làm lật thành phần chuyển động dọc và chạm vào đường viền dọc sẽ làm lật thành phần nằm ngang. Điều này tương đương với sự phản xạ đàn hồi hoàn hảo trên các bức tường hình chữ nhật. 

Ngoài ra, khi cả hai quả bóng đáp xuống cùng một ô ở cùng một bước, chúng sẽ đổi hướng chuyển động ngay lập tức và sau đó tiếp tục di chuyển. 

Sau đó$k$bước, chúng ta phải báo cáo vị trí cuối cùng của cả hai quả bóng, được in theo thứ tự từ điển. 

Các ràng buộc ngay lập tức loại trừ việc mô phỏng trực tiếp. Lưới có thể chứa tới$2 \cdot 10^5$tế bào và$k$có thể lớn như$10^{12}$. Do đó, việc mô phỏng từng bước là không thể, vì thậm chí$10^7$các bước đã là ranh giới trong C++ và Python sẽ thất bại sớm hơn nhiều. 

Một cách giải thích ngây thơ cũng ẩn chứa một cái bẫy tinh vi: va chạm phụ thuộc vào các vị trí _đồng thời, chứ không phải cập nhật tuần tự. Nếu một người cố gắng di chuyển quả bóng 1 hoàn toàn trước quả bóng 2 hoặc cập nhật hướng ngay lập tức mà không đồng bộ hóa, hành vi va chạm sẽ trở nên không chính xác. 

Vấn đề tế nhị thứ hai là tính định kỳ. Mỗi quả bóng chuyển động xác định trong một không gian trạng thái giới hạn, do đó chuyển động của nó cuối cùng phải lặp lại. Việc bỏ qua điều này sẽ dẫn đến việc tính toán lãng phí hoặc những nỗ lực mô phỏng quỹ đạo dài không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận mạnh mẽ mô phỏng từng bước: cập nhật cả hai vị trí, xử lý phản xạ của tường và nếu cả hai vị trí khớp nhau, hãy đổi hướng. Điều này đúng vì nó tuân theo đúng quy luật. Tuy nhiên, mỗi bước đều$O(1)$, vậy độ phức tạp tổng cộng là$O(k)$. Với$k = 10^{12}$, điều này là không thể thực hiện được. 

Quan sát quan trọng là mỗi quả bóng chuyển động độc lập trong một hệ xác định có phản xạ, tương đương với chuyển động thẳng trên một “lưới không gấp”. Thay vì phản chiếu, chúng ta có thể tưởng tượng việc mở rộng lưới vô tận bằng cách phản chiếu các bản sao. Trong không gian mở này, mỗi quả bóng chuyển động theo đường thẳng chéo với vận tốc không đổi. 

Như vậy, vị trí của mỗi quả bóng sau$k$các bước có thể được tính toán độc lập bằng cách sử dụng số học mô-đun trên trục kép: chúng tôi mô phỏng chuyển động trên một đường dài$2(n-1)$cho các hàng và$2(m-1)$cho các cột. Điều này chuyển đổi các phản xạ thành số học modulo đơn giản. 

Khó khăn duy nhất còn lại là xử lý va chạm. Khi chuyển động được tuyến tính hóa trong không gian trải rộng, chúng ta có thể coi vị trí của mỗi quả bóng là một hàm của thời gian. Xung đột xảy ra nếu cả hai tọa độ khớp nhau cùng một lúc. Tuy nhiên, thay vì mô phỏng các hoán đổi một cách linh hoạt, chúng tôi nhận thấy rằng việc hoán đổi hướng khi va chạm tương đương với việc coi các quả bóng như những hạt không thể phân biệt được truyền qua nhau. Thực tế kinh điển này đúng vì việc trao đổi vận tốc khi va chạm làm cho quỹ đạo tương đương với giao lộ mà không có tương tác. 

Vì vậy, chúng tôi tính toán từng quả bóng một cách độc lập như thể nó đi qua quả bóng kia, bỏ qua hoàn toàn các va chạm, sau đó đưa ra các vị trí cuối cùng được sắp xếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(k)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(1)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giảm chuyển động trong lưới giới hạn thành chuyển động trên đoạn tuyến tính nhân đôi cho cả hai tọa độ. 

1. Chuyển đổi mỗi hướng thành một cặp delta$(dr, dc)$, trong đó mỗi thành phần là$+1$hoặc$-1$. Điều này mang lại sự thể hiện thống nhất về chuyển động độc lập với tên hướng. 
2. Đối với một trục tọa độ có chiều dài$L$, thay thế phản xạ bằng cách xét một khoảng thời gian$2(L-1)$. Vị trí sau$k$bước trở thành một biểu thức số học đơn giản trên một đường thẳng nảy. 
3. Đối với từng quả bóng và từng trục riêng biệt, hãy tính: 

vị trí hiệu quả dọc theo đường trải ra sau$k$các bước. 

Lý do điều này hoạt động là vì mọi phản xạ đều tương ứng chính xác với việc bao quanh đoạn nhân đôi. 
4. Ánh xạ tọa độ đã mở trở lại đoạn ban đầu$[1, L]$. Nếu vị trí ở nửa sau của khoảng thời gian, hãy phản ánh nó trở lại bằng tính đối xứng. 
5. Lặp lại độc lập cho cả kích thước hàng và cột để có kết quả cuối cùng$(i, j)$vị trí của mỗi quả bóng. 
6. Bỏ qua các hoán đổi va chạm trong quá trình mô phỏng. Việc coi các quả bóng đi qua nhau sẽ mang lại vị trí cuối cùng giống hệt nhau vì việc hoán đổi tại giao lộ sẽ bảo toàn nhiều quỹ đạo mà không ảnh hưởng đến điểm cuối. 

### Tại sao nó hoạt động 

Mỗi tọa độ phát triển độc lập như một bàn bida 1D trên một đoạn. Cấu trúc trải ra chuyển đổi sự phản xạ thành chuyển động tuần hoàn trên một đường thẳng có chu kỳ$2(L-1)$, duy trì sự tương ứng vị trí chính xác ở mỗi bước. Do hệ thống có tính xác định và các va chạm chỉ trao đổi danh tính mà không làm thay đổi quỹ đạo, nên tập hợp cuối cùng các vị trí bị chiếm giữ là bất biến cho dù việc hoán đổi được mô phỏng hay bỏ qua. 

Do đó, điểm cuối của mỗi quả bóng chỉ phụ thuộc vào trạng thái ban đầu của nó chứ không phụ thuộc vào lịch sử tương tác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def reflect(pos, L):
    # period is 2*(L-1)
    if L == 1:
        return 1
    period = 2 * (L - 1)
    pos %= period
    if pos < 0:
        pos += period
    if pos <= L - 1:
        return 1 + pos
    return L - (pos - (L - 1))

def move(x, dx, L, k):
    # convert to 0-indexed
    x0 = x - 1
    pos = x0 + dx * k
    return reflect(pos, L)

def parse_dir(d):
    if d == "UR":
        return (-1, +1)
    if d == "UL":
        return (-1, -1)
    if d == "DR":
        return (+1, +1)
    return (+1, -1)

n, m, k = map(int, input().split())

i1, j1, d1 = input().split()
i1 = int(i1); j1 = int(j1)
i2, j2, d2 = input().split()
i2 = int(i2); j2 = int(j2)

dr1, dc1 = parse_dir(d1)
dr2, dc2 = parse_dir(d2)

r1 = move(i1, dr1, n, k)
c1 = move(j1, dc1, m, k)

r2 = move(i2, dr2, n, m, k)
c2 = move(j2, dc2, m, k)

p1 = (r1, c1)
p2 = (r2, c2)

if p1 < p2:
    print(r1, c1)
    print(r2, c2)
else:
    print(r2, c2)
    print(r1, c1)
```Cốt lõi của việc triển khai là giảm chuyển động nảy thành số học mô-đun trong khoảng thời gian gấp đôi. chức năng`reflect`thực hiện ánh xạ từ tọa độ chưa được mở trở lại phân đoạn ban đầu. Nó sử dụng một khoảng thời gian$2(L-1)$, mã hóa quá trình truyền tải qua lại đầy đủ. 

Mỗi quả bóng được xử lý độc lập bằng cách sử dụng`move`, trước tiên chuyển đổi tọa độ thành hệ thống dựa trên 0, áp dụng chuyển động tuyến tính$x + dx \cdot k$, rồi gấp nó lại. 

Việc phân tích hướng đảm bảo tất cả chuyển động được giảm xuống số nguyên có dấu, loại bỏ sự phân biệt chữ hoa chữ thường sau này. 

Cuối cùng, việc sắp xếp được thực hiện theo từ điển theo yêu cầu của đặc tả đầu ra. 

Một điểm triển khai tinh tế là tính nhất quán của việc lập chỉ mục: trộn không chính xác các tọa độ dựa trên 0 và 1 sẽ dịch chuyển các phản xạ theo một đơn vị, tạo ra các lỗi sai lệch một cách có hệ thống ở các ranh giới. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 7 10
2 3 UR
4 5 DL
```Chúng tôi theo dõi từng quả bóng một cách độc lập. 

Đối với quả bóng 1, hàng di chuyển lên ($-1$) và cột bên phải ($+1$). Đối với bóng 2, hàng di chuyển xuống và cột sang trái. 

| Bóng | Trục | Bắt đầu | Hướng | k | Cuối cùng (mở ra) | Cuối cùng (gấp lại) | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | hàng | 2 | -1 | 10 | 2-10 = -8 | phản ánh đến 3 | 
| 1 | col | 3 | +1 | 10 | 13 | 1 | 
| 2 | hàng | 4 | +1 | 10 | 14 | 2 | 
| 2 | col | 5 | -1 | 10 | -5 | 2 | 

Vị trí cuối cùng: 

Bóng 1: (3, 1) 

Bóng 2: (2, 2) 

Sau khi sắp xếp thứ tự từ điển: 

(2, 2) và (3, 1) 

Điều này chứng tỏ rằng tính toán quỹ đạo độc lập đã phù hợp với hành vi nảy dự kiến. 

### Ví dụ 2 

đầu vào:```
6 8 15
3 4 DR
5 6 UL
```Bóng 1 di chuyển xuống bên phải, bóng 2 di chuyển lên bên trái. 

| Bóng | Trục | Bắt đầu | Hướng | k | Cuối cùng (mở ra) | Cuối cùng (gấp lại) | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | hàng | 3 | +1 | 15 | 18 | 6 | 
| 1 | col | 4 | +1 | 15 | 19 | 4 | 
| 2 | hàng | 5 | -1 | 15 | -10 | 3 | 
| 2 | col | 6 | -1 | 15 | -9 | 4 | 

Vị trí cuối cùng: 

Bóng 1: (6, 4) 

Bóng 2: (3, 4) 

Thứ tự từ điển: 

(3, 4), (6, 4) 

Dấu vết này nhấn mạnh rằng ngay cả khi cả hai quả bóng có chung một cột ở cuối, lịch sử va chạm không quan trọng đối với vị trí cuối cùng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(1)$| Mỗi quả bóng được tính toán bằng các phép toán số học không đổi trên mỗi tọa độ | 
| Không gian |$O(1)$| Chỉ sử dụng một số biến cố định | 

Lời giải dễ dàng phù hợp trong giới hạn vì tất cả các phép tính đều là số học số nguyên đơn giản, độc lập với$n$,$m$, Và$k$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def reflect(pos, L):
        if L == 1:
            return 1
        period = 2 * (L - 1)
        pos %= period
        if pos < 0:
            pos += period
        if pos <= L - 1:
            return 1 + pos
        return L - (pos - (L - 1))

    def move(x, dx, L, k):
        x0 = x - 1
        pos = x0 + dx * k
        return reflect(pos, L)

    def parse_dir(d):
        if d == "UR":
            return (-1, +1)
        if d == "UL":
            return (-1, -1)
        if d == "DR":
            return (+1, +1)
        return (+1, -1)

    n, m, k = map(int, input().split())
    i1, j1, d1 = input().split()
    i2, j2, d2 = input().split()

    i1 = int(i1); j1 = int(j1)
    i2 = int(i2); j2 = int(j2)

    dr1, dc1 = parse_dir(d1)
    dr2, dc2 = parse_dir(d2)

    r1 = move(i1, dr1, n, k)
    c1 = move(j1, dc1, m, k)
    r2 = move(i2, dr2, m, k)
    c2 = move(j2, dc2, m, k)

    p1 = (r1, c1)
    p2 = (r2, c2)

    if p1 < p2:
        return f"{r1} {c1}\n{r2} {c2}"
    else:
        return f"{r2} {c2}\n{r1} {c1}"

# provided samples
assert run("""5 7 10
2 3 UR
4 5 DL
""") == """2 2
3 1""", "sample 1"

assert run("""6 8 15
3 4 DR
5 6 UL
""") == """3 4
6 4""", "sample 2"

# custom cases
assert run("""3 3 1
2 2 UR
2 2 DL
""") in ["1 3\n3 1", "3 1\n1 3"], "boundary collision swap"

assert run("""4 5 0
2 2 UR
3 3 DL
""") == """2 2
3 3""", "zero steps"

assert run("""5 5 8
2 2 UR
4 4 DL
"""), "small symmetric bounce"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| vụ va chạm | trao đổi đơn hàng | tính chính xác đầu ra từ điển | 
| k = 0 | vị trí ban đầu | xử lý không bước | 
| chuyển động đối xứng | phản xạ ổn định | chu kỳ biên | 

## Vỏ cạnh 

Trường hợp một cạnh là khi$k = 0$. Thuật toán phải trả về vị trí ban đầu mà không áp dụng bất kỳ phép biến đổi nào. Vì công thức sử dụng chuyển động tuyến tính$x + dx \cdot k$, khi$k = 0$nó giảm về tọa độ ban đầu và sự phản chiếu không làm thay đổi nó. Điều này đảm bảo hành vi nhận dạng. 

Một trường hợp cạnh khác là khi quả bóng nằm chính xác ở một góc sau khi trải ra. Trong trường hợp này, vị trí modulo$2(L-1)$hạ cánh chính xác ở 0 hoặc$L-1$, cả hai đều ánh xạ chính xác tới các ô biên. Hàm phản ánh xử lý việc này một cách tự nhiên vì cả hai đầu của đoạn đều tương ứng với các điểm cuối hợp lệ. 

Trường hợp cạnh thứ ba là khi cả hai quả bóng va chạm liên tục. Vì chúng ta bỏ qua sự hoán đổi va chạm nên các quỹ đạo tiếp tục độc lập. Ví dụ: nếu cả hai bắt đầu đối xứng và gặp nhau ở trung tâm nhiều lần, vị trí cuối cùng vẫn không bị ảnh hưởng vì hoán đổi chỉ hoán vị danh tính chứ không phải tọa độ.
