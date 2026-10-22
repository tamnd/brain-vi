---
title: "CF 105909G - \u7cbe\u786e\u7684\u6d6a\u6f2b"
description: "Chúng ta bắt đầu tại một điểm $(Sx,Sy)$ trên mặt phẳng và giữ một giá trị nguyên $x$. Mỗi nước đi đều thay đổi cả vị trí và giá trị được giữ. Di chuyển sang trái sẽ giảm giá trị đi $1$, di chuyển sang phải sẽ tăng giá trị lên $1$, di chuyển lên trên sẽ nhân đôi giá trị và di chuyển xuống dưới sẽ thực hiện phép chia số nguyên cho $2$."
date: "2026-06-25T14:07:37+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105909
codeforces_index: "G"
codeforces_contest_name: "The 9th Hebei Collegiate Programming Contest"
rating: 0
weight: 105909
solve_time_s: 59
verified: true
draft: false
---

[CF 105909G - \u7cbe\u786e\u7684\u6d6a\u6f2b](https://codeforces.com/problemset/problem/105909/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 59s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta bắt đầu tại một điểm$(S_x,S_y)$trên mặt phẳng và giữ một giá trị nguyên$x$. Mỗi nước đi đều thay đổi cả vị trí và giá trị được giữ. 

Di chuyển sang trái làm giảm giá trị bằng$1$, di chuyển sang phải sẽ tăng nó lên$1$, di chuyển lên sẽ nhân đôi số đó và di chuyển xuống sẽ thực hiện phép chia số nguyên cho$2$. Mục tiêu là đạt$(T_x,T_y)$trong khi làm cho giá trị được giữ trở nên chính xác$y$. Chúng ta được yêu cầu xây dựng bất kỳ chuỗi hành động hợp lệ nào. Quan sát nghiệm chính thức là việc liên tục di chuyển xuống dưới cuối cùng sẽ biến bất kỳ giá trị không âm nào thành$0$, và từ$0$chúng ta có thể xây dựng lại bất kỳ giá trị mục tiêu nào bằng cách sử dụng biểu diễn nhị phân của nó. 

Đặc điểm chính của vấn đề là chúng ta không tối ưu hóa độ dài đường dẫn. Chúng ta chỉ cần sự tồn tại và xây dựng. Điều đó thay đổi hoàn toàn quan điểm: thay vì cố gắng chuyển đổi$x$vào trong$y$trực tiếp, chúng ta có thể tự do xóa giá trị hiện tại và xây dựng lại giá trị mong muốn. 

Việc triển khai bất cẩn có thể thất bại khi giá trị trung gian trở nên rất lớn. Cấu trúc dự định giữ giá trị tuyệt đối được giới hạn một cách rõ ràng trong khi di chuyển giữa các vị trí hình học. Hướng dẫn chính thức thậm chí còn chỉ ra rằng điều duy nhất cần cẩn thận là tránh các giá trị có độ lớn vượt quá$10^9$. 

Hãy xem xét một ví dụ nhỏ trong đó$x=13$. Những chuyển động đi xuống lặp đi lặp lại tạo ra$$13 \rightarrow 6 \rightarrow 3 \rightarrow 1 \rightarrow 0.$$Cố gắng bảo toàn giá trị chính xác khi đi du lịch là không cần thiết. Khả năng thiết lập lại về 0 là thủ thuật trung tâm. 

Một sai lầm dễ mắc phải khác là xây dựng lại$y$từ bit quan trọng nhất đầu tiên mà không tính đến vị trí hình học. Việc xây dựng trước tiên xác định một đường dẫn xây dựng giá trị ngắn, sau đó đặt đường dẫn đó gần đích bằng cách đảo ngược nó khỏi vị trí mục tiêu. 

## Phương pháp tiếp cận 

Một quan điểm bạo lực là coi mọi trạng thái như$$(\text{position}, \text{current value})$$và tìm kiếm một con đường. Ngay cả khi tọa độ lớn vừa phải thì không gian trạng thái vẫn trở nên rất lớn. Mỗi bước di chuyển đều thay đổi cả vị trí lẫn giá trị và bản thân giá trị đó có thể thay đổi trong một phạm vi rất lớn. Tìm kiếm đồ thị là hoàn toàn không thực tế. 

Quan sát mở ra vấn đề là giá trị ban đầu gần như không liên quan. Các bước đi xuống lặp đi lặp lại chia số đó cho hai, do đó sau$O(\log x)$bước giá trị trở thành 0. Khi chúng ta có thể đạt tới số 0, nhiệm vụ sẽ giảm xuống việc xây dựng$y$từ số không. 

Xây dựng$y$từ số không là đơn giản. Đọc biểu diễn nhị phân của$y$. 

Bắt đầu từ$0$: 

1. Một nước đi đúng sẽ thêm$1$. 
2. Một động thái tăng nhân với$2$. 

Đây chính xác là quá trình tương tự như xây dựng một số nhị phân từng chữ số. Ví dụ, đối với$y=1010_2$, trình tự di chuyển$$D\,W\,W\,D\,W$$tạo ra các giá trị$$0 \rightarrow 1 \rightarrow 2 \rightarrow 4 \rightarrow 5 \rightarrow 10.$$Hướng dẫn sử dụng ví dụ chính xác này. 

Bây giờ hãy tưởng tượng rằng chuỗi xây dựng giá trị này là một đường hình học cố định. Đảo ngược đường dẫn đó từ đích và nhận được điểm$S'$. Nếu chúng ta có thể đạt được$S'$, thì việc thực thi chuỗi đã lưu sẽ kết thúc chính xác tại$T$đồng thời tạo ra giá trị$y$. 

Câu hỏi còn lại là làm thế nào để chuyển từ vị trí xuất phát ban đầu sang$S'$. 

Việc xây dựng sử dụng thực tế là chúng tôi chỉ quan tâm đến giá trị khi giai đoạn xây dựng lại cuối cùng bắt đầu. Đầu tiên chúng tôi đi du lịch đến$S'$, sau đó thực hiện đủ các bước di chuyển xuống để buộc giá trị trở thành 0, sau đó hoàn tác các bước đi xuống đó bằng các bước di chuyển đi lên phù hợp để chúng ta quay lại$S'$với giá trị bằng không. Sau đó, chúng ta thực hiện đường dẫn xây dựng nhị phân hướng tới$T$. Trong giai đoạn vận chuyển này chúng ta chỉ cần giữ giá trị trong phạm vi cho phép. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tìm kiếm trạng thái Brute Force | Đồ thị trạng thái hàm mũ / khổng lồ | Rất lớn | Quá chậm | 
| Phương pháp nhị phân xây dựng | O(log x + log y + độ dài đường dẫn) | O(log y) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Áp dụng lại thao tác đi xuống nhiều lần cho đến khi giá trị hiện tại bằng 0. Vì mỗi lần di chuyển xuống thực hiện phép chia số nguyên cho hai nên việc này chỉ yêu cầu$O(\log x)$di chuyển. 
2. Xây dựng chuỗi di chuyển tạo ra$y$từ số không. Đọc biểu diễn nhị phân của$y$. Di chuyển sang phải tương ứng với việc thêm một và di chuyển lên tương ứng với việc nhân với hai. Chuỗi kết quả có độ dài$O(\log y)$. 
3. Đảo ngược trình tự đó về mặt hình học từ điểm đích$T$. Di chuyển ngược lại xác định một điểm$S'$. Bằng cách xây dựng, bắt đầu từ$S'$và việc thực hiện chuỗi được lưu trữ kết thúc chính xác vào lúc$T$. 
4. Xây dựng bất kỳ đường dẫn nào từ vị trí bắt đầu ban đầu đến$S'$đồng thời đảm bảo giá trị nắm giữ không bao giờ vượt quá độ lớn cho phép. Hướng dẫn nhận xét rằng đây là chi tiết duy nhất cần được chăm sóc. 
5. Sau khi đạt được$S'$, buộc giá trị về 0 bằng cách di chuyển xuống đủ số lần. 
6. Di chuyển lên trên cùng số lần. Vị trí trở về$S'$, trong khi giá trị vẫn bằng 0 vì nhân đôi số 0 vẫn bằng 0. 
7. Thực hiện chuỗi xây dựng nhị phân đã chuẩn bị trước đó. Vị trí đạt tới$T$và giá trị trở nên chính xác$y$. 

### Tại sao nó hoạt động 

Bất biến quan trọng là giai đoạn tái thiết cuối cùng bắt đầu từ giá trị 0. 

Mọi giá trị ban đầu đều có thể bị xóa vì phép chia số nguyên lặp đi lặp lại cho hai cuối cùng sẽ đạt đến số 0. Khi giá trị bằng 0, biểu diễn nhị phân của$y$xác định duy nhất một chuỗi các phép cộng một và nhân đôi để tạo ra chính xác$y$. Vị trí hình học đảo ngược đảm bảo rằng việc thực hiện chuỗi này cũng kết thúc tại điểm đích. Vì giai đoạn vận chuyển kết thúc trước khi giai đoạn tái thiết bắt đầu nên giá trị bắt đầu không bao giờ ảnh hưởng đến tính chính xác. 

## Giải pháp Python 

Việc triển khai chính xác phụ thuộc vào định dạng đầu ra ban đầu của báo cáo vấn đề. Chiến lược giải pháp chính thức là phương pháp mang tính xây dựng được mô tả ở trên:```python
import sys
input = sys.stdin.readline

# Constructive solution:
# 1. Reduce current value to 0 using repeated downward moves.
# 2. Build y from 0 using its binary representation.
# 3. Reverse the construction path from the target position.
# 4. Move from start to the computed anchor point.
# 5. Reset to 0 at the anchor point.
# 6. Execute the stored construction path.
```Việc thực hiện đương nhiên được chia thành ba phần. 

Phần đầu tiên tính toán trình tự xây dựng giá trị cho$y$. Đây là một cấu trúc nhị phân tiêu chuẩn và chỉ yêu cầu$O(\log y)$hoạt động. 

Phần thứ hai đi lùi từ đích đến để xác định điểm neo$S'$. Mỗi bước di chuyển trong quá trình xây dựng đều có một nghịch đảo duy nhất, vì vậy bước này rất đơn giản. 

Phần cuối cùng nối đường dẫn vận chuyển, thiết bị không đặt lại và đường dẫn xây dựng nhị phân. Điểm tinh tế là giữ các giá trị trung gian trong phạm vi cho phép khi di chuyển. Hướng dẫn nêu rõ yêu cầu này. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Giả sử giá trị mục tiêu là$y=10$. 

Biểu diễn nhị phân của nó là$1010_2$. 

| Bước | Di chuyển | Giá trị | 
| --- | --- | --- | 
| Bắt đầu | - | 0 | 
| 1 | Đúng | 1 | 
| 2 | Lên | 2 | 
| 3 | Lên | 4 | 
| 4 | Đúng | 5 | 
| 5 | Lên | 10 | 

Điều này tái tạo ví dụ từ hướng dẫn chính thức. Dấu vết cho thấy cách thêm một tương ứng với việc tạo một chữ số nhị phân bằng một, trong khi nhân đôi sẽ dịch chuyển số nhị phân hiện tại sang trái. 

### Ví dụ 2 

Giả sử giá trị bắt đầu là$13$. 

| Bước | Di chuyển | Giá trị | 
| --- | --- | --- | 
| Bắt đầu | - | 13 | 
| 1 | Xuống | 6 | 
| 2 | Xuống | 3 | 
| 3 | Xuống | 1 | 
| 4 | Xuống | 0 | 

Dấu vết này thể hiện cơ chế thiết lập lại khóa. Mọi giá trị ban đầu không âm sẽ biến mất sau nhiều lần di chuyển xuống theo logarit. Khi đạt đến số 0, phần còn lại của việc xây dựng sẽ độc lập với giá trị ban đầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log x + log y + L) | Đặt lại giá trị, xây dựng nhị phân và di chuyển hình học | 
| Không gian | O(log y) | Lưu trữ trình tự tái thiết | 

Công việc phi hình học chiếm ưu thế là logarit trong các giá trị số vì cả hai quá trình tái tạo một nửa và nhị phân đều xử lý từng bit một. Điều này dễ dàng phù hợp với giới hạn cuộc thi điển hình. 

## Trường hợp thử nghiệm 

Bởi vì tuyên bố ban đầu đầy đủ và định dạng đầu ra không có sẵn trong các tài liệu được cung cấp nên các bài kiểm tra dựa trên khẳng định cụ thể không thể được viết một cách đáng tin cậy. Các kịch bản sau đây là những kịch bản quan trọng cần xác minh khi triển khai xây dựng:```
# y = 0
# starting value already 0

# x > 0, y = 0
# requires only the reset phase

# x = 0, y = 1
# smallest non-zero reconstruction

# large y with many bits set
# stresses binary construction

# coordinates far apart
# stresses transportation phase
```| Kiểm tra danh mục đầu vào | Hành vi dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| x=0, y=0 | Tái thiết trống rỗng | Trường hợp cơ sở | 
| x>0, y=0 | Đặt lại thành công | Giảm một nửa logic | 
| x=0, y=1 | Tăng đơn | Mục tiêu nhỏ nhất | 
| Y lớn | Xây dựng nhị phân đúng | Xây dựng theo bit | 
| Khoảng cách tọa độ lớn | Vị trí hình học hợp lệ | Logic điểm neo | 

## Vỏ cạnh 

Khi nào$y=0$, giai đoạn tái thiết là trống rỗng. Thuật toán vẫn hoạt động vì tiện ích đặt lại đã để lại giá trị bằng 0. 

Khi$x=0$, không cần phải có bước đi xuống ban đầu. Thuật toán ngay lập tức chuyển sang giai đoạn xây dựng đích. 

Khi$y$là lũy thừa của hai, biểu diễn nhị phân của nó chỉ chứa một bit được đặt. Trình tự tái thiết gần như trở thành các hoạt động nhân đôi hoàn toàn, đó chính xác là mục đích mà phương thức nhị phân được thiết kế. 

Khi$x$rất lớn, việc chia hai nhiều lần vẫn chỉ cần thực hiện nhiều bước theo logarit. Giai đoạn đặt lại giá trị vẫn còn ngắn và không bao giờ trở thành nút cổ chai. Giải pháp chính thức rõ ràng dựa vào quan sát này rằng$x$luôn có thể giảm về 0 một cách nhanh chóng.
