---
title: "CF 104886E - Kết hợp đường dẫn cây ngẫu nhiên"
description: "Chúng ta có một cây có gốc với trọng số ở mỗi đỉnh. Đối với mỗi truy vấn, chúng tôi xem xét hai nút, lấy đường dẫn đơn giản duy nhất từ ​​gốc đến từng nút, sau đó cố gắng “căn chỉnh” hai đường dẫn từ gốc đến nút này."
date: "2026-06-28T09:06:59+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104886
codeforces_index: "E"
codeforces_contest_name: "USI-Team-Selection 2023-2024"
rating: 0
weight: 104886
solve_time_s: 29
verified: false
draft: false
---

[CF 104886E - So khớp đường dẫn cây ngẫu nhiên](https://codeforces.com/problemset/problem/104886/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 29s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cây có gốc với trọng số ở mỗi đỉnh. Đối với mỗi truy vấn, chúng tôi xem xét hai nút, lấy đường dẫn đơn giản duy nhất từ ​​gốc đến từng nút, sau đó cố gắng “căn chỉnh” hai đường dẫn từ gốc đến nút này. 

Nhiệm vụ không phải là so sánh trực tiếp các đường dẫn dưới dạng các chuỗi mà là chọn hai chuỗi con, một chuỗi từ mỗi đường dẫn từ gốc đến nút, có cùng độ dài. Khi chúng tôi chọn một cặp dãy con như vậy, chúng tôi ghép các phần tử theo vị trí và tính tổng các tích có trọng số tương ứng. Mục tiêu của mỗi truy vấn là tối đa hóa giá trị này. 

Vì vậy, về mặt khái niệm, chúng tôi đang chọn một chuỗi các nút phù hợp dọc theo hai đường dẫn từ gốc đến nút, giữ nguyên thứ tự trong cả hai và tối đa hóa tích số chấm giữa các trọng số đã chọn. 

Bản thân cái cây không có tính đối nghịch; nó được tạo ngẫu nhiên với mỗi nút gắn vào một nút được chọn thống nhất trước đó. Chi tiết đó không mang tính chất trang trí. Nó đảm bảo rằng các đường dẫn gốc đến nút thông thường có độ dài trung bình ngắn, nhưng độ sâu trong trường hợp xấu nhất vẫn là tuyến tính, do đó mọi giải pháp đều phải hoạt động với độ dài đường dẫn trong trường hợp xấu nhất. 

Một cách giải thích ngây thơ sẽ gợi ý so sánh tất cả các cặp dãy con, nhưng điều đó đã gợi ý về sự bùng nổ theo cấp số nhân. Ngay cả việc hạn chế lập trình động trên hai chuỗi vẫn sẽ tốn chi phí bậc hai cho mỗi truy vấn trong trường hợp xấu nhất, con số này quá lớn nếu có nhiều truy vấn. 

Một trường hợp phức tạp nhưng quan trọng xuất hiện khi một đường dẫn là tiền tố của đường kia hoặc khi cả hai đường dẫn có chung một tiền tố dài và sau đó phân kỳ. Trong những trường hợp này, nhiều dãy con ứng cử viên phù hợp sẽ thu gọn thành các sắp xếp có cấu trúc tương tự nhau và DP ngây thơ sẽ tính toán lại các chuyển đổi tương tự nhiều lần. 

Ví dụ: nếu cả hai nút đều là hậu duệ sâu của 1 và đường dẫn của chúng gần như giống hệt nhau ngoại trừ sự phân kỳ hậu tố nhỏ, thì việc tính toán lại DP đầy đủ cho mỗi truy vấn sẽ lãng phí tỷ lệ thuận với độ sâu đầy đủ mặc dù chỉ khác một hậu tố nhỏ. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp xử lý từng truy vấn một cách độc lập. Chúng tôi trích xuất hai chuỗi gốc đến nút và chạy lập trình động kiểu chuỗi con chung dài nhất cổ điển, nhưng thay vì tối đa hóa độ dài, chúng tôi tối đa hóa tích số chấm có trọng số. Đối với các chuỗi có độ dài d1 và d2, DP này có giá O(d1 · d2). Trong cây suy biến có độ sâu là O(n), một truy vấn sẽ trở thành bậc hai. Với tối đa 10^5 truy vấn, điều này hoàn toàn không khả thi. 

Quan sát cấu trúc quan trọng là cả hai chuỗi đều là đường dẫn gốc trong cùng một cây. Điều đó có nghĩa là chúng không phải là các mảng tùy ý: chúng có chung một tiền tố dài và chỉ phân kỳ sau tổ tiên chung thấp nhất của chúng. Nếu chúng ta phân tách cả hai đường dẫn thành đoạn từ gốc đến LCA và từ LCA trở xuống
