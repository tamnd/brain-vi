---
title: "CF 293E - Đóng đỉnh"
description: "Cho một cây có n đỉnh, trong đó mỗi cạnh mang trọng số không âm. Đối với hai đỉnh bất kỳ, khoảng cách giữa chúng có thể được đo bằng hai cách: số cạnh dọc theo đường đi nối chúng và tổng trọng số của các cạnh dọc theo đường đi đó."
date: "2026-06-05T17:23:30+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "divide-and-conquer", "trees"]
categories: ["algorithms"]
codeforces_contest: 293
codeforces_index: "E"
codeforces_contest_name: "Croc Champ 2013 - Round 2"
rating: 2700
weight: 293
solve_time_s: 41
verified: false
draft: false
---

[CF 293E - Đóng các đỉnh](https://codeforces.com/problemset/problem/293/E) 

**Đánh giá:** 2700 
**Tas:** cấu trúc dữ liệu, phân chia để chinh phục, cây cối 
**Thời gian giải:** 41s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cây có _n_ đỉnh, trong đó mỗi cạnh mang một trọng số không âm. Đối với hai đỉnh bất kỳ, khoảng cách giữa chúng có thể được đo bằng hai cách: số cạnh dọc theo đường đi nối chúng và tổng trọng số của các cạnh dọc theo đường đi đó. Hai đỉnh được coi là _close_ nếu tồn tại một đường đi nối chúng sao cho đường đi đó chứa nhiều nhất _l_ cạnh và tổng trọng số không vượt quá _w_. Nhiệm vụ là đếm tất cả các cặp đỉnh phân biệt không có thứ tự thỏa mãn tiêu chí gần nhau này. 

Cấu trúc đầu vào tăng dần: mỗi cạnh kết nối đỉnh _i+1_ với một số đỉnh _pᵢ_ trước đó, do đó, về cơ bản, cây có gốc ở đỉnh 1. Cây có tối đa 10^5 đỉnh và trọng số cạnh có thể đạt tới 10^4. Giới hạn độ dài đường dẫn _l_ cũng có thể đạt tới 10^5, trong khi giới hạn trọng lượng _w_ có thể lớn tới 10^9. 

Với kích thước _n_, bất kỳ thuật toán nào kiểm tra tất cả các cặp một cách rõ ràng sẽ quá chậm. Lực lượng vũ phu sẽ liên quan đến các so sánh O(n²), tức là khoảng 10^10 thao tác trong trường hợp xấu nhất, vượt xa khả thi. Điều này báo hiệu rằng chúng ta cần một cách tiếp cận phức tạp hơn nhằm tận dụng cấu trúc cây để tránh các phép tính dư thừa. 

Các trường hợp biên phát sinh khi giới hạn _l_ hoặc _w_ rất nhỏ, có khả năng loại trừ hầu hết các cặp hoặc rất lớn, đếm hiệu quả gần như tất cả các cặp. Một cách tiếp cận ngây thơ không cắt bỏ những con đường không khả thi có thể vừa lãng phí thời gian vừa tạo ra kết quả sai lầm. Ví dụ: một cây có ba đỉnh có cạnh có trọng số là 10 và giới hạn _l_ = 1, _w_ = 5, sẽ không tạo ra cặp gần nhau nào. Một DFS bất cẩn đếm tất cả các nút lân cận sẽ tạo ra số đếm khác 0 không chính xác. 

## Phương pháp tiếp cận 

Giải pháp brute-force sẽ xem xét từng cặp đỉnh, tính toán độ dài và trọng lượng đường đi bằng cách sử dụng BFS hoặc DFS và tăng bộ đếm nếu cả hai điều kiện đều được thỏa mãn. Mặc dù đúng nhưng cách tiếp cận này là O(n2) về cả thời gian và có khả năng là O(n) về không gian để theo dõi đường dẫn. Rõ ràng, nó thất bại ở giới hạn trên của 10^5 đỉnh của vấn đề. 

Thông tin chi tiết quan trọng về tối ưu hóa là chúng ta đang xử lý một cái cây. Cây không có chu kỳ, điều này cho phép các phương pháp phân chia và chinh phục hoạt động hiệu quả. Một phương pháp hiệu quả là phân rã trung tâm. Bằng cách chọn đệ quy một centroid, chúng ta có thể chia cây thành các cây con nhỏ hơn và đếm các cặp gần nhau có đường đi qua centroid. Đối với mỗi cây con, chúng tôi thực hiện một DFS bị ràng buộc để thu thập khoảng cách và trọng số, sau đó kết hợp các kết quả với cấu trúc dữ liệu hiệu quả như Cây chỉ mục nhị phân (Cây Fenwick) hoặc nhiều tập hợp để đếm xem có bao nhiêu đường dẫn thỏa mãn cả hai ràng buộc. 

Cách tiếp cận này khai thác thực tế là mọi đường đi đều đi qua trọng tâm hoặc nằm hoàn toàn trong một cây con. Việc xử lý các đường đi qua tâm một cách riêng biệt cho phép chúng ta hợp nhất các cây con mà không cần tính hai lần và tránh việc kiểm tra cặp dư thừa. Việc phân rã đảm bảo rằng mỗi đỉnh có liên quan đến mức độ trung tâm O(log n), tạo ra độ phức tạp tổng thể là O(n log n). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Phân hủy Centroid + Cây Fenwick | O(n log n) | | |
