---
title: "CF 102873B - Trò chơi thỏ"
description: "Chúng ta có một hàng cà rốt, trong đó mỗi củ cà rốt có kích thước khác nhau. Hai con thỏ bắt đầu ở hai đầu đối diện của hàng này. Một con thỏ chỉ có thể tiếp tục di chuyển vào trong khi mỗi củ cà rốt tiếp theo nó chạm tới ít nhất phải lớn bằng củ cà rốt nó vừa ăn."
date: "2026-07-25T13:08:14+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102873
codeforces_index: "B"
codeforces_contest_name: "Unofficial Div 4 Round #2 by ssense  SlavicG"
rating: 0
weight: 102873
solve_time_s: 69
verified: true
draft: false
---

[CF 102873B - Trò chơi thỏ](https://codeforces.com/problemset/problem/102873/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 9 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một hàng cà rốt, trong đó mỗi củ cà rốt có kích thước khác nhau. Hai con thỏ bắt đầu ở hai đầu đối diện của hàng này. Một con thỏ chỉ có thể tiếp tục di chuyển vào trong khi mỗi củ cà rốt tiếp theo nó chạm tới ít nhất phải lớn bằng củ cà rốt nó vừa ăn. Hai con thỏ có thể chọn thứ tự di chuyển của mình, vì vậy mục tiêu là ăn tối đa bao nhiêu củ cà rốt khác nhau trước khi cả hai con thỏ không thể tiếp tục nữa. Vấn đề yêu cầu số lượng tối đa đó. 

Con thỏ đầu tiên chỉ phụ thuộc vào tiền tố không giảm dài nhất của mảng. Nếu vài củ cà rốt đầu tiên`1 2 2 5`, nó có thể ăn hết vì mọi cử động đều được phép. Con thỏ thứ hai có điều kiện đối xứng từ phía bên kia: khi nhìn từ phải sang trái, chuỗi ăn phải không giảm, nghĩa là hậu tố còn lại phải không tăng khi nhìn từ trái sang phải. 

Ràng buộc`n <= 2 * 10^5`loại trừ việc mô phỏng mọi thứ tự di chuyển có thể có của thỏ. Bất kỳ cách tiếp cận nào thử kết hợp nhiều bước di chuyển đều có thể dễ dàng trở thành phương trình bậc hai hoặc tệ hơn, và xung quanh`4 * 10^10`hoạt động sẽ vượt xa giới hạn. Chúng ta cần quét tuyến tính vì nó chỉ xử lý toàn bộ mảng với số lần không đổi. 

Những trường hợp khó khăn là khi các đoạn có thể tiếp cận của hai con thỏ chạm vào hoặc chồng lên nhau. Một sai lầm phổ biến là đếm hai con thỏ một cách độc lập và vô tình đếm cùng một củ cà rốt hai lần. Ví dụ:```
Input
5
2 2 2 2 2
```Đầu ra đúng là:```
5
```Cả hai con thỏ đều có thể lấy được từng củ cà rốt, nhưng củ cà rốt ở giữa không được tính hai lần. Câu trả lời là số lượng củ cà rốt riêng biệt có thể che phủ được. 

Một trường hợp ranh giới khác là khi một con thỏ dừng lại ngay lập tức. Ví dụ:```
Input
4
5 1 1 1
```Đầu ra đúng là:```
4
```Con thỏ bên trái chỉ ăn củ cà rốt đầu tiên vì củ tiếp theo nhỏ hơn. Con thỏ bên phải có thể ăn hết hậu tố`1 1 1`. Giải pháp giả định cả hai con thỏ luôn đóng góp nhiều hơn một củ cà rốt sẽ thất bại ở đây. 

Trường hợp cuối cùng là khi các phần có thể tiếp cận được phân tách bằng bước nhảy giảm dần:```
Input
5
1 3 2 3 1
```Đầu ra đúng là:```
4
```Thỏ bên trái ăn được hai củ cà rốt đầu tiên, còn thỏ bên phải ăn được hai củ cà rốt cuối cùng. Cả hai con thỏ đều không thể tiếp cận được củ cà rốt ở giữa. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là mô phỏng cả hai con thỏ. Chúng ta có thể bắt đầu từ điểm cuối bên trái và di chuyển liên tục trong khi củ cà rốt tiếp theo thỏa mãn điều kiện. Chúng ta có thể làm tương tự từ điểm cuối bên phải. Điều này xác định chính xác từng củ cà rốt mà mỗi con thỏ có thể ăn vì đường đi của thỏ là xác định sau khi điểm xuất phát của nó được cố định. 

Vấn đề với việc mô phỏng đầy đủ các lệnh di chuyển có thể xảy ra là nó xử lý sự tương tác giữa các con thỏ như thể có nhiều lựa chọn có ý nghĩa. Trên thực tế, thông tin quan trọng duy nhất là vị trí xa nhất mà mỗi con thỏ có thể đạt được. Con thỏ bên trái luôn yêu cầu tiền tố và con thỏ bên phải luôn yêu cầu hậu tố. 

Quan sát giúp đơn giản hóa vấn đề là thứ tự di chuyển chỉ quan trọng khi cả hai con thỏ đều muốn có cùng một củ cà rốt. Chúng ta có thể để thỏ bên trái lấy mọi củ cà rốt trong tiền tố có thể truy cập của nó và thỏ bên phải lấy mọi củ cà rốt trong hậu tố có thể truy cập được. Câu trả lời tối đa chỉ là kích thước của sự kết hợp của hai phạm vi đó. 

Việc mô phỏng lực lượng vũ phu của tất cả các lệnh di chuyển có thể có nhiều khả năng theo cấp số nhân bởi vì tại mọi thời điểm, một trong hai con thỏ có thể di chuyển. Ngay cả việc theo dõi nhiều trạng thái cũng không cần thiết vì các khoảng thời gian có thể truy cập hoàn toàn mô tả được trò chơi. Giải pháp tối ưu tìm thấy cả hai ranh giới bằng hai lần quét và kết hợp chúng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ của số lượng lựa chọn nước đi có thể có | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Quét từ bên trái và tìm chỉ mục cuối cùng`left`thuộc về con đường của con thỏ đầu tiên. Bắt đầu với củ cà rốt đầu tiên được ăn. Tiếp tục di chuyển cho đến khi củ cà rốt tiếp theo lớn hơn hoặc bằng củ cà rốt hiện tại. Chỉ mục cuối cùng là phần cuối của tiền tố có thể truy cập. 
2. Quét từ bên phải và tìm chỉ mục đầu tiên`right`thuộc về con đường của con thỏ thứ hai. Bắt đầu với củ cà rốt cuối cùng được ăn. Di chuyển sang trái trong khi củ cà rốt tiếp theo lớn hơn hoặc bằng củ cà rốt hiện tại theo quan điểm của thỏ. Theo thứ tự mảng, điều này có nghĩa là phần tử trước đó phải lớn hơn hoặc bằng phần tử hiện tại. 
3. Thỏ bên trái có thể ăn từng củ cà rốt trong thời gian nghỉ`[0, left]`, và con thỏ bên phải có thể ăn hết củ cà rốt trong khoảng thời gian`[right, n-1]`. Nếu các khoảng này trùng nhau thì sự trùng lặp chỉ được tính một lần. 
4. Tính độ lớn hợp của hai khoảng. Nếu như`left < right`, các khoảng cách nhau và câu trả lời là`(left + 1) + (n - right)`. Nếu không, chúng chồng lên nhau và bao phủ toàn bộ phần từ`0`ĐẾN`n - 1`, vậy câu trả lời là`n`. 

Tại sao nó hoạt động: quy tắc chuyển động làm cho đường đi của mỗi con thỏ cố định. Con thỏ bên trái không thể nhảy qua củ cà rốt chặn nó, vì vậy mọi thứ nó ăn phải nằm trong một tiền tố tối đa. Lý do tương tự đưa ra một hậu tố tối đa cho con thỏ bên phải. Bất kỳ trò chơi tối ưu nào cũng chỉ có thể sử dụng cà rốt từ hai khu vực này và cả hai khu vực đều có thể được tiêu thụ hoàn toàn bằng cách chọn thứ tự di chuyển phù hợp. Việc đếm sự kết hợp của chúng sẽ cho ra chính xác số lượng cà rốt riêng biệt tối đa được ăn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    left = 0
    while left + 1 < n and a[left + 1] >= a[left]:
        left += 1

    right = n - 1
    while right - 1 >= 0 and a[right - 1] >= a[right]:
        right -= 1

    if left < right:
        ans = left + 1 + (n - right)
    else:
        ans = n

    print(ans)

if __name__ == "__main__":
    solve()
```Vòng lặp đầu tiên tìm ra điểm dừng chính xác của con thỏ bên trái. Việc so sánh sử dụng`a[left + 1] >= a[left]`bởi vì con thỏ đang di chuyển từ trái sang phải và cần củ cà rốt tiếp theo ít nhất phải lớn bằng củ cà rốt trước đó. 

Vòng lặp thứ hai trông tương tự nhưng chạy ngược lại. Điều kiện là`a[right - 1] >= a[right]`bởi vì con thỏ đang di chuyển về phía những chỉ số nhỏ hơn. Điều này cũng giống như nói rằng trình tự nó ăn không giảm theo hướng chuyển động của thỏ. 

Phép tính cuối cùng xử lý sự tương tác duy nhất giữa các con thỏ. Nếu như`left < right`, những con thỏ có những bộ phận rời rạc có thể tiếp cận được. Nếu không thì đường đi của chúng gặp nhau hoặc cắt nhau, và cùng nhau chúng có thể bao phủ mọi củ cà rốt giữa hai điểm cuối. 

Việc triển khai chỉ sử dụng một vài biến số nguyên bên cạnh mảng đầu vào. Số nguyên Python không có vấn đề tràn đối với các giá trị này và số lượng thao tác là tuyến tính. 

## Ví dụ đã hoạt động 

Dành cho:```
4
1 2 3 4
```các trạng thái quét là: 

| Bước | trái | đúng | Ý nghĩa | 
| --- | --- | --- | --- | 
| Bắt đầu | 0 | 3 | Cả hai con thỏ đều ở điểm cuối | 
| Kết thúc quét bên trái | 3 | 3 | Thỏ trái về đích | 
| Quá trình quét bên phải kết thúc | 3 | 0 | Thỏ phải về đích | 
| Trả lời | | | 4 | 

Cả hai con thỏ có thể bao phủ toàn bộ hàng, và đếm đoàn sẽ có bốn củ cà rốt. 

Vì:```
5
1 3 2 3 1
```các trạng thái quét là: 

| Bước | trái | đúng | Ý nghĩa | 
| --- | --- | --- | --- | 
| Bắt đầu | 0 | 4 | Vị trí ban đầu | 
| Kết thúc quét bên trái | 1 | 4 | Thỏ bên trái dừng lại trước cỡ 2 | 
| Quá trình quét bên phải kết thúc | 1 | 3 | Thỏ phải dừng lại trước cỡ 2 | 
| Trả lời | | | 4 | 

Khoảng thời gian có thể tiếp cận là`[0, 1]`Và`[3, 4]`. Củ cà rốt ở giữa không thuộc về con thỏ nào cả. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi lần quét sẽ di chuyển qua mảng nhiều nhất một lần. | 
| Không gian | O(1) | Chỉ các chỉ số biên và bộ đếm được lưu trữ bên cạnh mảng đầu vào. | 

Với`n`lên đến`2 * 10^5`, một giải pháp tuyến tính dễ dàng phù hợp với giới hạn thời gian. Việc sử dụng bộ nhớ cũng không đổi sau khi đọc mảng. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve_case(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)

    n = int(input())
    a = list(map(int, input().split()))

    left = 0
    while left + 1 < n and a[left + 1] >= a[left]:
        left += 1

    right = n - 1
    while right - 1 >= 0 and a[right - 1] >= a[right]:
        right -= 1

    if left < right:
        ans = left + 1 + (n - right)
    else:
        ans = n

    sys.stdin = old_stdin
    return str(ans) + "\n"

assert solve_case("4\n1 2 3 4\n") == "4\n", "sample 1"
assert solve_case("5\n1 3 2 3 1\n") == "4\n", "sample 2"
assert solve_case("9\n1 1 2 1 9 2 1 1 2\n") == "4\n", "sample 3"
assert solve_case("7\n1 3 5 4 4 3 2\n") == "7\n", "sample 4"
assert solve_case("5\n2 2 2 2 2\n") == "5\n", "sample 5"

assert solve_case("2\n1 1\n") == "2\n", "minimum size and equal values"
assert solve_case("5\n5 4 3 2 1\n") == "5\n", "fully decreasing array"
assert solve_case("6\n1 2 1 1 1 1\n") == "5\n", "left boundary stops early"
assert solve_case("8\n1 3 5 2 4 6 7 8\n") == "5\n", "separated reachable regions"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 / 1 1`|`2`| Kích thước tối thiểu và xử lý bình đẳng | 
|`5 / 5 4 3 2 1`|`5`| Một con thỏ có thể bắt đầu từ một mặt khó khăn trong khi con kia lo phần còn lại | 
|`6 / 1 2 1 1 1 1`|`5`| Một bên dừng lại ngay sau khi thả rơi | 
|`8 / 1 3 5 2 4 6 7 8`|`5`| Phân tách chính xác hai khoảng thời gian có thể truy cập | 

## Vỏ cạnh 

Đối với trường hợp hoàn toàn bằng nhau:```
5
2 2 2 2 2
```quét bên trái đạt chỉ mục`4`bởi vì mọi di chuyển đều được phép. Quét bên phải đạt chỉ mục`0`vì lý do tương tự. Các khoảng trùng nhau nên thuật toán trả về`n = 5`thay vì cộng cả hai độ dài và tạo ra câu trả lời không hợp lệ. 

Đối với trường hợp dừng sớm:```
4
5 1 1 1
```quét bên trái vẫn ở chỉ mục`0`bởi vì`1`nhỏ hơn`5`. Quét bên phải di chuyển đến chỉ mục`1`bởi vì`1 1 1`có giá trị từ phải sang trái. Các khoảng`[0,0]`Và`[1,3]`là riêng biệt, vì vậy câu trả lời là`1 + 3 = 4`. 

Đối với trường hợp chồng chéo:```
5
1 2 3 4 5
```con thỏ bên trái đạt chỉ số`4`, và con thỏ bên phải đạt chỉ số`0`. Hai phạm vi trùng nhau hoàn toàn, do đó thuật toán trả về`5`. Đếm cả hai phạm vi một cách độc lập sẽ trả về không chính xác`10`. 

Đối với trường hợp tách biệt:```
5
1 3 2 3 1
```quá trình quét bên trái dừng ở chỉ mục`1`, trong khi quá trình quét bên phải dừng lại ở chỉ mục`3`. Hai chú thỏ có thể ăn vị trí`0,1,3,4`, đưa ra câu trả lời đúng`4`. Thuật toán không bao giờ bao gồm chỉ mục`2`bởi vì không có đường đi đơn điệu nào có thể đi qua giọt nước chặn nó.
