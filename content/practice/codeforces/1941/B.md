---
title: "CF 1941B - Rudolf và 121"
description: "Chúng ta được cho một mảng các số nguyên không âm. Trong một thao tác, chúng ta chọn một chỉ số $i$ sao cho $2 le i le n-1$, và"
date: "2026-05-27T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "dp", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1941
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 933 (Div. 3)"
rating: 1000
weight: 1941
solve_time_s: 167
verified: true
draft: false
---

[CF 1941B - Rudolf và 121](https://codeforces.com/problemset/problem/1941/B) 

**Đánh giá:** 1000 
**Tags:** vũ lực, dp, tham lam, toán học 
**Thời gian giải:** 2m 47s 
**Đã xác minh:** có 

## Giải pháp 
#Hiểu vấn đề 

Chúng ta được cho một mảng các số nguyên không âm. Trong một thao tác, chúng tôi chọn một chỉ mục$i$như vậy$2 \le i \le n-1$và giảm ba phần tử liên tiếp theo cách sau:$$a_{i-1} \mathrel{-}= 1,\quad a_i \mathrel{-}= 2,\quad a_{i+1} \mathrel{-}= 1$$Thao tác này có thể được sử dụng nhiều lần trên bất kỳ chỉ mục hợp lệ nào. Mục đích là để xác định xem có thể giảm toàn bộ mảng về toàn số 0 hay không. 

Chi tiết quan trọng là mọi thao tác chỉ trừ đi các giá trị. Chúng tôi không bao giờ được phép tăng một phần tử. Vì vậy, khi một phần tử trở thành số âm thì quá trình này không hợp lệ. Một giải pháp đúng phải xác định liệu có tồn tại một số chuỗi thao tác biến đổi mảng thành chính xác tất cả các số 0 mà không yêu cầu giá trị âm hay không. 

Các ràng buộc đủ lớn nên hiệu quả là vấn đề quan trọng: 

| Ràng buộc | Hàm ý | 
| --- | --- | 
|$t \le 10^4$| Chúng ta phải xử lý nhiều trường hợp thử nghiệm một cách hiệu quả. | 
|$\sum n \le 2 \cdot 10^5$| MỘT$O(n)$hoặc$O(n \log n)$giải pháp cho mỗi trường hợp thử nghiệm là an toàn. | 
|$a_i \le 10^9$| Chúng tôi không thể mô phỏng từng hoạt động một nếu số lượng của chúng quá lớn. | 
| Giới hạn thời gian: 2 giây | Tổng công việc trên tất cả các trường hợp thử nghiệm phải ở mức gần tuyến tính. | 
| Giới hạn bộ nhớ: 256 MB | Việc lưu trữ mảng và một vài biến phụ là hoàn toàn ổn. | 

Một mô phỏng đơn giản áp dụng nhiều lần các thao tác sẽ thất bại vì một phần tử có thể lớn bằng$10^9$, dẫn đến hàng tỷ hoạt động. 

Một số trường hợp cạnh rất dễ xử lý sai. 

### Mảng đã bằng 0 

Nếu mọi phần tử đều bằng 0 thì câu trả lời ngay lập tức là "CÓ". Việc triển khai bất cẩn vẫn có thể cố gắng thực hiện các thao tác và vô tình từ chối trường hợp đó. 

### Mảng nhỏ 

Kích thước tối thiểu là$n=3$. Trong tình huống này, chỉ có thể chọn chỉ số ở giữa. Giải pháp phải xử lý chính xác các mảng trong đó tồn tại chính xác một vị trí hoạt động. 

### Giá trị âm trong quá trình xử lý tham lam 

Giả sử chúng ta cố gắng loại bỏ các giá trị từ trái sang phải. Nếu tại một thời điểm nào đó, số lượng thao tác được yêu cầu vượt quá số lượng có sẵn trong các phần tử lân cận thì một trong số chúng sẽ trở thành số âm. Điều đó ngay lập tức chứng tỏ cấu hình là không thể. 

Ví dụ:$$[1,1,1,1]$$Cố gắng loại bỏ phần tử đầu tiên yêu cầu một thao tác ở chỉ số 2, nhưng điều đó sẽ làm giảm phần tử thứ hai đi 2 và làm cho nó âm. 

### Các phần tử đuôi khác 0 

Ngay cả khi vế trái trở thành 0 thành công, hai phần tử cuối cùng có thể vẫn khác 0. Vì các thao tác ảnh hưởng đến ba vị trí liên tiếp nên khi chúng ta xử lý xong phần bên trái sẽ không có cách nào để cố định phần đuôi một cách độc lập. Một thuật toán đúng phải xác minh rõ ràng mảng cuối cùng. 

# Phương pháp tiếp cận 

## Lực lượng vũ phu 

Một ý tưởng đơn giản là liên tục tìm kiếm một phép toán hợp lệ và áp dụng nó cho đến khi không thể thực hiện được thêm phép toán nào nữa. Ví dụ: chúng tôi có thể chọn bất kỳ chỉ mục nào$i$Ở đâu:$$a_{i-1} \ge 1,\quad a_i \ge 2,\quad a_{i+1} \ge 1$$và tiếp tục trừ. 

Cách tiếp cận này đúng về mặt logic vì mọi thao tác đều giảm tổng tổng của mảng một cách nghiêm ngặt, do đó quá trình cuối cùng sẽ kết thúc. Nếu chúng ta đạt đến tất cả số không, câu trả lời là "CÓ". Nếu không thì là "KHÔNG". 

Vấn đề là hiệu quả. Hãy xem xét một mảng chứa các giá trị gần$10^9$. Số lượng hoạt động cũng có thể gần$10^9$, làm cho việc mô phỏng trực tiếp không thể thực hiện được trong thời gian giới hạn. 

Vì vậy, vũ lực là quá chậm. 

## Phương pháp tiếp cận tối ưu 

Quan sát quan trọng là cấu trúc hoạt động buộc phải thực hiện chiến lược tham lam từ trái sang phải một cách tự nhiên. 

Giả sử chúng ta hiện đang kiểm tra vị trí$i$. Hoạt động duy nhất có thể giảm$a_i$trong khi vẫn ảnh hưởng đến các phần tử ở bên phải của nó là hoạt động tập trung vào$i+1$. 

Điều đó có nghĩa là một khi chúng ta di chuyển chỉ số qua$i$, chúng tôi sẽ không bao giờ có thể sửa đổi nó nữa. Vì vậy, nếu$a_i$không bằng 0 tại thời điểm đó, lựa chọn hợp lệ duy nhất là áp dụng thao tác tại$i+1$chính xác$a_i$lần. 

Điều này tạo ra một quá trình xác định. 

Nếu chúng ta áp dụng thao tác$x=a_i$lần ở trung tâm$i+1$:$$a_i \mathrel{-}= x$$

$$a_{i+1} \mathrel{-}= 2x$$

$$a_{i+2} \mathrel{-}= x$$Từ$x=a_i$, phần tử hiện tại sẽ trở thành 0 ngay lập tức. 

Nếu một trong hai giá trị lân cận trở thành âm thì không tồn tại chuỗi hợp lệ. 

Bằng cách xử lý từ trái sang phải, chúng tôi tham lam loại bỏ từng phần tử một cách chính xác khi nó cố định vĩnh viễn. Cuối cùng, nếu tất cả các phần tử đều bằng 0 thì việc chuyển đổi có thể thực hiện được. 

## So sánh 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(\text{number of operations})$|$O(1)$| Quá chậm | 
| Tối ưu |$O(n)$|$O(1)$thêm | Đã chấp nhận | 

#Hướng dẫn thuật toán 

1. Đọc mảng. 
2. Xử lý các chỉ số từ trái sang phải, cụ thể từ$0$ĐẾN$n-3$. 
3. Tại mỗi vị trí$i$, cho phép:$$x = a_i$$Vì các hoạt động trong tương lai không thể sửa đổi$a_i$ngoại trừ thông qua hoạt động tập trung vào$i+1$, chúng ta phải loại bỏ nó ngay bây giờ. 
4. Áp dụng thao tác$x$lần về mặt khái niệm:$$a_i \mathrel{-}= x$$

$$a_{i+1} \mathrel{-}= 2x$$

$$a_{i+2} \mathrel{-}= x$$5. Sau khi cập nhật, hãy kiểm tra xem:$$a_{i+1} < 0 \quad \text{or} \quad a_{i+2} < 0$$Nếu vậy, xuất ra "KHÔNG". 
6. Tiếp tục xử lý cho đến khi lập chỉ mục$n-3$. 
7. Sau tất cả các thao tác, hãy xác minh rằng mọi phần tử đều bằng 0. 
8. Nếu toàn bộ mảng bằng 0, xuất "CÓ". Nếu không thì xuất ra "NO". 

#Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())

for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))

    possible = True

    for i in range(n - 2):
        x = a[i]

        a[i] -= x
        a[i + 1] -= 2 * x
        a[i + 2] -= x

        if a[i + 1] < 0 or a[i + 2] < 0:
            possible = False
            break

    if possible and all(v == 0 for v in a):
        print("YES")
    else:
        print("NO")
```Chương trình bắt đầu với thao tác I/O nhanh vì tổng kích thước đầu vào có thể đạt tới$2 \cdot 10^5$. 

Đối với mỗi trường hợp thử nghiệm, chúng tôi lặp lại từ trái sang phải. Tại vị trí`i`, giá trị hiện tại`a[i]`xác định chính xác có bao nhiêu thao tác phải được áp dụng. Chúng tôi lưu trữ giá trị này trong`x`. 

Các bản cập nhật:```
a[i + 1] -= 2 * x
a[i + 2] -= x
```mô phỏng thực hiện thao tác`x`lần. 

Nếu một trong hai giá trị cập nhật trở thành âm thì không thể cấu hình được nên chúng tôi sẽ kết thúc sớm. 

Cuối cùng, chúng tôi xác minh rằng mọi phần tử đều chính xác bằng 0. Việc kiểm tra cuối cùng này rất cần thiết vì hai yếu tố cuối cùng có thể vẫn dương ngay cả khi không còn hoạt động nào nữa. 

# Ví dụ đã hoạt động 

## Ví dụ 1 

đầu vào:```
5
1 3 5 5 2
```### Dấu vết 

| Bước | tôi | x = a[i] | Mảng sau khi hoạt động | 
| --- | --- | --- | --- | 
| Ban đầu | - | - | [1, 3, 5, 5, 2] | 
| 1 | 0 | 1 | [0, 1, 4, 5, 2] | 
| 2 | 1 | 1 | [0, 0, 2, 4, 2] | 
| 3 | 2 | 2 | [0, 0, 0, 0, 0] | 

Tất cả các phần tử đều trở thành số 0, vì vậy câu trả lời là:```
YES
```Dấu vết này thể hiện rõ ràng nguyên lý tham lam. Ở mỗi bước, phần tử hiện tại phải được loại bỏ ngay lập tức vì các thao tác sau này không thể ảnh hưởng đến nó nữa. 

## Ví dụ 2 

đầu vào:```
5
2 4 4 5 1
```### Dấu vết 

| Bước | tôi | x = a[i] | Mảng sau khi hoạt động | 
| --- | --- | --- | --- | 
| Ban đầu | - | - | [2, 4, 4, 5, 1] ​​| 
| 1 | 0 | 2 | [0, 0, 2, 5, 1] ​​| 
| 2 | 1 | 0 | [0, 0, 2, 5, 1] ​​| 
| 3 | 2 | 2 | [0, 0, 0, 1, -1] | 

Phần tử cuối cùng trở thành âm nên quá trình này không thể thực hiện được. 

Đầu ra:```
NO
```Ví dụ này cho thấy tại sao việc kiểm tra các giá trị âm là cần thiết. Quá trình tham lam xác định số lượng thao tác duy nhất có thể thực hiện được và nếu điều đó gây ra thâm hụt thì không có trình tự thay thế nào tồn tại. 

# Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi phần tử được xử lý một lần | 
| Không gian |$O(1)$thêm | Chỉ có một vài biến ngoài mảng đầu vào | 

Thuật toán thực hiện một lượng công việc không đổi trên mỗi chỉ mục, do đó tổng độ phức tạp trên tất cả các trường hợp thử nghiệm là:$$O\left(\sum n\right)$$Từ:$$\sum n \le 2 \cdot 10^5$$giải pháp dễ dàng phù hợp trong thời hạn. 

Thuật toán sửa đổi mảng tại chỗ và chỉ sử dụng một vài biến phụ, do đó mức sử dụng bộ nhớ bổ sung là không đổi. 

# Vỏ cạnh 

## Tất cả số không 

Hãy xem xét:```
3
0 0 0
```Vòng lặp xử lý phần tử đầu tiên với`x = 0`, nên không có gì thay đổi. Mảng cuối cùng vẫn toàn là số 0, do đó câu trả lời là "CÓ". 

Thuật toán xử lý việc này một cách tự nhiên mà không yêu cầu bất kỳ trường hợp đặc biệt nào. 

## Mảng kích thước tối thiểu 

Hãy xem xét:```
3
1 2 1
```Chỉ có một địa điểm hoạt động khả thi. 

### Dấu vết 

| tôi | x | Mảng | 
| --- | --- | --- | 
| 0 | 1 | [0, 0, 0] | 

Mảng trở thành 0 ngay lập tức, vì vậy câu trả lời là "CÓ". 

Bây giờ hãy xem xét:```
3
1 1 1
```Áp dụng một thao tác sẽ tạo ra:```
[0, -1, 0]
```Vì giá trị âm xuất hiện nên thuật toán sẽ đưa ra kết quả "KHÔNG" một cách chính xác. 

## Không thể sửa được đuôi 

Hãy xem xét:```
4
0 0 1 1
```Hai phần tử đầu tiên đã bằng 0 nên vòng lặp không thực hiện các thao tác có ý nghĩa. 

Tuy nhiên, sau khi xử lý, mảng vẫn còn:```
[0, 0, 1, 1]
```Lần kiểm tra cuối cùng không thành công vì không có thao tác nào chỉ có thể sửa đổi hai phần tử cuối cùng. Thuật toán xuất ra chính xác "KHÔNG". 

## Giá trị lớn 

Hãy xem xét:```
5
1000000000 2000000000 1000000000 0 0
```Thuật toán xử lý các số lượng lớn trong thời gian không đổi bằng cách sử dụng các cập nhật số học. Không cần mô phỏng lặp lại. 

Điều này chứng tỏ tại sao cách tiếp cận số học tham lam lại đủ hiệu quả đối với các ràng buộc.
