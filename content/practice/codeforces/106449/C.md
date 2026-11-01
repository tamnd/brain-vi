---
title: "CF 106449C - Cân bằng túi"
description: "Bài toán hỏi liệu một tập hợp các mặt hàng tạp hóa có thể được chia thành hai túi sao cho tổng trọng lượng trong cả hai túi bằng nhau hay không. Mỗi món đồ phải cho vào đúng một túi."
date: "2026-06-25T09:21:55+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106449
codeforces_index: "C"
codeforces_contest_name: "2026 Spring UT CS104c Midterm #2"
rating: 0
weight: 106449
solve_time_s: 35
verified: true
draft: false
---

[CF 106449C - Cân bằng túi](https://codeforces.com/problemset/problem/106449/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 35s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Bài toán hỏi liệu một tập hợp các mặt hàng tạp hóa có thể được chia thành hai túi sao cho tổng trọng lượng trong cả hai túi bằng nhau hay không. Mỗi món đồ phải cho vào đúng một túi. Đầu vào cho biết số lượng mặt hàng và trọng lượng của mỗi mặt hàng, còn đầu ra là liệu sự phân chia như vậy có tồn tại hay không. 

Chi tiết quan trọng là tổng trọng lượng của tất cả các mặt hàng đủ nhỏ để có thể quản lý được tổng mục tiêu. Mặc dù số lượng mục có thể lên tới 100 nhưng tổng trọng lượng không vượt quá 1000. Điều này ngay lập tức gợi ý rằng các giải pháp dựa vào tổng trọng lượng hơn là số lượng tập hợp con có thể có là hợp lý. Một giải pháp mạnh mẽ để thử mọi tập hợp con sẽ cần tới$2^{100}$những lựa chọn vượt xa những gì có thể có trong một cuộc thi. Một giải pháp xung quanh$O(N \times S)$, Ở đâu$S$là tổng trọng lượng, có thể dễ dàng chấp nhận được vì$S$nhiều nhất là 1000. 

Một số trường hợp nguy hiểm có thể phá vỡ việc triển khai bất cẩn. Đầu tiên là tổng trọng lượng lẻ. Đối với đầu vào:```
3
1 2 4
```đầu ra đúng là:```
no
```bởi vì hai túi cần mỗi túi chứa một nửa của 7, điều này là không thể. Một giải pháp chỉ kiểm tra xem có tồn tại tập hợp con nào đó mà không xem xét một nửa bắt buộc hay không có thể cho kết quả sai. 

Một trường hợp khác là khi tổng số chẵn nhưng phần chia duy nhất có thể sử dụng một mục duy nhất. Đối với đầu vào:```
3
1 2 3
```đầu ra đúng là:```
yes
```vì một túi có thể chứa vật phẩm có trọng lượng 3 và túi kia có thể chứa vật phẩm có trọng lượng 1 và 2. Việc triển khai giả định rằng cả hai túi đều cần nhiều vật phẩm sẽ không thành công. 

Một sai lầm phổ biến cuối cùng là quên rằng các vật phẩm là những vật thể riêng lẻ. Vì:```
3
100 100 100
```đầu ra là:```
no
```Tổng số là 300, nhưng không có tập hợp con nào có trọng số 150. Chỉ đếm tổng trọng số có thể có mà không theo dõi chính xác các tổng có thể đạt được có thể dẫn đến kết luận sai. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là thử mọi cách phân bổ các vật phẩm có thể có vào túi đầu tiên. Mỗi mục có hai lựa chọn, vì vậy với$N$các mục này tạo ra$2^N$các phân phối có thể có. Đối với mỗi lần phân phối, chúng ta có thể tính trọng lượng của một túi và kiểm tra xem nó có bằng một nửa tổng số hay không. Điều này đúng vì nó khám phá mọi cách có thể để phân chia các mục. Vấn đề là số lượng hoạt động. Với$N = 100$, không gian tìm kiếm bao quanh$1.27 \times 10^{30}$, không thể vượt qua được. 

Quan sát hữu ích là trọng lượng chứ không phải số lượng vật phẩm mới là yếu tố hạn chế. Chúng tôi chỉ quan tâm liệu tổng trọng lượng nhất định có thể được hình thành hay không bằng cách chọn một số mặt hàng. Vì mục tiêu cuối cùng nhiều nhất là 500 nên chúng tôi có thể lưu trữ tất cả số tiền có thể truy cập trong khi xử lý từng mục một. Điều này biến bài toán thành bài toán tổng tập hợp con. 

Lực lượng vũ phu hoạt động vì nó kiểm tra mọi tập hợp con có thể. Nó thất bại vì có quá nhiều tập hợp con. Nhận xét rằng tổng trọng số nhỏ cho phép chúng ta thay thế việc tìm kiếm trên các tập hợp con bằng trạng thái lập trình động đại diện cho tất cả các trọng số có thể đạt được. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^N) | O(1) | Quá chậm | 
| Lập trình động | O(N×S) | O(S) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả trọng lượng của món hàng và tính tổng trọng lượng. Nếu tổng trọng lượng là số lẻ thì xuất ngay`no`bởi vì hai túi bằng nhau sẽ yêu cầu một nửa không nguyên. 
2. Đặt trọng lượng mục tiêu bằng một nửa tổng trọng lượng. Tạo một mảng lập trình động trong đó`dp[x]`có nghĩa là có thể chọn một số mặt hàng được xử lý có tổng trọng lượng chính xác`x`. 
3. Khởi tạo`dp[0]`là đúng vì không chọn mục nào sẽ tạo ra tổng bằng 0. 
4. Xử lý trọng lượng từng mặt hàng. Đối với mỗi số tiền hiện tại có thể có, hãy cập nhật xem việc thêm mục này có tạo ra số tiền mới có thể tiếp cận hay không. Việc lặp lại phải đi từ tổng lớn hơn xuống tổng nhỏ hơn để cùng một mục không được sử dụng nhiều lần. 
5. Sau khi tất cả các mục được xử lý, hãy kiểm tra`dp[target]`. Nếu đúng, có một tập hợp con các mặt hàng cho một túi có trọng lượng chính xác mục tiêu, do đó túi kia tự động có cùng trọng lượng. 

Tại sao nó hoạt động: điều bất biến là sau khi xử lý bất kỳ tiền tố nào của các mục,`dp[x]`đúng chính xác khi một số tập hợp con của tiền tố đó có tổng trọng số`x`. Ban đầu điều này đúng với tiền tố trống. Khi một mục mới được xem xét, mọi số tiền có thể truy cập cũ vẫn có thể truy cập được bằng cách bỏ qua mục đó hoặc tạo ra số tiền có thể truy cập mới bằng cách lấy mục đó. Vì chúng tôi cập nhật ngược nên sản phẩm không thể đóng góp nhiều lần. Sau khi xử lý mọi thứ, trạng thái mục tiêu thể hiện chính xác liệu túi đầu tiên hợp lệ có tồn tại hay không. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n_line = input().strip()
    if not n_line:
        return
    n = int(n_line)
    weights = list(map(int, input().split()))

    total = sum(weights)

    if total % 2:
        print("no")
        return

    target = total // 2
    dp = [False] * (target + 1)
    dp[0] = True

    for w in weights:
        for s in range(target - w, -1, -1):
            if dp[s]:
                dp[s + w] = True

    print("yes" if dp[target] else "no")

if __name__ == "__main__":
    solve()
```Giải pháp đầu tiên tính toán tổng trọng lượng vì trạng thái mục tiêu được xác định bởi yêu cầu số dư cuối cùng. Việc kiểm tra tổng số lẻ sẽ tránh được công việc lập trình động không cần thiết và xử lý ngay lập tức một trường hợp không thể thực hiện được. 

các`dp`mảng chỉ lưu trữ các giá trị từ 0 đến một nửa tổng số vì các tổng trên mục tiêu không bao giờ có thể là một phần của câu trả lời hợp lệ. Vòng lặp ngược là chi tiết triển khai quan trọng. Nếu vòng lặp di chuyển lên trên, trọng số có thể được thêm vào tổng được tạo bằng cách sử dụng cùng một mục trong cùng một lần lặp, điều này sẽ cho phép sử dụng một mục nhiều lần một cách không chính xác. 

Số nguyên Python không có vấn đề tràn ở đây vì giá trị được lưu trữ lớn nhất chỉ là 1000, nhưng việc giới hạn kích thước mảng vẫn giúp việc triển khai đơn giản và hiệu quả. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
6
30 100 70 20 1 21
```Tổng số là 242, vậy mục tiêu là 121. 

| Mặt hàng đã được xử lý | Cân nặng | Trạng thái mục tiêu mới có thể tiếp cận | 
| --- | --- | --- | 
| Bắt đầu | 0 | 0 | 
| 30 | 30 | 30 | 
| 100 | 100 | 100, 130 | 
| 70 | 70 | 70, 100, 121 | 
| 20 | 20 | 20, 30, 50, 70, 90, 120 | 
| 1 | 1 | 121 có thể truy cập được | 
| 21 | 21 | 121 vẫn có thể truy cập được | 

Lập trình động nhận thấy rằng có thể hình thành 121, ví dụ với các trọng số 100, 20 và 1. Các mục còn lại cũng có tổng số 121, vì vậy câu trả lời là`yes`. 

Đối với mẫu thứ hai:```
3
100 100 100
```Tổng cộng là 300, vậy mục tiêu là 150. 

| Mặt hàng đã được xử lý | Cân nặng | Có thể truy cập 150 | 
| --- | --- | --- | 
| Bắt đầu | 0 | Không | 
| 100 | 100 | Không | 
| 100 | 200 | Không | 
| 100 | 300 | Không | 

Không có tập hợp con nào tạo ra trọng số 150 nên kết quả đầu ra là`no`. Điều này chứng tỏ tại sao chỉ kiểm tra xem tổng số có chẵn hay không là không đủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N×S) | Mỗi mục cập nhật tất cả các tổng có thể có cho đến mục tiêu, trong đó S là tổng trọng lượng bị ràng buộc. | 
| Không gian | O(S) | Mảng DP lưu trữ một boolean cho mỗi tổng có thể. | 

Tổng trọng lượng tối đa chỉ là 1000, do đó, lập trình động thực hiện tối đa khoảng 100.000 lần chuyển đổi. Điều này hoàn toàn nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io

def solution(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n = int(input())
    weights = list(map(int, input().split()))

    total = sum(weights)
    if total % 2:
        return "no\n"

    target = total // 2
    dp = [False] * (target + 1)
    dp[0] = True

    for w in weights:
        for s in range(target - w, -1, -1):
            if dp[s]:
                dp[s + w] = True

    return ("yes" if dp[target] else "no") + "\n"

assert solution("6\n30 100 70 20 1 21\n") == "yes\n"
assert solution("3\n100 100 100\n") == "no\n"

assert solution("1\n1\n") == "no\n"
assert solution("4\n5 5 5 5\n") == "yes\n"
assert solution("5\n1 2 4 8 16\n") == "no\n"
assert solution("6\n1 2 4 8 16 31\n") == "yes\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1`|`no`| Kích thước tối thiểu và xử lý tổng số lẻ | 
|`5 5 5 5`|`yes`| Giá trị bằng nhau và chia trực tiếp | 
|`1 2 4 8 16`|`no`| Những trường hợp lựa chọn tham lam thất bại | 
|`1 2 4 8 16 31`|`yes`| Xây dựng mục tiêu lớn | 

## Vỏ cạnh 

Đối với trường hợp tổng lẻ:```
3
1 2 4
```thuật toán tính tổng cộng là 7. Vì 7 không thể chia thành hai số nguyên có trọng số bằng nhau nên nó trả về`no`trước khi xây dựng bảng DP. 

Đối với trường hợp phân chia một mục:```
3
1 2 3
```mục tiêu là 3. Việc xử lý trọng số tạo ra`dp[3]`đúng sau khi xem xét mục cuối cùng, do đó thuật toán trả về`yes`. Nó không yêu cầu cả hai túi phải chứa cùng số lượng đồ, chỉ có cùng trọng lượng. 

Đối với trọng lượng lặp lại:```
3
100 100 100
```mục tiêu là 150. Các trạng thái DP trở thành 100, rồi 200, rồi 300, nhưng không bao giờ đạt được 150. Thuật toán từ chối chính xác sự sắp xếp thay vì giả sử tổng số chẵn là đủ.
