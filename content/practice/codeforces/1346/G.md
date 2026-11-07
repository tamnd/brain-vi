---
title: "CF 1346G - Hai Camera IP"
description: "Chúng tôi được cung cấp hai camera IP giống hệt nhau, mỗi camera có khả năng chụp ảnh trong một khoảng thời gian cố định. Chu kỳ của mỗi camera phải được chọn từ một tập hợp số nguyên được xác định trước, nhưng thời điểm bắt đầu của mỗi camera rất linh hoạt."
date: "2026-06-11T14:53:06+07:00"
tags: ["codeforces", "competitive-programming", "*special", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1346
codeforces_index: "G"
codeforces_contest_name: "Kotlin Heroes: Episode 4"
rating: 2300
weight: 1346
solve_time_s: 80
verified: false
draft: false
---

[CF 1346G - Hai camera IP](https://codeforces.com/problemset/problem/1346/G) 

**Đánh giá:** 2300 
**Tags:** *đặc biệt, toán, lý thuyết số 
**Thời gian giải:** 1 phút 20s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp hai camera IP giống hệt nhau, mỗi camera có khả năng chụp ảnh trong một khoảng thời gian cố định. Chu kỳ của mỗi camera phải được chọn từ một tập hợp số nguyên được xác định trước, nhưng thời điểm bắt đầu của mỗi camera rất linh hoạt. Mục tiêu là bao gồm một loạt các khoảnh khắc đáng quan tâm, đảm bảo rằng trong mỗi khoảnh khắc, có ít nhất một camera ghi lại được khoảnh khắc đó. Đầu vào bao gồm danh sách các khoảng thời gian có sẵn và các thời điểm quan tâm. Đầu ra yêu cầu cấu hình cho hai camera đáp ứng mọi khoảnh khắc hoặc tuyên bố rằng điều đó là không thể. 

Hạn chế chính đó là`n`có thể đạt được`10^5`và khoảnh khắc có thể lên tới`10^6`. Điều này loại trừ những cách tiếp cận ngây thơ lặp đi lặp lại theo từng thời điểm trong từng khoảng thời gian, bởi vì điều đó có thể yêu cầu`10^11`hoạt động trong trường hợp xấu nhất. Chúng ta cần một cách tiếp cận có quy mô tuyến tính hoặc gần tuyến tính với`n`Và`k`. Một sự tinh tế khác là các khoảnh khắc và thời kỳ đang tăng lên một cách nghiêm ngặt. Việc triển khai bất cẩn có thể cho rằng phạm vi phủ sóng tham lam đơn giản có hiệu quả, nhưng do hai camera tương tác với nhau nên việc phân phối khoảnh khắc nhất định có thể khiến phạm vi phủ sóng ngây thơ không thành công. Ví dụ: nếu các khoảnh khắc xen kẽ theo cách mà không một khoảng thời gian nào có thể bao gồm một tập hợp con liên tiếp, thì việc quét một camera ngây thơ sẽ cho rằng không thể thực hiện được. 

Một trường hợp cạnh bê tông là những khoảnh khắc`[1, 4, 5, 7, 12]`và thời kỳ`[3, 5, 7]`. Lời giải phải nhận ra rằng một camera có chu kỳ 3 bao phủ`[1,4,7,10...]`trong khi chiếc máy ảnh thứ hai có che chu kỳ 7`[5,12...]`. Cách tiếp cận một camera tham lam có thể thử 3 hoặc 5 camera trước và sau đó không thể bao quát được tất cả các khoảnh khắc bằng khoảng thời gian còn lại nếu triển khai không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force sẽ thử từng cặp thời kỳ`(cp1, cp2)`từ danh sách và, với mỗi danh sách, hãy thử tất cả các thời điểm bắt đầu có thể`s1`Và`s2`có thể bao trùm mọi khoảnh khắc. Về nguyên tắc thì điều này đúng, nhưng số lượng kết hợp thì`k^2 * n^2`trong trường hợp xấu nhất, đạt đến`10^15`hoạt động và hoàn toàn không khả thi. 

Quan sát quan trọng là những khoảnh khắc quan tâm được sắp xếp. Điều này có nghĩa là kiểu phủ sóng của mỗi camera là định kỳ và có thể được mô tả như một cấp số cộng đơn giản. Thay vì kiểm tra tất cả các thời điểm bắt đầu, bạn chỉ cần thử bắt đầu tại thời điểm đầu tiên của đoạn chưa được khám phá. Đối với mỗi giai đoạn, người ta có thể đánh dấu tất cả các khoảnh khắc mà nó sẽ bao gồm bắt đầu từ lần bắt đầu ứng cử viên và sau đó xem những khoảnh khắc còn lại phải được camera thứ hai xử lý. Cái nhìn sâu sắc là một giải pháp hợp lệ tồn tại nếu chúng ta có thể phân chia các khoảnh khắc thành hai cấp số cộng với các khoảng thời gian từ tập hợp được phép, bắt đầu từ khoảnh khắc chưa được khám phá đầu tiên của mỗi phân đoạn. Điều này làm giảm vấn đề kiểm tra tất cả các cặp dấu chấm`O(k^2)`với quét tuyến tính trên`n`khoảnh khắc, mang lại`O(k^2 * n)`thời gian. Tối ưu hóa hơn nữa khai thác rằng chỉ cần hai thời điểm bắt đầu ứng cử viên cho mỗi giai đoạn (thời điểm đầu tiên hoặc thời điểm cuối cùng vẫn cho phép bao phủ) để giảm việc quét. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(k^2 * n^2) | O(n) | Quá chậm | 
| Quét thời gian được tối ưu hóa | O(k^2 * n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lặp lại tất cả các cặp dấu chấm`(cp1, cp2)`từ danh sách thời gian được phép. Mỗi cặp đại diện cho một tổ hợp ứng cử viên
