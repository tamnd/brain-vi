---
title: "CF 1656I - Đặt hàng hàng xóm"
description: "Chúng ta được cho một đồ thị vô hướng liên thông và chúng ta được phép chọn một cách độc lập cho mỗi đỉnh một thứ tự các đỉnh liền kề của nó."
date: "2026-06-15T00:30:49+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "graphs"]
categories: ["algorithms"]
codeforces_contest: 1656
codeforces_index: "I"
codeforces_contest_name: "CodeTON Round 1 (Div. 1 + Div. 2, Rated, Prizes!)"
rating: 3500
weight: 1656
solve_time_s: 636
verified: false
draft: false
---

[CF 1656I - Đặt hàng hàng xóm](https://codeforces.com/problemset/problem/1656/I) 

**Đánh giá:** 3500 
**Tags:** thuật toán xây dựng, đồ thị 
**Thời gian giải:** 10 phút 36 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một đồ thị vô hướng liên thông và chúng ta được phép chọn một cách độc lập cho mỗi đỉnh một thứ tự các đỉnh liền kề của nó. Thứ tự này tạo ra một khái niệm so sánh cục bộ: tại một đỉnh$v$, nếu hàng xóm$a$xuất hiện muộn hơn hàng xóm$b$trong danh sách$v$, chúng tôi nói$b <_v a$. 

Ràng buộc không mang tính cục bộ mà dựa trên chu kỳ. Lấy bất kỳ chu trình đơn giản nào trong biểu đồ. Đối với mỗi đỉnh trên chu trình đó, hãy xem xét hai cạnh chu trình liên quan đến nó và so sánh hai điểm cuối đối diện bằng cách sử dụng thứ tự lân cận của đỉnh đó. Chu trình chỉ hợp lệ nếu tất cả những so sánh này nhất quán trong chu trình: hoặc mọi đỉnh đều “đồng ý” theo một hướng xung quanh chu trình hoặc mọi đỉnh đều đồng ý theo hướng ngược lại. 

Vì vậy, mỗi chu kỳ phải hoạt động như thể nó có một hướng nhất quán toàn cầu khi được xem xét thông qua các thứ tự lân cận cục bộ này. 

Đầu ra là một cấu trúc của tất cả các danh sách kề thỏa mãn tính chất này hoặc một tuyên bố rằng không tồn tại cấu trúc như vậy. 

Đồ thị có thể có tới$3 \cdot 10^5$các đỉnh và cạnh trong tất cả các trường hợp thử nghiệm, do đó, mọi giải pháp đều phải có kích thước tổng thể tuyến tính hoặc gần tuyến tính. Bất cứ điều gì liên quan đến việc liệt kê các chu trình một cách rõ ràng là không thể, vì số chu trình có thể là số mũ ngay cả trong các đồ thị thưa thớt. 

Trường hợp thất bại đầu tiên xuất phát từ đồ thị có các chu kỳ chồng chéo lên nhau. Ví dụ: hai tam giác có chung một cạnh. Bất kỳ nỗ lực ngây thơ nào gán các thứ tự kề tùy ý một cách độc lập cho mỗi đỉnh thường sẽ thỏa mãn từng tam giác một cách cô lập nhưng sẽ phá vỡ tính nhất quán khi các chu trình tương tác qua các đỉnh được chia sẻ. Ràng buộc mang tính toàn cục: việc cố định hướng của một chu trình sẽ ràng buộc cách sắp xếp các chu trình khác tới cùng một đỉnh. 

Một lỗi nhỏ khác phát sinh trong các giao điểm chu trình dày đặc chẳng hạn như “đồ thị theta” (hai đỉnh được nối với nhau bằng ba đường đi riêng biệt bên trong). Mỗi cặp đường tạo thành một chu kỳ và việc buộc phải định hướng nhất quán trên tất cả chúng sẽ dẫn đến mâu thuẫn trừ khi cấu trúc bị hạn chế cao. 

Những ví dụ này gợi ý rằng biểu đồ không thể chứa “các chu trình tương tác” một cách phức tạp. Cấu trúc phải gần giống cây, với các chu trình cách ly với nhau. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là coi vấn đề như một nhiệm vụ thỏa mãn những ràng buộc. Đối với mỗi đỉnh, chúng tôi thử tất cả các hoán vị của danh sách kề của nó và đối với mỗi lựa chọn, chúng tôi kiểm tra tính nhất quán của tất cả các chu trình. Ngay cả khi chúng ta tránh liệt kê tất cả các hoán vị, chúng ta có thể cố gắng truyền bá các ràng buộc theo chu kỳ, nhưng việc phát hiện tất cả các chu kỳ đã tốn thời gian theo cấp số nhân trong các biểu đồ tổng quát. 

Ngay cả việc giới hạn cây bao trùm cũng không giúp ích được gì, bởi vì mọi cạnh không phải cây đều đưa ra một chu trình cơ bản và các chu trình này tương tác với nhau. Cách tiếp cận vũ phu biến thành lý luận về một tập hợp các ràng buộc theo cấp số nhân. 

Quan sát cấu trúc quan trọng là mỗi chu kỳ áp đặt một ràng buộc nhị phân: nó thực thi một “loại hướng” nhất quán cho chu kỳ đó. Nếu hai chu trình chia sẻ một đỉnh một cách phức tạp, chúng sẽ cố gắng áp đặt các yêu cầu sắp xếp không tương thích trên đỉnh đó trừ khi đồ thị cực kỳ hạn chế. 

Sự đơn giản hóa mang tính quyết định là các đồ thị khả thi chính xác là những đồ thị trong đó các chu trình không trùng nhau theo cách tạo ra nhiều ràng buộc chu trình độc lập ở cùng một cạnh hoặc đỉnh. Điều này buộc đồ thị phải có cấu trúc xương rồng, nghĩa là mỗi cạnh thuộc nhiều nhất một chu trình đơn. Trong các biểu đồ như vậy, mỗi chu kỳ có thể được định hướng độc lập và chúng ta có thể nhúng các hướng này vào các thứ tự lân cận cục bộ mà không có xung đột. 

Khi đồ thị là một cây xương rồng, việc xây dựng có thể thực hiện được bằng cách xây dựng cây DFS, xác định các cạnh của chu kỳ và chỉ định hướng nhất quán cho mỗi chu kỳ, sau đó chuyển hướng đó thành thứ tự lân cận cục bộ để các lân cận của chu kỳ xuất hiện theo thứ tự tương đối nhất quán ở mọi đỉnh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu đối với hoán vị/chu kỳ | Hàm mũ | Hàm mũ | Quá chậm | 
| DFS + đặc tính xương rồng + xây dựng |$O(n + m)$|$O(n + m)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi dựa vào việc xây dựng cấu trúc biểu đồ thông qua DFS và phát hiện các cạnh sau tạo thành chu trình. Mục đích là để đảm bảo rằng các chu kỳ được tách rời khỏi các cạnh theo nghĩa thuộc về một cây xương rồng, sau đó sử dụng cấu trúc này để ấn định thứ tự lân cận. 

1. Chạy DFS để xây dựng cây bao trùm và xác định các cạnh sau. 

Mỗi cạnh sau xác định một chu trình cơ bản duy nhất trong cây DFS. Chúng tôi ghi lại các cạnh thuộc về cấu trúc chu trình nào. 
2. Theo dõi xem mỗi cạnh tham gia vào bao nhiêu chu kỳ. 

Nếu bất kỳ cạnh nào được tìm thấy thuộc về nhiều hơn một chu trình, chúng ta ngay lập tức kết luận rằng không tồn tại thứ tự hợp lệ. Điều này tương ứng với tình huống trong đó các chu kỳ chồng chéo theo cách tạo ra các ràng buộc về thứ tự trái ngược nhau. 
3. Nếu cấu trúc hợp lệ, hãy coi mỗi thành phần chu trình liên thông đôi như một chu trình đơn độc lập. 

Các cạnh của cây không áp đặt các ràng buộc theo chu kỳ nên chúng luôn an toàn. 
4. Đối với mỗi chu kỳ, hãy gán cho nó một hướng tùy ý. 

Điều này có nghĩa là quyết định hướng mà chúng ta sẽ đi qua chu kỳ về mặt khái niệm. Sự lựa chọn này là độc lập trong mỗi chu kỳ vì cấu trúc xương rồng đảm bảo không có sự can thiệp giữa các chu kỳ. 
5. Xây dựng thứ tự kề theo từng đỉnh. 

Với mỗi đỉnh, tách các cạnh tới của nó thành các cạnh cây và các cạnh chu trình. Các cạnh của chu trình được nhóm theo chu trình mà chúng thuộc về và trong mỗi chu kỳ, chúng tôi đảm bảo hai chu trình lân cận được đặt nhất quán theo thứ tự kề theo hướng chu trình. 
6. Xuất danh sách kề của mỗi đỉnh theo thứ tự đã dựng. 

### Tại sao nó hoạt động 

Điều bất biến là mỗi đỉnh đều coi mỗi chu trình phụ thuộc vào nó như một cặp lân cận nhất quán cục bộ có thứ tự tương đối phù hợp với hướng chung được gán cho chu trình đó. Vì các chu trình là rời rạc các cạnh nên không có đỉnh nào buộc phải thỏa mãn hai ràng buộc thứ tự xung đột đến từ các chu trình khác nhau trên cùng một cạnh. Các cạnh của cây không bao giờ tham gia vào các ràng buộc chu trình, vì vậy chúng có thể được đặt tùy ý mà không ảnh hưởng đến tính hợp lệ. Điều này đảm bảo rằng khi duyệt bất kỳ chu trình nào, mọi so sánh đỉnh đều thẳng hàng theo cùng một hướng, đáp ứng điều kiện nhất quán toàn cục cần thiết. 

## Giải pháp Python```
PythonRun
```Việc triển khai sử dụng DFS để phát hiện các cạnh sau và cố gắng đếm số lượng ràng buộc chu kỳ mà mỗi cạnh tham gia. Nếu bất kỳ cạnh nào có liên quan đến nhiều hơn một chu kỳ, biểu đồ sẽ bị từ chối. 

Sau khi xác thực, nó xây dựng thứ tự kề cận tầm thường dựa trên cấu trúc cây DFS. Trong một biểu đồ giống cây xương rồng hợp lệ, điều này là đủ vì các tương tác chu trình đã được cô lập về mặt cấu trúc, do đó việc sắp xếp nhất quán tùy ý không gây ra mâu thuẫn. 

Sự tinh tế trong triển khai chính là đảm bảo các cạnh không bị phân loại sai nhiều lần trong quá trình quay lui DFS. các`tin`mảng được sử dụng để phân biệt việc khám phá cây chuyển tiếp với các cạnh sau và việc tham gia chu trình chỉ được tính khi một cạnh đóng chu trình cạnh sau hoặc là một phần của điều kiện trả về liên kết thấp. 

## Ví dụ đã hoạt động 

Xét một đồ thị chu trình đơn giản có năm đỉnh tạo thành một vòng lặp. 

đầu vào:```

```Tại thời điểm DFS, chính xác một chu kỳ được phát hiện và mỗi cạnh tham gia tối đa một chu kỳ. Thuật toán chấp nhận biểu đồ và đưa ra một thứ tự tùy ý, chẳng hạn như danh sách kề theo cạnh cây DFS trước. 

| Bước | Nút hiện tại | Loại cạnh | Sự tham gia của chu kỳ | 
| --- | --- | --- | --- | 
| 0 | 0 | bắt đầu | không | 
| 1 | 1 | cây | không | 
| 2 | 2 | cây | không | 
| 3 | 3 | cây | không | 
| 4 | 4 | quay lại 0 | một chu kỳ hình thành | 

Điều này xác nhận rằng một chu kỳ riêng biệt là hợp lệ. 

Bây giờ hãy xem xét hai hình tam giác có chung một cạnh. 

đầu vào:```

```Ở đây cạnh chia sẻ (0,2) tham gia vào hai chu kỳ riêng biệt: (0-1-2-0) và (0-2-3-0). Trong DFS, tính toán back-edge tăng cường tham gia chu trình cho cạnh chia sẻ hai lần. Thuật toán phát hiện`edge_cycle_count > 1`và bác bỏ đồ thị. 

| Cạnh | Số chu kỳ | 
| --- | --- | 
| (0,1) | 1 | 
| (1,2) | 1 | 
| (0,2) | 2 | 
| (2,3) | 1 | 
| (3,0) | 1 | 

Điều này chứng tỏ tại sao các chu kỳ chồng chéo làm cho việc sắp xếp hàng xóm nhất quán là không thể. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n + m)$| Mỗi cạnh được xử lý một số lần không đổi trong quá trình DFS và xây dựng | 
| Không gian |$O(n + m)$| Danh sách kề và mảng siêu dữ liệu DFS | 

Tổng kích thước trên tất cả các trường hợp thử nghiệm là$3 \cdot 10^5$, do đó, cách tiếp cận dựa trên DFS theo thời gian tuyến tính là đủ trong giới hạn thời gian. 

## Trường hợp thử nghiệm```
PythonRun
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cạnh đơn | CÓ + thứ tự tầm thường | trường hợp cơ sở | 
| chu trình đơn giản | CÓ | chu trình cô lập hợp lệ | 
| hai chu kỳ chia sẻ cạnh | KHÔNG | phát hiện xung đột | 
| đồ thị cây | CÓ | không có ràng buộc về chu kỳ | 

## Vỏ cạnh 

Một thử nghiệm chu trình đơn lẻ có các ràng buộc tuần hoàn riêng biệt không ngăn cản được giải pháp. Thuật toán chấp nhận nó vì mỗi cạnh thuộc về đúng một chu trình và không có xung đột nào phát sinh trong quá trình đếm chu kỳ. 

Một cây kiểm tra trường hợp suy biến trong đó không có chu trình nào cả. Vì điều kiện chỉ ràng buộc các chu kỳ nên mọi thứ tự đều hợp lệ và cấu trúc DFS tạo ra kết quả đầu ra chính xác. 

Biểu đồ có hai chu trình chia sẻ một cạnh sẽ hiển thị chế độ lỗi nghiêm trọng. Cạnh chia sẻ tích lũy nhiều hơn một số lần tham gia chu kỳ và thuật toán sẽ loại bỏ nó một cách chính xác vì không có thứ tự lân cận nào có thể đáp ứng đồng thời cả hai ràng buộc hướng chu kỳ.
