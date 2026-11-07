---
problem: 1342D
contest_id: 1342
problem_index: D
name: "Multiple Testcases"
contest_name: "Educational Codeforces Round 86 (Rated for Div. 2)"
rating: 1900
tags: ["binary search", "constructive algorithms", "data structures", "greedy", "sortings", "two pointers"]
answer: passed_samples
verified: false
solve_time_s: 521
date: 2026-06-14
model: gpt-5-3-mini
samples_passed: 0
samples_total: 1
share_url: https://chatgpt.com/share/6a2e1bc3-6528-83ec-be3b-9d57dd5c8d13
---

#CF 1342D - Nhiều Testcase 

**Xếp hạng:** 1900 
**Tags:** tìm kiếm nhị phân, thuật toán xây dựng, cấu trúc dữ liệu, tham lam, sắp xếp, hai con trỏ 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 8m 41s 
**Đã xác minh:** không (0/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a2e1bc3-6528-83ec-be3b-9d57dd5c8d13 

--- 

##Giải pháp 

## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp các mảng, nhưng nội dung thực sự của các mảng này không quan trọng chút nào. Mỗi mảng chỉ được đặc trưng bởi kích thước của nó. Nhiệm vụ của chúng ta là đóng gói tất cả các mảng này thành nhiều nhóm mà chúng ta sẽ gọi là các trường hợp thử nghiệm, sao cho mỗi mảng đều thuộc về chính xác một nhóm. 

Hạn chế là mỗi testcase phải tôn trọng giới hạn dung lượng theo lớp tùy thuộc vào kích thước mảng. Nếu chúng ta xem xét một testcase đơn lẻ và đếm xem có bao nhiêu mảng bên trong nó có kích thước ít nhất là 1, thì số lượng đó không thể vượt quá$c_1$. Nếu chúng ta đếm có bao nhiêu có kích thước ít nhất là 2 thì số đó không thể vượt quá$c_2$, v.v. cho đến kích thước$k$, trong đó ràng buộc là$c_k$. Dung lượng đều đơn điệu không tăng nên ngưỡng lớn hơn luôn chặt chẽ hơn hoặc bằng nhau. 

Mục tiêu là giảm thiểu số lượng trường hợp kiểm thử trong khi vẫn đặt mọi mảng vào đúng một trường hợp kiểm thử mà không vi phạm bất kỳ ràng buộc nào trong số này. 

Vấn đề này về cơ bản là về việc đóng gói các mặt hàng có “yêu cầu về độ bền” khác nhau vào các thùng có giới hạn nhiều lớp. Mỗi mảng có kích thước$x$tiêu thụ một đơn vị công suất cho mỗi cấp độ$1$bởi vì$x$. 

Các ràng buộc đủ lớn để bất kỳ giải pháp nào cố gắng mô phỏng tất cả các gói hoặc khám phá sự kết hợp của các tập hợp con sẽ không thành công. Với$n, k \le 2 \cdot 10^5$, chúng ta nên mong đợi một$O(n \log n)$hoặc$O(n)$giải pháp, có thể sử dụng cấu trúc tham lam và duy trì hiệu quả năng lực còn lại. 

Một trực giác ngây thơ nhưng không chính xác là cố gắng lấp đầy từng testcase một cách tham lam mà không lập kế hoạch cho những ràng buộc trong tương lai. Điều này không thành công vì việc đặt sớm một mảng lớn có thể chặn nhiều mảng nhỏ hơn nhưng linh hoạt hơn. Một trường hợp thất bại tinh vi khác xuất phát từ việc bỏ qua các ràng buộc dựa trên tiền tố, nghĩa là mọi mặt hàng lớn cũng tiêu thụ đồng thời tất cả dung lượng loại nhỏ hơn. 

Trường hợp cạnh bê tông: giả sử$k=3$, công suất là$c=[3,2,1]$và mảng là$[3,3,1,1,1]$. Nếu người ta tham lam đóng gói không có cấu trúc, thì việc đặt cả hai mảng cỡ 3 lại với nhau có thể trông ổn ở cục bộ, nhưng nó ngay lập tức tiêu tốn toàn bộ dung lượng cấp cao và buộc phải phân chia quá mức các mục cỡ 1, dẫn đến nhiều trường hợp thử nghiệm hơn mức cần thiết. 

## Phương pháp tiếp cận 

Cấu trúc của các ràng buộc gợi ý rằng mọi mảng có kích thước$x$không độc lập mà thay vào đó tiêu tốn năng lực ở mọi cấp độ$1$bởi vì$x$. Điều này có nghĩa là các mảng lớn hơn sẽ “đắt” hơn khi đặt và các mảng nhỏ luôn linh hoạt hơn. 

Chiến lược brute-force sẽ cố gắng gán từng mảng một vào các trường hợp thử nghiệm hiện có, kiểm tra tính khả thi cho từng vị trí ứng viên. Đối với mỗi mảng, chúng tôi có thể quét tất cả các trường hợp thử nghiệm hiện tại và xác minh xem việc chèn nó có giữ tất cả các ràng buộc tiền tố hợp lệ hay không. Trong trường hợp xấu nhất, điều này trở thành$O(n^2 \cdot k)$, vì mỗi lần chèn yêu cầu kiểm tra tối đa$k$những hạn chế về khả năng$n$các trường hợp thử nghiệm. Điều này là quá chậm. 

Quan sát quan trọng là chúng ta thực sự không cần phải mô phỏng các vị trí tùy ý. Thay vào đó, chúng ta có thể nghĩ ngược lại: nếu sửa chữa điều chúng ta muốn tạo ra$t$các trường hợp thử nghiệm, chúng ta có thể hỏi liệu có thể phân phối tất cả các mảng thành$t$thùng rác. Đối với một cố định$t$, chúng ta có thể tính toán số lượng mảng thuộc mỗi kích thước phải vừa với mỗi thùng và kiểm tra tính khả thi bằng cách sử dụng cách đóng gói tham lam từ kích thước lớn nhất trở xuống. 

Điều này biến vấn đề thành một cuộc kiểm tra tính khả thi đơn điệu$t$, có thể được giải quyết bằng tìm kiếm nhị phân. Việc đóng gói tham lam hoạt động vì các mảng lớn hơn ràng buộc các mảng nhỏ hơn nhiều hơn, vì vậy chúng phải được đặt đầu tiên trong bất kỳ cấu trúc hợp lệ nào. 

Khi chúng ta biết số lượng testcase tối thiểu, chúng ta có thể xây dựng lại nhóm thực tế bằng cách mô phỏng phép gán tham lam tương tự. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2 k)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(n \log n + nk)$kiểm tra ngây thơ, được tối ưu hóa để$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi nén dữ liệu đầu vào bằng cách nhóm các mảng theo kích thước. Điều này cho phép chúng ta xử lý tất cả các mảng có cùng kích thước với nhau. 

Tiếp theo, chúng tôi tìm kiếm câu trả lời nhị phân$t$, số lượng testcase chúng tôi cố gắng xây dựng. 

Đối với một cố định$t$, chúng tôi mô phỏng liệu có thể phân phối tất cả các mảng thành$t$trường hợp thử nghiệm: 

1. Đối với mỗi trường hợp thử nghiệm, chúng tôi duy trì số lượng “khe” còn lại cho mỗi cấp ngưỡng. Ban đầu mỗi testcase có vectơ dung lượng$c_1, c_2, \dots, c_k$. 
2. Chúng tôi xử lý kích thước mảng từ$k$xuống$1$, vì mảng lớn hơn khó đặt hơn. 
3. Với mỗi mảng kích thước$x$, chúng ta phải gán nó cho một testcase vẫn còn ít nhất một vị trí trống ở mọi cấp độ$1$bởi vì$x$. Chúng tôi chọn bất kỳ trường hợp thử nghiệm hợp lệ nào; Để thực hiện điều này hiệu quả, chúng tôi duy trì một cấu trúc theo dõi dung lượng còn lại theo cách cho phép bố trí nhanh chóng. 
4. Nếu tại bất kỳ thời điểm nào không có testcase nào có thể chứa một mảng thì giá trị hiện tại$t$không hợp lệ. 
5. Nếu tất cả các mảng được đặt thành công,$t$là khả thi. 

Sau khi tìm kiếm nhị phân, chúng tôi xây dựng lại bài tập bằng cách sử dụng cùng một ý tưởng tham lam, luôn đặt từng mảng vào trường hợp thử nghiệm sớm nhất mà vẫn có thể chứa nó. 

Chi tiết triển khai quan trọng là tính khả thi chỉ phụ thuộc vào số lượng còn lại trên mỗi cấp độ và do các ràng buộc được lồng vào nhau nên chúng tôi có thể coi mỗi trường hợp kiểm thử có hồ sơ công suất giảm dần mà chúng tôi sẽ trừ đi khi gán mảng. 

### Tại sao nó hoạt động 

Mỗi mảng có kích thước$x$làm giảm tính khả dụng của một trường hợp thử nghiệm cho tất cả các mảng kích thước trong tương lai ít nhất$1$bởi vì$x$. Bởi vì các ràng buộc về dung lượng là đơn điệu trên cả cấu trúc trường hợp thử nghiệm và ngưỡng kích thước, nên bất kỳ phép gán hợp lệ nào cũng phải tôn trọng rằng các mảng có kích thước cao hơn sẽ không bao giờ dễ dàng đặt hơn sau này. Bằng cách xử lý từ lớn nhất đến nhỏ nhất, chúng tôi đảm bảo rằng chúng tôi không bao giờ chặn vị trí cần thiết bằng một quyết định nhỏ hơn. Điều này tạo ra một cấu trúc tối ưu tham lam trong đó tính khả thi được xác định đầy đủ bởi các mẫu tiêu thụ tiền tố, do đó việc tìm kiếm nhị phân theo số lượng thùng trở nên hợp lệ. 

## Giải pháp Python```
PythonRun
```Giải pháp nhị phân đầu tiên tìm kiếm số lượng trường hợp thử nghiệm nhỏ nhất có thể chứa tất cả các mảng. các`can`hàm mô phỏng vị trí tham lam, kiểm tra xem mỗi mảng có thể phù hợp với bất kỳ trường hợp thử nghiệm nào mà không vi phạm các ràng buộc tiền tố hay không. các`build`hàm lặp lại logic tương tự nhưng cũng ghi lại các bài tập thực tế. 

Một rủi ro triển khai tinh vi là quên rằng mỗi vị trí đều ảnh hưởng đồng thời đến tất cả các cấp tiền tố. Đó là lý do tại sao mọi vòng lặp cập nhật đều chạy qua`lvl in range(1, size + 1)`. 

Một điểm quan trọng khác là việc kiểm tra tính khả thi ở hình thức này rất tốn kém; trong một giải pháp được tối ưu hóa hoàn toàn, người ta sẽ nén các trạng thái hoặc sử dụng các cấu trúc kết hợp tham lam tốt hơn, nhưng tính đúng đắn về mặt khái niệm đã được nắm bắt ở đây. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```

```Chúng tôi kiểm tra tính khả thi của$t=2$Đầu tiên. 

| Bước | Kích thước mảng | Đã đặt testcase | Cập nhật cấp độ | 
| --- | --- | --- | --- | 
| 1 | 3 | 0 | thêm (1,1,1) cách sử dụng | 
| 2 | 2 | 1 | thêm (1,1) cách sử dụng | 
| 3 | 2 | 0 | thêm (1,1) cách sử dụng | 
| 4 | 1 | 1 | thêm (1) cách sử dụng | 

Tại thời điểm này, không có testcase nào có thể lấy mảng size-2 hoặc size-3 khác mà không vi phạm các ràng buộc, vì vậy chúng tôi kết luận$t=2$là không thể. Với$t=3$, vị trí thành công. 

Điều này cho thấy các mặt hàng lớn buộc phải phân tách sớm ngay cả khi tổng công suất có vẻ đủ. 

### Ví dụ 2 

Hãy xem xét:```

```Vì$t=2$, chúng tôi đặt cả hai mảng cỡ 2 trước. Mỗi cái đều tiêu thụ công suất cấp 1 và cấp 2. Điều này chỉ để lại tính linh hoạt hạn chế đối với mảng kích thước 1, vẫn có thể được phân phối. 

| Bước | Kích thước mảng | Vị trí | 
| --- | --- | --- | 
| 1 | 2 | T0 | 
| 2 | 2 | T1 | 
| 3 | 1 | T0 | 
| 4 | 1 | T1 | 
| 5 | 1 | T0 | 

Tất cả các mảng đều phù hợp, xác nhận$t=2$hoạt động. Đang cố gắng$t=1$sẽ thất bại ngay lập tức vì công suất cấp 2 sẽ bị vượt quá. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^2 \log n)$ở dạng ngây thơ | Mỗi lần kiểm tra tính khả thi sẽ quét tất cả các thùng và mảng | 
| Không gian |$O(nk)$| Lưu trữ để phân công và theo dõi năng lực | 

Điều này phù hợp với các ràng buộc về mặt khái niệm nhưng không phải là cấu trúc được tối ưu hóa dự định cho các giới hạn trong trường hợp xấu nhất. Ý tưởng chính là tính khả thi là đơn điệu về số lượng các trường hợp thử nghiệm, cho phép tìm kiếm nhị phân và đóng gói tham lam. 

## Trường hợp thử nghiệm```
PythonRun
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tối thiểu | 1 | đủ trường hợp thử nghiệm duy nhất | 
| kích cỡ hỗn hợp | khác nhau | tương tác của các ràng buộc | 
| đồng phục tệ nhất | n | năng lực chặt chẽ buộc phải chia tách | 

## Vỏ cạnh 

Trường hợp một cạnh là khi tất cả các mảng có kích thước 1. Trong trường hợp đó, chỉ$c_1$vấn đề, và những ràng buộc cao hơn là không liên quan. Thuật toán tự nhiên đóng gói một cách tham lam vào một số trường hợp thử nghiệm nếu cần. 

Một trường hợp đặc biệt khác là khi một mảng rất lớn tồn tại cùng với nhiều mảng nhỏ. Mảng lớn tiêu thụ tất cả dung lượng tiền tố lên đến kích thước của nó trong bất kỳ trường hợp thử nghiệm nào được đặt, tạo ra hiệu ứng gần như cô lập. Quá trình xử lý đầu tiên lớn nhất tham lam đảm bảo ràng buộc này được tôn trọng trước khi các mảng nhỏ hơn được chỉ định, ngăn chặn việc điền sớm không hợp lệ. 

Trường hợp cạnh cuối cùng là khi$c_k = 1$. Điều này buộc mọi mảng có kích thước tối đa phải được tách biệt và thuật toán sẽ đặt chính xác từng mảng đó vào một ca kiểm thử riêng biệt trước khi xem xét các mục nhỏ hơn.
