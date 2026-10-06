---
title: "CF 104925D - Hệ thống tập tin"
description: "Chúng ta được cung cấp một tập hợp các tệp, mỗi tệp có hai thứ tự tổng độc lập được xác định trên đó. Một thứ tự theo tên tệp, thứ tự còn lại theo ngày tạo. Thứ tự tên tệp được cố định và được biểu thị bằng các chỉ số từ 1 đến n."
date: "2026-06-28T07:52:42+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104925
codeforces_index: "D"
codeforces_contest_name: "Osijek Competitive Programming Camp, Fall 2023. Day 6: Estonian Contest (The 2nd Universal Cup. Stage 19: Estonia)"
rating: 0
weight: 104925
solve_time_s: 30
verified: false
draft: false
---

[CF 104925D - Hệ thống tập tin](https://codeforces.com/problemset/problem/104925/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 30s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp các tệp, mỗi tệp có hai thứ tự tổng độc lập được xác định trên đó. Một thứ tự theo tên tệp, thứ tự còn lại theo ngày tạo. Thứ tự tên tệp được cố định và được biểu thị bằng các chỉ số từ 1 đến n. Thứ tự ngày tạo được đưa ra dưới dạng hoán vị của các chỉ số này. 

Một thao tác duy nhất cho phép chúng tôi chọn một trong hai thứ tự, sắp xếp theo tên hoặc sắp xếp theo ngày, sau đó lấy bất kỳ phân đoạn liền kề nào của thứ tự kết quả và “tải lên” tất cả các tệp trong phân đoạn đó. Các tệp không được tải lên vẫn còn trong hệ thống và các thao tác sau này được thực hiện độc lập, bắt đầu lại từ việc sắp xếp đầy đủ theo một trong hai thứ tự. 

Hạn chế là mỗi tệp bắt buộc phải được tải lên chính xác một lần và chúng tôi muốn giảm thiểu số lần tải lên phân đoạn đó. 

Cấu trúc chính là mỗi thao tác không tùy ý: nó luôn là một khoảng liền kề ở một trong hai hoán vị cố định. Điều này biến vấn đề thành một tập hợp các phần tử được đánh dấu bằng cách sử dụng các khoảng chỉ có giá trị theo hai thứ tự tuyến tính khác nhau. 

Các ràng buộc có tổng số nhỏ, với tổng n trên các trường hợp thử nghiệm nhiều nhất là 1000, do đó, giải pháp O(n^2) hoặc O(n log n) cho mỗi thử nghiệm là đủ. Tuy nhiên, lý luận thô bạo trên tất cả các tập hợp con hoặc tất cả các phân đoạn sẽ theo cấp số nhân và ngay lập tức không khả thi. 

Trường hợp cạnh tinh tế xuất hiện khi các tệp được chọn được xen kẽ theo cả hai thứ tự. Trong những trường hợp như vậy, một chiến lược tham lam ngây thơ như “luôn lấy khối liền kề lớn nhất có thể theo một thứ tự” có thể thất bại, bởi vì một khối trông tối ưu cục bộ có thể phá hủy khả năng nhóm các tệp sau này theo thứ tự khác. 

Một ví dụ tối thiểu về hiện tượng này là khi các chỉ số bắt buộc thay thế nhau trong cả hai hoán vị. Sau đó, không có hai tệp bắt buộc nào liền kề nhau theo bất kỳ thứ tự nào, buộc mỗi tệp phải được lấy riêng lẻ. Bất kỳ nỗ lực tham lam nào nhằm hợp nhất các mục bắt buộc liền kề trong một thứ tự sẽ ngay lập tức phá vỡ tính tối ưu vì nó ngăn cản việc sử dụng thứ tự khác. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng mô hình hóa từng hoạt động hợp lệ dưới dạng lựa chọn thứ tự cộng với khoảng thời gian đã chọn, sau đó tìm kiếm bằng mọi cách để phân vùng tập hợp các tệp cần thiết thành các khoảng thời gian như vậy. Vì có các khoảng O(n^2) cho mỗi đơn hàng và các cách kết hợp chúng theo cấp số nhân, nên điều này nhanh chóng trở nên khó thực hiện ngay cả đối với n khoảng 30. 

Quan sát quan trọng là chúng ta không bao giờ cần suy luận về các tập hợp con tùy ý của các tệp, mà chỉ về cách các tệp bắt buộc xuất hiện liên tiếp trong hai hoán vị. Mỗi thao tác về cơ bản là hợp nhất một khối liền kề trong một hoán vị, do đó, vấn đề trở thành việc chia các chỉ mục cần thiết thành các phân đoạn “nhất quán” theo ít nhất một trong hai thứ tự. 

Điều này dẫn đến quan điểm lập trình động: chúng tôi sắp xếp các phần tử cần thiết theo một thứ tự và cố gắng phân vùng chúng, nhưng việc chuyển đổi phụ thuộc vào việc khối tiếp theo có tiếp giáp nhau trong một trong hai hoán vị hay không. Cấu trúc được đơn giản hóa hơn nữa vì việc kiểm tra tính liên tục trong cả hai hoán vị có thể được giảm xuống thành các so sánh khoảng thời gian trên các vị trí. 

Một công thức hiệu quả hơn là quan sát rằng mỗi thao tác tương ứng với việc chọn một phân đoạn liền kề theo thứ tự tên hoặc theo thứ tự ngày. Vì vậy, chúng tôi tính toán trước các vị trí trong cả hai hoán vị và coi mỗi tệp được yêu cầu là một điểm trong mặt phẳng 2D. Khi đó, mỗi thao tác hợp lệ là một đoạn đơn điệu trên ít nhất một trục. Câu trả lời trở thành số lượng phân đoạn đơn điệu tối thiểu cần thiết để bao gồm tất cả các điểm, tôn trọng các ràng buộc về thứ tự do cả hai hoán vị gây ra.

Một cách tiêu chuẩn để giải quyết vấn đề này là lập trình động trên danh sách các phần tử bắt buộc được sắp xếp theo một thứ tự, duy trì cho mỗi tiền tố cách tốt nhất để kết thúc phân đoạn cuối cùng bằng cách sử dụng thứ tự tên hoặc thứ tự ngày. Việc chuyển đổi yêu cầu kiểm tra xem việc mở rộng phân đoạn hiện tại có bảo toàn tính liên tục trong hoán vị đã chọn hay không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên các phân đoạn | Hàm mũ | O(n) | Quá chậm | 
| DP theo thứ tự các phần tử bắt buộc với các lần kiểm tra theo khoảng thời gian | O(n^2) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi ánh xạ từng id tệp tới vị trí của nó theo thứ tự tên và thứ tự ngày. Đặt posA[x] là chỉ mục của nó theo thứ tự tên và posB[x] chỉ mục của nó theo thứ tự ngày. 

Chúng tôi trích xuất danh sách các tệp cần thiết và xem xét cấu trúc của chúng theo cả hai hoán vị. 

Sau đó, chúng tôi tính toán câu trả lời dưới dạng số lượng phân đoạn tối thiểu, trong đó mỗi phân đoạn hợp lệ nếu nó tạo thành một khoảng liền kề trong posA hoặc posB. 

1. Xây dựng các mảng posA và posB ánh xạ từng tệp tới vị trí của nó trong cả hai hoán vị. Điều này cho phép chúng tôi kiểm tra sự liên tục trong thời gian O(1). 
2. Gọi S là tập hợp các tệp cần thiết và sắp xếp S theo posA. Điều này đưa ra một thứ tự tự nhiên để thực hiện phân đoạn, bởi vì bất kỳ phân đoạn hợp lệ nào trong A đều phải xuất hiện dưới dạng một khối liên tiếp theo thứ tự này. 
3. Xác định mảng DP dp[i] là n tối thiểu
