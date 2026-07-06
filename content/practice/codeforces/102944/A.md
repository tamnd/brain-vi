---
title: "CF 102944A - Ann Arbor"
description: "Chúng tôi được cung cấp một nhật ký đơn giản hàng ngày về lượng khách hàng đến quán trà sữa. Mỗi ngày có một số lượng khách hàng và mỗi khi tổng số lượng khách hàng đạt bội số của một giá trị cố định k thì khách hàng đó sẽ nhận được một đồ uống miễn phí."
date: "2026-07-04T07:35:13+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102944
codeforces_index: "A"
codeforces_contest_name: "UMPT 2020-2021 Team Tryout Contest"
rating: 0
weight: 102944
solve_time_s: 41
verified: true
draft: false
---

[CF 102944A - Ann Arbor](https://codeforces.com/problemset/problem/102944/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 41s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một nhật ký đơn giản hàng ngày về lượng khách hàng đến quán trà sữa. Mỗi ngày có một số lượng khách hàng và mỗi khi tổng số lượng khách hàng đạt bội số của một giá trị cố định`k`, khách hàng đó sẽ nhận được một đồ uống miễn phí. Việc đếm được thực hiện liên tục đối với các khách hàng trong một ngày nhưng sẽ đặt lại mỗi ngày khi chúng tôi chỉ quan tâm đến việc liệu có ít nhất một bội số của`k`bị đánh trong ngày hôm đó. 

Nhiệm vụ không phải là mô phỏng phần thưởng trên toàn cầu mà là kiểm tra độc lập từng ngày và xác định xem ngày đó có chứa ít nhất một khách hàng có vị trí trong đơn hàng đến chia hết cho`k`. Nếu mỗi ngày có ít nhất một khách hàng như vậy thì chúng ta sẽ xuất ra`"awesome"`. Nếu không, chúng tôi sẽ xuất chỉ mục ngày đầu tiên mà không có khách hàng nào như vậy tồn tại. 

Kích thước đầu vào nhỏ: cả hai`k`Và`D`tối đa là 1000 và mỗi ngày có tối đa 2000 khách hàng. Điều này ngay lập tức loại trừ mọi nhu cầu về cấu trúc dữ liệu nâng cao hoặc tối ưu hóa ngoài chức năng quét tuyến tính. Ngay cả một phép tính đơn giản mỗi ngày cũng nằm trong giới hạn thoải mái vì tổng công việc tối đa là khoảng 2 triệu lần kiểm tra. 

Một trường hợp phức tạp xuất hiện khi một ngày có ít hơn`k`khách hàng. Trong trường hợp đó, không có bội số của`k`trong phạm vi đó, vì vậy câu trả lời cho ngày hôm đó đương nhiên là “không có đồ uống miễn phí”. Ví dụ, nếu`k = 10`Và`a_i = 5`, ngày đó chẳng đóng góp gì cả. Việc thực hiện bất cẩn chỉ kiểm tra xem`a_i % k == 0`sẽ không chính xác, vì có chính xác 10, 20, 30 khách hàng quan trọng chứ không chỉ chia hết cho tổng số. 

Một trường hợp góc khác là khi`k = 1`. Mọi khách hàng đều đủ điều kiện, vì vậy mỗi ngày có ít nhất một khách hàng phải thành công và ngay cả những ngày không có khách hàng nào vẫn không sản xuất đồ uống miễn phí, nghĩa là sẽ thất bại nếu có ngày nào như vậy tồn tại. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là mô phỏng mỗi ngày. Trong một ngày nhất định`i`với`a_i`khách hàng, chúng tôi lặp qua các vị trí của khách hàng từ 1 đến`a_i`và kiểm tra xem vị trí nào có thể chia hết cho`k`. Nếu chúng tôi tìm thấy ít nhất một vị trí như vậy thì ngày đó hợp lệ; nếu không, nó không hợp lệ. 

Điều này hoạt động vì vấn đề giảm xuống việc kiểm tra sự tồn tại của một số nguyên`x`TRONG`[1, a_i]`như vậy`x % k == 0`. Tuy nhiên, cách nhìn thô bạo này hơi lãng phí: nó quét tất cả khách hàng mặc dù chúng tôi chỉ quan tâm đến việc liệu bội số của`k`tồn tại trong phạm vi. 

Quan sát quan trọng là bội số của`k`xuất hiện ở các vị trí`k, 2k, 3k, ...`. Vì vậy, một ngày có một đồ uống miễn phí khi và chỉ khi`a_i >= k`. Một lần`a_i`ít nhất là`k`, cái`k`-khách hàng thứ trong ngày đó tồn tại và đảm bảo có ít nhất một đồ uống miễn phí. Nếu như`a_i < k`, không có bội số của`k`không hề. 

Điều này làm giảm mỗi ngày thành một so sánh duy nhất, biến toàn bộ vấn đề thành một lần quét đơn giản trên mảng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(D * tối đa a_i) | O(1) | Đã chấp nhận | 
| Quan sát trực tiếp | O(D) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng ngày một trong khi theo dõi xem mỗi ngày có thỏa mãn điều kiện hay không. 

1. Đọc`k`Và`D`, sau đó đọc mảng số lượng khách hàng hàng ngày. Điều này cung cấp tất cả thông tin cần thiết để đánh giá mỗi ngày một cách độc lập. 
2. Cho mỗi ngày`i`, kiểm tra xem`a_i >= k`. Điều kiện này nắm bắt liệu có ít nhất một bội số của`k`tồn tại giữa các vị trí khách hàng ngày hôm đó. Lý do là khách hàng đủ điều kiện đầu tiên luôn là người`k`-thứ đến. 
3. Nếu`a_i < k`, chúng tôi biết ngay ngày này không có đồ uống miễn phí và chúng tôi có thể ngừng xử lý các ngày tiếp theo. Chúng tôi xuất ra`i`như ngày thất bại đầu tiên. 
4. Nếu chúng tôi hoàn thành cả ngày mà không gặp lỗi nào, chúng tôi sẽ xuất ra`"awesome"`. 

### Tại sao nó hoạt động 

Trong vòng một ngày, đồ uống miễn phí diễn ra chính xác ở các vị trí gấp bội số`k`. Vị trí nhỏ nhất như vậy là`k`, do đó sự tồn tại của bất kỳ khách hàng đủ điều kiện nào chỉ phụ thuộc vào việc ngày đó có ít nhất`k`khách hàng. Điều này tạo ra một thuộc tính đơn điệu: một lần`a_i`thánh giá`k`, thành công được đảm bảo cho ngày hôm đó, và dưới đây`k`, thành công là điều không thể. Vì mỗi ngày là độc lập nên việc quét theo thứ tự sẽ xác định chính xác lỗi đầu tiên. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    k, D = map(int, input().split())
    a = list(map(int, input().split()))

    for i in range(D):
        if a[i] < k:
            print(i + 1)
            return

    print("awesome")

if __name__ == "__main__":
    solve()
```Giải pháp đọc các tham số và lặp lại số lượng khách hàng của mỗi ngày một lần. Quyết định cốt lõi là sự so sánh`a[i] < k`, mã hóa trực tiếp xem có bội số nào của`k`tồn tại theo trình tự của ngày hôm đó. Việc trả lại sớm đảm bảo chúng tôi sẽ dừng ngay ngày vi phạm đầu tiên. 

Một lỗi phổ biến là thử mô phỏng các chỉ số khách hàng hoặc kiểm tra điều kiện chia hết trên`a[i]`chính nó. Giải thích đúng là về các vị trí trong ngày chứ không phải thuộc tính của tổng số. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
10 5
14 33 5 58 74
```| Ngày | a_i | a_i < k | Kết quả cho đến nay | 
| --- | --- | --- | --- | 
| 1 | 14 | Không | hợp lệ | 
| 2 | 33 | Không | hợp lệ | 
| 3 | 5 | Có | thất bại ở ngày thứ 3 | 

Ngày thứ 3 chỉ có 5 khách nên không có vị trí nào đạt 10. Đây là lần thất bại đầu tiên nên sản lượng`3`. 

### Ví dụ 2 

đầu vào:```
10 5
22 71 79 94 50
```| Ngày | a_i | a_i < k | Kết quả cho đến nay | 
| --- | --- | --- | --- | 
| 1 | 22 | Không | hợp lệ | 
| 2 | 71 | Không | hợp lệ | 
| 3 | 79 | Không | hợp lệ | 
| 4 | 94 | Không | hợp lệ | 
| 5 | 50 | Không | hợp lệ | 

Mỗi ngày có ít nhất 10 khách hàng nên mỗi ngày bao gồm các vị trí 10, 20, v.v. Không xảy ra sai sót nên sản lượng đạt`"awesome"`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(D) | Mỗi ngày được kiểm tra một lần với sự so sánh liên tục theo thời gian | 
| Không gian | O(1) | Chỉ có mảng đầu vào và một vài biến được lưu trữ | 

Các ràng buộc cho phép lên tới 1000 ngày, do đó, một lần quét tuyến tính đơn lẻ là không đáng kể về mặt thời gian chạy. Ngay cả một giải pháp O(D * a_i) kém tối ưu hơn cũng có thể vượt qua một cách thoải mái, nhưng việc so sánh trực tiếp sẽ khiến giải pháp trở nên ngay lập tức. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided samples
assert run("10 5\n14 33 5 58 74\n") == "3"
assert run("10 5\n22 71 79 94 50\n") == "awesome"

# k = 1, always valid unless zero customers exist
assert run("1 3\n5 0 2\n") == "2"

# minimum edge: first day fails immediately
assert run("10 3\n1 20 30\n") == "1"

# all days valid
assert run("5 4\n5 5 5 5\n") == "awesome"

# boundary: exactly k customers
assert run("7 3\n7 6 7\n") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| k=1 trộn với 0 | 2 | ngày không có khách hàng thất bại | 
| thất bại ngay lập tức | 1 | thoát sớm đúng đắn | 
| tất cả đều có giá trị như nhau | tuyệt vời | trường hợp vượt qua đầy đủ | 
| ranh giới k chính xác | 2 | nghiêm ngặt`< k`logic | 

## Vỏ cạnh 

Khi nào`k = 1`, mỗi khách hàng là bội số của`k`, tức là bất kỳ ngày nào có ít nhất một khách hàng sẽ vượt qua. Thuật toán xử lý việc này một cách chính xác vì`a_i < 1`chỉ đúng khi`a_i = 0`, nên chỉ có những ngày trống rỗng mới thất bại. 

Khi một ngày có chính xác`k - 1`khách hàng, không có nhiều`k`trong phạm vi. Ví dụ,`k = 10`Và`a_i = 9`dẫn đến thất bại. Séc`a_i < k`nắm bắt chính xác ranh giới này mà không cần lặp lại rõ ràng. 

Khi một ngày có chính xác`k`khách hàng, các`k`-khách hàng thứ tồn tại và đảm bảo thành công. Ví dụ,`k = 10`,`a_i = 10`thành công ngay lập tức vì vị trí 10 là bội số của 10.
