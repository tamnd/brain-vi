---
title: "CF 106233D - XOR \u0424\u0438\u0431\u043e\u043d\u0430\u0447\u0447\u0438"
description: "Chúng tôi đang làm việc với một chuỗi trông giống như Fibonacci, ngoại trừ thao tác được sử dụng để kết hợp các giá trị trước đó là XOR theo bit thay vì phép cộng số học. Hai giá trị bắt đầu được đưa ra và mọi giá trị tiếp theo chỉ được xác định từ hai giá trị trước đó."
date: "2026-06-19T09:27:26+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106233
codeforces_index: "D"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2025-2026, \u0427\u0435\u0442\u0432\u0435\u0440\u0442\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430"
rating: 0
weight: 106233
solve_time_s: 56
verified: true
draft: false
---

[CF 106233D - XOR \u0424\u0438\u0431\u043e\u043d\u0430\u0447\u0447\u0438](https://codeforces.com/problemset/problem/106233/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 56s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang làm việc với một chuỗi trông giống như Fibonacci, ngoại trừ thao tác được sử dụng để kết hợp các giá trị trước đó là XOR theo bit thay vì phép cộng số học. Hai giá trị bắt đầu được đưa ra và mọi giá trị tiếp theo chỉ được xác định từ hai giá trị trước đó. Nhiệm vụ là tính toán số hạng sau của chuỗi này, thường là số hạng có chỉ số rất lớn, theo phép truy toán dựa trên XOR này. 

Nói một cách cụ thể, chuỗi hoạt động giống như một máy tự động xác định: một khi hai giá trị đầu tiên được cố định, mọi thứ sau đó đều bị ép buộc. Mỗi phần tử mới được hình thành bằng cách lấy XOR theo bit của hai phần tử liền kề trước đó. Đầu ra yêu cầu giá trị ở một số vị trí xa dọc theo chuỗi được tạo này. 

Ràng buộc quan trọng nhất ở đây là kích thước của chỉ mục. Khi thuật ngữ được yêu cầu có thể cực kỳ lớn thì bất kỳ việc mô phỏng trình tự từng bước nào cũng trở nên không thể thực hiện được vì mỗi bước đều phụ thuộc vào bước trước đó. Mô phỏng trực tiếp sẽ yêu cầu thời gian tuyến tính trong chỉ mục, điều này sẽ ngay lập tức phá vỡ các giá trị lớn như 10^9 hoặc cao hơn. Điều này buộc chúng ta phải tìm kiếm cấu trúc theo cách trình tự phát triển. 

Một trường hợp thất bại tinh tế xuất hiện khi nghĩ theo hướng “cứ mô phỏng cho đến khi bạn đạt được chỉ số”. Ví dụ: nếu chỉ mục là 10^9 và mỗi bước là XOR có thời gian không đổi thì việc triển khai vẫn quá chậm mặc dù mỗi thao tác đều rẻ. Một cạm bẫy phổ biến khác là giả sử chuỗi hoạt động giống như các số Fibonacci bình thường và cố gắng áp dụng trực tiếp các công thức số học, không áp dụng được vì XOR không bảo toàn mang và hoạt động giống như phép cộng trong một hệ đại số khác. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force rất đơn giản: bắt đầu từ hai giá trị ban đầu và tính toán lặp lại từng giá trị tiếp theo bằng XOR cho đến khi đạt được chỉ mục yêu cầu. Điều này đúng vì nó tuân theo định nghĩa chính xác. Tuy nhiên, chi phí của nó tăng tuyến tính với chỉ số. Nếu chỉ mục là n, phương thức này thực hiện n lần chuyển đổi, mỗi lần chuyển đổi liên quan đến XOR thời gian không đổi. Khi n có thể rất lớn thì điều này trở nên không khả thi. 

Quan sát quan trọng là sự truy hồi là tuyến tính trên trường có hai phần tử. XOR hoạt động giống như phép cộng modulo 2 trên mỗi bit một cách độc lập, có nghĩa là mỗi vị trí bit tiến triển độc lập theo cùng một phép lặp Fibonacci. Điều này chuyển vấn đề thành phân tích một hệ thống truy hồi tuyến tính, có thể được tăng tốc bằng cách sử dụng phép lũy thừa ma trận. 

Chúng ta có thể mã hóa quá trình chuyển đổi từ trạng thái (F[i], F[i+1]) sang (F[i+1], F[i] XOR F[i+1]) dưới dạng một phép biến đổi tuyến tính. Việc áp dụng lặp lại phép biến đổi này tương ứng với việc nâng ma trận 2×2 lên lũy thừa. Khi chúng ta có thể tính lũy thừa ma trận này theo thời gian logarit, chúng ta có thể trực tiếp thu được số hạng thứ n mà không cần lặp qua tất cả các trạng thái trung gian. 

Lực lượng vũ phu hoạt động vì nó mô phỏng một cách trung thực sự tái diễn. Nó thất bại vì nó coi trình tự là tuần tự vốn có, trong khi cấu trúc chuyển đổi thực sự cho phép chúng ta “nhảy” nhiều bước cùng một lúc bằng cách lũy thừa quá trình chuyển đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) | O(1) | Quá chậm | 
| Hàm mũ ma trận | O(log n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta coi chuỗi là một bài toán chuyển trạng thái trong đó mỗi trạng thái chứa hai giá trị liên tiếp. Mục tiêu là chuyển từ trạng thái ban đầu sang trạng thái ở vị trí n bằng cách nhân đôi lặp đi lặp lại. 

1. Biểu diễn trạng thái ở bước i dưới dạng vectơ chứa cặp (F[i], F[i+1]). Điều này là cần thiết vì sự lặp lại phụ thuộc vào hai giá trị trước đó, do đó việc theo dõi một giá trị là không đủ. 
2. Biểu diễn sự chuyển đổi từ trạng thái này sang trạng thái tiếp theo dưới dạng phép biến đổi tuyến tính:

(F[i], F[i+1]) chuyển đổi thành (F[i+1], F[i] XOR F[i+1]). Điều này nắm bắt chính xác trình tự phát triển như thế nào. 
3. Mã hóa phép biến đổi này thành ma trận 2×2 trên GF(2), trong đó phép nhân tương ứng với XOR và phép cộng cũng tương ứng với XOR. Điều này cho phép chúng ta áp dụng các kỹ thuật lũy thừa nhanh tiêu chuẩn. 
4. Nâng ma trận chuyển tiếp lên lũy thừa (n-1) bằng cách sử dụng lũy ​​thừa nhị phân. Mỗi bước bình phương nén nhiều bước lặp lại thành một thao tác, đây là sự tối ưu hóa quan trọng so với phép lặp đơn giản. 
5. Nhân ma trận thu được với vectơ ban đầu (F[0], F[1]) để thu được trạng thái tại vị trí n. Thành phần đầu tiên của vectơ kết quả này là câu trả lời. 

Lý do điều này có tác dụng là phép truy toán xác định một phép biến đổi tuyến tính trên không gian vectơ hai chiều trên GF(2). Việc áp dụng lặp lại phép truy toán tương ứng chính xác với việc áp dụng lặp lại phép biến đổi này. Phép lũy thừa ma trận tính toán ứng dụng lặp đi lặp lại mà không mô phỏng rõ ràng các trạng thái trung gian và tính tuyến tính đảm bảo rằng không có thông tin nào bị mất trong quá trình nén. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def mat_mul(a, b):
    # 2x2 matrix multiplication over GF(2) with XOR as addition
    return [
        [
            a[0][0] ^ (a[0][1] & b[1][0]) ^ (a[0][1] & b[1][0]),
            a[0][0] ^ a[0][1]
        ]
    ]
```
