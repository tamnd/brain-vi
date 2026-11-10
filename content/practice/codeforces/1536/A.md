---
title: "CF 1536A - Omkar và Truyện Xấu"
description: "Chúng ta được cấp một tập nhỏ các số nguyên phân biệt và được phép thêm các số nguyên phân biệt mới vào đó. Mục tiêu là xây dựng một tập hợp cuối cùng sao cho nó thỏa mãn thuộc tính đóng rất mạnh: bất cứ khi nào chúng ta chọn bất kỳ hai số nào từ tập hợp đó, hiệu tuyệt đối giữa chúng cũng phải…"
date: "2026-06-14T18:44:48+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "constructive-algorithms"]
categories: ["algorithms"]
codeforces_contest: 1536
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 724 (Div. 2)"
rating: 800
weight: 1536
solve_time_s: 102
verified: false
draft: false
---

[CF 1536A - Omkar và câu chuyện tồi tệ](https://codeforces.com/problemset/problem/1536/A) 

**Đánh giá:** 800 
**Tags:** vũ lực, thuật toán mang tính xây dựng 
**Thời gian giải:** 1 phút 42s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một tập nhỏ các số nguyên phân biệt và được phép thêm các số nguyên phân biệt mới vào đó. Mục tiêu là xây dựng một tập hợp cuối cùng sao cho nó thỏa mãn một thuộc tính đóng rất mạnh: bất cứ khi nào chúng ta chọn bất kỳ hai số nào từ tập hợp, thì hiệu tuyệt đối giữa chúng cũng phải có sẵn trong cùng một tập hợp. 

Yêu cầu này buộc bộ này phải có cấu trúc cực kỳ chặt chẽ. Nếu hai giá trị cách xa nhau thì khoảng cách của chúng phải tồn tại bên trong tập hợp dưới dạng một phần tử. Giá trị mới được giới thiệu đó sau đó sẽ tương tác lại với các phần tử khác, có khả năng tạo ra nhiều khoảng trống hơn, v.v. Việc xây dựng không mang tính cục bộ, bởi vì mỗi cặp tương tác với mọi cặp khác thông qua sự khác biệt. 

Mỗi trường hợp thử nghiệm hỏi liệu có thể hoàn thành tập hợp đã cho thành một tập hợp "đóng sai phân" như vậy trong khi vẫn giữ tổng kích thước tối đa là 300 hay không và nếu vậy, chúng ta phải xây dựng rõ ràng một hoàn thành hợp lệ. 

Các ràng buộc rất khiêm tốn: ban đầu có nhiều nhất 50 trường hợp thử nghiệm và các giá trị được giới hạn trong một phạm vi nhỏ. Tuy nhiên, giới hạn đầu ra là 300 phần tử là rất quan trọng. Nó cho chúng ta biết rằng bất kỳ giải pháp nào dựa vào việc đóng theo cấp số nhân hoặc mở rộng đóng không hạn chế sẽ thất bại. Chúng ta cần một cấu trúc nơi việc đóng cửa ổn định nhanh chóng. 

Một mối lo ngại ngây thơ là việc liên tục thêm những khác biệt còn thiếu có thể bùng nổ. Ví dụ: bắt đầu từ một tập hợp nhỏ không đều như`{0, 4, 10}`sẽ tạo ra`{4, 6, 10}`sau đó`{2, 4, 6, 8, 10}`vân vân, có vẻ như nó có thể tiếp tục vô tận hoặc phát triển quá lớn. Khó khăn chính là kiểm soát liệu quá trình này có ổn định trong phạm vi 300 phần tử hay không. 

Trường hợp tinh tế thứ hai là khi đầu vào đã chứa nhiều thang đo số học được trộn lẫn với nhau, chẳng hạn như`{1, 4, 6, 10}`. Một số cặp tạo ra những khác biệt không phù hợp với bất kỳ cấu trúc hiện có nào và việc cố gắng sửa một cặp có thể phá vỡ một cặp khác trừ khi toàn bộ tập hợp được nhúng vào một mẫu nhất quán. 

## Phương pháp tiếp cận 

Quan điểm bạo lực là liên tục thực thi điều kiện. Chúng tôi lấy tập hợp hiện tại, tính toán tất cả các khác biệt tuyệt đối theo cặp và chèn bất kỳ sai lệch tuyệt đối nào còn thiếu. Chúng tôi tiếp tục cho đến khi không có phần tử mới nào xuất hiện hoặc kích thước vượt quá 300. Về mặt khái niệm, điều này đơn giản vì nó trực tiếp thực thi định nghĩa về một mảng đẹp. 

Vấn đề là mỗi lần lặp lại liên quan đến việc kiểm tra tất cả các cặp, tức là O(k²) và mỗi lần chèn sẽ thay đổi cấu trúc, có thể yêu cầu nhiều lần lặp lại. Trong trường hợp xấu nhất, việc đóng có thể hoạt động giống như một phép mở rộng thuật toán Euclide trên nhiều số, tạo ra một tập hợp trung gian lớn trước khi ổn định hoặc vượt quá giới hạn. Điều này làm cho quá trình không thể đoán trước và quá chậm để có thể dựa trực tiếp vào. 

Quan sát quan trọng là một tập hợp hợp lệ phải hoạt động giống như một mạng được tạo bởi các phần tử của nó khi đóng phép trừ. Cách duy nhất để tránh sự tăng trưởng không kiểm soát được là tất cả các số đều nằm trong một cấp số cộng duy nhất. Nếu tập hợp có dạng`{x, x+d, x+2d, ..., x+kd}`, thì mọi hiệu đều là bội số của`d`và việc đóng được thỏa mãn miễn là tất cả các bội số trung gian đều có mặt. Điều này ngay lập tức gợi ý rằng tập hợp đó thực sự phải là một phân đoạn liền kề trong một số hệ tọa độ được chia tỷ lệ. 

Do đó, bài toán rút gọn thành việc tìm xem liệu các số đã cho có thể được nhúng vào một cấp số cộng mà bước chia tất cả các hiệu theo từng cặp hay không. Ứng cử viên đương nhiên là xem xét ước số chung lớn nhất của tất cả các khác biệt theo cặp. Nếu chúng ta tính toán`g = gcd(|a_i - a_j|)`, thì tất cả các số hợp lệ phải nằm trên một lưới bậc`g`. Một khi chúng ta bình thường hóa bằng cách chia mọi thứ cho`g`, vấn đề trở thành việc kiểm tra xem liệu chúng ta có thể mở rộng tập hợp thành một đoạn liên tiếp đầy đủ hay không. 

Tại thời điểm này, việc xây dựng trở nên đơn giản: chúng ta cố gắng lấp đầy toàn bộ khoảng từ`min(a)`ĐẾN`max(a)`với bước`g`. Tập hợp đó được đảm bảo thỏa mãn thuộc tính đóng vì mọi hiệu là bội số của`g`và mọi bội số như vậy xuất hiện dưới dạng một phần tử trong khoảng. 

Sự cản trở duy nhất xảy ra khi việc hoàn thành được yêu cầu vượt quá giới hạn 300 phần tử. Vì kích thước khoảng là`(max - min)/g + 1`, chúng ta chỉ thành công khi con số này ≤ 300. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mở rộng đóng cửa Brute Force | O(k³) trường hợp xấu nhất | O(k) | Quá chậm/không thể đoán trước | 
| GCD + hoàn thành số học | O(n² log A) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc mảng và tính giá trị tối thiểu và tối đa của nó. Hai giá trị này xác định khoảng thời gian nhỏ nhất có thể chứa bất kỳ sự hoàn thành hợp lệ nào. Bất kỳ giải pháp nào ít nhất phải bao gồm các điểm cuối này vì chúng đã là một phần của đầu vào. 
2. Tính ước chung lớn nhất`g`của tất cả sự khác biệt theo cặp`|a_i - a_1|`. Điều này ghi lại kích thước bước nhỏ nhất nhất quán trên toàn bộ tập hợp. Lý do neo đậu tại`a_1`là tất cả sự khác biệt đều tương đương với bản dịch. 
3. Nếu`g`bằng 0, nghĩa là tất cả các giá trị đều giống hệt nhau, tập hợp này rất đẹp và không cần mở rộng. Chúng tôi chỉ đơn giản là xuất ra cùng một phần tử. 
4. Xây dựng tập ứng viên bằng cách sinh ra tất cả các giá trị từ`min(a)`ĐẾN`max(a)`bước qua`g`. Điều này đảm bảo chúng ta lấp đầy mọi điểm mạng trung gian có thể được yêu cầu bởi điều kiện đóng. 
5. Kiểm tra xem kích thước tập hợp được xây dựng có vượt quá 300 hay không. Nếu có, không có cấu trúc hợp lệ nào tồn tại trong các ràng buộc, vì vậy chúng tôi xuất NO. 
6. Nếu không thì xuất CÓ theo sau là trình tự đã xây dựng. 

### Tại sao nó hoạt động 

Điều kiện đóng buộc bất kỳ tập hợp lệ nào phải ổn định khi lấy hiệu, nghĩa là tất cả các phần tử phải nằm trong một nhóm con cộng duy nhất của các số nguyên được tạo bởi kích thước bước cố định`g`. Khi cấu trúc đó được xác định, cách duy nhất để đảm bảo đóng là đưa mọi điểm có thể tiếp cận vào khoảng giữa giá trị tối thiểu và tối đa. Bất kỳ điểm nào bị thiếu sẽ tạo ra sự khác biệt không có trong tập hợp, vi phạm điều kiện. Vì vậy việc xây dựng vừa cần thiết vừa đủ trong phạm vi quy mô cho phép. 

## Giải pháp Python
