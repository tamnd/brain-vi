---
title: "CF 105168L - Kết nối Terabyte"
description: "Chúng ta được cung cấp một tập hợp các đoạn tệp độc lập hoạt động giống như các tác vụ khởi động bị trì hoãn. Mỗi đoạn sẽ có sẵn để kết nối tại một thời điểm cụ thể $pi$ và khi chúng tôi bắt đầu tải xuống đoạn đó, sẽ mất chính xác $ti$ giây để hoàn tất."
date: "2026-06-27T09:07:11+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105168
codeforces_index: "L"
codeforces_contest_name: "2024 Fujian Normal University Programming Contest"
rating: 0
weight: 105168
solve_time_s: 53
verified: true
draft: false
---

[CF 105168L - Kết nối Terabyte](https://codeforces.com/problemset/problem/105168/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 53s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp các đoạn tệp độc lập hoạt động giống như các tác vụ khởi động bị trì hoãn. Mỗi đoạn sẽ có sẵn để kết nối tại một thời điểm cụ thể$p_i$và khi chúng tôi bắt đầu tải xuống đoạn đó, phải mất chính xác$t_i$giây để kết thúc. Chúng tôi chỉ được phép bắt đầu tải xuống một đoạn sau thời gian kết nối của nó và chúng tôi không cần tải xuống các đoạn một cách tuần tự theo bất kỳ thứ tự cụ thể nào; mỗi đoạn tiến triển độc lập sau khi nó “có sẵn” để bắt đầu. 

Hai đại lượng phải được tính toán cho mỗi trường hợp thử nghiệm. Lần đầu tiên là thời điểm sớm nhất khi tất cả các khối được kết nối ít nhất một lần, đơn giản là thời điểm đến cuối cùng trong số tất cả các khối.$p_i$. Thời điểm thứ hai là thời điểm sớm nhất khi tất cả các đoạn tải xuống hoàn tất, giả sử rằng mỗi đoạn bắt đầu tải xuống ngay lập tức tại thời điểm kết nối của nó. 

Kích thước đầu vào có thể đạt tới$2 \cdot 10^5$khối trên mỗi tệp thử nghiệm trên các trường hợp thử nghiệm. Điều này ngay lập tức loại trừ mọi phương pháp mô phỏng thời gian từng giây hoặc duy trì mô phỏng dựa trên thời gian. Thậm chí$O(n^2)$lý luận cho mỗi trường hợp kiểm thử sẽ quá chậm; chúng tôi cần một$O(n)$quét cho mỗi trường hợp thử nghiệm. 

Một vấn đề tế nhị sẽ xuất hiện nếu người ta giải thích vấn đề theo cách yêu cầu các quyết định về lịch trình. Vì các khối không cạnh tranh về băng thông hoặc tài nguyên nên không có sự tương tác giữa chúng. Mỗi đoạn thực sự là một khoảng độc lập bắt đầu từ$p_i$và kết thúc tại$p_i + t_i$. Số lượng toàn cầu duy nhất là thời gian bắt đầu tối đa và thời gian kết thúc tối đa. 

Các trường hợp biên phát sinh khi nhiều khối chia sẻ cùng thời gian kết nối hoặc khi một khối có$p_i$có một cái rất lớn$t_i$. Ví dụ, nếu chúng ta có$p = [1, 100]$Và$t = [1000, 1]$, kết nối đầy đủ sớm nhất là 100, nhưng thời gian hoàn thành cuối cùng là 1001. Một cách tiếp cận sai lầm ngây thơ có thể nhầm tưởng rằng thời gian kết nối cuối cùng chiếm ưu thế trong việc hoàn thành, bỏ qua các lượt tải xuống dài đã bắt đầu trước đó. 

## Phương pháp tiếp cận 

Giải thích bạo lực sẽ mô phỏng từng đoạn một cách độc lập theo thời gian, nâng cao dòng thời gian và theo dõi khi mỗi đoạn bắt đầu hoạt động và hoàn thành. Người ta có thể tưởng tượng việc lặp lại thời gian từ cái nhỏ nhất$p_i$đến thời điểm hoàn thành lớn nhất và cập nhật trạng thái khi các khối bắt đầu và kết thúc. Điều này đúng về mặt khái niệm vì vòng đời của mỗi khối rất đơn giản nhưng nó hoàn toàn không khả thi trong trường hợp xấu nhất. Nếu như$p_i$Và$t_i$lớn, trục thời gian kéo dài tới$10^9$và thậm chí các sự kiện nén vẫn để lại$O(n \log n)$hoặc tệ hơn là chi phí chung nếu chúng ta cố gắng mô phỏng sự kiện. 

Quan sát chính là không có sự tương tác giữa các khối. Mỗi đoạn đóng góp chính xác hai giá trị: thời điểm bắt đầu của nó$p_i$và thời điểm kết thúc của nó$p_i + t_i$. Câu trả lời chung cho việc hoàn thành kết nối chỉ đơn giản là thời gian bắt đầu tối đa, vì đó là thời điểm cuối cùng mà bất kỳ đoạn nào có sẵn. Câu trả lời chung cho việc hoàn thiện là thời gian tối đa của tất cả các thời gian hoàn thành được tính toán, vì mỗi đoạn chạy độc lập. 

Khi điều này được nhận ra, vấn đề sẽ giảm xuống còn một lần quét tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(max(p_i + t_i)) | O(n) | Quá chậm | 
| Quét tuyến tính | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập và tính toán hai giá trị cực đại trong một lần duyệt qua các khối. 

1. Khởi tạo hai biến$P = 0$Và$F = 0$, sẽ theo dõi thời gian kết nối tối đa và thời gian hoàn thiện tối đa tương ứng. Chúng đại diện cho sự tập hợp toàn cầu trong các khoảng thời gian độc lập. 
2. Đối với mỗi đoạn$i$, đọc$p_i$Và$t_i$. Cập nhật$P$BẰNG$P = \max(P, p_i)$. Bước này đảm bảo chúng tôi luôn giữ lại khoảnh khắc mới nhất mà bất kỳ đoạn nào có sẵn. 
3. Tính thời gian hoàn thành của đoạn hiện tại là$p_i + t_i$, và cập nhật$F = \max(F, p_i + t_i)$. Điều này cho thấy thực tế là mỗi đoạn đóng góp một thời điểm hoàn thành của ứng viên một cách độc lập với các đoạn khác. 
4. Sau khi xử lý tất cả các chunk, xuất ra$P$Và$F$. 

Thứ tự cập nhật không quan trọng vì mỗi đoạn là độc lập; chúng tôi chỉ tổng hợp các cực trị trên một tập hợp các giá trị dẫn xuất. 

### Tại sao nó hoạt động 

Mỗi đoạn xác định một khoảng xác định$[p_i, p_i + t_i]$. Thời điểm tất cả các khối được kết nối là điểm cuối bên trái tối đa trong số các khoảng này. Thời điểm tất cả các khối được hoàn thành là điểm cuối bên phải tối đa. Vì không có sự phụ thuộc hoặc hạn chế về tài nguyên được chia sẻ nên không có đoạn nào có thể trì hoãn phần khác và không có sự sắp xếp lại nào ảnh hưởng đến bất kỳ khoảng thời gian nào. Do đó, vấn đề giảm chính xác xuống mức tính toán cực đại trên các điểm cuối khoảng, bất biến theo bất kỳ thứ tự xử lý nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    P = 0
    F = 0
    for _ in range(n):
        p, ti = map(int, input().split())
        P = max(P, p)
        F = max(F, p + ti)
    print(P, F)
```Giải pháp duy trì hai bộ tích lũy trong một lần truyền. Bộ tích lũy đầu tiên theo dõi thời điểm kết nối mới nhất và bộ tích lũy thứ hai theo dõi thời điểm hoàn thành mới nhất được hình thành bằng cách thêm thời lượng cho mỗi thời gian bắt đầu. 

Chi tiết triển khai tinh tế duy nhất là đảm bảo rằng việc bổ sung$p + t$được tính toán trên mỗi đoạn trước khi lấy mức tối đa. Bất kỳ nỗ lực nào nhằm tách các hoạt động này hoặc sử dụng lại các giá trị từng phần trong các lần lặp sẽ phá vỡ tính chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 3
chunks = (1,1), (3,2), (2,3)
```Chúng tôi theo dõi$P$Và$F$từng bước một. 

| Đoạn | p | t | p + t | P | F | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 2 | 1 | 2 | 
| 2 | 3 | 2 | 5 | 3 | 5 | 
| 3 | 2 | 3 | 5 | 3 | 5 | 

Kết quả cuối cùng là$P = 3$,$F = 5$. 

Dấu vết này cho thấy các phần bắt đầu sớm không xác định thời gian hoàn thành; thay vào đó, các đoạn dài chiếm ưu thế$F$, trong khi thời gian kết nối cuối cùng sẽ xác định$P$. 

### Ví dụ 2 

đầu vào:```
n = 4
chunks = (10,1), (2,100), (7,3), (5,50)
```| Đoạn | p | t | p + t | P | F | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 10 | 1 | 11 | 10 | 11 | 
| 2 | 2 | 100 | 102 | 10 | 102 | 
| 3 | 7 | 3 | 10 | 10 | 102 | 
| 4 | 5 | 50 | 55 | 10 | 102 | 

Kết quả cuối cùng là$P = 10$,$F = 102$. 

Ví dụ này chứng minh rằng một đoạn có thời gian bắt đầu rất sớm nhưng thời lượng lớn có thể chiếm ưu thế trong thời gian hoàn thiện cuối cùng mặc dù nó không ảnh hưởng đến kết nối tối đa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi đoạn được xử lý một lần với công việc liên tục | 
| Không gian | O(1) | Chỉ có hai cực đại đang chạy được lưu trữ | 

Tổng cộng$n$trên các trường hợp thử nghiệm được giới hạn bởi$2 \cdot 10^5$, do đó, giải pháp thực hiện một lần quét tuyến tính tổng thể, dễ dàng phù hợp với giới hạn thời gian thông thường là 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        P = 0
        F = 0
        for _ in range(n):
            p, ti = map(int, input().split())
            P = max(P, p)
            F = max(F, p + ti)
        out.append(f"{P} {F}")
    return "\n".join(out)

# provided sample
assert run("""1
3
1 1
3 2
2 3
""") == "3 5"

# minimum size
assert run("""1
1
5 7
""") == "5 12"

# all equal
assert run("""1
3
4 4
4 4
4 4
""") == "4 8"

# mixed values
assert run("""1
4
10 1
2 100
7 3
5 50
""") == "10 102"

# large dominance of early finish
assert run("""1
2
1 1000
999 1
""") == "999 1001"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đoạn đơn | hành vi tối đa tầm thường | trường hợp cơ sở đúng đắn | 
| khối giống hệt nhau | ổn định dưới sự trùng lặp | không phụ thuộc vào đơn hàng | 
| giá trị hỗn hợp | thời gian dài chiếm ưu thế F | tính đúng đắn của phép gộp p+t | 
| bắt đầu sai lệch | tách P và F | hành vi cực đại độc lập | 

## Vỏ cạnh 

Hãy xem xét trường hợp đoạn đầu tiên có thời gian tải xuống lâu nhất:```
1
2
1 1000
999 1
```Quá trình xử lý tiến hành như sau. Ban đầu$P = 0$,$F = 0$. Sau miếng đầu tiên,$P = 1$,$F = 1001$. Sau miếng thứ hai,$P = 999$,$F = 1001$. Câu trả lời cuối cùng là$P = 999$,$F = 1001$. 

Điều này xác nhận rằng đoạn đóng góp thời gian hoàn thành tối đa không cần phải là đoạn cuối cùng kết nối. Thuật toán phân tách chính xác hai cực trị độc lập: một trên điểm bắt đầu và một trên điểm cuối được tính toán.
