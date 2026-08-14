---
title: "CF 102330A - \u0414\u043e\u043a\u0442\u043e\u0440 \u0410\u0439\u0431\u043e\u043b\u0438\u0442"
description: "Chúng ta có một mảng a gồm n con vật. Giá trị a[i] là khoảng thời gian bác sĩ Aibolit cần để kiểm tra con vật i. Bác sĩ xử lý từng con vật một, vì vậy các con vật xếp thành một hàng duy nhất. Đối với thứ tự đã chọn, con vật đầu tiên đợi 0 đơn vị thời gian."
date: "2026-08-14T01:00:14+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102330
codeforces_index: "A"
codeforces_contest_name: "\u0421\u0438\u0440\u0438\u0443\u0441.2019.\u041d\u043e\u044f\u0431\u0440\u044c.\u041e\u0447\u043d\u044b\u0439 \u043e\u0442\u0431\u043e\u0440"
rating: 0
weight: 102330
solve_time_s: 309
verified: true
draft: false
---

[CF 102330A - \u0414\u043e\u043a\u0442\u043e\u0440 \u0410\u0439\u0431\u043e\u043b\u0438\u0442](https://codeforces.com/problemset/problem/102330/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 5 phút 9 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một mảng`a`của`n`động vật. giá trị`a[i]`là khoảng thời gian bác sĩ Aibolit cần để khám cho động vật`i`. Bác sĩ xử lý từng con vật một, vì vậy các con vật xếp thành một hàng duy nhất. 

Đối với thứ tự đã chọn, con vật đầu tiên sẽ đợi`0`đơn vị thời gian. Lần thứ hai chờ kiểm tra toàn bộ con vật đầu tiên. Người thứ ba chờ đợi hai kỳ thi đầu tiên, v.v. Chúng ta cần chọn thứ tự các phần tử của mảng sao cho tổng tất cả thời gian chờ càng nhỏ càng tốt. 

Ví dụ: nếu thời gian kiểm tra là`[5, 1, 2]`và chúng tôi sử dụng thứ tự`[1, 2, 5]`, thời gian chờ đợi là`0`,`1`, Và`3`, mang lại tổng cộng`4`. 

Số lượng động vật có thể đạt tới`10^6`, trong khi mỗi lần thi tối đa là`10^5`. Một thuật toán như`O(n^2)`sẽ yêu cầu khoảng`10^12`hoạt động trong trường hợp xấu nhất, vượt xa giới hạn một giây. Thậm chí`O(n log n)`phải được triển khai một cách hợp lý và hiệu quả vì bản thân đầu vào chứa một triệu con số nhưng lại dễ dàng thực hiện được. Câu trả lời có thể lớn hơn nhiều so với số nguyên 32 bit: nếu tất cả`10^6`thời gian thi bằng nhau`10^5`, câu trả lời là về`5 * 10^16`. Số nguyên Python tự động xử lý việc này. 

Có một số trường hợp nhỏ có thể bộc lộ sai sót. Ví dụ, với một con vật, đầu vào là```
1
7
```và câu trả lời là`0`, bởi vì không ai chờ đợi. Việc triển khai thêm thời gian kiểm tra hiện tại trước khi tích lũy câu trả lời sẽ tạo ra kết quả không chính xác`7`. 

Với thời gian thi bằng nhau, mọi thứ tự đều tương đương. Vì```
5
2 2 2 2 2
```thời gian chờ đợi là`0, 2, 4, 6, 8`, vậy câu trả lời là`20`. Giải pháp vô tình sắp xếp theo thứ tự giảm dần sẽ không thất bại trong trường hợp này, điều này làm cho các giá trị bằng nhau trở nên hữu ích trong việc kiểm tra xem bản thân công thức có đúng hay không thay vì chỉ dựa vào thứ tự. 

Trường hợp phổ biến khác là con vật ngắn nhất phải được xử lý trước ngay cả khi nó xuất hiện sau trong đầu vào. Vì```
3
7 1 2
```thứ tự tối ưu là`1, 2, 7`, đưa ra thời gian chờ đợi`0, 1, 3`và trả lời`4`. Đơn giản chỉ cần xử lý động vật theo thứ tự đầu vào của chúng sẽ mang lại`0 + 7 + 8 = 15`, do đó việc triển khai không bao giờ sắp xếp lại mảng sẽ thất bại ngay lập tức. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp sẽ thử mọi mệnh lệnh có thể có của động vật. Đối với mỗi hoán vị, chúng ta có thể quét nó từ trái sang phải, giữ nguyên tổng thời gian kiểm tra đã thực hiện và cộng giá trị đó vào câu trả lời cho mọi con vật. Điều này đúng vì mọi lịch trình có thể đều được xem xét và chúng ta có thể chọn lịch trình có tổng thời gian chờ nhỏ nhất. 

Vấn đề là số lượng hoán vị. có`n!`các đơn hàng có thể, và việc đánh giá một đơn hàng cần`O(n)`thời gian, do đó độ phức tạp tổng thể là`O(n · n!)`. Ngay cả đối với`n = 20`, cái này đã lớn lắm rồi. Vì`n = 10^6`, không chỉ là quá chậm mà còn không thể bắt đầu liệt kê các lịch trình. 

Cấu trúc của thời gian chờ đợi cho chúng ta một quan sát mạnh mẽ hơn nhiều. Giả sử hai con vật liên tiếp có thời gian kiểm tra`x`Và`y`, và mọi thứ trước đó đã bị chiếm mất`T`thời gian. Nếu đơn đặt hàng là`x, y`, sự đóng góp của họ vào tổng thời gian chờ đợi là`T + (T + x) = 2T + x`. 

Nếu chúng ta hoán đổi chúng thành`y, x`, sự đóng góp của họ trở thành`T + (T + y) = 2T + y`. 

Sự khác biệt chỉ phụ thuộc vào`x`Và`y`. Nếu như`x > y`, đặt`y`đầu tiên làm cho tổng số nhỏ hơn. Vì vậy, bất cứ khi nào một bài kiểm tra dài hơn diễn ra ngay trước một bài kiểm tra ngắn hơn, việc hoán đổi chúng sẽ cải thiện lịch trình. 

Bằng cách loại bỏ nhiều lần các phép đảo ngược như vậy, thời gian kiểm tra sẽ được sắp xếp theo thứ tự không giảm. Vì mọi phép đảo ngược chỉ có thể làm cho câu trả lời tệ hơn nên ta có thể đạt được lịch trình tối ưu bằng cách sắp xếp mảng từ nhỏ nhất đến lớn nhất. 

Khi đơn hàng đã được biết, không cần phải lưu trữ rõ ràng mỗi lần chờ đợi. Duy trì`prefix`, tổng thời gian kiểm tra của tất cả các động vật đã được xử lý. Con vật hiện tại đang đợi chính xác`prefix`đơn vị thời gian, vì vậy hãy thêm`prefix`đến câu trả lời và sau đó tăng lên`prefix`theo thời gian thi hiện tại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(n · n!)`|`O(n)`| Quá chậm | 
| Tối ưu |`O(n log n)`|`O(n)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`và`n`lần kiểm tra vào một mảng. Vị trí ban đầu không quan trọng vì mục tiêu chỉ là tìm thứ tự tốt nhất. 
2. Sắp xếp mảng theo thứ tự không giảm. Một cuộc kiểm tra ngắn hơn nên được thực hiện sớm hơn vì mọi con vật sau đó đều được hưởng lợi từ khoảng thời gian chờ đợi được cộng thêm ít hơn. 
3. Khởi tạo`prefix = 0`Và`answer = 0`. Đây`prefix`thể hiện tổng thời gian kiểm tra của tất cả các con vật đã được đặt trước con vật hiện tại. 
4. Duyệt mảng đã sắp xếp từ trái sang phải. Đối với thời gian thi hiện tại`x`, thêm vào`prefix`ĐẾN`answer`, bởi vì chính xác`prefix`đơn vị thời gian đã trôi qua trước khi con vật này có thể vào phòng khám của bác sĩ. 
5. Thêm`x`ĐẾN`prefix`. Con vật hiện tại đã được xử lý nên thời gian kiểm tra của nó trở thành một phần của thời gian chờ đợi cho mọi con vật tiếp theo. 
6. Sau khi tất cả các con vật đã được xử lý, xuất ra`answer`. Con vật đầu tiên đóng góp bằng 0 vì`prefix`ban đầu là bằng không. 

### Tại sao nó hoạt động 

Xem xét bất kỳ lịch trình nào có hai lần kiểm tra liền kề`x`Và`y`với`x > y`. Mọi thứ trước hai con vật này đều chiếm một lượng cố định`T`. Theo thứ tự`x, y`, tổng đóng góp của họ là`2T + x`, trong khi theo thứ tự`y, x`nó là`2T + y`. Từ`y < x`, việc hoán đổi chúng sẽ làm giảm tổng thời gian chờ đợi. 

Do đó, một lịch trình tối ưu không thể chứa một sự đảo ngược liền kề. Nếu một mảng không được sắp xếp, nó sẽ bị đảo ngược và việc hoán đổi liên tục các cặp liền kề bị đảo ngược cuối cùng sẽ tạo ra thứ tự không giảm mà không làm tăng kết quả. Như vậy thứ tự sắp xếp là tối ưu. Sau khi sắp xếp, tổng tiền tố chính xác bằng thời gian chờ đợi của mỗi con vật hiện tại, do đó việc tích lũy nó sẽ tạo ra tổng số tiền tố tối thiểu có thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    a.sort()

    prefix = 0
    answer = 0

    for x in a:
        answer += prefix
        prefix += x

    print(answer)

if __name__ == "__main__":
    solve()
```Hai dòng đầu ghi số lượng con vật và thời gian kiểm tra. Chỉ có một ca kiểm thử trong bài toán này, do đó không cần vòng lặp ca kiểm thử bên ngoài.`a.sort()`sắp xếp thời gian kiểm tra theo thứ tự ngắn nhất được yêu cầu. Sắp xếp tích hợp của Python chạy trong`O(n log n)`thời gian và được triển khai đủ hiệu quả cho một triệu số nguyên. 

Vòng lặp cố tình thêm vào`prefix`ĐẾN`answer`trước khi thêm hiện tại`x`ĐẾN`prefix`. Lệnh này là cần thiết. Con vật hiện tại không chờ đợi sự kiểm tra của chính nó, nó chỉ chờ đợi sự kiểm tra của những con vật trước nó. Đối với một mảng`[1, 2, 3]`, chuỗi tiền tố được sử dụng cho câu trả lời là`0, 1, 3`, không`1, 3, 6`. 

Kiểu số nguyên của Python cũng tránh được tình trạng tràn. Câu trả lời tối đa có thể là theo thứ tự`5 * 10^16`, sẽ vượt quá số nguyên 32 bit có dấu, nhưng Python thể hiện chính xác các giá trị đó. 

Kích thước đầu vào lớn nên giải pháp sử dụng`sys.stdin.readline`như yêu cầu. Việc sử dụng bộ nhớ bị chi phối bởi việc lưu trữ mảng một triệu phần tử. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Mẫu đầu tiên là```
5
2 2 2 2 2
```Sắp xếp không thay đổi gì cả. Mỗi con vật yêu cầu hai đơn vị thời gian kiểm tra. 

| Động vật | Thời gian thi | Tiền tố trước động vật | Đã thêm vào câu trả lời | Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | 0 | 0 | 0 | 
| 2 | 2 | 2 | 2 | 2 | 
| 3 | 2 | 4 | 4 | 6 | 
| 4 | 2 | 6 | 6 | 12 | 
| 5 | 2 | 8 | 8 | 20 | 

Câu trả lời cuối cùng là`20`. Điều này cũng chứng tỏ rằng con vật đầu tiên không đóng góp thời gian chờ đợi, trong khi mọi con vật sau đó đều phải đợi tất cả các cuộc kiểm tra trước nó. 

### Mẫu 2 

Mẫu thứ hai là```
5
5 1 2 7 3
```Sau khi phân loại, thời gian kiểm tra là`[1, 2, 3, 5, 7]`. 

| Động vật | Thời gian thi | Tiền tố trước động vật | Đã thêm vào câu trả lời | Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | 0 | 0 | 
| 2 | 2 | 1 | 1 | 1 | 
| 3 | 3 | 3 | 3 | 4 | 
| 4 | 5 | 6 | 6 | 10 | 
| 5 | 7 | 11 | 11 | 21 | 

Câu trả lời cuối cùng là`21`. Dấu vết cho thấy trực tiếp bất biến trung tâm: trước khi xử lý từng con vật,`prefix`chính xác là khoảng thời gian mà con vật đó phải đợi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(n log n)`| Việc sắp xếp chiếm ưu thế trong quá trình quét tuyến tính | 
| Không gian |`O(n)`| Mảng lưu trữ tất cả`n`lần thi | 

Vì`n = 10^6`, việc sắp xếp một triệu số nguyên là thực tế, trong khi lần quét sau chỉ mất`O(n)`các hoạt động bổ sung. Câu trả lời có thể đạt được một cách đại khái`5 * 10^16`và số nguyên Python thể hiện nó một cách an toàn. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve():
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))

    a.sort()

    prefix = 0
    answer = 0

    for x in a:
        answer += prefix
        prefix += x

    print(answer)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solve()
        return sys.stdout.getvalue().strip()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided samples
assert run("5\n2 2 2 2 2\n") == "20", "sample 1"
assert run("5\n5 1 2 7 3\n") == "21", "sample 2"

# Minimum-size input
assert run("1\n7\n") == "0", "one animal waits zero time"

# Already sorted, checks that the prefix is added before the current duration
assert run("3\n1 2 3\n") == "4", "already sorted input"

# Reverse order, checks that sorting is actually performed
assert run("3\n7 2 1\n") == "4", "reverse order"

# All values at the maximum boundary
assert run("4\n100000 100000 100000 100000\n") == "600000", "maximum ai"

# Large n and large values, checks that the answer exceeds 32-bit range
assert run("1000000\n" + "100000 " * 1000000) == "49999950000000000", "maximum n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 7`|`0`| Kích thước tối thiểu và thời gian chờ bằng không cho con vật đầu tiên | 
|`3 / 1 2 3`|`4`| Tiền tố phải được thêm trước thời hạn hiện tại | 
|`3 / 7 2 1`|`4`| Sắp xếp là cần thiết | 
|`4 / 100000 100000 100000 100000`|`600000`| Giới hạn thời gian thi tối đa | 
|`1000000 / all values 100000`|`49999950000000000`| Tối đa`n`và kết quả số nguyên lớn | 

Thử nghiệm cuối cùng là cố ý lớn. Nó kiểm tra cả hành vi tiệm cận và thực tế là việc triển khai không vô tình sử dụng bộ tích lũy 32 bit có chiều rộng cố định. 

## Vỏ cạnh 

Đối với một con vật, đầu vào```
1
7
```được sắp xếp không thay đổi. Vòng lặp bắt đầu với`prefix = 0`, vì vậy câu trả lời nhận được`0`. Sau đó`prefix`trở thành`7`, nhưng không có con vật tiếp theo nào có thể đợi được điều đó. Đầu ra là`0`. 

Để có thời gian thi ngang nhau```
5
2 2 2 2 2
```mọi thứ tự có thể đều có kết quả giống hệt nhau. Thuật toán xử lý năm giá trị và thêm tiền tố`0, 2, 4, 6, 8`, sản xuất`20`. Không có cách xử lý đặc biệt nào đối với các giá trị bằng nhau vì đối số trao đổi chỉ yêu cầu một cuộc kiểm tra dài hơn không được đặt trước một cuộc kiểm tra ngắn hơn. 

Đối với đầu vào mà con vật ngắn nhất xuất hiện cuối cùng,```
3
7 1 2
```sắp xếp thay đổi thứ tự thành`1, 2, 7`. Các tiền tố là`0, 1, 3`, vậy câu trả lời là`4`. Đơn hàng ban đầu sẽ sản xuất`15`, chứng minh tại sao việc giữ nguyên thứ tự đầu vào là không hợp lệ. 

Để có thời gian thi tối đa,```
4
100000 100000 100000 100000
```tiền tố là`0, 100000, 200000, 300000`, cho`600000`. Bản thân các giá trị vừa vặn thoải mái trong phạm vi số nguyên thông thường, nhưng câu trả lời tích lũy sẽ tăng nhanh hơn nhiều. 

Để có số lượng động vật tối đa với thời gian kiểm tra tối đa, mọi con vật ngoại trừ con đầu tiên sẽ đợi bội số tăng dần của`100000`. Tổng kết quả là`100000 * (0 + 1 + 2 + ... + 999999) = 49999950000000000`. 

Việc triển khai tính toán điều này trực tiếp bằng cách sử dụng bất biến tổng tiền tố, không mô phỏng các đơn vị thời gian riêng lẻ và không có hành vi bậc hai.
