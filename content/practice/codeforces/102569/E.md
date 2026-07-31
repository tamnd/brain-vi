---
title: "CF 102569E - Biến động của Mana"
description: "Cuộc hành trình bao gồm việc đi thăm các nguồn ma thuật theo một thứ tự cố định. Mỗi nguồn thay đổi lượng mana hiện tại của pháp sư theo một lượng nhất định và điều kiện thất bại duy nhất là mức mana giảm xuống dưới 0 bất cứ lúc nào."
date: "2026-07-31T07:49:00+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102569
codeforces_index: "E"
codeforces_contest_name: "2020, XIII Samara Regional Intercollegiate Programming Contest"
rating: 0
weight: 102569
solve_time_s: 79
verified: true
draft: false
---

[CF 102569E - Biến động của Mana](https://codeforces.com/problemset/problem/102569/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 19s 
**Đã xác minh:** có 

##Giải pháp 
#Hiểu vấn đề 

Cuộc hành trình bao gồm việc đi thăm các nguồn ma thuật theo một thứ tự cố định. Mỗi nguồn thay đổi lượng mana hiện tại của pháp sư theo một lượng nhất định và điều kiện thất bại duy nhất là mức mana giảm xuống dưới 0 bất cứ lúc nào. Nhiệm vụ là xác định giá trị năng lượng ban đầu nhỏ nhất cho phép pháp sư sống sót trong toàn bộ chuỗi. 

Mảng đầu vào mô tả những thay đổi về năng lượng do các nguồn gây ra. Giá trị dương làm tăng năng lượng hiện tại, giá trị âm tiêu thụ năng lượng và giá trị 0 không thay đổi năng lượng. Đầu ra không phải là năng lượng cuối cùng sau hành trình mà là lượng năng lượng ban đầu tối thiểu cần thiết để mọi tiền tố của hành trình giữ cho năng lượng không âm. 

Giới hạn lên tới 500000 nguồn có nghĩa là giải pháp phải xử lý mảng theo thời gian tuyến tính. Một cách tiếp cận thử nhiều giá trị bắt đầu có thể có hoặc mô phỏng lặp đi lặp lại hành trình sẽ thực hiện quá nhiều thao tác. Với giới hạn 2 giây, một thuật toán xoay quanh O(n) hoặc O(n log n) được mong đợi, trong khi các phương pháp tiếp cận O(n²) vượt xa phạm vi khả thi. 

Một số trường hợp thường phá vỡ các giải pháp không chính xác. Một chuỗi chỉ chứa những thay đổi tích cực có câu trả lời là 0 vì pháp sư không bao giờ mất năng lượng. Ví dụ:```
3
5 2 8
```Đầu ra đúng là:```
0
```Một giải pháp giả định luôn cần một lượng mana ban đầu tích cực sẽ thất bại ở đây. 

Giá trị âm lớn ở đầu phải được xử lý trước khi xem xét các giá trị dương sau này. Ví dụ:```
3
-10 20 -5
```Đầu ra đúng là:```
10
```Nguồn đầu tiên ngay lập tức cần ít nhất 10 mana. Một cách tiếp cận bất cẩn chỉ nhìn vào tổng cuối cùng sẽ thấy rằng tổng thể hành trình đạt được 5 năng lượng và có thể trả lời sai bằng 0. 

Một trường hợp tinh tế khác là khi thâm hụt tồi tệ nhất xảy ra ở giữa chứ không phải ở cuối. Ví dụ:```
5
4 -7 1 1 10
```Đầu ra đúng là:```
3
```Sau hai nguồn đầu tiên, tổng số thay đổi là -3, vì vậy cần có ba nguồn năng lượng ban đầu. Chỉ nhìn vào tổng số tiền là 9 sẽ bỏ lỡ sự sụt giảm tạm thời. 

# Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là đoán mana ban đầu và mô phỏng hành trình. Đối với giá trị bắt đầu cố định, việc kiểm tra xem pháp sư có sống sót hay không mất O(n) thời gian vì mỗi nguồn phải được truy cập một lần. Chúng ta có thể thử tất cả các giá trị bắt đầu có thể từ 0 trở lên cho đến khi có một giá trị hoạt động. Vì câu trả lời có thể lớn bằng tổng của nhiều thay đổi âm nên điều này có thể yêu cầu tới khoảng 500000 × 10^9 phép tính trong trường hợp xấu nhất, điều này là không thể. 

Tìm kiếm nhị phân trên câu trả lời sẽ cải thiện ý tưởng này. Điều kiện đơn điệu: nếu giá trị mana ban đầu hoạt động thì mọi giá trị lớn hơn cũng hoạt động. Điều này đưa ra cách tiếp cận O(n log A), trong đó A là kích thước của phạm vi câu trả lời có thể có. Tuy nhiên, cấu trúc của vấn đề cho phép quan sát thậm chí còn đơn giản hơn. 

Thay vì liên tục hỏi liệu giá trị ban đầu được chọn có đủ hay không, hãy theo dõi sự thay đổi năng lượng. Hãy tưởng tượng bắt đầu với lượng mana bằng 0 và đi theo toàn bộ cuộc hành trình. Tại mọi thời điểm, tổng số hiện có cho chúng ta biết pháp sư sẽ tụt xuống dưới 0 bao xa. Mức giảm lớn nhất xuống dưới 0 chính xác là lượng mana bị thiếu lúc đầu. Việc thêm số tiền còn thiếu đó sẽ dịch chuyển mọi tiền tố lên trên vừa đủ để làm cho tiền tố tối thiểu bằng 0. 

Ví dụ: nếu tổng số đang chạy là:```
3, -1, 1, -2, -4, 3
```giá trị thấp nhất là -4. Bắt đầu với 4 mana sẽ chuyển tổng số này thành:```
7, 3, 5, 2, 0, 7
```Mọi giá trị hiện đều hợp lệ và bất kỳ số tiền ban đầu nhỏ hơn nào vẫn sẽ để lại điểm âm thấp nhất. 

Brute-force hoạt động vì nó kiểm tra xem giá trị bắt đầu có bảo vệ mọi tiền tố của hành trình hay không, nhưng nó lặp lại cùng một mô phỏng nhiều lần. Nhận xét rằng chỉ có tổng tiền tố tối thiểu mới quan trọng cho phép chúng ta tìm ra câu trả lời chỉ trong một lần duyệt. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n × câu trả lời) | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Bắt đầu với số dư năng lượng đang chạy bằng 0 và một biến lưu trữ số dư tối thiểu đạt được cho đến nay. Số dư đang chạy thể hiện điều gì sẽ xảy ra nếu pháp sư bắt đầu với lượng mana bằng 0. 
2. Xử lý từng thay đổi năng lượng theo thứ tự và thêm nó vào số dư hiện hành. Sau mỗi lần thêm, hãy cập nhật số dư tối thiểu nếu giá trị hiện tại nhỏ hơn. 
3. Sau khi toàn bộ chuỗi được xử lý, nếu số dư tối thiểu âm, hãy trả về giá trị tuyệt đối của nó. Nếu số dư tối thiểu bằng 0 hoặc dương, trả về 0. 

Lý do tiền tố tối thiểu là đủ là vì mọi thời điểm nguy hiểm đều xảy ra sau khi một số tiền tố của mảng đã được xử lý. Năng lượng ban đầu cần thiết lớn nhất chính xác là lượng cần thiết để nâng tiền tố thấp nhất về 0. 

Tại sao nó hoạt động: Tổng số mô tả tất cả các điểm có thể xảy ra mà pháp sư có thể chết khi bắt đầu với lượng mana bằng 0. Giả sử tổng số tiền chạy tối thiểu là`-x`. Bất kỳ mana ban đầu nào nhỏ hơn`x`sẽ để điểm đó dưới 0, nên ít nhất`x`mana là cần thiết. Bắt đầu bằng chính xác`x`tăng mỗi tổng số tiền hiện tại lên`x`, làm cho giá trị tối thiểu bằng 0 và tất cả các giá trị khác không âm. Như vậy`x`là vừa cần vừa đủ. 

#Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n_line = input().strip()
    if not n_line:
        return

    n = int(n_line)
    a = list(map(int, input().split()))

    current = 0
    minimum = 0

    for value in a:
        current += value
        if current < minimum:
            minimum = current

    print(-minimum)

if __name__ == "__main__":
    solve()
```Mã chỉ giữ lại hai phần thông tin trong khi quét mảng.`current`là mức năng lượng trong mô phỏng tưởng tượng trong đó pháp sư bắt đầu với năng lượng bằng 0.`minimum`lưu trữ giá trị thấp nhất đạt được bởi mô phỏng đó. 

Bất cứ khi nào số dư chạy trở nên nhỏ hơn, điều đó có nghĩa là pháp sư sẽ cần thêm mana ban đầu để tồn tại ở thời điểm đó. Câu trả lời cuối cùng là số âm của số dư thấp nhất. Từ`minimum`bắt đầu từ số 0, các chuỗi không bao giờ âm sẽ tự động tạo ra câu trả lời bằng 0. 

Số nguyên Python không bị tràn, điều này quan trọng vì tổng 500000 giá trị có độ lớn lên tới 10^9 có thể đạt tới khoảng 5 × 10^14. Thuật toán cũng tránh lưu trữ các tổng tiền tố không cần thiết, giữ mức sử dụng bộ nhớ không đổi. 

# Ví dụ đã hoạt động 

Đối với đầu vào mẫu:```
6
3 -4 2 -3 -2 7
```dấu vết là: 

| Giá trị nguồn | Số dư hiện tại | Số dư tối thiểu | 
| --- | --- | --- | 
| 3 | 3 | 0 | 
| -4 | -1 | -1 | 
| 2 | 1 | -1 | 
| -3 | -2 | -2 | 
| -2 | -4 | -4 | 
| 7 | 3 | -4 | 

Số dư tối thiểu là -4, vì vậy pháp sư cần 4 năng lượng ban đầu. Thêm 4 vào mỗi điểm của hành trình sẽ ngăn cản sự cân bằng trở nên âm. 

Một ví dụ thứ hai:```
5
5 -2 -8 4 10
```| Giá trị nguồn | Số dư hiện tại | Số dư tối thiểu | 
| --- | --- | --- | 
| 5 | 5 | 0 | 
| -2 | 3 | 0 | 
| -8 | -5 | -5 | 
| 4 | -1 | -5 | 
| 10 | 9 | -5 | 

Số dư mô phỏng thấp nhất là -5, do đó, lượng mana ban đầu cần thiết là 5. Ví dụ này cho thấy rằng việc phục hồi lớn ở cuối không giúp ích gì cho điểm nguy hiểm trước đó. 

# Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi nguồn được xử lý chính xác một lần. | 
| Không gian | O(1) | Chỉ có tổng hiện tại và giá trị tiền tố tối thiểu được lưu trữ. | 

Giải pháp dễ dàng phù hợp với các ràng buộc vì nó thực hiện quét tuyến tính duy nhất trên tối đa 500000 giá trị và không phân bổ bộ nhớ tỷ lệ với kích thước đầu vào. 

# Trường hợp thử nghiệm```python
import sys
import io

def solve_io(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)

    import builtins
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))

    current = 0
    minimum = 0

    for x in a:
        current += x
        minimum = min(minimum, current)

    result = str(-minimum)

    sys.stdin = old_stdin
    return result

assert solve_io("6\n3 -4 2 -3 -2 7\n") == "4", "sample 1"

assert solve_io("1\n0\n") == "0", "single zero"

assert solve_io("3\n5 2 8\n") == "0", "all positive values"

assert solve_io("3\n-10 20 -5\n") == "10", "large first deficit"

assert solve_io("5\n4 -7 1 1 10\n") == "3", "middle deficit"

assert solve_io("500000\n" + " ".join(["-1000000000"] * 500000) + "\n") == str(500000000000000), "maximum size input"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`6\n3 -4 2 -3 -2 7`|`4`| Cung cấp mẫu và theo dõi tiền tố chung | 
|`1\n0`|`0`| Đầu vào có kích thước tối thiểu và không thay đổi | 
|`5 2 8`|`0`| Không có giá trị tiền tố âm | 
|`-10 20 -5`|`10`| Thâm hụt lớn ngay lập tức | 
|`4 -7 1 1 10`|`3`| Thâm hụt xảy ra trước cuối | 
| 500000 giá trị của`-1000000000`|`500000000000000`| Kích thước đầu vào tối đa và xử lý số nguyên lớn | 

# Vỏ cạnh 

Đối với trường hợp toàn dương tính:```
3
5 2 8
```số dư đang chạy là 5, 7 và 15. Giá trị tiền tố tối thiểu vẫn bằng 0 vì pháp sư mô phỏng không bao giờ mất năng lượng. Thuật toán trả về`-0`, đơn giản là`0`, xử lý chính xác thực tế là không cần mana ban đầu. 

Đối với trường hợp giá trị âm ban đầu:```
3
-10 20 -5
```số dư đang chạy là -10, 10 và 5. Tiền tố tối thiểu là -10, do đó câu trả lời trở thành 10. Điều này phù hợp với yêu cầu vì pháp sư phải sống sót ngay từ nguồn đầu tiên trước khi có thể phục hồi. 

Đối với trường hợp thâm hụt trung bình:```
5
4 -7 1 1 10
```số dư đang chạy là 4, -3, -2, -1 và 9. Giá trị tối thiểu là -3 nên câu trả lời là 3. Thuật toán không phụ thuộc vào lượng mana cuối cùng và xác định chính xác điểm nguy hiểm nhất trong suốt hành trình. 

Đối với đầu vào kích thước tối đa:```
500000
-1000000000 -1000000000 ... -1000000000
```số dư giảm 10^9 mỗi bước. Số dư tối thiểu sau tất cả các nguồn là -5000000000000000, vì vậy câu trả lời là 5000000000000000. Việc triển khai xử lý vấn đề này vì số nguyên Python có thể biểu thị các giá trị lớn hơn nhiều so với giới hạn 64 bit.
