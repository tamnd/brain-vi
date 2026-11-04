---
title: "CF 1172A - Nauuo và Thẻ"
description: "Chúng ta được cung cấp một hệ thống được chia thành hai phần: một tay bài và một chồng bài. Cùng nhau, chúng chứa mọi thẻ số nguyên từ 1 đến n chính xác một lần, trong khi số 0 đại diện cho phần giữ chỗ trống hoạt động giống như thẻ giả không có giá trị."
date: "2026-06-15T17:14:33+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1172
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 564 (Div. 1)"
rating: 1800
weight: 1172
solve_time_s: 445
verified: true
draft: false
---

[CF 1172A - Nauuo và Thẻ](https://codeforces.com/problemset/problem/1172/A) 

**Đánh giá:** 1800 
**Tags:** tham lam, thực hiện 
**Thời gian giải:** 7 phút 25 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một hệ thống được chia thành hai phần: một tay bài và một chồng bài. Cùng nhau, chúng chứa mọi thẻ số nguyên từ 1 đến n chính xác một lần, trong khi số 0 đại diện cho phần giữ chỗ trống hoạt động giống như thẻ giả không có giá trị. 

Đống có thứ tự cố định từ trên xuống dưới và mục tiêu là biến đổi đống này sao cho nó được sắp xếp hoàn hảo thành 1, 2, 3, …, n từ trên xuống dưới. Cách duy nhất để tác động đến hệ thống là thông qua thao tác tiêu tốn một quân bài từ tay bài: chúng ta chọn bất kỳ vị trí tay bài nào, đẩy quân bài đó xuống cuối cọc, sau đó rút ngay quân bài trên cùng hiện tại của cọc trở lại vị trí ván bài đó. 

Điều này tạo ra một quy trình kết hợp: mọi hành động đồng thời đưa một lá bài trên tay vào cọc và rút một lá bài trên tay vào tay. Nhiệm vụ là tìm số lượng hành động tối thiểu cần thiết để sắp xếp đống. 

Khó khăn chính là đống không thể được sắp xếp lại một cách tự do. Trật tự nội bộ của nó chỉ có thể được sửa đổi gián tiếp thông qua việc hoán đổi bằng các lá bài trên tay và mọi lần hoán đổi đều bị hạn chế bởi cấu trúc hoạt động cố định. 

Các ràng buộc rất lớn, với n lên tới 200.000. Điều này ngay lập tức loại trừ bất kỳ mô phỏng nào liên tục quét hoặc xây dựng lại cọc cho mỗi hoạt động. Bất kỳ giải pháp nào cố gắng mô phỏng quy trình từng bước đều có nguy cơ xảy ra hành vi O(n^2) trong trường hợp xấu nhất là quá chậm. 

Một trường hợp khó khăn xuất hiện khi hầu hết các lá bài hữu ích đều nằm trong tay. Ví dụ: nếu đống đã chứa một tiền tố dài gồm các số chính xác nhưng theo thứ tự bị xáo trộn, thì một chiến lược ngây thơ tham lam sửa các điểm không khớp sớm nhất có thể lãng phí các hoạt động hoán đổi các thẻ không liên quan. Một kịch bản có vấn đề khác là khi số 0 thống trị các vị trí cọc sớm, làm chậm khả năng tiếp cận các thẻ có ý nghĩa và các chiến lược gây hiểu lầm cho rằng tiến bộ sớm hàm ý tiến bộ toàn cầu. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực mô phỏng quá trình một cách trực tiếp. Ở mỗi bước, chúng tôi sẽ thử tất cả các lựa chọn bài có thể có, áp dụng thao tác và khám phá các cấu hình kết quả cho đến khi đống được sắp xếp. Về nguyên tắc, điều này đúng vì nó khám phá toàn bộ không gian trạng thái của các cấu hình có thể tiếp cận, nhưng hệ số phân nhánh là n ở mỗi bước và độ sâu cũng có thể tuyến tính tính bằng n. Điều này dẫn đến sự bùng nổ theo cấp số nhân về khả năng, vượt xa mọi giới hạn khả thi đối với n lên tới 200.000. 

Quan sát quan trọng là thao tác không phụ thuộc một cách có ý nghĩa vào vị trí tay nào được chọn mà chỉ phụ thuộc vào giá trị nào được chơi. Khi một lá bài được di chuyển vào cọc, sự phát triển của cọc chỉ phụ thuộc vào chuỗi các giá trị được chèn chứ không phụ thuộc vào vị trí của chúng. Điều này có nghĩa là vấn đề có thể được trình bày lại thành việc quyết định thứ tự các thẻ được đánh số được “kích hoạt” vào cọc. 

Bây giờ hãy xem xét cấu hình cuối cùng mong muốn: đống phải theo thứ tự từ 1 đến n. Điều đó có nghĩa là chúng tôi đang cố gắng đảm bảo rằng khi hệ thống ổn định, mỗi số i xuất hiện ở đúng vị trí so với các số trước đó. Trở ngại duy nhất là cần bao nhiêu lần chèn không liên quan trước khi mỗi thẻ bắt buộc có thể truy cập được trong cấu trúc trên cùng của cọc. 

Cái nhìn sâu sắc quan trọng là chúng ta có thể mô phỏng ngược từ trạng thái mong muốn cuối cùng, theo dõi số lượng thao tác được yêu cầu để “hiển thị” từng số theo thứ tự tăng dần. Mỗi số hoặc đã có sẵn trong chồng hoặc bị mắc kẹt đằng sau các phần tử khác và việc giải nó có thể yêu cầu phải tuần hoàn qua cấu trúc bằng cách sử dụng các thẻ bài có sẵn. 

Điều này làm giảm vấn đề thành một sự tái cấu trúc tham lam trong đó chúng tôi xử lý các số từ n xuống 1 và đếm xem cần bao nhiêu thao tác bắt buộc trước khi mỗi số có thể được đặt chính xác.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Tái thiết tham lam từ trật tự mục tiêu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý thứ tự mong muốn cuối cùng từ thẻ lớn nhất xuống thẻ nhỏ nhất, duy trì các con trỏ trong phân phối cọc và tay. 

1. Xây dựng bản đồ vị trí cho mỗi quân bài từ 1 đến n cho biết nó bắt đầu ở cọc hay trên tay. Điều này cho phép kiểm tra liên tục vị trí hiện tại của mỗi thẻ. 
2. Duy trì một con trỏ trong đống biểu thị mức độ hiệu quả mà chúng ta đã “làm sạch” hoặc căn chỉnh đống thông qua các thao tác. Ban đầu đây là phần trên cùng của đống. 
3. Lặp lại các giá trị đích từ n xuống 1. Với mỗi giá trị x, hãy xác định xem x hiện có thể truy cập được tại hay trước con trỏ cọc hiện tại hay không. Nếu đúng như vậy, chúng ta có thể căn chỉnh nó về mặt khái niệm mà không cần thêm các hoạt động bắt buộc ngoài cấu trúc nâng cao. 
4. Nếu x vẫn chưa thể truy cập được, chúng ta phải thực hiện các thao tác xoay vòng hệ thống một cách hiệu quả cho đến khi x có thể truy cập được. Mỗi yêu cầu như vậy đều góp phần tạo ra câu trả lời, vì nó tương ứng với một chu kỳ hoán đổi cần thiết để đưa x vào đúng vị trí. 
5. Mỗi lần chúng ta mô phỏng nhu cầu “bỏ qua” trong cọc để đạt x, chúng ta sẽ tăng câu trả lời và cập nhật trạng thái hiệu quả để phản ánh rằng cọc đã được xoay thông qua việc chèn thẻ tay và vẽ phần tử cọc tiếp theo. 

Cơ chế cốt lõi là theo dõi cách con trỏ cọc di chuyển về phía trước để đáp ứng các hoạt động bắt buộc cần thiết để lộ ra các phần tử bị thiếu hoặc bị chặn. 

### Tại sao nó hoạt động 

Quá trình này tương đương với việc giải quyết liên tục giá trị yêu cầu lớn nhất tiếp theo theo thứ tự ngược lại. Vì mỗi thao tác chỉ có thể nâng cao quyền truy cập vào các phần tử cọc sâu hơn bằng một bước hiệu quả, nên mỗi khi gặp một giá trị chưa thể tiếp cận được, chúng tôi buộc phải dành một thao tác để nâng cao trạng thái hệ thống. Điều này tạo ra một tiến trình đơn điệu trong đó mỗi thao tác đóng góp chính xác một đơn vị tiến trình để đưa ra thẻ được yêu cầu tiếp theo. Bởi vì chúng tôi luôn xử lý theo thứ tự giảm dần nên chúng tôi không bao giờ xem lại các ràng buộc đã được giải quyết, đảm bảo tính tối ưu. 

## Giải pháp Python```
PythonRun
```Việc triển khai bắt đầu bằng cách tách biệt vị trí ban đầu của mỗi thẻ được đánh số. Vị trí cọc được lưu trữ để chúng ta có thể suy luận về thứ tự, trong khi mảng boolean theo dõi xem lá bài bắt đầu trong cọc hay trong tay. 

Con trỏ`ptr`thể hiện khoảng cách mà chúng tôi đã đồng bộ hóa cấu trúc một cách hiệu quả. Khi chúng tôi gặp một thẻ đã có trong chồng nhưng nằm trước con trỏ này, điều đó cho biết chúng tôi đã bọc thẻ đó theo cách yêu cầu một thao tác bổ sung để truy cập lại chính xác, vì vậy chúng tôi tính một thao tác và đặt lại tiến trình. 

Các quân bài ban đầu có trong tay luôn tốn ít nhất một thao tác vì chúng phải được đưa vào cọc thông qua một hành động trước khi có thể tham gia vào việc hình thành cấu trúc được sắp xếp. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```

```Chúng tôi theo dõi các vị trí: 

| x | trong đống | vị trí | ptr | hành động | trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 1 | vâng | 2 | 0 | di chuyển ptr | 0 | 
| 2 | vâng | 1 | 0 | chu kỳ cần thiết | 1 | 
| 3 | vâng | 0 | 1 | di chuyển ptr | 1 | 

Thời điểm quan trọng là việc xử lý 2 sau 3 đã dịch chuyển sự căn chỉnh hiệu quả, buộc một thao tác phải đồng bộ lại cấu trúc. 

### Ví dụ 2 

đầu vào:```

```| x | trong đống | vị trí | ptr | hành động | trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 1 | vâng | 0 | 0 | bắt đầu căn chỉnh | 0 | 
| 2 | vâng | 0 | 0 | chu kỳ cần thiết | 1 | 
| 3 | vâng | 1 | 0 | di chuyển ptr | 1 | 

Điều này chứng tỏ rằng ngay cả khi hầu hết các phần tử đã có trong đống, các ràng buộc về thứ tự sẽ buộc phải thực hiện các thao tác bổ sung khi vị trí tương đối không nhất quán. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | mỗi thẻ được xử lý một lần với kiểm tra O(1) | 
| Không gian | O(n) | mảng lưu trữ vị trí và thông tin thành viên | 

Cấu trúc tuyến tính phù hợp thoải mái trong giới hạn n lên tới 200.000 và tất cả các hoạt động là tra cứu thời gian liên tục hoặc so sánh đơn giản. 

## Trường hợp thử nghiệm```
PythonRun
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 0 | ranh giới tối thiểu | 
| trao đổi đơn giản | 1 | tương tác cơ bản | 
| cọc ngược | 2 | đặt hàng tệ nhất | 
| đã được sắp xếp | 0 | trường hợp không hoạt động | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là khi tất cả các số được yêu cầu đã theo đúng thứ tự nhưng được phân phối giữa tay và cọc theo cách buộc phải chuyển ít nhất một lần. Ví dụ: nếu hầu hết các số nhỏ đều có trong tay, thuật toán vẫn phải tính đến thực tế là việc đưa chúng vào đống sẽ làm thay đổi khả năng tiếp cận các số sau này. 

Một trường hợp khó phát hiện khác là khi đống gần như đã được sắp xếp nhưng lại chứa các số 0 xen kẽ. Số 0 không góp phần vào việc đặt hàng nhưng vẫn sử dụng quá trình chuyển đổi, do đó, bất kỳ chiến lược nào coi chúng là trung lập đều có thể đánh giá thấp các hoạt động cần thiết. Thuật toán xử lý vấn đề này vì số 0 không bao giờ xuất hiện trong bản đồ vị trí, do đó chúng chỉ ảnh hưởng đến khả năng tiếp cận một cách gián tiếp thông qua chuyển động của con trỏ chứ không bị nhầm lẫn với tiến trình hợp lệ.
