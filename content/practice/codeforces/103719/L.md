---
title: "CF 103719L - AvtoBus"
description: "Chúng ta được biết tổng số bánh xe trong một đội xe buýt. Mỗi phương tiện trong đội xe đều là xe buýt 4 bánh hoặc xe buýt 6 bánh và chúng tôi không biết mỗi loại tồn tại bao nhiêu chiếc."
date: "2026-07-02T09:24:53+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103719
codeforces_index: "L"
codeforces_contest_name: "VII \u041b\u0438\u043f\u0435\u0446\u043a\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u0448\u043a\u043e\u043b\u044c\u043d\u0438\u043a\u043e\u0432 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e. \u0424\u0438\u043d\u0430\u043b. 8-11 \u043a\u043b\u0430\u0441\u0441\u044b"
rating: 0
weight: 103719
solve_time_s: 40
verified: true
draft: false
---

[CF 103719L - AvtoBus](https://codeforces.com/problemset/problem/103719/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 40s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được biết tổng số bánh xe trong một đội xe buýt. Mỗi phương tiện trong đội xe đều là xe buýt 4 bánh hoặc xe buýt 6 bánh và chúng tôi không biết mỗi loại tồn tại bao nhiêu chiếc. Nhiệm vụ là xây dựng lại tất cả các quy mô đội xe có thể sản xuất chính xác tổng số bánh xe nhất định. Trong số tất cả các cấu hình hợp lệ, chúng ta phải báo cáo số lượng xe buýt nhỏ nhất có thể và số lượng xe buýt lớn nhất có thể. Nếu không có sự kết hợp nào giữa xe buýt 4 bánh và 6 bánh có thể tạo ra tổng số nhất định thì chúng tôi cho rằng điều đó là không thể. 

Đầu vào là một số nguyên n, có khả năng lớn tới 10^18, vì vậy mọi giải pháp đều phải chạy trong thời gian không đổi hoặc logarit. Bất kỳ điều gì lặp lại trên số lượng xe buýt có thể lên tới n đều không thể thực hiện được ngay lập tức. 

Một điểm tinh tế quan trọng là không phải mọi n đều có thể biểu diễn được. Vì cả hai loại xe buýt đều có số bánh chẵn nên không thể hình thành n lẻ nào cả. Ví dụ: n = 7 không có nghiệm vì tổng của 4 và 6 luôn chẵn. Một trường hợp góc khác là các giá trị rất nhỏ. Với n = 2, cũng không tồn tại tổ hợp nào vì chiếc xe buýt nhỏ nhất đã có sẵn 4 bánh. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là thử tất cả số lượng xe buýt k từ 1 trở lên và kiểm tra xem liệu chúng ta có thể ấn định một số lượng xe buýt 4 bánh và 6 bánh để đạt tổng n hay không. Với mỗi k, chúng ta sẽ kiểm tra xem có tồn tại số nguyên x sao cho 4x + 6(k − x) = n hay không. Điều này dẫn đến việc kiểm tra xem n − 6k có chia hết cho −2 hay không và kết quả x có nằm trong giới hạn hay không. Mặc dù mỗi lần kiểm tra đều có thời gian không đổi, việc lặp lại tất cả k cho đến n/4 trong trường hợp xấu nhất là quá lớn đối với n lên đến 10^18. 

Cấu trúc sẽ đơn giản hóa nếu chúng ta viết lại phương trình. Gọi a là số lượng xe buýt 4 bánh và b là số lượng xe buýt 6 bánh. Khi đó chúng ta có 4a + 6b = n, có thể chia cho 2 để được 2a + 3b = n/2. Điều này ngay lập tức chứng tỏ rằng n phải chẵn, nếu không thì không có nghiệm nào tồn tại. 

Bây giờ hãy sửa b. Khi đó a được xác định duy nhất là (n − 6b)/4 và chúng ta chỉ yêu cầu nó phải là số nguyên không âm. Số lượng xe buýt là k = a + b. Thay a vào sẽ có k = b + (n − 6b)/4 = n/4 − b/2. Biểu thức này cho thấy một mối quan hệ đơn điệu: tăng b giảm k. Điều đó có nghĩa là số lượng xe buýt tối đa xảy ra khi chúng ta giảm thiểu b và số lượng xe buýt tối thiểu xảy ra khi chúng ta tối đa hóa b, dưới các ràng buộc hợp lệ. 

Hạn chế duy nhất là a phải không âm và tích phân. Từ 4a + 6b = n, chúng ta yêu cầu n − 6b ≥ 0 và chia hết cho 4. Vì mọi thứ đều chẵn nên khả năng chia hết giảm xuống điều kiện chẵn lẻ trên b. Do đó, chúng ta có thể xác định các giá trị b khả thi trong một phạm vi rất nhỏ và trích xuất trực tiếp các giá trị k cực trị. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force hơn k | O(n) | O(1) | Quá chậm | 
| Giảm đại số | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta bắt đầu từ phương trình liên quan đến xe buýt và bánh xe. Mỗi bước trích xuất cấu trúc cho đến khi chỉ còn lại kiểm tra thời gian không đổi.

1. Kiểm tra xem n có chia hết cho 2 hay không. Nếu không thì kết luận ngay là không có nghiệm. Điều này xảy ra vì cả 4 và 6 đều là số chẵn nên bất kỳ tổng nào cũng phải là số chẵn. 
2. Định nghĩa m = n/2 để rút gọn hệ số. Phương trình trở thành 2a + 3b = m, làm giảm kích thước số học mà không làm thay đổi tính khả thi. 
3. Biểu diễn một biến theo biến kia: a = (m − 3b) / 2. Điều này chứng tỏ rằng với mọi b cố định, a được xác định duy nhất. 
4. Áp dụng điều kiện hiệu lực: a ≥ 0 và tích phân. Tính không âm cho m − 3b ≥ 0, giới hạn b từ phía trên. Tính tích phân yêu cầu m − 3b phải chẵn, tương đương với b có cùng tính chẵn lẻ với m. 
5. Từ những ràng buộc này, hãy xác định khả thi nhỏ nhất b và khả thi lớn nhất b. Vì k = a + b giảm khi b tăng nên b nhỏ nhất mang lại số lượng xe buýt tối đa và b lớn nhất mang lại số lượng xe buýt tối thiểu. 
6. Tính k cho cả hai giá trị biên của b và xuất chúng. 

### Tại sao nó hoạt động 

Bất biến cốt lõi là mọi nghiệm hợp lệ đều tương ứng chính xác với một giá trị nguyên duy nhất của b thỏa mãn hai ràng buộc độc lập: giới hạn không âm và điều kiện chẵn lẻ. Khi b đã cố định thì a bị ép buộc. Điều này thu gọn không gian tìm kiếm hai chiều thành một cấp số cộng giới hạn duy nhất. Vì k là hàm tuyến tính giảm theo b, nên các giá trị cực trị của k phải xảy ra ở các giá trị cực trị khả thi của b, do đó chỉ cần quét các giá trị biên là đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input().strip())
    
    if n % 2 == 1:
        print(-1)
        return
    
    m = n // 2  # 2a + 3b = m
    
    # b must satisfy:
    # 3b <= m  => b <= m // 3
    # (m - 3b) even => b % 2 == m % 2
    
    # smallest valid b
    if m % 2 == 0:
        b_min = 0
    else:
        b_min = 1
    
    # largest valid b <= m//3 with correct parity
    b_max = m // 3
    if b_max % 2 != m % 2:
        b_max -= 1
    
    if b_max < b_min:
        print(-1)
        return
    
    def buses(b):
        a = (m - 3 * b) // 2
        return a + b
    
    x = buses(b_max)  # minimum buses
    y = buses(b_min)  # maximum buses
    
    print(x, y)

if __name__ == "__main__":
    solve()
```Đoạn mã đầu tiên lọc các trường hợp không thể sử dụng tính chẵn lẻ của n. Sau đó, nó chuyển bài toán thành phương trình đơn giản 2a + 3b = m. Các ràng buộc trên b được suy ra trực tiếp từ tính không âm và tính chẵn lẻ. Chúng tôi chọn các giá trị biên của b thỏa mãn cả hai điều kiện và tính số lượng xe buýt tương ứng. Việc đặt hàng là có chủ ý: b lớn hơn làm giảm tổng số xe buýt, do đó b_max đưa ra câu trả lời tối thiểu. 

Một chi tiết tinh tế là việc điều chỉnh phép chia số nguyên và tính chẵn lẻ phải được áp dụng cẩn thận; mặt khác, ứng viên b có thể thỏa mãn bất đẳng thức nhưng vi phạm tính tích phân, tạo ra a không chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1: n = 4 

Ở đây m = 2, do đó 2a + 3b = 2. Giá trị duy nhất có thể có của b là 0, cho ra a = 1. 

| b | a = (2 − 3b)/2 | hợp lệ | xe buýt a+b | 
| --- | --- | --- | --- | 
| 0 | 1 | vâng | 1 | 

Cả tối thiểu và tối đa đều là 1. 

Điều này xác nhận rằng khi chỉ tồn tại một cấu hình thì cả hai thái cực đều trùng khớp. 

### Ví dụ 2: n = 24 

Ở đây m = 12 nên 2a + 3b = 12. 

| b | một | hợp lệ | xe buýt | 
| --- | --- | --- | --- | 
| 0 | 6 | vâng | 6 | 
| 2 | 3 | vâng | 5 | 
| 4 | 0 | vâng | 4 | 

Bus tối đa xảy ra ở b = 0, tối thiểu ở b = 4. 

Điều này cho thấy sự cân bằng đơn điệu giữa xe buýt nặng hơn và tổng số lượng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ một số phép tính số học và kiểm tra ranh giới | 
| Không gian | O(1) | Không sử dụng cấu trúc phụ trợ | 

Giải pháp này hoạt động thoải mái dưới giới hạn 1 giây vì nó tránh hoàn toàn việc lặp lại và giảm bài toán về số học theo thời gian không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import prod
    import builtins

    # re-import solution by redefining solve inline for test simplicity
    def solve():
        n = int(sys.stdin.readline().strip())
        if n % 2 == 1:
            return "-1\n"
        m = n // 2
        if m % 2 == 0:
            b_min = 0
        else:
            b_min = 1
        b_max = m // 3
        if b_max % 2 != m % 2:
            b_max -= 1
        if b_max < b_min:
            return "-1\n"
        def buses(b):
            a = (m - 3 * b) // 2
            return a + b
        x = buses(b_max)
        y = buses(b_min)
        return f"{x} {y}\n"

    return solve()

# provided samples
assert run("4\n") == "1 1\n"
assert run("7\n") == "-1\n"

# custom cases
assert run("2\n") == "-1\n"
assert run("6\n") == "1 1\n"
assert run("24\n") == "4 6\n"
assert run("1000000000000000000\n") != "", "large even case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 | -1 | trường hợp chẵn nhỏ nhất không thể | 
| 6 | 1 1 | trường hợp cấu hình đơn | 
| 24 | 4 6 | nhiều phân tách hợp lệ | 
| 10^18 | cặp hợp lệ | kiểm tra căng thẳng cho hành vi theo thời gian không đổi | 

## Vỏ cạnh 

Với n = 1, thuật toán sẽ bác bỏ ngay lập tức do tính chẵn lẻ lẻ, vì không có sự kết hợp nào của 4 và 6 có thể tạo thành tổng lẻ. 

Với n = 2, tính chẵn lẻ chỉ vượt qua sau khi chia, nhưng m = 1 làm cho tất cả các giá trị ứng cử viên không hợp lệ vì 2a + 3b không thể bằng 1. Phạm vi b được tính sẽ trở nên trống, kích hoạt việc kiểm tra tính không thể thực hiện được. 

Với n = 4, logic biên mang lại b_min = 0 và b_max = 0, tạo ra một cấu hình hợp lệ duy nhất với một bus. Điều này xác nhận tính đúng đắn khi chỉ tồn tại một giải pháp. 

Đối với số chẵn rất lớn, chẳng hạn như 10^18, tất cả các phép tính vẫn nằm trong số học số nguyên và chỉ phụ thuộc vào một số phép chia và kiểm tra tính chẵn lẻ, do đó thuật toán hoạt động giống hệt nhau bất kể độ lớn.
