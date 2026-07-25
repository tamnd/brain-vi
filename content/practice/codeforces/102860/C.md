---
title: "CF 102860C - Trò chơi"
description: "Chỉnh sửa Trò chơi bao gồm các vòng độc lập. Khi bắt đầu mỗi vòng, điểm sẽ trở thành n. Mỗi giá trị trình tạo đại diện cho một nước đi: Petya cố gắng trừ giá trị đó khỏi điểm hiện tại, nhưng chỉ áp dụng các phép trừ thành công."
date: "2026-07-25T14:19:05+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102860
codeforces_index: "C"
codeforces_contest_name: "2020-2021 Saint-Petersburg Open High School Programming Contest (SpbKOSHP 20)"
rating: 0
weight: 102860
solve_time_s: 36
verified: true
draft: false
---

[CF 102860C - Trò chơi](https://codeforces.com/problemset/problem/102860/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 36s 
**Đã xác minh:** có 

##Giải pháp 
Chỉnh sửa 

#Hiểu vấn đề 

Trò chơi bao gồm các vòng độc lập. Khi bắt đầu mỗi vòng, điểm số sẽ trở thành`n`. Mỗi giá trị trình tạo đại diện cho một nước đi: Petya cố gắng trừ giá trị đó khỏi điểm hiện tại, nhưng chỉ áp dụng các phép trừ thành công. Khi điểm đạt chính xác bằng 0, vòng hiện tại được hoàn thành và nước đi tiếp theo, nếu có, sẽ bắt đầu từ điểm mới là`n`. 

Đầu vào cung cấp số lần di chuyển, điểm ban đầu của một vòng và toàn bộ chuỗi đầu ra của trình tạo. Nhiệm vụ là chơi lại các nước đi này và xác định xem có bao nhiêu hiệp đã hoàn thành đầy đủ và số điểm còn lại sau nước đi được ghi cuối cùng. Nếu nước đi cuối cùng kết thúc một hiệp thì không có hiệp mới nào bắt đầu sau đó nên điểm cuối cùng là 0. 

Các giới hạn đủ nhỏ để mô phỏng trực tiếp. Có nhiều nhất`100000`các nước đi, do đó, một thuật toán xử lý mỗi nước đi một lần chỉ thực hiện được khoảng`100000`hoạt động. Bất kỳ cách tiếp cận nào cố gắng xây dựng tất cả các phép chia vòng có thể có hoặc quét liên tục trình tự sẽ tạo thêm công việc không cần thiết, nhưng thời gian tuyến tính dễ dàng nằm trong giới hạn. 

Các trường hợp đặc biệt chính đến từ việc xử lý chính xác các phép trừ thất bại và thời điểm chính xác mà một vòng đấu kết thúc. Ví dụ, với đầu vào```
1 5
7
```đầu ra là```
0
5
```bởi vì`7`không thể bị trừ khỏi`5`, do đó điểm số không thay đổi. Một giải pháp bất cẩn luôn trừ đi giá trị của trình tạo sẽ tạo ra điểm âm không hợp lệ. 

Một trường hợp khác là```
1 5
5
```với đầu ra```
1
0
```bởi vì nước đi duy nhất sẽ hoàn thành vòng chơi. Một lỗi phổ biến là đặt lại điểm về`5`ngay sau khi về 0, điều này sẽ báo cáo không chính xác điểm cuối cùng là điểm bắt đầu của một vòng mới. 

Trường hợp ranh giới thứ ba là```
3 5
2 2 2
```với đầu ra```
0
1
```Các bước di chuyển thay đổi điểm số như`5 -> 3 -> 1 -> 1`, vì phép trừ cuối cùng là không thể. Một giải pháp tính mọi giá trị của trình tạo khi tiến tới một vòng hoàn thành sẽ bị tính quá mức. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất là mô phỏng trò chơi một cách chính xác. Chúng tôi giữ số điểm hiện tại và số vòng hoàn thành. Đối với mỗi giá trị của trình tạo, chúng tôi kiểm tra xem việc trừ nó có giữ cho điểm không âm hay không. Nếu có, chúng tôi áp dụng phép trừ. Khi điểm số bằng 0 nghĩa là đã hết một hiệp nên chúng ta tăng đáp án và chuẩn bị điểm cho nước đi tiếp theo. 

Cách giải thích bạo lực thực sự giống với phương pháp tối ưu ở đây. Không cần thiết phải có một phím tắt toán học nhanh hơn vì trạng thái trò chơi chỉ thay đổi sau mỗi lần di chuyển và số lần di chuyển bị hạn chế. Một cách tiếp cận giả định thử mọi phân vùng có thể có của chuỗi thành các vòng sẽ khám phá vô số khả năng, nhưng mô phỏng tránh hoàn toàn điều đó bằng cách tuân theo thứ tự sự kiện hợp lệ duy nhất. 

Quan sát mở ra giải pháp là trình tự tạo đã là toàn bộ lịch sử của trò chơi. Không có lựa chọn nào để thực hiện. Trạng thái tiếp theo chỉ phụ thuộc vào điểm hiện tại và giá trị bộ tạo tiếp theo, do đó, chỉ cần vượt qua chuỗi là đủ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(k) | O(1) | Đã chấp nhận | 
| Tối ưu | O(k) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo điểm hiện tại thành`n`và bộ đếm vòng hoàn thành về 0. Điểm số thể hiện trạng thái của Petya trước khi xử lý nước đi đầu tiên được ghi lại. 
2. Xử lý từng giá trị trình tạo theo thứ tự xuất hiện. Nếu giá trị không lớn hơn điểm hiện tại, hãy trừ nó khỏi điểm. Nếu không thì bỏ qua vì luật không cho phép điểm trở thành âm. 
3. Sau mỗi lần trừ thành công, hãy kiểm tra xem điểm có bằng 0 hay không. Nếu đúng như vậy, hãy tăng bộ đếm vòng đã hoàn thành và khôi phục điểm về`n`cho những bước đi trong tương lai. Việc khôi phục thể hiện sự bắt đầu của vòng tiếp theo, nhưng nó không ảnh hưởng đến câu trả lời cuối cùng nếu nước đi hiện tại là nước đi cuối cùng vì điểm cuối cùng bắt buộc trong tình huống đó là 0. 
4. Sau khi xử lý xong tất cả nước đi, hãy in số vòng đã hoàn thành và số điểm hiện tại. Nếu nước đi cuối cùng kết thúc một hiệp, bước khôi phục sẽ không diễn ra sau nước đi cuối cùng, do đó điểm cuối cùng vẫn bằng 0. 

Việc thực hiện cần một sự điều chỉnh nhỏ cho bước cuối cùng. Chúng ta có thể khôi phục điểm ngay lập tức khi một hiệp đấu kết thúc vì sau vòng lặp chúng ta chỉ cần điểm vào thời điểm trận đấu dừng lại. Để lưu giữ thông tin đó, mã sẽ kiểm tra xem nước đi hiện tại có phải là nước đi cuối cùng hay không trước khi đặt lại. 

Tại sao nó hoạt động: Trong quá trình mô phỏng, điểm được lưu trữ luôn là điểm chính xác mà Petya có được sau tất cả các nước đi được xử lý. Một nước đi được áp dụng chính xác khi luật chơi cho phép và một vòng hoàn thành được tính chính xác khi điểm về 0. Vì mọi nước đi được ghi lại đều được xử lý một lần và theo thứ tự ban đầu nên trạng thái được lưu trữ cuối cùng khớp với trạng thái trò chơi thực. 

## Giải pháp Python```python
import sys

input = sys.stdin.readline

def solve():
    k, n = map(int, input().split())
    a = list(map(int, input().split()))

    score = n
    rounds = 0

    for i, x in enumerate(a):
        if x <= score:
            score -= x

        if score == 0:
            rounds += 1
            if i != k - 1:
                score = n

    print(rounds)
    print(score)

if __name__ == "__main__":
    solve()
```Các biến`score`Và`rounds`trực tiếp đại diện cho hai đại lượng được yêu cầu ở đầu ra. Vòng lặp theo lịch sử trình tạo từ trái sang phải, khớp với thứ tự trò chơi. 

Điều kiện trừ sử dụng`x <= score`thay vì kiểm tra sau phép trừ vì số nguyên Python cho phép giá trị âm, nhưng bản thân trò chơi thì không. Điều này ngăn chặn trạng thái trung gian không hợp lệ. 

Điều kiện đặt lại kiểm tra xem nước đi hiện tại có phải là nước đi cuối cùng hay không. Nếu một vòng đấu kết thúc sớm hơn thì nước đi được ghi tiếp theo sẽ thuộc về một vòng mới và phải bắt đầu từ`n`. Nếu nước đi cuối cùng kết thúc một hiệp thì không có hiệp mới nào bắt đầu nên đáp án phải giữ nguyên số điểm bằng 0. 

Không có thủ thuật lập chỉ mục hoặc mối quan tâm về số nguyên lớn. Các giá trị dễ dàng khớp với kiểu số nguyên của Python và thuật toán chỉ lưu trữ chuỗi đầu vào và một vài biến. 

## Ví dụ đã hoạt động 

Đối với đầu vào mẫu```
4 3
1 2 4 1
```dấu vết là: 

| Di chuyển | Giá trị máy phát điện | Ghi điểm trước khi di chuyển | Điểm sau khi di chuyển | Đã hoàn thành vòng | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 3 | 2 | 0 | 
| 2 | 2 | 2 | 0 | 1 | 
| 3 | 4 | 3 | 3 | 1 | 
| 4 | 1 | 3 | 2 | 1 | 

Nước đi thứ hai kết thúc hiệp đầu tiên. Nước đi thứ ba bắt đầu với số điểm mới là ba, và các nước đi còn lại để lại số điểm cuối cùng là hai. Điều này thể hiện hành vi thiết lập lại giữa các vòng. 

Đối với đầu vào```
5 4
3 1 5 4 2
```dấu vết là: 

| Di chuyển | Giá trị máy phát điện | Ghi điểm trước khi di chuyển | Điểm sau khi di chuyển | Đã hoàn thành vòng | 
| --- | --- | --- | --- | --- | 
| 1 | 3 | 4 | 1 | 0 | 
| 2 | 1 | 1 | 0 | 1 | 
| 3 | 5 | 4 | 4 | 1 | 
| 4 | 4 | 4 | 0 | 2 | 
| 5 | 2 | 4 | 2 | 2 | 

Dấu vết cho thấy hai vòng đã hoàn thành và số điểm còn lại là hai. Nó cũng kiểm tra trường hợp nước đi sau kết thúc một vòng và nước đi khác tiếp tục trò chơi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(k) | Mỗi giá trị trình tạo được xử lý chính xác một lần. | 
| Không gian | O(k) | Việc thực hiện lưu trữ mảng đầu vào. | 

Độ phức tạp về thời gian là tuyến tính theo số lần di chuyển, phù hợp với`k`lên tới`100000`. Việc sử dụng bộ nhớ cũng ở dưới mức giới hạn. Bộ lưu trữ mảng có thể được loại bỏ bằng trình phân tích cú pháp phát trực tuyến, nhưng điều này là không cần thiết đối với những ràng buộc này. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve(data):
    input = io.StringIO(data).readline
    k, n = map(int, input().split())
    a = list(map(int, input().split()))

    score = n
    rounds = 0

    for i, x in enumerate(a):
        if x <= score:
            score -= x
        if score == 0:
            rounds += 1
            if i != k - 1:
                score = n

    return f"{rounds}\n{score}\n"

assert solve("4 3\n1 2 4 1\n") == "1\n2\n", "sample 1"
assert solve("1 5\n5\n") == "1\n0\n", "single completed round"
assert solve("1 5\n7\n") == "0\n5\n", "failed subtraction"
assert solve("3 5\n2 2 2\n") == "0\n1\n", "ignored final move"
assert solve("5 4\n3 1 5 4 2\n") == "2\n2\n", "multiple rounds"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`4 3 / 1 2 4 1`|`1 / 2`| Cung cấp hành vi mẫu | 
|`1 5 / 5`|`1 / 0`| Nước đi cuối cùng kết thúc một hiệp | 
|`1 5 / 7`|`0 / 5`| Phép trừ không hợp lệ bị bỏ qua | 
|`3 5 / 2 2 2`|`0 / 1`| Ranh giới sau khi trừ thất bại | 
|`5 4 / 3 1 5 4 2`|`2 / 2`| Một số vòng đã hoàn thành | 

## Vỏ cạnh 

Đối với trường hợp```
1 5
7
```thuật toán bắt đầu với điểm năm. Giá trị trình tạo duy nhất lớn hơn điểm, do đó không xảy ra phép trừ. Điểm không bao giờ bằng 0, bộ đếm vòng vẫn bằng 0 và điểm cuối cùng là năm. 

Đối với trường hợp```
1 5
5
```phép trừ thành công và thay đổi điểm từ năm thành 0. Thuật toán tính một vòng hoàn thành. Vì đây là nước đi cuối cùng nên nó không đặt lại điểm, để lại điểm cuối cùng bắt buộc là 0. 

Đối với trường hợp```
3 5
2 2 2
```nước đi đầu tiên thay đổi điểm thành ba và nước thứ hai thay đổi thành một. Nước đi thứ ba không thể áp dụng vì hai lớn hơn một nên tỉ số vẫn là một. Thuật toán không bao giờ tính một vòng vì không bao giờ đạt tới số 0. 

Đối với trường hợp```
5 4
3 1 5 4 2
```hai nước đi đầu tiên hoàn thành một hiệp. Nước thứ ba bắt đầu với điểm bốn, thất bại và giữ nguyên điểm. Nước thứ tư hoàn thành vòng thứ hai. Nước đi cuối cùng bắt đầu từ bốn và để lại hai. Thuật toán xử lý chính xác cả điểm đặt lại ở giữa và điểm khác 0 cuối cùng.
