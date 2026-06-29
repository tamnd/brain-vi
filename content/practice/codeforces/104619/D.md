---
title: "CF 104619D - Chia lồi"
description: "Chúng ta có một đa giác lồi có thứ tự lên tới 100000 đỉnh. Chúng ta phải chọn hai điểm P và Q, mỗi điểm nằm trên các cạnh khác nhau của đa giác và vẽ đoạn PQ bên trong đa giác. Đoạn này chia đa giác thành hai đa giác lồi."
date: "2026-06-29T17:25:56+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104619
codeforces_index: "D"
codeforces_contest_name: "2023 ICPC Asia Taiwan Online Programming Contest"
rating: 0
weight: 104619
solve_time_s: 24
verified: false
draft: false
---

[CF 104619D - Chia một lồi](https://codeforces.com/problemset/problem/104619/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 24s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một đa giác lồi có thứ tự lên tới 100000 đỉnh. Chúng ta phải chọn hai điểm P và Q, mỗi điểm nằm trên các cạnh khác nhau của đa giác và vẽ đoạn PQ bên trong đa giác. Đoạn này chia đa giác thành hai đa giác lồi. Yêu cầu là hai đa giác thu được này phải có chu vi bằng nhau và trong số tất cả các lựa chọn hợp lệ như vậy, chúng ta phải giảm thiểu độ dài của PQ. 

Về mặt hình học, việc chọn P và Q xác định dây cung bên trong đa giác lồi. Cắt dọc theo dây cung này tạo ra hai chu trình biên: một chu trình đi từ P đến Q dọc theo một hướng của biên đa giác, và chu trình kia đi từ P đến Q dọc theo hướng ngược lại. Điều kiện là tổng chu vi dọc theo hai đường biên đó, bao gồm cả đoạn PQ, cân bằng sao cho cả hai đa giác thu được đều có cùng chu vi. 

Khó khăn chính là P và Q không bị giới hạn ở các đỉnh, chúng có thể nằm ở bất kỳ đâu trên các cạnh. Điều này biến vấn đề từ tổ hợp rời rạc thành tối ưu hóa liên tục trên các vị trí biên của đa giác. 

Ràng buộc n lên tới 100000 buộc bất kỳ lý do O(n^2) nào trên các cặp cạnh đều không thành công. Ngay cả O(n log n) chỉ khả thi nếu mỗi đỉnh được xử lý với số lần không đổi hoặc thông qua quét tuyến tính. Bất kỳ giải pháp nào cũng phải khai thác cấu trúc tuần hoàn của các ranh giới đa giác lồi và chuyển đổi các điều kiện hình học thành hàm đơn điệu dọc theo chu vi. 

Trường hợp cạnh tinh tế phát sinh khi P hoặc Q tiến tới các đỉnh. Việc triển khai ngây thơ chỉ xem xét các đỉnh hoặc giả sử các cạnh hoàn toàn rời rạc sẽ bỏ lỡ các đường cắt tối ưu trong đó điểm cân bằng nằm hoàn toàn bên trong các cạnh. Một cạm bẫy khác là bỏ qua rằng sự bằng nhau về chu vi phụ thuộc vào độ dài cung dọc theo ranh giới đa giác, chứ không chỉ khoảng cách Euclide. 

Ví dụ, trong một hình chữ nhật, đường cắt tối ưu để cân bằng chu vi có thể kết nối các điểm giữa của các cạnh đối diện thay vì bất kỳ cặp đỉnh nào. Việc hạn chế ứng viên ở các đỉnh sẽ thất bại ngay lập tức. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là chọn hai cạnh, tham số hóa các điểm P và Q trên chúng và thực thi điều kiện chu vi bằng nhau. Đối với mỗi cặp cạnh, chúng ta sẽ giải một phương trình liên tục hai biến và tính toán đoạn khả thi nhất. Ngay cả khi giải từng cặp là O(1), vẫn có các cặp cạnh O(n^2), đã vượt quá 10^10 phép toán ở giới hạn trên của n, vì vậy điều này là không thể. 

Quan sát cấu trúc quan trọng là độ lồi đảm bảo ranh giới hoạt động giống như một chuỗi tuần hoàn duy nhất trong đó khoảng cách dọc theo chu vi được xác định rõ ràng và đơn điệu. Nếu chúng ta cố định một điểm P trên đường biên, điều kiện là hai chu vi thu được có lực Q bằng nhau sẽ ở một vị trí đối cực cụ thể dọc theo chu vi: nó phải chia chu trình biên thành hai cung có độ dài được điều chỉnh phù hợp với một ràng buộc tổng cố định. 

Thay vì suy nghĩ dưới dạng hình học của các đoạn PQ, chúng ta diễn giải lại vấn đề như việc chọn hai điểm trên đường biên sao cho hiệu chu vi giữa hai đường biên bằng với độ dài của PQ một cách nhất quán. Điều này chuyển đổi điều kiện thành mối quan hệ giữa độ dài cung dọc theo cấu trúc hình tròn 1D. 

Khi mọi thứ được giảm xuống tham số chu vi hình tròn, vấn đề sẽ trở thành: với mỗi điểm P, hãy tìm điểm Q tương ứng ở một khoảng cách chu vi mục tiêu cụ thể, nhưng P và Q phải nằm trên các cạnh khác nhau, vì vậy chúng ta phải tránh các trường hợp suy biến trong đó ánh xạ rơi vào cùng một cấu hình cạnh. T
