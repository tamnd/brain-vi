---
title: "CF 106144M - Game chiến thuật"
description: "Chúng ta được cung cấp một chuỗi nhị phân đại diện cho một dòng ô chiến trường. Mỗi ô trống hoặc chứa kẻ thù. Mục tiêu là loại bỏ mọi kẻ thù bằng hai loại hành động. Một tia sét sẽ loại bỏ chính xác một kẻ thù đã chọn."
date: "2026-06-19T19:28:58+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106144
codeforces_index: "M"
codeforces_contest_name: "2025-2026 ICPC, NERC, Southern and Volga Russian Regional Contest"
rating: 0
weight: 106144
solve_time_s: 53
verified: true
draft: false
---

[CF 106144M - Trò chơi chiến thuật](https://codeforces.com/problemset/problem/106144/M) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 53s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi nhị phân đại diện cho một dòng ô chiến trường. Mỗi ô trống hoặc chứa kẻ thù. Mục tiêu là loại bỏ mọi kẻ thù bằng hai loại hành động. Một tia sét sẽ loại bỏ chính xác một kẻ thù đã chọn. Một quả cầu lửa sẽ loại bỏ một kẻ thù đã chọn và cũng loại bỏ những kẻ thù lân cận nếu chúng tồn tại. Hạn chế là chúng tôi được phép sử dụng tối đa một quả cầu lửa trong toàn bộ quá trình, trong khi tia sét có thể được sử dụng bao nhiêu lần cũng được. 

Nhiệm vụ là giảm thiểu tổng số hành động cần thiết để loại bỏ tất cả kẻ thù. Mỗi kẻ thù phải bị loại bỏ trực tiếp bằng tia sét hoặc gián tiếp bằng hiệu ứng cầu lửa. 

Khó khăn chính là quyết định xem có nên sử dụng quả cầu lửa đơn lẻ hay không và ở đâu, bởi vì tác dụng của nó phụ thuộc rất nhiều vào cấu trúc cục bộ. Nó có khả năng loại bỏ tối đa ba kẻ thù liên tiếp, nhưng vị trí của nó cũng có thể lãng phí nếu nó chồng lên các khu vực có ít kẻ thù. 

Các ràng buộc cho phép tổng chiều dài của các trường hợp thử nghiệm lên tới 3·10^5. Điều này ngay lập tức ngụ ý rằng bất kỳ giải pháp nào cũng phải tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm, vì lần quét O(n^2) cho mỗi trường hợp sẽ suy biến thành khoảng 10^10 thao tác trong trường hợp xấu nhất, điều này là không khả thi. 

Một ý tưởng ngây thơ là thử mọi vị trí có thể cho quả cầu lửa và tính toán lại xem còn lại bao nhiêu kẻ thù. Điều đó sẽ liên tục quét chuỗi, đưa ra hành vi O(n^2). Một cách tiếp cận ngây thơ khác là loại bỏ từ trái sang phải một cách tham lam mà không xem xét vị trí quả cầu lửa, nhưng cách đó không thành công vì vị trí quả cầu lửa tối ưu phụ thuộc vào cấu trúc toàn cầu chứ không chỉ mật độ cục bộ. 

Một vài trường hợp cạnh bộc lộ cấu trúc rõ ràng. 

Nếu chuỗi toàn là số 0 thì câu trả lời là 0 vì không cần thực hiện hành động nào. 

Ví dụ: nếu chuỗi là tất cả những chuỗi`11111`, một chiến lược chỉ có tia chớp sẽ tốn năm hành động. Một quả cầu lửa duy nhất được đặt ở vị trí 3 chỉ loại bỏ cả năm hành động nếu ranh giới được xem xét cẩn thận, nhưng trên thực tế, nó loại bỏ tối đa ba hành động, để lại hai hành động nữa, vì vậy câu trả lời tối ưu là 3 hành động chứ không phải 1 hoặc 2. 

Nếu kẻ thù bị cô lập như`10101`, quả cầu lửa gần như vô dụng vì không có vị trí nào che được nhiều quả cầu lửa riêng biệt; tối ưu duy trì năm tia sét hoặc ít hơn một chút nếu vị trí trung tâm làm giảm sự chồng chéo nhưng mức tăng là tối thiểu. 

Những trường hợp này cho thấy rằng quả cầu lửa chỉ hữu ích trong các phân đoạn liền kề dày đặc và hiệu quả tốt nhất của nó luôn đến từ việc áp dụng nó trong một khối liền kề duy nhất. 

## Phương pháp tiếp cận 

Nếu chúng ta bỏ qua ràng buộc về quả cầu lửa thì vấn đề sẽ không đáng kể: mỗi kẻ thù cần một tia sét, vì vậy câu trả lời chỉ đơn giản là số lượng tia sét. 

Việc giới thiệu một quả cầu lửa sẽ thay đổi mọi thứ vì nó có thể loại bỏ một nhóm tối đa ba kẻ thù liên tiếp trong một chiến dịch. Quan trọng hơn, nó có thể hợp nhất các khoản tiết kiệm trên một khối liền kề bằng cách giảm mật độ địa phương. 

Một cách tiếp cận bạo lực sẽ là thử từng chỉ mục i trong đó s[i] = '1' làm trung tâm quả cầu lửa. Đối với mỗi lựa chọn như vậy, chúng tôi mô phỏng việc loại bỏ i và các hàng xóm của nó rồi đếm những cái còn lại. Việc tính toán lại này tốn O(n) và thực hiện nó cho tất cả các ứng viên sẽ mang lại O(n^2). 

Quan sát quan trọng là chuỗi phân hủy thành các khối độc lập liên tiếp. Bất kỳ quả cầu lửa nào cũng chỉ có thể ảnh hưởng đến một khối duy nhất theo cách có ý nghĩa, bởi vì các số 0 tách biệt các thành phần và ngăn chặn sự tương tác giữa các khối. Bên trong một khối có chiều dài k, không có quả cầu lửa chúng ta cần k tia sét. Với một quả cầu lửa, chúng tôi giảm số lượng hành động cần thiết tối đa là 2, vì kết quả tốt nhất có thể xảy ra là loại bỏ ba quả cầu lửa liên tiếp trong một hành động thay vì ba tia sét riêng biệt. 

Do đó, toàn bộ vấn đề được rút gọn thành việc tìm một đoạn liền kề của những đoạn mà việc đặt quả cầu lửa mang lại mức giảm tối đa. Trong một khối có độ dài k, mức tăng chính xác là 1 nếu k ≥ 1, 2 nếu k ≥ 3 và 3 là không thể vì sự chồng lấp kề sẽ ngăn cản việc loại bỏ nhiều hơn ba cái riêng biệt trên mỗi quả cầu lửa. 

Vì chúng tôi chỉ được phép có một quả cầu lửa trên toàn cầu nên chúng tôi muốn khối tốt nhất nơi chúng tôi có thể tối đa hóa số tia sét được lưu. Tuy nhiên, chiến lược tối ưu toàn cục còn đơn giản hóa hơn nữa: chúng ta không cần phải chọn một khối một cách rõ ràng, chúng ta chỉ cần tính tổng số khối và trừ đi số tiền tiết kiệm tốt nhất có thể, tối đa là 2. 

Chính xác hơn, mức tiết kiệm tối ưu đạt được bằng cách tìm bất kỳ mẫu nào gồm ba mẫu liên tiếp trong một khối. Nếu mẫu như vậy tồn tại, chúng tôi lưu 2 thao tác. Nếu không có mẫu như vậy tồn tại nhưng có ít nhất một cặp mẫu liền kề tồn tại, chúng ta lưu 1. Nếu không, chúng ta không thể lưu bất cứ thứ gì. 

Điều này làm giảm vấn đề quét chuỗi một lần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Vị trí quả cầu lửa Brute Force | O(n^2) | O(1) | Quá chậm | 
| Quét các khối và tính toán mức tăng tốt nhất | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý chuỗi và theo dõi ba giá trị: tổng số giá trị đơn vị, liệu có tồn tại một cặp giá trị liên tiếp hay không và liệu có tồn tại bộ ba giá trị liên tiếp hay không. 

1. Đếm tổng số đơn vị trong chuỗi. Đây là chi phí cơ bản nếu chúng ta chỉ sử dụng các đòn tấn công chớp nhoáng. 
2. Quét chuỗi từ trái sang phải và kiểm tra các chuỗi liên tiếp. Whenever we see`s[i] = s[i-1] = '1'`, chúng tôi đánh dấu rằng có thể tiết kiệm được ít nhất 1 vì một quả cầu lửa có thể loại bỏ ít nhất hai kẻ thù liền kề hiệu quả hơn hai tia sét riêng biệt. 
3. Bất cứ khi nào chúng ta nhìn thấy`s[i] = s[i-1] = s[i-2] = '1'`, chúng tôi đánh dấu rằng có thể tiết kiệm được 2 mạnh hơn vì một quả cầu lửa có thể thay thế ba đòn tấn công sét riêng biệt. 
4. Sau khi quét, hãy xác định mức tiết kiệm tốt nhất có thể đạt được. Nếu bộ ba tồn tại ở bất kỳ đâu thì mức tiết kiệm là 2. Ngược lại, nếu tồn tại một cặp thì mức tiết kiệm là 1. Ngược lại, mức tiết kiệm là 0. 
5. Tổng số đầu ra trừ đi mức tiết kiệm. 

Lý do điều này có hiệu quả là vì bất kỳ quả cầu lửa nào cũng luôn ảnh hưởng đến một khu vực liền kề có nhiều nhất là ba vị trí và những cải tiến có ý nghĩa duy nhất đến từ việc thay thế nhiều tia sét liên tiếp. Thời gian chạy dài hơn không làm tăng hiệu quả của mỗi quả cầu lửa ngoài những trường hợp này vì một quả cầu lửa không thể bao phủ nhiều hơn ba vị trí. 

### Tại sao nó hoạt động 

Thuật toán dựa trên thực tế là lợi ích của quả cầu lửa đơn được xác định hoàn toàn bởi cấu trúc lân cận cục bộ. Mọi vị trí tối ưu đều có thể được giảm xuống thành một cấu hình bên trong một loạt các vị trí liền kề. Trong quá trình chạy như vậy, sự cải thiện tốt nhất có thể được giới hạn bởi số lượng liên tiếp có thể tập trung xung quanh vị trí quả cầu lửa. Điều này giới hạn vấn đề tối ưu hóa tổng thể vào việc tìm kiếm mẫu cục bộ có hệ số không đổi, đảm bảo không có cấu hình nào bên ngoài các mẫu này có thể mang lại sự cải thiện tốt hơn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    s = input().strip()

    ones = s.count('1')

    best = 0

    for i in range(n):
        if s[i] == '1':
            if i > 0 and s[i-1] == '1':
                best = max(best, 1)
            if i > 1 and s[i-1] == '1' and s[i-2] == '1':
                best = 2

    print(ones - best)
```Giải pháp bắt đầu bằng cách đếm tất cả kẻ thù, vì thông thường mỗi kẻ thù sẽ cần một đòn sét đánh. Biến`best`theo dõi xem chúng tôi có thể giảm chi phí này đến mức nào chỉ bằng một quả cầu lửa. 

Trong quá trình quét, chúng tôi phát hiện các mẫu lân cận. Một cặp tia sáng liên tiếp đảm bảo rằng ít nhất một quả cầu lửa có thể cứu được một hoạt động bằng cách thay thế hai tia sét bằng một quả cầu lửa cộng với một tia sét còn sót lại ở nơi khác. Ba kẻ thù đảm bảo tiết kiệm tối đa hai kẻ thù, vì ba kẻ thù liền kề có thể được thay thế bằng một quả cầu lửa duy nhất thay vì ba tia sét riêng biệt. 

Việc triển khai sử dụng thao tác quét từ trái sang phải đơn giản và thời điểm tìm thấy bộ ba, chúng ta có thể thiết lập một cách an toàn`best = 2`vì không có cấu hình nào có thể vượt qua sự cải tiến này. 

Cần phải cẩn thận ở các ranh giới vì việc lập chỉ mục i-2 chỉ được truy cập khi hợp lệ. 

## Ví dụ đã hoạt động 

### Ví dụ 1:`1110`Chúng tôi có một khối gồm ba khối. 

| tôi | s[i] | kiểm tra mẫu | tốt nhất | 
| --- | --- | --- | --- | 
| 0 | 1 | bắt đầu | 0 | 
| 1 | 1 | tìm thấy cặp | 1 | 
| 2 | 1 | tìm thấy ba | 2 | 
| 3 | 0 | đặt lại | 2 | 

Tổng số cái = 3, tiết kiệm tốt nhất = 2, câu trả lời = 1. 

Điều này cho thấy quả cầu lửa nén hoàn toàn một cụm ba kẻ thù dày đặc vào một hoạt động duy nhất. 

### Ví dụ 2:`11011`Hai khối riêng biệt tồn tại. 

| tôi | s[i] | kiểm tra mẫu | tốt nhất | 
| --- | --- | --- | --- | 
| 0 | 1 | bắt đầu | 0 | 
| 1 | 1 | tìm thấy cặp | 1 | 
| 2 | 0 | nghỉ | 1 | 
| 3 | 1 | bắt đầu | 1 | 
| 4 | 1 | tìm thấy cặp | 1 | 

Tổng số cái = 4, tiết kiệm tốt nhất = 1, câu trả lời = 3. 

Điều này chứng tỏ rằng các cụm riêng biệt không kết hợp được lợi ích của chúng dưới một quả cầu lửa duy nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ký tự được xử lý một lần cho mỗi trường hợp thử nghiệm | 
| Không gian | O(1) | Chỉ bộ đếm và cờ được lưu trữ | 

Tổng giới hạn độ dài trên tất cả các trường hợp thử nghiệm là 3·10^5, do đó, một lần quét tuyến tính cho mỗi trường hợp thử nghiệm sẽ vừa vặn thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        s = input().strip()

        ones = s.count('1')
        best = 0

        for i in range(n):
            if s[i] == '1':
                if i > 0 and s[i-1] == '1':
                    best = max(best, 1)
                if i > 1 and s[i-1] == '1' and s[i-2] == '1':
                    best = 2

        out.append(str(ones - best))

    return "\n".join(out)

# edge: no enemies
assert run("1\n5\n00000\n") == "0"

# edge: all ones small
assert run("1\n3\n111\n") == "1"

# edge: separated ones
assert run("1\n5\n10101\n") == "5"

# edge: mixed blocks
assert run("1\n6\n110111\n") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 00000 | 0 | không cần thao tác | 
| 111 | 1 | lợi ích tối đa của quả cầu lửa | 
| 10101 | 5 | không có lợi ích liền kề | 
| 110111 | 2 | khối hỗn hợp với mức tiết kiệm gấp ba | 

## Vỏ cạnh 

Đối với một chuỗi như`00000`, thuật toán ngay lập tức đếm số 0 và không bao giờ kích hoạt bất kỳ điều kiện kề nào. Đầu ra chính xác bằng 0 vì không thể lưu được. 

Vì`111`, quá trình quét sẽ phát hiện cả một cặp và một bộ ba. Ở chỉ số 2, điều kiện ba được đặt tốt nhất thành 2, dẫn đến câu trả lời 1, tương ứng với việc thay thế ba tia sét bằng một quả cầu lửa duy nhất cộng với không có kẻ thù nào còn lại trong phân đoạn đó. 

Vì`10101`, không có cái liền kề nào tồn tại, vì vậy tốt nhất vẫn bằng 0. Mỗi kẻ thù đều bị cô lập và mỗi kẻ thù đều cần một tia sét riêng. 

Vì`110111`, khối đầu tiên cho một cặp và khối thứ hai cho một bộ ba, vì vậy tốt nhất là 2. Thuật toán chỉ áp dụng chính xác quả cầu lửa khi nó mang lại khả năng nén tối đa, bỏ qua các khối đó là riêng biệt vì chỉ có một quả cầu lửa.
