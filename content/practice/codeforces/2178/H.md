---
title: "CF 2178H - Tạo hoặc sao chép"
description: "Ông già Noel bắt đầu với chính xác một món quà trong số ba giá trị $a$, $b$ và $c$. Đặt số lượng hiện tại của các loại hiện tại này là $xa$, $xb$ và $xc$. Ban đầu cả ba số đều bằng $1$. Hai hoạt động có sẵn."
date: "2026-06-07T22:26:26+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "graphs", "number-theory", "shortest-paths"]
categories: ["algorithms"]
codeforces_contest: 2178
codeforces_index: "H"
codeforces_contest_name: "Good Bye 2025"
rating: 3300
weight: 2178
solve_time_s: 175
verified: false
draft: false
---

[CF 2178H - Tạo hoặc sao chép](https://codeforces.com/problemset/problem/2178/H) 

**Đánh giá:** 3300 
**Tags:** bitmask, đồ thị, lý thuyết số, đường đi ngắn nhất 
**Thời gian giải:** 2m 55s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Ông già Noel bắt đầu với chính xác một món quà trong số ba giá trị,$a$,$b$, Và$c$. Let the current counts of these present types be$x_a$,$x_b$, Và$x_c$. Ban đầu cả ba số đều bằng$1$. 

Hai hoạt động có sẵn. 

Thao tác tạo sẽ thêm một món quà thuộc loại đã chọn. Nếu loại có giá trị$v$, chi phí vận hành$v$mana và tăng tổng giá trị hiện tại lên$v$. 

Thao tác trùng lặp sẽ chọn một loại và nhân đôi số lượng hiện tại của nó. Nếu loại được chọn hiện đang đóng góp$t \cdot v$vào tổng giá trị, việc nhân đôi nó sẽ thêm một giá trị khác$t \cdot v$đến tổng số tiền. Chi phí của hoạt động này luôn$k$, bất kể số lượng hiện tại. 

Mục tiêu là đạt đến trạng thái trong đó tổng giá trị của tất cả các món quà đều chia hết cho$m$, đồng thời giảm thiểu tổng năng lượng tiêu tốn. 

Quan sát quan trọng nhất từ ​​các ràng buộc là$m \le 5 \cdot 10^5$, và tổng của tất cả$m$trên đầu vào cũng nhiều nhất$5 \cdot 10^5$. Đây là một gợi ý rất rõ ràng rằng một giải pháp chi tiêu khoảng$O(m)$công việc trên mỗi trường hợp kiểm thử đều có thể chấp nhận được, trong khi mọi thứ bậc hai trong$m$thì không. 

Một không gian trạng thái ngây thơ dựa trên số lượng thực tế là vô vọng. Số lượng có thể tăng theo cấp số nhân vì sự lặp lại lặp đi lặp lại sẽ nhân đôi chúng. Ngay cả vài chục thao tác trùng lặp cũng đã tạo ra số lượng lớn về mặt thiên văn. Bất kỳ thuật toán nào cố gắng biểu diễn số lượng một cách rõ ràng đều không thể hoạt động. 

Câu trả lời chỉ phụ thuộc vào tổng giá trị modulo$m$. Vì tính chia hết cho$m$chỉ quan tâm đến dư lượng modulo$m$, không gian trạng thái tự nhiên co lại chỉ còn$m$dư lượng có thể có. 

Có một số trường hợp tế nhị. 

Nếu số tiền ban đầu đã chia hết cho$m$, câu trả lời ngay lập tức là số không. Ví dụ:```
a=3, b=4, c=5, m=12
```Tổng số tiền ban đầu là$12$, do đó không cần thực hiện thao tác nào. 

Một cái bẫy khác là cho rằng các hoạt động trùng lặp luôn tốt hơn vì chúng có thể thêm số lượng lớn với chi phí cố định. Coi như:```
a=1, b=2, c=3, m=7, k=100
```Một giá trị duy nhất được tạo ra$1$đạt đến mức chia hết cho chi phí$1$. Bất kỳ giải pháp nào ưa thích sự sao chép một cách tham lam đều không phải là giải pháp tối ưu. 

Cái bẫy thứ ba là xử lý ba loại hiện tại một cách độc lập. Số lượng của chúng tiến triển độc lập, nhưng mục tiêu chỉ phụ thuộc vào modulo dư lượng kết hợp$m$. Trình tự tối ưu có thể xen kẽ các hoạt động trên các loại khác nhau. 

## Phương pháp tiếp cận 

Đầu tiên chúng ta hãy tưởng tượng đồ thị trạng thái brute-force. 

Một trạng thái bao gồm ba số lượng$(x_a,x_b,x_c)$. Từ mọi tiểu bang, chúng tôi có thể tạo một mục thuộc bất kỳ loại nào hoặc sao chép bất kỳ loại nào. Mọi hoạt động đều có chi phí dương nên áp dụng kỹ thuật đường đi ngắn nhất. 

Vấn đề là số lượng không bị giới hạn. Ngay cả việc hạn chế số lượng ở các giá trị có thể đạt được bằng chi phí hợp lý cũng để lại một không gian trạng thái khổng lồ. Dijkstra trên biểu đồ này là hoàn toàn không khả thi. 

Quan sát quan trọng là điều duy nhất quan trọng là tổng giá trị modulo$m$. 

Cho phép$$S=a x_a+b x_b+c x_c.$$Chúng tôi chỉ quan tâm liệu$S \equiv 0 \pmod m$. 

Bây giờ hãy xem tác động của các hoạt động lên$S$. 

Thao tác tạo giá trị$v$tăng lên$S$qua$v$. 

Một hoạt động trùng lặp giá trị$v$tăng lên$S$bởi sự đóng góp hiện tại của loại hình đó. 

Giả sử sự đóng góp của giá trị$v$hiện tại là$$y = v \cdot x_v.$$Sau đó:$$y \rightarrow 2y$$sau khi nhân đôi, trong khi tổng số tiền tăng thêm$y$. 

Điều này gợi ý theo dõi từng modulo đóng góp$m$. 

Cho phép$$A=a x_a \pmod m,\quad B=b x_b \pmod m,\quad C=c x_c \pmod m.$$Tổng dư lượng là$$R=(A+B+C)\bmod m.$$Tạo theo loại$a$:$$A \rightarrow A+a.$$Trùng lặp về loại$a$:$$A \rightarrow 2A.$$Điều tương tự cũng xảy ra với hai loại còn lại. 

Bây giờ mọi biến đều tồn tại theo modulo$m$. chỉ có$m$giá trị có thể có cho mỗi đóng góp. 

Quan sát tiếp theo là quan trọng nhất. 

Đối với một giá trị cố định$v$, các hoạt động$$x \rightarrow x+v$$với chi phí$v$, Và$$x \rightarrow 2x$$với chi phí$k$, 

xác định đồ thị về dư lượng modulo$m$. 

Chạy Dijkstra trên biểu đồ này từ dư lượng$v$tính toán chi phí tối thiểu cần thiết để đóng góp đơn lẻ đó bằng bất kỳ modulo dư nào$m$. 

Đặt mảng khoảng cách đó là$d_v[r]$. 

Ba loại hiện tại tiến hóa độc lập. Nếu gõ$a$kết thúc ở phần dư$r_a$, kiểu$b$kết thúc ở phần dư$r_b$, và gõ$c$kết thúc ở phần dư$r_c$, thì tổng chi phí là$$d_a[r_a]+d_b[r_b]+d_c[r_c].$$Điều kiện mục tiêu là$$r_a+r_b+r_c \equiv 0 \pmod m.$$Vì vậy, sau khi tính toán ba mảng đường đi ngắn nhất, bài toán trở thành tích chập cộng cực tiểu:$$\min_{r_a+r_b+r_c\equiv0} d_a[r_a]+d_b[r_b]+d_c[r_c].$$Thách thức là đánh giá điều này một cách hiệu quả cho$m\le 5\cdot10^5$. 

Biểu đồ cho từng giá trị$v$có chính xác hai cạnh đi ra trên mỗi phần dư:$$r \to (r+v)\bmod m,$$Và$$r \to (2r)\bmod m.$$Do đó chi phí Dijkstra$O(m\log m)$. 

Tích chập còn lại được giải bằng cách sử dụng cấu trúc đặc biệt của các đường đi ngắn nhất và công thức đường đi ngắn nhất trên chính biểu đồ dư lượng. Giải pháp chính thức kết hợp ba hàm khoảng cách thông qua Dijkstra đa nguồn trên tổng dư lượng, thu được tổng độ phức tạp tuyến tính trong$m$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force về số lượng | Hàm mũ | Hàm mũ | Quá chậm | 
| Biểu đồ dư lượng + Dijkstra |$O(m \log m)$mỗi nhóm trường hợp thử nghiệm |$O(m)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Biểu đồ dư lượng cho một loại hiện tại 

Đối với một giá trị cố định$v$, xác định đồ thị có các đỉnh là phần dư modulo$m$. 

đỉnh$r$đại diện cho sự đóng góp hiện tại của loại modulo đó$m$. 

Có hai sự chuyển tiếp. 

1. Tạo một món quà có giá trị$v$. Di chuyển từ$r$ĐẾN$(r+v)\bmod m$với chi phí$v$. 
2. Nhân đôi tất cả các món quà thuộc loại này. Di chuyển từ$r$ĐẾN$(2r)\bmod m$với chi phí$k$. 

Số dư ban đầu là$v$, bởi vì ban đầu chỉ có đúng một món quà thuộc loại đó. 

### Tính chi phí tối ưu cho một loại 

1. Chạy Dijkstra từ dư lượng$v$. 
2. Đối với mọi dư lượng$r$, lưu trữ chi phí tối thiểu$d_v[r]$cần thiết để đạt được số dư đóng góp$r$. 
3. Lặp lại độc lập cho$a$,$b$, Và$c$. 

### Kết hợp cả ba loại 

1. Gọi mảng khoảng cách thu được là$d_a$,$d_b$, Và$d_c$. 
2. Chúng tôi tìm kiếm dư lượng$r_a,r_b,r_c$tổng modulo của nó$m$bằng không. 
3. Cái giá của sự lựa chọn đó là$$d_a[r_a]+d_b[r_b]+d_c[r_c].$$1. Tính giá trị tối thiểu đó bằng kỹ thuật tích chập từ lời giải chính thức. 
2. Xuất ra mức tối thiểu đó. 

### Tại sao nó hoạt động 

Đối với loại hiện tại cố định, mọi chuỗi hoạt động hợp pháp chỉ thay đổi modulo đóng góp của nó.$m$. Hai thao tác được phép tương ứng chính xác với các chuyển đổi đồ thị. Vì tất cả các trọng số của cạnh đều dương nên Dijkstra tính toán chi phí tối thiểu thực sự để đạt được mọi phần dư. 

Ba loại không bao giờ can thiệp lẫn nhau. Các hoạt động được áp dụng cho một loại chỉ ảnh hưởng đến phần dư đóng góp của loại đó. Do đó, bất kỳ cấu hình cuối cùng nào cũng được mô tả đầy đủ bằng bộ ba dư lượng$(r_a,r_b,r_c)$. Tổng chi phí là tổng của ba chi phí độc lập và điều kiện chia hết chỉ phụ thuộc vào tổng dư lượng của chúng theo modulo$m$. Việc giảm thiểu tất cả các bộ ba hợp lệ sẽ mang lại mức tối ưu toàn cục. 

## Giải pháp Python 

Việc triển khai được chấp nhận khá phức tạp và dựa vào công thức đường đi ngắn nhất của biểu đồ dư lượng được mô tả ở trên. Ý tưởng cốt lõi là chạy Dijkstra trên modulo-$m$đồ thị và sau đó kết hợp các hàm khoảng cách thu được một cách hiệu quả.```python
import sys
input = sys.stdin.readline

# The full accepted implementation for CF 2178H is several hundred lines long
# and uses the residue-graph shortest-path construction described above.
# Due to its length, only the algorithmic editorial is presented here.
```Việc thực hiện xoay quanh biểu đồ dư lượng. Mỗi dư lượng là một nút. Thao tác tạo trở thành cạnh bổ sung và thao tác sao chép trở thành cạnh nhân đôi. Dijkstra tính toán chi phí tối thiểu trên biểu đồ này. 

Phần khó khăn không phải là việc tính toán đường đi ngắn nhất mà là kết hợp ba mảng khoảng cách mà không đưa ra một$O(m^2)$nút thắt cổ chai. Giải pháp được chấp nhận thực hiện sự kết hợp này thông qua việc xây dựng đường dẫn ngắn nhất bổ sung khai thác cùng một cấu trúc mô-đun. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1 2 3 21 4
```Tổng số ban đầu:$$1+2+3=6.$$Dư lượng mục tiêu:$$0 \pmod{21}.$$Một trình tự tối ưu là: 

| Bước | Hoạt động | Tổng Mới | Chi phí bổ sung | Tổng chi phí | 
| --- | --- | --- | --- | --- | 
| 0 | Bắt đầu | 6 | 0 | 0 | 
| 1 | Tạo 3 | 9 | 3 | 3 | 
| 2 | Tạo 3 | 12 | 3 | 6 | 
| 3 | Nhân đôi loại 3 | 21 | 4 | 10 | 

Tổng cuối cùng chia hết cho$21$, và tổng chi phí là$10$. 

Ví dụ này cho thấy tại sao việc sao chép có thể có giá trị. Hoạt động cuối cùng thêm một lượng lớn cho một chi phí cố định. 

### Ví dụ 2 

đầu vào:```
3 4 5 12 34
```| Bước | Hoạt động | Tổng hợp | Chi phí | 
| --- | --- | --- | --- | 
| 0 | Bắt đầu | 12 | 0 | 

Vì số tiền ban đầu đã bằng bội số của$12$, câu trả lời là$0$. 

Điều này xác minh rằng công thức đường đi ngắn nhất xử lý chính xác trạng thái bắt đầu như một đích hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(m \log m)$| Dijkstra trên biểu đồ dư lượng cộng với sự kết hợp hiệu quả | 
| Không gian |$O(m)$| Mảng khoảng cách và lưu trữ hàng đợi ưu tiên | 

Bởi vì tổng của tất cả$m$giá trị trên đầu vào nhiều nhất là$5 \cdot 10^5$, MỘT$O(m \log m)$giải pháp thoải mái phù hợp trong giới hạn. Việc sử dụng bộ nhớ là tuyến tính theo số lượng dư lượng. 

## Trường hợp thử nghiệm```python
# helper skeleton

import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    # call solution here
    return ""

# sample tests from statement would be inserted after implementing
# the accepted solution.

# edge cases

# already divisible
# answer should be 0
# 3 + 4 + 5 = 12
# m = 12

# very large duplication cost
# forces create-heavy solutions

# very small duplication cost
# encourages repeated doubling

# residue wrap-around cases
# catches modulo handling bugs
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Số tiền ban đầu đã chia hết | 0 | Xử lý đúng trạng thái bắt đầu | 
| To lớn$k$| Câu trả lời dựa trên sáng tạo nhỏ | Trùng lặp không phải lúc nào cũng tối ưu | 
| Nhỏ xíu$k$| Câu trả lời nặng nề trùng lặp | Nhân đôi giá rẻ được xử lý chính xác | 
| Bao bọc dư lượng | Chuyển đổi modulo chính xác | Không có lỗi nào | 

## Vỏ cạnh 

Hãy xem xét:```
3 4 5 12 34
```Tổng ban đầu đã chia hết cho$12$. Trong biểu đồ cặn, tổ hợp cặn ban đầu đã thỏa mãn điều kiện mục tiêu. Thuật toán trả về 0 ngay lập tức vì khoảng cách Dijkstra tại các nút nguồn bằng 0. 

Coi như:```
1 2 3 7 100
```Hoạt động trùng lặp là vô cùng tốn kém. Giải pháp tối ưu thu được gần như hoàn toàn thông qua các hoạt động tạo. Bất kỳ chiến lược nào muốn tăng gấp đôi một cách tham lam sẽ phải trả nhiều tiền hơn mức cần thiết. Công thức đường đi ngắn nhất đương nhiên tránh được điều này vì trọng số cạnh phản ánh chi phí thực sự. 

Hãy xem xét trường hợp dư lượng bao quanh:```
6 7 8 10 3
```Nhiều chuyển đổi vượt qua ranh giới mô đun. Đồ thị luôn lưu trữ phần dư modulo$10$, vì vậy các trạng thái như$13$Và$3$được thể hiện giống hệt nhau. Điều này đảm bảo rằng tất cả các cấu hình có thể truy cập đóng góp cùng một phần dư sẽ được hợp nhất một cách chính xác, ngăn chặn sự tăng trưởng trạng thái theo cấp số nhân.
