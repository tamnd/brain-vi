---
title: "CF 104664C - Bữa tiệc của thợ làm mũ"
description: "Chúng tôi được cung cấp một bộ sưu tập các sợi mì, mỗi sợi mang một giá trị hương vị bằng số. Chúng ta được phép chia các sợi này thành nhiều món ăn, trong đó mỗi món ăn phải chứa ít nhất các sợi $K$."
date: "2026-06-29T12:00:50+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104664
codeforces_index: "C"
codeforces_contest_name: "UTPC Contest 10-06-23 Div. 2 (Beginner)"
rating: 0
weight: 104664
solve_time_s: 33
verified: false
draft: false
---

[CF 104664C - Bữa tiệc của thợ làm mũ](https://codeforces.com/problemset/problem/104664/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 33s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một bộ sưu tập các sợi mì, mỗi sợi mang một giá trị hương vị bằng số. Chúng ta được phép chia những sợi này thành nhiều món ăn, trong đó mỗi món ăn phải chứa ít nhất$K$sợi. Sự đóng góp hương vị của một món ăn chỉ được xác định bởi giá trị hương vị cao nhất trong số các sợi được gán cho nó. Nhiệm vụ là sắp xếp tất cả các chuỗi thành các món ăn hợp lệ (hoặc có thể chỉ để lại một số món không được sử dụng nếu điều đó cải thiện câu trả lời, mặc dù tính tối ưu sẽ cho thấy điều này không bao giờ có ích) để tổng hương vị món ăn được tối đa hóa. 

Điểm cấu trúc quan trọng là mọi chuỗi đều bị bỏ qua hoặc đóng góp như một phần của nhóm có giá trị được xác định đầy đủ bởi phần tử tối đa của nó. Điều này ngay lập tức gợi ý rằng chỉ những phần tử lớn nhất mới đóng vai trò quan trọng với tư cách là những phần tử tiềm năng đóng góp vào tổng cuối cùng, trong khi những phần tử nhỏ hơn chủ yếu đóng vai trò là “phần bổ sung” để đáp ứng ràng buộc về kích thước tối thiểu. 

Kích thước đầu vào tăng lên$N = 10^5$, loại trừ mọi cách tiếp cận xem xét tất cả các tập hợp con hoặc phân vùng một cách rõ ràng. Ngay cả hành vi bậc hai, chẳng hạn như thử tất cả các ranh giới nhóm có thể, cũng quá chậm. Chúng ta cần thứ gì đó gần gũi hơn$O(N \log N)$hoặc$O(N)$, với việc sắp xếp được chấp nhận. 

Một vấn đề tế nhị nảy sinh khi suy nghĩ tham lam: người ta có thể cho rằng một cách sai lầm rằng việc thành lập nhóm một cách tùy tiện hoặc tham lam lấy cực đại cục bộ có thể thất bại tùy thuộc vào cấu trúc nhóm. Thách thức thực sự là quyết định làm thế nào để đảm bảo rằng các giá trị lớn được sử dụng một cách tối ưu làm cực đại của món ăn, trong khi vẫn tôn trọng giới hạn kích thước nhóm tối thiểu. 

Các trường hợp cạnh đáng chú ý bao gồm các tình huống trong đó$K = 1$, trong đó mọi phần tử có thể tạo thành món ăn riêng của mình và câu trả lời chỉ đơn giản là tổng của tất cả các giá trị và các trường hợp tất cả các giá trị đều bằng nhau, trong đó chiến lược nhóm không quan trọng nhưng vẫn phải tôn trọng logic phân vùng. Một trường hợp quan trọng khác là khi tồn tại một giá trị rất lớn giữa nhiều giá trị nhỏ; chúng ta phải đảm bảo nó không bị “lãng phí” trong một nhóm dưới mức tối ưu. 

## Phương pháp tiếp cận 

Chiến lược brute-force sẽ thử mọi cách có thể để phân chia mảng thành các nhóm có kích thước ít nhất là$K$. Đối với mỗi phân vùng, chúng tôi tính tổng cực đại của mỗi nhóm. Số lượng phân vùng tăng theo cấp số nhân với$N$, vì mọi vị trí đều có khả năng bắt đầu hoặc mở rộng một nhóm. Ngay cả khi chúng tôi giới hạn bản thân trong các nhóm liền kề, chúng tôi vẫn phải đối mặt với sự bùng nổ tổ hợp của các phân đoạn, khiến phương pháp này không thể thực hiện được ngoài những đầu vào rất nhỏ. 

Quan sát quan trọng là chỉ những yếu tố lớn nhất trong mỗi nhóm mới đóng góp vào điểm số và mỗi nhóm phải chứa ít nhất$K$các phần tử. Điều này gợi ý rằng chúng tôi muốn chỉ định mỗi “người đóng góp tối đa” được chọn làm đại diện cho một nhóm và sau đó “trả tiền” cho nhóm đó bằng cách sử dụng$K-1$các phần tử bổ sung không đóng góp vào tổng. Để tối đa hóa tổng, chúng tôi muốn các giá trị lớn đóng vai trò là cực đại nhóm thường xuyên nhất có thể, đồng thời đảm bảo rằng mỗi mức tối đa đã chọn được hỗ trợ bởi đủ phần tử nhỏ hơn hoặc không được sử dụng. 

Khi mảng được sắp xếp theo thứ tự giảm dần, cấu trúc sẽ trở nên rõ ràng hơn. Nếu chúng ta lấy các phần tử từ lớn nhất đến nhỏ nhất, mỗi lần chúng ta chọn một phần tử làm nhóm tối đa, chúng ta phải bỏ qua$K$các vị trí theo thứ tự được sắp xếp, bởi vì những vị trí đó$K$các phần tử có thể tạo thành một nhóm hợp lệ có giá trị tối đa là nhóm đầu tiên. Điều này tự nhiên dẫn đến sự lựa chọn tham lam trên các khối có kích thước$K$. 

Brute-force không thành công vì nó phân chia rõ ràng các phân vùng, trong khi giải pháp tối ưu nhận ra rằng sau khi sắp xếp, quyết định có ý nghĩa duy nhất là làm thế nào để chia danh sách đã sắp xếp thành các khối có kích thước$K$, mỗi đóng góp chính xác một mức tối đa. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(N) | Quá chậm | 
| Tối ưu | O(N log N) | O(1) hoặc O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp tất cả các giá trị hương vị theo thứ tự giảm dần. 

Điều này đảm bảo rằng trong bất kỳ nhóm nào, phần tử đầu tiên chúng ta gặp phải là ứng cử viên lớn nhất có thể cho mức tối đa của nhóm đó. 
2. Khởi tạo tổng hiện có về 0. Điều này sẽ lưu trữ tổng hương vị đóng góp của tất cả các món ăn. 
3. Lặp lại mảng đã được sắp xếp theo các bước có kích thước (
