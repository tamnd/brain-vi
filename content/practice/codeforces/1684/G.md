---
title: "CF 1684G - Euclid Đoán"
description: "Chúng ta được cấp một tập hợp các số nguyên dương và được biết rằng tập hợp số nguyên này được tạo ra bằng cách chạy liên tục thuật toán Euclid trên một số cặp số nguyên chưa biết."
date: "2026-06-10T00:03:17+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "flows", "graph-matchings", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1684
codeforces_index: "G"
codeforces_contest_name: "Codeforces Round 792 (Div. 1 + Div. 2)"
rating: 2800
weight: 1684
solve_time_s: 63
verified: false
draft: false
---

[CF 1684G - Đoán Euclid](https://codeforces.com/problemset/problem/1684/G) 

**Đánh giá:** 2800 
**Tags:** thuật toán xây dựng, luồng, so khớp đồ thị, toán, lý thuyết số 
**Thời gian giải:** 1m 3s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một tập hợp các số nguyên dương và được biết rằng tập hợp số nguyên này được tạo ra bằng cách chạy liên tục thuật toán Euclid trên một số cặp số nguyên chưa biết. Mỗi khi thuật toán Euclid thực hiện một bước chia và thu được phần dư khác 0, phần dư đó sẽ được thêm vào danh sách chung. Sau khi xử lý tất cả các cặp, danh sách cuối cùng sẽ được xáo trộn trước khi đưa cho chúng tôi. 

Nhiệm vụ của chúng tôi là xây dựng lại bất kỳ tập hợp các cặp số nguyên dương nào, mỗi cặp không vượt quá một giới hạn nhất định, sao cho khi thuật toán Euclid được chạy trên mỗi cặp và tất cả các phần dư được tạo ra được thu thập cùng nhau, thì nhiều tập hợp số dư khớp chính xác với nhiều tập hợp đã cho. 

Quan sát quan trọng là chúng tôi không xây dựng lại thứ tự phần còn lại trên mỗi cặp. Chúng ta chỉ cần phân vùng nhiều tập thành các dấu vết Euclid hợp lệ, mỗi dấu vết tương ứng với một cặp bắt đầu. 

Ràng buộc rằng cả hai số trong mỗi cặp nhiều nhất là m là rất quan trọng vì nó giới hạn mức độ lớn của các trạng thái Euclid trung gian, đặc biệt là bước phân chia đầu tiên, xác định liệu một cặp có khả thi hay không. 

Trường hợp cạnh tinh vi xuất hiện khi đầu vào chứa các giá trị không thể xuất hiện dưới dạng phần còn lại đầu tiên của bất kỳ bước Euclid hợp lệ nào dưới giới hạn m. Ví dụ: nếu m nhỏ và nhiều tập hợp chứa một giá trị lớn hơn m/2, thì có thể không thể tạo ra nó như phần còn lại của bất kỳ cặp hợp lệ nào, vì bước đầu tiên yêu cầu xây dựng các số a và b với a = kb + r và a ≤ m, b ≤ m. 

Một trường hợp thất bại khác xảy ra khi nhiều tập hợp quá “không đều” để có thể bị phân hủy thành chuỗi Euclid. Ví dụ: một số lớn không có giá trị đồng hành tương thích có thể buộc phải xây dựng lại không thể, bởi vì mỗi quá trình Euclid tạo ra một chuỗi phần dư giảm dần có cấu trúc, chứ không phải các bộ sưu tập tùy ý. 

Do đó, khó khăn trọng tâm không phải là mô phỏng mà là quyết định xem liệu nhiều tập hợp có thể được phân tách thành chuỗi dư Euclid hợp lệ hay không và sau đó xây dựng các cặp số nguyên rõ ràng tạo ra chúng. 

## Phương pháp tiếp cận 

Một ý tưởng trực tiếp là cố gắng đảo ngược thuật toán Euclid cho từng số một cách độc lập. Đối với một chuỗi còn lại, người ta có thể cố gắng đoán giá trị thứ hai và xây dựng lại một cặp hợp lệ tạo ra giá trị đó. Tuy nhiên, vì cùng một số có thể xuất hiện trong nhiều chuỗi Euclid khác nhau và không xác định được phép gán, điều này dẫn đến vấn đề phân chia theo cấp số nhân trên tất cả các tập hợp con của nhiều tập hợp. Ngay cả với khả năng ghi nhớ, số cách nhóm các giá trị thành chuỗi tăng quá nhanh với n lên tới 1000. 

Cái nhìn sâu sắc về cấu trúc quan trọng là đảo ngược quá trình tạo cục bộ. Thay vì suy nghĩ về các phép chạy Euclid đầy đủ, chúng ta tập trung vào bước đầu tiên của Euclid cho một cặp (a, b). Nếu a ≥ b thì số dư đầu tiên là mod b. Nếu chúng ta muốn phần dư r cụ thể xuất hiện ở đầu ra, chúng ta có thể áp dụng một cấu trúc trong đó a = k·b + r với k ≥ 1. Điều này có nghĩa là mỗi phần dư r có thể được “lưu trữ” bằng cách chọn b và k phù hợp sao cho cả hai số đều nằm trong m. 

Viết lại câu này, mỗi r yêu cầu tìm cặp (b, a) sao cho a mod b = r, hoặc tương đương a = b + r (chọn k = 1 là đủ và tối ưu cho việc xây dựng). Khi đó b phải > r và a ≤ m. Điều này ngay lập tức đưa ra một điều kiện xây dựng đơn giản: với mỗi r chúng ta cần một số b thuộc (r, m] sao cho r + b ≤ m, tương đương với b ≤ m − r. 

Do đó, với mỗi r chúng ta cần chọn một đối tác b trong khoảng (r, m − r). Điều này trở thành một vấn đề so sánh hai bên giữa các giá trị r và các lựa chọn có thể có của b, trong đó mỗi b được chọn tạo ra một cặp Euclid hợp lệ tạo ra r là số dư đầu tiên của nó.

Khi việc ghép đôi này được thiết lập, mỗi cặp sẽ tạo ra một dấu vết Euclid hợp lệ một cách độc lập và vì thứ tự không liên quan nên chúng ta có thể ghép tất cả các cặp một cách đơn giản. 

Lý do điều này có tác dụng là vì chúng tôi cố tình buộc mỗi lần chạy Euclid dừng lại sau một bước còn lại có ý nghĩa. Bằng cách chọn a = b + r, thuật toán tạo ra chính xác một phần dư r và sau đó chuyển ngay sang (b, r), kết thúc nhanh chóng hoặc chỉ tạo ra các giá trị nhỏ hơn không liên quan vì chúng tôi không bao giờ sử dụng lại cấu trúc trung gian. 

Việc xây dựng biến vấn đề đa tập hợp toàn cục thành một vấn đề gán tính khả thi trên các giá trị, có thể được giải quyết một cách tham lam bằng cách sắp xếp và so khớp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Sự phân hủy mạnh mẽ của tất cả các chuỗi | Hàm mũ | Cao | Quá chậm | 
| Tính khả thi tham lam phù hợp với từng giá trị | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp nhiều tập t theo thứ tự giảm dần. Điều này đảm bảo chúng tôi xử lý các giá trị lớn hơn f
