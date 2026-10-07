---
title: "CF 104937E - Hải ly giám sát"
description: "Chúng ta được cung cấp một hệ thống định hướng về các mối quan hệ giữa hải ly. Mỗi cạnh đầu vào đại diện cho một cặp hải ly trong đó một cạnh hiện đang giám sát cạnh kia."
date: "2026-06-28T18:15:43+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104937
codeforces_index: "E"
codeforces_contest_name: "MITIT 2024 Advanced Round"
rating: 0
weight: 104937
solve_time_s: 26
verified: false
draft: false
---

[CF 104937E - Hải ly giám sát](https://codeforces.com/problemset/problem/104937/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 26s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một hệ thống định hướng về các mối quan hệ giữa hải ly. Mỗi cạnh đầu vào đại diện cho một cặp hải ly trong đó một cạnh hiện đang giám sát cạnh kia. Mỗi cặp luôn được định hướng chính xác theo một hướng và ban đầu hệ thống sao cho mỗi hải ly đều có ít nhất một cạnh giám sát đến. 

Hoạt động được phép mang tính cục bộ: chọn một cạnh có hướng hiện có và đảo hướng của nó. Vì vậy, nếu hiện tại chúng ta có cạnh từ u đến v, thay vào đó, chúng ta có thể đảo ngược nó để làm cho v giám sát u. Ràng buộc làm cho vấn đề trở nên không tầm thường có tính toàn cục: sau mỗi lần lật, mỗi con hải ly vẫn phải có ít nhất một cạnh tới. Chúng tôi cũng được cung cấp hướng cuối cùng mong muốn cho mọi cạnh và chúng tôi phải đạt được hướng đó bằng cách sử dụng số lần lật tối thiểu hoặc báo cáo rằng điều đó không thể thực hiện được. 

Về mặt khái niệm, mỗi cạnh là một mũi tên có thể chuyển đổi được, nhưng việc chuyển đổi nó có thể tạm thời làm mất đi cấp độ đến điểm cuối của nó, do đó các hoạt động được kết hợp chặt chẽ thông qua các ràng buộc về cấp độ đỉnh. 

Các ràng buộc rất lớn, với N và M lên tới 100000 cho mỗi trường hợp thử nghiệm và tổng số tiền cũng bị giới hạn bởi 100000. Điều này ngay lập tức loại trừ mọi cách tiếp cận mô phỏng chuỗi trạng thái dài hoặc cố gắng tính toán lại tính hợp lệ toàn cục sau mỗi thao tác một cách ngây thơ. Bất kỳ giải pháp nào cũng phải tuyến tính hoặc gần tuyến tính về số cạnh, vì thậm chí O(M log M) cho mỗi trường hợp thử nghiệm đều có thể chấp nhận được nhưng mọi thứ bậc hai đều không thể thực hiện được. 

Một điểm tinh tế quan trọng là tính khả thi không được xác định độc lập trên mỗi cạnh. Ngay cả khi mỗi cạnh riêng lẻ có thể được lật, thứ tự vẫn quan trọng vì các trạng thái trung gian phải bảo toàn đẳng thức ở mọi đỉnh. Một kẻ tham lam ngây thơ lật các cạnh trực tiếp về hướng mục tiêu của chúng có thể phá vỡ tính hợp lệ khi một đỉnh tạm thời mất cạnh đến cuối cùng của nó. 

Một trường hợp lỗi minh họa nhỏ là một tam giác: ba nút a, b, c với các cạnh tạo thành một chu trình. Nếu chúng ta cố gắng lật hai cạnh một cách độc lập để khớp với hướng đích, trước tiên chúng ta có thể phá vỡ cạnh đến duy nhất của một nút trước khi khôi phục nó, mặc dù cấu hình cuối cùng là hợp lệ. Câu trả lời đúng có thể yêu cầu một thứ tự cụ thể hoặc có thể không thực hiện được dù có tính nhất quán cục bộ. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực là coi đây là bài toán đường đi ngắn nhất trên tất cả các hướng hợp lệ của M cạnh, trong đó mỗi trạng thái là một vectơ định hướng và các chuyển đổi sẽ lật một cạnh nếu hướng kết quả giữ tất cả các mức độ ít nhất một. Biểu đồ trạng thái này có kích thước 2^M và thậm chí việc tạo ra các lân cận là O(M), khiến nó hoàn toàn không khả thi. 

Cái nhìn sâu sắc về cấu trúc quan trọng là tính khả thi chỉ được kiểm soát bởi mức độ đỉnh chứ không phải bởi cấu hình đầy đủ. Mỗi lần lật thay đổi độ của chính xác hai đỉnh bằng ±1. Ràng buộc “mỗi đỉnh luôn có ít nhất một bậc” có nghĩa là chúng ta không bao giờ được để bậc vô độ của một đỉnh giảm xuống 0. Vì vậy, vấn đề trở thành vấn đề sắp xếp các bản cập nhật đã ký để không có tiền tố nào vi phạm ràng buộc giới hạn dưới. 

Điều này gợi ý việc sắp xếp lại các cạnh như các hoạt động tiêu thụ và tạo ra “sự hỗ trợ” tại các đỉnh. Mỗi cạnh ban đầu đóng góp hỗ trợ cho một điểm cuối và cuối cùng phải đóng góp cho điểm cuối kia. Một cú lật di chuyển một đơn vị hỗ trợ qua cạnh và hạn chế là mỗi đỉnh luôn có ít nhất một đơn vị hỗ trợ. 

Phối cảnh đúng là xem mỗi đỉnh luôn cần ít nhất một cạnh đến đang hoạt động, vì vậy chúng ta phải duy trì sự phân công động trong đó các cạnh sự cố hiện đang “bao phủ” nó. Nếu một cạnh là cạnh đến cuối cùng còn lại của một đỉnh thì nó sẽ bị khóa cho đến khi một cạnh đến khác được tạo cho đỉnh đó. Điều này ngay lập tức biến quy trình thành một biểu đồ phụ thuộc trên các cạnh: một cạnh không thể bị lật trước khi có vùng phủ sóng thay thế cho nguồn hoặc đích của nó, tùy thuộc vào vai trò hiện tại của nó.

Giải pháp tối ưu xuất hiện từ việc xây dựng một cấu trúc bao trùm các cạnh đảm bảo chuỗi các cú lật an toàn. Chúng tôi mô hình hóa sự phụ thuộc giữa các cạnh dựa trên việc liệu việc lật một cạnh có tạm thời làm lộ ra một đỉnh hay không. Cấu trúc phụ thuộc này không theo chu kỳ khi có thể truy cập được cấu hình mục tiêu và thứ tự tôpô của các lần lật cạnh sẽ đưa ra số lượng thao tác tối thiểu, chính xác là số cạnh có hướng khác với hướng ban đầu. 

Do đó, thay vì tìm kiếm theo hướng, chúng tôi giảm vấn đề xuống việc xác định thứ tự hợp lệ để áp dụng các lần lật cần thiết, trong đó mỗi lần lật chỉ được phép khi cả hai điểm cuối còn lại đủ hỗ trợ đến từ các cạnh khác. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force về định hướng | O(2^M) | O(2^M) | Quá chậm | 
| Thứ tự phụ thuộc trên các cạnh | O(N + M) | O(N + M) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi coi mỗi cạnh được định hướng ban đầu theo cấu trúc đầu vào (ngầm một hướng, nhưng chúng tôi hiểu nó là hướng hiện tại) và chúng tôi so sánh nó với hướng mong muốn. 

Chúng tôi duy trì mức độ hiện tại của nó trong cấu hình đang phát triển cho mọi đỉnh. Chúng ta cũng duy trì, đối với mỗi đỉnh, tập hợp các cạnh phụ hiện vẫn trỏ vào nó. Đây là những cạnh cung cấp hỗ trợ. 

Chúng tôi cũng phân loại các cạnh thành những cạnh phải được lật và những cạnh đã khớp với mục tiêu. 

Ý tưởng cốt lõi là chỉ lật một cạnh khi cả hai điểm cuối đều không giảm xuống 0 độ do việc lật. 

### bước 

1. Tính góc ban đầu cho mỗi đỉnh từ hướng ban đầu của tất cả các cạnh. 
2. Xác định mỗi cạnh có “sai hướng” hay không, nghĩa là phải lật. 
3. Xây dựng danh sách kề để mỗi đỉnh biết cạnh nào hiện đang đóng góp hỗ trợ tới cho nó. 
4. Duy trì một danh sách các cạnh có thể lật một cách an toàn. Một cạnh được coi là an toàn nếu cả hai điểm cuối hiện có ít nhất một cạnh đến khác ngoài cạnh này tại thời điểm lật. Điều này đảm bảo rằng việc loại bỏ đóng góp hiện tại của nó không vi phạm ràng buộc mức độ tối thiểu. 
5. Liên tục lấy một cạnh an toàn và lật nó, cập nhật
