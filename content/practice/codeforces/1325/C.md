---
title: "CF 1325C - MEX Ehab và Path-etic"
description: "Chúng ta có một cây có $n$ nút, được biểu thị bằng các cạnh $n-1$ kết nối các cặp nút. Mỗi cạnh phải được gán một nhãn số nguyên riêng biệt trong khoảng từ $0$ đến $n-2$."
date: "2026-06-11T16:40:17+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "dfs-and-similar", "greedy", "trees"]
categories: ["algorithms"]
codeforces_contest: 1325
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 628 (Div. 2)"
rating: 1500
weight: 1325
solve_time_s: 67
verified: false
draft: false
---

[CF 1325C - Ehab và MEX gây bệnh](https://codeforces.com/problemset/problem/1325/C) 

**Đánh giá:** 1500 
**Thẻ:** thuật toán xây dựng, dfs và các cây tương tự, tham lam 
**Thời gian giải:** 1 phút 7s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được tặng một cái cây với$n$các nút, được đại diện bởi$n-1$các cạnh kết nối các cặp nút. Mỗi cạnh phải được gán một nhãn số nguyên riêng biệt giữa$0$Và$n-2$. Đối với hai nút bất kỳ$u$Và$v$, MEX của đường dẫn giữa chúng được xác định là số nguyên không âm nhỏ nhất không xuất hiện trên các cạnh dọc theo đường dẫn duy nhất kết nối$u$Và$v$. Mục tiêu của chúng tôi là gắn nhãn các cạnh sao cho MEX tối đa trên tất cả các cặp nút có thể được giảm thiểu. 

Những ràng buộc chỉ ra rằng$n$có thể lên đến$10^5$, ngụ ý rằng bất kỳ giải pháp nào lặp qua tất cả các cặp nút sẽ yêu cầu theo thứ tự$10^{10}$hoạt động và do đó không thể thực hiện được. Điều này buộc chúng ta phải tìm ra giải pháp gán nhãn cạnh theo cách tận dụng cấu trúc của cây thay vì tính toán tất cả các giá trị MEX một cách rõ ràng. Các trường hợp cạnh phát sinh trong cây hình ngôi sao hoặc chuỗi tuyến tính, trong đó các phép gán đơn giản có thể dẫn đến giá trị MEX tối đa lớn, vì vậy chúng ta phải xử lý các nút cấp cao một cách cẩn thận. 

Một ví dụ nhỏ minh họa sự tinh tế. Đối với cây gồm 3 nút nối thành hình ngôi sao: các cạnh (1,2) và (1,3), gán nhãn$0$Và$1$đảm bảo MEX tối đa là$2$. Gán nhãn cả hai cạnh$0$Và$2$sẽ tăng MEX tối đa một cách không cần thiết cho đường dẫn (2,3), tạo ra kết quả lớn hơn mức cần thiết. 

## Phương pháp tiếp cận 

Một cách tiếp cận mạnh mẽ sẽ cố gắng gán nhãn cho các cạnh trong mọi hoán vị có thể và sau đó tính MEX cho tất cả$\binom{n}{2}$cặp nút. Điều này đúng về nguyên tắc nhưng có quy mô như$O((n-1)! \cdot n^2)$, điều này hoàn toàn không thực tế đối với$n = 10^5$. 

Cái nhìn sâu sắc quan trọng đến từ việc xem xét cấu trúc của cây. MEX tối đa dọc theo một đường dẫn được xác định bởi các cạnh dọc theo đường dẫn đó. Nếu chúng ta gán các nhãn liên tiếp cho các cạnh liên quan đến các nút có độ lớn hơn hai thì các đường dẫn giữa các nút có độ cao sẽ nhanh chóng tiêu thụ số lượng nhỏ nhất có sẵn. Quan sát thấy rằng ở bất kỳ cây nào mức độ tối đa$\Delta$là số cạnh liên quan đến nút được kết nối nhiều nhất, chúng ta có thể giảm thiểu MEX tối đa bằng cách gán nhãn nhỏ nhất cho các cạnh liên quan đến nút cấp cao nhất. Trong thực tế, chỉ cần phát hiện xem một nút có cấp độ ba trở lên hay không là đủ. Nếu đúng như vậy, các cạnh liên quan đến nó sẽ nhận được nhãn bắt đầu từ 0, với các cạnh còn lại được gán các nhãn có sẵn tiếp theo. Nếu cây là một đường đi đơn giản hoặc một ngôi sao có bậc nhiều nhất là hai, chúng ta có thể gán nhãn tùy ý dọc theo các cạnh vì MEX tối đa sẽ không vượt quá 2. 

Quan sát này làm giảm vấn đề thành một cách dán nhãn tham lam đơn giản dựa trên mức độ. Sự bùng nổ tổ hợp của lực lượng vũ phu hoàn toàn có thể tránh được bằng cách sử dụng các thuộc tính cấp độ cây. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((n-1)! * n^2) | O(n^2) | Quá chậm | 
| Tham lam dựa trên bằng cấp | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng nút$n$và lưu trữ tất cả các cạnh theo thứ tự đầu vào. Khởi tạo danh sách kề để đếm bậc của mỗi nút. 
2. Đi qua tất cả các cạnh và tăng độ của cả hai điểm cuối. Theo dõi mức độ tối đa
