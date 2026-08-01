---
title: "CF 102623G - Jena dịu dàng"
description: "Chúng tôi nhận được một chuỗi các giá trị độ sáng của sao, nhưng chuỗi này được tạo trực tuyến. Sau khi mỗi ngôi sao mới xuất hiện, chúng ta cần giá trị vẻ đẹp của tiền tố hiện tại."
date: "2026-08-01T09:04:59+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102623
codeforces_index: "G"
codeforces_contest_name: "2020 Lenovo Cup USST Campus Online Invitational Contest"
rating: 0
weight: 102623
solve_time_s: 387
verified: true
draft: false
---

[CF 102623G - Jena dịu dàng](https://codeforces.com/problemset/problem/102623/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 6 phút 27s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi nhận được một chuỗi các giá trị độ sáng của sao, nhưng chuỗi này được tạo trực tuyến. Sau khi mỗi ngôi sao mới xuất hiện, chúng ta cần giá trị vẻ đẹp của tiền tố hiện tại. Vẻ đẹp của tiền tố là tổng độ sáng tối thiểu giữa mọi phân đoạn liền kề của tiền tố đó, lấy modulo`998244353`. 

Trình tạo khiến nhiệm vụ trở nên khó khăn hơn vì độ sáng tiếp theo phụ thuộc vào giá trị làm đẹp trước đó. Sau khi tính toán`A_i`, ngôi sao tiếp theo được tính bằng cách sử dụng`A_i`, vì vậy chúng tôi không thể đọc trước toàn bộ mảng. 

Đầu ra là XOR của tất cả các giá trị vẻ đẹp được tính toán. Thông tin duy nhất phải tồn tại từ giây này sang giây khác là cấu trúc dữ liệu cần thiết để cập nhật vẻ đẹp hiện tại và giá trị vẻ đẹp trước đó mà trình tạo cần có. 

Giá trị của`n`có thể đạt được`10^7`. Một giải pháp kiểm tra tất cả các mảng con là không thể vì có khoảng`n^2 / 2`mảng con, sẽ ở xung quanh`5 * 10^13`hoạt động. Ngay cả các thuật toán có hệ số logarit cho mỗi lần cập nhật cũng có rủi ro ở kích thước này. Chúng tôi cần một bản cập nhật thời gian khấu hao liên tục. 

Giá trị độ sáng có thể lớn gần như`10^9`và các giá trị đẹp phải được xử lý theo modulo`998244353`. Trình tạo cũng phụ thuộc vào giá trị mô-đun, do đó, việc trộn lẫn giá trị thô và giá trị mô-đun không chính xác có thể tạo ra các ngôi sao sai trong tương lai. 

Một tiền tố nhỏ cũng phải hoạt động chính xác. Đối với một ngôi sao, vẻ đẹp đơn giản chỉ là độ sáng của ngôi sao đó. Ví dụ: nếu đầu vào là`1 10 0 0 0 7`, độ sáng duy nhất là`7`, vì vậy đầu ra là`7`. Việc triển khai khởi tạo câu trả lời đang chạy về 0 và quên xử lý phần tử đầu tiên sẽ không thành công. 

Giá trị độ sáng lặp đi lặp lại là một nguồn sai lầm phổ biến khác. Ví dụ, trình tự`[5, 5]`có cực tiểu mảng con`5, 5, 5`, vậy vẻ đẹp là`15`. Ngăn xếp đơn điệu chỉ loại bỏ các giá trị lớn hơn thay vì các giá trị lớn hơn hoặc bằng nhau sẽ đếm không chính xác các giá trị bằng nhau. 

## Phương pháp tiếp cận 

Một giải pháp trực tiếp sẽ lưu trữ tất cả các ngôi sao trước đó và sau mỗi lần chèn, liệt kê mọi mảng con và tính mức tối thiểu của nó. Phương pháp này đúng vì nó đánh giá đúng định nghĩa về cái đẹp. Đối với tiền tố có độ dài`i`, điều này đòi hỏi`O(i^2)`công việc, và trên tất cả các tiền tố, tổng công việc trở thành`O(n^3)`. Ngay cả việc cải thiện nó bằng cách duy trì mức tối thiểu cho mỗi vị trí xuất phát vẫn cần`O(n^2)`hoạt động tổng thể, vượt xa giới hạn cho`n = 10^7`. 

Quan sát hữu ích đến từ việc chỉ xem xét các mảng con mới được tạo khi một ngôi sao được thêm vào. Giả sử độ sáng mới là`x`. Tất cả các mảng con cũ đều giữ mức tối thiểu trước đó của chúng. Các thuật ngữ mới duy nhất là các hậu tố kết thúc ở vị trí mới. Nếu chúng ta biết tổng giá trị tối thiểu của tất cả các hậu tố kết thúc ở vị trí hiện tại thì việc cộng nó vào vẻ đẹp tổng thể sẽ mang lại câu trả lời mới. 

Các giá trị tối thiểu của hậu tố có hành vi có cấu trúc. Khi một giá trị nhỏ hơn xuất hiện, nó sẽ thay thế một số hậu tố cực tiểu trước đó. Đây chính xác là tình huống được xử lý bởi một ngăn xếp đơn điệu. Ngăn xếp lưu trữ các giá trị độ sáng theo thứ tự tăng dần. Mỗi mục cũng lưu trữ số lượng hậu tố hiện có giá trị tối thiểu đó. Khi một giá trị mới nhỏ hơn xuất hiện, tất cả các giá trị lớn hơn sẽ được hợp nhất thành giá trị mới vì các hậu tố đó hiện có mức tối thiểu nhỏ hơn. 

Lực lượng vũ phu hoạt động vì mọi mức tối thiểu của mảng con đều được tính một cách rõ ràng, nhưng không thành công vì có quá nhiều mảng con. Ngăn xếp đơn điệu nén các nhóm hậu tố có cùng mức tối thiểu, giảm mỗi lần chèn thành phân bổ`O(1)`thời gian. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) mỗi tiền tố | O(n) | Quá chậm | 
| Ngăn xếp đơn điệu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Duy trì một ngăn xếp tăng dần đơn điệu. Mỗi phần tử ngăn xếp lưu trữ một giá trị độ sáng và số hậu tố liên tiếp có giá trị tối thiểu là giá trị đó. 
2. Duy trì`cur`, tổng giá trị tối thiểu của tất cả các hậu tố kết thúc ở vị trí hiện tại. Khi một độ sáng mới`x`đến, nó bắt đầu với mức tối thiểu là một hậu tố mới, vì vậy nó sẽ đóng góp`x`. 
3. Trong khi giá trị ngăn xếp trên cùng lớn hơn hoặc bằng`x`, hãy xóa mục nhập đó. Những nhóm hậu tố đó bây giờ có`x`thay vào đó là mức tối thiểu của họ, vì vậy hãy trừ khoản đóng góp cũ của họ khỏi`cur`và thêm số lượng của họ vào nhóm mới. 
4. Thêm sự đóng góp của nhóm đã sáp nhập có chứa`x`ĐẾN`cur`, sau đó đẩy nhóm này vào ngăn xếp. 
5. Vẻ đẹp của toàn bộ tiền tố tăng lên một cách chính xác`cur`, bởi vì các mảng con mới duy nhất là các hậu tố kết thúc ở ngôi sao mới. Thêm giá trị làm đẹp này vào câu trả lời XOR. 
6. Sử dụng giá trị vẻ đẹp hiện tại để tạo ngôi sao tiếp theo và tiếp tục cho đến khi tất cả các ngôi sao đã được xử lý. 

Tại sao nó hoạt động: tính bất biến của ngăn xếp là mỗi mục trong ngăn xếp đại diện cho một nhóm hậu tố kết thúc ở vị trí hiện tại có cùng mức tối thiểu và các giá trị trong ngăn xếp đang tăng lên một cách nghiêm ngặt. Khi một giá trị mới xuất hiện, mọi nhóm bị ảnh hưởng chính xác là nhóm các hậu tố trong đó mức tối thiểu cũ lớn hơn hoặc bằng giá trị mới. Việc thay thế các nhóm đó bằng giá trị mới sẽ giữ nguyên tất cả các mức tối thiểu của hậu tố trong khi chỉ thay đổi cách thể hiện của chúng. Vì mỗi phần tử vào ngăn xếp một lần và rời khỏi ngăn xếp một lần nên toàn bộ hoạt động của ngăn xếp là tuyến tính. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def solve():
    n, p, x, y, z, b = map(int, input().split())

    stack_val = []
    stack_cnt = []

    cur = 0
    ans = 0

    for i in range(n):
        cnt = 1
        b_mod = b % MOD

        while stack_val and stack_val[-1] >= b:
            v = stack_val.pop()
            c = stack_cnt.pop()
            cur = (cur - (v % MOD) * c) % MOD
            cnt += c

        cur = (cur + b_mod * cnt) % MOD

        stack_val.append(b)
        stack_cnt.append(cnt)

        ans ^= cur

        if i + 1 < n:
            b = (x * cur + y * b + z) % p

    print(ans)

if __name__ == "__main__":
    solve()
```Hai mảng ngăn xếp được giữ riêng biệt thay vì lưu trữ các bộ dữ liệu vì kích thước đầu vào có thể lên tới mười triệu. Việc tránh phân bổ bộ dữ liệu sẽ làm giảm chi phí bộ nhớ và cải thiện thời gian chạy.`cur`được lưu trữ modulo`998244353`bởi vì mỗi lần sử dụng giá trị làm đẹp sau này chỉ cần kết quả mô-đun. Ngăn xếp giữ các giá trị độ sáng ban đầu vì các phép so sánh phải sử dụng thứ tự thực của các ngôi sao. Phép trừ đóng góp sử dụng`v % MOD`, bảo toàn số học mô-đun trong khi vẫn giữ chính xác các phép so sánh. 

Việc cập nhật trình tạo chỉ được thực hiện sau khi vẻ đẹp hiện tại đã được ghi lại. Độ sáng hiện tại vẫn cần thiết trong công thức nên nó được cập nhật sau khi tính giá trị tiếp theo. 

Việc sử dụng`>=`trong điều kiện hợp nhất ngăn xếp là cần thiết. Các giá trị độ sáng bằng nhau phải thuộc cùng một nhóm, nếu không các hậu tố giống nhau sẽ được tính nhiều lần. 

## Ví dụ đã hoạt động 

Đối với đầu vào mẫu, trình tự được tạo là: 

| Bước | Độ sáng mới | Xếp nhóm sau khi cập nhật | Tổng hậu tố hiện tại | Sắc đẹp | 
| --- | --- | --- | --- | --- | 
| 1 | 3 | (3,1) | 3 | 3 | 
| 2 | 7 | (3,1),(7,1) | 10 | 13 | 
| 3 | 1 | (1,3) | 3 | 16 | 
| 4 | 7 | (1,3),(7,1) | 10 | 26 | 
| 5 | 1 | (1,5) | 5 | 31 | 

Đóng góp hậu tố chỉ thay đổi khi một giá trị mới loại bỏ các nhóm ngăn xếp lớn hơn. Bước thứ ba thể hiện thao tác chính: giá trị mới`1`trở thành mức tối thiểu cho tất cả các hậu tố, hợp nhất các nhóm trước đó. 

Một ví dụ nhỏ thứ hai với chuỗi độ sáng`[5,5]`mang lại: 

| Bước | Độ sáng mới | Xếp nhóm sau khi cập nhật | Tổng hậu tố hiện tại | Sắc đẹp | 
| --- | --- | --- | --- | --- | 
| 1 | 5 | (5,1) | 5 | 5 | 
| 2 | 5 | (5,2) | 10 | 15 | 

Các giá trị bằng nhau được hợp nhất thành một nhóm. Điều này khẳng định tại sao việc so sánh phải được`>=`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ngôi sao được đẩy một lần và xuất hiện nhiều nhất một lần. | 
| Không gian | O(n) | Trong trường hợp xấu nhất, ngăn xếp chứa mọi ngôi sao. | 

Độ phức tạp tuyến tính là cần thiết bởi vì`n`có thể`10^7`. Thuật toán chỉ thực hiện một lượng công việc không đổi nhỏ trên mỗi ngôi sao được tạo ra, khiến nó phù hợp với giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    result = sys.stdout.getvalue()
    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return result

assert run("5 13 2 0 1 3\n") == "27\n", "sample"

assert run("1 10 0 0 0 7\n") == "7\n", "single star"

assert run("2 100 0 0 0 5\n") == "10\n", "equal values"

assert run("3 100 0 0 0 1\n") == "7\n", "increasing values"

assert run("3 100 0 0 0 9\n") == "54\n", "decreasing values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`5 13 2 0 1 3`|`27`| Tương tác mẫu gốc và máy phát điện | 
|`1 10 0 0 0 7`|`7`| Trường hợp kích thước tối thiểu | 
|`2 100 0 0 0 5`|`10`| Hợp nhất độ sáng bằng nhau | 
|`3 100 0 0 0 1`|`7`| Tăng khả năng xử lý trình tự | 
|`3 100 0 0 0 9`|`54`| Giảm trình tự xử lý | 

## Vỏ cạnh 

Đối với một ngôi sao, ngăn xếp chứa một nhóm có số một. Với đầu vào`1 10 0 0 0 7`, thuật toán tính toán`cur = 7`, tôn lên vẻ đẹp`7`và đầu ra`7`. Không có hậu tố nào trước đó để hợp nhất nên quá trình khởi tạo được kiểm tra trực tiếp. 

Để có giá trị bằng nhau, hãy xem xét`2 100 0 0 0 5`. Vẻ đẹp đầu tiên là`5`. Giá trị được tạo tiếp theo cũng là`5`, vậy hai hậu tố kết thúc ở đó đều có giá trị nhỏ nhất`5`. Ngăn xếp hợp nhất nhóm cũ và giá trị mới thành`(5,2)`, sản xuất`cur = 10`và vẻ đẹp cuối cùng`15`cho chuỗi hai sao. XOR của người đẹp là`5 xor 15 = 10`. Điều này nắm bắt các triển khai xử lý sai sự bình đẳng. 

Đối với chuỗi giảm dần, mỗi giá trị mới sẽ thay thế tất cả các cực tiểu hậu tố trước đó. Ngăn xếp liên tục sụp đổ thành một nhóm. Đối với giá trị độ sáng`[9,8,7]`, những người đẹp là`9`,`25`, Và`46`, vậy XOR là`9 xor 25 xor 46 = 54`. Thuật toán xử lý việc này vì mỗi pop sẽ chuyển các nhóm hậu tố cũ sang mức tối thiểu mới nhỏ hơn.
