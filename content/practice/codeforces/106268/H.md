---
title: "CF 106268H - Tấm chữ U"
description: "Chúng ta có một lưới hình chữ nhật tượng trưng cho một cái ao, trong đó mỗi ô phải được che phủ hoặc phải để trống. Cấu hình mục tiêu được mô tả bằng mẫu và . ký tự: mỗi ô phải được bao phủ chính xác một lần bởi một tập hợp các bảng cứng giống hệt nhau, trong khi ."
date: "2026-06-18T23:09:16+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106268
codeforces_index: "H"
codeforces_contest_name: "The 2025 Asia Yokohama Regional Contest"
rating: 0
weight: 106268
solve_time_s: 29
verified: false
draft: false
---

[CF 106268H - Tấm hình chữ U](https://codeforces.com/problemset/problem/106268/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 29s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một lưới hình chữ nhật tượng trưng cho một cái ao, trong đó mỗi ô phải được che phủ hoặc phải để trống. Cấu hình mục tiêu được mô tả bằng mẫu`#`Và`.`ký tự: mỗi`#`ô phải được che phủ chính xác một lần bằng một bộ tấm cứng giống hệt nhau, đồng thời`.`các tế bào phải được giữ nguyên. 

Mỗi bảng là một polyomino cố định có hình dạng giống như một khung hình vuông rỗng. Chính xác hơn, nó vừa khít bên trong một hộp giới hạn k x k, nhưng bên trong nó trống ngoại trừ cấu trúc biên, tạo thành một đường viền “hình chữ U” chiếm chính xác 3k − 2 đơn vị ô vuông dọc theo đường viền. Các bảng có thể xoay tùy ý nhưng không được chồng lên nhau, không được mở rộng ra ngoài lưới và phải bao phủ chính xác tất cả`#`các tế bào không có vùng phủ sóng bổ sung trên`.`tế bào. 

Nhiệm vụ là quyết định xem có thể đặt một số bảng như vậy trên lưới sao cho sự kết hợp của các ô được chiếm giữ của chúng khớp chính xác với tập hợp các ô được sử dụng hay không.`#`tế bào. 

Các ràng buộc n, m ≤ 1000 và tổng diện tích lưới trên các trường hợp thử nghiệm lên tới 10^6 chỉ ra rằng bất kỳ giải pháp nào kém tuyến tính hoặc gần tuyến tính về kích thước lưới cho mỗi trường hợp thử nghiệm sẽ quá chậm. Điều này gợi ý rõ ràng rằng chúng ta phải xử lý lưới trong thời gian tệ nhất là O(nm) hoặc O(nm log nm) và mọi nỗ lực thử tất cả các vị trí của bảng một cách rõ ràng sẽ thất bại. 

Khó khăn chính là các bảng chồng lên nhau theo cách có cấu trúc: chúng không phải là các hình dạng tùy ý mà là các đường ranh giới bị ràng buộc, do đó các mẫu kề cận cục bộ phải mã hóa tính nhất quán toàn cục. 

Một số trường hợp đặc biệt quan trọng ngay lập tức. 

Trường hợp cạnh đầu tiên là khi một thành phần được kết nối của`#`quá nhỏ để chứa dù chỉ một ranh giới bảng hợp lệ. Ví dụ: một cụm nhỏ như:```
###
#.#
###
```không thể lưu trữ bảng điều khiển k = 5 vì diện tích của bảng điều khiển lớn hơn nhiều so với bất kỳ cấu trúc cục bộ nào. Một thuật toán đơn giản chỉ kiểm tra các điều kiện mức độ cục bộ có thể chấp nhận những trường hợp như vậy một cách không chính xác. 

Trường hợp cạnh thứ hai là khi hình dạng nhất quán cục bộ nhưng không nhất quán trên toàn cầu do tính chẵn lẻ của đường dẫn hoặc cấu trúc phân nhánh không khớp. Ví dụ, hành lang dài mỏng của`#`các ô có thể có vẻ tương thích với ranh giới hình chữ U cục bộ, nhưng không thành công vì các góc của bảng phải căn chỉnh theo cấu trúc tuần hoàn cứng nhắc. 

Trường hợp cạnh thứ ba là cấu trúc ngụ ý chồng chéo: nhiều bảng có thể “cạnh tranh” cho cùng một phân đoạn ranh giới nếu chúng ta chỉ kiểm tra tính lân cận một cách tham lam, vì vậy tính chính xác đòi hỏi phải đảm bảo rằng mọi`#`ô được gán cho chính xác một chu kỳ ranh giới nhất quán. 

## Phương pháp tiếp cận 

Cách giải thích mạnh mẽ là thử đặt các bảng ở tất cả các vị trí và hướng có thể có trên lưới và kiểm tra xem một số tập hợp con có bao phủ chính xác tất cả các bảng hay không.`#`tế bào. Mỗi vị trí bao gồm việc kiểm tra các ô O(k^2) và số lượng vị trí là O(nm) trên mỗi hướng, với tối đa 8 hướng trong trường hợp xấu nhất. Điều này đã dẫn đến O(nm k^2) cho mỗi trường hợp thử nghiệm, vượt xa giới hạn khi k và kích thước lưới đều có thể đạt tới 1000. 

Ngay cả khi chúng tôi cố gắng cắt bớt các vị trí bằng cách chỉ bắt đầu từ`#`tế bào, chúng ta vẫn phải đối mặt với sự lựa chọn theo cấp số nhân: e
