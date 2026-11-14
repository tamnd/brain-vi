---
title: "CF 180F - Đá phân tích toán học!"
description: "Mỗi học sinh ban đầu sở hữu một cuốn sổ duy nhất. Có một hoán vị ẩn cố định p trên học sinh, nghĩa là mỗi học sinh có chính xác một “bạn thân” và mỗi học sinh là bạn thân nhất của đúng một học sinh khác."
date: "2026-06-03T00:50:04+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 180
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 116 (Div. 2, ACM-ICPC Rules)"
rating: 1200
weight: 180
solve_time_s: 35
verified: false
draft: false
---

[CF 180F - Đá phân tích toán học!](https://codeforces.com/problemset/problem/180/F) 

**Đánh giá:** 1200 
**Tags:** thuật toán xây dựng, triển khai, toán học 
**Thời gian giải:** 35s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Mỗi học sinh ban đầu sở hữu một cuốn sổ duy nhất. Có một hoán vị ẩn cố định`p`hơn học sinh, nghĩa là mỗi học sinh có chính xác một “bạn thân” và mỗi học sinh là bạn thân nhất của đúng một học sinh khác. Bởi vì nó là một hoán vị, việc đi theo các con trỏ bạn thân lặp đi lặp lại chỉ đi dọc theo các chu kỳ có hướng rời rạc, không có phân nhánh và không có nút nào bị thiếu. 

Quá trình này mô tả cách các sổ ghi chép di chuyển theo hoán vị này theo thời gian. Vào ngày thứ nhất, mỗi học sinh đọc vở của mình. Từ ngày thứ 2 trở đi, tất cả các vở đều được chuyển đồng thời theo hoán vị: một cuốn vở thuộc sở hữu của học sinh`i`chuyển sang sinh viên`p[i]`vào ngày hôm sau. Điều này có nghĩa là vào ngày`t`, cuốn sổ bắt đầu lúc`i`nằm ở`p^(t-1)[i]`. 

Chúng tôi không được trao`p`. Thay vào đó, chúng ta được cung cấp hai ảnh chụp nhanh đầy đủ: mỗi sổ ghi chép nằm ở ngày thứ 3 và ngày thứ 4. Đây là những hoán vị`a`Và`b`. giá trị`a[i]`là học sinh đang cầm cuốn sổ`i`vào ngày thứ 3, và`b[i]`là học sinh đang cầm cuốn sổ`i`vào ngày thứ 4. Vì quá trình chuyển đổi ngày bị chi phối bởi cùng một hoán vị ẩn nên chúng ta phải xây dựng lại`p`. 

Hạn chế chính là`n ≤ 10^5`, loại trừ bất kỳ giải pháp nào cố gắng mô phỏng quy trình cho từng ánh xạ ứng viên có thể có hoặc cho từng học sinh một cách độc lập trong thời gian bậc hai. Chúng ta cần một cái gì đó tuyến tính hoặc gần tuyến tính, vì khoảng`10^8`các thao tác đơn giản đã là ranh giới trong Python. 

Một đặc tính cấu trúc tinh tế nhưng quan trọng là`a`Và`b`đều là hoán vị. Điều đó đảm bảo rằng mỗi học sinh xuất hiện chính xác một lần trong mỗi mảng, vì vậy mỗi ảnh chụp nhanh là sự kết hợp giữa sổ ghi chép và học sinh. 

Một trường hợp thất bại điển hình cho lối suy nghĩ ngây thơ là giả định rằng chúng ta có thể theo dõi từng cuốn sổ một cách độc lập bằng cách đoán xem nó sẽ đi đâu từ ngày thứ 3 đến ngày thứ 4 mà không cần tận dụng cấu trúc hoán vị. Ví dụ, nếu chúng ta cố gắng suy luận`p[i]`bằng cách “tìm xem học sinh i đang ở đâu trong khoảng thời gian từ ngày thứ 3 đến ngày thứ 4”, chúng tôi sẽ trộn lẫn vai trò của sổ ghi chép và học sinh, bởi vì mảng mô tả vị trí của sổ ghi chép chứ không phải sự chuyển đổi của học sinh. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ trực tiếp là thử tất cả các hoán vị có thể`p`và mô phỏng chuyển động của tất cả các sổ ghi chép từ ngày 1 đến ngày 4, sau đó kiểm tra xem cấu hình kết quả của ngày 3 và ngày 4 có khớp không`a`Và`b`. Điều này đúng về mặt lý thuyết, nhưng có`n!`hoán vị ứng cử viên, và thậm chí kiểm tra một hoán vị đòi hỏi ít nhất`O(n)`mô phỏng. Điều này trở nên lớn về mặt thiên văn ngay cả đối với`n = 10`, vì vậy nó ngay lập tức không thể thực hiện được. 

Quan sát chính xuất phát từ việc viết lại quy trình về mặt thành phần chức năng. Cho phép`p`hành động trên các vị trí máy tính xách tay. Sau đó ngày thứ 3 tương ứng với việc nộp đơn`p^2`với cấu hình nhận dạng ban đầu và ngày thứ 4 tương ứng với việc áp dụng`p^3`. Chúng tôi được trao cả hai`p^2`Và`p^3`, nhưng không`p`. 

Điều này gợi ý một mối quan hệ trực tiếp: sáng tác với`p`biến ngày thứ 3 thành ngày thứ 4. Nói cách khác, nếu chúng ta diễn giải`a`như một hoán vị mô tả vị trí của mỗi cuốn sổ vào ngày thứ 3 và`b`tương tự cho ngày thứ 4, sau đó áp dụng`p`ánh xạ cấu hình ngày 3 sang cấu hình ngày 4. 

Cụ thể hơn, nếu sổ ghi chép`i`đang ở sinh viên`a[i]`vào ngày thứ 3, rồi đến ngày thứ 4 cuốn sổ đó phải có ở học sinh`b[i]`. Kể từ khi một ứng dụng duy nhất của`p`di chuyển người giữ về phía trước, chúng tôi nhận được ràng buộc:`p[a[i]] = b[i]`. 

Điều này đưa ra quy tắc xây dựng lại trực tiếp: cho mỗi sổ ghi chép`i`, chúng tôi biết chính xác cách học sinh hiện đang giữ nó (vào ngày thứ 3) chuyển sang người giữ nó tiếp theo (vào ngày thứ 4). Đó chính xác là định nghĩa của`p`. 

Vì vậy, toàn bộ vấn đề giảm xuống việc khớp các chuyển tiếp giữa hai hoán vị. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n! · n) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi khai thác thực tế rằng mọi chuyển đổi sổ ghi chép từ ngày thứ 3 sang ngày thứ 4 đều tương ứng với chính xác một ứng dụng của hoán vị ẩn`p`. 

1. Xây dựng ánh xạ nghịch đảo của`a`, để chúng tôi có thể nhanh chóng xác định sổ ghi chép nào hiện có ở mỗi học sinh vào ngày thứ 3. 

Điều này là cần thiết bởi vì`a[i]`là “notebook i is at school a[i]”, nhưng chúng ta muốn suy luận từ sinh viên này sang notebook khác. 
2. Đối với mỗi học sinh`x`, xác định sổ ghi chép nào hiện đang ở`x`vào ngày thứ 3 bằng cách sử dụng nghịch đảo của`a`. 
3. Cuốn sổ đó chuyển đến học sinh`b[notebook]`vào ngày thứ 4. 

Điều này mang lại sự chuyển đổi trực tiếp từ`x`ĐẾN`b[notebook]`. 
4. Chỉ định`p[x] = b[notebook]`cho mỗi học sinh`x`. 

Điều này xây dựng hoán vị từng cạnh. 
5. Xuất mảng kết quả`p`. 

Điểm quan trọng là mỗi học sinh đóng góp chính xác một ràng buộc và những ràng buộc này cùng nhau tạo thành một hoán vị đầy đủ mà không có sự mơ hồ. 

### Tại sao nó hoạt động 

Vào ngày thứ 3, mỗi học sinh cầm đúng một cuốn vở và`a`mã hóa song ánh này. Sử dụng nghịch đảo của nó, chúng tôi khôi phục sổ ghi chép duy nhất của mỗi học sinh. Vào ngày thứ 4, cuốn sổ đó phải xuất hiện chính xác một lần nữa do cấu trúc hoán vị và vị trí của nó được cho bởi`b`. Vì mỗi cuốn sổ đều di chuyển theo cùng một hoán vị ẩn`p`, quá trình chuyển đổi từ người nắm giữ ngày thứ 3 sang người nắm giữ ngày thứ 4 phải chính xác là một ứng dụng của`p`. Bởi vì mỗi học sinh xuất hiện đúng một lần trong ngày thứ 3 nên chúng ta phục hồi tất cả các cạnh của`p`không có xung đột. 

## Giải pháp Python
