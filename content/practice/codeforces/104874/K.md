---
title: "CF 104874K - Con Vua"
description: "Chúng ta có một lưới $n nhân m$ trong đó mỗi ô trống hoặc chứa chính xác một lâu đài được gắn nhãn bằng một chữ cái viết hoa. Có chính xác một lâu đài được dán nhãn 'A', thuộc về đứa trẻ yêu thích."
date: "2026-06-28T10:09:13+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104874
codeforces_index: "K"
codeforces_contest_name: "2019-2020 ICPC NERC (NEERC), North-Western Russia Regional Contest (Northern Subregionals)"
rating: 0
weight: 104874
solve_time_s: 21
verified: false
draft: false
---

[CF 104874K - Những đứa con của nhà vua](https://codeforces.com/problemset/problem/104874/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 21s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một$n \times m$lưới trong đó mỗi ô trống hoặc chứa chính xác một lâu đài được gắn nhãn bằng chữ in hoa. Có chính xác một lâu đài được dán nhãn 'A', thuộc về đứa trẻ yêu thích. Mọi lâu đài khác đều được gắn nhãn bằng một chữ cái viết hoa riêng biệt từ 'B' đến 'Z'. 

Nhiệm vụ là phân vùng toàn bộ lưới thành các vùng hình chữ nhật sao cho mỗi ô thuộc về chính xác một hình chữ nhật và mỗi hình chữ nhật chứa chính xác một lâu đài. Mỗi hình chữ nhật được gán cho chủ sở hữu của lâu đài mà nó chứa và tất cả các ô trống bên trong hình chữ nhật đó được điền bằng chữ cái viết thường của chủ sở hữu đó. 

Trong số tất cả các phân vùng hình chữ nhật hợp lệ, chúng ta phải chọn một phân vùng có diện tích tối đa hóa hình chữ nhật chứa 'A'. 

Cấu trúc này là một tấm lưới hoàn chỉnh với các hình chữ nhật thẳng hàng theo trục, mỗi hình chữ nhật được neo bởi chính xác một ô đặc biệt (một lâu đài). Điều này buộc mọi hình chữ nhật phải là một vùng tối đa có thể được gán cho một lâu đài duy nhất mà không chồng chéo lên các lâu đài khác. 

Những hạn chế$n, m \le 1000$ngụ ý lên tới một triệu tế bào. Bất kỳ giải pháp nào cố gắng liệt kê tất cả các phân vùng hình chữ nhật hoặc kiểm tra tất cả các phần mở rộng một cách độc lập sẽ quá chậm. Một giải pháp phải hoạt động trong thời gian gần tuyến tính hoặc gần tuyến tính trên lưới. 

Một điểm tinh tế quan trọng là hình chữ nhật không độc lập. Việc mở rộng một hình chữ nhật sẽ làm giảm không gian dành cho những hình chữ nhật khác, do đó việc mở rộng cục bộ tham lam mà không có cấu trúc toàn cầu có thể thất bại. 

Một trường hợp thất bại điển hình phát sinh nếu người ta cố gắng phát triển từng lâu đài một cách độc lập theo mọi hướng cho đến khi chạm vào một lâu đài khác. Điều này có thể chiếm quá nhiều không gian hoặc chặn các phân vùng hợp lệ, vì hình chữ nhật phải xếp toàn bộ lưới mà không bị chồng chéo hoặc có khoảng trống. 

Ví dụ: nếu hai lâu đài được đặt theo đường chéo, việc mở rộng đơn giản có thể cho phép cả hai phát triển thành cùng một vùng trống tùy thuộc vào thứ tự xử lý, điều này không hợp lệ vì hình chữ nhật phải phân vùng lưới. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là xem xét mọi cách có thể để gán từng ô cho một trong các lâu đài trong khi vẫn đảm bảo vùng được chỉ định của mỗi lâu đài vẫn có hình chữ nhật và được kết nối. Ngay cả khi chúng ta đơn giản hóa và nói rằng mỗi lâu đài xác định một hình chữ nhật, chúng ta vẫn cần thử tất cả các ranh giới hình chữ nhật có thể có cho mỗi lâu đài. 

Đối với mỗi lâu đài, có$O(nm)$hình chữ nhật có thể chứa nó. Với tối đa 26 lâu đài, việc khám phá tất cả các kết hợp đã trở nên rộng lớn về mặt thiên văn, theo thứ tự$(nm)^{25}$ở dạng khái niệm tồi tệ nhất. Ngay cả việc xác minh một ô xếp đầy đủ cũng sẽ yêu cầu kiểm tra sự chồng chéo và phạm vi bao phủ, bản thân điều này là$O(nm)$. Điều này làm cho vũ lực hoàn toàn không thể thực hiện được. 

Chìa khóa
