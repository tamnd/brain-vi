---
title: "CF 105172I - Nanami và cánh đồng hoa hướng dương vàng rực"
description: "Chúng ta được cung cấp một lưới $n lần n$ trong đó một số ô đã bị chiếm giữ. Mỗi ô bị chiếm đóng hoạt động giống như một nút trong biểu đồ lưới và các cạnh tồn tại giữa các ô liền kề trực giao."
date: "2026-06-27T08:25:23+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105172
codeforces_index: "I"
codeforces_contest_name: "The 20th Southeast University Programming Contest (Summer)"
rating: 0
weight: 105172
solve_time_s: 36
verified: false
draft: false
---

[CF 105172I - Nanami và cánh đồng hoa hướng dương vàng rực](https://codeforces.com/problemset/problem/105172/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 36s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một$n \times n$lưới nơi một số ô đã bị chiếm dụng. Mỗi ô bị chiếm đóng hoạt động giống như một nút trong biểu đồ lưới và các cạnh tồn tại giữa các ô liền kề trực giao. Nhiệm vụ của chúng ta là thêm các ô đã chiếm mới vào các vị trí trống để tất cả các ô đã chiếm, cả ô gốc và ô được thêm vào, tạo thành một thành phần được kết nối duy nhất theo chuyển động 4 hướng. 

Có một ràng buộc cứng rắn về số lượng ô chúng ta được phép thêm: nhiều nhất$\lfloor n^2 / 2 \rfloor$. Chúng tôi không thể di chuyển hoặc xóa bất kỳ ô nào bị chiếm giữ ban đầu, vì vậy tập hợp cuối cùng phải chứa chúng một cách nghiêm ngặt. 

Đầu ra chỉ đơn giản là một lưới khác có cùng kích thước, đánh dấu ô nào bị chiếm sau khi thêm. Mọi cấu hình hợp lệ đều được chấp nhận miễn là tất cả các ô được chiếm giữ ban đầu vẫn còn, các ô được chiếm giữ cuối cùng được kết nối và số lượng ô được thêm vào không vượt quá giới hạn. 

Kích thước lưới tối đa là$50 \times 50$, do đó có tối đa 2500 ô cho mỗi trường hợp thử nghiệm. Điều này đủ nhỏ để chúng ta có thể cung cấp các thuật toán có hành vi bậc hai cho mỗi trường hợp thử nghiệm, nhưng không phải bất kỳ thứ gì liên quan đến tìm kiếm theo cấp số nhân trên các tập hợp con của ô. 

Ý nghĩa cấu trúc quan trọng của ràng buộc là thành phần được kết nối cuối cùng không thể vượt quá khoảng một nửa lưới. Vì chúng ta có thể thêm tối đa một nửa số ô nên số lượng ô cuối cùng được chiếm nhiều nhất là$\lceil n^2 / 2 \rceil$. Điều này có nghĩa là cấu hình ban đầu luôn đủ thưa để có thể kết nối trong một “ngân sách” giới hạn gồm các ô được thêm vào, ngay cả khi nó trông có vẻ rải rác. 

Các trường hợp thất bại chính của lý luận ngây thơ đến từ các giả định về khả năng kết nối. 

Nếu chúng ta cố gắng kết nối các thành phần một cách tham lam mà không xem xét đến sự chồng chéo đường dẫn, chúng ta có thể đếm quá mức và vượt quá số lượng bổ sung được phép. Ví dụ: nếu mỗi ô ban đầu bị cô lập và chúng ta kết nối chúng từng ô một bằng các đường đi ngắn nhất, thì chúng ta có thể đi qua nhiều lần các khu vực đã được sử dụng theo nhiều cách khác nhau, xây dựng lại một cách hiệu quả các phần lớn của lưới nhiều lần. 

Một trường hợp tế nhị khác là khi xuất xứ
