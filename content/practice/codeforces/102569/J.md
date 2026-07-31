---
title: "CF 102569J - Trận chiến của các pháp sư"
description: "Đây là một vấn đề xây dựng chỉ có đầu ra. Không có đầu vào vì nhiệm vụ không phải là xử lý dữ liệu mà là in ra hai bộ sức mạnh của sinh vật tạo ra mối quan hệ xác suất rất cụ thể."
date: "2026-07-31T07:55:54+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102569
codeforces_index: "J"
codeforces_contest_name: "2020, XIII Samara Regional Intercollegiate Programming Contest"
rating: 0
weight: 102569
solve_time_s: 91
verified: false
draft: false
---

[CF 102569J - Trận chiến của các pháp sư](https://codeforces.com/problemset/problem/102569/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 31s 
**Đã xác minh:** không 

##Giải pháp 
#Hiểu vấn đề 

Đây là một vấn đề xây dựng chỉ có đầu ra. Không có đầu vào vì nhiệm vụ không phải là xử lý dữ liệu mà là in ra hai bộ sức mạnh của sinh vật tạo ra mối quan hệ xác suất rất cụ thể. 

Chúng ta cần tạo ra nhiều điểm mạnh của pháp sư đầu tiên và nhiều điểm mạnh của pháp sư thứ hai. Khi cả hai pháp sư ngẫu nhiên chọn chính xác`k`sinh vật, pháp sư đầu tiên phải có nhiều khả năng giành chiến thắng hơn`k = 1`Và`k = 3`, trong khi pháp sư thứ hai phải có nhiều khả năng giành chiến thắng hơn`k = 2`. 

Những hạn chế là nhỏ. Mỗi bộ có thể chứa từ 3 đến 10 sinh vật và mọi sức mạnh phải nằm trong khoảng từ 1 đến 10. Điều này có nghĩa là giải pháp mong muốn không phải là tìm kiếm thuật toán trong quá trình thực thi. Chúng ta chỉ cần khám phá một cách xây dựng hợp lệ và in nó. Chương trình đã gửi có thể chỉ xuất ra các số cố định. 

Phần tinh vi nhất là sự ràng buộc khiến trò chơi bắt đầu lại. Xác suất chiến thắng cuối cùng không được xác định chỉ bằng lần so sánh đầu tiên. Nếu pháp sư đầu tiên thắng một hiệp với xác suất`W1`, thua với xác suất`W2`và quan hệ với xác suất`T`, thì sau khi loại bỏ các mối quan hệ lặp đi lặp lại, việc so sánh người chiến thắng sẽ trở thành`W1`so với`W2`. Vì vậy chúng ta chỉ cần số kết quả thắng lớn hơn số kết quả thua đối với các giá trị yêu cầu của`k`. 

Một sai lầm phổ biến là chỉ so sánh điểm mạnh trung bình. Chỉ riêng sức mạnh trung bình không quyết định được người chiến thắng vì việc phân bổ tập hợp con ngẫu nhiên rất quan trọng. Ví dụ: sử dụng ý tưởng mẫu:```
3
1 2 3
3
2 2 2
```pháp sư đầu tiên có cơ hội chiến thắng`k = 1`, nếu không có`k = 3`cả hai tổng số đều bằng nhau nên trận đấu sẽ hòa. Việc xây dựng cần kiểm soát cả ba kích thước tập hợp con cùng một lúc. 

## Phương pháp tiếp cận 

Cách mạnh mẽ nhất để giải quyết loại vấn đề đầu ra này là liệt kê tất cả các tập hợp có thể, tính xác suất cho`k = 1`,`k = 2`, Và`k = 3`và dừng lại khi tìm thấy một cặp hợp lệ. Vì mỗi bộ có tối đa 10 phần tử và có phạm vi điểm mạnh từ 1 đến 10 nên việc tìm kiếm như vậy có thể thực hiện được khi ngoại tuyến. Tuy nhiên, việc đưa tìm kiếm đó vào giải pháp đã gửi là không cần thiết và lớn hơn nhiều so với nhiệm vụ thực tế. Một bảng liệt kê ngây thơ trên tất cả các tập hợp có kích thước 10 sẽ được xem xét một cách đại khái`C(19,10)`khả năng cho mỗi bên, vốn đã đưa ra hàng trăm nghìn ứng cử viên trước khi kiểm tra sự kết hợp. 

Quan sát hữu ích là chúng ta không cần những phân bố phức tạp. Cấu trúc ba sinh vật là đủ. Đối với ba sinh vật,`k = 3`chỉ là sự so sánh tổng số tiền. Chúng ta có thể giúp pháp sư đầu tiên giành chiến thắng ở đó bằng cách cho tổng điểm lớn hơn trong set đầu tiên. Đồng thời, đối với`k = 2`, các cặp có thể có liên quan chặt chẽ với tổng số vì mỗi cặp là tổng trừ một sinh vật. Điều này cho phép chúng tôi làm cho các cặp pháp sư đầu tiên mạnh nhất xuất hiện quá hiếm. 

Việc xây dựng thành công là:```
First mage: 1 7 8
Second mage: 5 5 5
```Vì`k = 1`, pháp sư đầu tiên có hai sinh vật mạnh hơn 5 và một sinh vật yếu hơn, vì vậy anh ta thắng nhiều hơn trong các cuộc so sánh sinh vật đơn lẻ. 

Vì`k = 2`, tổng cặp của pháp sư thứ nhất là 8, 9 và 15. Tổng cặp của pháp sư thứ hai luôn là 10. Pháp sư thứ nhất thua hai lần và thắng một. 

Vì`k = 3`, tổng số là 16 và 15 nên pháp sư đầu tiên sẽ thắng. 

Ý tưởng brute-force hoạt động vì không gian tìm kiếm đủ nhỏ để khám phá, nhưng giải pháp cuối cùng có thể được rút gọn thành việc in một công trình đã được xác minh này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(10^10) hoặc tệ hơn tùy theo cách liệt kê | O(1) | Không cần thiết | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. In bộ pháp sư đầu tiên thành`1 7 8`. Tổng sức mạnh là 16, lớn hơn tổng sức mạnh của pháp sư thứ hai, mang lại lợi thế cần thiết khi chọn tất cả sinh vật. 
2. In bộ pháp sư thứ hai thành`5 5 5`. Các giá trị cố định của nó làm cho việc so sánh tập hợp con trở nên dễ kiểm soát vì mọi lựa chọn đều có cùng độ mạnh. 
3. Xác minh ba trường hợp bắt buộc. Với một sinh vật, pháp sư đầu tiên có hai giá trị chiến thắng. Với hai sinh vật, tổng số cặp của pháp sư đầu tiên hầu hết thấp hơn tổng số cặp cố định của pháp sư thứ hai. Với ba sinh vật, tổng số của pháp sư đầu tiên lớn hơn. 

Lý do cấu trúc này hoạt động là vì ba số giống nhau tạo ra hành vi trái ngược nhau đối với từng sinh vật và cặp. Giá trị cao giúp ích cho`k = 1`, nhưng việc kết hợp những giá trị cao đó khiến pháp sư đầu tiên chỉ có một cặp rất mạnh, gây tổn thất cho`k = 2`. Tổng số vẫn đủ cao để giành chiến thắng cho`k = 3`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    print(3)
    print("1 7 8")
    print(3)
    print("5 5 5")

if __name__ == "__main__":
    solve()
```Chương trình không đọc được gì vì đầu vào được cố tình để trống. Nó in trực tiếp hai bộ sinh vật được tìm thấy ở trên. 

Hai dòng in đầu tiên mô tả bộ sưu tập của pháp sư đầu tiên và hai dòng tiếp theo mô tả bộ sưu tập của pháp sư thứ hai. Không có vòng lặp hoặc tính toán vì bài toán chỉ yêu cầu một nhân chứng hợp lệ. 

Định dạng đầu ra nhỏ cũng là lý do tại sao không có vấn đề tràn hoặc ranh giới. Tất cả các giá trị đều nằm trong phạm vi cho phép. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Mẫu được cung cấp không phải là câu trả lời hợp lệ nên thay vào đó, chúng tôi theo dõi cấu trúc đã gửi. 

| k | Lựa chọn pháp sư đầu tiên | Lựa chọn pháp sư thứ hai | Kết quả | 
| --- | --- | --- | --- | 
| 1 | 1, 7, 8 | 5, 5, 5 | Thắng: 2, thua: 1 | 
| 2 | 8, 9, 15 | 10 | Thắng: 1, thua: 2 | 
| 3 | 16 | 15 | Pháp sư đầu tiên chiến thắng | 

Dấu vết này cho thấy thuộc tính quan trọng của công trình. Cùng một tập hợp mạnh hơn về tổng thể sẽ trở nên yếu hơn đối với hầu hết các lựa chọn gồm hai sinh vật. 

### Ví dụ 2 

| k | Lựa chọn pháp sư đầu tiên | Lựa chọn pháp sư thứ hai | Kết quả | 
| --- | --- | --- | --- | 
| 1 | 7 đấu với 5 | 5 | Thắng | 
| 1 | 1 đấu 5 | 5 | Mất mát | 
| 2 | 7 + 8 = 15 | 5 + 5 = 10 | Thắng | 
| 2 | 1 + 7 = 8 | 5 + 5 = 10 | Mất mát | 
| 2 | 1 + 8 = 9 | 5 + 5 = 10 | Mất mát | 

Ví dụ này nêu bật lý do tại sao chỉ kiểm tra cường độ tối đa lại gây hiểu nhầm. Pháp sư đầu tiên có cặp mạnh nhất có thể, nhưng hầu hết các cặp đều yếu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chương trình chỉ in bốn dòng. | 
| Không gian | O(1) | Không có bộ nhớ bổ sung được sử dụng. | 

Các ràng buộc rất nhỏ vì đây là một vấn đề xây dựng. Giải pháp đầu ra theo thời gian không đổi nằm trong giới hạn thoải mái. 

## Trường hợp thử nghiệm 

Vì đầu vào chính thức trống nên việc kiểm tra tập trung vào việc xác minh rằng đầu ra được tạo ra có đáp ứng định dạng và điều kiện xác suất hay không.```python
import sys
import io
import itertools

def run(inp: str) -> str:
    return "3\n1 7 8\n3\n5 5 5\n"

def check(output):
    data = list(map(int, output.split()))
    n1 = data[0]
    a = data[1:1+n1]
    n2 = data[1+n1]
    b = data[2+n1:2+n1+n2]

    assert 3 <= n1 <= 10
    assert 3 <= n2 <= 10
    assert all(1 <= x <= 10 for x in a)
    assert all(1 <= x <= 10 for x in b)

    for k in (1, 2, 3):
        x = [sum(c) for c in itertools.combinations(a, k)]
        y = [sum(c) for c in itertools.combinations(b, k)]
        win = sum(i > j for i in x for j in y)
        lose = sum(i < j for i in x for j in y)

        if k in (1, 3):
            assert win > lose
        else:
            assert win < lose

assert run("") == "3\n1 7 8\n3\n5 5 5\n"

check(run(""))

# Minimum size style validation
check("3\n1 7 8\n3\n5 5 5\n")

# Maximum allowed style validation of parser logic
assert len(run("").split()) == 8
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Đầu vào trống | Xây dựng cố định | Phù hợp với yêu cầu chỉ đầu ra | 
| Đầu ra được tạo | Cùng xây dựng | Kiểm tra định dạng và tính hợp lệ | 
| Xác thực lặp lại | Cùng xây dựng | Khẳng định điều kiện xác suất | 

## Vỏ cạnh 

Trường hợp cạnh thứ nhất là khi tất cả sinh vật được chọn có tổng sức mạnh như nhau`k = 3`. Một công trình như`1 2 3`so với`2 2 2`thất bại vì cả hai tổng điểm đều là 6. Trò chơi không bao giờ tạo ra người chiến thắng ở`k = 3`. Việc xây dựng được gửi tránh điều này bởi vì`1 + 7 + 8 = 16`Và`5 + 5 + 5 = 15`. 

Trường hợp cạnh thứ hai chỉ dựa vào sức mạnh trung bình. Mức trung bình của pháp sư đầu tiên cao hơn trong kết cấu cuối cùng, nhưng thực tế đó không giải thích được`k = 2`kết quả. Tổng cặp thực tế là`8`,`9`, Và`15`, do đó hai trong số ba lựa chọn thua giá trị cố định`10`. Một giải pháp bất cẩn chỉ kiểm tra tổng số sẽ bỏ lỡ hành vi này. 

Trường hợp cạnh thứ ba là quan hệ. Nếu số kết quả thắng và thua bằng nhau thì các hiệp đấu lặp lại sẽ không tạo ra lợi thế. Ở đây, đối với`k = 1`, có hai so sánh thắng và một so sánh thua. Vì`k = 2`, có một so sánh thắng và hai so sánh thua. Không có sự bình đẳng ẩn nào làm thay đổi kết quả.
