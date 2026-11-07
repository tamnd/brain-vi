---
title: "CF 1312G - Tự động hoàn thành"
description: "Chúng ta được cung cấp một cấu trúc gốc gồm nhiều chuỗi trong đó mỗi nút đại diện cho một chuỗi và mỗi cạnh tương ứng với việc nối thêm một ký tự. Nút gốc là chuỗi rỗng và mỗi nút i được tạo bằng cách lấy chuỗi cha pi của nó và thêm một chữ cái viết thường ci."
date: "2026-06-11T17:13:53+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dfs-and-similar", "dp"]
categories: ["algorithms"]
codeforces_contest: 1312
codeforces_index: "G"
codeforces_contest_name: "Educational Codeforces Round 83 (Rated for Div. 2)"
rating: 2600
weight: 1312
solve_time_s: 51
verified: false
draft: false
---

[CF 1312G - Tự động hoàn thành](https://codeforces.com/problemset/problem/1312/G) 

**Đánh giá:** 2600 
**Thẻ:** cấu trúc dữ liệu, dfs và tương tự, dp 
**Thời gian giải:** 51s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một cấu trúc gốc gồm nhiều chuỗi trong đó mỗi nút đại diện cho một chuỗi và mỗi cạnh tương ứng với việc nối thêm một ký tự. Nút gốc là chuỗi rỗng và mỗi nút i được tạo bằng cách lấy chuỗi cha p_i của nó và nối thêm một chữ cái viết thường c_i. Điều này xác định một cây trong đó mỗi nút tương ứng với một chuỗi duy nhất. 

Từ cây này, một tập hợp con các nút được đánh dấu là “chuỗi đích”. Đối với mỗi chuỗi mục tiêu, chúng ta phải tính thời gian tối thiểu để xây dựng chuỗi đó bắt đầu từ chuỗi trống bằng hai loại thao tác. Một thao tác sẽ thêm một ký tự duy nhất, luôn tốn một giây, tương ứng với việc đi xuống một cạnh của cây. Thao tác còn lại là bước tự động hoàn thành: từ chuỗi t hiện tại, chúng ta xem xét tất cả các chuỗi trong tập hợp S có t làm tiền tố, sắp xếp chúng theo từ điển và chúng ta có thể chuyển tới bất kỳ chuỗi nào trong số đó theo thời gian bằng với thứ hạng của nó trong danh sách được sắp xếp đó. 

Khó khăn chính là việc tự động hoàn thành phụ thuộc vào thứ tự từ điển chỉ trong số các chuỗi mục tiêu hợp lệ dưới tiền tố, trong khi các thao tác chắp thêm phụ thuộc vào cấu trúc trie đầy đủ. 

Các ràng buộc lên tới n = 10^6, do đó, mọi giải pháp về cơ bản đều phải tuyến tính hoặc gần tuyến tính. Một cách tiếp cận bậc hai tính toán lại các bộ tiền tố hoặc sắp xếp trên mỗi nút là không thể ngay lập tức, vì ngay cả O(n log n) được lặp lại trên mỗi nút cũng sẽ vượt quá giới hạn. Điều này đẩy chúng ta tới một tập hợp dựa trên cây DP hoặc DFS, trong đó mỗi nút được xử lý một lần và thông tin được hợp nhất một cách cẩn thận. 

Một trường hợp thất bại tinh tế xuất phát từ việc hiểu nhầm phạm vi tự động hoàn thành. Việc xếp hạng không nằm trong số tất cả các nút con trong cây mà chỉ giữa các nút nằm trong S và có chung tiền tố. Nếu bao gồm tất cả các nút không chính xác hoặc bỏ qua thứ tự từ điển, chi phí tính toán sẽ trở nên không nhất quán. 

## Phương pháp tiếp cận 

Một mô phỏng trực tiếp sẽ cố gắng tính toán, đối với mỗi chuỗi mục tiêu, tất cả các tiền tố hợp lệ và tại mỗi tiền tố sẽ tính toán danh sách từ điển của các chuỗi phù hợp. Điều này nhanh chóng trở nên không khả thi: một nút có thể có O(n) con cháu và việc sắp xếp hoặc duy trì các tập hợp có thứ tự trên mỗi nút sẽ dẫn đến hành vi O(n^2) hoặc O(n log n) trên mỗi nút trong trường hợp xấu nhất. 

Quan sát quan trọng là cấu trúc từ điển của các chuỗi trong một trie có thể được xử lý bằng cách xử lý các nút con theo thứ tự ký tự và duy trì thứ tự DFS của các nút mục tiêu. Thay vì tính toán lại các bộ tiền tố nhiều lần, chúng tôi tính toán cho mỗi nút có bao nhiêu nút mục tiêu tồn tại trong cây con của nó và duy trì thứ tự duyệt nhất quán chỉ trên các nút có liên quan. 

Chúng tôi giải thích vấn đề
