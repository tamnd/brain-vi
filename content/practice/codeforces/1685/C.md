---
title: "CF 1685C - Mang lại sự cân bằng"
description: "Chúng ta được cho một chuỗi các dấu ngoặc đơn có độ dài $2n$, chứa chính xác $n$ dấu ngoặc mở và $n$. Nhiệm vụ là chuyển đổi chuỗi này thành một chuỗi dấu ngoặc đơn cân bằng chính xác bằng cách sử dụng số lượng thao tác tối thiểu, trong đó mỗi thao tác bao gồm đảo ngược…"
date: "2026-06-09T23:49:03+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "constructive-algorithms", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1685
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 794 (Div. 1)"
rating: 2600
weight: 1685
solve_time_s: 45
verified: false
draft: false
---

[CF 1685C - Mang lại sự cân bằng](https://codeforces.com/problemset/problem/1685/C) 

**Đánh giá:** 2600 
**Tags:** vũ lực, thuật toán mang tính xây dựng, lòng tham 
**Thời gian giải:** 45s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một chuỗi các dấu ngoặc đơn có độ dài$2n$, chứa chính xác$n$mở đầu và$n$dấu ngoặc đóng. Nhiệm vụ là chuyển đổi chuỗi này thành chuỗi dấu ngoặc đơn cân bằng chính xác bằng cách sử dụng số lượng thao tác tối thiểu, trong đó mỗi thao tác bao gồm việc đảo ngược bất kỳ chuỗi con liền kề nào. 

Chuỗi dấu ngoặc cân bằng là chuỗi trong đó tiền tố không bao giờ chứa nhiều dấu ngoặc đóng hơn dấu ngoặc mở và tổng số dấu ngoặc mở và đóng khớp nhau. Mục tiêu cuối cùng không phải là sắp xếp lại một cách tùy tiện mà cụ thể là để đạt được bất kỳ cấu hình cân bằng hợp lệ nào bằng cách sử dụng đảo ngược chuỗi con. 

Hạn chế chính là tổng của$n$trên tất cả các trường hợp thử nghiệm là nhiều nhất$2 \cdot 10^5$. Điều này ngay lập tức loại trừ mọi thứ bậc hai cho mỗi trường hợp thử nghiệm, vì một mô phỏng ngây thơ của các tìm kiếm toàn cầu lặp đi lặp lại hoặc sửa lỗi tham lam bằng các lần quét lại sẽ giảm xuống$O(n^2)$trong trường hợp xấu nhất và hết thời gian. Chúng ta cần một cấu trúc chạy theo thời gian tuyến tính cho mỗi trường hợp thử nghiệm hoặc gần với nó. 

Một điểm tinh tế trong vấn đề này là sự đảo ngược rất mạnh mẽ: chúng có thể di chuyển nhân vật qua một khoảng cách xa đồng thời chuyển đổi vai trò cục bộ của họ. Điều này khiến người ta dễ vô tình cho rằng vấn đề hoạt động giống như các giao dịch hoán đổi liền kề, điều này không đúng. 

Một số trường hợp đặc biệt bộc lộ các chiến lược ngây thơ. 

Nếu chuỗi đã cân bằng thì câu trả lời là 0. Bất kỳ thuật toán nào không kiểm tra số dư một cách rõ ràng vẫn có thể cố gắng áp dụng các hoạt động không cần thiết. 

Nếu chuỗi bị đảo ngược hoàn toàn như`)))...(((`, một cách tiếp cận sửa tiền tố tham lam chỉ khắc phục một sự mất cân bằng tại một thời điểm có thể liên tục di chuyển các dấu ngoặc đơn và tạo ra quá nhiều thao tác, mặc dù câu trả lời tối ưu là nhỏ. 

Một trường hợp phức tạp khác là cấu trúc xen kẽ như`())(())(()...)`, trong đó các bản sửa lỗi tham lam cục bộ có thể “sửa quá mức” các vùng trước đó và phá vỡ cấu trúc cố định trước đó nếu không được kiểm soát cẩn thận. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là mô phỏng quá trình: quét chuỗi liên tục để tìm vị trí đầu tiên mà số dư tiền tố trở nên âm, sau đó tìm kiếm vị trí mà việc đảo ngược sẽ sửa tiền tố đó. Sau mỗi thao tác, hãy tính lại số dư từ đầu. 

Về nguyên tắc, điều này đúng vì mọi sự đảo ngược đều bảo toàn nhiều dấu ngoặc đơn và các bản sửa lỗi cục bộ lặp đi lặp lại cuối cùng có thể loại bỏ tất cả các vi phạm tiền tố. Tuy nhiên, mỗi thao tác có thể yêu cầu quét toàn bộ chuỗi để xác định vi phạm tiếp theo và có thể có tới$O(n)$hoạt động. Điều này dẫn đến$O(n^2)$hành vi trên mỗi trường hợp thử nghiệm, quá chậm đối với$2 \cdot 10^5$tổng chiều dài. 

Quan sát quan trọng là chúng ta không cần mô phỏng các trạng thái trung gian. Thay vào đó, chúng ta có thể trực tiếp xây dựng một chuỗi hợp lệ cuối cùng và “sửa chữa” chuỗi bằng cách cố định các vị trí từ trái sang phải. Mỗi lần chúng tôi phát hiện rằng tiền tố hiện tại không thể tạo thành một chuỗi hợp lệ, chúng tôi sử dụng một đảo ngược duy nhất để đưa ký tự cần thiết vào vị trí trong khi vẫn duy trì cấu trúc tiền tố đã cố định. 

Cấu trúc sâu hơn là một chuỗi cân bằng có thể được xây dựng một cách tham lam từ trái sang phải bằng cách đảm bảo rằng ở mỗi vị trí chúng ta đặt đúng dấu ngoặc đơn cần thiết để duy trì tính khả thi. Vì việc đảo ngược có thể di chuyển bất kỳ ký tự nào trong tương lai vào vị trí hiện tại nên mỗi lần sửa có thể được thực hiện bằng một thao tác. 

Điều này làm giảm vấn đề thành một quy trình tham lam mang tính xây dựng nhằm căn chỉnh chuỗi đã cho thành cấu trúc hợp lệ mục tiêu mà không cần tính toán lại toàn bộ trạng thái toàn cục. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu |$O(n^2)$|$O(n)$| Quá chậm | 
| Xây dựng tham lam với sự đảo ngược |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì một chuỗi hoạt động và từng bước xây dựng một tiền tố hợp lệ. Chúng tôi cũng theo dõi số dư hiện tại theo số lần mở trừ đi số lần đóng. 

1. Lặp lại các vị trí từ trái sang phải. Tại mỗi vị trí, hãy quyết định xem chúng ta cần dấu ngoặc mở hay đóng để giữ cho tiền tố hợp lệ. Tiền tố hợp lệ không bao giờ được có số dư âm, vì vậy, bất cứ khi nào số dư ở mức thấp, chúng tôi sẽ ưu tiên sử dụng dấu ngoặc mở. 
2. Nếu ký tự hiện tại đã khớp với những gì chúng tôi cần, chúng tôi chỉ cần tiếp tục và cập nhật số dư. 
3. Nếu không khớp, chúng ta xác định vị trí tiếp theo bên phải có chứa ký tự mong muốn. 
4. Chúng ta đảo ngược chuỗi con giữa vị trí hiện tại và vị trí tìm thấy đó. Điều này đưa ký tự cần thiết vào đúng vị trí trong khi chỉ ảnh hưởng đến hậu tố chưa được sửa. 
5. Sau khi thực hiện rev
