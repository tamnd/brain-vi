---
title: "CF 1288E - Trình mô phỏng tin nhắn"
description: "Chúng tôi đang duy trì một “danh sách trò chuyện gần đây” động gồm bạn bè, được biểu thị dưới dạng hoán vị của các số từ 1 đến n. Danh sách được sắp xếp từ tương tác gần đây nhất đến tương tác gần đây nhất."
date: "2026-06-16T04:02:39+07:00"
tags: ["codeforces", "competitive-programming", "data-structures"]
categories: ["algorithms"]
codeforces_contest: 1288
codeforces_index: "E"
codeforces_contest_name: "Educational Codeforces Round 80 (Rated for Div. 2)"
rating: 2000
weight: 1288
solve_time_s: 611
verified: false
draft: false
---

[CF 1288E - Trình mô phỏng Messenger](https://codeforces.com/problemset/problem/1288/E) 

**Xếp hạng:** 2000 
**Thẻ:** cấu trúc dữ liệu 
**Thời gian giải:** 10 phút 11 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang duy trì một “danh sách trò chuyện gần đây” động gồm bạn bè, được biểu thị dưới dạng hoán vị của các số từ 1 đến n. Danh sách được sắp xếp từ tương tác gần đây nhất đến tương tác gần đây nhất. Bất cứ khi nào có tin nhắn đến từ người bạn x, người bạn đó sẽ được chuyển lên đầu danh sách và những người đứng trước x sẽ chuyển một vị trí sang phải. 

Chúng ta bắt đầu từ thứ tự nhận dạng, nghĩa là bạn i ban đầu ở vị trí i. Sau khi xử lý tất cả m tin nhắn, mỗi người bạn đều được di chuyển nhiều lần, lúc tiến lên và đôi khi gián tiếp lùi lại do những người bạn khác bị kéo lên phía trước. Trong toàn bộ quá trình, bao gồm cấu hình ban đầu và mọi cấu hình trung gian sau mỗi tin nhắn, mỗi người bạn sẽ chiếm một chuỗi vị trí. 

Nhiệm vụ là tính toán cho mỗi người bạn vị trí tối thiểu và tối đa mà họ từng chiếm giữ trong toàn bộ quá trình tiến hóa này. 

Các ràng buộc n, m 3 · 10^5 ngay lập tức loại trừ bất kỳ mô phỏng nào duy trì rõ ràng hoán vị đầy đủ sau mỗi thao tác. Một bản cập nhật đơn giản của mảng trên mỗi tin nhắn đã là O(n) trong trường hợp xấu nhất, dẫn đến O(nm), vượt xa giới hạn khả thi. 

Một khía cạnh tinh tế của vấn đề là các vị trí không độc lập. Khi một người bạn được chuyển lên phía trước, nó sẽ ảnh hưởng đến vị trí của tất cả những người khác, vì vậy ngay cả việc theo dõi một người bạn một cách ngây thơ vẫn yêu cầu cập nhật toàn cầu. 

Một số trường hợp đặc biệt cho thấy những sai lầm có thể xảy ra với lối suy nghĩ ngây thơ. Nếu tất cả tin nhắn đều đến từ cùng một người bạn, ví dụ 1 lần lặp lại m lần, thì người bạn đó luôn ở vị trí 1, còn những người khác ban đầu chỉ bị đẩy lùi rồi ổn định. Một giải pháp ngây thơ chỉ tính toán lại vị trí vào thời điểm có tin nhắn có thể bỏ lỡ rằng vị trí thay đổi ngay cả khi một người bạn không liên quan trực tiếp. 

Một trường hợp khác là khi tin nhắn giới thiệu bạn bè muộn, ví dụ n = 5 và tin nhắn là [5, 4, 3, 2, 1]. Thứ tự cuối cùng bị đảo ngược và các vị trí trung gian biến động mạnh; vị trí tối đa của những người bạn di chuyển sớm thường xảy ra rất lâu trước khi kết thúc. 

Khó khăn chính là các vị trí phụ thuộc vào trật tự toàn cầu thay đổi liên tục. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là mô phỏng trực tiếp. Chúng tôi duy trì một mảng đại diện cho hoán vị. Đối với mỗi tin nhắn từ người bạn x, chúng tôi xác định vị trí x trong mảng, dịch chuyển tất cả các phần tử trước nó sang phải một bước và đặt x ở phía trước. Trong khi thực hiện việc này, chúng tôi có thể theo dõi vị trí của từng người bạn ở mỗi bước và cập nhật mức tối thiểu và tối đa của họ. 

Việc định vị x mất O(n) trừ khi chúng ta duy trì cấu trúc bổ sung và việc dịch chuyển cũng mất O(n). Do đó, mỗi m thao tác là O(n), dẫn đến O(nm), tức là khoảng 9 × 10^10 thao tác trong trường hợp xấu nhất. Điều này là quá chậm. 

Quan sát quan trọng là chúng ta không thực sự cần sự tiến hóa hoán vị đầy đủ. Đối với mỗi người bạn, chúng ta chỉ cần tập hợp các vị trí mà họ xuất hiện vào những thời điểm cụ thể. Cấu hình ban đầu đóng góp một vị trí đã biết i cho bạn i. Sau đó, lần duy nhất vị trí thay đổi là ngay sau một tin nhắn và tại mỗi thời điểm như vậy, chỉ có cấu trúc tiền tố mới quan trọng. 

Thay vì theo dõi toàn bộ hoán vị, chúng tôi có thể theo dõi lần cuối cùng họ được truy cập đối với từng người bạn. Thứ tự tại bất kỳ thời điểm nào được xác định theo thời gian gần đây: các tin nhắn gần đây hơn tương ứng với các vị trí nhỏ hơn. Một người bạn mới được nhìn thấy gần đây đang ở gần phía trước hơn. 

Điều này dẫn đến một sự cải cách quan trọng: tại bất kỳ thời điểm nào, vị trí của một người bạn được xác định bởi số lượng người bạn “hoạt động” khác biệt gần đây hơn nó. Thay vì mô phỏng giao dịch hoán đổi, chúng tôi mô phỏng dấu thời gian. Mỗi tin nhắn chỉ định một chỉ mục thời gian và sau đó chúng tôi xử lý những thời gian này theo cách có cấu trúc để tính toán động lực xếp hạng.

Để trích xuất các vị trí tối thiểu và tối đa, chúng tôi coi các vị trí ban đầu là cố định trong thời gian đầu và sau đó coi mỗi tin nhắn có khả năng đẩy một người bạn lên vị trí 1, tạo ra vị trí (tối thiểu) tốt nhất của người đó. Vị trí tối đa xảy ra khi nó lùi xa nhất có thể, xảy ra trước lần xuất hiện đầu tiên của nó hoặc khi nhiều người bạn khác biệt đã được chuyển lên trước nó. 

Chúng tôi xử lý các khoản đóng góp bằng cách sắp xếp theo lần xuất hiện gần đây nhất và sử dụng cây Fenwick (hoặc cây được lập chỉ mục nhị phân) theo thời gian để tính toán số lượng sự kiện "đang hoạt động" trước các phân đoạn có liên quan của mỗi người bạn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(nm) | O(n) | Quá chậm | 
| Cây Fenwick + Theo dõi lần xuất hiện cuối cùng | O((n + m) log n) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi biến hành vi của mỗi người bạn thành một vấn đề về dòng thời gian. 

1. Ghi lại toàn bộ thời gian nhắn tin của từng người bạn. Đối với bạn x, lưu trữ danh sách các chỉ mục nơi x xuất hiện trong chuỗi tin nhắn. Điều này là cần thiết vì mỗi diện mạo đều có khả năng di chuyển x lên phía trước và do đó ảnh hưởng đến vị trí tối thiểu của nó. 
2. Xác định “thời gian kích hoạt cuối cùng có hiệu lực” cho mỗi người bạn là lần cuối cùng nó xuất hiện trong danh sách tin nhắn. Đây là điểm mà sau đó không có hoạt động nào trong tương lai trực tiếp cải thiện vị thế của nó. 
3. Tính vị trí tối thiểu cho mỗi người bạn. Một người bạn đạt được vị trí số 1 chính xác khi nó được xử lý trong tin nhắn. Nếu một người bạn không bao giờ xuất hiện, vị trí tối thiểu của người đó vẫn là vị trí ban đầu. Nếu không, mức tối thiểu của nó là 1. Điều này ngay lập tức xuất phát từ quy tắc mọi tin nhắn sẽ chuyển người bạn đó lên phía trước. 
4. Phần khó hơn là tính toán vị trí tối đa. Chúng tôi xử lý bạn bè theo thứ tự tăng dần về thời gian xuất hiện cuối cùng của họ. Chúng tôi duy trì một cây Fenwick để lưu trữ số lượng bạn bè đã được “kích hoạt” vào hoặc trước một thời điểm nhất định. 
5. Với người bạn x, xét vị trí ban đầu của nó i. Trước lần kích hoạt đầu tiên, tất cả bạn bè có thời gian xuất hiện lần cuối trước đó không phải là x có thể đã được chuyển lên phía trước và do đó có thể dịch chuyển x lùi lại. Chúng tôi tính toán có bao nhiêu lần kích hoạt như vậy xảy ra trước lần xuất hiện đầu tiên của x và cộng số đó với i để ước tính vị trí tồi tệ nhất của nó. 
6. Chúng tôi giới hạn giá trị này bằng n vì các vị trí không thể vượt quá kích thước mảng. 
7. Cập nhật cây Fenwick khi xử lý lần xuất hiện gần đây nhất của mỗi người bạn để các truy vấn trong tương lai đếm chính xác các tiền tố hoạt động. 

### Tại sao nó hoạt động 

Điều bất biến chính là mỗi khi một người bạn xuất hiện trong chuỗi tin nhắn, nó sẽ trở nên gần đây hơn tất cả những người bạn chưa được xử lý theo thứ tự lần xuất hiện cuối cùng. Do đó, tại bất kỳ thời điểm nào, nhóm bạn bè có thể xuất hiện trước một người bạn nhất định chính xác là nhóm bạn bè có lần kích hoạt cuối cùng sớm hơn và đã được tính đến. Điều này làm giảm vấn đề hoán vị động trong việc đếm xem có bao nhiêu phần tử “hoạt động cuối cùng trước đó” như vậy tồn tại trước một ngưỡng nhất định, đây chính xác là những gì cây Fenwick duy trì. 

Bởi vì mọi chuyển động về phía trước tương ứng với một sự kiện kích hoạt riêng biệt và những sự kiện này tạo thành một thứ tự tổng thể theo thời gian, chúng tôi không bao giờ tính hai lần hoặc bỏ sót bất kỳ đóng góp nào cho sự dịch chuyển trong trường hợp xấu nhất của một người bạn. 

## Giải pháp Python```
PythonRun
```Quá trình triển khai bắt đầu bằng cách ghi lại lần xuất hiện đầu tiên và cuối cùng của mỗi người bạn trong chuỗi tin nhắn. Điều này rất cần thiết vì bất kỳ người bạn nào được nhắn tin đều có thể ngay lập tức đạt đến vị trí 1, vì vậy mức tối thiểu của nó được cố định là 1. 

Cây Fenwick được xây dựng dựa trên chỉ mục tin nhắn chứ không phải dựa trên ID bạn bè. Mỗi lần kích hoạt tương ứng với lần xuất hiện cuối cùng và chúng tôi chèn nó khi xử lý một người bạn theo thứ tự tăng dần của lần xuất hiện cuối cùng. 

Đối với mỗi người bạn x, trước khi kích hoạt nó, chúng tôi truy vấn có bao nhiêu lần kích hoạt đã xảy ra trước khi nó xuất hiện lần đầu. Con số đó tương ứng với số lượng bạn bè khác đã được chuyển lên phía trước trước đó trong quá trình này và có thể góp phần đẩy x lùi lại trong trường hợp xấu nhất. 

Mức tối đa cuối cùng được giới hạn bởi n vì ngay cả trong sự sắp xếp lại tồi tệ nhất, các vị trí cũng không thể vượt quá kích thước danh sách. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```

```Chúng tôi tính toán lần xuất hiện đầu tiên và cuối cùng: 

| Bạn | đầu tiên | cuối cùng | 
| --- | --- | --- | 
| 1 | 3 | 3 | 
| 2 | 5 | 0 | 
| 3 | 1 | 1 | 
| 4 | 4 | 4 | 
| 5 | 2 | 2 | 

Thứ tự xử lý theo lần xuất hiện gần nhất: 3, 5, 1, 4, 2. 

Chúng tôi duy trì BIT theo thời gian. 

| Bước | Bạn | đầu tiên | cuối cùng | Tổng BIT trước | phút | tối đa | cập nhật BIT | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | 3 | 1 | 1 | 0 | 1 | 3 | thêm (1) | 
| 2 | 5 | 2 | 2 | 1 | 1 | 5 | thêm (2) | 
| 3 | 1 | 3 | 3 | 2 | 1 | 5 | thêm (3) | 
| 4 | 4 | 4 | 4 | 3 | 1 | 5 | thêm(4) | 
| 5 | 2 | - | 0 | - | 2 | 2 | - | 

Điều này cho thấy mức độ tích lũy của các kích hoạt sớm và ảnh hưởng đến các giới hạn sau này. 

### Ví dụ 2 

đầu vào:```

```| Bạn | đầu tiên | cuối cùng | phút | tối đa | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | 2 | 1 | 3 | 
| 2 | 1 | 1 | 1 | 2 | 
| 3 | - | 0 | 3 | 3 | 
| 4 | 3 | 3 | 1 | 4 | 

Ví dụ này nêu bật cách một người bạn chưa bao giờ kích hoạt giữ cả hai giới hạn cố định ở vị trí ban đầu, trong khi những người khác giảm xuống 1 ngay sau bất kỳ tin nhắn nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + m) log m) | Mỗi cập nhật và truy vấn trên cây Fenwick đều là logarit và mỗi người bạn được xử lý một lần | 
| Không gian | O(n + m) | Lưu trữ các lần xuất hiện và cây Fenwick | 

Điều này phù hợp một cách thoải mái trong giới hạn của n, m cho đến 3 · 10^5, vì hệ số logarit vẫn nhỏ. 

## Trường hợp thử nghiệm```
PythonRun
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả cùng một người bạn | ổn định tối thiểu/tối đa | xử lý kích hoạt lặp đi lặp lại | 
| không có tin nhắn | giới hạn nhận dạng | trường hợp cơ sở đúng đắn | 
| thứ tự đảo ngược | cải tổ toàn bộ | chuyển vị tồi tệ nhất | 
| dãy giảm dần | mức chênh lệch tối đa | logic sắp xếp ứng suất | 

## Vỏ cạnh 

Một người bạn không bao giờ xuất hiện trong tin nhắn sẽ không bao giờ di chuyển lên phía trước nên mức tối thiểu và tối đa của nó vẫn bằng vị trí ban đầu. Thuật toán xử lý vấn đề này một cách rõ ràng bằng cách kiểm tra Last[x] = 0 và gán trực tiếp cả hai giá trị. Ví dụ: ở đầu vào n = 4 với tin nhắn [2, 3], bạn 1 không bao giờ được kích hoạt nên vẫn ở vị trí 1. 

Một người bạn xuất hiện trong mọi tin nhắn sẽ ngay lập tức đạt được vị trí tối thiểu 1. Cây Fenwick đảm bảo rằng vị trí tối đa của nó vẫn được tính toán dựa trên những lần kích hoạt trước đó chứ không phải những lần kích hoạt muộn hơn, do đó nó không tích lũy sai các ca làm việc trong tương lai. 

Khi tất cả các tin nhắn giống hệt nhau, chẳng hạn [1, 1, 1, 1], cấu trúc của cây Fenwick vẫn đơn giản: chỉ một lần kích hoạt được chèn và tất cả những người bạn khác không bị ảnh hưởng. Điều này ngăn chặn việc đếm quá mức các ca không thực sự di chuyển bất kỳ phần tử mới nào.
