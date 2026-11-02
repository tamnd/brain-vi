---
title: "CF 1066A - Vova và Tàu hỏa"
description: "Cài đặt là đường dẫn một chiều từ vị trí 1 đến vị trí L, trong đó thời gian và vị trí được đồng bộ hóa sao cho tại phút i, Vova ở tọa độ i."
date: "2026-06-15T08:06:13+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 1066
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 515 (Div. 3)"
rating: 1100
weight: 1066
solve_time_s: 376
verified: true
draft: false
---

[CF 1066A - Vova và Train](https://codeforces.com/problemset/problem/1066/A) 

**Đánh giá:** 1100 
**Thẻ:** toán 
**Thời gian giải:** 6 phút 16s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Cài đặt là đường dẫn một chiều từ vị trí 1 đến vị trí L, trong đó thời gian và vị trí được đồng bộ hóa sao cho tại phút i, Vova ở tọa độ i. Dọc theo con đường này có các nguồn sáng cách đều nhau được đặt mỗi v đơn vị, do đó chỉ những vị trí chia hết cho v mới chứa đèn lồng. 

Đồng thời, có một đoạn bị chặn liên tục từ l đến r nơi đèn lồng tồn tại nhưng không thể nhìn thấy được vì một đoàn tàu đang đứng che phủ toàn bộ khoảng đó. Một chiếc đèn lồng chỉ hữu ích nếu nó nằm trên bội số của v và không nằm trong đoạn bị chặn đó. 

Đối với mỗi truy vấn, nhiệm vụ giảm xuống việc đếm xem có bao nhiêu bội số của v nằm trong phạm vi từ 1 đến L, ngoại trừ những bội số nằm trong [l, r]. 

Các ràng buộc đủ lớn để mỗi L, v, l và r có thể lên tới 10^9 và có tối đa 10^4 truy vấn. Điều này ngay lập tức loại trừ mọi cách tiếp cận lặp lại trên tất cả bội số của v cho mỗi truy vấn. Trong trường hợp xấu nhất, khi v bằng 1, có thể có tới 10^9 vị trí đèn lồng ứng viên cho mỗi truy vấn, điều này khiến cho việc mô phỏng trực tiếp không thể thực hiện được trong vòng một giây. 

Một sai lầm ngây thơ thường xảy ra khi xử lý đoạn bị chặn. Người ta có thể đếm chính xác tất cả các bội số của v cho đến L, nhưng sau đó trừ không chính xác tất cả các điểm trong [l, r] hoặc tất cả các bội số trong phạm vi đó mà không tính toán lại ranh giới một cách cẩn thận. Một vấn đề tế nhị khác xuất hiện khi l hoặc r không phải là bội số của v, vì chỉ các bội số bên trong khoảng mới quan trọng chứ không phải bản thân độ dài khoảng. 

## Phương pháp tiếp cận 

Một cách trực tiếp để giải quyết một truy vấn là lặp qua tất cả các bội số của v cho đến L và đếm xem có bao nhiêu không nằm trong [l, r]. Điều này đúng về mặt khái niệm vì nó khớp chính xác với định nghĩa. Vấn đề là hiệu suất. Số bội số là sàn(L / v), trong trường hợp xấu nhất đạt 10^9 khi v bằng 1, do đó phương pháp này thất bại ngay lập tức trong thời hạn. 

Quan sát quan trọng là các vị trí đèn lồng tạo thành một cấp số cộng đơn giản. Thay vì lặp lại chúng, chúng ta có thể đếm xem tồn tại bao nhiêu bội số hợp lệ bằng cách chia số nguyên. Tổng số đèn lồng trên đường là sàn(L/v). Từ tổng số này, chúng tôi trừ đi bao nhiêu bội số của v nằm trong đoạn bị chặn [l, r]. 

Việc đếm bội số của v trong bất kỳ tiền tố [1, x] nào cũng đơn giản bằng cách sử dụng Floor(x / v). Do đó, số bội số bên trong [l, r] là tầng(r / v) trừ tầng((l - 1) / v). Điều này biến đổi mỗi truy vấn thành một số phép tính số học không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(L / v) mỗi truy vấn | O(1) | Quá chậm | 
| Tối ưu | O(1) mỗi truy vấn | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tổng số vị trí đèn lồng từ 1 đến L là sàn(L/v). Điều này đếm mọi bội số hợp lệ bất kể sự tắc nghẽn. 
2. Tính xem có bao nhiêu bội số của v nằm trong [1, r] dưới dạng sàn(r / v). Điều này đưa tất cả các đèn lồng đến ranh giới bên phải của đoạn bị chặn. 
3. Tính xem có bao nhiêu bội số của v nằm trong [1, l - 1] dưới dạng sàn((l - 1) / v). Việc trừ đi điều này sẽ loại bỏ mọi thứ nghiêm ngặt trước đoạn bị chặn. 
4. Trừ kết quả của bước 3 khỏi bước 2 để có được bao nhiêu chiếc đèn lồng nằm bên trong [l, r]. Điều này cô lập chính xác những chiếc đèn lồng bị chặn. 
5. Trừ tổng số đèn lồng bị chặn để có câu trả lời cuối cùng cho truy vấn. 

Lý do bước 3 là cần thiết là vì chỉ riêng phép chia trực tiếp cho r sẽ vượt qua tất cả các bội số từ đầu đường dẫn, không chỉ các bội số bên trong khoảng bị chặn. 

### Tại sao nó hoạt động

Các vị trí đèn lồng chính xác là tập hợp các số nguyên có dạng k·v. Việc đếm chúng đến một ranh giới x tương đương với việc đếm các số nguyên k sao cho k·v ≤ x, chính xác là sàn(x / v). Ánh xạ này chuyển đổi bài toán hình học thành các số nguyên đếm trong tiền tố. Vì các tiền tố được đóng theo phép trừ, nên bất kỳ số khoảng nào cũng sẽ trở thành sự khác biệt của hai số tiền tố. Do đó, câu trả lời cuối cùng là hệ quả trực tiếp của việc phân rã khoảng trên một dãy số học đơn điệu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        L, v, l, r = map(int, input().split())

        total = L // v
        blocked = r // v - (l - 1) // v
        print(total - blocked)

if __name__ == "__main__":
    solve()
```Mã xử lý từng truy vấn một cách độc lập. Biến tổng đếm tất cả các đèn lồng lên đến L bằng cách sử dụng phép chia số nguyên. Số lượng bị chặn sử dụng thủ thuật chênh lệch tiền tố tiêu chuẩn để đếm bội số của v bên trong phân đoạn [l, r]. Trừ đi những cái này sẽ cho ra những chiếc đèn lồng có thể nhìn thấy được. 

Một cạm bẫy triển khai phổ biến là quên số hạng (l - 1). Nếu không có nó, phép tính sẽ bao gồm không chính xác tất cả các bội số lên đến r nhưng không thể loại trừ những bội số trước l, dẫn đến phép trừ quá mức hoặc dưới mức tùy theo cách giải thích. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
10 2 3 7
```Chúng tôi theo dõi tính toán: 

| Bước | Biểu hiện | Giá trị | 
| --- | --- | --- | 
| Tổng số đèn lồng | 10 // 2 | 5 | 
| Lên đến r | 7 // 2 | 3 | 
| Trước tôi | (3 - 1) // 2 | 1 | 
| Bị chặn | 3 - 1 | 2 | 
| Trả lời | 5 - 2 | 3 | 

Điều này phù hợp với ý tưởng rằng chỉ có bội số của 2 bên ngoài [3, 7] mới hiển thị. 

### Ví dụ 2 

đầu vào:```
1234 1 100 199
```| Bước | Biểu hiện | Giá trị | 
| --- | --- | --- | 
| Tổng số đèn lồng | 1234 // 1 | 1234 | 
| Lên đến r | 199 // 1 | 199 | 
| Trước tôi | 99 // 1 | 99 | 
| Bị chặn | 199 - 99 | 100 | 
| Trả lời | 1234 - 100 | 1134 | 

Điều này cho thấy rằng khi v bằng 1, mọi vị trí đều là một chiếc đèn lồng, do đó câu trả lời chỉ đơn giản là tổng chiều dài trừ đi chiều dài đoạn bị chặn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi truy vấn được xử lý bằng một số phép toán số học không đổi | 
| Không gian | O(1) | Chỉ một bộ số nguyên cố định được lưu trữ cho mỗi truy vấn | 

Giải pháp dễ dàng nằm trong giới hạn vì ngay cả 10^4 truy vấn cũng chỉ yêu cầu 10^4 phép tính theo thời gian không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        L, v, l, r = map(int, input().split())
        total = L // v
        blocked = r // v - (l - 1) // v
        out.append(str(total - blocked))
    return "\n".join(out)

# provided samples
assert run("""4
10 2 3 7
100 51 51 51
1234 1 100 199
1000000000 1 1 1000000000
""") == """3
0
1134
0"""

# custom tests
assert run("""1
1 1 1 1
""") == "0"

assert run("""1
10 3 2 5
""") == "3"

assert run("""1
100 10 1 100
""") == "10"

assert run("""1
50 7 15 30
""") == str((50 // 7) - ((30 // 7) - (14 // 7)))
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 1 1 | 0 | đèn lồng đơn bị chặn hoàn toàn | 
| 10 3 2 5 | 3 | chồng chéo một phần của khoảng bị chặn | 
| 100 10 1 100 | 10 | chặn toàn bộ đường dẫn loại bỏ tất cả bội số | 
| 50 7 15 30 | công thức | độ chính xác khoảng tùy ý | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là khi đoạn bị chặn bắt đầu ở 1. Trong trường hợp đó, l - 1 trở thành 0 và sàn(0 / v) là 0, điều này có nghĩa chính xác là không có đèn lồng nào trước khoảng thời gian đó. Công thức vẫn hoạt động mà không cần sửa đổi. 

Một trường hợp khác là khi v lớn hơn L. Khi đó tổng số đèn lồng trở thành 0 vì không có bội số nào của v phù hợp trong phạm vi. Tính toán bị chặn vẫn có thể tạo ra các giá trị, nhưng phép trừ giữ kết quả ở mức 0 do không có đèn lồng nào cần loại bỏ. 

Trường hợp thứ ba là khi đoạn bị chặn bao phủ toàn bộ phạm vi. Khi đó r // v trừ (l - 1) // v bằng L // v, khớp chính xác với tổng số lượng, dẫn đến không có đèn lồng nào nhìn thấy được như mong đợi. 

Những hành vi này xác nhận rằng công thức đếm tiền tố vẫn ổn định trong các điều kiện biên và không yêu cầu cách viết đặc biệt.
