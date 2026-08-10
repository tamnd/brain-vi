---
title: "CF 104020I - Đơn vị Imperial không hoàn hảo"
description: "Chúng ta được cung cấp một tập hợp các quy tắc chuyển đổi giữa các đơn vị đo lường trừu tượng. Mỗi quy tắc nêu rõ rằng một đơn vị tương đương với số lượng theo tỷ lệ của một đơn vị khác."
date: "2026-07-02T04:41:45+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104020
codeforces_index: "I"
codeforces_contest_name: "2022 Benelux Algorithm Programming Contest (BAPC 22)"
rating: 0
weight: 104020
solve_time_s: 27
verified: false
draft: false
---

[CF 104020I - Đơn vị Đế quốc Không hoàn hảo](https://codeforces.com/problemset/problem/104020/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 27s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp các quy tắc chuyển đổi giữa các đơn vị đo lường trừu tượng. Mỗi quy tắc nêu rõ rằng một đơn vị tương đương với số lượng theo tỷ lệ của một đơn vị khác. Hệ thống nhất quán theo nghĩa là bất kỳ đơn vị nào được kết nối thông qua các quy tắc này đều có thể được chuyển đổi thành bất kỳ đơn vị nào khác dọc theo một đường dẫn duy nhất. 

Sau khi đọc tất cả các quy tắc chuyển đổi, chúng ta phải trả lời các truy vấn có dạng: cho một số lượng theo một đơn vị, tính giá trị của nó theo đơn vị khác nếu tồn tại đường dẫn chuyển đổi. Nếu không có chuỗi chuyển đổi nào kết nối hai đơn vị thì câu trả lời là không thể. 

Điểm trừu tượng chính là các đơn vị tạo thành các nút trong biểu đồ và mỗi quy tắc chuyển đổi tạo thành một cạnh có trọng số. Trọng số mã hóa một hệ số nhân và các truy vấn yêu cầu tích của các trọng số dọc theo một đường dẫn. 

Các ràng buộc được cố tình không đối xứng: số lượng quy tắc chuyển đổi nhỏ, nhiều nhất là 100, nhưng số lượng truy vấn lớn, lên tới 10.000. Điều này gợi ý rõ ràng rằng việc xử lý trước cấu trúc của hệ thống đơn vị một lần và sau đó trả lời các truy vấn nhanh chóng là chiến lược dự kiến. Bất kỳ giải pháp nào tính toán lại tìm kiếm đường dẫn cho mỗi truy vấn đều có nguy cơ lặp lại cùng một quá trình duyệt nhiều lần. 

Một vấn đề số khó phát hiện do các hệ số chuyển đổi là giá trị dấu phẩy động và phép nhân lặp lại dọc theo chuỗi dài có thể tích lũy lỗi chính xác. Dung sai yêu cầu tương đối nghiêm ngặt, vì vậy việc biểu diễn đường dẫn phải tránh việc tính toán lại các sản phẩm nổi lặp đi lặp lại không cần thiết. 

Một số trường hợp đặc biệt quan trọng. 

Nếu hai đơn vị tồn tại trong các thành phần bị ngắt kết nối khác nhau, chẳng hạn như “mét” và “inch” mà không có chuỗi giữa chúng, thì truy vấn giữa chúng sẽ không thể xuất ra ngay cả khi cả hai đều xuất hiện trong đầu vào. 

Nếu một thiết bị được kết nối qua nhiều bước trung gian, như A đến B đến C, chúng tôi phải đảm bảo rằng chúng tôi không vô tình tính toán lại các đường dẫn tỷ lệ không nhất quán do độ lệch nổi. 

Cuối cùng, các truy vấn có thể yêu cầu chuyển đổi theo một trong hai hướng so với quy tắc được lưu trữ, do đó việc biểu diễn phải hỗ trợ chia tỷ lệ hai chiều. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp xử lý từng truy vấn một cách độc lập. Đối với mỗi truy vấn, chúng tôi thực hiện tìm kiếm biểu đồ từ đơn vị nguồn đến đơn vị đích, nhân các hệ số chuyển đổi dọc theo đường dẫn. Bởi vì mỗi tìm kiếm có thể chạm vào tất cả các nút và cạnh, điều này gây ra trường hợp xấu nhất là khoảng 10.000 tìm kiếm trên một biểu đồ có tối đa 100 nút và 100 cạnh. Đó đã là ranh giới nhưng vẫn hợp lý; tuy nhiên, vấn đề thực sự là sự dư thừa. Cùng một biểu đồ được duyệt lặp đi lặp lại và vì mỗi cặp đơn vị có một đường dẫn chuyển đổi duy nhất nên mọi truy vấn sẽ tính toán lại thứ gì đó được cố định trên toàn cầu. 

Cấu trúc của bài toán cứng nhắc hơn so với đồ thị có trọng số tổng quát. Mỗi thành phần được kết nối hoạt động giống như một cây quan hệ nhân. Khi chúng tôi chỉ định một “tỷ lệ cơ sở” nhất quán cho mỗi đơn vị so với một gốc đại diện, mọi chuyển đổi sẽ giảm xuống thành một tra cứu tỷ lệ đơn giản. Điều này loại bỏ hoàn toàn việc truyền tải biểu đồ trong các truy vấn. 

Thông tin chi tiết quan trọng là gán cho mỗi đơn vị một giá trị chuẩn tương ứng với một gốc tùy ý trong thành phần được kết nối của nó. Nếu chúng ta biết rằng đơn vị A bằng x nhân căn và đơn vị B bằng y nhân căn, thì sự chuyển đổi A thành B chỉ đơn giản là x / y. Điều này biến các sản phẩm đường dẫn thành nhãn nút. 

Chúng tôi xây dựng các nhãn này bằng cách sử dụng một lần truyền tải duy nhất cho mỗi thành phần, BFS hoặc DFS, truyền các tỷ lệ đã biết từ một nút đến các nút lân cận của nó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| BFS/DFS lặp lại cho mỗi truy vấn | O(q(n + e)) | O(n + e) ​​| Quá chậm | 
| Ghi nhãn thành phần bằng DFS/BFS | O(n + e + q) | O(n + e) ​​| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng biểu đồ trong đó mỗi đơn vị là một nút và mỗi quy tắc chuyển đổi là một
