---
title: "CF 1924D - Dãy số cân bằng"
description: "Chúng tôi đang làm việc với các chuỗi chỉ được tạo từ hai loại dấu ngoặc đơn và chúng tôi sửa số lượng dấu ngoặc mở và đóng mà chúng tôi phải sử dụng. Mỗi trường hợp thử nghiệm đưa ra ba số: chúng ta phải đặt bao nhiêu '(', chúng ta phải đặt bao nhiêu ')' và tham số đích k."
date: "2026-06-08T19:07:39+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "dp", "math"]
categories: ["algorithms"]
codeforces_contest: 1924
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 921 (Div. 1)"
rating: 2700
weight: 1924
solve_time_s: 32
verified: false
draft: false
---

[CF 1924D - Chuỗi cân bằng](https://codeforces.com/problemset/problem/1924/D) 

**Đánh giá:** 2700 
**Tags:** tổ hợp, dp, toán 
**Thời gian giải:** 32s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang làm việc với các chuỗi chỉ được tạo từ hai loại dấu ngoặc đơn và chúng tôi sửa số lượng dấu ngoặc mở và đóng mà chúng tôi phải sử dụng. Mỗi trường hợp thử nghiệm đưa ra ba số: chúng ta phải đặt bao nhiêu '(', chúng ta phải đặt bao nhiêu ')' và tham số đích k. 

Trong số tất cả các cách có thể để sắp xếp các dấu ngoặc này, chúng ta quan tâm đến dãy con cân bằng dài nhất. Một dãy con cân bằng được xác định theo nghĩa ngoặc thông thường: sau khi xóa một số ký tự (không sắp xếp lại), chúng ta có thể khớp các dấu ngoặc đơn để nó hoạt động giống như một biểu thức hợp lệ. Điểm mấu chốt là chúng tôi không yêu cầu toàn bộ chuỗi phải cân bằng, chỉ một số chuỗi con có thể khớp hoàn toàn và chúng tôi đo kích thước tối đa có thể có của chuỗi con đó. 

Nhiệm vụ là đếm xem có bao nhiêu chuỗi ngoặc riêng biệt tạo ra chuỗi con cân bằng dài nhất có độ dài chính xác là 2k. 

Các ràng buộc rất chặt chẽ: mỗi số n, m, k có thể lên tới 2000 và có tới 3000 trường hợp kiểm thử. Bất kỳ giải pháp nào cố gắng liệt kê các chuỗi hoặc mô phỏng trực tiếp các chuỗi con sẽ ngay lập tức thất bại, vì số lượng các chuỗi tự nó theo thứ tự của các hệ số nhị thức như C (4000, 2000), rất lớn về mặt thiên văn. Ngay cả lập trình động theo khối hoặc tệ hơn cho mỗi trường hợp thử nghiệm cũng không khả thi; chúng tôi cần thứ gì đó gần O(nm) hoặc cao hơn cho mỗi lần kiểm tra. 

Trường hợp cạnh tinh tế xuất hiện khi k lớn so với min(n, m). Ví dụ: nếu n = m = 2 và k = 2, chỉ các chuỗi cân bằng hoàn toàn mới đóng góp, vì chuỗi con cân bằng dài nhất có thể không thể vượt quá 4. Nếu k quá lớn so với các dấu ngoặc đơn có sẵn, câu trả lời sẽ ngay lập tức trở thành 0. Một giải pháp đơn giản vẫn có thể cố gắng đếm các cấu hình và đếm vượt quá các trạng thái không thể. 

Một trường hợp góc quan trọng khác là khi k nhỏ. Ví dụ: nếu k = 1, chúng tôi buộc chuỗi con cân bằng dài nhất phải có chính xác hai ký tự, nghĩa là không có cặp dấu ngoặc đơn nào có thể được nhúng hoàn toàn vào cấu trúc dài hơn. Điều này hạn chế rất nhiều việc lồng ghép và rất dễ bị xử lý sai trong lý luận tham lam. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force sẽ liệt kê tất cả các chuỗi có n dấu ngoặc mở và m đóng, và đối với mỗi chuỗi, tính toán chuỗi con cân bằng dài nhất của nó bằng cách sử dụng quy trình khớp DP giống như ngăn xếp. Việc tính toán chuỗi con cân bằng tốt nhất của một chuỗi cố định là tuyến tính, nhưng số lượng chuỗi là C(n+m, n), là số mũ theo kích thước đầu vào. Ngay cả với n = m = 20, điều này đã trở nên không khả thi, do đó việc liệt kê hoàn toàn nằm ngoài tầm với. 

Chúng ta cần thay thế “kiểm tra từng chuỗi một cách độc lập” bằng “đếm tất cả các chuỗi theo cấu trúc”. 

Quan sát cấu trúc quan trọng là chuỗi con cân bằng dài nhất của chuỗi khung chỉ phụ thuộc vào số lượng cặp có thể khớp trong một chuỗi con tối ưu, tương đương với số lượng tối đa các cặp chính xác '(' và ')' có thể được trích xuất trong khi duy trì trật tự. Mức tối đa này bị chi phối bởi số lượng tiền tố cho phép đủ số lượng chưa từng có ')' và số lượng '(' vẫn có sẵn để ghép nối sau này. Trên thực tế, bất kỳ chuỗi nào cũng tạo ra quá trình so khớp tham lam: quét từ trái sang phải, duy trì bộ đếm cân bằng và so khớp bất cứ khi nào có thể. Việc so khớp tham lam này tạo ra số lượng cặp tối đa trong bất kỳ chuỗi con nào, do đó vấn đề giảm xuống việc đếm các chuỗi mà kết hợp tham lam tạo ra chính xác k cặp. 

Điều này biến bài toán thành bài toán đếm đường mạng bị ràng buộc. Chúng tôi coi '(' là +1 và ')' là -1 và chúng tôi mô phỏng số dư nhưng chúng tôi không yêu cầu số dư phải không âm. Thay vào đó, chúng tôi theo dõi số lượng kết quả trùng khớp thực sự được hình thành trong chuỗi con tốt nhất, tương ứng với số lần ')' tìm thấy '(' chưa từng có trước đó trong quy trình ghép nối tối ưu.

Thủ thuật tiêu chuẩn là diễn giải lại quy trình dưới dạng DP theo độ dài tiền tố và số dư hiện tại, nhưng điều đó vẫn quá lớn. Sự đơn giản hóa thực sự là hãy quan sát rằng chỉ có sự khác biệt giữa số lần mở và đóng sẵn có và số lượng trận đấu đã được hình thành mới là vấn đề quan trọng. Khi chúng tôi khắc phục rằng chuỗi con tối ưu có k kết quả trùng khớp, chúng tôi sẽ chọn một cách hiệu quả các ký hiệu k ')' nào sẽ khớp với k '(' ký hiệu nào, đồng thời đảm bảo các ràng buộc khả thi về thứ tự được thỏa mãn. 

Điều này dẫn đến DP tổ hợp trong đó chúng tôi xử lý chuỗi từ trái sang phải, theo dõi ba đại lượng: số lượng '(' và ')' chúng tôi đã sử dụng cho đến nay và số lượng cặp chúng tôi đã tạo thành trong kết quả khớp tối ưu. Việc chuyển đổi chỉ phụ thuộc vào việc ký tự tiếp theo là '(' hay ')' và liệu nó có được sử dụng để tạo thành một kết quả khớp tiềm năng mới hay trở nên không thể so sánh được. Không gian trạng thái DP là O(nm) cho mỗi trường hợp thử nghiệm. 

Để thực hiện điều này hiệu quả trong tất cả các trường hợp thử nghiệm, chúng tôi tính toán trước các giai thừa và giai thừa nghịch đảo lên tới 4000 để xử lý các chuyển đổi nhị thức nếu cần và sử dụng lại các bảng DP một cách cẩn thận. 

Tóm lại, Brute Force không thành công vì nó liệt kê các cấu trúc một cách rõ ràng, trong khi giải pháp tối ưu hóa lại nén một phần
