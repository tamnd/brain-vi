---
title: "CF 103433C - Quà Tết"
description: "Chúng ta được cấp một chuỗi gồm các chữ số 0, 2, 5 và 6 và chúng ta được phép thay đổi bất kỳ ký tự nào thành bất kỳ chữ số được phép nào khác chỉ bằng một thao tác. Mục tiêu là chuyển đổi chuỗi sao cho nó thỏa mãn “điều kiện Năm Mới” cụ thể liên quan đến các chuỗi con “2025” và “2026”."
date: "2026-07-03T07:55:19+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103433
codeforces_index: "C"
codeforces_contest_name: "2018-2019 Russia Team Open, High School Programming Contest (VKOSHP 18)"
rating: 0
weight: 103433
solve_time_s: 34
verified: false
draft: false
---

[CF 103433C - Quà Tết](https://codeforces.com/problemset/problem/103433/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 34s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một chuỗi gồm các chữ số 0, 2, 5 và 6 và chúng ta được phép thay đổi bất kỳ ký tự nào thành bất kỳ chữ số được phép nào khác chỉ bằng một thao tác. Mục tiêu là chuyển đổi chuỗi sao cho nó thỏa mãn “điều kiện Năm Mới” cụ thể liên quan đến các chuỗi con “2025” và “2026”. 

Một chuỗi được coi là hợp lệ nếu nó đáp ứng ít nhất một trong các điều kiện sau: nó chứa chuỗi con “2026” hoặc nó không chứa chuỗi con “2025”. Vì hai điều kiện này trùng lặp một cách không hề tầm thường, nên vấn đề về cơ bản là yêu cầu số lượng thay thế ký tự tối thiểu cần thiết để chúng ta “ép buộc” thành công mẫu năm 2026 ở đâu đó hoặc chúng ta “phá vỡ” tất cả các lần xuất hiện của năm 2025. 

Đầu vào đưa ra nhiều trường hợp kiểm thử, mỗi trường hợp bao gồm một chuỗi. Đối với mỗi chuỗi, chúng ta phải tính toán số lần chỉnh sửa một ký tự tối thiểu cần thiết để làm cho nó hợp lệ. 

Các ràng buộc ngụ ý rằng mỗi chuỗi rất nhỏ, có độ dài tối đa là 20, trong khi số lượng ca kiểm thử có thể lớn. Sự kết hợp này gợi ý rõ ràng rằng bất kỳ giải pháp nào theo cấp số nhân về độ dài chuỗi đều có thể chấp nhận được, nhưng bất kỳ giải pháp nào nhân số đó với hệ số lớn cho mỗi trường hợp thử nghiệm vẫn sẽ ổn. Chúng ta có thể tự do xem xét tất cả các chuỗi con và tất cả sự sắp xếp của các mẫu mà không phải lo lắng về sự tăng vọt hiệu suất. 

Một số trường hợp khó nhận thấy ngay từ cái nhìn đầu tiên. 

Một là khi chuỗi đã chứa “2026”. Trong trường hợp đó, câu trả lời là 0 ngay cả khi có nhiều chuỗi con “2025” ở nơi khác, vì điều kiện đầu tiên đã được thỏa mãn. Ví dụ: trong “20252026”, không cần thực hiện thao tác nào. 

Một trường hợp khác là khi chuỗi chứa sự chồng chéo hoặc nhiều lần xuất hiện của “2025”. Ví dụ: trong “20252025”, việc sửa một lần xuất hiện có thể vẫn giữ nguyên một lần xuất hiện khác, vì vậy chúng ta không thể chỉ sửa một cửa sổ một cách tham lam; chúng ta phải xem xét tác động toàn cầu của các chỉnh sửa. 

Trường hợp thứ ba là khi cả hai mẫu xuất hiện dưới dạng các biến thể gần giống nhau. Ví dụ: “2025” và “2026” chỉ khác nhau ở một ký tự, do đó, một thay đổi duy nhất có thể chuyển đổi giữa việc đáp ứng và phá vỡ điều kiện. Một cách tiếp cận ngây thơ chỉ xem xét cục bộ một chuỗi con tại một thời điểm có thể bỏ lỡ rằng một chỉnh sửa duy nhất có thể loại bỏ đồng thời tất cả các lần xuất hiện “2025” đồng thời tạo ra “2026”. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử mọi cách có thể để sửa đổi chuỗi. Vì mỗi vị trí có 4 lựa chọn và độ dài tối đa là 20, điều này dẫn đến 4^20 khả năng trong trường hợp xấu nhất, con số này quá lớn. Ngay cả khi chúng ta giảm phân nhánh, việc khám phá tất cả các phép gán đầy đủ là không cần thiết vì điều kiện chỉ phụ thuộc vào chuỗi con có độ dài 4. 

Quan sát quan trọng là điều kiện chỉ phụ thuộc vào việc chuỗi cuối cùng có chứa mẫu 4 ký tự cụ thể hay tránh mẫu 4 ký tự khác. Điều này làm giảm cấu trúc đáng kể. Thay vì suy nghĩ về các sửa đổi tùy ý của toàn bộ chuỗi, chúng ta có thể xem xét vị trí “2026” tiềm năng có thể xuất hiện và cần bao nhiêu chỉnh sửa để thực thi nó, đồng thời tính toán riêng số lượng chỉnh sửa cần thiết để loại bỏ tất cả các lần xuất hiện “2025”. 

Trong trường hợp đầu tiên, chúng tôi sửa vị trí bắt đầu i và cố gắng chuyển đổi chuỗi con s[i:i+4] thành “2026”. Chi phí chỉ đơn giản là số lượng ký tự không khớp. Chúng tôi lấy mức tối thiểu trên tất cả các vị trí bắt đầu hợp lệ. 

Đối với trường hợp thứ hai, chúng tôi muốn đảm bảo rằng không có chuỗi con nào bằng “2025”. Thay vì theo dõi các ràng buộc toàn cục, chúng ta lại trượt một cửa sổ có độ dài 4. Đối với mỗi lần xuất hiện của mẫu “2025”, chúng ta phải thay đổi ít nhất một ký tự trong cửa sổ đó. Một thay đổi duy nhất có thể phá vỡ nhiều lần xuất hiện chồng chéo, vì vậy chúng tôi tính toán số lượng vị trí tối thiểu cần sửa đổi sao cho mọi cửa sổ “2025” đều bị vô hiệu. Vì n nhiều nhất là 20 nên chúng ta có thể thử tất cả các tập hợp con của các vị trí hoặc sử dụng phép liệt kê mặt nạ bit trên các vị trí để kiểm tra phạm vi bao phủ.

Quan điểm kép này là điều khiến vấn đề trở nên dễ giải quyết: chúng tôi trả tiền để tạo ra một “2026” hoặc chúng tôi trả tiền để hủy bỏ tất cả các lần xuất hiện “2025” và chúng tôi chấp nhận mức tối thiểu của hai chi phí này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên tất cả các sửa đổi | O(4^n · n) | O(n) | Quá chậm | 
| Liệt kê vị trí/cửa sổ | O(n · 2^n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### 1. Cố gắng ép chuỗi con “2026” 

Với mọi chỉ số i trong đó i + 3 < n, hãy so sánh s[i], s[i+1], s[i+2], s[i+3] với “2026”. Đếm những sự không phù hợp. Giữ số lượng không khớp tối thiểu trên tất cả i. 

Điều này trực tiếp đo lường cách rẻ nhất để tạo chuỗi con hợp lệ thỏa mãn điều kiện đầu tiên. 

### 2. Thu thập tất cả các cửa sổ “2025” 

Quét tất cả các chuỗi con có độ dài 4. Với mỗi chuỗi i trong đó s[i:i+4] == “2025”, hãy ghi lại cửa sổ này dưới dạng ràng buộc mà m
