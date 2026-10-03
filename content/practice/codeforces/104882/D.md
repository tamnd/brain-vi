---
title: "CF 104882D - Bánh nướng thơm ngon"
description: "Chúng ta được đưa cho một hàng bánh nướng được đánh số từ trái sang phải, trong đó mỗi chiếc bánh là bắp cải hoặc nấm. Hai em liên tục lấy những chiếc bánh từ hàng này theo quy tắc vị trí: một em nhắm vào chiếc bánh thứ 3 còn lại, đứa còn lại nhắm vào chiếc bánh thứ 7 còn lại."
date: "2026-06-28T09:18:07+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104882
codeforces_index: "D"
codeforces_contest_name: "Voronezh State University - Sitronics contest II"
rating: 0
weight: 104882
solve_time_s: 26
verified: false
draft: false
---

[CF 104882D - Bánh nướng thơm ngon](https://codeforces.com/problemset/problem/104882/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 26s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được đưa cho một hàng bánh nướng được đánh số từ trái sang phải, trong đó mỗi chiếc bánh là bắp cải hoặc nấm. Hai em liên tục lấy những chiếc bánh từ hàng này theo quy tắc vị trí: một em nhắm vào chiếc bánh thứ 3 còn lại, đứa còn lại nhắm vào chiếc bánh thứ 7 còn lại. Họ không hành động độc lập trên một hệ thống chỉ số cố định. Thay vào đó, họ luân phiên nhau chọn từ dãy bánh còn lại hiện tại, bắt đầu từ Masha. 

Một chi tiết quan trọng là “chiếc bánh thứ 3” hoặc “chiếc bánh thứ 7” được hiểu tương ứng với trạng thái hiện tại của những chiếc bánh còn lại sau khi loại bỏ trước đó. Mỗi lần di chuyển sẽ loại bỏ một chiếc bánh, làm thay đổi chỉ mục của tất cả các lựa chọn tiếp theo. Quá trình này tiếp tục cho đến khi tất cả các chiếc bánh được lấy hết, và chúng ta phải đếm xem mỗi đứa trẻ sẽ ăn bao nhiêu chiếc bánh nấm. 

Kích thước đầu vào lên tới 100000 bánh nướng, vì vậy mọi cách tiếp cận mô phỏng việc lập chỉ mục lại bằng cách xóa ngây thơ đều phải được xem xét cẩn thận. Một mô phỏng trực tiếp quét mảng nhiều lần và loại bỏ các phần tử sẽ chuyển sang trạng thái bậc hai, quá chậm trong giới hạn 1 giây. Tinh vi hơn nữa, việc sử dụng danh sách động và liên tục chọn phần tử hoạt động thứ k bằng cách quét tuyến tính sẽ dẫn đến hành vi khoảng O(n^2) trong trường hợp xấu nhất. 

Một cạm bẫy đơn giản xuất hiện khi người ta cho rằng Masha luôn lấy các chỉ số 3, 6, 9 và Petya luôn lấy 7, 14, 21 trong mảng ban đầu. Cách giải thích đó sai vì sự loại bỏ làm dịch chuyển vị trí. Ví dụ: với một chuỗi nhỏ như 1 1 0 1 0 1 1, sự chồng chéo ở các vị trí giống chỉ số 21 là vô nghĩa; các phần tử được chọn thực tế phụ thuộc vào cấu trúc phát triển chứ không phải cấp số cộng cố định. 

Một vấn đề tế nhị khác là sự ràng buộc do sự chồng chéo về các mục tiêu. Khi cả hai đứa trẻ đều chọn cùng một chiếc bánh trong cùng một bước khái niệm, chỉ một trong số chúng thực sự lấy nó và đứa còn lại phải tiếp tục quét sang mục tiêu hợp lệ tiếp theo trong cấu trúc còn lại. Điều này ngăn cản việc đếm hai lần và đảm bảo mỗi chiếc bánh được lấy chính xác một lần. 

## Phương pháp tiếp cận 

Một mô phỏng đơn giản duy trì một danh sách các bánh còn lại và thực hiện liên tục hai thao tác: tiến một con trỏ đếm các bánh còn sống cho đến khi đạt đến vị trí thứ k có sẵn, sau đó loại bỏ nó. Điều này đúng vì nó phản ánh trực tiếp định nghĩa quy trình. Tuy nhiên, mỗi lần xóa yêu cầu quét qua tối đa O(n) phần tử và điều này xảy ra O(n) lần, gây ra độ phức tạp O(n^2). 

Nhận xét quan trọng là điều duy nhất quan trọng là thứ tự các bánh được lấy ra chứ không phải chỉ số dịch chuyển của chúng. Ở mỗi bước, chúng tôi đang thực hiện một cách hiệu quả thao tác “chọn phần tử còn sống thứ k” lặp đi lặp lại trên một chuỗi thu nhỏ. Đây là một bài toán thống kê thứ tự cổ điển trên một mảng động. Thay vì mô phỏng việc xóa, chúng tôi duy trì cấu trúc có thể nhanh chóng tìm và xóa phần tử hoạt động thứ k. 

Cây Fenwick (cây được lập chỉ mục nhị phân) trên mảng 0/1 cho biết liệu một chiếc bánh có còn tồn tại hay không cho phép chúng ta hỗ trợ hai thao tác một cách hiệu quả: đếm xem có bao nhiêu chiếc bánh vẫn còn sống đến một vị trí và tìm vị trí của chiếc bánh còn sống thứ k thông qua việc nâng nhị phân. Mỗi nước đi sẽ trở thành O(log n), dẫn đến giải pháp tổng thể là O(n log n). 

Chúng tôi mô phỏng quy trình từng bước, xen kẽ giữa Masha và Petya, nhưng mỗi lựa chọn được giải quyết bằng cách sử dụng thống kê thứ tự trên tập sống. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(n) | Quá chậm | 
| Mô phỏng cây Fenwick | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta duy trì cây Fenwick trên một mảng có kích thước n, trong đó mỗi vị trí ban đầu có giá trị 1 cho biết chiếc bánh vẫn còn tồn tại.

Chúng tôi cũng duy trì hai bộ đếm: một bộ đếm cho cấp độ mục tiêu tiếp theo của Masha và một bộ đếm cho cấp độ mục tiêu tiếp theo của Petya. Chúng đại diện cho "lấy mỗi chiếc bánh còn lại thứ 3" và "lấy mỗi chiếc bánh thứ 7 còn lại", nhưng được giải thích theo trình tự thu hẹp. 

### bước 

1. Khởi tạo cây Fenwick với tất cả các vị trí được đặt thành 1. Điều này thể hiện tất cả các bánh nướng đều có sẵn. 
2. Đặt con trỏ`turn = 0`để biểu thị Masha bắt đầu trước. 
3. Duy trì hai quầy:`kM = 3`Và`kP = 7`, đại diện cho thống kê thứ tự mục tiêu tiếp theo trong chuỗi còn lại tương ứng cho Masha và Petya. 
4. Lặp lại cho đến khi lấy hết bánh: 

1. Nếu`turn == 0`, chúng tôi muốn chiếc bánh thứ km còn lại của Masha. Chúng tôi truy vấn cây Fenwick để tìm chỉ mục nhỏ nhất trong đó tổng tiền tố bằng kM. Điều này mang lại vị trí thực tế trong mảng ban đầu trong số những chiếc bánh còn lại. Loại bỏ nó bằng cách cập nhật cây ở vị trí đó thành 0. Nếu đó là chiếc bánh nấm, hãy tăng điểm của Masha. Sau đó tăng kM lên 3 vì bây giờ cô ấy nhắm mục tiêu bội số tiếp theo trong chuỗi còn lại. 
2. Nếu`turn == 1`, chúng tôi cũng làm tương tự với Petya bằng cách sử dụng kP và tăng thêm 7 sau mỗi lần xóa thành công. 
3. Chuyển đổi`turn`tới người chơi khác. 

Mỗi lựa chọn đều hoạt động vì cây Fenwick duy trì ánh xạ giữa “bánh sống thứ k” và chỉ mục thực tế trong mảng ban đầu, mặc dù việc xóa đã thay đổi vị trí. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, các bánh còn lại tạo thành một dãy con có thứ tự của mảng ban đầu. Cây Fenwick mã hóa chuỗi con này dưới dạng cấu trúc tổng tiền tố động. Truy vấn còn hoạt động thứ k luôn trả về đúng vị trí hiện tại trong chuỗi con đó, không phụ thuộc vào các lần xóa trước đó. Vì quy tắc của mỗi người chơi chỉ phụ thuộc vào thứ hạng tương đối trong chuỗi hiện tại nên việc duy trì số liệu thống kê thứ tự chính xác là đủ để
