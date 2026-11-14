---
title: "CF 1777F - Tê một cách thoải mái"
description: "Chúng tôi được cung cấp một chuỗi các số nguyên không âm và được yêu cầu kiểm tra mọi phân đoạn liền kề. Đối với mỗi phân đoạn, chúng tôi tính toán hai giá trị: XOR theo bit của tất cả các phần tử trong phân đoạn và phần tử tối đa trong cùng phân đoạn đó."
date: "2026-06-09T11:40:36+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "data-structures", "divide-and-conquer", "strings", "trees"]
categories: ["algorithms"]
codeforces_contest: 1777
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 845 (Div. 2) and ByteRace 2023"
rating: 2400
weight: 1777
solve_time_s: 34
verified: false
draft: false
---

[CF 1777F - Tê đến mức thoải mái](https://codeforces.com/problemset/problem/1777/F) 

**Đánh giá:** 2400 
**Thẻ:** bitmask, cấu trúc dữ liệu, phân chia và chinh phục, chuỗi, cây 
**Thời gian giải:** 34s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một chuỗi các số nguyên không âm và được yêu cầu kiểm tra mọi phân đoạn liền kề. Đối với mỗi phân đoạn, chúng tôi tính toán hai giá trị: XOR theo bit của tất cả các phần tử trong phân đoạn và phần tử tối đa trong cùng phân đoạn đó. “Độ tê liệt” của phân đoạn được xác định là XOR của hai giá trị này. 

Nhiệm vụ là tìm mức độ tê liệt tối đa có thể có trên tất cả các mảng con. 

Các ràng buộc ngụ ý rằng kích thước mảng trên tất cả các trường hợp thử nghiệm tối đa là 2⋅10^5, do đó, bất kỳ giải pháp nào có hành vi bậc hai cho mỗi trường hợp thử nghiệm đều không thể thực hiện được. Việc liệt kê trực tiếp O(n^2) các mảng con, kết hợp với O(1) hoặc thậm chí O(log n) cho mỗi truy vấn, vẫn sẽ quá chậm trong trường hợp xấu nhất vì nó dẫn đến khoảng 2⋅10^10 mảng con trong tất cả các thử nghiệm. 

Khó khăn đến từ sự tương tác của hai thống kê mảng con toàn cầu: XOR và tối đa. XOR hoạt động tốt trong các hoạt động tiền tố, trong khi mức tối đa thì không. Sự không phù hợp này là trở ngại cốt lõi. 

Việc triển khai ngây thơ cũng sẽ thất bại theo những cách tinh tế nếu nó cố gắng “tối ưu hóa” bằng cách sửa các điểm cuối hoặc mở rộng các mảng con một cách tham lam, bởi vì việc tăng mức tối đa thường làm thay đổi cấu trúc XOR theo những cách không đơn điệu. 

Ví dụ, hãy xem xét`[1, 2, 3]`. Mảng con`[2, 3]`có tối đa 3 và XOR 1, cho 3 ⊕ 1 = 2. Mở rộng thành`[1, 2, 3]`tăng tối đa lên 3 nhưng XOR trở thành 0, cho 3 ⊕ 0 = 3. Rút gọn thành`[3]`cho 3 ⊕ 3 = 0. Không có mối quan hệ đơn điệu giữa kích thước mảng con và kết quả, do đó, bất kỳ chiến lược tham lam nào dựa trên việc mở rộng hoặc cực đại cố định đều thất bại. 

## Phương pháp tiếp cận 

Phương pháp brute-force liệt kê tất cả các mảng con, tính toán XOR và mức tối đa của chúng, đồng thời theo dõi kết quả tốt nhất. Với tiền tố XOR, phần XOR là O(1), nhưng tối đa vẫn có giá O(n) trừ khi được xử lý trước. Ngay cả với một bảng thưa thớt, quá trình xử lý trước sẽ giúp truy vấn, nhưng chúng tôi vẫn có các mảng con O(n^2), vượt xa giới hạn. 

Thông tin chi tiết về cấu trúc quan trọng là đảo ngược quan điểm: thay vì lặp lại các mảng con và tính toán mức tối đa, chúng tôi cố định một giá trị ứng cử viên ở mức tối đa và chỉ xem xét các mảng con trong đó giá trị đó là lớn nhất. Nếu chúng ta biết rằng giá trị lớn nhất của mảng con bằng một giá trị x nào đó thì mọi phần tử trong mảng con có nhiều nhất là x và ít nhất một phần tử bằng x. Điều này gợi ý việc chia mảng xung quanh các vị trí có phần tử vượt quá x. 

Điều này dẫn đến ý tưởng phân chia và chinh phục tiêu chuẩn đối với giá trị tối đa. Chúng tôi đệ quy chia mảng theo vị trí tối đa toàn cầu của nó. Bất kỳ mảng con nào không bao gồm mức tối đa đó hoàn toàn thuộc về một bên và bất kỳ mảng con nào bao gồm nó phải coi nó như điểm neo ràng buộc tối đa. 

cho một fi
