---
title: "CF 105160D - \u65b9\u5757\u6e38\u620f"
description: "Chúng ta có một lưới $n nhân m$ tượng trưng cho một bảng trò chơi được xếp theo từng ô. Mỗi ô trống hoặc được tô màu bằng một trong ba màu có nhãn 1, 2 và 3."
date: "2026-06-27T11:00:39+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105160
codeforces_index: "D"
codeforces_contest_name: "2024 University of Shanghai for Science and Technology(USST) Freshman Challenge Contest"
rating: 0
weight: 105160
solve_time_s: 30
verified: false
draft: false
---

[CF 105160D - \u65b9\u5757\u6e38\u620f](https://codeforces.com/problemset/problem/105160/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 30s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một$n \times m$lưới đại diện cho một bảng trò chơi lát gạch. Mỗi ô trống hoặc được tô màu bằng một trong ba màu có nhãn 1, 2 và 3. Các ô màu này tạo thành một số thành phần được kết nối và mỗi thành phần được kết nối tương ứng với chính xác một phần trò chơi ẩn. 

Có ba loại mảnh có thể. Mỗi mảnh chiếm một số ô lưới và có thể xuất hiện ở bất kỳ dạng xoay hoặc phản chiếu nào, nhưng cấu trúc hình dạng của nó được cố định ở mức đối xứng. Điều đảm bảo chính là toàn bộ lưới được hình thành bằng cách đặt các bản sao không chồng chéo của ba loại mảnh này, do đó, mọi ô khác 0 đều thuộc về chính xác một phiên bản mảnh hợp lệ. 

Nhiệm vụ là xác định có bao nhiêu phần của mỗi loại trong số ba loại xuất hiện trong lưới. 

Những hạn chế$n, m \le 500$có nghĩa là lên tới 250.000 tế bào. Bất kỳ giải pháp nào về cơ bản phải tuyến tính theo kích thước lưới, vì thậm chí$O((nm)^2)$sẽ là quá chậm. Điều này cho thấy rõ ràng sự phân rã lưới điện dựa trên sự tích tụ hoặc dựa trên liên kết thành các thành phần được kết nối, sau đó là phân loại từng thành phần theo thời gian không đổi hoặc gần như không đổi. 

Một vấn đề tế nhị là các phần có thể bị xoay hoặc phản chiếu. Điều này loại trừ việc khớp hình dạng trực tiếp ngây thơ mà không chuẩn hóa. Một cạm bẫy tiềm ẩn khác là giả sử các điểm neo cố định hoặc so sánh hộp giới hạn mà không tính đến tính đối xứng. Vì các thành phần có thể được chuyển đổi, chữ ký ổn định duy nhất là thứ gì đó bất biến khi quay và phản xạ, chẳng hạn như cấu trúc của đồ thị kề hoặc chuẩn hóa tọa độ chính tắc. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là coi mọi thành phần được kết nối như một tập hợp các ô và so sánh nó với tất cả các hướng có thể có của ba hình dạng mảnh đã biết. Đối với mỗi thành phần, chúng tôi sẽ liệt kê tất cả các góc quay và hình phản chiếu của từng hình dạng và cố gắng khớp nó với các ô của thành phần. Nếu chúng ta đại diện cho một thành phần với$k$các ô, một kết hợp đơn giản có thể tốn kém$O(k)$theo hướng và với nhiều hướng cho mỗi hình dạng, điều này trở nên đắt đỏ. Trong trường hợp xấu nhất, nếu tất cả các ô tạo thành một thành phần lớn thì thành phần này sẽ thoái hóa thành$O(nm)$so sánh trên mỗi hình dạng, dẫn đến$O((nm)^2)$hành vi, rõ ràng là quá chậm. 

Quan sát quan trọng là vấn đề đảm bảo một lát gạch đầy đủ chỉ có ba loại mảnh, nghĩa là mỗi thành phần được kết nối chính xác là một trong những loại đó. Điều này cho phép chúng ta tránh hoàn toàn việc kết hợp hình học. Thay vào đó, chúng ta có thể mô tả từng thành phần bằng cấu trúc kề của nó: mỗi ô có bao nhiêu lân cận trong cùng một vùng màu. Đối với các hình dạng giống như polyomino, đặc biệt là các hình cố định nhỏ, nhiều mức độ hoặc chữ ký truyền tải chính tắc sẽ xác định duy nhất hình dạng ngay cả khi xoay hoặc phản chiếu. 

Vì vậy, thay vì thử tất cả các phép biến đổi, chúng tôi chỉ cần lấp đầy từng thành phần, tính toán chữ ký cấu trúc nhỏ gọn và phân loại nó bằng cách sử dụng chữ ký được tính toán trước cho ba hình dạng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O((nm)^2)$|$O(nm)$| Quá chậm | 
| Tối ưu (thành phần + chữ ký) |$O(nm)$|$O(nm)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý lưới dưới dạng biểu đồ trong đó mỗi ô khác 0 được kết nối với bốn ô lân cận nếu chúng có cùng màu. 

1. Quét từng ô lưới. Khi chúng tôi tìm thấy một ô khác 0 chưa được truy cập, chúng tôi sẽ bắt đầu tràn từ ô đó. Điều này cô lập một thành phần được kết nối tương ứng với một phần duy nhất. 
2. Trong quá trình lấp lũ, thu thập tất cả các ô thuộc thành phần này. Chúng tôi lưu trữ tọa độ của chúng để có thể phân tích cấu trúc sau đó. 
3. Trong khi khám phá thành phần, hãy xây dựng cấu trúc liền kề của nó. Đối với mỗi ô, hãy đếm xem có bao nhiêu ô lân cận trong thành phần đó. Thông tin mức độ này là bất biến khi quay một
