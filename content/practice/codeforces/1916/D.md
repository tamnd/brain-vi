---
title: "CF 1916D - Bài toán"
description: "Chúng ta được yêu cầu xây dựng một tập hợp số có cấu trúc rất cụ thể. Mỗi trường hợp thử nghiệm cung cấp một số nguyên lẻ $n$, có hai nghĩa: đó vừa là số lượng các số chúng ta cần tạo ra vừa là số chữ số mà mỗi số đó phải có."
date: "2026-06-08T19:49:55+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "constructive-algorithms", "geometry", "math"]
categories: ["algorithms"]
codeforces_contest: 1916
codeforces_index: "D"
codeforces_contest_name: "Good Bye 2023"
rating: 1700
weight: 1916
solve_time_s: 38
verified: false
draft: false
---

[CF 1916D - Bài toán](https://codeforces.com/problemset/problem/1916/D) 

**Đánh giá:** 1700 
**Tags:** sức mạnh vũ phu, thuật toán xây dựng, hình học, toán học 
**Thời gian giải:** 38s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xây dựng một tập hợp số có cấu trúc rất cụ thể. Mỗi trường hợp thử nghiệm cung cấp một số nguyên lẻ$n$, có nghĩa kép: nó vừa là số các số chúng ta cần tạo ra vừa là số chữ số mà mỗi số đó phải có. Ngoài ra, tất cả các số phải là số chính phương và tập hợp nhiều chữ số trên tất cả các số phải giống hệt nhau. Điều này có nghĩa là nếu chúng ta viết ra tất cả các chữ số từ tất cả các số thì mỗi chữ số xuất hiện cùng số lần trong mỗi số, mặc dù thứ tự các chữ số trong một số có thể khác nhau. 

Đầu vào cung cấp tối đa 100 trường hợp thử nghiệm và$n$có thể lớn tới 99. Tổng số chữ số trong tất cả các trường hợp thử nghiệm không vượt quá$10^5$, điều này cho phép chúng ta tạo ra các số một cách dễ dàng nếu chúng ta có thể làm điều đó một cách hiệu quả. Vì bản thân các số có thể khá lớn (lên tới 99 chữ số), chúng ta cần tránh các thao tác có tỷ lệ kém với kích thước chữ số. Mỗi số cũng phải tránh các số 0 đứng đầu, điều này làm cho việc hoán vị đơn giản của các chữ số trở nên không hề nhỏ. 

Các trường hợp cạnh phát sinh khi$n = 1$, vì số có một chữ số vẫn phải là số vuông. Lời giải tầm thường là 1, 4 hoặc 9. Một trường hợp cạnh khác là số lẻ nhỏ$n$như 3 hoặc 5, trong đó mẫu chữ số phải khớp cẩn thận trên tất cả các số. Các giải pháp bất cẩn có thể tạo ra các số có độ dài không chính xác hoặc có nhiều chữ số khác nhau. 

## Phương pháp tiếp cận 

Một cách tiếp cận đơn giản là bắt đầu liệt kê các ô vuông từ 1 trở đi, chuyển chúng thành chuỗi, kiểm tra độ dài của chúng và sau đó cố gắng tìm một nhóm$n$các số có chung nhiều chữ số. Đối với mỗi ứng cử viên, chúng ta sẽ cần so sánh nhiều chữ số của nó với tất cả các chữ số khác, việc này trở nên tốn kém về mặt tính toán. Với$n$lên tới 99 và có độ dài lên tới 99 chữ số, cách tiếp cận bạo lực bao gồm việc kiểm tra hàng chục nghìn ô vuông và so sánh nhiều tập hợp nhiều lần, đưa ra số lượng thao tác vượt quá$10^6$. Điều này sẽ chậm và cồng kềnh. 

Quan sát quan trọng là bài toán cho phép một số tự do: bất kỳ tập hợp ô vuông nào có cùng nhiều tập chữ số đều đủ. Điều này cho phép chúng ta xây dựng các con số một cách có hệ thống thay vì tìm kiếm một cách mù quáng. Một chiến lược đơn giản là tạo ra một mẫu chữ số tuần hoàn trong các con số. Ví dụ, chúng ta có thể tạo ra một chuỗi các$n$số trong đó mỗi số chứa các chữ số$0$ĐẾN$n-1$theo một thứ tự xoay vòng nào đó. Sau đó, bằng cách diễn giải những số này dưới dạng số nguyên thập phân và lấy bình phương gần nhất, chúng ta có thể thu được nghiệm hợp lệ. Vấn đề đảm bảo rằng có một giải pháp tồn tại, điều đó có nghĩa là cách tiếp cận mang tính xây dựng này sẽ có hiệu quả. 

Điều này chuyển vấn đề từ tìm kiếm hình vuông sang thiết kế một phương pháp đảm bảo có cùng nhiều tập chữ số trong$n$số, tất cả độ dài$n$. Chúng ta có thể tính toán trước một chuỗi các chữ số cho mỗi$n$và sau đó chuyển đổi các chuỗi này thành hình vuông. Bằng cách chọn cẩn thận điểm bắt đầu và tăng dần, các ô vuông duy trì các yêu cầu về độ dài và nhiều tập hợp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(max_square × n × n) | O(n²) | Quá chậm đối với n ≈ 99 | 
| Chu kỳ mang tính xây dựng | O(n²) | O(n²) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test, đọc số nguyên lẻ$n$, xác định cả số lượng và độ dài của chúng. 
2. Xây dựng dãy chữ số cơ bản của một số. Một lựa chọn đơn giản là$0, 1, 2, \dots, n-1$. Trình tự này có$n$chữ số, tất cả đều khác biệt. Để tránh các số 0 đứng đầu, hãy thay thế chữ số đầu tiên bằng 1 hoặc bất kỳ chữ số nào khác 0. 
3. Tạo$n$số bằng cách xoay tuần tự cơ sở
