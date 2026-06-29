---
title: "CF 104618G - Cờ bạc kem"
description: "Chúng tôi được cấp hai bộ sưu tập độc lập tương tác thông qua quy trình giao dịch. Một bộ sưu tập đại diện cho khách hàng, mỗi khách hàng $i$ sẵn sàng trả $ri$ nếu họ nhận thành công kem sô-cô-la-bạc hà."
date: "2026-06-29T17:30:28+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104618
codeforces_index: "G"
codeforces_contest_name: "UTPC Contest 09-22-23 Div. 1"
rating: 0
weight: 104618
solve_time_s: 33
verified: false
draft: false
---

[CF 104618G - Cờ bạc kem](https://codeforces.com/problemset/problem/104618/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 33s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp hai bộ sưu tập độc lập tương tác thông qua quy trình giao dịch. Một bộ sưu tập đại diện cho khách hàng, mỗi khách hàng$i$sẵn sàng trả tiền$r_i$nếu họ nhận được kem sô cô la bạc hà thành công. Bộ sưu tập còn lại đại diện cho các hình nón mà Greg đã tìm thấy, trong đó mỗi hình nón$j$có chi phí mua hàng$c_j$. Chúng ta phải quyết định nên mua loại nón nào và phục vụ khách hàng nào. 

Điều khó hiểu là hình nón là “không xác định”: hình nón có thể có hoặc không phải là sô cô la-bạc hà. Nếu chúng tôi giao một hình nón cho một khách hàng, chúng tôi sẽ luôn phục vụ nó, nhưng lợi nhuận phụ thuộc vào việc hình nón đó có hợp lệ hay không. Trò cờ bạc của Alex giới thiệu khả năng đặt cược xem hình nón có phải là sô cô la bạc hà hay không, điều này cho phép chúng ta vô hiệu hóa sự không chắc chắn một cách hiệu quả một cách tối ưu, nhưng không miễn phí. Kết quả cuối cùng là mỗi nhiệm vụ đóng góp một giá trị được đảm bảo xác định bắt nguồn từ sự kết hợp giữa giá trị khách hàng và chi phí hình nón. 

Nhiệm vụ là tối đa hóa lợi nhuận được đảm bảo và trong số mọi cách để đạt được lợi nhuận tối đa đó, hãy tối đa hóa số lượng khách hàng được phục vụ. Chúng ta cũng phải đếm xem có bao nhiêu cấu hình phân công đạt được cả hai mục tiêu. 

Những hạn chế$N, M \le 10^5$ngay lập tức loại trừ bất kỳ giải pháp đối sánh bậc hai hoặc dựa trên luồng nào trên biểu đồ lưỡng cực đầy đủ. Bất kỳ cách tiếp cận nào xem xét rõ ràng tất cả các cặp hình nón khách hàng sẽ yêu cầu$10^{10}$hoạt động, điều đó là không thể thực hiện được. Điều này thúc đẩy chúng ta hướng tới việc sắp xếp, ghép nối tham lam và đếm tổ hợp. 

Một trường hợp phức tạp xuất hiện khi tất cả chi phí đều cao so với giá trị. Một kẻ tham lam ngây thơ có thể quyết định tránh hoàn toàn việc khớp lệnh, nhưng vấn đề đảm bảo rằng không làm gì sẽ mang lại mức lợi nhuận cơ bản bằng 0 hợp lệ. Một trường hợp khác là khi nhiều hình nón hoặc khách hàng chia sẻ các giá trị hoặc chi phí giống hệt nhau, điều này ảnh hưởng đến việc tính toán các nhiệm vụ tối ưu vì hoán vị giữa các phần tử bằng nhau vẫn tạo ra những cách riêng biệt. 

## Phương pháp tiếp cận 

Nếu chúng tôi thử sức mạnh, chúng tôi sẽ chỉ định cho mỗi khách hàng không có hình nón hoặc một trong các hình nón có sẵn và mỗi hình nón nhiều nhất là một lần. Đối với mỗi tập hợp nhiệm vụ, chúng tôi sẽ tính toán lợi nhuận được đảm bảo, sau đó theo dõi kết quả tốt nhất. Số lượng tập hợp con của các cặp là tổ hợp trong cả hai$N$Và$M$, tăng nhanh hơn$2^{100000}$, vì vậy điều này là hoàn toàn không thể. 

Quan sát cấu trúc quan trọng là chỉ có thứ tự tương đối mới quan trọng. Mỗi khách hàng đóng góp độc lập dựa trên việc chúng tôi có giao cho họ một hình nón hay không và chúng tôi chọn hình nón nào. Vì các hình nón có thể hoán đổi cho nhau ngoại trừ chi phí của chúng và khách hàng có thể hoán đổi cho nhau ngoại trừ phần thưởng của họ, nên cấu trúc tối ưu sẽ xuất hiện sau khi sắp xếp cả hai mảng. 

Ý tưởng quan trọng là chúng tôi kết hợp hiệu quả những khách hàng có giá trị cao với những chiếc nón có chi phí thấp. Bất kỳ sự sai lệch nào khi ghép một khách hàng có giá trị cao với một chiếc nón đắt tiền hơn trong khi không sử dụng một cặp tốt hơn chỉ có thể làm giảm lợi nhuận được đảm bảo. Điều này biến vấn đề thành một sự kết hợp giữa các chuỗi được sắp xếp trong đó các giải pháp tối ưu tôn trọng thứ tự: lớn nhất$r_i$phải phù hợp với giá trị nhỏ nhất$c_j$trong tiền tố được kiểm soát. 

Khi chúng tôi sắp xếp cả hai mảng, chúng tôi có thể xem xét việc lấy chính xác$k$khách hàng và$k$hình nón. Đối với một cố định$k$, cấu trúc được đảm bảo tốt nhất là ghép nối$k$lớn nhất$r_i$với$k$nhỏ nhất$c_j$. Điều này làm giảm vấn đề đánh giá một hàm trên$k$, tối đa hóa lợi nhuận và tối đa hóa giữa các mối quan hệ$k$, sau đó đếm các hoán vị nhận ra cùng một cấu trúc tối ưu. 

Việc đếm phát sinh vì trong các khối có giá trị bằng nhau, nhiều nhiệm vụ mang lại lợi nhuận giống nhau. Chúng ta phải nhân các lựa chọn của các hình nón giống nhau và các khách hàng giống nhau bằng cách sử dụng các hệ số tổ hợp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(N + M) | Quá chậm | 
| Sắp xếp tối ưu + Ghép đôi tham lam + Tổ hợp |$O(N \log N + M \log M)$| O(N + M) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### 1. Sắp xếp cả hai mảng 

Sắp xếp giá trị khách hàng$r$theo thứ tự giảm dần và chi phí hình nón$c$theo thứ tự tăng dần. Điều này giúp sắp xếp những khách hàng tốt nhất với những chiếc nón rẻ nhất hiện có theo bất kỳ sự kết hợp tối ưu nào. 

### 2. Tính tiền tiền tố 

Xây dựng tổng tiền tố cho cả hai mảng để chúng tôi có thể nhanh chóng đánh giá tổng lợi nhuận đóng góp cho bất kỳ độ dài tiền tố nào$k$. 

### 3. Đánh giá tất cả các kích thước phù hợp có thể 

Đối với mỗi$k$từ$0$ĐẾN$\min(N, M)$, tính toán lợi nhuận ứng viên phù hợp với top$k$khách hàng với giá rẻ nhất$k$hình nón. Cấu trúc đảm bảo rằng mọi giải pháp tối ưu về kích thước$k$phải trông như thế này, vì việc hoán đổi bất kỳ cặp thứ tự không khớp nào sẽ làm tăng chi phí hoặc giảm phần thưởng. 

Lợi nhuận cố định$k$được tính từ các tổng tiền tố như sau:$$\text{profit}(k) = \sum_{i=1}^{k} r_i^{(desc)} - \sum_{j=1}^{k} c_j^{(asc)}$$### 4. Theo dõi lợi nhuận tốt nhất và kích thước tốt nhất 

Duy trì lợi nhuận tối đa được thấy cho đến nay. Nếu tìm thấy lợi nhuận lớn hơn, hãy đặt lại kích thước tốt nhất. Nếu tìm thấy lợi nhuận tương tự, thích lớn hơn$k$. 

### 5. Đếm các bài tập tối ưu 

Đối với phương án tối ưu đã chọn$k$, chúng tôi đếm có bao nhiêu cách chúng tôi có thể chọn cái nào$k$khách hàng và$k$hình nón tạo thành cấu trúc ghép đôi tối ưu. 

Điều này phụ thuộc vào sự đa dạng. Giả sử nằm trong top đầu$k$khách hàng có những giá trị lặp lại; bất kỳ hoán vị nào của các giá trị giống hệt nhau đều không làm thay đổi kết quả. Tương tự cho hình nón. 

Do đó, số kết quả phù hợp hợp lệ là:$$\frac{k!}{\prod \text{freq of equal } r} \times \frac{k!}{\prod \text{freq of equal } c}$$modulo$10^9+7$, được điều chỉnh cẩn thận để có sự đối xứng có giá trị giống hệt nhau. 

### Tại sao nó hoạt động 

Bất biến cốt lõi là trong bất kỳ giải pháp tối ưu nào, các cặp khớp phải tôn trọng thứ tự sắp xếp. Nếu tồn tại một cặp mà phần thưởng lớn hơn được khớp với một hình nón đắt tiền hơn một cặp chưa sử dụng khác, việc hoán đổi chúng sẽ cải thiện đáng kể hoặc bảo toàn lợi nhuận. Việc lặp lại đối số trao đổi này sẽ loại bỏ tất cả các phép đảo ngược, buộc cấu trúc tối ưu phải được thực hiện.
