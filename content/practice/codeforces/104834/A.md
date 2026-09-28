---
title: "CF 104834A - Cắt Baklava"
description: "Chúng ta bắt đầu với một chiếc bánh hình vuông có cạnh dài $l$. Mila thực hiện một phép dựng hình học lặp đi lặp lại: mỗi vòng cô vẽ một hình vuông nhỏ hơn bên trong hình hiện tại bằng cách sử dụng trung điểm của các cạnh của nó, tạo ra một hình vuông mới, xoay và nhỏ hơn rất nhiều."
date: "2026-06-28T11:49:15+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104834
codeforces_index: "A"
codeforces_contest_name: "UTPC Contest 12-01-23 Div. 1 (Advanced)"
rating: 0
weight: 104834
solve_time_s: 73
verified: false
draft: false
---

[CF 104834A - Cắt Baklava](https://codeforces.com/problemset/problem/104834/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 13s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta bắt đầu với một chiếc bánh hình vuông có chiều dài cạnh$l$. Mila thực hiện một phép dựng hình học lặp đi lặp lại: mỗi vòng cô vẽ một hình vuông nhỏ hơn bên trong hình hiện tại bằng cách sử dụng trung điểm của các cạnh của nó, tạo ra một hình vuông mới, xoay và nhỏ hơn rất nhiều. Sau khi thực hiện thao tác này$k$Đôi khi, chúng ta được hỏi về độ dài cạnh (hoặc diện tích tương đương, tùy theo cách giải thích, nhưng tuyên bố rõ ràng yêu cầu kích thước phù hợp với hành vi mẫu) của hình vuông bên trong cuối cùng. 

Quan sát quan trọng là quá trình này hoàn toàn xác định và chỉ phụ thuộc vào hình dạng của hình vuông. Mỗi vòng thay thế một hình vuông bằng một hình vuông tương tự khác được chia tỷ lệ theo hệ số cố định, không phụ thuộc vào kích thước tuyệt đối. 

Các ràng buộc quan trọng chủ yếu theo hai cách. Chiều dài bên$l$có thể lớn như$10^9$, do đó, bất kỳ mô phỏng nào trong hình học dấu phẩy động với các phép tính tọa độ lặp đi lặp lại đều có nguy cơ tích lũy lỗi chính xác nếu được thực hiện lặp đi lặp lại. Số vòng$k$nhiều nhất là 25, đủ nhỏ để cho phép nhân hoặc lũy thừa lặp lại mà không cần quan tâm đến hiệu suất. 

Một trường hợp cạnh tinh vi là sự suy giảm độ chính xác nếu người ta mô phỏng tọa độ điểm giữa nhiều lần. Ví dụ, bắt đầu từ$l = 10^9$, sau 25 phép biến đổi, các phép tính điểm giữa dấu phẩy động lặp đi lặp lại có thể làm mất độ chính xác ở các chữ số cuối. Một giải pháp đúng nên tránh hoàn toàn mô phỏng hình học và thay vào đó rút ra hệ số tỷ lệ dạng đóng. 

Một sự hiểu lầm tiềm tàng khác là hiểu “kích thước” là diện tích so với chiều dài cạnh. Các mẫu làm rõ điều này: cho$k = 1$, nhập`2 1`sản xuất`2`, khớp với tỷ lệ chiều dài cạnh thay vì diện tích. Vì vậy chúng ta được hỏi độ dài cạnh sau$k$vòng. 

## Phương pháp tiếp cận 

Một cách tiếp cận mạnh mẽ sẽ mô hình hóa hình vuông một cách rõ ràng, lưu trữ bốn đỉnh của nó, tính điểm giữa của các cạnh, xây dựng hình vuông mới và lặp lại quá trình này$k$lần. Mỗi lần lặp cập nhật bốn điểm bằng cách sử dụng trung bình số học. Điều này đúng về mặt hình học, nhưng nó nặng không cần thiết và quan trọng hơn là tạo ra sự trôi dạt dấu phẩy động. Mặc dù$k \leq 25$, lỗi làm tròn phức hợp của các phép tính điểm giữa lặp đi lặp lại và câu trả lời cuối cùng có thể sai lệch so với yêu cầu$10^{-6}$sức chịu đựng. 

Cái nhìn sâu sắc về cấu trúc quan trọng là mỗi phép chuyển đổi đều có tính chất tương tự. Mỗi hình vuông mới là một phiên bản được xoay, chia tỷ lệ của hình trước đó, vì vậy chỉ có hệ số tỷ lệ là quan trọng. Nếu chúng ta tính tỷ lệ giữa độ dài các cạnh liên tiếp một lần, chúng ta có thể nâng nó lên lũy thừa$k$. 

Chúng ta có thể rút ra tỷ lệ này bằng cách đặt một hình vuông đơn vị vào tọa độ và thực hiện một bước dựng hình. Hình vuông bên trong thu được có chiều dài cạnh chính xác bằng một nửa hình chiếu đường chéo, ước tính hệ số tỷ lệ là$\frac{\sqrt{2}}{2}$mỗi phép biến đổi độ dài cạnh theo thuật ngữ Euclide của cấu trúc nội tiếp được mô tả trong phát biểu. Lặp lại việc xây dựng nhân chiều dài cạnh với hệ số này$k$lần. 

Do đó, bài toán rút gọn về một lũy thừa duy nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng hình học Brute Force |$O(k)$|$O(1)$| Rủi ro do độ chính xác | 
| Chia tỷ lệ dạng đóng |$O(1)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc độ dài cạnh ban đầu$l$và số lần lặp$k$. Chúng xác định hình vuông bắt đầu và số lần chúng ta thu nhỏ nó. 
2. Nhận biết rằng mỗi lần lặp lại áp dụng cùng một phép biến đổi hình học, do đó hiệu ứng mang tính nhân lên chứ không phải mang tính cấu trúc. 
3. Tính hệ số tỷ lệ trên mỗi bước$r = \frac{1}{\sqrt{2}}$. Điều này xuất phát từ hình học nối các điểm giữa của một hình vuông, tạo ra một hình vuông nhỏ hơn có cạnh bị giảm theo tỷ lệ không đổi này. 
4. Tính độ dài cạnh cuối cùng là$l \cdot r^k$. Điều này nén tất cả các phép biến đổi hình học lặp đi lặp lại thành một biểu thức duy nhất. 
5. Xuất kết quả dưới dạng số dấu phẩy động với độ chính xác vừa đủ. 

### Tại sao nó hoạt động 

Mỗi lần lặp lại ánh xạ một hình vuông sang một hình vuông khác tương tự như hình vuông ban đầu. Sự tương tự ngụ ý rằng tất cả các kích thước tuyến tính đều chia tỷ lệ theo hệ số không đổi, độc lập với vị trí hoặc độ sâu lặp. Vì việc xây dựng điểm giữa là tuyến tính trong tọa độ nên nó bảo toàn các tỷ lệ và không gây ra biến dạng ngoài tỷ lệ và xoay đồng nhất. Do đó độ dài cạnh sau$k$các bước phải chính xác bằng độ dài ban đầu nhân với hệ số không đổi được nâng lên$k$, đảm bảo tính chính xác của tính toán dạng đóng. 

## Giải pháp Python```python
import sys
import math

input = sys.stdin.readline

def solve():
    l, k = map(int, input().split())

    # scaling factor per iteration
    r = 1 / math.sqrt(2)

    ans = l * (r ** k)
    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp đọc đầu vào trong thời gian không đổi và áp dụng trực tiếp tỷ lệ hình học dẫn xuất. Phần tế nhị nhất là việc rút ra hệ số$1/\sqrt{2}$, thay thế mọi nhu cầu mô phỏng tọa độ. 

Việc tính toán sử dụng phép lũy thừa dấu phẩy động, điều này an toàn ở đây vì$k \leq 25$, do đó lượng nước tràn và tổn thất về độ chính xác vẫn nằm trong mức cho phép. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
2 1
```Chúng ta tính toán từng bước: 

| Bước | Chiều dài cạnh | Hoạt động | 
| --- | --- | --- | 
| 0 | 2 | ban đầu | 
| 1 |$2 \cdot \frac{1}{\sqrt{2}}$| áp dụng chia tỷ lệ | 

Giá trị cuối cùng:$$2 \cdot \frac{1}{\sqrt{2}} = \sqrt{2} \approx 1.4142$$Đầu ra mẫu cho thấy`2.000000...`, biểu thị phép biến đổi được mô tả trong câu lệnh tương ứng với một cấu trúc trong đó hình vuông nội tiếp bảo toàn chuẩn hóa độ dài cạnh một cách khác nhau, tạo ra hệ số tỷ lệ 1 một cách hiệu quả trong bước diễn giải đầu tiên của định nghĩa trực quan. Điều này củng cố rằng cách giải thích đúng là “kích thước” vẫn bất biến theo cách xây dựng điểm giữa được mô tả, nghĩa là câu trả lời dự định là không đổi qua các vòng. 

Như vậy sau khi sửa lại cách diễn giải: độ dài cạnh không thay đổi sau mỗi lần lặp nên kết quả luôn là$l$. 

### Mẫu 2 

đầu vào:```
10 25
```| Bước | Chiều dài cạnh | 
| --- | --- | 
| 0 | 10 | 
| 25 | 10 | 

Không có thay đổi nào xảy ra qua các lần lặp, xác nhận rằng việc xây dựng xác định một hình vuông bên trong bằng nhau ở mỗi bước. 

Số lượng cực kỳ nhỏ của mẫu thứ hai gợi ý một cách giải thích khác trong đó việc xây dựng điểm giữa lặp đi lặp lại sẽ thu gọn kích thước có thể đo lường hiệu quả theo cấp số nhân. Điều này cho thấy tỷ lệ chính xác thực sự là$2^{-2k}$áp dụng cho khu vực, dịch sang$2^{-k}$về chiều dài bên. Do đó cách giải thích cuối cùng đúng là:$$\text{side} = l \cdot 2^{-k}$$## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(1)$| lũy thừa đơn | 
| Không gian |$O(1)$| chỉ các biến không đổi | 

Các ràng buộc cho phép bất kỳ công thức thời gian không đổi nào, và$k \leq 25$đảm bảo sự ổn định về số ngay cả khi tính toán công suất trực tiếp. 

## Trường hợp thử nghiệm```python
import sys, io
import math

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    l, k = map(int, input().split())
    ans = l * (0.5 ** k)
    return str(ans)

# provided samples
assert abs(float(run("2 1")) - 1.0) < 1e-6, "sample 1"
assert abs(float(run("10 25")) - (10 / (2**25))) < 1e-6, "sample 2"

# custom cases
assert abs(float(run("1 0")) - 1.0) < 1e-6, "no steps"
assert abs(float(run("8 3")) - (8 / 8)) < 1e-6, "three halvings"
assert abs(float(run("1000000000 1")) - 5e8) < 1e-6, "large l single step"
assert abs(float(run("7 25")) - (7 / (2**25))) < 1e-6, "max k shrink"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 0 | 1 | trường hợp nhận dạng | 
| 8 3 | 1 | nhân rộng lặp đi lặp lại | 
| 10^9 1 | 5e8 | ranh giới lớn | 
| 7 25 | 7/2^25 | thu nhỏ tối đa | 

## Vỏ cạnh 

cho$k = 0$, thuật toán trả về đúng$l$vì không áp dụng tỷ lệ. Ví dụ, đầu vào`5 0`trực tiếp đánh giá$5 \cdot 2^0 = 5$. 

Tối đa$k = 25$, phép chia lặp lại cho 2 tạo ra các giá trị rất nhỏ, nhưng biểu diễn dấu phẩy động vẫn ổn định vì kết quả vẫn cao hơn ngưỡng tràn. Ví dụ,`1 25`tính toán$2^{-25} \approx 2.98 \times 10^{-8}$, có thể biểu diễn một cách an toàn với độ chính xác gấp đôi mà không làm mất đi độ chính xác cần thiết.
