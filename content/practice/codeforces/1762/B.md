---
title: "CF 1762B - Làm mảng tốt"
description: "Chúng ta được yêu cầu chuyển đổi một mảng các số nguyên dương thành một mảng \"tốt\" theo tính chất chia hết: đối với mỗi cặp phần tử, số lớn hơn phải chia hết cho số nhỏ hơn."
date: "2026-06-09T13:47:39+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "implementation", "number-theory", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1762
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 838 (Div. 2)"
rating: 1100
weight: 1762
solve_time_s: 184
verified: false
draft: false
---

[CF 1762B - Làm cho mảng trở nên tốt](https://codeforces.com/problemset/problem/1762/B) 

**Đánh giá:** 1100 
**Tags:** thuật toán xây dựng, cách thực hiện, lý thuyết số, sắp xếp 
**Thời gian giải:** 3m 4s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu chuyển đổi một mảng các số nguyên dương thành một mảng "tốt" theo tính chất chia hết: đối với mỗi cặp phần tử, số lớn hơn phải chia hết cho số nhỏ hơn. Các hoạt động được phép là bổ sung và chúng tôi có thể tăng bất kỳ phần tử nào lên bất kỳ số lượng nào lên đến giá trị hiện tại của nó. Đầu ra là một chuỗi các thao tác đảm bảo mảng kết quả là tốt và chúng ta không bắt buộc phải giảm thiểu số lượng thao tác. 

Ràng buộc đầu vào của bài toán rất lớn:$n$có thể đạt được$10^5$cho mỗi trường hợp thử nghiệm và tổng của$n$trên tất cả các trường hợp thử nghiệm cũng bị giới hạn bởi$10^5$. Điều này chỉ ra rằng thuật toán của chúng tôi về cơ bản phải tuyến tính trong$n$. Các vòng lặp lồng nhau hoặc kiểm tra toàn diện khả năng phân chia giữa mỗi cặp sẽ ngay lập tức quá chậm. Hơn nữa, vì mỗi phần tử có thể được tăng lên với số lượng rất lớn (lên tới$10^{18}$), chúng ta có thể dựa vào các phương pháp tiếp cận mang tính xây dựng hơn là lo lắng về việc chạm trần. 

Các trường hợp cạnh bao gồm các mảng vốn đã tốt, các mảng trong đó tất cả các phần tử đều giống nhau và các mảng trong đó các phần tử là nguyên tố cùng nhau theo cặp. Ví dụ, nếu$a = [2,3,5]$, một cách tiếp cận ngây thơ có thể không thể cộng các số một cách xây dựng sao cho tất cả các điều kiện chia hết được giữ đồng thời. Đầu ra chính xác trong những trường hợp như vậy thường yêu cầu tăng các phần tử một cách có chọn lọc để khớp với bội số chung nhỏ nhất hoặc cơ sở chung. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng kiểm tra từng mức tăng có thể có trên mỗi phần tử và kiểm tra mảng kết quả để tìm thuộc tính tốt. Điều này đúng về mặt khái niệm vì về nguyên tắc, chúng ta có thể tìm thấy một chuỗi các thao tác hoạt động. Tuy nhiên, số lượng các chuỗi có thể là rất lớn và việc lặp đi lặp lại tất cả các cặp phần tử sẽ cho kết quả$O(n^2)$sự phức tạp, quá chậm$n \sim 10^5$. 

Cách tiếp cận tối ưu dựa vào việc quan sát rằng một mảng "tốt" được đảm bảo nếu mọi phần tử là bội số của phần tử tối thiểu. Do đó, chúng ta có thể sửa phần tử nhỏ nhất làm tham chiếu và điều chỉnh tất cả các phần tử khác thành bội số của nó. Vì phép toán cho phép chúng ta tăng các phần tử một cách tùy ý nên việc làm cho mỗi phần tử bằng giá trị hiện tại của nó được làm tròn lên bội số gần nhất của giá trị tối thiểu là đủ. Cách tiếp cận này đảm bảo rằng mọi cặp đều thỏa mãn tính chất chia hết. Không có phần tử nào cần giảm và mọi điều chỉnh có thể được tính toán theo thời gian không đổi cho mỗi phần tử. 

Việc xây dựng rất đơn giản và tuyến tính: chúng tôi tìm mức tối thiểu, sau đó lặp qua tất cả các phần tử, tính toán mức tăng cộng cần thiết để đạt bội số của mức tối thiểu. Điều này tạo ra một giải pháp tối đa$n$hoạt động cho mỗi trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết
