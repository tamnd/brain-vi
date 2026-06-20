---
title: "CF 1042E - Vasya và Ma trận ma thuật"
description: "Chúng ta được cung cấp một lưới trong đó mỗi ô đều có một giá trị số và một con chip bắt đầu tại một ô cụ thể. Từ vị trí hiện tại, chip chỉ có thể di chuyển đến các ô có giá trị nhỏ hơn giá trị hiện tại."
date: "2026-06-16T17:54:32+07:00"
tags: ["codeforces", "competitive-programming", "dp", "math", "probabilities"]
categories: ["algorithms"]
codeforces_contest: 1042
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 510 (Div. 2)"
rating: 2300
weight: 1042
solve_time_s: 169
verified: false
draft: false
---

[CF 1042E - Vasya và Ma trận ma thuật](https://codeforces.com/problemset/problem/1042/E) 

**Đánh giá:** 2300 
**Tags:** dp, toán, xác suất 
**Thời gian giải:** 2m 49s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một lưới trong đó mỗi ô đều có một giá trị số và một con chip bắt đầu tại một ô cụ thể. Từ vị trí hiện tại, chip chỉ có thể di chuyển đến các ô có giá trị nhỏ hơn giá trị hiện tại. Trong số tất cả các ô đủ điều kiện như vậy, vị trí tiếp theo được chọn ngẫu nhiên một cách thống nhất. Sau mỗi lần di chuyển, chúng tôi cộng khoảng cách Euclide bình phương giữa vị trí trước đó và vị trí mới vào điểm chạy. Quá trình dừng lại khi chip đến một ô không có ô nào có giá trị nhỏ hơn ở bất kỳ đâu trong lưới. 

Vì vậy, lưới tạo ra một cấu trúc có hướng từ giá trị cao hơn đến giá trị thấp hơn, nhưng quá trình này không phải là một đường dẫn cố định. Ở mỗi bước, chip sẽ chọn ngẫu nhiên trong số tất cả các ô có giá trị thấp hơn trong toàn bộ ma trận, không chỉ các ô liền kề. Nhiệm vụ là tính tổng khoảng cách bình phương tích lũy dự kiến ​​cho đến khi quá trình kết thúc. 

Các ràng buộc cho phép lên tới một triệu ô. Điều này loại trừ mọi cách tiếp cận cố gắng mô phỏng chuyển tiếp một cách rõ ràng cho từng bước hoặc cho từng cặp ô. Ngay cả việc lưu trữ tất cả các mối quan hệ theo cặp giữa các ô cũng sẽ là phương trình bậc hai và không thể thực hiện được. 

Các trường hợp đặc biệt quan trọng đến từ cách “các ô có giá trị thấp hơn” hoạt động trên toàn cầu. Một sai lầm ngây thơ là cho rằng các bước di chuyển phụ thuộc vào sự kề cận hoặc sắp xếp các lân cận, nhưng tập hợp di chuyển là toàn cục trên toàn bộ ma trận. Một trường hợp tinh tế khác là các giá trị bằng nhau: các ô có giá trị bằng nhau không bao giờ có thể truy cập được từ nhau và một ô chỉ xem xét các giá trị nhỏ hơn. 

Điểm tinh tế thứ hai là nhiều bước di chuyển có thể xem lại cùng tọa độ thông qua các đường dẫn khác nhau, nhưng quy trình luôn giảm giá trị một cách nghiêm ngặt, do đó chuỗi các giá trị giảm đơn điệu và quy trình được đảm bảo kết thúc ở nhiều nhất số lượng giá trị riêng biệt. 

## Phương pháp tiếp cận 

Cách giải thích brute-force coi quá trình này như một chuỗi Markov trên tất cả các ô. Từ một ô, chúng tôi liệt kê tất cả các ô nhỏ hơn, tính toán xác suất chuyển tiếp và tính toán đệ quy chi phí dự kiến. Điều này đúng về mặt khái niệm vì mỗi trạng thái chỉ phụ thuộc vào các trạng thái thấp hơn. 

Tuy nhiên, công thức trực tiếp này không thành công ngay lập tức. Mỗi tiểu bang có tới$n \cdot m$chuyển tiếp và có$n \cdot m$các trạng thái, do đó, ngay cả việc xây dựng cấu trúc chuyển tiếp cũng là$O((nm)^2)$. Tệ hơn nữa, việc giải quyết hệ thống kỳ vọng một cách đơn giản sẽ yêu cầu DP lặp đi lặp lại trên một biểu đồ phụ thuộc dày đặc, điều này là không khả thi. 

Quan sát quan trọng là sự chuyển đổi chỉ phụ thuộc vào thứ tự xếp hạng của các giá trị. Nếu chúng ta xử lý các ô theo thứ tự giá trị tăng dần thì khi chúng ta tính toán câu trả lời cho một ô, tất cả các trạng thái mà nó có thể di chuyển đến đều đã được giải quyết. Tính ngẫu nhiên là đồng nhất trên tất cả các ô có giá trị thấp hơn, vì vậy chúng tôi chỉ cần thông tin tổng hợp về những ô đã được xử lý. 

Việc giảm trọng tâm là duy trì tổng số đóng góp dự kiến ​​​​trên tất cả các ô đã được xử lý, cùng với số lượng cho phép thể hiện lựa chọn thống nhất mà không cần liệt kê rõ ràng các chuyển đổi. Thuật ngữ khoảng cách bình phương phân tách độc đáo thành một dạng có thể được tích lũy bằng cách sử dụng thống kê tiền tố trên tọa độ. 

Do đó, bài toán trở thành một quá trình quét các giá trị, duy trì tổng hình học tổng hợp của các ô được xử lý và sử dụng chúng để tính toán chi phí chuyển đổi dự kiến ​​một cách hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O((nm)^2)$|$O(nm)$| Quá chậm | 
| DP được sắp xếp theo giá trị với tổng hợp |$O(nm \log (nm))$|$O(nm)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi sắp xếp tất cả các ô theo giá trị của chúng theo thứ tự tăng dần. Sau đó chúng tôi xử lý chúng theo nhóm có giá trị bằng nhau, vì bằng nhau
