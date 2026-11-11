---
title: "CF 1554D - Diane"
description: "Chúng ta được yêu cầu xây dựng một chuỗi có độ dài n bằng cách sử dụng các chữ cái tiếng Anh viết thường sao cho có điều kiện chẵn lẻ rất mạnh: mọi chuỗi con không trống phải xuất hiện bên trong chuỗi với số lần lẻ khi chúng ta đếm tất cả các lần xuất hiện."
date: "2026-06-16T16:06:13+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy", "strings"]
categories: ["algorithms"]
codeforces_contest: 1554
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 735 (Div. 2)"
rating: 1800
weight: 1554
solve_time_s: 105
verified: false
draft: false
---

[CF 1554D - Diane](https://codeforces.com/problemset/problem/1554/D) 

**Đánh giá:** 1800 
**Tags:** thuật toán xây dựng, tham lam, chuỗi 
**Thời gian giải:** 1 phút 45s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xây dựng một chuỗi có độ dài`n`sử dụng các chữ cái tiếng Anh viết thường sao cho thỏa mãn điều kiện chẵn lẻ rất mạnh: mọi chuỗi con không trống phải xuất hiện bên trong chuỗi một số lần lẻ khi chúng ta đếm tất cả các lần xuất hiện. 

Chuỗi con ở đây có nghĩa là bất kỳ đoạn liền kề nào của chuỗi. Vì vậy, đối với một chuỗi cố định`s`, chúng tôi xem xét mọi khả năng`s[l:r]`và đối với mỗi giá trị chuỗi con riêng biệt, chúng tôi đếm số lần nó xuất hiện trên tất cả các vị trí. Yêu cầu là mỗi chuỗi con riêng biệt có tần số lẻ. 

Thách thức chính là các chuỗi con chồng chéo lên nhau rất nhiều, do đó số lượng có mối tương quan cao. Một nỗ lực ngây thơ xử lý các chuỗi con một cách độc lập sẽ thất bại ngay lập tức vì việc thay đổi một ký tự sẽ ảnh hưởng đến các chuỗi con Θ(n²). 

Các ràng buộc buộc chúng ta phải suy nghĩ về cách xây dựng tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm. Vì tổng của`n`tùy thuộc vào`3 · 10^5`, bất kỳ giải pháp nào tệ hơn O(n) cho mỗi trường hợp thử nghiệm đều có nguy cơ TLE. Ngay cả O(n log n) cho mỗi trường hợp thử nghiệm cũng quá chậm trong trường hợp xấu nhất. 

Cạm bẫy tiềm ẩn nguy hiểm nhất là giả sử điều kiện hoạt động giống như một hạn chế tần số đơn giản đối với các ký tự. Ví dụ: làm cho tất cả các ký tự trở nên khác biệt là không đủ để kiểm soát các chuỗi con lặp lại như`"ab"`hoặc`"bc"`xuất hiện nhiều lần trong các chuỗi lớn hơn. Một cạm bẫy khác là cố gắng suy luận cục bộ về các chuỗi con có độ dài cố định, bỏ qua các tương tác giữa các độ dài. 

Một ví dụ nhỏ về sự tỉnh táo cho thấy tại sao các công trình ngây thơ lại thất bại: 

cho`n = 3`,`"aaa"`tạo chuỗi con`"a"`xảy ra 3 lần (lẻ), nhưng`"aa"`xảy ra 2 lần (chẵn) nên không thành công. Điều này cho thấy cấu trúc lặp lại nhanh chóng phá vỡ điều kiện chẵn lẻ. 

Việc xây dựng đúng phải kiểm soát cẩn thận các mẫu lặp lại trên tất cả các độ dài chuỗi con cùng một lúc. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ là tạo ra một chuỗi ứng cử viên và liệt kê rõ ràng tất cả các chuỗi con, đếm tần số của chúng bằng cách sử dụng bản đồ băm và xác minh điều kiện chẵn lẻ. Điều này yêu cầu tạo ra các chuỗi con Θ(n2) và mỗi lần chèn vào cấu trúc băm sẽ được khấu hao O(1), mang lại O(n2) cho mỗi lần kiểm tra. Ngay cả đối với`n = 10^5`, điều này hoàn toàn không thể thực hiện được. 

Thông tin chi tiết về cấu trúc quan trọng là chúng ta thực sự không cần quản lý số lượng chuỗi con một cách rõ ràng. Thay vào đó, chúng tôi xây dựng một chuỗi có thuộc tính phá vỡ đối xứng mạnh, buộc mọi chuỗi con phải có một “hành vi ghép nối gương” duy nhất đảm bảo tính đa bội lẻ. Cấu trúc dự định cực kỳ đơn giản: xây dựng chuỗi theo mẫu tránh mọi cấu trúc chuỗi con lặp lại ngoại trừ theo cách được kiểm soát để đảm bảo tính nhất quán chẵn lẻ. 

Giải pháp kinh điển là xây dựng một chuỗi bằng cách sử dụng mẫu lặp lại trên một bảng chữ cái nhỏ trong khi vẫn đảm bảo cẩn thận rằng mỗi lần xuất hiện của chuỗi con đều có một chuỗi đối chiếu duy nhất ngoại trừ một lần xuất hiện không ghép đôi. Một cách rõ ràng để đạt được điều này là xây dựng một chuỗi trong đó các vị trí được sắp xếp sao cho bất kỳ sự xuất hiện của chuỗi con nào cũng có thể được ánh xạ tới một chuỗi đối xứng, ngoại trừ một chuỗi con duy nhất.
