---
title: "CF 1365G - Mật khẩu an toàn"
description: "Chúng ta được cung cấp một mảng các số nguyên không âm chưa xác định, mỗi số có phạm vi tối đa 64 bit. Chúng ta không thể nhìn thấy mảng trực tiếp. Thay vào đó, chúng ta có thể truy vấn bất kỳ tập hợp con nào của các chỉ mục và thẩm phán trả về OR theo bit của các phần tử tại các vị trí đó."
date: "2026-06-16T11:50:19+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "combinatorics", "constructive-algorithms", "interactive", "math"]
categories: ["algorithms"]
codeforces_contest: 1365
codeforces_index: "G"
codeforces_contest_name: "Codeforces Round 648 (Div. 2)"
rating: 2800
weight: 1365
solve_time_s: 123
verified: false
draft: false
---

[CF 1365G - Mật khẩu an toàn](https://codeforces.com/problemset/problem/1365/G) 

**Đánh giá:** 2800 
**Tags:** bitmasks, tổ hợp, thuật toán xây dựng, tương tác, toán học 
**Thời gian giải:** 2m 3s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên không âm chưa xác định, mỗi số có phạm vi tối đa 64 bit. Chúng ta không thể nhìn thấy mảng trực tiếp. Thay vào đó, chúng ta có thể truy vấn bất kỳ tập hợp con nào của các chỉ mục và thẩm phán trả về OR theo bit của các phần tử tại các vị trí đó. 

Từ mảng ẩn này, một mảng thứ hai được xác định. Đối với mỗi vị trí i, giá trị đầu ra là bit OR của tất cả các phần tử ngoại trừ phần tử thứ i. Vì vậy, mỗi mục đầu ra là OR của một mảng gần như hoàn chỉnh, thiếu chính xác một phần tử. 

Nhiệm vụ của chúng ta là xây dựng lại toàn bộ mảng đầu ra này bằng cách sử dụng tối đa 13 tập con OR truy vấn. 

Quan sát quan trọng là chúng tôi không cố gắng khôi phục mảng ban đầu. Với mỗi chỉ mục i, chúng ta chỉ cần OR của “tất cả ngoại trừ i”. Điều đó làm thay đổi hoàn toàn bài toán, vì OR là lũy đẳng và mất thông tin, nên việc xây dựng lại A nói chung là không thể, nhưng việc xây dựng lại các phần bù này là khả thi. 

Ràng buộc n 1000 chỉ với 13 truy vấn ngay lập tức loại trừ mọi chiến lược truy vấn trên mỗi chỉ mục. Một cách tiếp cận đơn giản sẽ truy vấn n lần hoặc thậm chí n log n lần, vượt xa ngân sách. Điều này gợi ý mạnh mẽ một chiến lược bitwise toàn cầu trong đó mỗi truy vấn trích xuất thông tin cho nhiều chỉ mục cùng một lúc. 

Trường hợp cạnh tinh tế phát sinh khi nhiều phần tử chia sẻ bit. Nếu chúng ta giả sử rằng chúng ta có thể cô lập các phần tử một cách rõ ràng, thì chúng ta có thể cố gắng xây dựng lại từng A[i], nhưng OR không phân tách một cách duy nhất. Ví dụ: A = [1, 2, 3] và A = [3, 0, 3] có thể tạo ra hành vi OR giống hệt nhau cho nhiều tập hợp con. Vì vậy, bất kỳ cách tiếp cận nào dựa vào việc khôi phục các giá trị riêng lẻ về cơ bản đều không an toàn. Đại lượng ổn định duy nhất là OR đầy đủ trên tất cả các phần tử. 

## Phương pháp tiếp cận 

Một ý tưởng trực tiếp nhưng không chính xác là cố gắng xây dựng lại từng A[i] bằng cách so sánh tổng OR có và không có i. Nếu chúng ta có thể tính tổng OR của tất cả các phần tử thì mỗi câu trả lời sẽ chỉ đơn giản là OR đó, trừ khi loại bỏ i loại bỏ một số bit duy nhất. Khó khăn là chúng ta không biết chỉ mục nào đóng góp các bit duy nhất mà không cách ly chúng và việc cách ly đòi hỏi quá nhiều truy vấn. 

Cái nhìn sâu sắc về cấu trúc quan trọng là khai thác tính độc lập của bit. Vì OR hoạt động trên mỗi bit nên mỗi bit hoạt động độc lập trên các chỉ số. Một bit đóng góp vào P[i] khi và chỉ khi nó tồn tại trong ít nhất một A[j] với j ≠ i. Vì vậy, đối với một bit cố định, trường hợp có vấn đề duy nhất đối với vị trí i là khi bit đó chỉ xuất hiện trong A[i]. Điều này biến vấn đề thành việc xác định, đối với mỗi chỉ mục, liệu nó có phải là sóng mang duy nhất của một bit hay không. 

Chúng ta có thể khai thác điều này bằng cách sử dụng một kỹ thuật cổ điển: truy vấn phân vùng và xây dựng lại thông qua các đóng góp bổ sung. Ý tưởng cốt lõi là tính toán OR của tất cả các phần tử, sau đó với mỗi chỉ mục, hãy xác định xem việc xóa nó có làm thay đổi OR toàn cục hay không. Nếu việc loại bỏ i không làm thay đổi tổng OR thì A[i] không đóng góp các bit duy nhất. Nếu việc loại bỏ i làm thay đổi nó thì A[i] chứa ít nhất một bit dành riêng cho nó. 

Để tính toán tất cả P[i] một cách hiệu quả, chúng tôi quan sát một công thức mạnh hơn. Đặt T là OR của toàn bộ mảng. Sau đó: 

P[i] = T nếu A[i] không chứa bất kỳ bit nào là duy nhất trong toàn bộ mảng ngoại trừ i, nếu không thì P[i] vẫn là T vì việc loại bỏ phần tử không duy nhất không ảnh hưởng đến OR. Tình huống duy nhất mà P[i] khác với T là khi A[i] là nguồn duy nhất của một số bit; trong trường hợp đó việc loại bỏ nó sẽ làm giảm OR. 

Vì vậy, chúng ta cần phát hiện xem nó xuất hiện bao nhiêu lần đối với mỗi bit. Thay vì đếm chính xác, chúng ta chỉ cần biết, với mỗi i, liệu A[i] có phải là phần giữ duy nhất của một bit nào đó hay không.

Chúng tôi đạt được điều này bằng cách sử dụng cấu trúc phân tách bit tiêu chuẩn: biểu diễn các chỉ mục ở dạng nhị phân sử dụng tối đa 10 bit (kể từ n 1000) và thực hiện các truy vấn nhóm OR trên các tập hợp con được xác định bởi vị trí bit. Mỗi truy vấn thu thập OR trên một phân vùng được chọn cẩn thận để mỗi chỉ mục được loại trừ chính xác khỏi một nhóm truy vấn được kiểm soát. Bằng cách kết hợp các kết quả, chúng ta có thể xây dựng lại OR của tất cả các phần tử ngoại trừ chính nó cho mỗi chỉ mục. 

Việc xây dựng
