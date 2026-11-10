---
title: "CF 1523C - Nén và mở rộng"
description: "Chúng ta được cấp một chuỗi số nguyên, mỗi số đại diện cho số cuối cùng của một mục danh sách lồng nhau sau khi William vô tình xóa mọi thứ khác. Mục tiêu là xây dựng lại một danh sách lồng nhau hợp lệ có thể tạo ra chuỗi này."
date: "2026-06-10T17:33:54+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "data-structures", "greedy", "implementation", "trees"]
categories: ["algorithms"]
codeforces_contest: 1523
codeforces_index: "C"
codeforces_contest_name: "Deltix Round, Spring 2021 (open for everyone, rated, Div. 1 + Div. 2)"
rating: 1600
weight: 1523
solve_time_s: 73
verified: false
draft: false
---

[CF 1523C - Nén và mở rộng](https://codeforces.com/problemset/problem/1523/C) 

**Đánh giá:** 1600 
**Tags:** bạo lực, cấu trúc dữ liệu, tham lam, triển khai, cây cối 
**Thời gian giải:** 1 phút 13s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một chuỗi số nguyên, mỗi số đại diện cho số cuối cùng của một mục danh sách lồng nhau sau khi William vô tình xóa mọi thứ khác. Mục tiêu là xây dựng lại một danh sách lồng nhau hợp lệ có thể tạo ra chuỗi này. Mỗi mục trong danh sách lồng nhau hợp lệ là một chuỗi các số nguyên được phân tách bằng dấu chấm, ví dụ:`1.2.3`. Một số mới có thể được thêm bằng cách mở rộng mục trước đó ở cấp độ hiện tại (tăng số cuối cùng) hoặc bằng cách bắt đầu một cấp độ sâu hơn (nối thêm`.1`). Danh sách được xây dựng lại phải duy trì thứ tự từ điển tăng dần ban đầu. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm, mỗi trường hợp đưa ra số dòng`n`và dãy số cuối cùng`a_i`. Tối đa`n`trên tất cả các trường hợp thử nghiệm có tổng bằng 1000, điều đó có nghĩa là chúng tôi có thể thoải mái mua các giải pháp O(n^2) hoặc tốt hơn. Mỗi`a_i`nhiều nhất là`n`, vì vậy chúng ta có thể theo dõi các chuỗi mà không phải lo lắng về số lượng lớn hoặc tràn. 

Trường hợp phức tạp xảy ra khi các số liên tiếp lặp lại`1`, chẳng hạn như`1 1 1`. Một cách tiếp cận ngây thơ chỉ đơn giản là thêm vào`.1`lần nào cũng sẽ thất bại nếu nó không quay trở lại cấp độ cha mẹ phù hợp một cách chính xác. Một tình huống tế nhị khác phát sinh khi trình tự nhảy về phía trước, chẳng hạn`1 2 1 2`. Chúng ta phải xác định chính xác thời điểm cần sao lưu hệ thống phân cấp để đính kèm số tiếp theo. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ là thử tất cả các danh sách lồng nhau hợp lệ có thể có và xem danh sách nào tạo ra chuỗi đã cho. Về nguyên tắc, điều này đúng, nhưng mỗi dòng có nhiều vị trí và thao tác chèn hợp lệ, dẫn đến độ phức tạp theo cấp số nhân. Với`n`lên tới 1000, số lượng kết hợp trở nên vô cùng lớn, khiến điều này không thể thực hiện được. 

Thông tin chi tiết quan trọng là cấu trúc được phân cấp hoàn toàn và chúng ta có thể duy trì đường dẫn hiện tại gồm các số đại diện cho mục cuối cùng được chèn vào. Mỗi số mới bắt đầu một danh sách con mới (`1`), tiếp tục mức hiện tại (`prev + 1`) hoặc di chuyển lên một hoặc nhiều cấp cho đến khi chúng ta có thể đặt nó. Quan sát này cho phép chúng ta xây dựng lại danh sách lồng nhau một cách tham lam chỉ trong một lần. Bằng cách duy trì "chuỗi hiện tại" và loại bỏ các phần tử khi số tiếp theo không tiếp nối số cuối cùng, chúng ta có thể xây dựng lại chuỗi một cách chính xác. 

Phương pháp tham lam hoạt động vì chuỗi đã cho đảm bảo tồn tại ít nhất một danh sách hợp lệ và các ràng buộc về cách tăng số lượng cũng như cách thức tăng`.1`trình tự bắt đầu thực thi một vị trí duy nhất để nối thêm mỗi số. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Tái thiết tham lam | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo danh sách trống`path`để biểu thị chuỗi số hiện tại cho mục được xây dựng gần đây nhất. 
2. Lặp qua từng số`x`trong trình tự. Nếu như`x`là`1`, nối thêm`1`đến con đường. Việc này sẽ bắt đầu một danh sách con mới ở cấp độ hiện tại hoặc cấp độ trước đó. 
3. Nếu`x`lớn hơn`1`, kiểm tra xem nó có thể mở rộng số cuối cùng trong đường dẫn hay không. Trong khi phần tử cuối cùng trong p
