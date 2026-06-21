---
title: "CF 1057C - Tanya và kẹo màu"
description: "Chúng ta được cấp một hàng hộp kẹo, mỗi hộp được đặt ở một tọa độ nguyên. Mỗi hộp chứa một số lượng kẹo cố định, tất cả đều có cùng màu và mỗi hộp có màu đỏ, xanh lá cây hoặc xanh lam. Chúng tôi bắt đầu ở một hộp cụ thể."
date: "2026-06-15T13:04:39+07:00"
tags: ["codeforces", "competitive-programming", "*special", "dp"]
categories: ["algorithms"]
codeforces_contest: 1057
codeforces_index: "C"
codeforces_contest_name: "Mail.Ru Cup 2018 - Practice Round"
rating: 2000
weight: 1057
solve_time_s: 186
verified: false
draft: false
---

[CF 1057C - Tanya và kẹo màu](https://codeforces.com/problemset/problem/1057/C) 

**Xếp hạng:** 2000 
**Thẻ:** *đặc biệt, dp 
**Thời gian giải:** 3 phút 6s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một hàng hộp kẹo, mỗi hộp được đặt ở một tọa độ nguyên. Mỗi hộp chứa một số lượng kẹo cố định, tất cả đều có cùng màu và mỗi hộp có màu đỏ, xanh lá cây hoặc xanh lam. Chúng tôi bắt đầu ở một hộp cụ thể. Từ đó, chúng ta có thể đi sang trái hoặc phải đến hộp liền kề, tốn một giây cho mỗi lần di chuyển hoặc chúng ta có thể tiêu thụ ngay tất cả kẹo trong hộp hiện tại. 

Quá trình ăn bị hạn chế bởi hai điều kiện áp dụng cho chuỗi hộp mà kẹo được ăn. Đầu tiên, các hộp ăn liền nhau phải có màu sắc khác nhau. Thứ hai, số lượng kẹo trong mỗi hộp mới ăn phải vượt quá hộp đã ăn trước đó. Chúng ta muốn chọn một chuỗi các hộp đã ăn thỏa mãn các ràng buộc này sao cho tổng số kẹo thu được ít nhất là k, đồng thời giảm thiểu tổng thời gian đi bộ. 

Cấu trúc chính là việc ăn uống là miễn phí về mặt thời gian, nhưng không có chuyển động, vì vậy chi phí phụ thuộc hoàn toàn vào cách chúng ta điều hướng đường để đạt đến dãy con có màu xen kẽ, tăng dần hợp lệ. 

Các ràng buộc n 50 và k 2000 gợi ý rõ ràng rằng chúng ta được phép xem xét quy hoạch động khá dày đặc trên các tập hợp con hoặc vị trí, nhưng không được phép khám phá theo cấp số nhân của các chuỗi đầy đủ không có cấu trúc. Một tìm kiếm theo cấp số nhân đơn giản trên tất cả các chuỗi hộp đã ăn sẽ tăng lên khoảng 3^n hoặc tệ hơn, điều này là không cần thiết do các ràng buộc về thứ tự mạnh mẽ. 

Một trường hợp phức tạp phát sinh khi giải pháp tốt nhất yêu cầu bỏ qua các hộp gần đó “đẹp mắt”. Ví dụ: một hộp có nhiều kẹo có thể không thể sử dụng được sớm nếu màu của nó trùng với màu được ăn lần cuối hoặc nếu số lượng kẹo của nó vi phạm mức tăng nghiêm ngặt. Cách tiếp cận tham lam luôn chọn ô hợp lệ gần nhất có thể thất bại. 

Một dạng thất bại khác là giả định rằng một khi chúng ta chọn một dãy hộp tăng dần hợp lệ, chúng ta có thể tham lam bước đi giữa chúng mà không cần xem xét lại các quyết định trước đó. Bởi vì chi phí di chuyển phụ thuộc vào định vị toàn cầu chứ không chỉ tính hợp lệ của trình tự, nên các quyết định cục bộ có thể dẫn đến các tuyến đường di chuyển dưới mức tối ưu. 

## Phương pháp tiếp cận 

Một cách giải thích thô bạo là hãy tưởng tượng việc chọn mọi dãy hộp có thể có để ăn, kiểm tra xem nó có thỏa mãn sự xen kẽ màu sắc và ràng buộc tăng dần nghiêm ngặt hay không, sau đó tính toán chi phí đi bộ tối thiểu cần thiết để đến thăm chúng theo thứ tự bắt đầu từ s. Ngay cả khi chúng ta sửa một chuỗi, việc tính toán chi phí di chuyển tối thiểu là không hề nhỏ vì chúng ta có thể di chuyển sang trái và phải một cách tự do và có thể đi qua các hộp đã được thăm. 

Số lượng chuỗi hợp lệ là số mũ tính bằng n. Ngay cả khi chúng ta giới hạn bản thân ở các dãy con, chúng ta vẫn phải đối mặt với một không gian trạng thái khổng lồ. Cách tiếp cận này trở nên không khả thi khi ở khoảng n = 50. 

Quan sát quan trọng là cấu trúc có ý nghĩa duy nhất của một “trạng thái” là chiếc hộp được ăn cuối cùng. Khi chúng ta biết chiếc hộp cuối cùng mà chúng ta đã ăn, mọi quyết định trong tương lai chỉ phụ thuộc vào vị trí, màu sắc và số lượng kẹo của nó. Điều này gợi ý một công thức lập trình động trên các điểm cuối. 

Chúng tôi xác định sự chuyển tiếp dp giữa các cặp hộp i → j, trong đó j là hộp được ăn tiếp theo hợp lệ sau i nếu màu của nó khác và số lượng kẹo của nó lớn hơn. Chi phí chuyển đổi là khoảng cách đi bộ ngắn nhất giữa vị trí hiện tại và j. Tuy nhiên, chúng ta cũng phải tính đến thực tế là sau khi ăn ở vị trí i, vị trí của chúng ta trở thành i, do đó chuyển động chỉ đơn giản là |i − j|. 

Điều này làm giảm vấn đề tìm đường dẫn có chi phí tối thiểu trong biểu đồ có hướng trong đó các nút là các hộp và các cạnh tôn trọng số lượng kẹo tăng dần và sự xen kẽ màu sắc, đồng thời trọng số cạnh là khoảng cách tuyệt đối giữa các chỉ số. Chúng ta cũng cần cho phép bắt đầu từ vị trí ban đầu s, có thể di chuyển tự do đến ô được chọn đầu tiên.

Bởi vì chúng tôi cũng theo dõi tổng số kẹo được thu thập nên chúng tôi cần một thông số bổ sung: cho đến nay có bao nhiêu viên kẹo đã được ăn. Tuy nhiên, vì k 2000 và mỗi hộp có nhiều nhất 50 viên kẹo nên kích thước kiểu ba lô là khả thi. 

Chúng ta định nghĩa dp[i][t] là thời gian tối thiểu để kết thúc ở hộp tôi đã ăn đúng t chiếc kẹo. Quá trình chuyển đổi xem xét tất cả các hộp trước đó j với số lượng kẹo nhỏ hơn và màu sắc khác nhau. Từ dp[j][t − r[i]] chúng ta cập nhật dp[i][t] với chi phí dp[j][t − r[i]] + |i − j|. Trạng thái ban đầu cho phép bắt đầu từ s với cost |s − i| và t = r[i]. 

Điều này tạo ra một lớp DP rõ ràng trên các hộp và tổng số kẹo. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force theo trình tự | hàm mũ | O(n) | Quá chậm | 
| Trạng thái DP qua (hộp, kẹo) | O(n^2 * k) | O(nk) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chúng tôi coi mỗi hộp là điểm cuối tiềm năng của trình tự ăn uống hợp lý ngày càng tăng. Đối với mỗi hộp thứ i, chúng tôi ghi lại số lượng và màu sắc kẹo của nó. Điều này thiết lập các đối tượng mà quá trình chuyển đổi sẽ xảy ra. 
2. Chúng ta tạo một bảng DP trong đó dp[i][c] biểu thị thời gian đi bộ tối thiểu cần thiết để kết thúc ở hộp i sau khi đã ăn đúng c viên kẹo. Chúng tôi khởi tạo tất cả các giá trị thành vô cùng vì chúng tôi đang giảm thiểu. 
3. Đối với mỗi hộp i, chúng ta khởi tạo trạng thái bắt đầu một chuỗi trực tiếp từ vị trí bắt đầu s. Nếu chúng ta ăn hộp i trước thì thời gian tốn là |s − i| và số kẹo trở thành r[i]. Điều này nắm bắt tất cả các bước di chuyển đầu tiên hợp lệ. 
4. Sau đó chúng tôi xem xét việc mở rộng chuỗi. Với mỗi cặp hộp (j, i), trong đó r[i] > r[j] và c[i] ≠ c[j], chúng ta cho phép chuyển đổi từ j sang i. Chi phí của quá trình chuyển đổi này là dp[j][c − r[i]] + |i − j|. Sự khác biệt tuyệt đối phản ánh việc đi bộ từ j đến i sau khi kết thúc ở j. 
5. Chúng tôi lặp lại tổng số kẹo theo thứ tự tăng dần để khi xử lý dp
