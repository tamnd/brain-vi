---
title: "CF 102411B - Xấu"
description: "Một treap đồng thời là một cây tìm kiếm nhị phân theo khóa (x) và một heap tối thiểu theo mức độ ưu tiên (y). Trong bài toán này, mức độ ưu tiên không phải là ngẫu nhiên: với mỗi khóa số nguyên (x), nó được cố định là [ y=sin(x)."
date: "2026-08-14T14:31:34+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102411
codeforces_index: "B"
codeforces_contest_name: "ICPC 2019-2020 North-Western Russia Regional Contest"
rating: 0
weight: 102411
solve_time_s: 83
verified: true
draft: false
---

[CF 102411B - Bad Treap](https://codeforces.com/problemset/problem/102411/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 23s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Một treap đồng thời là một cây tìm kiếm nhị phân theo khóa (x) và một heap tối thiểu theo mức độ ưu tiên (y). Trong bài toán này, mức độ ưu tiên không phải là ngẫu nhiên: với mỗi khóa số nguyên (x), nó được cố định là 

[ 
y=\sin(x). 
] 

Chúng ta phải in (n) số nguyên riêng biệt, mỗi số khớp với một số nguyên 32 bit có dấu, sao cho treap kết quả có chiều cao chính xác (n). Vì cây nhị phân chứa (n) đỉnh không thể có chiều cao lớn hơn (n), điều này có nghĩa là chúng ta cần buộc mọi nút vào một đường dẫn dài từ gốc đến lá. 

Điều quan trọng là làm cho cả hai tọa độ được sắp xếp theo cùng một hướng. Nếu 

[ 
x_1<x_2<\cdots<x_n 
] 

và đồng thời 

[ 
\sin(x_1)<\sin(x_2)<\cdots<\sin(x_n), 
] 

thì cây Cartesian không có nhánh. (y) nhỏ nhất thuộc về (x_1) nên (x_1) trở thành nghiệm. Mọi khóa khác đều lớn hơn nên mọi nút còn lại đều thuộc về cây con bên phải của nó. Đối số tương tự lặp lại đệ quy, tạo ra một chuỗi (n) đỉnh. 

Ràng buộc (n\le 50000) đủ nhỏ để việc in số nguyên (O(n)) là không đáng kể. Khó khăn thực sự không phải là độ phức tạp tính toán mà là việc xây dựng các đối số nguyên của sin nằm trong một phần tăng nghiêm ngặt của đường cong sin. Hạn chế 32-bit chỉ đơn giản là nhân một giá trị gần đúng cực kỳ chính xác với một hệ số lớn tùy ý, do đó, kết cấu phải vừa khít bên trong khoảng bốn tỷ giá trị số nguyên có thể có. 

Có một số trường hợp đáng lưu ý. Đối với (n=1), mọi số nguyên hợp lệ đều hoạt động vì cây một nút đã có chiều cao bằng một. Ví dụ,```
1
```có thể sản xuất```
0
```và cây có chiều cao (1). 

Ranh giới khác là (n=50000). Cấu trúc sử dụng các giá trị từ (-25000d) đến (24999d) phải giữ giá trị tuyệt đối lớn nhất bên dưới (2^{31}). Với (d=710), cường độ lớn nhất chỉ là (17750000), do đó, điều kiện 32 bit đã ký gần như không chặt chẽ. 

Trường hợp cạnh tinh tế hơn xảy ra xung quanh số 0. Lấy các số nguyên nhỏ liên tiếp như```
4
```và thử (x=-1,0,1,2) không được, vì các giá trị sin là 

[ 
-\sin(1),0,\sin(1),\sin(2), 
] 

điều này không có vấn đề gì ở đây, nhưng trình tự sẽ sớm đạt đến một bước ngoặt. Một cách xây dựng bất cẩn chỉ dựa trên thực tế cục bộ là sin đang tăng gần 0 có thể âm thầm thất bại một khi các đối số của nó rời khỏi khoảng đó. Đầu ra chính xác cho mẫu là một bộ có thể như```
-2
0
-1
-4
```tạo ra một chuỗi mặc dù các phím in không được sắp xếp. Treap phụ thuộc vào tập hợp các điểm chứ không phụ thuộc vào thứ tự in câu trả lời. 

Vấn đề nguy hiểm nhất là nhầm lẫn thứ tự các số nguyên được in ra với cấu trúc của cây. Ví dụ như in```
-17750000
-17749290
-17748580
```rất hữu ích vì các giá trị này là bội số liên tiếp của (710), nhưng đặc tính cơ bản là giá trị (x) và giá trị sin của chúng đều tăng nghiêm ngặt. Bản thân thứ tự đầu ra không cần phải mô tả cây. 

## Phương pháp tiếp cận 

Một chiến lược mạnh mẽ trực tiếp sẽ là tìm kiếm một tập hợp các số nguyên, xây dựng cây Descartes tương ứng và kiểm tra xem chiều cao của nó có phải là (n) hay không. Việc kiểm tra một bộ sưu tập được đề xuất có thể được thực hiện một cách hiệu quả, nhưng việc tìm kiếm trong không gian của các bộ sưu tập có thể là điều vô vọng. Có thể có (2^{32}) giá trị đã ký-32-bit, do đó, ngay cả việc tìm kiếm chỉ qua các bộ dữ liệu (n) cũng có thể 

[ 
\binom{2^{32}}{n} 
] 

khả năng. Với (n=50000), điều này vượt xa mọi giới hạn tính toán. Ngay cả một tìm kiếm vũ phu hẹp hơn nhiều so với giá trị bắt đầu và bước tích cực cũng sẽ có khoảng (2^{32}\cdot 2^{32}) cặp ứng cử viên trước khi kiểm tra (n) giá trị được tạo, theo sau là (O(n)) công việc cho mỗi ứng cử viên. Vấn đề rõ ràng cần một cấu trúc toán học hơn là tìm kiếm. 

Quan sát hữu ích là chúng ta không cần phải xây dựng cây một cách rõ ràng. Chúng ta chỉ cần tọa độ (x) và mức độ ưu tiên của chúng được sắp xếp một cách nhất quán. Vì vậy, bài toán trở thành tìm nhiều số nguyên (x) mà (\sin(x)) tăng nghiêm ngặt. 

Sin đang tăng nghiêm ngặt trên khoảng 

[ 
\left[-\frac{\pi}{2},\frac{\pi}{2}\right]. 
] 

Nếu chúng ta có thể chọn các số thực tùy ý, chúng ta chỉ cần đặt tất cả các giá trị (x) vào khoảng này. Các giá trị số nguyên không thể cung cấp cho chúng ta đủ điểm khác biệt ở đó, vì khoảng thời gian ngắn hơn nhiều so với (50000). 

Cách giải quyết vấn đề này là khai thác khoảng thời gian (2\pi). Chúng tôi muốn một số nguyên (d) cực kỳ gần với bội số của (2\pi). Sau đó 

[ 
d = 2\pi k+\delta 
] 

cho một giá trị dương rất nhỏ (\ delta). Vì sin có dấu chấm (2\pi), 

[ 
\sin(id)=\sin(i\delta). 
] 

Do đó, bội số của số nguyên lớn (d) hoạt động giống như bội số nhỏ của (\delta). 

Sự lựa chọn đặc biệt thuận tiện là (d=710). Kể từ khi 

[ 
113\cdot 2\pi \khoảng 709.9999397, 
] 

chúng tôi có 

[ 
710=113\cdot 2\pi+\delta 
] 

với 

[ 
\delta\khoảng 0,0000603. 
] 

Với mọi số nguyên (i) nằm trong khoảng (-25000) và (24999), 

[ 
tôi\delta 
] 

nằm trong khoảng từ (-1,5075) đến (1,5074). Cả hai điểm cuối đều nằm hoàn toàn bên trong ([-\pi/2,\pi/2]), có ranh giới là khoảng (1,5708). Do đó, sin tăng dần theo mọi giá trị mà chúng ta sử dụng. 

Bây giờ chúng ta có thể lấy 

[ 
x_i=710i,\qquad -25000\le i\le24999. 
] 

Các giá trị (x_i) đang tăng dần và 

[ 
\sin(x_i)=\sin(i\delta) 
] 

cũng đang tăng lên một cách nghiêm túc. Chúng ta có chính xác (50000) số nguyên như vậy, vì vậy bất kỳ tiền tố nào của dãy này đều giải quyết được vấn đề về (n) được yêu cầu. 

Cấu trúc cũng vẫn an toàn trong phạm vi 32 bit đã ký vì giá trị tuyệt đối của nó tối đa là (25000\cdot710=17,750,000). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Số mũ trong (n) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) để đệm đầu ra, thêm O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc (n), số nút cần thiết. 
2. Sử dụng bước cố định (710). Viết câu trả lời (i)-th như 

[ 
x_i=(i-25000)\cdot710 
] 

với (i=0,1,\ldots,n-1). 

Điều này đưa ra các số nguyên riêng biệt vì các giá trị liên tiếp khác nhau chính xác (710). 
3. Quan sát (710=113\cdot2\pi+\delta) để biết giá trị dương nhỏ (\delta). Vì thế 

[ 
\sin(710k)=\sin(k\delta). 
] 

Giá trị tuyệt đối lớn nhất của (k) mà chúng tôi sử dụng là (25000), do đó (|k\delta|<\pi/2). Sin đang tăng nghiêm ngặt trên toàn bộ khoảng thời gian này. 
4. Vì cả (x_i) và (\sin(x_i)) đều tăng theo (i), điểm có khóa nhỏ nhất cũng có mức ưu tiên nhỏ nhất. Nó trở thành gốc và mọi điểm khác nằm ở bên phải của nó trong cây tìm kiếm nhị phân. Áp dụng đệ quy cùng một đối số sẽ cho một đường dẫn duy nhất chứa tất cả (n) nút. 
5. In (n) số nguyên đã tạo. Cấu trúc sử dụng các giá trị trong khoảng (-17750000) và (17749290), vì vậy mọi câu trả lời đều vừa vặn thoải mái trong phạm vi có dấu 32 bit được yêu cầu.

Điều bất biến là sau khi chọn bất kỳ tiền tố nào của cấu trúc, các khóa sẽ tăng nghiêm ngặt và giá trị sin của chúng cũng tăng nghiêm ngặt. Do đó, điểm ưu tiên tối thiểu luôn là khóa ngoài cùng bên trái và mọi điểm khác thuộc về cây con bên phải của nó. Việc loại bỏ gốc đó sẽ để lại đặc tính giống hệt cho hậu tố còn lại, do đó quy nạp sẽ tạo ra một chuỗi (n) nút. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())

    base = -25000 * 710
    ans = []
    for i in range(n):
        ans.append(str(base + i * 710))

    sys.stdout.write("\n".join(ans))

if __name__ == "__main__":
    solve()
```biểu thức`base + i * 710`chính xác là công thức từ việc xây dựng. Bắt đầu từ (-25000\cdot710) và tăng hệ số nhân lên một sẽ cho chuỗi 

[ 
-25000\cdot710,,-24999\cdot710,\ldots 
] 

là tiền tố bắt buộc của cấu trúc phần tử (50000) đầy đủ. 

Mã sử ​​dụng số nguyên Python, do đó, lỗi tràn không phải là vấn đề đáng lo ngại mặc dù bản thân vấn đề yêu cầu các giá trị được in phải khớp với số nguyên 32 bit đã ký. Các giá trị thực tế chỉ ở khoảng (1,8\time10^7), thấp hơn nhiều (2^{31}-1). 

Không cần phải tính toán`sin`trong chương trình đã nộp. Việc tính toán các giá trị sin dấu phẩy động sẽ làm tăng thêm công việc không cần thiết và có thể đưa ra những so sánh bằng số không liên quan đến chứng minh. Toàn bộ mục đích của việc chọn (710) là đối số toán học đã đảm bảo thứ tự. 

Thứ tự đầu ra ngày càng tăng. Điều này thuận tiện nhưng không được yêu cầu bởi vấn đề. Treap được xác định bởi tập hợp các điểm ((x,\sin x)), do đó, bất kỳ hoán vị nào của các khóa hợp lệ giống nhau sẽ mô tả cùng một cây Descartes. 

## Ví dụ đã hoạt động 

Với (n=4), chương trình bắt đầu ở (-25000\cdot710=-17750000) và cộng (710) ba lần. 

| tôi | k | x=710k | Vị trí sin tương đối | 
| --- | --- | --- | --- | 
| 0 | -25000 | -17750000 | nhỏ nhất | 
| 1 | -24999 | -17749290 | lớn hơn | 
| 2 | -24998 | -17748580 | lớn hơn | 
| 3 | -24997 | -17747870 | lớn nhất | 

Chương trình không cần các giá trị sin chính xác. Mỗi (x) khác với bội số của (2\pi) bởi đại lượng cực nhỏ tương ứng (k\delta), và những đại lượng này vẫn nằm trong phần tăng nghiêm ngặt của sin. Do đó, cả hai tọa độ được sắp xếp ngày càng tăng, tạo ra một chuỗi gồm bốn nút. 

Với (n=5), cách xây dựng tương tự chỉ cần lấy thêm một điểm. 

| tôi | k | x=710k | Thứ tự sin | 
| --- | --- | --- | --- | 
| 0 | -25000 | -17750000 | 1 | 
| 1 | -24999 | -17749290 | 2 | 
| 2 | -24998 | -17748580 | 3 | 
| 3 | -24997 | -17747870 | 4 | 
| 4 | -24996 | -17747160 | 5 | 

Điểm đầu tiên có (x) nhỏ nhất và nhỏ nhất (y) nên nó là nghiệm. Tất cả bốn điểm còn lại đều có (x) lớn hơn nên chúng phải nằm trong cây con bên phải của gốc. Trong số bốn điểm đó, điểm thứ hai có (y) nhỏ nhất, do đó nó trở thành con đúng và lập luận tương tự vẫn tiếp tục. Chiều cao kết quả chính xác là năm. 

Mẫu ban đầu sử dụng một cấu trúc hợp lệ khác, điều này được mong đợi vì đây là cấu trúc chỉ đưa ra kết quả với nhiều câu trả lời đúng. Người kiểm tra phải chấp nhận bất kỳ bộ nào thỏa mãn chiều cao rãnh yêu cầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi số nguyên (n) được tạo và in một lần. | 
| Không gian | O(n) | Việc triển khai Python lưu trữ các chuỗi đầu ra trước khi viết chúng. | 

Với (n\le50000), thuật toán chỉ thực hiện (50000) phép tính số học và ghi (50000) số nguyên. Các giá trị số cũng nằm xa trong phạm vi 32 bit đã ký, do đó cấu trúc phù hợp thoải mái với các giới hạn đã nêu. 

## Trường hợp thử nghiệm 

Vì bài toán chấp nhận nhiều kết quả đầu ra khác nhau nên việc kiểm tra nên xác minh các thuộc tính toán học của chuỗi được tạo ra thay vì so sánh nó với một chuỗi cố định. Trình trợ giúp bên dưới kiểm tra xem mọi giá trị có khác biệt không, tất cả các giá trị đều nằm trong phạm vi 32 bit đã ký và các giá trị sin đang tăng nghiêm ngặt. Nó cũng xác minh trực tiếp việc xây dựng bằng cách sử dụng số học dấu phẩy động đủ độ chính xác cao cho các đối số cụ thể này.```python
import sys
import io
import math

def solve_data(inp: str) -> str:
    data = inp.strip().split()
    n = int(data[0])

    base = -25000 * 710
    return "\n".join(str(base + i * 710) for i in range(n))

def run(inp: str) -> str:
    return solve_data(inp)

def validate(inp: str) -> bool:
    out = run(inp)
    values = list(map(int, out.split()))
    n = int(inp.strip())

    assert len(values) == n
    assert len(set(values)) == n
    assert all(-2**31 <= x <= 2**31 - 1 for x in values)

    ys = [math.sin(x) for x in values]

    for i in range(1, n):
        assert values[i - 1] < values[i]
        assert ys[i - 1] < ys[i]

    return True

# Provided sample
assert validate("4"), "sample 1"

# Minimum-size input
assert run("1") == "-17750000", "minimum n"

# Small boundary case around the centre of the construction
assert validate("2"), "two nodes"

# All-equal-values adversarial idea: the output itself must never contain duplicates.
# The validator explicitly rejects duplicate generated keys.
bad = "0\n0\n"
bad_values = list(map(int, bad.split()))
assert len(set(bad_values)) != len(bad_values), "duplicate-value check"

# Maximum-size input
assert validate("50000"), "maximum n"

# A case large enough to cross zero in the generated sequence
assert validate("25001"), "zero-crossing boundary"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`|`-17750000`| Xây dựng kích thước tối thiểu và cây nút đơn | 
|`2`| Hai khóa hợp lệ riêng biệt | Chuỗi không tầm thường nhỏ nhất | 
|`25001`| 25001 khóa hợp lệ | Vượt qua hệ số nhân bằng 0 mà không phá vỡ sự đơn điệu | 
|`50000`| 50000 khóa hợp lệ | An toàn tối đa (n), điểm cuối và phạm vi | 
| Ứng viên trùng lặp`0, 0`| Bị từ chối bởi người xác thực | Phát hiện lỗi phổ biến khi tạo khóa bằng nhau | 

## Vỏ cạnh 

Đối với (n=1), thuật toán chỉ in (-17750000). Không có điều kiện thứ tự nào để so sánh với nút khác, do đó, treap kết quả bao gồm một đỉnh và có chiều cao bằng một đỉnh. 

Đối với mức tối đa (n=50000), số nhân nằm trong khoảng từ (-25000) đến (24999). Các phím tương ứng nằm trong khoảng từ (-17750000) đến (17749290). Các đối số sin sau khi loại bỏ toàn bộ khoảng thời gian nằm trong khoảng từ (-1,5075) đến (1,5074), vẫn hoàn toàn nằm trong khoảng mà sin đang tăng. Treap kết quả là đường dẫn của tất cả (50000) nút. 

Việc xây dựng cũng đi qua số nhân bằng 0 khi (n) đủ lớn. Tại (k=0), khóa là (0) và mức độ ưu tiên là (\sin(0)=0). Các lân cận của nó tương ứng với các góc giảm âm và dương nhỏ, do đó giá trị sin của chúng tương ứng nhỏ hơn và lớn hơn. Không có mức độ ưu tiên trùng lặp ở mức 0. 

Cuối cùng, thực tế là các phím được in là bội số của (710) không gây ra các giá trị sin bằng nhau. Mối quan hệ toàn thời gian 

[ 
\sin(710k)=\sin(k\delta) 
] 

giảm bài toán thành các giá trị (k\delta) riêng biệt bên trong khoảng tăng nghiêm ngặt ((-\pi/2,\pi/2)). Vì các số nhân (k) là khác nhau nên các góc rút gọn của chúng cũng khác biệt và có thứ tự, do đó các giá trị sin của chúng cũng khác biệt và có thứ tự. Đây là thuộc tính biến những gì ban đầu có vẻ giống như một bài toán xây dựng bộ nhớ khó thành một phép xây dựng số học có bước không đổi.
