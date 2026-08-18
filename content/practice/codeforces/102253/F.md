---
title: "CF 102253F - Chức năng"
description: "Chúng ta có hai hoán vị, a trên n vị trí và b trên m giá trị. Chúng ta muốn đếm các hàm f gán mọi vị trí của a giá trị từ 0 đến m - 1, tuân theo quy tắc [ f(i)=b(f(ai))."
date: "2026-08-17T21:34:44+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102253
codeforces_index: "F"
codeforces_contest_name: "2017 Chinese Multi-University Training, BeihangU Contest"
rating: 0
weight: 102253
solve_time_s: 350
verified: true
draft: false
---

[CF 102253F - Chức năng](https://codeforces.com/problemset/problem/102253/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 5 phút 50 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai hoán vị,`a`TRÊN`n`vị trí và`b`TRÊN`m`các giá trị. Chúng tôi muốn đếm các chức năng`f`phân công mọi vị trí của`a`một giá trị từ`0`bởi vì`m - 1`, tuân theo quy luật 

[ 
f(i)=b(f(a_i)). 
] 

Cách hữu ích để đọc phương trình này là di chuyển một lần dọc theo một chu kỳ`a`buộc giá trị của`f`di chuyển theo một chu kỳ của`b`theo hướng ngược lại. Nhiệm vụ là đếm tất cả các nhiệm vụ vẫn nhất quán khi mỗi chu trình được đóng lại. Câu trả lời được báo cáo theo modulo (10^9+7). Trang vấn đề ban đầu và bài xã luận của cuộc thi xác nhận công thức dựa trên chu trình này. 

Bởi vì`a`Và`b`là các hoán vị, đồ thị hàm số của chúng bao gồm toàn bộ các chu trình có hướng rời nhau. Đây là đặc tính cấu trúc làm cho bài toán trở nên dễ dàng hơn nhiều so với bài toán tổng hợp hàm số. 

Các giá trị của`n`Và`m`mỗi trường hợp có thể đạt tới (10^5) và tổng của tất cả các trường hợp thử nghiệm đạt tới (10^6). Với giới hạn một giây, mọi thứ bậc hai trong`n`hoặc`m`đã quá đắt trong trường hợp xấu nhất. Chúng ta cần một giải pháp gần tuyến tính về tổng kích thước đầu vào. May mắn thay, sau khi phân tách các hoán vị thành các chu trình, phép liệt kê số chia còn lại cũng có thể được giới hạn tuyến tính. 

Có một số trường hợp đặc biệt có thể khiến việc triển khai có vẻ hợp lý không thành công. 

Coi như```
1 2
0
1 0
```Vị trí nguồn duy nhất là một chu kỳ có độ dài`1`, trong khi`b`là một chu trình có độ dài`2`. Phương trình yêu cầu giá trị mục tiêu đã chọn phải được cố định bởi`b`, nhưng không có mục tiêu nào cố định. Câu trả lời đúng là`0`. Việc triển khai chỉ đếm tất cả các giá trị đích mà không kiểm tra độ dài chu kỳ sẽ trả về không chính xác`2`. 

Bây giờ hãy xem xét```
2 2
1 0
0 1
```Đây`a`là một chu trình có độ dài`2`, trong khi`b`gồm hai điểm cố định. Điểm cố định có thể được sử dụng cho toàn bộ chu trình nguồn, vì vậy câu trả lời là`2`. Việc triển khai bất cẩn có thể chỉ tìm kiếm các chu kỳ mục tiêu có cùng độ dài và lợi nhuận`0`, nhưng điều kiện bắt buộc là độ dài chu kỳ đích chia cho độ dài chu kỳ nguồn. 

Một trường hợp biên liên quan đến ước số bình phương là```
4 4
1 2 3 0
0 1 3 2
```Nguồn có độ dài một chu kỳ`4`. Mục tiêu có hai chu kỳ dài`1`và một chu kỳ có độ dài`2`. Cả ba chu kỳ mục tiêu đều có độ dài chia`4`, đóng góp (1+1+2=4) giá trị ban đầu có thể có. Câu trả lời là`4`. Khi liệt kê các ước số bằng cách kiểm tra`d * d <= L`, số chia`2`chỉ được tính một lần vì nó là căn bậc hai của`4`. 

Cuối cùng, một hoán vị không thể có tất cả các phần tử bằng nhau theo đúng nghĩa đen trừ khi kích thước của nó là`1`. Ví dụ,`[0]`là hoán vị duy nhất mà các phần tử đều bằng nhau. Đối với kích thước lớn hơn, phiên bản có ý nghĩa của trường hợp căng thẳng này là tất cả các chu trình đều có cùng độ dài. Sáu phần tử được chia thành ba chu kỳ 2 là một ví dụ hữu ích vì nó kiểm tra độ dài chu kỳ lặp lại và phép nhân lặp lại của cùng một hệ số. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp rất đơn giản về mặt khái niệm. Đối với mỗi một trong số (m^n) hàm có thể, hãy gán một giá trị đích cho mỗi hàm`n`vị trí nguồn và kiểm tra tất cả`n`phương trình. Điều này đúng vì mọi hàm có thể đều được xem xét và chỉ các hàm thỏa mãn mọi phương trình mới được tính. 

Vấn đề là số lượng ứng viên. Trong trường hợp xấu nhất với (n=m=10^5), vũ lực sẽ xem xét 

[ 
100000^{100000} 
] 

các chức năng khác nhau và việc kiểm tra từng ứng cử viên có thể yêu cầu đánh giá phương trình (100000). Điều đó tùy thuộc vào 

[ 
100000\cdot100000^{100000} 
] 

kiểm tra cơ bản, vượt xa mọi giới hạn khả thi. 

Quan sát quan trọng là các ràng buộc không kết hợp các vị trí nguồn tùy ý. Họ kết hợp các vị trí dọc theo chu kỳ của`a`. Giả sử một chu kỳ`a`có chiều dài`L`, bắt đầu từ một vị trí nào đó`x`: 

[ 
x,\ a(x),\ a^2(x),\ldots,a^{L-1}(x). 
]

Một lần`f(x)`được chọn thì phương trình xác định giá trị của`f`ở mọi vị trí khác trong chu kỳ này. Câu hỏi duy nhất là liệu bài tập có nhất quán khi chúng ta quay lại`x`. 

Áp dụng phương trình cho toàn bộ chu trình sẽ cho 

[ 
f(x)=b^L(f(x)). 
] 

Như vậy`f(x)`phải là một giá trị đích sẽ quay về chính nó sau khi chính xác`L`ứng dụng của`b`. Nếu giá trị mục tiêu đó nằm trong một chu kỳ`b`chiều dài`d`, thì (b^L) sẽ sửa nó chính xác khi 

[ 
d\mid L. 
] 

Giả sử`b`chứa (c_d) chu kỳ có độ dài`d`. Mỗi chu kỳ như vậy góp phần`d`những lựa chọn có thể có cho`f(x)`, bởi vì bất kỳ điều gì trong số đó`d`các phần tử thỏa mãn (b^L(y)=y). Do đó, chu trình nguồn có độ dài`L`có 

[ 
\sum_{d\mid L} d,c_d 
] 

nhiệm vụ hợp lệ. 

Các chu kỳ khác nhau`a`là độc lập. Việc chọn một giá trị cho một chu kỳ nguồn không ảnh hưởng đến bất kỳ chu kỳ nguồn nào khác, do đó câu trả lời cuối cùng là tích của các đại lượng này qua tất cả các chu kỳ của`a`. 

Nhiệm vụ còn lại là tính toán các tổng chia này một cách hiệu quả. Chúng ta có thể đếm độ dài chu kỳ của`b`một lần. Sau đó, với mỗi độ dài chu kỳ nguồn`L`, liệt kê các ước của nó trong thời gian (O(\sqrt L)). Vì độ dài chu kỳ nguồn tổng cộng là`n`, tổng công việc tìm kiếm ước số được giới hạn bởi 

[ 
\sum_i O(\sqrt{L_i}) 
\le O\left(\sqrt{k\sum_i L_i}\right) 
\le O(n), 
] 

ở đâu`k`là số chu kỳ của`a`. Bản thân quá trình phân tách chu trình mất (O(n+m)), do đó thuật toán hoàn chỉnh là (O(n+m)). Đây cũng chính là sự phức tạp mà ban biên tập cuộc thi đưa ra. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(nm^n)) | (O(n)) | Quá chậm | 
| Tối ưu | (O(n+m)) | (O(n+m)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Phân hủy hoán vị`b`thành các chu kỳ rời rạc và đếm xem mỗi độ dài có thể có bao nhiêu chu kỳ. Cho phép`cnt[d]`là số chu kỳ có độ dài`d`. 

Chúng ta chỉ cần số chu kỳ của mỗi độ dài chứ không phải các phần tử thực tế bên trong các chu kỳ đó, bởi vì mọi phần tử của chu kỳ đích hợp lệ đều có giá trị bắt đầu hợp lệ như nhau. 
2. Phân hủy hoán vị`a`thành các chu kỳ rời rạc và ghi lại độ dài của chúng. 

Đối với chu trình nguồn, tất cả các giá trị hàm được xác định bằng cách chọn ảnh của một vị trí. Điều này làm giảm vấn đề đếm từ việc gán`n`các giá trị độc lập để gán một giá trị bắt đầu cho mỗi chu kỳ nguồn. 
3. Đối với chu trình nguồn có độ dài`L`, tính toán 

[ 
cách(L)=\sum_{d\mid L}d\cdot cnt[d]. 
] 

Độ dài chu kỳ mục tiêu`d`có thể sử dụng được chính xác khi`d`chia rẽ`L`. Nó góp phần`d`các lựa chọn vì bất kỳ phần tử nào của nó đều có thể là hình ảnh của vị trí nguồn đã chọn. 
4. Liệt kê các ước của`L`bằng cách kiểm tra số nguyên`d`từ`1`thông qua (\lfloor\sqrt L\rfloor). 

Khi`d`chia rẽ`L`, cả hai`d`Và`L // d`là các ước số. Nếu chúng khác nhau, hãy thêm cả hai đóng góp. Nếu như`d * d == L`, chỉ thêm`d`một lần. 
5. Nhân`ways(L)`vào câu trả lời cho mỗi chu kỳ nguồn và giảm modulo (10^9+7). 

Phép nhân hợp lệ vì các chu trình nguồn là độc lập. Mỗi hàm toàn cục tương ứng duy nhất với một lựa chọn hợp lệ cho mỗi chu kỳ nguồn. 

### Tại sao nó hoạt động 

Lấy bất kỳ chu kỳ nào của`a`với chiều dài`L`và chọn một vị trí`x`trên đó. Phương trình xác định xác định tất cả các giá trị khác trên chu trình nguồn đó từ`f(x)`. Sau khi tuân theo toàn bộ chu trình nguồn, tính nhất quán yêu cầu (b^L(f(x))=f(x)). Một giá trị trong chu kỳ mục tiêu có độ dài`d`thỏa mãn điều này chính xác khi`d`chia rẽ`L`. Do đó số lựa chọn cho chu trình nguồn này chính xác là (\sum_{d\mid L}d,cnt[d]). 

Mỗi hàm hợp lệ được xác định duy nhất bởi các lựa chọn của nó trên các chu kỳ riêng biệt của`a`và các lựa chọn được thực hiện trên các chu kỳ khác nhau không bao giờ tương tác với nhau. Nhân số lượng lựa chọn cho tất cả các chu trình nguồn sẽ tính mọi hàm hợp lệ chính xác một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def cycle_lengths(p):
    n = len(p)
    seen = [False] * n
    lengths = []

    for start in range(n):
        if seen[star]()
```
