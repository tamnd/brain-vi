---
title: "CF 104797H - Radar"
description: "Chúng ta được cung cấp một hệ thống radar tạo ra một tập hợp hữu hạn các điểm được quét trên mặt phẳng. Mỗi điểm được quét được hình thành bằng cách chọn hướng và khoảng cách từ điểm gốc, sau đó đi khoảng cách đó dọc theo hướng đó."
date: "2026-06-28T13:45:20+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104797
codeforces_index: "H"
codeforces_contest_name: "2021-2022 ICPC Central Europe Regional Contest (CERC 21)"
rating: 0
weight: 104797
solve_time_s: 31
verified: false
draft: false
---

[CF 104797H - Radar](https://codeforces.com/problemset/problem/104797/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 31s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một hệ thống radar tạo ra một tập hợp hữu hạn các điểm được quét trên mặt phẳng. Mỗi điểm được quét được hình thành bằng cách chọn hướng và khoảng cách từ điểm gốc, sau đó đi khoảng cách đó dọc theo hướng đó. 

Cụ thể, đầu vào cung cấp danh sách bán kính và danh sách hướng. Mỗi hướng xuất phát từ một vectơ khác 0, nghĩa là nó xác định một tia bắt đầu từ gốc tọa độ. Đối với mọi bán kính và mọi hướng, radar sẽ tạo ra một điểm nằm chính xác tại bán kính đó dọc theo tia đó. Vậy tập quét là tất cả các điểm có dạng$r \cdot \frac{v}{\|v\|}$, Ở đâu$r$là từ danh sách bán kính và$v$là một trong các vectơ chỉ phương. 

Sau đó, chúng ta được cung cấp các điểm truy vấn trong mặt phẳng và với mỗi điểm truy vấn, chúng ta phải tính khoảng cách Euclide đến điểm được quét gần nhất. 

Các ràng buộc rất chặt chẽ: lên đến$10^5$bán kính,$10^5$chỉ đường và$10^5$truy vấn. Một cấu trúc đơn giản của tất cả các điểm được quét sẽ tạo ra tới$10^{10}$điểm, điều này hoàn toàn không thể thực hiện được. Ngay cả việc lặp lại tất cả các kết hợp cho mỗi truy vấn cũng sẽ vượt xa giới hạn thời gian. 

Đầu ra đòi hỏi độ chính xác cao nên lời giải phải tránh các lối tắt hình học không ổn định và thay vào đó dựa vào việc rút gọn đại số ổn định. 

Một vấn đề tế nhị phát sinh từ sự suy thoái hình học. Ví dụ: nếu một truy vấn nằm chính xác trên một trong các tia và khoảng cách của nó khớp với bán kính thì câu trả lời là 0. Một vấn đề khác là sự bao quanh góc: các hướng gần 0 độ và 360 độ phải được coi là liền kề khi tìm kiếm hướng gần nhất. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ tạo ra mọi điểm được quét một cách rõ ràng và đối với mỗi điểm truy vấn, hãy tính toán Euclid tối thiểu
