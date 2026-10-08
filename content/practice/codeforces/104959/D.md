---
title: "CF 104959D - Ký ức lịch sử"
description: "Lục địa là cây của các thành phố. Đi dọc theo bất kỳ con đường nào cũng mất đúng một năm và vì đồ thị là một cái cây nên có một con đường đơn giản duy nhất giữa hai thành phố bất kỳ."
date: "2026-06-28T07:02:07+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104959
codeforces_index: "D"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2023-2024, \u041f\u0435\u0440\u0432\u0430\u044f \u043b\u0438\u0447\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430"
rating: 0
weight: 104959
solve_time_s: 33
verified: false
draft: false
---

[CF 104959D - Ký ức lịch sử](https://codeforces.com/problemset/problem/104959/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 33s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Lục địa là cây của các thành phố. Đi dọc theo bất kỳ con đường nào cũng mất đúng một năm và vì đồ thị là một cái cây nên có một con đường đơn giản duy nhất giữa hai thành phố bất kỳ. Mỗi thành phố bắt đầu bằng một “giá trị bộ nhớ” ban đầu$s_i$và giá trị này thường giảm đi 1 mỗi năm cho đến khi đạt 0. 

Điều khó khăn là mỗi thành phố có thể tạm thời ngăn chặn sự phân hủy này. Khi một sự kiện “trừ” xảy ra tại một thành phố vào thời điểm đó$t$, thành phố đó sẽ bị đóng băng từ năm đó trở đi và giá trị bộ nhớ của nó không còn giảm nữa. Khi một sự kiện “cộng” xảy ra sau đó, quá trình phân hủy sẽ tiếp tục diễn ra từ năm đó trở đi. Các sự kiện dành cho một thành phố thay thế và sự kiện đầu tiên luôn là một điểm trừ, do đó, mỗi thành phố có một chuỗi khoảng thời gian trong đó quá trình phân rã của nó hoạt động hoặc tạm dừng. 

Một truy vấn hỏi: bắt đầu từ một thành phố nhất định tại một thời điểm nhất định, bạn đi dọc theo các cạnh của cây và bạn muốn giá trị bộ nhớ tối đa mà bạn có thể gặp ở bất kỳ thành phố nào dọc theo bất kỳ con đường nào bạn đi qua. Bạn không bị hạn chế về thời gian đi bộ; cấu trúc duy nhất là thời gian di chuyển bằng số cạnh, do đó việc đến một nút muộn hơn có nghĩa là có nhiều sự phân rã đã xảy ra trên toàn cầu theo thời gian. 

Khó khăn chính là giá trị của một thành phố phụ thuộc vào thời gian truy vấn và vào việc phân rã của nó hiện có hoạt động hay không, điều này thay đổi theo thời gian thông qua các bản cập nhật. Vì có tới$10^5$thành phố và$10^5$các sự kiện và truy vấn xen kẽ theo thứ tự thời gian, bất kỳ giải pháp nào tính toán lại các giá trị hoặc tính toán lại đường dẫn cho mỗi truy vấn đều không khả thi ngay lập tức. 

Một cách tiếp cận đơn giản, đối với mỗi truy vấn, sẽ tính toán lại các giá trị hiện tại của tất cả các nút tại một thời điểm$t$, sau đó chạy duyệt cây từ nút bắt đầu. Điều đó đã tốn kém rồi$O(n)$cho mỗi truy vấn, sẽ trở thành$10^{10}$hoạt động trong trường hợp xấu nhất, vượt xa giới hạn. 

Một trường hợp thất bại tinh tế hơn đến từ việc bỏ qua thời gian di chuyển. Nếu một nút ở khoảng cách 5 so với điểm bắt đầu và thời gian truy vấn nhỏ thì giá trị hiệu quả của nó sẽ được tính sau nếu chúng ta thực sự tiếp cận được nó. Việc xử lý tất cả các nút như thể được đánh giá ở cùng một thời điểm sẽ phá vỡ tính chính xác. 

Thách thức là kết hợp các trọng số nút phụ thuộc vào thời gian động với các truy vấn khoảng cách cây theo cách tránh tính toán lại các trạng thái toàn cầu cho mỗi truy vấn. 

## Phương pháp tiếp cận 

Ý tưởng về vũ lực rất đơn giản. Đối với mỗi truy vấn, chúng tôi tính toán giá trị hiệu dụng hiện tại của mỗi nút bằng cách mô phỏng sự phân rã của nó từ thời điểm 0 cho đến thời điểm truy vấn, tôn trọng các khoảng thời gian hoạt động hoặc cố định. Sau đó, chúng tôi chạy DFS hoặc BFS từ nút bắt đầu, tính toán giá trị tốt nhất có thể truy cập được. Điều này đúng vì nó mô phỏng trực tiếp định nghĩa của quy trình. Tuy nhiên, việc tính toán lại tất cả các giá trị nút trên mỗi truy vấn sẽ tốn chi phí$O(n)$, và việc đi qua cây sẽ tốn thêm chi phí$O(n)$, cho$O(nq)$, quá chậm. 

Quan sát chính là giá trị của nút tại thời điểm$t$không phải là tùy tiện. Giữa các sự kiện, nó là một hàm tuyến tính theo thời gian có độ dốc$-1$hoặc$0$, bị cắt ở mức 0. Vì vậy, mỗi nút đóng góp một hàm tuyến tính từng phần theo thời gian. Giá trị tại thời điểm truy vấn được xác định bằng cách đánh giá các hàm này. 

Bây giờ hãy xem xét cấu trúc cây. Đường đi từ gốc đến bất kỳ nút nào đều tích lũy khoảng cách, nhưng khoảng cách chỉ quan trọng vì nó thay đổi thời gian đánh giá hiệu quả. Nếu chúng ta xác định rằng việc tiếp cận một nút ở khoảng cách$d$có nghĩa là đánh giá nó vào thời điểm đó$t + d$, thì mỗi nút đóng góp một hàm có dạng:$$f_i(t + d) = \max(0, s_i - \text{decay}(i, t + d))$$Vì vậy, mỗi truy vấn giảm xuống: trên tất cả các nút$i$, cực đại hóa hàm của$d(i)$Và$t$, Ở đâu$d(i)$là khoảng cách của cây từ đầu. 

Cấu trúc này gợi ý một phép biến đổi tiêu chuẩn: lấy gốc cây và chuyển đổi nó thành khoảng cách, sau đó coi mỗi nút đóng góp một dòng về mặt$d$tại một tham số thời gian đã dịch chuyển. Vì các truy vấn yêu cầu đường dẫn tối đa, điều này trở thành vấn đề truy vấn cây tối đa động trên không gian hàm tăng cường khoảng cách. 

Cách cổ điển để xử lý vấn đề này là phân rã centroid. Mỗi nút đóng góp giá trị của nó cho nhiều lớp trung tâm. Đối với mỗi centroid, chúng tôi duy trì một cấu trúc lưu trữ các giá trị tốt nhất dưới dạng hàm của khoảng cách. Cập nhật chỉ ảnh hưởng$O(\log n)$centroid và các truy vấn tương tự kết hợp sự đóng góp từ tổ tiên của centroid. 

Tại mỗi trung tâm, chúng tôi duy trì cấu trúc phụ thuộc vào thời gian để theo dõi sự đóng góp của nút theo khoảng cách, thường sử dụng cây phân đoạn hoặc nhiều tập hợp trên các nhóm khoảng cách, với các cập nhật lười biếng tương ứng với khoảng thời gian phân rã. Bởi vì sự phân rã chỉ thay đổi vào những thời điểm diễn ra sự kiện nên chúng tôi xử lý thời gian theo thứ tự và duy trì các đóng góp tích cực tăng dần. 

Điều này làm giảm mỗi lần cập nhật và truy vấn xuống$O(\log^2 n)$, đến từ độ sâu phân hủy trung tâm và cấu trúc phân đoạn trên mỗi trung tâm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(nq)$|$O(n)$| Quá chậm | 
| Phân rã Centroid + Giá trị động |$O(q \log^2 n)$|$O(n \log n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý các sự kiện theo thứ tự thời gian trong khi vẫn duy trì sự phân rã trung tâm của cây. 

1. Xây dựng phân tách trọng tâm của cây và tính toán trước, cho mỗi nút, khoảng cách của nó đến tất cả các trọng tâm trên đường phân rã của nó. Điều này cho phép cập nhật nhanh chóng tất cả các cấu trúc trung tâm bị ảnh hưởng bởi một nút. 
2. Đối với mỗi centroid, hãy duy trì một cấu trúc được khóa theo khoảng cách để lưu trữ sự đóng góp tốt nhất hiện tại của các nút trong cây con của nó. Sự đóng góp của một nút tại thời điểm$t$là giá trị bộ nhớ hiện tại của nó, nhưng được lưu trữ theo cách có thể cập nhật khi trạng thái phân rã thay đổi. 
3. Duy trì trạng thái suy tàn đang hoạt động hiện tại của mỗi thành phố khi chúng tôi xem qua các sự kiện. Mỗi thành phố xen kẽ giữa trạng thái phân rã đang hoạt động và trạng thái đóng băng, vì vậy chúng tôi theo dõi xem độ dốc của nó hiện có ổn định hay không.$-1$hoặc$0$. 
4. Khi xử lý một sự kiện “trừ” tại một thời điểm$t$, chúng tôi chuyển thành phố sang trạng thái đóng băng. Chúng tôi cập nhật tất cả các cấu trúc trung tâm chứa nút này bằng cách chèn hoặc cập nhật giá trị của nó tại thời điểm đó.$t$, đóng băng sự phân rã của nó một cách hiệu quả kể từ thời điểm đó trở đi. Điều này có nghĩa là các truy vấn trong tương lai sẽ coi giá trị của nó là không đổi kể từ thời điểm đó trở đi. 
5. Khi xử lý một sự kiện “cộng”, chúng tôi chuyển thành phố trở lại trạng thái hoạt động phân rã. Chúng tôi lại cập nhật cấu trúc trung tâm để phản ánh điều đó theo thời gian$t$, giá trị tiếp tục giảm tuyến tính. Điều này được xử lý bằng cách cập nhật hàm đóng góp được lưu trữ thay vì một đại lượng vô hướng. 
6. Đối với một truy vấn tại$(t, x)$, chúng ta đi qua đường trung tâm của nút$x$. Tại mỗi tâm, chúng tôi tính toán giá trị tốt nhất có thể được đóng góp bởi các nút có khoảng cách tới$x$được biết đến. Vì khoảng cách trong cây ban đầu có thể được phân tách thành khoảng cách thông qua tổ tiên của centroid, nên chúng tôi truy vấn từng cấu trúc centroid với quỹ khoảng cách còn lại thích hợp. 
7. Câu trả lời là giá trị tối đa trên tất cả các mức trọng tâm của giá trị thu được bằng cách kết hợp các đóng góp được lưu trữ của trọng tâm với độ lệch khoảng cách. 

### Tại sao nó hoạt động 

Mỗi nút đóng góp chính xác vào$O(\log n)$mức trung tâm và ở mỗi cấp độ đóng góp của nó được lưu trữ với độ lệch khoảng cách chính xác so với trung tâm đó. Bất kỳ đường dẫn nào từ nút truy vấn đến nút khác đều đi qua tổ tiên trung tâm chung thấp nhất của chúng trong quá trình phân tách, do đó khoảng cách được sử dụng trong đánh giá được xây dựng lại chính xác từ khoảng cách được tính toán trước. Vì các bản cập nhật chỉ sửa đổi cấu trúc trung tâm chứa nút bị ảnh hưởng và các truy vấn tổng hợp trên tất cả các phân vùng trung tâm có thể có của các đường dẫn từ nút truy vấn, nên mọi ứng cử viên đường dẫn hợp lệ đều được xem xét chính xác một lần trong một hệ tọa độ nhất quán. 

Tính đúng đắn dựa trên thực tế là việc phân tách centroid sẽ phân chia tất cả các đường đi thành các đoạn đi qua trọng tâm và khoảng cách của mỗi đoạn được xác định trước.
