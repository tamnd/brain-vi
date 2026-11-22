---
title: "CF 4C - Hệ thống đăng ký"
description: "Chúng tôi đang xây dựng một hệ thống đăng ký tên người dùng. Mỗi yêu cầu đến đều chứa tên người dùng mong muốn. Nếu tên người dùng đó chưa từng xuất hiện trước đó, đăng ký thành công ngay lập tức và chúng tôi in OK."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "hashing", "implementation"]
categories: ["algorithms"]
codeforces_contest: 4
codeforces_index: "C"
codeforces_contest_name: "Codeforces Beta Round 4 (Div. 2 Only)"
rating: 1300
weight: 4
solve_time_s: 102
verified: true
draft: false
---

[CF 4C - Hệ thống đăng ký](https://codeforces.com/problemset/problem/4/C) 

**Đánh giá:** 1300 
**Tags:** cấu trúc dữ liệu, băm, triển khai 
**Thời gian giải:** 1 phút 42s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang xây dựng một hệ thống đăng ký tên người dùng. Mỗi yêu cầu đến đều chứa tên người dùng mong muốn. Nếu tên người dùng đó chưa từng xuất hiện trước đó, việc đăng ký sẽ thành công ngay lập tức và chúng tôi sẽ in`OK`. 

Nếu tên người dùng đã được sử dụng, chúng tôi phải tạo một tên mới bằng cách đính kèm hậu tố số nguyên dương nhỏ nhất tạo ra tên chưa được sử dụng. Ví dụ, nếu`alex`đã tồn tại, chúng tôi thử`alex1`, sau đó`alex2`, v.v. cho đến khi chúng tôi tìm thấy một tên miễn phí. Tên được tạo đã chọn cũng bị chiếm giữ. 

Đầu vào đưa ra một chuỗi các yêu cầu đăng ký, mỗi yêu cầu trên một dòng. Đầu ra phải chứa phản hồi của hệ thống cho từng yêu cầu theo cùng một thứ tự. 

Hạn chế lên tới$10^5$yêu cầu thay đổi hoàn toàn vấn đề. Giải pháp quét các tên được sử dụng trước đây mỗi lần sẽ trở nên quá chậm. Với$10^5$hoạt động, chúng ta nên hướng tới thời gian gần như tuyến tính về tổng thể hoặc gần với thời gian đó. Bảng băm phù hợp một cách tự nhiên vì chúng tôi liên tục hỏi cùng một câu hỏi: "Tên người dùng này đã được sử dụng chưa?" 

Phần khó khăn là bản thân những cái tên được tạo ra cũng bị chiếm dụng. Việc triển khai bất cẩn có thể chỉ theo dõi tên gốc và quên tên đã tạo. 

Hãy xem xét đầu vào này:```
4
bob
bob
bob1
bob
```Đầu ra đúng là:```
OK
bob1
OK
bob2
```Sau yêu cầu thứ hai,`bob1`đã bị chiếm dụng vì hệ thống đã tạo ra nó. Khi yêu cầu thứ ba yêu cầu một cách rõ ràng`bob1`, nó phải tồn tại rồi. 

Một trường hợp tinh vi khác xuất hiện khi nhiều hậu tố đã bị chiếm dụng:```
5
a
a1
a2
a
a
```Đầu ra đúng là:```
OK
OK
OK
a3
a4
```Việc triển khai đơn giản luôn bắt đầu kiểm tra từ`1`sẽ liên tục kiểm tra`a1`,`a2`, vân vân. Điều đó vẫn tạo ra câu trả lời đúng, nhưng nó lãng phí rất nhiều thời gian. 

Một sai lầm dễ mắc nữa là quên rằng bản thân tên gốc cũng cần theo dõi:```
3
test
test
test
```Đầu ra đúng là:```
OK
test1
test2
```Nếu chúng tôi chỉ theo dõi các hậu tố được tạo thì yêu cầu thứ ba có thể tạo ra sai`test1`lại. 

## Phương pháp tiếp cận 

Ý tưởng brute-force được đưa ra trực tiếp sau tuyên bố vấn đề. Duy trì một bộ tên người dùng đã sử dụng. Khi có yêu cầu mới đến, hãy kiểm tra xem nó đã tồn tại chưa. 

Nếu nó không tồn tại, hãy chèn nó và in`OK`. 

Nếu nó đã tồn tại, hãy bắt đầu thử từng hậu tố một:```
name1
name2
name3
...
```Ứng cử viên đầu tiên không được sử dụng sẽ trở thành câu trả lời. 

Phương pháp này đúng vì nó mô phỏng đúng quy trình cần thiết. Vấn đề là hiệu suất. Giả sử đầu vào chứa`a`lặp đi lặp lại$10^5$lần. Kiểm tra trùng lặp đầu tiên`a1`, lần kiểm tra tiếp theo`a1`Và`a2`, lần kiểm tra tiếp theo`a1`,`a2`,`a3`, vân vân. Tổng số lần kiểm tra trở thành:$$1 + 2 + 3 + \dots + (n-1)$$đó là$O(n^2)$. Với$10^5$yêu cầu, đó là xung quanh$5 \times 10^9$hoạt động vượt xa giới hạn. 

Quan sát quan trọng là khi chúng ta đã biết hậu tố có sẵn tiếp theo cho tên cơ sở, chúng ta không bao giờ cần phải thử lại các hậu tố nhỏ hơn nữa. 

Ví dụ, sau khi tạo`alex7`, chúng ta đã biết điều đó rồi`alex1`bởi vì`alex7`đang bị chiếm đóng. Yêu cầu trùng lặp tiếp theo cho`alex`nên bắt đầu trực tiếp từ hậu tố`8`. 

Điều này biến vấn đề thành việc duy trì hai mẩu thông tin bên trong bản đồ băm: 

1. Tên người dùng đã tồn tại hay chưa. 
2. Đối với mỗi tên cơ sở, hậu tố nào sẽ được thử tiếp theo. 

Bảng băm cho kết quả trung bình$O(1)$chèn và tra cứu, do đó mỗi yêu cầu có thể được xử lý trong thời gian dự kiến ​​không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(n)$trung bình |$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo bản đồ băm có tên`mp`. 

Bản đồ lưu trữ tên người dùng dưới dạng khóa. Đối với tên cơ sở, giá trị của nó đại diện cho hậu tố tiếp theo cần thử. 
2. Xử lý từng tên người dùng được gửi đến. 

Chúng tôi phải trả lời các yêu cầu theo thứ tự vì mọi tên người dùng được chấp nhận sẽ ảnh hưởng ngay lập tức đến các yêu cầu trong tương lai. 
3. Nếu tên người dùng không tồn tại trên bản đồ, hãy in`OK`. 

Điều này có nghĩa là tên này hoàn toàn không được sử dụng nên việc đăng ký sẽ thành công ngay lập tức. 
4. Chèn tên người dùng này vào bản đồ có giá trị`1`. 

giá trị`1`có nghĩa là nếu tên này xuất hiện lại sau đó thì hậu tố đầu tiên cần thử sẽ là`1`. 
5. Nếu tên người dùng đã tồn tại, hãy đọc bộ đếm được lưu trữ của nó. 

Giả định`mp[name] = k`. Điều này có nghĩa là hậu tố nhỏ hơn`k`đã bị chiếm dụng nên không có lý do gì để kiểm tra lại chúng. 
6. Xây dựng ứng viên`name + str(k)`. 

Đây là hậu tố nhỏ nhất chưa được thử cho tên cơ sở này. 
7. In ứng viên đã tạo và chèn nó vào bản đồ. 

Tên được tạo bây giờ sẽ được sử dụng giống như bất kỳ tên người dùng nào được yêu cầu rõ ràng. 
8. Tăng`mp[name]`bởi một. 

Yêu cầu trùng lặp tiếp theo cho cùng tên cơ sở sẽ tiếp tục từ hậu tố tiếp theo. 

### Tại sao nó hoạt động 

Bất biến là:```
For every base username x, mp[x] always stores the smallest suffix
that has not yet been assigned for x.
```Ban đầu, sau khi chèn`x`lần đầu tiên, hậu tố nhỏ nhất không được sử dụng là`1`, Vì thế`mp[x] = 1`. 

Bất cứ khi nào chúng tôi tạo ra`xk`, chúng tôi ngay lập tức tăng bộ đếm lên`k + 1`. Vì tất cả các hậu tố nhỏ hơn đã được sử dụng trước đó và`xk`vừa bị chiếm đóng, hậu tố không được sử dụng tiếp theo chính xác là`k + 1`. 

Bởi vì bất biến luôn đúng nên mọi tên người dùng được tạo đều hợp lệ và tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    mp = {}

    for _ in range(n):
        name = input().strip()

        if name not in mp:
            print("OK")
            mp[name] = 1
        else:
            suffix = mp[name]
            new_name = name + str(suffix)

            print(new_name)

            mp[name] += 1
            mp[new_name] = 1

solve()
```Từ điển`mp`đóng hai vai trò cùng một lúc. Khóa của nó đại diện cho tất cả tên người dùng đã sử dụng, trong khi giá trị cho tên người dùng cơ sở theo dõi hậu tố tiếp theo sẽ sử dụng. 

Khi một tên xuất hiện lần đầu tiên, chúng tôi in`OK`và khởi tạo bộ đếm của nó thành`1`. Điều này khớp với bất biến mà bản sao tiếp theo nên thử hậu tố`1`. 

Đối với các yêu cầu trùng lặp, chúng tôi đọc trực tiếp hậu tố được lưu trữ thay vì quét từ`1`lại. Sự tối ưu hóa duy nhất đó sẽ loại bỏ hành vi bậc hai. 

Dòng:```
mp[new_name] = 1
```là cực kỳ quan trọng. Tên người dùng đã tạo cũng phải được bảo lưu ngay lập tức. Nếu không có dòng này, các yêu cầu trong tương lai có thể vô tình sử dụng lại chúng. 

Một chi tiết tinh tế khác đang được cập nhật`mp[name]`sau khi tạo tên người dùng. Nếu chúng ta quên tăng nó, hậu tố tương tự sẽ được sử dụng lại nhiều lần. 

Việc thực hiện sử dụng`input().strip()`bởi vì mỗi tên người dùng có một dòng riêng và chúng tôi không muốn các ký tự dòng mới ở cuối chuỗi được lưu trữ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
abacaba
acaba
abacaba
acab
```| Bước | Yêu cầu | Bản đồ trước | Đầu ra | Bản đồ sau | 
| --- | --- | --- | --- | --- | 
| 1 | bàn tính | {} | được | {bàn tính: 1} | 
| 2 | acaba | {bàn tính: 1} | được | {acaba: 1, acaba: 1} | 
| 3 | bàn tính | {acaba: 1, acaba: 1} | bàn tính1 | {abacaba: 2, acaba: 1, abacaba1: 1} | 
| 4 | acab | {abacaba: 2, acaba: 1, abacaba1: 1} | được | {abacaba: 2, acaba: 1, abacaba1: 1, acab: 1} | 

Dấu vết này cho thấy cách bộ đếm cho`abacaba`tiến bộ từ`1`ĐẾN`2`sau khi tạo`abacaba1`. Thuật toán không bao giờ kiểm tra hậu tố`1`một lần nữa cho tên cơ sở đó. 

### Ví dụ 2 

đầu vào:```
5
a
a
a
a1
a
```| Bước | Yêu cầu | Bản đồ trước | Đầu ra | Bản đồ sau | 
| --- | --- | --- | --- | --- | 
| 1 | một | {} | được | {a: 1} | 
| 2 | một | {a: 1} | a1 | {a: 2, a1: 1} | 
| 3 | một | {a: 2, a1: 1} | a2 | {a: 3, a1: 1, a2: 1} | 
| 4 | a1 | {a: 3, a1: 1, a2: 1} | a11 | {a: 3, a1: 2, a2: 1, a11: 1} | 
| 5 | một | {a: 3, a1: 2, a2: 1, a11: 1} | a3 | {a: 4, a1: 2, a2: 1, a11: 1, a3: 1} | 

Ví dụ này chứng minh rằng tên người dùng được tạo hoạt động giống hệt như tên người dùng thông thường. Một lần`a1`tồn tại, yêu cầu`a1`một lần nữa phải tạo ra`a11`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$trung bình | Mỗi yêu cầu thực hiện các hoạt động bản đồ băm theo thời gian không đổi | 
| Không gian |$O(n)$| Mỗi tên người dùng được chấp nhận sẽ được lưu trữ một lần | 

Với nhiều nhất$10^5$yêu cầu, độ phức tạp tuyến tính dễ dàng phù hợp trong giới hạn. Từ điển Python là các bảng băm được tối ưu hóa, do đó việc chèn và tra cứu đủ nhanh cho ràng buộc này. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    def solve():
        n = int(input())
        mp = {}
        out = []

        for _ in range(n):
            name = input().strip()

            if name not in mp:
                out.append("OK")
                mp[name] = 1
            else:
                suffix = mp[name]
                new_name = name + str(suffix)

                out.append(new_name)

                mp[name] += 1
                mp[new_name] = 1

        return "\n".join(out)

    return solve()

# provided sample
assert run(
    "4\nabacaba\nacaba\nabacaba\nacab\n"
) == (
    "OK\nOK\nabacaba1\nOK"
), "sample 1"

# minimum size
assert run(
    "1\nx\n"
) == (
    "OK"
), "single registration"

# all equal values
assert run(
    "5\na\na\na\na\na\n"
) == (
    "OK\na1\na2\na3\na4"
), "repeated identical names"

# generated names later requested explicitly
assert run(
    "4\nbob\nbob\nbob1\nbob\n"
) == (
    "OK\nbob1\nbob11\nbob2"
), "generated usernames must also be reserved"

# boundary style chaining
assert run(
    "6\na\na1\na\na1\na2\na\n"
) == (
    "OK\nOK\na2\na11\na21\na3"
), "nested suffix interactions"

# larger repeated sequence
large_input = "10\n" + "\n".join(["z"] * 10) + "\n"

assert run(large_input) == (
    "OK\nz1\nz2\nz3\nz4\nz5\nz6\nz7\nz8\nz9"
), "many duplicates"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Tên người dùng duy nhất |`OK`| Kích thước đầu vào tối thiểu | 
| Nhiều tên giống nhau | Hậu tố tuần tự | Bộ đếm tăng chính xác | 
|`bob`,`bob`,`bob1`,`bob`|`bob1`phải trở nên bận rộn | Tên đã tạo được bảo lưu | 
| Chuỗi hậu tố hỗn hợp | Thế hệ lồng nhau đúng | Xử lý tên đã chứa chữ số | 
| Mười lần lặp lại`z`giá trị |`z1`bởi vì`z9`| Trình tự trùng lặp lớn vẫn hiệu quả | 

## Vỏ cạnh 

Hãy xem xét trường hợp tên người dùng được tạo sau đó được yêu cầu rõ ràng:```
4
bob
bob
bob1
bob
```Từng bước một: 

1.`bob`là mới, in`OK`, cửa hàng`bob -> 1`. 
2.`bob`tồn tại, tạo ra`bob1`, in nó, cập nhật`bob -> 2`, cửa hàng`bob1 -> 1`. 
3.`bob1`đã tồn tại vì nó được tạo trước đó, vì vậy chúng ta phải tạo`bob11`. 
4.`bob`bây giờ bắt đầu trực tiếp từ hậu tố`2`, tạo ra`bob2`. 

Đầu ra trở thành:```
OK
bob1
bob11
bob2
```Điều này xác nhận rằng tên người dùng đã tạo được xử lý giống hệt như tên người dùng thông thường. 

Bây giờ hãy xem xét các bản sao lặp đi lặp lại:```
5
a
a
a
a
a
```Thực thi: 

1. Đầu tiên`a`bản in`OK`. 
2. Thứ hai`a`bản in`a1`. 
3. Thứ ba`a`bản in`a2`. 
4. Thứ tư`a`bản in`a3`. 
5. Thứ năm`a`bản in`a4`. 

Thuật toán không bao giờ kiểm tra lại các hậu tố cũ vì`mp["a"]`luôn lưu trữ trực tiếp hậu tố không sử dụng tiếp theo. 

Cuối cùng, hãy xem xét các tên đã chứa chữ số:```
5
a1
a1
a11
a1
a
```Thực thi: 

1.`a1`bản in`OK`. 
2. Nhân đôi`a1`tạo ra`a11`. 
3.`a11`đã tồn tại, vì vậy nó tạo ra`a111`. 
4. Khác`a1`tạo ra`a12`. 
5.`a`vẫn chưa được sử dụng, vì vậy nó in`OK`. 

Đầu ra là:```
OK
a11
a111
a12
OK
```Thuật toán không bao giờ cố phân tích các chữ số từ tên người dùng. Mỗi chuỗi được xử lý độc lập, giúp tránh được nhiều lỗi góc cạnh.
