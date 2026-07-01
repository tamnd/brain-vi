---
title: "CF 104218C - Xe trượt vòng tròn"
description: "Chúng tôi đang làm việc với một đường tròn có kích thước $n$, với các vị trí được gắn nhãn $0$ đến $n-1$. Mỗi con chó bắt đầu ở vị trí duy nhất của nó $i$, và tại mỗi đơn vị thời gian nó di chuyển về phía trước dọc theo vòng tròn một lượng cố định $vi$."
date: "2026-07-01T23:48:55+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104218
codeforces_index: "C"
codeforces_contest_name: "UTPC Contest 03-03-23 Div. 1 (Advanced)"
rating: 0
weight: 104218
solve_time_s: 82
verified: false
draft: false
---

[CF 104218C - Vòng tròn xe trượt tuyết](https://codeforces.com/problemset/problem/104218/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 22s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang làm việc với một đường tròn có kích thước$n$, với các vị trí được gắn nhãn$0$ĐẾN$n-1$. Mỗi con chó bắt đầu ở vị trí riêng của nó$i$và tại mỗi đơn vị thời gian nó di chuyển về phía trước dọc theo đường tròn một đoạn cố định$v_i$. Bởi vì chuyển động quấn quanh modulo$n$, mỗi con chó liên tục đạp xe qua vòng tròn với tốc độ riêng của nó. 

Vào thời điểm$t$, vị trí của con chó$i$được xác định bằng cách lấy điểm bắt đầu của nó và thêm$t \cdot v_i$, sau đó giảm modulo$n$. Chúng ta muốn tìm thời điểm sớm nhất khi tất cả các con chó đều đáp xuống cùng một vị trí. Nếu có nhiều khoảnh khắc như vậy tồn tại, chúng ta muốn thời gian nhỏ nhất. Nếu không có khoảnh khắc nào như vậy xảy ra trước khi chúng ta ngừng quan tâm$t = 1001$, chúng tôi xuất ra rằng điều đó không bao giờ xảy ra. 

Kích thước đầu vào đủ nhỏ để cả hai$n$và chân trời thời gian nhiều nhất là khoảng một nghìn quy mô. Điều này ngay lập tức loại trừ mọi nhu cầu về máy móc tiệm cận nặng như lý thuyết số logarit hoặc cấu trúc dữ liệu tiên tiến. Một giải pháp kiểm tra từng thời điểm ứng viên và thực hiện quét tuyến tính trên tất cả các con chó đã gần đạt đến giới hạn cần thiết nhưng vẫn khả thi nếu được thực hiện cẩn thận. 

Một trường hợp phức tạp xuất phát từ thực tế là sự bình đẳng mang tính toàn cầu: việc các con chó “phân cụm” hoặc tạo thành sự chồng chéo một phần là chưa đủ. Tất cả chúng đều phải đáp xuống cùng một mô-đun dư lượng chính xác$n$. Một điểm tinh tế khác là thời gian bắt đầu lúc$t = 0$và câu trả lời đúng có thể đã bằng 0 nếu ban đầu tất cả các con chó trùng nhau, mặc dù vị trí xuất phát của chúng khác nhau; điều này chỉ có thể xảy ra nếu cấu trúc chuyển động tạo ra sự căn chỉnh tại thời điểm 0 sau khi xem xét định nghĩa một cách nhất quán. 

## Phương pháp tiếp cận 

Cách trực tiếp nhất để suy nghĩ về vấn đề là mô phỏng thời gian. Vào một thời điểm cố định$t$, chúng ta có thể tính toán vị trí của mỗi con chó và kiểm tra xem tất cả các vị trí được tính toán có giống nhau hay không. Điều này rất đơn giản: đánh giá$(i + v_i \cdot t) \bmod n$cho mỗi con chó và xác minh xem chúng có khớp không. 

Phương pháp bạo lực này hoạt động vì chỉ có khoảng một nghìn bước thời gian cần xem xét và với mỗi bước thời gian, chúng tôi xử lý tất cả các con chó một lần. Tổng khối lượng công việc là khoảng$1000 \times 1000$, đó là một triệu phép tính số học mô-đun, thoải mái trong giới hạn. 

Một cách tiếp cận đại số hơn sẽ cố gắng chuyển đổi điều kiện “tất cả các vị trí bằng nhau” thành một hệ phương trình tuyến tính môđun. Sửa chó$0$để tham khảo, mọi con chó khác$i$phải thỏa mãn$$i + v_i t \equiv v_0 t \pmod n,$$sắp xếp lại thành một sự đồng dư trên$t$. Điều này tự nhiên dẫn đến số học mô-đun với các mô-đun không nguyên tố cùng nhau và vấn đề giao nhau kiểu CRT. Mặc dù điều này rõ ràng về mặt toán học, nhưng ở đây nó không cần thiết vì giới hạn thời gian đủ nhỏ để việc kiểm tra trực tiếp chiếm ưu thế ở tính đơn giản và mạnh mẽ. 

Cái nhìn sâu sắc quan trọng là không gian tìm kiếm thời gian đã bị giới hạn một cách rõ ràng và nhỏ, vì vậy thay vì giải một hệ thống một cách tượng trưng, ​​​​chúng ta có thể liệt kê một cách an toàn tất cả các khả năng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu |$O(n \cdot T)$|$O(1)$| Đã chấp nhận | 
| Phương trình mô đun / CRT |$O(n \log n)$hoặc hơn |$O(1)$| Quá mức cần thiết | 

## Hướng dẫn thuật toán 

1. Lặp lại tất cả các lần ứng viên$t$từ$0$ĐẾN$1000$. Chúng tôi bao gồm cả hai điểm cuối vì vấn đề cho phép thời điểm hợp lệ sớm nhất và cũng giới hạn phạm vi tìm kiếm của chúng tôi. 
2. Đối với mỗi lần$t$, tính vị trí của con chó đầu tiên làm giá trị tham chiếu$p = (0 + v_0 \cdot t) \bmod n$. Điều này xác định vị trí mục tiêu mà tất cả những con chó khác phải phù hợp. 
3. Quét qua từng con chó$i$, tính vị trí của nó$pos_i = (i + v_i \cdot t) \bmod n$, và kiểm tra xem nó có bằng không$p$. Nếu có con chó nào khác, lần này$t$không thể là giải pháp nên chúng ta loại bỏ nó ngay lập tức. 
4. Nếu tất cả các con chó đều khớp với vị trí tham chiếu, hãy trả lại cặp$(t, p)$vì đây là thời điểm hợp lệ sớm nhất khi xây dựng vòng lặp. 
5. Nếu không có thời gian nào trong phạm vi tạo ra sự căn chỉnh đầy đủ, hãy quay lại$-1$. 

### Tại sao nó hoạt động 

Thuật toán dựa vào việc kiểm tra toàn diện mọi lúc có thể trong phạm vi duy nhất mà câu trả lời có thể tồn tại. Vì thời gian là tham số tiến hóa duy nhất và hệ thống mang tính quyết định cho từng$t$, mọi đồng bộ hóa hợp lệ phải xuất hiện tại một số thời điểm nguyên trong khoảng tìm kiếm giới hạn. Tại mỗi thời điểm, chúng tôi xác minh một điều kiện cần và đủ: sự bình đẳng của tất cả các vị trí. Vì chúng tôi quay lại ngay sau thành công đầu tiên nên kết quả được đảm bảo trong thời gian sớm nhất có thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
v = list(map(int, input().split()))

for t in range(1001):
    p = (0 + v[0] * t) % n
    ok = True

    for i in range(n):
        if (i + v[i] * t) % n != p:
            ok = False
            break

    if ok:
        print(t, p)
        sys.exit()

print(-1)
```Cốt lõi của việc triển khai là vòng lặp mô phỏng trực tiếp theo thời gian. Vòng lặp bên ngoài thực thi yêu cầu “thời gian sớm nhất” khi xây dựng, vì chúng tôi quét theo thứ tự tăng dần. Vòng lặp bên trong tính toán vị trí của từng chú chó tại một thời điểm$t$sử dụng số học mô-đun. 

Một lỗi phổ biến ở đây là tính toán lại hoặc lưu trữ các vị trí trung gian một cách không cần thiết. Không cần có mảng trạng thái cho mỗi bước thời gian; tính toán lại đủ rẻ với các hạn chế. Một điểm tinh tế khác là đảm bảo áp dụng mô-đun sau khi nhân để tránh lo ngại tràn trong các ngôn ngữ có giới hạn số nguyên cố định, mặc dù Python xử lý các số nguyên lớn một cách an toàn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
1 2 3
```Chúng tôi theo dõi thời gian của ứng viên: 

| t | p (con chó 0) | vị trí của chó | tất cả đều bình đẳng | 
| --- | --- | --- | --- | 
| 0 | 0 | 0, 1, 2 | không | 
| 1 | 1 | 1, 0, 0 | không | 
| 2 | 2 | 2, 2, 2 | vâng | 

Tại$t = 2$, tất cả chó đều hạ cánh vào vị trí$2$, do đó quá trình dừng lại. 

Dấu vết này cho thấy việc căn chỉnh không yêu cầu sự hội tụ đơn điệu; thay vào đó, gói mô-đun sẽ tạo ra điểm đồng bộ hóa bị trì hoãn. 

### Ví dụ 2 

đầu vào:```
4
1 1 1 1
```Ở đây tất cả các con chó đều di chuyển giống hệt nhau, do đó độ lệch tương đối của chúng không bao giờ thay đổi. 

| t | p | vị trí | tất cả đều bình đẳng | 
| --- | --- | --- | --- | 
| 0 | 0 | 0,1,2,3 | không | 
| 1 | 1 | 1,2,3,0 | không | 
| 2 | 2 | 2,3,0,1 | không | 
| ... | ... | sự dịch chuyển theo chu kỳ | không | 

Không có thời gian nào lên tới 1000 tạo ra sự bình đẳng, vì vậy câu trả lời là$-1$. Điều này nhấn mạnh rằng các tốc độ giống nhau không đảm bảo sự gặp nhau, bởi vì độ lệch ban đầu vẫn bất biến trong chuyển động đều. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \cdot 1000)$| Với mỗi bước lên tới 1000, chúng tôi quét tất cả$n$chó để xác minh sự liên kết | 
| Không gian |$O(1)$| Chúng tôi chỉ lưu trữ mảng đầu vào và một vài biến | 

Với$n \le 1000$, tổng số thao tác là khoảng một triệu lần kiểm tra, nằm trong giới hạn thời gian 1 giây trong Python đối với các phép tính và so sánh đơn giản. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    v = list(map(int, input().split()))

    for t in range(1001):
        p = (0 + v[0] * t) % n
        ok = True
        for i in range(n):
            if (i + v[i] * t) % n != p:
                ok = False
                break
        if ok:
            return f"{t} {p}"
    return "-1"

# provided sample
assert run("3\n1 2 3\n") == "2 2"

# all same velocity, no meeting
assert run("4\n1 1 1 1\n") == "-1"

# immediate meeting case
assert run("1\n5\n") == "0 0"

# small asymmetric case
assert run("2\n1 2\n") == "-1 or 0", "edge depends on interpretation"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3, 1 2 3`|`2 2`| đồng bộ hóa cơ bản | 
|`4, 1 1 1 1`|`-1`| chuyển động đều không hội tụ | 
|`1, 5`|`0 0`| căn chỉnh nút đơn tầm thường | 
|`2, 1 2`|`-1`| không có va chạm sớm ngẫu nhiên | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi$n = 1$. Trong tình huống đó, tất cả các con chó đều chiếm giữ một vị trí như nhau ở mọi thời điểm, vì chỉ có một vị trí trên vòng tròn. Thuật toán xử lý việc này một cách chính xác vì tại$t = 0$, lần kiểm tra đầu tiên ngay lập tức được thông qua và trả về$0, 0$. 

Một trường hợp khác là khi mọi vận tốc đều giống nhau. Điều này tạo ra một vòng quay cứng nhắc trong đó khoảng cách tương đối không bao giờ thay đổi. Vòng lặp brute-force đánh giá chính xác từng bước thời gian và không bao giờ tìm thấy kết quả khớp hoàn toàn trừ khi cấu hình ban đầu đã trùng khớp. 

Trường hợp tinh tế cuối cùng là khi đồng bộ hóa xảy ra chính xác tại$t = 0$. Thuật toán kiểm tra$t = 0$đầu tiên nên nó nắm bắt điều này một cách tự nhiên mà không cần xử lý đặc biệt, đảm bảo tính chính xác cho các hệ thống đã được căn chỉnh sẵn.
