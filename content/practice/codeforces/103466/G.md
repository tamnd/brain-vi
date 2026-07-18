---
title: "CF 103466G - Trò Chơi Poker"
description: "Đây là một giải đấu poker mô phỏng trong đó năm người chơi có chiến lược cố định liên tục tham gia vào một loạt vòng đấu độc lập."
date: "2026-07-03T06:49:23+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103466
codeforces_index: "G"
codeforces_contest_name: "The 2019 ICPC Asia Nanjing Regional Contest"
rating: 0
weight: 103466
solve_time_s: 31
verified: false
draft: false
---

[CF 103466G - Trò chơi Poker](https://codeforces.com/problemset/problem/103466/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 31s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Đây là một giải đấu poker mô phỏng trong đó năm người chơi có chiến lược cố định liên tục tham gia vào một loạt vòng đấu độc lập. Trong mỗi vòng, một chuỗi các lá bài được xáo trộn đã được ấn định trước và người chia bài sẽ phân chia chúng một cách xác định: mỗi người chơi vẫn đang hoạt động sẽ nhận được hai lá bài riêng theo thứ tự, tiếp theo là các lá bài chung được chia theo từng giai đoạn, với các vòng đặt cược ở giữa. 

Mỗi người chơi bắt đầu mỗi vòng với một chồng chip cố định, nhưng có thể mất chip khi gọi các cược có kích thước cố định trong tối đa ba vòng đặt cược. Nếu một người chơi đạt đến 0 chip, họ sẽ bị loại khỏi tất cả các vòng trong tương lai. Điều duy nhất chúng ta phải tính toán là tổng số chip mà mỗi người chơi đã tích lũy được trong tất cả các vòng sau khi mô phỏng tất cả các quyết định và khoản thanh toán. 

Một vòng có một chuỗi các quyết định: đặt cược trước vòng, đặt cược ở vòng và đặt cược trên sông. Ở mỗi giai đoạn, người chơi có thể bỏ bài hoặc gọi cược dựa trên các quy tắc riêng của họ và trong một số trường hợp có thể chơi hết mình nếu số tiền của họ nhỏ. Nếu tại bất kỳ thời điểm nào chỉ còn lại một người chơi, họ sẽ ngay lập tức giành được tất cả số chip đã cam kết cho vòng đó. Mặt khác, nếu nhiều người chơi sống sót đến cuối cùng, một cuộc đối đầu sẽ xảy ra và ván bài poker tốt nhất trong số bảy lá bài của họ sẽ quyết định người chiến thắng, với việc hòa quyết định sẽ nghiêng về người chơi theo thứ tự lượt sau. 

Các ràng buộc về cơ bản được ẩn giấu bên trong sự phức tạp của mô phỏng. Có nhiều nhất 1000 vòng và chỉ có năm người chơi. Mỗi vòng bao gồm việc đánh giá poker theo thời gian liên tục cho một số ván bài 7 lá và một số lần kiểm tra quy tắc cố định. Điều này có nghĩa là giải pháp dự định là một mô phỏng đơn giản, chú ý cẩn thận đến tính chính xác của thứ hạng ván bài poker và logic quyết định của người chơi. Bất kỳ giải pháp nào cố gắng tìm kiếm tổ hợp đối với các quyết định hoặc kết quả tay sẽ không cần thiết và có nguy cơ phức tạp trong việc triển khai mà không mang lại lợi ích về hiệu suất. 

Các trường hợp thất bại tinh tế chính đến từ các chi tiết so sánh ván bài poker. Các quy tắc thẳng, đặc biệt là các trường hợp cạnh A2345 và TJQKA, yêu cầu xử lý cẩn thận. Một cạm bẫy phổ biến khác là xử lý những người chơi tất tay: sau khi tất tay, họ không còn tham gia vào logic cá cược nhưng vẫn góp phần đánh giá trận đấu. Cuối cùng, việc hòa phụ thuộc vào thứ tự của người chơi, thứ tự này phải được bảo toàn chính xác. 

Ví dụ về tình huống thất bại: nếu A2345 được coi là có quân Át là quân bài cao thay vì 5 để so sánh trình tự, thì việc so sánh với các quân bài khác sẽ trở nên không nhất quán, dẫn đến sai người chiến thắng trong các tình huống có quân bài thấp. 

Một ví dụ khác: nếu tuôn ra và thẳng không được thống nhất thành một loại mạnh hơn (xả thẳng được ngụ ý theo quy tắc), thứ tự độ mạnh của tay sẽ trở nên không chính xác trong các bộ đồ như A2345, tất cả cùng một bộ, được coi rõ ràng là cấu trúc đặc biệt mạnh nhất trong phân đoạn đó. 

## Phương pháp tiếp cận 

Ý tưởng ngây thơ là mô phỏng rõ ràng từng bước từng vòng, duy trì trạng thái của người chơi, chia bài, đánh giá các quyết định một cách tuần tự và tính toán kết quả cuối cùng. Vì mỗi vòng có số lượng người chơi và hành động cố định nên mô phỏng này đã đủ hiệu quả. Sự phức tạp duy nhất nằm ở việc thực hiện chính xác các điều kiện đánh giá và chiến lược poker. 

Nút thắt bạo lực sẽ chỉ xuất hiện nếu chúng tôi cố gắng liệt kê các kết quả có thể xảy ra của các quyết định bỏ bài hoặc kết hợp các quân bài. Điều đó là không cần thiết vì tất cả tính ngẫu nhiên đều bị loại bỏ: thứ tự và chiến lược quân bài đều mang tính quyết định. Do đó, toàn bộ vấn đề giảm xuống mô phỏng trạng thái xác định. 

Cái nhìn sâu sắc quan trọng là vấn đề không mang tính chất tổ hợp mặc dù chủ đề poker của nó. Đây là một bài toán mô phỏng công cụ quy tắc với yêu cầu cao: đánh giá chính xác các ván bài poker 7 lá và tuân thủ nghiêm ngặt logic quyết định của người chơi. Khi chúng tôi lập mô hình một vòng một cách chính xác, mọi thứ khác sẽ tuyến tính về số vòng. 

Giải pháp tối ưu chỉ đơn giản là:

mô phỏng các vòng → mô phỏng các giai đoạn đặt cược → theo dõi ngăn xếp → đánh giá ván bài → chỉ định tiền cược. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force về kết quả | hàm mũ | cao | Quá chậm | 
| Mô phỏng xác định đầy đủ | O(n) vòng × O(1) công việc | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi mô phỏng từng vòng một cách độc lập, duy trì ngăn xếp chip hiện tại và trạng thái hoạt động. 

### 1. Khởi tạo người chơi 

Mỗi người chơi bắt đầu với 100 chip và được đánh dấu là đang hoạt động. Khi một người chơi đạt đến số 0, họ sẽ bị loại vĩnh viễn khỏi các vòng chơi trong tương lai. 

### 2. Chia bài theo thứ tự 

Đối với mỗi vòng, lặp lại qua những người chơi đang hoạt động theo thứ tự cố định và gán cho mỗi người hai thẻ theo trình tự được tính toán trước. Sau đó chia bài cộng đồng ở các giai đoạn nhất định. 

Thứ tự quan trọng vì những so sánh sau này dựa vào thứ tự chỗ ngồi xác định. 

### 3. Đặt cược pre-flop 

Mỗi người chơi theo thứ tự quyết định dựa trên quy tắc riêng của họ. Nếu họ bỏ bài, họ sẽ rời khỏi vòng chơi ngay lập tức. Nếu họ call, trừ 5 chip. Nếu họ tất tay, toàn bộ số tiền của họ sẽ được cam kết và họ sẽ bị “khóa” đối với logic đặt cược còn lại. 

Bước này xác định bộ sinh tồn ban đầu cho vòng chơi. 

### 4. Cược Flop 

Thẻ cộng đồng tăng lên 3 thẻ hiển thị. Những người chơi còn lại quyết định lại dựa trên quy tắc của họ. Chip cũng bị trừ tương tự trừ khi tất tay. 

Bước này thường loại bỏ sớm những ván bài yếu và thay đổi kích thước pot. 

### 5. Cá cược sông 

Thẻ cộng đồng trở thành 5. Quyết định đặt cược cuối cùng được áp dụng. Sau giai đoạn này, những người chơi sống sót sẽ tiếp tục đối đầu trừ khi chỉ còn lại một người. 

### 6. Kiểm tra chấm dứt sớm 

Nếu ở bất kỳ giai đoạn nào chỉ còn lại một người chơi, họ sẽ giành được tất cả số chip đã cam kết ngay lập tức. Không cần đánh giá tay. 

Đây là điều kiện tối ưu hóa và chính xác quan trọng: chỉ riêng cấu trúc cá cược có thể kết thúc vòng chơi. 

### 7. Đánh giá trận đấu 

Nếu vẫn còn nhiều người chơi, hãy tính ván bài poker 5 lá tốt nhất từ 7 lá bài của mỗi người chơi. So sánh bàn tay bằng cách sử dụng: 

Đầu tiên, xếp hạng danh mục (thẻ cao < cặp < ba loại < thẳng < tuôn ra). 

Sau đó, hòa bằng cách sử dụng các quy tắc quân bài quan trọng, xử lý cẩn thận A2345 trong đó quân bài cao nhất hiệu quả là 5, không phải Át. 

Nếu hòa, người chơi ở vị trí sau trong thứ tự chỗ ngồi sẽ thắng. 

### 8. Nồi thưởng 

Người chiến thắng nhận được tất cả số chip đã cam kết trong vòng đó. bạn
