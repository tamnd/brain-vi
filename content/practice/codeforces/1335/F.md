---
title: "CF 1335F - Robot trên lưới"
description: "Mỗi ô trong lưới hoạt động giống như một trạng thái xác định trong biểu đồ có hướng. Từ mỗi ô, có chính xác một cạnh đi ra chỉ vào một trong bốn ô lân cận của nó, như được chỉ định bởi mũi tên trong ô đó."
date: "2026-06-16T08:48:14+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dfs-and-similar", "dsu", "graphs", "greedy", "matrices"]
categories: ["algorithms"]
codeforces_contest: 1335
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 634 (Div. 3)"
rating: 2200
weight: 1335
solve_time_s: 71
verified: false
draft: false
---

[CF 1335F - Robot trên lưới](https://codeforces.com/problemset/problem/1335/F) 

**Đánh giá:** 2200 
**Thẻ:** cấu trúc dữ liệu, dfs và tương tự, dsu, đồ thị, tham lam, ma trận 
**Thời gian giải:** 1 phút 11s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Mỗi ô trong lưới hoạt động giống như một trạng thái xác định trong biểu đồ có hướng. Từ mỗi ô, có chính xác một cạnh đi ra chỉ vào một trong bốn ô lân cận của nó, như được chỉ định bởi mũi tên trong ô đó. Do sự đảm bảo về ranh giới, mọi mũi tên đều hợp lệ và nằm trong lưới. 

Nếu chúng ta khởi động một robot trong một ô nào đó, nó sẽ tuân theo hệ thống con trỏ xác định này mãi mãi. Chuyển động của robot mang tính đồng bộ: tất cả các robot đều di chuyển từng bước cùng một lúc. Hạn chế là không có hai robot nào được phép chiếm giữ cùng một ô ở cùng một bước thời gian, kể cả thời gian 0. Vì chuyển động mang tính quyết định nên hai robot xuất phát từ các vị trí khác nhau cuối cùng có thể va chạm nếu quỹ đạo của chúng hợp nhất. 

Nhiệm vụ là chọn một tập hợp các ô bắt đầu có kích thước tối đa sao cho tất cả các quỹ đạo thu được đều rời rạc theo từng cặp ở mỗi bước thời gian. Trong số tất cả các lựa chọn tối ưu, chúng tôi cũng muốn tối đa hóa số lượng ô bắt đầu được chọn có màu đen. 

Các ràng buộc ngụ ý rằng chúng ta không thể chấp nhận bất cứ điều gì tệ hơn tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm. Vì tổng số ô trong tất cả các thử nghiệm tối đa là 10^6 nên cần phải có giải pháp O(nm) hoặc O(nm log nm), trong khi mọi thứ bậc hai trong kích thước lưới là không thể. 

Một trường hợp thất bại tinh vi xuất hiện khi nhiều ô bắt đầu cuối cùng đi vào cùng một chu trình hoặc đường dẫn. Ví dụ: nếu cả hai chuỗi đều dẫn đến một điểm đầu vào của chu trình chung, thì việc chọn cả hai ô ban đầu đều không hợp lệ ngay cả khi ban đầu đường dẫn của chúng không khớp nhau. Một kẻ tham lam ngây thơ chỉ kiểm tra những người hàng xóm ngay lập tức hoặc xung đột cục bộ sẽ thất bại ở đây vì xung đột xảy ra sau một vài bước. 

Một tình huống phức tạp khác là khi tồn tại một chu kỳ. Tất cả các nút trong một chu trình được định hướng đều có khả năng chứa robot, nhưng chúng ta phải cẩn thận: nếu hai điểm bắt đầu nằm trong cùng một chu trình được định hướng, chúng sẽ va chạm ngay lập tức khi bước vào chu trình trừ khi chúng được đặt cách đều nhau xung quanh nó. Trên thực tế, trong một chu kỳ, mọi nút đều có thể truy cập được từ mọi nút khác sau một số thay đổi về thời gian, do đó, nhiều rô-bốt trên cùng một chu kỳ đều bị cấm trừ khi cấu trúc chu trình cho phép các pha độc lập, điều này không có trong cài đặt đồng bộ này. 

## Phương pháp tiếp cận 

Lưới xác định một đồ thị hàm số: mỗi ô có chính xác một cạnh đi ra. Những đồ thị như vậy phân hủy thành các chu kỳ có hướng với các cây chảy vào chúng. Mỗi nút cuối cùng đạt đến đúng một chu kỳ. 

Cách tiếp cận bạo lực sẽ mô phỏng quy trình cho từng tập hợp con được chọn của các ô bắt đầu. Đối với một tập hợp con nhất định, chúng tôi sẽ mô phỏng chuyển động của robot theo các bước O(nm) hoặc cho đến khi phát hiện được tính tuần hoàn và kiểm tra xem có bất kỳ va chạm nào xảy ra ở bất kỳ bước thời gian nào hay không. Có 2^(nm) tập hợp con, điều này rõ ràng là không khả thi. Ngay cả việc kiểm tra một cấu hình cũng tốn kém vì quỹ đạo có thể mất nhiều thời gian trước khi bước vào chu kỳ. 

Quan sát quan trọng là các va chạm chỉ phụ thuộc vào khả năng tiếp cận cùng một chu trình và pha bên trong chu trình đó. Khi hai đường đi vào cùng một chu kỳ, độ lệch tương đối của chúng là cố định và không thể tránh xung đột nếu cả hai được chọn. Tương tự, dọc theo các cây dẫn vào chu kỳ, các đường đi hợp nhất giống như trong cấu trúc rừng DSU: nếu cuối cùng hai nút gặp nhau ở cùng một chuỗi kế thừa thì chúng không tương thích.

Điều này làm giảm vấn đề xác định các thành phần được kết nối của đồ thị hàm trong đó mỗi thành phần bao gồm một chu trình duy nhất với các cây sắp tới. Bên trong mỗi thành phần như vậy, chúng ta phải quyết định những nút nào có thể được chọn làm điểm bắt đầu mà không vi phạm các ràng buộc “chiếm dụng cùng thời gian”. Cấu trúc đúng là mỗi chu kỳ đóng góp một ràng buộc trong đó tối đa một robot có thể được đặt trong mỗi chu kỳ, nhưng các nút trong cây có thể được chọn độc lập miễn là chúng không cạnh tranh ở cùng một trạng thái xuôi dòng ở cùng một lớp thời gian. Điều này trở nên tương đương với việc xử lý từng thành phần và tham lam chọn các nút “an toàn”, nghĩa là không có nút nào được chọn khác ánh xạ tới cùng một nút kế thừa ở cùng một khoảng cách. 

Giải pháp tiêu chuẩn sử dụng truyền bá đồ thị ngược với đánh dấu DFS và xử lý từng thành phần bằng cách thu gọn các đường dẫn của cây vào chu trình và xử lý các lựa chọn trên cây theo cách tránh xung đột. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(2^(nm) · nm · T) | O(nm) | Quá chậm | 
| Phân tách đồ thị hàm số + DFS/DP | O(nm) | O(nm) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Coi mỗi ô là một nút trong đồ thị có hướng với đúng một cạnh hướng ra. Xây dựng hoặc tính toán các chuyển tiếp ngầm từ hướng lưới. Điều này đưa ra một biểu đồ chức năng trong đó mọi nút cuối cùng đều đạt đến một chu kỳ. 
2. Chạy DFS (hoặc truyền tải dựa trên ngăn xếp lặp lại) để phân loại mỗi nút thành một trong ba trạng thái: chưa truy cập, đang truy cập hoặc đã xử lý. Khi tìm thấy cạnh sau, chúng tôi xác định một chu trình có hướng. 
3. Đối với mỗi chu kỳ, trích xuất tất cả các nút thuộc về nó. Các nút này tạo thành một vòng khép kín trong đó mỗi nút ánh xạ tới chính xác một nút kế tiếp trong chu trình. 
4. Đánh dấu tất cả các nút chu trình là “đại diện chu trình gốc” của thành phần của chúng. Mỗi chu kỳ xác định một lưu vực: tất cả các nút cuối cùng chảy vào đó đều thuộc cùng một thành phần. 
5. Đối với mỗi thành phần, duyệt cây đến bằng cách sử dụng các cạnh ngược. Đối với mỗi nút, hãy tính khoảng cách của nó tới chu trình và điểm vào của nó trên chu trình. 
6. Hạn chế quan trọng là nếu hai nút ánh xạ vào cùng một nút chu trình ở cùng một bước thời gian tương đối thì chúng sẽ va chạm nhau. Do đó, đối với mỗi nút chu trình, chỉ có thể chọn một “lớp pha” của các đường dẫn đến. 
7. Xử lý từng chu kỳ một cách độc lập. Đối với chu kỳ có độ dài k, hãy coi mỗi nút trong chu kỳ là một điểm neo tiềm năng. Đối với mỗi cây đến được gắn vào nút chu trình đó, chúng ta có thể chọn nhiều nhất một đại diện cho mỗi lớp độ sâu được căn chỉnh theo
