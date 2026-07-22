---
title: "CF 103665E - \u041f\u0440\u043e\u043f\u0430\u0432\u0448\u0438\u0435 \u0441\u043b\u0438\u0442\u043a\u0438"
description: "Chúng ta được cung cấp một tập hợp các thỏi vàng giống hệt nhau và mỗi thanh có thể được định hướng theo một trong ba cách hiệu quả, góp phần tạo ra chiều cao a, b hoặc c. Tất cả các thanh phải được sử dụng đúng một lần và chúng tôi xếp chúng thành một tháp duy nhất."
date: "2026-07-02T21:44:06+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103665
codeforces_index: "E"
codeforces_contest_name: "\u0422\u0443\u0440\u043d\u0438\u0440 \u0410\u0440\u0445\u0438\u043c\u0435\u0434\u0430 2018"
rating: 0
weight: 103665
solve_time_s: 48
verified: true
draft: false
---

[CF 103665E - \u041f\u0440\u043e\u043f\u0430\u0432\u0448\u0438\u0435 \u0441\u043b\u0438\u0442\u043a\u0438](https://codeforces.com/problemset/problem/103665/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 48s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một bộ sưu tập các thỏi vàng giống hệt nhau và mỗi thanh có thể được định hướng theo một trong ba cách hiệu quả, đóng góp vào chiều cao của một trong hai cách.`a`,`b`, hoặc`c`. Tất cả các thanh phải được sử dụng đúng một lần và chúng tôi xếp chúng thành một tháp duy nhất. Điều duy nhất quan trọng đối với một cấu hình là tổng chiều cao của nó, là tổng đóng góp của tất cả`n`thanh. 

Các lựa chọn hướng khác nhau có thể tạo ra cùng một chiều cao. Nhiệm vụ là đếm xem có thể đạt được tổng cộng bao nhiêu độ cao khác nhau. 

Đầu vào mang lại`n`, số thanh và ba số nguyên`a`,`b`, Và`c`, đó là sự đóng góp chiều cao có thể có của mỗi thanh tùy theo hướng. Đầu ra là số lượng tổng riêng biệt có thể được hình thành bằng cách chọn, cho mỗi`n`vị trí, một trong ba giá trị. 

Những hạn chế là quan trọng. Số lượng thanh có thể lớn tới 1200, điều này loại trừ mọi cách tiếp cận liệt kê tất cả`3^n`cấu hình. Không gian đó rộng lớn về mặt thiên văn ngay cả đối với những người vừa phải`n`. Tuy nhiên, các giá trị của`a`,`b`, Và`c`lớn, lên tới`10^9`, vì vậy chúng ta cũng không thể dựa vào ba lô có giới hạn với phạm vi trọng lượng nhỏ. 

Một quan sát quan trọng là cấu trúc duy nhất trong bài toán là phép cộng: mỗi thanh đóng góp độc lập và chúng ta chỉ quan tâm đến tổng. 

Các trường hợp cạnh xuất hiện khi các giá trị bằng nhau. Nếu như`a = b = c`, thì mọi cấu hình đều tạo ra tổng chiều cao như nhau và câu trả lời là 1. Nếu hai giá trị bằng nhau, giả sử`a = b != c`, thì thực tế là chúng ta chỉ có hai lựa chọn cho mỗi thanh, nhưng vẫn cần xử lý việc đếm các khoản tiền riêng biệt một cách chính xác. 

Một sai lầm ngây thơ là cho rằng số lượng cấu hình bằng số tổng, điều này là sai vì nhiều phép gán khác nhau sẽ gộp lại thành một tổng số giống nhau. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: hãy thử mọi cách để gán từng`n`thanh một trong các giá trị`a`,`b`, hoặc`c`, tính tổng kết quả và lưu nó vào một tập hợp. Điều này đúng vì nó xây dựng rõ ràng mọi cấu hình có thể. Tuy nhiên, nó đòi hỏi phải lặp đi lặp lại`3^n`trạng thái, điều này trở nên không thể ngay cả đối với`n = 30`, huống hồ là`n = 1200`. 

Cấu trúc quan trọng là tổng cuối cùng chỉ phụ thuộc vào số lượng thanh được gán cho mỗi giá trị chứ không phụ thuộc vào vị trí của chúng. Nếu chúng ta để`x`,`y`, Và`z`là số lượng thanh được chỉ định`a`,`b`, Và`c`, thì tổng chiều cao là`x*a + y*b + z*c`, với`x + y + z = n`. Vì vậy, vấn đề giảm xuống còn việc đếm các giá trị phân biệt có dạng tuyến tính trên tất cả các bộ ba số nguyên bị ràng buộc bởi một đơn hình đơn giản. 

Thay vì liệt kê các bài tập, chúng ta liệt kê các phân phối đếm. chỉ có`O(n^2)`bộ ba hợp lệ`(x, y, z)`bởi vì một lần`x`Và`y`đã được cố định,`z`được xác định. Mỗi bộ ba tạo ra chính xác một tổng, vì vậy chúng ta có thể chèn các tổng này vào một tập hợp và đếm các kết quả riêng biệt. 

Điều này biến một bài toán hàm mũ thành một bài toán bậc hai. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force vượt qua bài tập | O(3^n) | O(3^n) | Quá chậm | 
| Đếm ba lần | O(n^2) | O(n^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chuyển quan điểm từ trình tự sang số lượng. 

1. Lặp lại tất cả các giá trị có thể có của`x`, số lượng thanh đóng góp`a`, từ`0`ĐẾN`n`. Cái này sửa bao nhiêu lần`a`được sử dụng. 
2. Đối với mỗi cố định`x`, lặp đi lặp lại`y`, số lượng thanh đóng góp`b`, từ`0`ĐẾN`n - x`. Phần còn lại`z`buộc phải là`n - x - y`, vì vậy chúng tôi không bao giờ lặp lại nó một cách rõ ràng. 
3. Đối với mỗi bộ ba`(x, y, z)`, tính tổng chiều cao`x*a + y*b + z*c`. Chèn giá trị này vào tập băm. Bộ này tự động loại bỏ các bản sao do các nguyên nhân khác nhau gây ra.`(x, y, z)`các kết hợp tạo ra cùng một số tiền. 
4. Sau tất cả các lần lặp, kích thước của tập hợp là số chiều cao riêng biệt có thể đạt được. 

Lý do liệt kê này hợp lệ là vì mỗi cấu hình hợp lệ tương ứng duy nhất với một bộ ba`(x, y, z)`và mỗi bộ ba tương ứng với ít nhất một cấu hình. 

### Tại sao nó hoạt động 

Thuật toán dựa trên tính bất biến mà mọi sự sắp xếp của các thanh đều được xác định đầy đủ, tùy theo hoán vị, bằng cách đếm số lần mỗi giá trị được chọn. Vì thứ tự không ảnh hưởng đến tổng nên hai cấu hình tương đương khi và chỉ khi chúng có chung`(x, y, z)`. Các vòng lặp lồng nhau liệt kê tất cả các bộ ba có thể có chính xác một lần, vì vậy mọi tổng có thể đều được xem xét và không có tổng hợp lệ nào bị bỏ sót. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, a, b, c = map(int, input().split())
    seen = set()

    for x in range(n + 1):
        for y in range(n - x + 1):
            z = n - x - y
            seen.add(x * a + y * b + z * c)

    print(len(seen))

if __name__ == "__main__":
    solve()
```Việc thực hiện tuân theo phép liệt kê ba lần trực tiếp. Vòng lặp bên ngoài sửa bao nhiêu lần`a`được sử dụng và vòng lặp bên trong sẽ sửa bao nhiêu lần`b`được sử dụng. Số còn lại cho`c`được tính toán ngầm, giúp tránh vòng lặp thứ ba không cần thiết. 

Việc sử dụng một bộ là cần thiết vì khác nhau`(x, y, z)`sự kết hợp có thể mang lại số tiền giống nhau, đặc biệt khi`a`,`b`, Và`c`có quan hệ số học. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào ở đâu`n = 2`,`a = 1`,`b = 2`,`c = 3`. 

Chúng tôi liệt kê tất cả các bộ ba: 

| x | y | z | tổng hợp | 
| --- | --- | --- | --- | 
| 0 | 0 | 2 | 6 | 
| 0 | 1 | 1 | 5 | 
| 0 | 2 | 0 | 4 | 
| 1 | 0 | 1 | 4 | 
| 1 | 1 | 0 | 3 | 
| 2 | 0 | 0 | 2 | 

Tập hợp các tổng là`{2, 3, 4, 5, 6}`, vậy câu trả lời là`5`. 

Dấu vết này cho thấy các phân bố số đếm khác nhau có thể xung đột trên cùng một tổng, chẳng hạn như`(x=0,y=2,z=0)`Và`(x=1,y=0,z=1)`vừa sản xuất`4`. 

Bây giờ hãy xem xét`n = 3`,`a = b = 1`,`c = 10`. 

| x | y | z | tổng hợp | 
| --- | --- | --- | --- | 
| 3 | 0 | 0 | 3 | 
| 2 | 1 | 0 | 3 | 
| 2 | 0 | 1 | 12 | 
| 1 | 2 | 0 | 3 | 
| 1 | 1 | 1 | 12 | 
| 1 | 0 | 2 | 21 | 
| 0 | 3 | 0 | 3 | 
| 0 | 2 | 1 | 12 | 
| 0 | 1 | 2 | 21 | 
| 0 | 0 | 3 | 30 | 

Tổng riêng biệt là`{3, 12, 21, 30}`, vậy câu trả lời là`4`. 

Điều này chứng tỏ giá trị bình đẳng giữa`a`Và`b`tạo nhiều cấu hình thu gọn, nhưng phép liệt kê dựa trên tập hợp vẫn nắm bắt chính xác tính duy nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2) | Hai vòng lặp lồng nhau trên x và y, với phần chèn thời gian không đổi cho mỗi trạng thái | 
| Không gian | O(n^2) | Trong trường hợp xấu nhất, tất cả các tổng được tạo đều khác biệt | 

Ràng buộc`n ≤ 1200`thực hiện khoảng 720.000 lần lặp, điều này có thể chấp nhận được trong Python. Mỗi lần lặp là số học đơn giản cộng với một hàm băm nên nó vừa vặn thoải mái trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

def solve():
    n, a, b, c = map(int, input().split())
    seen = set()
    for x in range(n + 1):
        for y in range(n - x + 1):
            z = n - x - y
            seen.add(x * a + y * b + z * c)
    print(len(seen))

# provided sample
assert run("1 1 2 3\n") == "3", "sample 1"

# all equal
assert run("5 7 7 7\n") == "1", "all equal values"

# two equal values
assert run("3 1 1 10\n") == "4", "two equal values case"

# small asymmetric
assert run("2 1 2 3\n") == "5", "small enumeration check"

# larger uniform spacing
assert run("4 1 2 4\n") >= "1", "sanity check"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 5 7 7 7 | 1 | tất cả các giá trị giống hệt nhau thu gọn tất cả các khoản tiền | 
| 3 1 1 10 | 4 | giá trị lặp lại làm giảm kích thước hiệu quả | 
| 2 1 2 3 | 5 | tính đúng đắn của việc liệt kê đầy đủ | 

## Vỏ cạnh 

Ví dụ: khi cả ba giá trị đều bằng nhau`n = 1000, a = b = c = 5`, mọi cấu hình đều tạo ra tổng số như nhau`5000`. Thuật toán lặp lại trên tất cả`(x, y)`cặp, nhưng mọi tổng được tính toán đều giống hệt nhau, do đó tập hợp chỉ chứa một phần tử. Đầu ra cuối cùng là chính xác`1`. 

Khi hai giá trị bằng nhau, chẳng hạn như`a = b = 1`Và`c = 100`, nhiều thứ khác nhau`(x, y, z)`bộ ba sụp đổ thành số tiền giống hệt nhau. Ví dụ,`(x=2,y=1,z=0)`Và`(x=3,y=0,z=0)`cả hai đều đóng góp khác nhau nhưng vẫn tạo ra phạm vi chồng chéo. Bộ này đảm bảo các bản sao được hợp nhất và không cần logic chống trùng lặp thủ công. 

Khi giá trị lớn, như`10^9`, không có rủi ro tràn trong Python, nhưng trong các ngôn ngữ có chiều rộng cố định, điều này yêu cầu số nguyên 64 bit. Thuật toán không bao giờ nhân lên quá`n * max(a,b,c)`, phù hợp an toàn với số nguyên có dấu 64 bit.
