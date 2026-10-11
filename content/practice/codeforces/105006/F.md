---
title: "CF 105006F - Đi dạo trong công viên"
description: "Mỗi công việc dắt chó đi dạo có thể được coi là một hạng mục mà Alice có thể chọn nhiều nhất một lần. Một công việc có hai thông tin: mẫu yêu cầu 7 ngày và giá trị thanh toán. Mẫu này là một vectơ nhị phân có độ dài 7 cho biết những ngày nào trong tuần con chó phải được dắt đi dạo."
date: "2026-06-28T03:12:46+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105006
codeforces_index: "F"
codeforces_contest_name: "UTPC Contest 03-01-24 Div. 1 (Advanced)"
rating: 0
weight: 105006
solve_time_s: 36
verified: true
draft: false
---

[CF 105006F - Đi dạo trong công viên](https://codeforces.com/problemset/problem/105006/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 36s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi công việc dắt chó đi dạo có thể được coi là một hạng mục mà Alice có thể chọn nhiều nhất một lần. Một công việc có hai thông tin: mẫu yêu cầu 7 ngày và giá trị thanh toán. Mẫu này là một vectơ nhị phân có độ dài 7 cho biết những ngày nào trong tuần con chó phải được dắt đi dạo. Nếu Alice chấp nhận một công việc, cô ấy cam kết sẽ dắt con chó đó đi dạo vào mỗi ngày được đánh dấu số 1 trong khuôn mẫu của nó và mỗi cam kết như vậy sẽ tiêu tốn một trong những năng lực hàng ngày của cô ấy. 

Lịch trình hàng tuần là cố định: vào mỗi ngày trong số 7 ngày, Alice có thể dắt tối đa 2 con chó đi dạo, vì vậy mỗi ngày có công suất là 2. Ràng buộc mang tính toàn cầu đối với tất cả các công việc đã chọn, vì một công việc có thể tiêu tốn công suất trong nhiều ngày cùng một lúc. 

Nhiệm vụ là chọn một tập hợp con các công việc tối đa hóa tổng số tiền thanh toán đồng thời đảm bảo rằng trong mỗi ngày, số lượng công việc được chọn yêu cầu trong ngày đó không vượt quá 2. 

Kích thước đầu vào nhỏ về mặt N, nhiều nhất là 100 công việc, nhưng khả năng kết hợp ràng buộc trong 7 ngày là rất mạnh. Điều này ngay lập tức gợi ý rằng về mặt khái niệm có thể áp dụng bạo lực đối với các tập hợp con công việc vì có nhiều nhất$2^{100}$tập hợp con, nhưng rõ ràng là không khả thi. Tuy nhiên, cấu trúc thực tế là mỗi công việc chỉ tương tác với 7 tài nguyên được chia sẻ, nghĩa là không gian trạng thái về cơ bản bị chi phối bởi tải mỗi ngày chứ không phải bởi chính N. 

Cách tiếp cận tham lam ngây thơ không thành công vì chọn sớm một công việc lương cao có thể cản trở nhiều công việc nhỏ hơn cùng nhau vượt quá giá trị của nó. Ví dụ: một công việc yêu cầu cả 7 ngày với phần thưởng 100 có thể trông tối ưu tại địa phương, nhưng hai công việc yêu cầu các tập hợp con ngày rời rạc có thể mang lại tổng cộng 200 ngày trong khi vẫn tuân thủ các hạn chế về năng lực mỗi ngày. 

Một trường hợp thất bại tinh tế khác xảy ra khi nhiều công việc chồng chéo lên nhau trong một ngày. Việc lựa chọn quá nhiều công việc có giá trị vừa phải mà tất cả chỉ bao gồm một ngày bận rộn có thể vi phạm tính khả thi chỉ ở giai đoạn cuối, khiến cho việc lựa chọn tham lam khó có thể sửa chữa về sau. 

## Phương pháp tiếp cận 

Phương pháp brute-force trực tiếp liệt kê mọi tập hợp con công việc, kiểm tra tính khả thi bằng cách mô phỏng mức sử dụng công suất hàng tuần và tính toán tổng lợi nhuận. Chi phí kiểm tra tính khả thi$O(7N)$mỗi tập hợp con vì chúng tôi đếm số lượng công việc đã chọn yêu cầu mỗi ngày. Với$2^N$tập hợp con, điều này trở thành$O(2^N \cdot N)$, lớn về mặt thiên văn đối với$N = 100$. 

Quan sát quan trọng là các hạn chế tài nguyên được chia sẻ duy nhất là 7 ngày trong tuần, mỗi ngày có dung lượng 2. Điều này có nghĩa là trạng thái hệ thống được mô tả đầy đủ bằng số lượng vị trí được sử dụng trong mỗi 7 ngày, tức là vectơ 7 chiều trong đó mỗi mục nhập nằm trong$\{0,1,2\}$. Tổng số các trạng thái như vậy là$3^7 = 2187$, đủ nhỏ để lập trình động. 

Mỗi công việc sẽ chuyển đổi hệ thống bằng cách thêm một vectơ nhị phân cố định vào trạng thái hiện tại, với điều kiện là không có tọa độ nào vượt quá 2. Điều này biến vấn đề thành một DP kiểu ba lô cổ điển trên một không gian dung lượng đa chiều nhỏ, trong đó các mục là công việc và các kích thước là các ngày trong tuần. 

Do đó, chúng tôi thực hiện DP trên tất cả các cấu hình tải khả thi, cập nhật trạng thái bằng cách xem xét nên thực hiện từng công việc hay bỏ qua nó. Việc nén từ 100 công việc vào không gian trạng thái có kích thước 2187 là điều làm cho giải pháp trở nên hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^N \cdot N)$|$O(7)$| Quá chậm | 
| DP tối ưu qua các trạng thái tải hàng ngày |$O(N \cdot 3^7)$|$O(3^7)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi biểu thị mỗi trạng thái DP dưới dạng 7 bộ số nguyên, trong đó mỗi mục nhập ghi lại số lượng con chó đã được chỉ định cho ngày đó. Mỗi mục có phạm vi từ 0 đến 2. 

1. Khởi tạo từ điển hoặc mảng`dp`trong đó khóa là trạng thái 7 ngày và giá trị là lợi nhuận tối đa có thể đạt được. Bắt đầu với trạng thái hoàn toàn bằng 0 có giá trị 0. Điều này thể hiện việc không chọn công việc nào. 
2. Xử lý từng công việc một. Đối với mỗi công việc, hãy trích xuất vectơ yêu cầu 7 ngày và khoản thanh toán của nó. 
3. Đối với mỗi trạng thái DP hiện có, hãy xem xét hai khả năng: bỏ qua công việc, giữ trạng thái không thay đổi hoặc nhận công việc, thêm vectơ yêu cầu của nó vào trạng thái. 
4. Khi cố gắng nhận một công việc, hãy xác minh tính khả thi bằng cách đảm bảo rằng cứ mỗi ngày, số lượng kết quả không vượt quá 2. Điều này đảm bảo rằng chúng tôi tôn trọng giới hạn năng lực của Alice. 
5. Nếu trạng thái kết quả là hợp lệ, hãy cập nhật mục nhập bảng DP cho trạng thái đó với giá trị tối đa hiện tại và giá trị trạng thái trước đó cộng với khoản thanh toán của công việc. 
6. Sau khi xử lý tất cả công việc, câu trả lời là giá trị tối đa trên tất cả các trạng thái DP. 

Ý tưởng quan trọng là DP không bao giờ cần nhớ công việc nào đã được chọn mà chỉ cần nhớ cấu hình tải cảm ứng. Điều này hợp lệ vì tất cả các ràng buộc trong tương lai chỉ phụ thuộc vào khối lượng công việc hiện tại chứ không phụ thuộc vào nhận dạng công việc. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, mọi trạng thái DP đều mã hóa chính xác tập hợp cấu hình tải có thể đạt được sau khi xử lý tiền tố công việc, cùng với lợi nhuận tốt nhất có thể đạt được cho mỗi cấu hình. Khi xử lý một công việc mới, quá trình chuyển đổi sẽ xem xét tất cả các cách mở rộng các cấu hình hợp lệ trước đó bằng cách loại trừ hoặc bao gồm công việc đó. Vì tính khả thi chỉ phụ thuộc vào tải ngày tích lũy và chúng được thể hiện đầy đủ ở trạng thái nên không có thông tin cần thiết nào cho các quyết định trong tương lai bị mất. Do đó, bất biến DP khẳng định: sau khi xử lý i công việc, tất cả các trạng thái hợp lệ có thể truy cập được với lợi nhuận tối ưu của chúng đều được ghi lại một cách chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def encode(state):
    # base-3 encoding of 7 digits
    res = 0
    for x in state:
        res = res * 3 + x
    return res

def decode(x):
    state = [0] * 7
    for i in range(6, -1, -1):
        state[i] = x % 3
        x //= 3
    return state

def add_state(state, req):
    new_state = state[:]
    for i in range(7):
        new_state[i] += req[i]
        if new_state[i] > 2:
            return None
    return new_state

def solve():
    n = int(input())
    jobs = []
    for _ in range(n):
        *days, val = map(int, input().split())
        jobs.append((days, val))

    dp = {encode([0]*7): 0}

    for days, val in jobs:
        cur_items = list(dp.items())
        for mask, profit in cur_items:
            state = decode(mask)

            new_state = add_state(state, days)
            if new_state is not None:
                new_mask = encode(new_state)
                if new_mask not in dp or dp[new_mask] < profit + val:
                    dp[new_mask] = profit + val

    print(max(dp.values()))

if __name__ == "__main__":
    solve()
```Bảng DP được lưu trữ dưới dạng từ điển được khóa bằng mã hóa cơ sở 3 của vectơ tải 7 ngày. Điều này tránh việc sao chép liên tục danh sách 7 phần tử trong các vòng lặp bên trong trong khi vẫn giữ trạng thái nhỏ gọn. 

Mỗi công việc được xử lý bằng cách lặp qua ảnh chụp nhanh các trạng thái hiện tại. Điều này rất cần thiết vì các cập nhật cho công việc hiện tại không được ảnh hưởng ngay lập tức đến các quá trình chuyển đổi khác của cùng một công việc, nếu không sẽ làm tăng số lượng lựa chọn. 

Mã hóa cơ sở 3 đảm bảo băm và so sánh nhanh các trạng thái và việc giải mã chỉ được thực hiện khi chúng ta cần áp dụng chuyển tiếp. Vì không gian trạng thái nhỏ nên chi phí này không đáng kể. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2
1 0 0 0 0 0 0 10
1 0 0 0 0 0 0 20
```Chỉ dùng ngày 1, dung lượng là 2. 

| Bước | Tiểu bang (ngày 1) | Hành động | Lợi nhuận | 
| --- | --- | --- | --- | 
| Bắt đầu | 0 | không | 0 | 
| Công việc 1 | 1 | lấy | 10 | 
| Công việc 2 | 2 | lấy | 30 | 

DP cho thấy cả hai công việc đều có thể được thực hiện vì năng lực là 2, tổng cộng là 30. 

### Ví dụ 2 

đầu vào:```
3
1 1 1 0 0 0 0 0 50
1 1 1 0 0 0 0 0 60
1 1 1 0 0 0 0 0 40
```Tất cả các công việc đều sử dụng hai ngày giống nhau, công suất mỗi ngày là 2. 

| Bước | Trạng thái (d1, d2) | Hành động | Lợi nhuận | 
| --- | --- | --- | --- | 
| Bắt đầu | (0,0) | không | 0 | 
| Công việc 1 | (1,1) | lấy | 50 | 
| Công việc 2 | (2,2) | lấy | 110 | 
| Công việc 3 | bỏ qua | không hợp lệ (sẽ vượt quá 2) | 110 | 

Điều này xác nhận rằng DP thực thi chính xác năng lực chia sẻ giữa các công việc. 

## Phân tích độ phức tạp 

| Đo | có
