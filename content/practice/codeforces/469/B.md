---
title: "CF 469B - Trò chuyện trực tuyến"
description: "Chúng ta được yêu cầu đếm xem có bao nhiêu thời điểm khác nhau mà Bé X có thể thức dậy sao cho bé trùng với Bé Z trên mạng. Little Z có lịch trình cố định bao gồm nhiều phân đoạn."
date: "2026-05-30T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 469
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 268 (Div. 2)"
rating: 1300
weight: 469
solve_time_s: 74
verified: true
draft: false
---

[CF 469B - Trò chuyện trực tuyến](https://codeforces.com/problemset/problem/469/B) 

**Đánh giá:** 1300 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 14s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu đếm xem có bao nhiêu thời điểm khác nhau mà Bé X có thể thức dậy sao cho bé trùng với Bé Z trên mạng. Little Z có lịch trình cố định bao gồm nhiều phân đoạn. Bé X có một lịch trình được xác định tương ứng với thời gian thức dậy của mình: nếu bé thức dậy đúng giờ`t`, mỗi phân đoạn trong lịch trình của anh ấy sẽ dịch chuyển về phía trước một khoảng`t`. Chúng tôi được yêu cầu xác định có bao nhiêu số nguyên thời gian thức dậy`t`trong phạm vi`[l, r]`cho phép có ít nhất một khoảnh khắc trùng lặp giữa bất kỳ phân đoạn nào trong lịch trình đã thay đổi của Little X và bất kỳ phân đoạn nào trong lịch trình của Little Z. 

Những hạn chế còn khiêm tốn:`p`Và`q`, số lượng phân đoạn, tối đa là 50 và cửa sổ đánh thức`[l, r]`có thể lên tới 1000. Mỗi phân đoạn có thể có độ dài lên tới 1000, nhưng vì tổng số phân đoạn nhỏ nên chúng tôi có thể sử dụng phương pháp kiểm tra sự trùng lặp theo từng phân đoạn. 

Trường hợp cạnh tinh vi phát sinh khi các đoạn tiếp xúc chính xác tại điểm cuối của chúng. Ví dụ: nếu Little Z trực tuyến từ 2 đến 3 và Little X trực tuyến từ 3 đến 4, thì chúng trùng nhau ở thời điểm 3. Một kiểm tra chồng chéo "nghiêm túc nhỏ hơn" sẽ bỏ lỡ điều này. Tương tự, các ca làm việc có thể tạo ra sự chồng chéo ở thời điểm thức dậy đầu tiên hoặc cuối cùng; thật dễ dàng để quên bao gồm các giới hạn`l`Và`r`như những ứng cử viên hợp lệ. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Đối với thời gian thức dậy của mỗi ứng viên`t`TRONG`[l, r]`, dịch chuyển tất cả các phân đoạn của Little X bằng cách`t`và sau đó kiểm tra từng cặp phân đoạn`(X_segment, Z_segment)`cho sự chồng chéo. Mỗi lần kiểm tra chồng chéo mất thời gian không đổi, do đó tổng số thao tác là`O((r-l+1) * p * q)`. Trong trường hợp xấu nhất, điều này gần như`1001 * 50 * 50 = 2.5 million`kiểm tra, có thể chấp nhận được trong giới hạn 1 giây. Cách tiếp cận này đơn giản, dễ thực hiện và đảm bảo hoạt động trong giới hạn cho phép. 

Quan sát dẫn đến việc triển khai thậm chí còn rõ ràng hơn là sự chồng chéo xảy ra nếu`max(start_X + t, start_Z) <= min(end_X + t, end_Z)`. Điều này đưa ra một công thức trực tiếp, theo thời gian không đổi để kiểm tra xem hai phân đoạn có trùng nhau hay không, giúp loại bỏ nhu cầu lặp lại từng lần bên trong các phân đoạn. Vì số lượng phân đoạn nhỏ nên cách tiếp cận này hiệu quả, dễ đọc và tránh được các lỗi riêng lẻ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((r-l+1) * p * q) | O(p+q) | Đã chấp nhận | 
| Kiểm tra được tối ưu hóa | O((r-l+1) * p * q) | O(p+q) | Đã chấp nhận | 

Phương pháp brute-force và phương pháp kiểm tra chồng chéo được tối ưu hóa về cơ bản có độ phức tạp tiệm cận giống nhau ở đây, nhưng việc sử dụng công thức trực tiếp sẽ tránh được những lỗi khó phát hiện và làm cho mã sạch hơn. 

## Hướng dẫn thuật toán 

1. Đọc tất cả đầu vào: số đoạn`p`Và`q`, phạm vi đánh thức`[l, r]`, các phân đoạn của Little Z`[a_i, b_i]`, và các đoạn của Tiểu X`[c_j, d_j]`. Store segments as pairs of integers for easy access.
 2. Khởi tạo bộ đếm`answer`về không. Điều này sẽ theo dõi số lần thức dậy hợp lệ. 
3. Lặp lại mọi số nguyên`t`từ`l`ĐẾN`r`. Đối với mỗi`t`, chúng ta sẽ kiểm tra xem có đoạn nào của Little X bị dịch chuyển bởi`t`trùng lặp với bất kỳ phân đoạn nào của Little Z. 
4. Đối với mỗi`t`, lặp qua tất cả các cặp phân đoạn`(c_j + t, d_j + t)`cho X và`(a_i, b_i)`đối với Z. Tính giá trị lớn nhất ở phần đầu và giá trị nhỏ nhất ở phần cuối. Nếu như`max(c_j + t, a_i) <= min(d_j + t, b_i)`, đánh dấu cái này`t`hợp lệ. 
5. Nếu tìm thấy ít nhất một sự chồng chéo cho hiện tại`t`, tăng`answer`và nghỉ sớm để đến phần tiếp theo`t`. 
6. Sau khi tất cả thí sinh đã được kiểm tra, hãy in`answer`. 

Lý do điều này có tác dụng là vì việc kiểm tra`max(start_X_shifted, start_Z) <= min(end_X_shifted, end_Z)`ghi lại chính xác liệu các khoảng có giao nhau hay không. Vì chúng tôi sẽ ngắt ngay khi tìm thấy một phần chồng chéo trong thời gian thức dậy, nên chúng tôi đảm bảo rằng mỗi phần chồng chéo hợp lệ`t`được tính đúng một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

p, q, l, r = map(int, input().split())
z_segments = [tuple(map(int, input().split())) for _ in range(p)]
x_segments = [tuple(map(int, input().split())) for _ in range(q)]

answer = 0
for t in range(l, r + 1):
    can_chat = False
    for (c, d) in x_segments:
        c_shifted, d_shifted = c + t, d + t
        for (a, b) in z_segments:
            if max(c_shifted, a) <= min(d_shifted, b):
                can_chat = True
                break
        if can_chat:
            break
    if can_chat:
        answer += 1

print(answer)
```Chúng tôi đọc các phân đoạn và phạm vi đánh thức trực tiếp từ đầu vào và lưu trữ chúng dưới dạng danh sách các bộ dữ liệu. Các vòng lặp lồng nhau kiểm tra tất cả các cặp phân đoạn, nhưng các ngắt bên trong đảm bảo chúng tôi dừng các bước kiểm tra không cần thiết sau khi tìm thấy sự chồng chéo. Sự thay đổi được áp dụng nhanh chóng bằng cách sử dụng`c + t`Và`d + t`, giữ mã sạch sẽ. Tất cả các so sánh đều bao gồm các điểm cuối, tránh từng lỗi một. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
1 1 0 4
2 3
0 1
```| t | Đoạn X bị dịch chuyển | Kiểm tra chồng chéo với Z (2-3) | Có thể trò chuyện | 
| --- | --- | --- | --- | 
| 0 | 0-1 | max(0,2)=2 > min(1,3)=1 | Sai | 
| 1 | 1-2 | tối đa(1,2)=2 <= phút(2,3)=2 | Đúng | 
| 2 | 2-3 | max(2,2)=2 <= min(3,3)=3 | Đúng | 
| 3 | 3-4 | max(3,2)=3 <= min(4,3)=3 | Đúng | 
| 4 | 4-5 | max(4,2)=4 > min(5,3)=3 | Sai | 

Thời gian đánh thức hợp lệ là 1, 2, 3, do đó đầu ra là 3. Điều này xác nhận hoạt động xử lý điểm cuối. 

### Ví dụ tùy chỉnh 

đầu vào:```
2 2 0 3
1 2
4 5
0 1
2 3
```| t | X dịch chuyển | Chồng chéo? | 
| --- | --- | --- | 
| 0 | 0-1, 2-3 | chồng chéo với 1-2 (0-1 vs 1-2) -> có | 
| 1 | 1-2, 3-4 | 1-2 trùng nhau 1-2 -> có | 
| 2 | 2-3, 4-5 | 2-3 trùng nhau 1-2? KHÔNG; 2-3 trùng nhau 4-5? KHÔNG; 4-5 chồng chéo 4-5 có | 
| 3 | 3-4, 5-6 | chồng chéo? 3-4 vs 1-2 không; 3-4 vs 4-5 có | 

Thời gian hợp lệ: 0,1,2,3 → đầu ra 4. Điều này thể hiện nhiều phân đoạn và logic ngắt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((r-l+1) * p * q) | Đối với mỗi thời điểm thức dậy, chúng tôi kiểm tra tất cả các cặp phân đoạn X và Z; tối đa 1001 * 50 * 50 séc | 
| Không gian | O(p + q) | Chúng tôi lưu trữ danh sách phân đoạn cho X và Z | 

Độ phức tạp về thời gian nằm trong giới hạn vì 2,5 triệu phép so sánh có thể thực hiện được trong vòng chưa đầy một giây. Việc sử dụng không gian là tối thiểu, chỉ lưu trữ các điểm cuối của phân đoạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    p, q, l, r = map(int, input().split())
    z_segments = [tuple(map(int, input().split())) for _ in range(p)]
    x_segments = [tuple(map(int, input().split())) for _ in range(q)]
    answer = 0
    for t in range(l, r + 1):
        can_chat = False
        for (c, d) in x_segments:
            c_shifted, d_shifted = c + t, d + t
            for (a, b) in z_segments:
                if max(c_shifted, a) <= min(d_shifted, b):
                    can_chat = True
                    break
            if can_chat:
                break
        if can_chat:
            answer += 1
    return str(answer)

# Provided sample
assert run("1 1 0 4\n2 3\n0 1\n") == "3", "sample 1"

# Custom tests
assert run("2 2 0 3\n1 2\n4 5\n0 1\n2 3\n") == "4", "multi-segment overlap"
assert run("1
```
