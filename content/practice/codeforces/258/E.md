---
title: "CF 258E - Chú Voi Nhỏ Và Cây"
description: "Chúng ta có một cây có gốc với n nút, trong đó nút 1 là gốc. Mỗi nút chứa một danh sách các số bắt đầu trống. Sau đó chúng ta thực hiện m thao tác."
date: "2026-06-04T17:24:20+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dfs-and-similar", "trees"]
categories: ["algorithms"]
codeforces_contest: 258
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 157 (Div. 1)"
rating: 2400
weight: 258
solve_time_s: 64
verified: false
draft: false
---

[CF 258E - Chú voi nhỏ và cái cây](https://codeforces.com/problemset/problem/258/E) 

**Đánh giá:** 2400 
**Thẻ:** cấu trúc dữ liệu, dfs và tương tự, cây 
**Thời gian giải:** 1m 4s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một cây có rễ với`n`các nút, trong đó nút 1 là gốc. Mỗi nút chứa một danh sách các số bắt đầu trống. Sau đó chúng tôi thực hiện`m`hoạt động. Mỗi hoạt động bao gồm hai nút,`a_i`Và`b_i`, và thao tác thêm chỉ mục của thao tác`i`tới mọi nút trong cây con có gốc tại`a_i`và tới mọi nút trong cây con có gốc tại`b_i`. Sau tất cả các thao tác, chúng ta cần tính toán cho từng nút`i`một con số`c_i`, đếm xem có bao nhiêu nút khác chia sẻ ít nhất một số trong danh sách của chúng với nút`i`. 

Các ràng buộc rất lớn:`n`Và`m`có thể lên tới 100.000. Một cách tiếp cận đơn giản lưu trữ rõ ràng tất cả các danh sách sẽ yêu cầu bộ nhớ và thời gian O(n*m), có thể đạt tới 10^10 thao tác và rõ ràng là không khả thi. Điều này buộc chúng tôi phải xem xét các cách hiệu quả hơn để theo dõi sự chồng chéo giữa các cây con. 

Trường hợp phức tạp xảy ra khi nhiều thao tác nhắm vào các cây con chồng chéo. Ví dụ, nếu`a_1`Và`b_1`là cha và con, thì cả hai cây con đều chứa cùng một nút và danh sách của nút đó nhận được các số trùng lặp. Việc triển khai bất cẩn cố gắng lặp lại tất cả các nút cho mỗi thao tác hoặc đếm các phần chồng chéo một cách ngây thơ có thể đếm gấp đôi hoặc bỏ lỡ số lượng trong những trường hợp như vậy. Một trường hợp cạnh khác là khi`n = 1`hoặc`m = 1`-cây là tối thiểu và thuật toán phải xử lý chính xác cây con chỉ có gốc. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ là mô phỏng từng hoạt động bằng cách cập nhật rõ ràng danh sách trong tất cả các nút bị ảnh hưởng. Sau tất cả các thao tác, chúng tôi sẽ so sánh danh sách của từng nút với mọi nút khác để đếm các phần tử được chia sẻ. Phương pháp này đúng vì nó giải quyết vấn đề theo đúng nghĩa đen, nhưng nó có thời gian O(n*m + n^2), vượt quá giới hạn khả thi đối với`n, m`lên đến 10^5. Mỗi thao tác có thể chạm vào hầu hết tất cả các nút trong trường hợp xấu nhất và việc kiểm tra sự chồng chéo cho mỗi cặp là bậc hai. 

Quan sát quan trọng cho một giải pháp tối ưu là chúng ta không cần danh sách chính xác ở mỗi nút. Chúng tôi chỉ cần số lượng nút chia sẻ ít nhất một số. Mỗi thao tác thêm một số vào tất cả các nút trong cây con, do đó kết quả cho nút`i`là sự kết hợp của các tập hợp các nút bị ảnh hưởng bởi bất kỳ hoạt động nào chạm vào`i`. Về mặt tập hợp, hai nút chia sẻ một số nếu có ít nhất một thao tác trong đó cả hai nút đều thuộc cây con của`a_i`hoặc`b_i`. 

Cấu trúc bài toán này cho phép chúng ta rút gọn nó thành tính toán, đối với mỗi nút, có bao nhiêu nút bị ảnh hưởng bởi các hoạt động tương tự như nút đó. Chúng ta có thể biểu diễn từng thao tác dưới dạng phép cộng cây con bằng cách sử dụng chuyến tham quan Euler cây hoặc đánh số DFS và tính toán hiệu quả số lượng bằng cách sử dụng các nguyên tắc loại trừ bao gồm
