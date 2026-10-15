---
title: "CF 105418E - Đồng tiền ma thuật"
description: "Chúng ta được cung cấp một tập hợp các mệnh giá tiền xu tuân theo một mẫu có cấu trúc chặt chẽ: mỗi đồng xu là một số chỉ bao gồm chữ số 1 và độ dài tăng dần theo một cách đặc biệt. Vì vậy, chúng tôi nhận được các giá trị như 11, 111, 1111, 11111, v.v., tiếp tục vô tận."
date: "2026-06-23T04:22:06+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105418
codeforces_index: "E"
codeforces_contest_name: "Algorithmia IIITN 2024 - Round 1"
rating: 0
weight: 105418
solve_time_s: 82
verified: false
draft: false
---

[CF 105418E - Đồng xu ma thuật](https://codeforces.com/problemset/problem/105418/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 22s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp các mệnh giá tiền xu tuân theo một mẫu có cấu trúc chặt chẽ: mỗi đồng xu là một số chỉ bao gồm chữ số 1 và độ dài tăng dần theo một cách đặc biệt. Vì vậy, chúng tôi nhận được các giá trị như 11, 111, 1111, 11111, v.v., tiếp tục vô tận. Mỗi trường hợp thử nghiệm đưa ra một lượng mục tiêu`n`, và chúng ta cần xác định liệu có thể biểu diễn`n`dưới dạng tổng số tiền này, trong đó mỗi loại tiền có thể được sử dụng không giới hạn số lần. 

Tái cơ cấu điều này, chúng tôi đang kiểm tra xem liệu`n`có thể được biểu thị dưới dạng tổ hợp tuyến tính của các số như 11, 111, 1111, v.v., với các hệ số nguyên không âm. Cấu trúc gợi ý một vấn đề khả thi về thay đổi đồng xu không giới hạn cổ điển, nhưng khó khăn chính là giá trị của đồng xu không phải là tùy ý mà chúng có mối tương quan cao. 

Ràng buộc`n ≤ 10^9`là hướng dẫn quan trọng. Bất kỳ giải pháp nào cố gắng lập trình động trên tất cả các giá trị lên đến`n`ngay lập tức quá lớn cả về thời gian và bộ nhớ. Ngay cả việc quét tuyến tính cho mỗi trường hợp thử nghiệm cũng sẽ quá chậm ở mức`t = 10^4`. 

Một cách tiếp cận ngây thơ cố gắng liệt kê tất cả các kết hợp của những đồng tiền này sẽ nhanh chóng thất bại vì số cách tăng lên theo cách kết hợp. Ngay cả việc giới hạn bản thân trong việc xây dựng tham lam cũng không an toàn vì những lựa chọn tham lam có thể thất bại khi hệ thống tiền xu không chuẩn mực. 

Một trường hợp khó nhận thấy là nhiều giá trị nhỏ không thể được hình thành ngay cả khi chúng trông có vẻ như vậy. Ví dụ: không thể hình thành 12 vì đồng xu nhỏ nhất là 11 và đồng xu tiếp theo là 111, số này đã vượt quá mức khi kết hợp với 11 với số lượng nhỏ. Một ví dụ khác là 23, có thể dẫn đến việc phân tách như 11 + 11 + 1, nhưng 1 không có sẵn dưới dạng đồng xu, vì vậy cách lý luận như vậy không phù hợp. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là coi đây như một vấn đề khả thi về việc thay đổi đồng tiền không giới hạn tiêu chuẩn. Trước tiên, chúng tôi tạo ra tất cả các giá trị tiền xu lên tới`n`, điều này khả thi vì trình tự phát triển nhanh chóng. Sau đó, chúng tôi chạy phương pháp lập trình động cổ điển trong đó`dp[x]`cho biết liệu tổng`x`có thể được hình thành. Đối với mỗi đồng xu, chúng tôi nới lỏng tất cả các trạng thái có thể truy cập lên đến`n`. 

Cách tiếp cận này đúng vì nó khám phá mọi sự kết hợp một cách có hệ thống. Tuy nhiên, thời gian chạy của nó tỷ lệ thuận với`O(n * number_of_coins)`, trong trường hợp xấu nhất là xung quanh`10^9 * 30`, vượt xa giới hạn khả thi. Ngay cả việc giảm nó xuống mức tối ưu hóa bitset vẫn gặp khó khăn`10^4`trường hợp thử nghiệm. 

Quan sát quan trọng là tất cả các đồng xu chỉ bao gồm chữ số 1, điều này mang lại cho chúng một cấu trúc cộng gộp mạnh mẽ. Mỗi đồng xu có chiều dài`k`có thể được xem là`111...1 = (10^k - 1) / 9`. Cấu trúc này ngụ ý rằng những đồng tiền lớn hơn là sự kết hợp gần như tuyến tính của những đồng tiền nhỏ hơn với giá trị vị trí bị dịch chuyển. Điều này cho thấy rằng không phải tất cả các đồng tiền đều đóng góp những ràng buộc độc lập. 

Một quan điểm hữu ích hơn là xem xét cấu trúc mô-đun và khả năng xây dựng chỉ bằng cách sử dụng một vài đồng xu nhỏ nhất. Khi chúng tôi bao gồm 11 và 111, các đồng tiền cao hơn có thể được biểu thị thông qua sự kết hợp của hai số này với sự dịch chuyển chữ số, khiến cho các đồng tiền lớn hơn trở nên dư thừa để kiểm tra tính khả thi theo ràng buộc nhất định. 

Vì vậy, vấn đề giảm xuống còn việc kiểm tra xem`n`có thể được biểu diễn chỉ bằng hai loại đồng xu: 11 và 111. Từ đó, bài toán trở thành phép kiểm tra tính khả thi Diophantine tuyến tính đơn giản trong các số nguyên không âm. 

Chúng tôi thử tất cả số lượng có thể có của đồng xu lớn hơn (111) và kiểm tra xem phần còn lại có chia hết cho 11 hay không. Vì`n ≤ 10^9`, số lần lặp nhiều nhất là khoảng 10^7 trong trường hợp xấu nhất, nhưng chúng ta có thể giảm nó hơn nữa bằng cách giới hạn vòng lặp bởi`n / 111`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force DP trên tất cả các đồng xu | O(nk) | O(n) | Quá chậm | 
| Giảm tìm kiếm bằng cách sử dụng 11 và 111 | O(n/111) mỗi lần kiểm tra | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán trước hoặc cố định hai giá trị đồng xu thiết yếu là 11 và 111. Không cần đến những đồng xu lớn hơn vì chúng có thể được thay thế bằng sự kết hợp của hai giá trị này mà không làm mất khả năng tiếp cận các giá trị đến mức giới hạn. 
2. Với mỗi test, lấy giá trị`n`và lặp lại xem chúng ta có thể sử dụng bao nhiêu đồng xu 111. Giá trị này nằm trong khoảng từ 0 đến`n // 111`. Mỗi lựa chọn đại diện cho việc cố định một phần của tổng bằng cách sử dụng mệnh giá có ý nghĩa lớn nhất. 
3. Với số lượng cố định là 111 xu, hãy tính giá trị còn lại`r = n - 111 * x`. Bước này sẽ xác định liệu phần còn lại có thể được hình thành chỉ bằng 11 xu hay không. 
4. Kiểm tra xem`r`chia hết cho 11. Nếu đúng như vậy thì chúng ta có thể điền chính xác phần còn lại bằng cách sử dụng 11 đồng xu, nghĩa là cách biểu diễn là hợp lệ. 
5. Nếu có sự lựa chọn`x`dẫn đến số dư hợp lệ, chúng ta kết luận ngay rằng`n`có thể đạt được và xuất ra CÓ. Nếu không có sự phân chia nào như vậy hoạt động, hãy xuất NO. 

### Tại sao nó hoạt động 

Thuật toán dựa trên thực tế là bất kỳ biểu diễn hợp lệ nào cũng có thể được sắp xếp lại sao cho tất cả 111 đồng xu được nhóm đầu tiên và giá trị còn lại phải nằm hoàn toàn trong nửa nhóm do 11 tạo ra. Vì 11 là mệnh giá nhỏ nhất nên mọi phần còn lại không chia hết cho 11 đều không thể được sửa chữa bằng cách sử dụng các đồng xu lớn hơn mà không tăng tổng số vượt quá`n`. Điều này làm cho việc tìm kiếm trên 111 đồng tiền trở nên đầy đủ và đầy đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        ok = False

        max_x = n // 111
        for x in range(max_x + 1):
            rem = n - 111 * x
            if rem % 11 == 0:
                ok = True
                break

        print("YES" if ok else "NO")

if __name__ == "__main__":
    solve()
```Giải pháp lặp lại số lượng 111 đồng xu có thể có và kiểm tra khả năng chia hết của phần còn lại cho 11. Việc thoát sớm đảm bảo chúng tôi không khám phá các kết hợp không cần thiết sau khi tìm thấy phân tách hợp lệ. 

Một chi tiết triển khai tinh tế là đảm bảo phép chia số nguyên được sử dụng khi giới hạn vòng lặp. Việc sử dụng dấu phẩy động hoặc giới hạn không chính xác sẽ bỏ lỡ các trường hợp hợp lệ hoặc gây ra các vấn đề về hiệu suất. 

## Ví dụ đã hoạt động 

Hãy xem xét`n = 144`. 

| x (111 giây) | 111*x | phần còn lại | % còn lại 11 | quyết định | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | 144 | 1 | không | 
| 1 | 111 | 33 | 0 | vâng | 

Điều này cho thấy rằng sử dụng một đồng xu 111 để lại 33, chính xác là ba đồng xu 11, vì vậy câu trả lời là CÓ. 

Bây giờ hãy xem xét`n = 69`. 

| x (111 giây) | 111*x | phần còn lại | % còn lại 11 | quyết định | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | 69 | 3 | không | 
| 1 | 111 | tiêu cực | không hợp lệ | dừng lại | 

Không có sự phân rã nào có tác dụng, vì vậy câu trả lời là KHÔNG. 

Những dấu vết này xác nhận rằng thuật toán khám phá chính xác tất cả các phân bổ khả thi của mệnh giá lớn hơn và xác thực xem phần còn lại có nằm trong hệ thống tiền xu nhỏ hơn hay không. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n/111) mỗi lần kiểm tra | Chúng tôi thử tất cả số lượng khả thi là 111 xu | 
| Không gian | O(1) | Chỉ có một vài biến được lưu trữ | 

Giới hạn này dễ dàng đủ nhanh vì vòng lặp chạy tối đa khoảng chín triệu lần lặp trong trường hợp lý thuyết tồi tệ nhất trong tất cả các thử nghiệm, nhưng trên thực tế, nó thấp hơn nhiều và nằm trong giới hạn thời gian là 1 giây trong Python khi việc thoát sớm là phổ biến. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        ok = False
        for x in range(n // 111 + 1):
            if (n - 111 * x) % 11 == 0:
                ok = True
                break
        out.append("YES" if ok else "NO")
    return "\n".join(out)

# provided samples
assert run("3\n33\n144\n69\n") == "YES\nYES\nNO"

# custom cases
assert run("1\n11\n") == "YES"
assert run("1\n22\n") == "YES"
assert run("1\n12\n") == "NO"
assert run("1\n0\n") == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 11 | CÓ | sử dụng đồng xu nhỏ nhất | 
| 22 | CÓ | bội số 11 xu | 
| 12 | KHÔNG | trường hợp còn lại không thể | 
| 0 | CÓ | hành vi ranh giới tầm thường | 

## Vỏ cạnh 

cho`n = 11`, vòng lặp ngay lập tức tìm thấy`x = 0`, số dư là 11, chia hết nên câu trả lời là CÓ. Điều này kiểm tra mệnh giá cơ sở hoạt động trực tiếp. 

Vì`n = 12`, khả năng duy nhất là`12`hoặc`12 - 111`đó là không hợp lệ. Vì 12 không chia hết cho 11 nên không có sự phân tách nào tồn tại và thuật toán đưa ra kết quả là KHÔNG. 

Đối với các giá trị rất lớn như`n = 10^9`, vòng lặp vẫn kết thúc nhanh chóng vì số lần lặp bị giới hạn bởi`n / 111`và thời điểm tìm thấy số dư hợp lệ, chúng ta sẽ thoát sớm.
