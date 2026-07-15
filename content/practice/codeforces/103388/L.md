---
title: "CF 103388L - Danh sách mật khẩu"
description: "Cây nhị thức $Tn$ được sử dụng trong phần này có các nút $2^n$, mỗi nút tương ứng với một chuỗi nhị phân có độ dài $n$, và $Tinfty$ là cấu trúc giới hạn trong đó các nút tương ứng với tất cả các chuỗi nhị phân hữu hạn thu được bằng cách loại bỏ các số 0 đứng đầu."
date: "2026-07-03T18:19:20+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103388
codeforces_index: "L"
codeforces_contest_name: "2021-2022 ACM-ICPC Brazil Subregional Programming Contest"
rating: 0
weight: 103388
solve_time_s: 85
verified: false
draft: false
---

[CF 103388L - Liệt kê mật khẩu](https://codeforces.com/problemset/problem/103388/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 25s 
**Đã xác minh:** không 

## Giải pháp 
## Giải pháp 

Cây nhị thức$T_n$được sử dụng trong phần này có$2^n$các nút, mỗi nút tương ứng với một chuỗi nhị phân có độ dài$n$, Và$T_\infty$là cấu trúc giới hạn trong đó các nút tương ứng với tất cả các chuỗi nhị phân hữu hạn thu được bằng cách loại bỏ các số 0 đứng đầu. Mỗi nút được xác định bằng chuỗi bit của nó và cấu trúc cây được xác định sao cho việc mở rộng chuỗi bằng cách nối thêm các bit tương ứng với việc di chuyển xuống cây. 

Tuyên bố đặt hàng trước trên$T_\infty$trùng với ký hiệu nhị phân tăng dần có nghĩa là thứ tự trước liệt kê các nút theo cùng thứ tự với việc sắp xếp nhãn nhị phân của chúng dưới dạng số nguyên được viết trong cơ sở$2$(với các số 0 đứng đầu bị loại bỏ). Do đó, thứ tự trước là phần mở rộng tuyến tính của thứ tự tự nhiên trên các chuỗi nhị phân này được hiểu là số. 

Cho phép$P$biểu thị trình tự đặt hàng trước và$Q$thứ tự sau của$T_\infty$. Cây là cây nhị phân đầy đủ trong đó mỗi nút có một nút con trái thu được bằng cách nối thêm$0$và một đứa trẻ đúng có được bằng cách nối thêm$1$. Do đó, việc duyệt theo thứ tự sau sẽ truy cập toàn bộ cây con bên trái trước cây con bên phải và chỉ sau đó mới truy cập vào gốc. 

Đối với bất kỳ độ sâu hữu hạn cố định$n$, cái cây$T_n$là một cây nhị phân hoàn chỉnh có chiều cao$n$các nút của nó chính xác là các chuỗi nhị phân có độ dài tối đa$n$, được nhúng trong một cấu trúc thống nhất từ ​​trái sang phải. Trong một cây như vậy, việc duyệt thứ tự sau có dạng đệ quy tiêu chuẩn: danh sách thứ tự sau của cây có được bằng cách nối danh sách thứ tự sau của cây con bên trái, sau đó là danh sách thứ tự sau của cây con bên phải và cuối cùng là gốc. Vượt tới giới hạn$n \to \infty$duy trì cấu trúc đệ quy này cho mọi tiền tố hữu hạn của các nút, vì mọi nút hữu hạn đều nằm trong một cây con hữu hạn. 

Việc liệt kê thứ tự trước tương ứng với việc tăng giá trị nhị phân. Trong cây nhị phân đầy đủ được sắp xếp bằng cách nối thêm$0$bên trái và$1$ở bên phải, điều này ngụ ý rằng một nút có chuỗi nhị phân$b_k \dots b_1$được truy cập trước tất cả các nút có giá trị nhị phân lớn hơn, điều này buộc phải sắp xếp trước các nút theo thứ tự từ điển với$0 < 1$, và do đó làm tăng ký hiệu nhị phân. 

Thứ tự sau đảo ngược mức độ ưu tiên về cấu trúc: đối với bất kỳ tiền tố cố định nào, tất cả các nút trong cây con có gốc tại một nút đều được truy cập trước chính nút đó. Trong cây nhị phân đầy đủ, điều này gây ra sự đảo ngược cấu trúc đệ quy xây dựng chuỗi nhị phân từ phần mở rộng ít quan trọng nhất. Cụ thể, nếu thứ tự trước tương ứng với việc diễn giải chuỗi nhị phân dưới dạng số được xây dựng từ cấu trúc quan trọng nhất trở ra ngoài, thì thứ tự sau tương ứng với việc hoàn thành tất cả các phần mở rộng trước khi ghi tiền tố, điều này đảo ngược thứ tự xây dựng bit hiệu quả. 

Sự đảo ngược này biểu hiện trên toàn cầu dưới dạng đảo ngược của việc mở rộng nhị phân khi quá trình truyền tải được biểu thị dưới dạng một chuỗi vô hạn duy nhất được lập chỉ mục theo thứ tự trước. Nếu chỉ số thứ tự trước của một nút được xác định bởi chuỗi nhị phân của nó$x_1 x_2 \dots x_m$, thì chỉ số thứ tự sau có được bằng cách đọc cùng một đường dẫn cấu trúc theo thứ tự hoàn thành ngược lại, tương ứng với việc đảo ngược chuỗi các quyết định tạo ra nút từ gốc. Do đó, nhãn thứ tự sau là sự đảo ngược bit của nhãn thứ tự trước. 

Nút thứ một triệu trong thứ tự trước được đưa ra là$$11110100001000111111.$$Đảo ngược chuỗi này sẽ cho nhãn postorder. 

Viết chuỗi và đảo ngược chữ số bằng chữ số mang lại kết quả$$11110100001000111111 \;\mapsto\; 11111100010000101111.$$Do đó nút thứ một triệu trong thứ tự sau là$$\boxed{11111100010000101111}.$$Điều này hoàn thành giải pháp. ∎
