---
title: "CF 103466F - Phân loại giấy"
description: "Chúng tôi duy trì một danh sách động các chuỗi, được lập chỉ mục từ 1 đến n, có thể được sắp xếp lại bằng các thao tác hoán đổi. Cùng với danh sách đang phát triển này, chúng tôi nhận được các truy vấn hỏi về một phân đoạn chỉ mục [l, r] và chuỗi truy vấn q cùng với ngưỡng k."
date: "2026-07-03T06:48:51+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103466
codeforces_index: "F"
codeforces_contest_name: "The 2019 ICPC Asia Nanjing Regional Contest"
rating: 0
weight: 103466
solve_time_s: 26
verified: false
draft: false
---

[CF 103466F - Chấm điểm trên giấy](https://codeforces.com/problemset/problem/103466/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 26s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi duy trì một danh sách động các chuỗi, được lập chỉ mục từ 1 đến n, có thể được sắp xếp lại bằng các thao tác hoán đổi. Cùng với danh sách đang phát triển này, chúng tôi nhận được các truy vấn hỏi về một phân đoạn chỉ mục [l, r] và chuỗi truy vấn q cùng với ngưỡng k. 

Đối với một truy vấn, chúng ta phải đếm có bao nhiêu chuỗi ở các vị trí từ l đến r có chung một tiền tố với q có độ dài ít nhất là k. Nói cách khác, đối với mỗi chuỗi si theo thứ tự mảng hiện tại, chúng tôi so sánh nó với q và tính tiền tố dài nhất mà chúng chia sẻ; chúng ta đếm si nếu giá trị đó ít nhất là k. 

Khó khăn chính là cả thứ tự mảng cơ bản và bộ truy vấn đều động. Thao tác hoán đổi thay đổi chuỗi nào nằm trong phạm vi chỉ mục nhất định và các truy vấn có thể xuất hiện ở bất kỳ đâu trong chuỗi thao tác. Có tối đa 2×10^5 chuỗi và 2×10^5 thao tác, đồng thời tổng độ dài của tất cả các chuỗi và chuỗi truy vấn cũng bị giới hạn bởi 2×10^5, điều này cho thấy rõ ràng rằng mọi thao tác quét tuyến tính trên mỗi truy vấn trên chuỗi hoặc ký tự đều quá chậm. 

Một cách tiếp cận đơn giản, đối với mỗi truy vấn, sẽ quét phạm vi [l, r], tính LCP(q, si) và đếm những giá trị thỏa mãn ngưỡng. Trong trường hợp xấu nhất, đây là O(n · |q|) cho mỗi truy vấn, điều này sẽ dẫn đến so sánh tổng thể khoảng 10^10 ký tự và ngay lập tức thất bại. 

Một trường hợp cạnh tinh tế hơn xuất hiện khi k = 0. Trong trường hợp đó mọi chuỗi trong [l, r] đều đủ điều kiện, vì vậy câu trả lời chỉ đơn giản là r − l + 1 bất kể nội dung chuỗi. Một giải pháp đúng phải nhận ra rõ ràng rằng trường hợp này bỏ qua tất cả logic chuỗi. 

Một tình huống phức tạp khác là các thao tác hoán đổi: sau khi hoán đổi vị trí i và j, tất cả các truy vấn trong tương lai phải phản ánh thứ tự đã cập nhật. Bất kỳ giải pháp nào giả định vị trí tĩnh hoặc xử lý truy vấn ngoại tuyến mà không theo dõi chính xác các giao dịch hoán đổi sẽ âm thầm tính sai phạm vi. 

## Phương pháp tiếp cận 

Quan sát trọng tâm là điều kiện “LCP(q, si) ≥ k” tương đương với việc nói rằng k ký tự đầu tiên của si khớp với k ký tự đầu tiên của q. Điều này làm giảm vấn đề từ việc khớp tiền tố tùy ý đến sự bằng nhau chính xác trên các tiền tố có độ dài cố định. 

Vì vậy, mỗi truy vấn sẽ trở thành: trong số các chỉ số [l, r], đếm các chuỗi có tiền tố có độ dài k bằng q[0:k]. Nếu k vượt quá |q| thì câu trả lời ngay lập tức là 0. 

Điều này biến vấn đề thành một vấn đề đếm phạm vi động trên nhiều chuỗi, trong đó mỗi chuỗi được phân loại theo tiền tố có độ dài k, nhưng k thay đổi theo truy vấn. Chúng tôi không thể lập chỉ mục trước cho tất cả k riêng biệt. 

Cấu trúc khóa là coi mỗi chuỗi đóng góp nhiều “khóa tiền tố” trên toàn bộ độ dài tiền tố của nó. Tuy nhiên, việc lưu trữ tất cả các tiền tố một cách rõ ràng là quá lớn. Thay vào đó, chúng tôi sử dụng hàm băm cuộn (hoặc mã định danh đường dẫn ba) để bất kỳ tiền tố nào cũng có thể được biểu thị bằng O(1) sau khi xử lý trước. 

Chúng tôi duy trì cây phân đoạn trên các vị trí 1..n. Mỗi nút lưu trữ một tập hợp hàm băm (hoặc bản đồ tần số) của tất cả các hàm băm tiền tố chuỗi cho độ sâu tối đa cố định k lên đến một ngưỡng nào đó. Nhưng vì k thay đổi nên thay vào đó, chúng tôi có một góc nhìn khác: chúng tôi xử lý từng truy vấn bằng cách băm q[0:k], sau đó đếm xem có bao nhiêu chuỗi trong [l, r] có cùng hàm băm tiền tố đó ở độ sâu k. 

Để hỗ trợ điều này một cách hiệu quả, chúng tôi xây dựng cây phân đoạn ổn định (hoặc BIT của nhóm băm). Mỗi chuỗi đóng góp chuỗi băm tiền tố đầy đủ của nó, nhưng thay vì lưu trữ tất cả độ dài, chúng tôi tính toán trước các giá trị băm cuộn cho mỗi tiền tố của mỗi chuỗi và coi mỗi (vị trí, độ dài tiền tố) là một khóa. 

Vì tổng chiều dài chuỗi trên tất cả các đầu vào là 2×10^5, nên tổng số nút tiền tố trên tất cả các chuỗi cũng bị giới hạn, điều này cho phép chúng tôi nén tất cả các giá trị băm tiền tố và duy trì cây Fenwick hoặc cây phân đoạn theo các vị trí cho từng nhóm riêng biệt (hàm băm tiền tố, độ dài). 

Mỗi thao tác hoán đổi chỉ đơn giản là hoán đổi mã định danh của hai vị trí, trong cây phân đoạn là một bản cập nhật điểm trên hai chỉ mục.

Do đó, mỗi truy vấn sẽ trở thành: truy vấn cây phân đoạn trên [l, r] đếm xem có bao nhiêu vị trí hiện đang giữ một chuỗi có hàm băm tiền tố có độ dài k bằng hàm băm truy vấn. 

Lý do điều này hiệu quả là vì k là một phần của khóa, vì vậy chúng tôi không bao giờ trộn lẫn các độ dài tiền tố khác nhau; mỗi truy vấn giảm xuống thành kiểm tra đẳng thức 2D: vị trí trong phạm vi và khóa tiền tố khớp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(m · n · | q | ) | 
| Tiền tố băm + BIT trên các khóa | O((n + m) log n) | O(n + tổng số tiền tố) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giả sử hàm băm cuộn đa thức cho chuỗi, tính toán trước hàm băm tiền tố và lũy thừa. 

Chúng tôi nén tất cả các cặp riêng biệt (băm của tiền tố, k) từng xuất hiện trong truy vấn hoặc chuỗi. 

1. Tính toán trước các giá trị băm tiền tố cho mỗi chuỗi si, nhờ đó chúng ta có thể trích xuất hàm băm(si[0:k]) trong O(1) cho bất kỳ k nào. Điều này cho phép so sánh các tiền tố theo thời gian liên tục mà không cần quét từng ký tự. 
2. Thu thập tất cả các yêu cầu về tiền tố truy vấn. Đối với mỗi truy vấn có k > 0, hãy tính hàm băm của q[0:k] và lưu nó dưới dạng khóa (băm, k). Điều này đảm bảo mọi mẫu tiền tố cần thiết đều được biết trước. 
3. Đối với mỗi vị trí i từ 1 đến n và với mỗi độ dài tiền tố k xuất hiện trong bất kỳ truy vấn nào, chúng ta liên kết hàm băm của si[0:k] với vị trí i. Về mặt khái niệm, điều này tạo ra một tập hợp các “sự kiện” cho biết vị trí i thuộc về nhóm (băm, k). Vì tổng độ dài chuỗi bị giới hạn nên tổng số sự kiện như vậy vẫn có thể quản lý được. 
4. Xây dựng ánh xạ từ mỗi (băm, k) tới cây Fenwick qua các vị trí. Mỗi cây Fenwick lưu trữ các chỉ mục hiện chứa một chuỗi có tiền tố khớp với khóa đó. 
5. Khởi tạo bằng cách chèn tất cả các vị trí vào nhóm tiền tố tương ứng bằng chuỗi hiện tại của chúng. 
6. Đối với các thao tác hoán đổi (i, j), hãy xóa i và j khỏi tất cả các nhóm tiền tố co
