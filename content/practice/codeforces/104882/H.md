---
title: "CF 104882H - Chúc các bạn làm bài thi vui vẻ"
description: "Chúng tôi đang giải quyết một bài kiểm tra trắc nghiệm tương tác. Bài kiểm tra bao gồm $n$ câu hỏi và mỗi câu hỏi có chính xác hai câu trả lời có thể có, trong đó có chính xác một câu trả lời đúng. Chúng tôi không biết trước câu trả lời chính xác và chúng tôi không thể truy vấn chúng trực tiếp."
date: "2026-06-28T09:19:19+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104882
codeforces_index: "H"
codeforces_contest_name: "Voronezh State University - Sitronics contest II"
rating: 0
weight: 104882
solve_time_s: 36
verified: false
draft: false
---

[CF 104882H - Chúc các bạn làm bài kiểm tra vui vẻ](https://codeforces.com/problemset/problem/104882/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 36s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang giải quyết một bài kiểm tra trắc nghiệm tương tác. Bài kiểm tra có chứa$n$câu hỏi và mỗi câu hỏi có chính xác hai câu trả lời có thể có, trong đó có chính xác một câu trả lời đúng. Chúng tôi không biết trước câu trả lời chính xác và chúng tôi không thể truy vấn chúng trực tiếp. Thay vào đó, chúng tôi trả lời từng câu hỏi một và chỉ biết điểm cuối cùng sau mỗi lần thử đầy đủ. 

Sau khi hoàn thành một lần thử, chúng ta chỉ được cho điểm dựa trên tỷ lệ câu trả lời đúng. Số điểm ít nhất 50% đã được coi là đạt yêu cầu và bất kỳ nỗ lực nào như vậy ngay lập tức kết thúc tương tác thành công. Nếu không, chúng tôi được phép thử lại, với tổng số lần thử tối đa là ba lần. 

Khó khăn chính là chúng tôi không bao giờ nhận được phản hồi theo từng câu hỏi mà chỉ nhận được kết quả tổng hợp của toàn bộ nỗ lực. Điều này có nghĩa là bất kỳ chiến lược nào cũng phải dựa hoàn toàn vào cấu trúc chứ không phải học các câu trả lời riêng lẻ. 

Các hạn chế là nhỏ:$n \le 100$. Điều này loại trừ bất cứ điều gì nặng nề về mặt tính toán cho mỗi câu hỏi, nhưng quan trọng hơn, đây không phải là vấn đề tắc nghẽn máy tính. Hạn chế là về mặt thông tin: đơn giản là chúng tôi không có đủ phản hồi để xây dựng lại đáp án một cách chi tiết. 

Một quan niệm sai lầm ngây thơ nhưng phổ biến là cho rằng chúng ta cần "học" câu trả lời đúng sau mỗi lần thử. Tuy nhiên, vì chúng tôi chỉ nhận được một con số duy nhất cho mỗi lần thử nên không có cách nào để xác định tính chính xác của từng câu hỏi. Bất kỳ cách tiếp cận nào tùy thuộc vào suy luận của mỗi câu hỏi sẽ thất bại. 

Trường hợp cạnh có ý nghĩa duy nhất cần xem xét là vị trí đối nghịch của các câu trả lời đúng. Ví dụ: nếu tất cả các câu trả lời đúng đều là "A" thì việc luôn chọn "A" sẽ thành công ngay lập tức. Ngược lại, nếu các câu trả lời đúng được phân bố đều, việc đoán mò ngây thơ có thể thất bại nhưng chúng ta vẫn phải đảm bảo thành công trong vòng ba lần thử. 

Ràng buộc tương tác rất nghiêm ngặt: bất kỳ định dạng sai hoặc vượt quá giới hạn lần thử sẽ ngay lập tức chấm dứt chương trình. Tuy nhiên, thách thức cốt lõi không phải là định dạng mà là thiết kế một chiến lược đảm bảo đạt độ chính xác ít nhất 50%. 

## Phương pháp tiếp cận 

Một cách giải thích thô bạo sẽ là cố gắng suy ra câu trả lời đúng cho mỗi câu hỏi. Người ta có thể tưởng tượng việc sử dụng các mẫu câu trả lời khác nhau qua các lần thử và so sánh điểm số để suy ra tính đúng đắn của mỗi câu hỏi. Ví dụ: lật các tập hợp con của câu trả lời giữa các lần thử và giải hệ phương trình trên các câu trả lời đúng chưa biết. 

Ý tưởng này nhanh chóng trở nên không khả thi trong thực tế. Mặc dù mỗi lần thử đều cho một kết quả vô hướng, việc xây dựng lại$n$biến nhị phân từ tối đa ba biến vô hướng
