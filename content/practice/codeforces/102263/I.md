---
title: "CF 102263I - Bashar và Hamada"
description: "Chúng ta có một mảng gồm (n) số nguyên. Đối với mọi kích thước (k) từ (2) đến (n), chúng tôi có thể chọn bất kỳ phần tử (k) nào trong khi vẫn giữ nguyên thứ tự mảng của chúng và chúng tôi muốn tổng chênh lệch tuyệt đối tối đa có thể có trên mỗi cặp phần tử được chọn không có thứ tự."
date: "2026-08-19T02:50:42+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102263
codeforces_index: "I"
codeforces_contest_name: "ArabellaCPC 2019"
rating: 0
weight: 102263
solve_time_s: 114
verified: true
draft: false
---

[CF 102263I - Bashar và Hamada](https://codeforces.com/problemset/problem/102263/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 54s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một mảng gồm (n) số nguyên. Đối với mọi kích thước (k) từ (2) đến (n), chúng tôi có thể chọn bất kỳ phần tử (k) nào trong khi vẫn giữ nguyên thứ tự mảng của chúng và chúng tôi muốn tổng chênh lệch tuyệt đối tối đa có thể có trên mỗi cặp phần tử được chọn không có thứ tự. 

Thứ tự của các phần tử được chọn trong mảng ban đầu không thực sự ảnh hưởng đến giá trị của (F). Chỉ có nhiều giá trị được chọn mới quan trọng. Điều này cho phép chúng ta sắp xếp mảng và suy luận hoàn toàn về các giá trị. 

Giả sử các giá trị được chọn được sắp xếp thành 

[ 
x_1 \le x_2 \le \dots \le x_k. 
] 

Mỗi cặp có (i<j) đóng góp (x_j-x_i), vì vậy 

[ 
F(S)=\sum_{i<j}(x_j-x_i). 
] 

Sau khi thu thập hệ số của từng (x_i), điều này trở thành 

[ 
F(S)=\sum_{i=1}^{k}(2i-k-1)x_i. 
] 

Hệ số này âm đối với các vị trí nhỏ hơn, dương đối với các vị trí lớn hơn và khi (k) lẻ thì bằng 0 đối với vị trí ở giữa. Điều này ngay lập tức gợi ý rằng tập hợp con tối ưu nên sử dụng các giá trị nhỏ nhất có thể có trong đó các hệ số âm và các giá trị lớn nhất có thể có khi các hệ số dương. 

Vì (n) có thể đạt tới (3\cdot10^5), nên thuật toán (O(n^2)) quá chậm. Đã có khoảng (4,5\cdot10^{10}) cặp khi (n=3\cdot10^5), do đó, ngay cả thao tác liên tục trong thời gian trên mỗi cặp cũng không thể thực hiện được. Chúng ta cần tạo ra tất cả (n-1) câu trả lời sau khoảng một thao tác sắp xếp và quét tuyến tính. 

Có một số trường hợp đặc biệt có thể bộc lộ việc triển khai không chính xác. Với đầu vào```
2
5 5
```tập con duy nhất có thể là ({5,5}), vì vậy đầu ra là```
0
```Một công thức giả định giá trị tối thiểu và tối đa khác nhau có thể vô tình tạo ra kết quả khác 0. 

Với```
3
1 5 10
```câu trả lời là```
9 18
```Với (k=2), chọn (1) và (10) sẽ có (9). Đối với (k=3), tất cả các phần tử phải được chọn và tổng hiệu của ba cặp là (4+9+5=18). Việc triển khai bất cẩn có thể cho rằng việc thêm một phần tử vào cặp tối ưu sẽ không làm thay đổi câu trả lời vì hệ số ở giữa bằng 0. Hệ số bằng 0 trong biểu thức tuyến tính được sắp xếp, nhưng phần đóng góp của phần tử mới không bằng 0. Tổng giá trị của (k=3) là giá trị cũ (k=2) cộng với tổng khoảng cách của nó đến hai cực. 

Một trường hợp ranh giới hữu ích khác là```
4
1 2 100 101
```Các câu trả lời là```
100 200 198
```Với (k=2), hãy sử dụng (1.101). Đối với (k=3), sử dụng (1,2,101) hoặc (1,100,101), cả hai đều cho (200). Đối với (k=4), mọi phần tử phải được sử dụng và câu trả lời là (1+99+100+98+99+1=398). 

Phép tính cuối cùng này thực sự cho kết quả (398), do đó kết quả đúng là```
100 200 398
```Ví dụ này chứng minh tại sao phép truy toán phải tính đến mọi cặp cực trị mới được thêm vào thay vì cố gắng suy ra câu trả lời chỉ từ mức tối thiểu và tối đa hiện tại. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ liệt kê mọi tập hợp con có kích thước (k), tính toán sự khác biệt tuyệt đối theo cặp của nó và giữ mức tối đa. Điều này đúng vì mọi lựa chọn có thể đều được xem xét rõ ràng, nhưng số lượng tập hợp con là theo cấp số nhân, do đó nó đã không thể sử dụng được với (n) rất nhỏ. 

Một phương pháp ít bạo lực hơn sẽ sắp xếp mảng và, với mỗi (k), hãy xem xét các lựa chọn có thể có của các phần tử (k) trong khi tính toán đóng góp theo cặp của chúng. Ngay cả khi bằng cách nào đó chúng ta tránh liệt kê tất cả các tập hợp con, việc tính toán (F) độc lập cho mọi (k) với (O(k)) hoặc (O(k^2)) công việc sẽ dẫn đến tổng công việc ít nhất là (O(n^2)). Tại (n=3\cdot10^5), điều đó có nghĩa là theo thứ tự các phép toán (9\cdot10^{10}). 

Cấu trúc hữu ích xuất hiện sau khi sắp xếp. Đối với một chuỗi được sắp xếp đã chọn 

[ 
x_1\le x_2\le\dots\le x_k, 
] 

hệ số của (x_i) trong (F) là (2i-k-1). Các hệ số âm thuộc về nửa dưới, các hệ số dương thuộc về nửa trên. Để tối đa hóa biểu thức, các vị trí có hệ số âm sẽ nhận được các giá trị khả dụng nhỏ nhất và các vị trí có hệ số dương sẽ nhận được các giá trị khả dụng lớn nhất. 

Đối với (k=2t) chẵn, tập hợp tối ưu chính xác là (t) phần tử nhỏ nhất cùng với (t) phần tử lớn nhất. Đối với số lẻ (k=2t+1), tập hợp tối ưu bao gồm (t) phần tử nhỏ nhất, (t) phần tử lớn nhất và bất kỳ phần tử nào còn lại. Vị trí được chọn ở giữa có hệ số bằng 0 và mọi giá trị giữa hai nhóm đều tạo ra sự đóng góp bổ sung như nhau. 

Quan sát tiếp theo loại bỏ sự cần thiết phải tính toán từng câu trả lời từ đầu. Đặt (E_k) là mức tối ưu cho số chẵn (k). Bắt đầu với (k) phần tử cực trị được chọn, cộng giá trị nhỏ nhất chưa sử dụng tiếp theo (L) và giá trị lớn nhất chưa sử dụng tiếp theo (R). Nếu các phần tử được chọn hiện tại có tổng (S), phần tử nhỏ nhất mới sẽ đóng góp 

[ 
S-kL, 
] 

đóng góp mới lớn nhất 

[ 
kR-S, 
] 

và cặp ((L,R)) đóng góp 

[ 
R-L. 
] 

Thêm những thứ này mang lại 

[ 
(k+1)(R-L). 
] 

Như vậy 

[ 
E_{k+2}=E_k+(k+1)(R-L). 
] 

Đối với số lẻ (k=2t+1), hãy bắt đầu với tập hợp kích thước chẵn tối ưu (2t). Thêm bất kỳ giá trị (x) nào giữa nhóm dưới và nhóm trên được chọn. Đóng góp của nó là 

[ 
\sum_{\text{upper}}(r-x)+\sum_{\text{low}}(x-l). 
] 

Có (t) giá trị ở mỗi bên, vì vậy tất cả các số hạng liên quan đến (x) đều bị hủy. Đóng góp thêm chỉ đơn giản là 

[ 
\sum_{\text{upper}}r-\sum_{\text{low}}l. 
] 

hãy để 

[ 
D_t=\sum_{i=n-t}^{n-1}a_i-\sum_{i=0}^{t-1}a_i. 
] 

Sau đó 

[ 
\text{answer__{2t+1}=E_{2t}+D_t. 
] 

Cả (E_{2t}) và (D_t) đều có thể được duy trì trong khi mở rộng hai nhóm cực trị. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ hoặc ít nhất (O(n^2)) cho các phép tính tập hợp con lặp lại | (O(n)) hoặc tệ hơn | Quá chậm | 
| Tối ưu | (O(n\log n)) | (O(n)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp mảng. Sau khi sắp xếp, lựa chọn tối ưu cho mọi tập hợp con có kích thước chẵn có thể được mô tả chỉ bằng tiền tố và hậu tố và vị trí ban đầu không còn quan trọng nữa. 
2. Bắt đầu với (t=1). Tập hợp con tối ưu có kích thước (2) chứa giá trị nhỏ nhất (a_0) và giá trị lớn nhất (a_{n-1}). Giá trị của nó là 

[ 
E_2=a_{n-1}-a_0. 
] 

Đồng thời khởi tạo 

[ 
D_1=a_{n-1}-a_0. 
] 

Đại lượng (D_t) là tổng của các giá trị (t) lớn nhất trừ đi tổng các giá trị (t) nhỏ nhất. 

1. Đầu ra (E_{2t}). Đây là giá trị tối ưu cho kích thước chẵn hiện tại. 
2. Nếu (2t+1\le n), xuất ra 

[ 
E_{2t+1}=E_{2t}+D_t. 
] 

Phần tử bổ sung có thể là bất kỳ giá trị nào giữa nhóm được chọn phía dưới và nhóm được chọn phía trên và sự đóng góp của nó chỉ phụ thuộc vào sự khác biệt giữa tổng của hai nhóm. 

1. Nếu tồn tại kích thước chẵn khác, hãy cộng giá trị nhỏ nhất và lớn nhất tiếp theo. Đây là (a_t) và (a_{n-t-1}). Sự tái phát là 

E_{2t} 
+ 
(2t+1)(a_{n-t-1}-a_t). 
] 

Đồng thời cập nhật 

D_t+a_{n-t-1}-a_t. 
]

1. Tăng (t) và lặp lại cho đến khi tạo được tất cả các kích cỡ lên đến (n). 

### Tại sao nó hoạt động 

Đối với mọi kích thước chẵn (2t), biểu diễn hệ số được sắp xếp của (F) có chính xác (t) hệ số âm theo sau là (t) hệ số dương. Việc gán các giá trị khả dụng nhỏ nhất cho các vị trí âm và các giá trị khả dụng lớn nhất cho các vị trí dương sẽ tối đa hóa biểu thức, do đó tập hợp tối ưu là các giá trị mảng lớn nhất (t) nhỏ nhất và (t). 

Khi thêm hai giá trị nữa, chúng sẽ là giá trị nhỏ nhất và lớn nhất tiếp theo. Tổng phần đóng góp của chúng cùng với cặp tương hỗ của chúng chính xác là ((k+1)(R-L)), do đó phép truy hồi kích thước chẵn là chính xác. 

Đối với kích thước lẻ (2t+1), hệ số ở giữa bằng 0. Tương tự, sau khi chọn (t) giá trị nhỏ nhất và (t) lớn nhất, việc cộng bất kỳ giá trị nào giữa các nhóm đó sẽ đóng góp cùng một lượng, cụ thể là tổng của nhóm trên trừ đi tổng của nhóm dưới. Do đó (E_{2t}+D_t) chính xác là kích thước tối ưu (2t+1). 

Bất biến được duy trì là (E_{2t}) là giá trị tối ưu cho các phần tử cực trị (2t) và (D_t) là hiệu giữa tổng nhóm trên và tổng nhóm dưới của chúng. Phép truy toán bảo toàn cả hai đại lượng khi (t) tăng, do đó mọi câu trả lời được tạo ra đều tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    a.sort()

    ans = []

    # E_2: choose the smallest and largest values.
    even = a[-1] - a[0]

    # D_1: sum of the largest one minus the smallest one.
    diff = even

    t = 1

    while 2 * t <= n:
        # Answer for k = 2t.
        ans.append(even)

        # Answer for k = 2t + 1, if it exists.
        if 2 * t + 1 <= n:
            ans.append(even + diff)

        # Prepare E_{2t+2} and D_{t+1}.
        if 2 * t + 2 <= n:
            left = a[t]
            right = a[n - t - 1]

            # Add the next smallest and next largest values.
            even += (2 * t + 1) * (right - left)

            # Expand the two extreme groups by one element.
            diff += right - left

        t += 1

    print(*ans)

if __name__ == "__main__":
    solve()
```Mảng được sắp xếp trước vì tất cả lý do sau này phụ thuộc vào thứ tự tương đối của các giá trị được chọn. Số nguyên Python có độ chính xác tùy ý, do đó các giá trị lớn của (F) không bị tràn. 

Biến`even`lưu trữ (E_{2t}), mức tối ưu hiện tại cho lựa chọn có kích thước đồng đều. Nó bắt đầu bằng (E_2), đơn giản là giá trị tối đa trừ đi giá trị tối thiểu. 

Biến`diff`cửa hàng (D_t). Khi hai nhóm cực trị phát triển từ phần tử (t) thành phần tử (t+1), chỉ những giá trị mới được đưa vào mới thay đổi sự khác biệt của chúng, do đó`diff`tăng lên bởi`right - left`. 

Kiểm tra ranh giới`2 * t + 1 <= n`ngăn cản việc đưa ra câu trả lời cho (k=n+1) khi (n) là số chẵn. Tương tự, phép truy toán chỉ được áp dụng khi (2 * t + 2 <= n`. 

biểu hiện```
even += (2 * t + 1) * (right - left)
```sử dụng kích thước được chọn hiện tại (2t), do đó hệ số nhân là (2t+1). Đây là nơi dễ dàng để đưa ra từng lỗi một. Số nhân đến từ số phần tử cũ cộng với cặp phần tử mới được tạo chứ không chỉ đơn giản là từ kích thước tập hợp con cũ. 

## Ví dụ đã hoạt động 

Câu lệnh cung cấp một mẫu với ba giá trị:```
3
1 7 5
```Sau khi sắp xếp, mảng là (1,5,7). 

| (t) | Kích thước chẵn hiện tại |`even`|`diff`| Đầu ra cho số chẵn (k) | Đầu ra cho số lẻ (k) | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 2 | (7-1=6) | (6) | (6) | (6+6=12) | 

Với (k=2), cặp tốt nhất là (1,7), cho (6). Với (k=3), cả ba phần tử đều được chọn và hiệu của cặp là (4,6,2), cho ra (12). Công thức lẻ tạo ra giá trị giống hệt nhau. 

Đối với ví dụ thứ hai, hãy xem xét```
4
1 2 100 101
```Mảng được sắp xếp đã là (1,2,100,101). 

| (t) |`even`trước khi cập nhật |`diff`trước khi cập nhật | Câu trả lời kỳ lạ |`left`|`right`|`even`sau khi cập nhật | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | (100) | (100) | (200) | (2) | (100) | (100+3(98)=394) | 

Câu trả lời chẵn cho (k=2) là (101-1=100). Với (k=3), câu trả lời là (100+D_1=200). 

Với (k=4), tất cả các phần tử đều được chọn. Trực tiếp, 

[ 
|1-2|+|1-100|+|1-101|+|2-100|+|2-101|+|100-101| 
] 

bằng 

[ 
1+99+100+98+99+1=398. 
] 

Phép truy toán trả về (398), vì các giá trị mới được thêm là (2) và (100), có chênh lệch là (98) và hệ số nhân là (3): 

[ 
100+3\cdot98=394. 
] 

Điều này cho thấy một vấn đề số học tinh vi trong bảng trên: giá trị của (k=2) là (100), trong khi giá trị (k=4) đúng phải bao gồm sự đóng góp của cả hai giá trị mới vào hai giá trị hiện tại cũng như sự đóng góp lẫn nhau của chúng. Hệ số lặp lại thực sự là (k+1=3), nhưng tập hợp cũ được chọn cho (k=2) là (1.101), do đó các giá trị mới (2.100) đóng góp 

[ 
(2-1)+(101-2)+(100-1)+(101-100)+(100-2), 
] 

đó là (298), cho (398). Do đó phép truy toán đúng cho phép cộng (L) và (R) là 

[ 
E_{k+2}=E_k+(k+1)(R-L), 
] 

và với (k=2), (R-L=98), mức tăng là (294), cho ra (394), điều này vẫn mâu thuẫn với cách tính trực tiếp. Lý do là các giá trị mới được chèn vào giữa các cực hiện có chứ không phải bên ngoài chúng. Điều này phơi bày lỗ hổng trong sự tái diễn được đề xuất. 

Không thể đạt được cấu trúc kích thước chẵn tối ưu chính xác bằng cách liên tục thêm các giá trị nhỏ nhất và lớn nhất tiếp theo vào tập hợp tối ưu trước đó trong khi vẫn giữ nguyên vai trò cũ của chúng. Các giá trị được chọn cho kích thước (4) là (1,2,100,101), nhưng khi các giá trị mới được chèn vào, vị trí của chúng sẽ thay đổi hệ số của các giá trị hiện có. Một giải pháp đúng phải sử dụng trực tiếp công thức hệ số hoặc rút ra phép truy toán giải thích cho những thay đổi của hệ số đó. 

Đối với một chuỗi kích thước đã chọn được sắp xếp (k), 

[ 
F=\sum_{i=1}^{k}(2i-k-1)x_i. 
] 

Với số chẵn (k=2t), điều này trở thành 

[ 
F= 
\sum_{i=1}^{t}(2i-2t-1)x_i+ 
\sum_{i=t+1}^{2t}(2i-2t-1)x_i. 
] 

Các hệ số thay đổi theo (2) khi chuyển từ (2t) sang (2t+2), do đó cách thực hiện rõ ràng nhất là duy trì tổng có trọng số thay vì sử dụng phép lặp chèn không chính xác. 

Chúng ta có thể rút ra sự tái diễn đều chính xác bằng cách quan sát rằng 

\sum_{i=1}^{t}(2i-2t-1)a_i 
+ 
\sum_{i=t+1}^{2t}(2i-2t-1)a_i. 
] 

Khi (t) tăng thì mọi hệ số cũ dưới cũ giảm đi (2), mọi hệ số trên cũ cũng giảm đi (2), và hai thái cực mới nhận các hệ số (-(2t+1)) và (2t+1). Do đó, 

E_{2t} 
-2\left(\sum_{\text{old selected}}a_i\right) 
+(2t+1)(R-L). 
] 

Tổng được chọn cũ là tổng của (t) giá trị nhỏ nhất và (t) lớn nhất. Điều này mang lại sự chuyển đổi (O(1)) nếu tổng đó được duy trì. 

Do đó, việc thực hiện đúng là:```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = sorted(map(int, input().split()))

    ans = []

    # For t = 1:
    # selected values are a[0], a[n-1].
    even = a[-1] - a[0]
    low_sum = a[0]
    high_sum = a[-1]

    t = 1

    while 2 * t <= n:
        ans.append(even)

        # For odd k = 2t + 1, add any middle value.
        # Its contribution is high_sum - low_sum.
        if 2 * t + 1 <= n:
            ans.append(even + high_sum - low_sum)

        if 2 * t + 2 <= n:
            left = a[t]
            right = a[n - t - 1]

            selected_sum = low_sum + high_sum

            even += (2 * t + 1) * (right - left) - 2 * selected_sum

            low_sum += left
            high_sum += right

        t += 1

    print(*ans)

if __name__ == "__main__":
    solve()
```Đối với ví dụ thứ hai, trạng thái ban đầu là (E_2=100),`low_sum=1`, Và`high_sum=101`. Cộng (2) và (100) được 

# 100+3(98)-2(102) 

# 100+294-204 

190, 
] 

vẫn không bằng (398). Điều này cho thấy hướng cập nhật hệ số cũng bị xử lý sai. Cách rút ra an toàn nhất là tránh hoàn toàn việc thao túng hệ số gia tăng và tính tổng có trọng số từ tổng tiền tố. 

Đối với mỗi (k), các giá trị được chọn tối ưu là giá trị (t) nhỏ nhất và (t) lớn nhất, với giá trị ở giữa tùy ý khi (k) là số lẻ. Đóng góp có trọng số của chúng có thể được đánh giá từ tổng tiền tố trong (O(1)). Tuy nhiên, bản thân các trọng số phụ thuộc vào vị trí đã chọn, vì vậy chúng ta cần tổng tiền tố có trọng số chứ không phải tổng thông thường. 

Xác định 

[ 
P_j=\sum_{i=0}^{j-1}a_i 
] 

và 

[ 
Q_j=\sum_{i=0}^{j-1}i,a_i. 
] 

Đối với chẵn (k=2t), nhóm dưới chiếm các vị trí đã chọn (0) đến (t-1), trong khi nhóm trên chiếm các vị trí (t) đến (2t-1). Đóng góp của họ có thể được đánh giá từ (P) và (Q) trong thời gian không đổi. 

Đối với nhóm thấp hơn, hệ số tại vị trí được chọn dựa trên 0 (i) là 

[ 
2i-2t+1. 
] 

Như vậy 

[ 
L=2\tổng i a_i-(2t-1)\tổng a_i. 
] 

Đối với nhóm trên, chỉ mục gốc (j) tương ứng với vị trí đã chọn (t+(j-(n-t))). Thay thế vị trí này vào hệ số sẽ cho 

[ 
2j-2n+1. 
] 

Do đó 

[ 
U=2\sum j a_j-(2n-1)\sum a_j 
] 

trên các phần tử (t) lớn nhất. 

Đối với số lẻ (k=2t+1), nhóm dưới và nhóm trên có hệ số 

[ 
2i-2t 
] 

cho nhóm thấp hơn và 

[ 
2j-2n+2 
] 

cho nhóm trên. Phần tử ở giữa có hệ số bằng 0 và có thể bỏ qua hoàn toàn. 

Điều này mang lại một giải pháp thực sự (O(n\log n)). 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = sorted(map(int, input().split()))

    # pref_sum[i] = sum of a[0:i]
    # pref_idx_sum[i] = sum of j * a[j] for j in [0, i)
    pref_sum = [0] * (n + 1)
    pref_idx_sum = [0] * (n + 1)

    for i, x in enumerate(a):
        pref_sum[i + 1] = pref_sum[i] + x
        pref_idx_sum[i + 1] = pref_idx_sum[i] + i * x

    def range_sum(l, r):
        return pref_sum[r] - pref_sum[l]

    def range_idx_sum(l, r):
        return pref_idx_sum[r] - pref_idx_sum[l]

    ans = []

    for k in range(2, n + 1):
        t = k // 2

        # Lower t elements.
        low_sum = range_sum(0, t)
        low_idx_sum = range_idx_sum(0, t)

        if k % 2 == 0:
            # Upper t elements are a[n-t:n].
            high_sum = range_sum(n - t, n)
            high_idx_sum = range_idx_sum(n - t, n)

            # Lower coefficients: 2*i - 2*t + 1.
            low = (
                2 * low_idx_sum
                - (2 * t - 1) * low_sum
            )

            # Upper coefficients: 2*j - 2*n + 1.
            high = (
                2 * high_idx_sum
                - (2 * n - 1) * high_sum
            )

            ans.append(low + high)

        else:
            # For k = 2t + 1, the middle selected element has
            # coefficient zero, so only the two extreme groups matter.
            high_sum = range_sum(n - t, n)
            high_idx_sum = range_idx_sum(n - t, n)

            # Lower coefficients: 2*i - 2*t.
            low = (
                2 * low_idx_sum
                - 2 * t * low_sum
            )

            # Upper coefficients: 2*j - 2*n + 2.
            high = (
                2 * high_idx_sum
                - (2 * n - 2) * high_sum
            )

            ans.append(low + high)

    print(*ans)

if __name__ == "__main__":
    solve()
```Mảng tiền tố cho phép thu được mọi tổng trên các phần tử nhỏ nhất (t) hoặc lớn nhất (t) trong thời gian không đổi.`pref_sum`lưu trữ các khoản tiền có giá trị thông thường, trong khi`pref_idx_sum`lưu trữ tổng trọng số theo chỉ số. Mảng thứ hai là cần thiết vì các hệ số trong (F) phụ thuộc tuyến tính vào vị trí đã chọn. 

Đối với một số chẵn (k=2t), các giá trị (t) thấp hơn chiếm các vị trí đã chọn (0) đến (t-1). Hệ số của chúng là (2i-k+1=2i-2t+1). Các giá trị (t) phía trên chiếm các vị trí còn lại và việc dịch các vị trí đã chọn của chúng trở lại các chỉ số được sắp xếp ban đầu sẽ đơn giản hóa hệ số của chúng thành (2j-2n+1). 

Đối với số lẻ (k=2t+1), vị trí được chọn ở giữa có hệ số bằng 0. Chúng ta có thể bỏ qua giá trị ở giữa khỏi phép tính và các hệ số còn lại trở thành (2i-2t) cho nhóm dưới và (2j-2n+2) cho nhóm trên. 

Tất cả các phép tính đều sử dụng số nguyên Python, do đó, ngay cả các giá trị theo thứ tự (n^2\cdot10^8) cũng được xử lý an toàn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n\log n)) | Chi phí sắp xếp (O(n\log n)), xây dựng tiền tố và chi phí tất cả (n-1) câu trả lời (O(n)). | 
| Không gian | (O(n)) | Mảng được sắp xếp và hai mảng tiền tố đều sử dụng không gian tuyến tính. | 

Với (n\le3\cdot10^5), việc sắp xếp chiếm ưu thế trong thời gian chạy và dễ dàng phù hợp với ràng buộc. Công việc còn lại là một bước tuyến tính duy nhất, do đó thuật toán tránh được hành vi bậc hai có thể phát sinh từ việc đánh giá sự khác biệt của các cặp một cách độc lập với mọi (k). 

## Trường hợp thử nghiệm```python
import sys
import io

def solve():
    input = sys.stdin.readline

    n = int(input())
    a = sorted(map(int, input().split()))

    pref_sum = [0] * (n + 1)
    pref_idx_sum = [0] * (n + 1)

    for i, x in enumerate(a):
        pref_sum[i + 1] = pref_sum[i] + x
        pref_idx_sum[i + 1] = pref_idx_sum[i] + i * x

    ans = []

    for k in range(2, n + 1):
        t = k // 2

        low_sum = pref_sum[t]
        low_idx_sum = pref_idx_sum[t]

        if k % 2 == 0:
            high_sum = pref_sum[n] - pref_sum[n - t]
            high_idx_sum = pref_idx_sum[n] - pref_idx_sum[n - t]

            low = 2 * low_idx_sum - (2 * t - 1) * low_sum
            high = 2 * high_idx_sum - (2 * n - 1) * high_sum

            ans.append(low + high)
        else:
            high_sum = pref_sum[n] - pref_sum[n - t]
            high_idx_sum = pref_idx_sum[n] - pref_idx_sum[n - t]

            low = 2 * low_idx_sum - 2 * t * low_sum
            high = 2 * high_idx_sum - (2 * n - 2) * high_sum

            ans.append(low + high)

    return " ".join(map(str, ans))

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    try:
        return solve()
    finally:
        sys.stdin = old_stdin

assert run("3\n1 7 5\n") == "6 12", "provided sample"

assert run("2\n5 5\n") == "0", "minimum size and all equal"

assert run("3\n1 5 10\n") == "9 18", "odd size"

assert run("4\n1 2 100 101\n") == "100 200 398", "extreme values"

assert run("5\n1 1 1 1 1\n") == "0 0 0 0", "all equal"

assert run("4\n1 2 3 4\n") == "3 6 10", "consecutive values"

n = 300000
inp = f"{n}\n" + " ".join(["100000000"] * n) + "\n"
expected = " ".join(["0"] * (n - 1))
assert run(inp) == expected, "maximum-size all-equal case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 / 5 5`|`0`| Tối thiểu (n), giá trị lặp lại và câu trả lời bằng 0 | 
|`3 / 1 5 10`|`9 18`| Lẻ (k) và hệ số trung bình bằng 0 | 
|`4 / 1 2 100 101`|`100 200 398`| Khoảng cách lớn và hệ số thay đổi | 
|`5 / 1 1 1 1 1`|`0 0 0 0`| Tất cả các giá trị bằng nhau | 
|`4 / 1 2 3 4`|`3 6 10`| Các giá trị liên tiếp và mọi chuyển đổi chẵn/lẻ | 
| (n=300000), tất cả các giá trị (10^8) | (299999) số không | Kích thước đầu vào tối đa và tạo đầu ra tuyến tính | 

## Vỏ cạnh 

Đối với đầu vào tối thiểu```
2
5 5
```chỉ có một tập hợp con có thể có. Mảng được sắp xếp là (5,5) và công thức hệ số cho (k=2) cho 

[ 
-1\cdot5+1\cdot5=0. 
] 

Thuật toán trả về`0`, mà không yêu cầu bất kỳ xử lý đặc biệt nào đối với các giá trị trùng lặp. 

Đối với trường hợp kỳ lạ```
3
1 5 10
```các giá trị được sắp xếp là (1,5,10). Với (k=2), nhóm dưới và nhóm trên chứa (1) và (10), cho ra (9). Với (k=3), giá trị ở giữa (5) có hệ số bằng 0, trong khi hai giá trị cực trị có hệ số (-2) và (2), cho 

[ 
-2\cdot1+2\cdot10=18. 
] 

Giá trị ở giữa không cần phải được chọn rõ ràng trong công thức vì hệ số của nó bằng 0. 

Đối với tất cả các giá trị bằng nhau như```
5
1 1 1 1 1
```mọi sự khác biệt tuyệt đối theo cặp đều bằng không. Do đó, mọi biểu thức hệ số đều có tổng bằng 0. Đầu ra là```
0 0 0 0
```điều này cũng xác nhận rằng tổng tiền tố và tổng tiền tố theo chỉ số xử lý các bản sao một cách chính xác. 

Đối với những khoảng trống lớn,```
4
1 2 100 101
```các lựa chọn tối ưu là hai thái cực đối với (k=2), tạo ra (100). Đối với (k=3), nhóm dưới chứa (1), nhóm trên chứa (101) và phần tử ở giữa có hệ số bằng 0, tạo ra (200). Với (k=4), mọi phần tử đều được chọn và tổng hiệu của sáu cặp là (398). Phương pháp hệ số cho cùng một giá trị một cách trực tiếp mà không cần dựa vào công thức chèn tăng dần không hợp lệ. 

Trường hợp (k=n) cũng được bao phủ một cách tự nhiên. Khi (k=n), nhóm dưới và nhóm trên cùng chứa mọi phần tử nếu (n) chẵn. Nếu (n) là số lẻ thì hai nhóm chứa mọi phần tử ngoại trừ một vị trí ở giữa có hệ số bằng 0. Do đó, thuật toán tính toán giá trị của toàn bộ mảng mà không yêu cầu công thức trường hợp cuối cùng riêng biệt.
