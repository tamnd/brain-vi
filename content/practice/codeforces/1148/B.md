---
title: "CF 1148B - Sinh Ra Như Thế"
description: "Chúng tôi được cung cấp hai mốc thời gian sắp xếp của các chuyến bay tạo thành một hành trình bắt buộc phải có hai chặng. Đầu tiên, một hành khách chọn chuyến bay từ A đến B, dành thời gian di chuyển cố định, sau đó ngay lập tức kết nối với chuyến bay từ B đến C, lại với thời gian di chuyển cố định."
date: "2026-06-12T03:08:59+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "brute-force", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1148
codeforces_index: "B"
codeforces_contest_name: "Codeforces Global Round 3"
rating: 1600
weight: 1148
solve_time_s: 32
verified: false
draft: false
---

[CF 1148B - Sinh ra theo cách này](https://codeforces.com/problemset/problem/1148/B) 

**Đánh giá:** 1600 
**Tags:** tìm kiếm nhị phân, lực lượng vũ phu, hai con trỏ 
**Thời gian giải:** 32s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp hai mốc thời gian sắp xếp của các chuyến bay tạo thành một hành trình bắt buộc phải có hai chặng. Đầu tiên, một hành khách chọn chuyến bay từ A đến B, dành thời gian di chuyển cố định, sau đó ngay lập tức kết nối với chuyến bay từ B đến C, lại với thời gian di chuyển cố định. Việc nối chuyến chỉ có hiệu lực nếu chuyến bay thứ hai khởi hành không sớm hơn thời gian đến của chặng đầu tiên. 

Chúng tôi được phép loại bỏ tổng cộng tối đa k chuyến bay khỏi một trong hai chặng bay. Sau khi loại bỏ các chuyến bay, chúng tôi muốn làm cho hành trình tốt nhất có thể của hành khách trở nên tồi tệ nhất có thể, nghĩa là chúng tôi muốn tối đa hóa thời gian đến thành phố C sớm nhất có thể trong số tất cả các tuyến hợp lệ còn lại. Nếu chúng ta có thể hủy tất cả các tuyến hợp lệ bằng cách sử dụng tối đa k lần xóa thì câu trả lời là -1. 

Mục tiêu chính không chỉ là các cặp chuyến bay mà còn là cách xóa các cặp chuyến bay tốt nhất hiện có. Việc loại bỏ các chuyến bay A-to-B sớm buộc hành khách phải khởi hành muộn hơn. Việc loại bỏ các chuyến bay B-to-C sớm sẽ buộc phải kết nối sau này hoặc có khả năng phá vỡ hoàn toàn tính khả thi. 

Các ràng buộc n, m lên tới 200000 ngay lập tức loại trừ mọi chiến lược ghép đôi bậc hai. Bất kỳ cách tiếp cận nào cố gắng kiểm tra tất cả các cặp (i, j) hoặc mô phỏng việc xóa một cách rõ ràng sẽ không tồn tại. Ngay cả lý do O(nm) về khả năng tương thích cũng quá lớn. Chúng ta buộc phải hướng tới giải pháp O((n + m) log (n + m)) hoặc O(n + m), thường liên quan đến tìm kiếm nhị phân hoặc suy luận tiền tố hai con trỏ. 

Một số trường hợp thất bại nảy sinh do lối suy nghĩ ngây thơ. 

Một trường hợp tế nhị là giả sử chúng ta luôn hủy các chuyến bay sớm nhất trong cả hai danh sách. Điều đó là sai vì đôi khi việc loại bỏ chuyến bay B muộn hơn một chút lại có giá trị hơn việc loại bỏ chuyến bay A trước đó, vì nó chặn nhiều kết nối có thể có. 

Một trường hợp khác là giả định sự độc lập giữa hai phân đoạn. Ví dụ: việc chọn chuyến bay A-to-B mới nhất có thể và chuyến bay B-to-C mới nhất có thể không thành công vì tính hợp lệ phụ thuộc vào ràng buộc kết nối. 

Cạm bẫy cuối cùng là quên rằng sau khi hủy, việc ghép đôi tối ưu vẫn mang tính tham lam: một khi A đã được sửa, B tốt nhất là cặp hợp lệ sớm nhất, không nhất thiết là cặp cuối cùng còn lại. 

## Phương pháp tiếp cận 

Quan điểm bạo lực là thử mọi cách để loại bỏ tối đa k chuyến bay, sau đó tính toán lại hành trình tốt nhất có thể. Ngay cả khi chúng tôi sửa một loạt các thao tác xóa, việc tính toán thời gian đến tốt nhất là tuyến tính bằng cách quét các chuyến bay A và đối với mỗi chuyến bay, tìm chuyến bay B hợp lệ sớm nhất thông qua tìm kiếm nhị phân. Nhưng số lượng tập hợp con bị xóa là theo cấp số nhân tính theo k và bản thân k có thể lớn tới 400000 trong trường hợp xấu nhất. Điều này ngay lập tức bùng nổ vượt quá khả năng thực hiện. 

Quan sát quan trọng là chúng ta không thực sự
