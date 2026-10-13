---
title: "CF 105160M - \u8521\u5149\u6570\u7ec4"
description: "Chúng ta được cấp một mảng gồm chính xác bốn số nguyên dương, mỗi số từ 1 đến 9. Nhiệm vụ là quyết định xem mảng này có khớp với mẫu ẩn được xác định bởi chuỗi “USST” hay không, trong đó các ký tự giống hệt nhau trong chuỗi thực thi các ràng buộc đẳng thức giữa các vị trí tương ứng trong…"
date: "2026-06-27T11:03:07+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105160
codeforces_index: "M"
codeforces_contest_name: "2024 University of Shanghai for Science and Technology(USST) Freshman Challenge Contest"
rating: 0
weight: 105160
solve_time_s: 28
verified: false
draft: false
---

[CF 105160M - \u8521\u5149\u6570\u7ec4](https://codeforces.com/problemset/problem/105160/M) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 28s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một mảng gồm chính xác bốn số nguyên dương, mỗi số từ 1 đến 9. Nhiệm vụ là quyết định xem mảng này có khớp với mẫu ẩn được xác định bởi chuỗi “USST” hay không, trong đó các ký tự giống hệt nhau trong chuỗi thực thi các ràng buộc đẳng thức giữa các vị trí tương ứng trong mảng và các ký tự khác nhau thực thi các ràng buộc bất đẳng thức. 

Nói một cách đơn giản hơn, mỗi vị trí trong chuỗi gắn nhãn một chỉ mục mảng. Nếu hai vị trí trong chuỗi có cùng một chữ cái thì giá trị mảng tương ứng của chúng phải bằng nhau. Nếu hai vị trí có các chữ cái khác nhau thì giá trị của chúng phải khác nhau. Chuỗi cho vấn đề này được cố định là “USST”, do đó cấu trúc ràng buộc cũng được cố định. 

Chuỗi “USST” ngụ ý bốn vị trí có mối quan hệ sau. Vị trí thứ nhất và thứ hai tương ứng với các chữ cái khác nhau nên chúng phải khác nhau. Vị trí thứ hai, thứ ba và thứ tư tương ứng với các chữ cái S, S và T, vì vậy vị trí 2 và 3 phải bằng nhau, vị trí 3 và 4 phải khác nhau, đồng thời vị trí 2 và 4 cũng phải khác nhau. Kết hợp những điều này, chúng ta có được một hệ thống nhỏ gồm các đẳng thức và bất đẳng thức trên bốn phần tử mảng. 

Vì kích thước mảng không đổi nên không có vấn đề về tỷ lệ tiệm cận. Bất kỳ giải pháp nào thực hiện số lượng kiểm tra liên tục là đủ. Ràng buộc 1 ₫ ai ₫ 9 chỉ đảm bảo các giá trị là số nguyên nhỏ nhưng không làm thay đổi bản chất của logic. 

Kiểu thất bại chính trong lối suy nghĩ ngây thơ là chỉ kiểm tra các đẳng thức ngụ ý bằng các chữ cái lặp đi lặp lại mà quên đi các ràng buộc bất đẳng thức cần thiết. Ví dụ: coi “USST” chỉ là “a2 = a3” sẽ chấp nhận sai các mảng như`[1, 2, 2, 2]`, mặc dù vị trí 2 bằng vị trí 4 vi phạm thực tế rằng S và T là các ký tự khác nhau. 

Một lỗi phổ biến khác là chỉ kiểm tra tính duy nhất theo cặp hoặc chỉ kiểm tra các ràng buộc một phần. Ví dụ: việc từ chối các mảng trong đó tất cả các giá trị đều khác biệt sẽ là sai vì cấu hình hợp lệ yêu cầu một số giá trị bằng nhau nhưng không có sự khác biệt hoàn toàn. 

## Phương pháp tiếp cận 

Một cách diễn giải thô bạo sẽ coi mỗi vị trí thuộc về một lớp nhãn và sau đó kiểm tra tất cả các cặp chỉ mục: nếu các ký tự của chúng khớp nhau, hãy thực thi sự bình đẳng; mặt khác thực thi sự bất bình đẳng. Nói chung, đối với các chuỗi dài hơn, điều này trở thành một vấn đề ràng buộc về biểu đồ trong đó chúng tôi xác minh tính nhất quán trên tất cả các cặp. Đối với bài toán này, chỉ với bốn vị trí và một mẫu cố định, điều này giảm xuống còn việc kiểm tra một số điều kiện không đổi. 

Nếu chúng tôi liệt kê rõ ràng tất cả các ràng buộc cặp, chúng tôi sẽ kiểm tra tối đa 6 cặp: (1,2), (1,3), (1,4), (2,3), (2,4), (3,4) và xác minh xem mỗi cặp có phải bằng nhau hay không dựa trên mẫu USST. Đây đã là O(1), vì vậy vũ lực ở đây là tối ưu một cách hiệu quả. 

Quan sát chính là chúng ta không cần bất kỳ sự trừu tượng nào như ánh xạ các ký tự tới ID hoặc xây dựng cấu trúc dữ liệu. Cấu trúc cố định nên chúng ta có thể mã hóa trực tiếp các ràng buộc mà mẫu ngụ ý và xác minh chúng. 

Cách tiếp cận vũ phu có hiệu quả vì quy mô vấn đề là không đổi, nhưng nó trở nên nặng nề về mặt khái niệm nếu được khái quát hóa. Việc kiểm tra ràng buộc trực tiếp làm giảm mọi thứ xuống còn một vài so sánh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Kiểm tra cặp Brute Force | O(1) | O(1) | Đã chấp nhận | 
| Đánh giá ràng buộc trực tiếp | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Mẫu USST tương ứng với bốn vị trí: 

Vị trí 1: U 

Vị trí 2: S 

Vị trí 3: S 

Vị trí 4: T 

Từ đó ta rút ra được các ràng buộc: 

1. Vị trí 2 và 3 phải bằng nhau vì cả hai đều là S. 
2. Vị trí 1 và 2 phải khác nhau vì U và S khác nhau. 
3. Vị trí 1 và 3 phải khác nhau vì cùng một lý do. 
4. Vị trí 2 và 4 phải khác nhau vì S và T khác nhau. 
5. Vị trí 3 và 4 phải khác nhau vì S và T khác nhau. 

Chúng ta chỉ cần xác minh các điều kiện này trên mảng đã cho. 

Thuật toán tiến hành như sau. 

1. Đọc bốn số nguyên thành mảng a. 
2. Kiểm tra xem a[1] có bằng a[2] hay không. Nếu không thì bác bỏ ngay vì S phải lặp lại. 
3. Kiểm tra xem a[0] có khác a[1] hay không. Nếu không thì từ chối
