---
title: "CF 979A - Pizza, Pizza, Pizza!!!"
description: "Chúng ta có một số người bạn tham dự một bữa tiệc pizza và người chủ trì phải chia một chiếc bánh pizza hình tròn thành những lát bằng nhau sao cho mỗi người có một lát, kể cả chủ nhà. Nếu có $n$ người bạn, chiếc bánh pizza phải được chia thành $n + 1$ phần bằng nhau."
date: "2026-06-17T01:18:45+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 979
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 482 (Div. 2)"
rating: 1000
weight: 979
solve_time_s: 66
verified: true
draft: false
---

[CF 979A - Pizza, Pizza, Pizza!!!](https://codeforces.com/problemset/problem/979/A) 

**Đánh giá:** 1000 
**Thẻ:** toán 
**Thời gian giải:** 1 phút 6s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một số người bạn tham dự một bữa tiệc pizza và người chủ trì phải chia một chiếc bánh pizza hình tròn thành những lát bằng nhau sao cho mỗi người có một lát, kể cả chủ nhà. Nếu có$n$các bạn, pizza phải được chia thành$n + 1$phần bằng nhau. 

Thao tác được phép là cắt thẳng trên chiếc bánh pizza và mỗi lần cắt góp phần tạo thành các phần bằng nhau này. Mục tiêu là giảm thiểu số lượng vết cắt thẳng như vậy được yêu cầu để tạo ra$n + 1$lát giống hệt nhau. 

Đầu vào là một số nguyên duy nhất$n$, có thể lớn bằng$10^{18}$, điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào mô phỏng các vết cắt hoặc xây dựng hình học một cách rõ ràng. Bất kỳ giải pháp nào cũng phải giảm vấn đề thành một phép tính toán học trực tiếp. 

Một vài trường hợp đặc biệt tiết lộ cấu trúc của vấn đề. Khi$n = 0$, chỉ có một người nên không cần cắt. Khi$n = 3$, chúng ta cần 4 lát bằng nhau, cần 2 đường cắt thẳng qua tâm. Khi$n = 4$, chúng ta cần 5 lát bằng nhau và mẫu gợi ý 5 vết cắt, cho biết tính chất chẵn lẻ hoặc tính chia hết là quan trọng. Giả định ngây thơ rằng mỗi lần cắt luôn tăng số lát cắt lên một lượng cố định sẽ không thành công, bởi vì các vết cắt hoạt động khác nhau tùy thuộc vào việc chúng có giao nhau với tâm hay không và cách chúng được sắp xếp. 

## Phương pháp tiếp cận 

Cách giải thích bạo lực sẽ cố gắng mô phỏng việc đặt các đường cắt thẳng và theo dõi xem vòng tròn được chia thành bao nhiêu vùng. Mỗi vết cắt mới có thể giao nhau với tất cả các vết cắt trước đó, làm tăng số lượng lát cắt một cách không cần thiết. Người ta có thể thử mô hình hóa điều này theo từng bước: bắt đầu với một mảnh, sau đó thử tất cả các hướng cắt có thể có và đếm xem có bao nhiêu vùng được hình thành. 

Vấn đề là mỗi vết cắt có thể giao nhau với tất cả các vết cắt trước đó, vì vậy sau$k$vết cắt, số lượng mảnh có thể tăng bậc hai đối với$k$. Cố gắng mô phỏng hoặc tìm kiếm cấu hình tối thiểu mang lại kết quả chính xác$n + 1$các khu vực góc bằng nhau nhanh chóng trở nên khó điều khiển, đặc biệt là vì$n$có thể lên tới$10^{18}$. 

Quan sát quan trọng là chúng ta không thực sự tối ưu hóa hình học theo nghĩa liên tục, mà thay vào đó, phù hợp với một cấu trúc tổ hợp rất cứng nhắc: các phân chia góc bằng nhau của một hình tròn. Các cấu hình có ý nghĩa duy nhất đến từ việc vẽ đường kính qua tâm hoặc sử dụng một cấu trúc đặc biệt trong đó tất cả các vết cắt được sắp xếp đối xứng giống như nan hoa của bánh xe. Điều này làm giảm vấn đề khi hỏi cần bao nhiêu đường cắt thẳng đối xứng để tạo thành$n + 1$các góc bằng nhau quanh một đường tròn. 

Từ cấu trúc này, câu trả lời rút gọn thành một quy tắc chẵn lẻ đơn giản. Nếu như$n + 1$Thật kỳ quặc, chúng ta không thể ghép các lát cắt một cách đối xứng bằng cách sử dụng các đường cắt đường kính, vì vậy chúng ta phải vẽ một bộ đầy đủ các đường cắt xuyên tâm, mỗi đường cắt một cung. Nếu như$n + 1$chẵn, mỗi đường thẳng đi qua tâm sẽ tạo ra hai lát cắt đối diện nhau, do đó mỗi vết cắt đóng góp hiệu quả hơn. 

Điều này dẫn đến một công thức trực tiếp dựa trên việc liệu$n + 1$là chẵn hoặc lẻ, điều này giúp đơn giản hóa hơn nữa việc kiểm tra xem$n$là chẵn hoặc lẻ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | Hàm mũ | O(1) | Quá chậm | 
| Công thức dựa trên tính chẵn lẻ | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên$n$, đại diện cho số lượng bạn bè. 
2. Tính toán$k = n + 1$, số lát pizza bằng nhau cần thiết. 
3. Nếu$n = 0$, xuất 0 ngay lập tức vì một lát cắt không cần cắt. 
4. Nếu$k = 2$, đầu ra 1 vì một đường cắt thẳng sẽ chia chiếc bánh pizza thành hai nửa bằng nhau. 
5. Nếu$k$chẵn, đầu ra$k // 2$, vì mỗi vết cắt giống đường kính sẽ tạo ra hai lát cắt đối diện bằng nhau. 
6. Nếu$k$là số lẻ và lớn hơn 1, xuất ra$k$, vì mỗi lát cắt phải được hình thành độc lập bởi một ranh giới xuyên tâm. 

Lý do đằng sau trường hợp chẵn xuất phát từ tính đối xứng: một đường thẳng đi qua tâm luôn tạo ra đúng hai nửa bằng nhau và nhiều đường như vậy có thể được sắp xếp để chia hình tròn thành các cặp cung đối diện. Khi số lượng lĩnh vực bằng nhau, cấu trúc ghép nối này hoàn toàn phù hợp. Khi nó là số lẻ, người ta không thể ghép tất cả các lĩnh vực, buộc phải có một cấu hình suy biến trong đó mỗi ranh giới khu vực phải được giới thiệu rõ ràng. 

### Tại sao nó hoạt động 

Vòng tròn chỉ có thể được chia thành các vùng góc bằng nhau bằng cách sử dụng các đường thẳng có cấu hình tôn trọng tính đối xứng quay. Một đường cắt thẳng luôn tạo ra một đường đối xứng phân chia các vùng góc hiện có thành từng cặp. Điều này có nghĩa là các công trình hợp lệ phải phù hợp với cấu trúc ghép nối xung quanh tâm. Khi số lượng lát cắt yêu cầu là chẵn, mỗi lát cắt có thể được ghép với một lát cắt đối diện, như vậy mỗi lát cắt sẽ đóng góp tối đa. Khi nó là số lẻ, không có cặp đôi hoàn chỉnh nào tồn tại, do đó cấu trúc suy biến thành một vết cắt trên mỗi hướng biên, buộc phải tăng trưởng tuyến tính. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input().strip())
k = n + 1

if k == 1:
    print(0)
elif k == 2:
    print(1)
elif k % 2 == 0:
    print(k // 2)
else:
    print(k)
```Mã trực tiếp thực hiện quy tắc dựa trên tính chẵn lẻ xuất phát ở trên. Hai điều kiện đầu tiên xử lý các trường hợp tầm thường trong đó không cần suy luận hình học. Nhánh trường hợp chẵn mã hóa đối số ghép nối, trong khi nhánh trường hợp lẻ phản ánh sự thiếu đối xứng, buộc phải cắt một hướng ranh giới trên mỗi lát cắt. 

Phải cẩn thận với$k = 1$trường hợp này, vì không cần cắt khi chỉ có một lát. Một sự tinh tế khác là việc phân biệt$k = 2$từ trường hợp chẵn tổng quát, vì công thức$k // 2$mặt khác sẽ đề xuất không chính xác một lý do phân số nếu áp dụng sai mà không giải thích. 

## Ví dụ đã hoạt động 

### Ví dụ 1:$n = 3$Chúng tôi cần$k = 4$lát. 

| Bước | n | k = n+1 | chẵn lẻ | quyết định | 
| --- | --- | --- | --- | --- | 
| 1 | 3 | 4 | thậm chí | đầu ra k/2 = 2 | 

Điều này phù hợp với ý tưởng hai đường cắt vuông góc xuyên qua tâm tạo thành bốn góc phần tư bằng nhau. 

### Ví dụ 2:$n = 4$Chúng tôi cần$k = 5$lát. 

| Bước | n | k = n+1 | chẵn lẻ | quyết định | 
| --- | --- | --- | --- | --- | 
| 1 | 4 | 5 | lẻ | đầu ra k = 5 | 

Điều này phản ánh rằng việc ghép nối đối xứng là không thể, vì vậy mỗi ranh giới khu vực phải được đưa ra một cách độc lập. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ thực hiện số học và một số kiểm tra có điều kiện | 
| Không gian | O(1) | Không sử dụng cấu trúc dữ liệu phụ trợ | 

Giải pháp dễ dàng phù hợp với các ràng buộc vì nó thực hiện tính toán theo thời gian không đổi ngay cả đối với các giá trị lên tới$10^{18}$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    input = _sys.stdin.readline
    n = int(input().strip())
    k = n + 1

    if k == 1:
        return "0"
    elif k == 2:
        return "1"
    elif k % 2 == 0:
        return str(k // 2)
    else:
        return str(k)

# provided sample
assert run("3\n") == "2"

# custom cases
assert run("0\n") == "0", "single person needs no cut"
assert run("1\n") == "1", "two slices require one cut"
assert run("2\n") == "3", "odd slice count behavior"
assert run("999999999999999999\n") == str(500000000000000000), "large even k case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 0 | 0 | trường hợp cạnh tối thiểu | 
| 1 | 1 | cắt không tầm thường nhỏ nhất | 
| 2 | 3 | hành vi cấu trúc kỳ quặc | 
| lớn | k//2 | hiệu suất và an toàn tràn | 

## Vỏ cạnh 

cho$n = 0$, chúng tôi có$k = 1$. Thuật toán ngay lập tức trả về 0, tương ứng với việc không cần cắt một lát nào. 

Vì$n = 1$, chúng tôi có$k = 2$. Điều kiện trả về rõ ràng là 1, khớp với một đường kính cắt thẳng. 

Đối với các giá trị chẵn lớn, chẳng hạn như$n = 10^{18} - 1$, phép tính giảm xuống thành phép chia số nguyên của$k$bằng 2, vẫn an toàn trong Python do các số nguyên có độ chính xác tùy ý. 

Đối với các giá trị lẻ như$n = 4$, Ở đâu$k = 5$, nhánh dự phòng trả về$k$, phản ánh rằng không có cấu trúc ghép nối nào tồn tại và mỗi hướng lát cắt phải được thực hiện độc lập.
