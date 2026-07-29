---
title: "CF 102800C - Trò chơi trên dây"
description: "Trò chơi xoay quanh hai dây. Clair có chuỗi dài hơn và Bob chọn một chuỗi khác làm từ mục tiêu."
date: "2026-07-28T22:51:31+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102800
codeforces_index: "C"
codeforces_contest_name: "The 14th Jilin Provincial Collegiate Programming Contest"
rating: 0
weight: 102800
solve_time_s: 55
verified: true
draft: false
---

[CF 102800C - Trò chơi dây](https://codeforces.com/problemset/problem/102800/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 55s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Trò chơi xoay quanh hai dây. Clair có chuỗi dài hơn và Bob chọn một chuỗi khác làm từ mục tiêu. Chuỗi được Bob chọn có thể có lỗi, vì vậy nhiệm vụ là kiểm tra xem có bao nhiêu cách khác nhau để tạo thành chuỗi đó bằng cách xóa một số ký tự khỏi chuỗi của Clair trong khi vẫn giữ nguyên thứ tự các ký tự còn lại. 

Một cách hình thành chuỗi mục tiêu là so khớp chuỗi con. Ví dụ, từ`eeettt`, chuỗi`et`có thể được hình thành bằng cách chọn bất kỳ một trong ba`e`ký tự và bất kỳ một trong ba ký tự`t`các ký tự sau nó, đưa ra chín kết quả phù hợp. Đầu ra được yêu cầu là tổng số lựa chọn theo modulo (10^9+7). 

Chuỗi đầu tiên có thể có độ dài lên tới 5000, trong khi chuỗi mục tiêu có thể có độ dài lên tới 1000. Giải pháp bậc hai thường có khoảng 25 triệu thao tác cho đầu vào lớn nhất, điều này rất thoải mái trong Python. Một giải pháp thử mọi tập hợp con vị trí có thể có sẽ yêu cầu kiểm tra tới (2^{5000}) lựa chọn, điều này là không thể. Các ràng buộc gợi ý rằng các vị trí của chuỗi đích phải được xử lý bằng lập trình động thay vì liệt kê rõ ràng. 

Một số chi tiết có thể phá vỡ việc triển khai ngây thơ. Nếu chuỗi đích dài hơn chuỗi gốc thì không thể tồn tại chuỗi con nào. Đối với đầu vào:```
abc
abcd
```câu trả lời đúng là`0`. Một phương pháp chỉ đếm các ký tự trùng khớp mà không xem xét thứ tự và độ dài có thể tạo ra giá trị dương không chính xác. 

Các ký tự lặp đi lặp lại tạo ra một lỗi phổ biến khác. Vì:```
ee
e
```câu trả lời là`2`, bởi vì hoặc`e`trong chuỗi đầu tiên có thể được chọn. Chỉ đếm các chuỗi kết quả riêng biệt sẽ trả về không chính xác`1`. 

Thứ tự của các ký tự cũng quan trọng. Vì:```
abc
ba
```câu trả lời là`0`, bởi vì mặc dù cả hai ký tự đều xuất hiện,`b`đến sau`a`trong chuỗi nguồn và không thể được chọn trước nó. Phương pháp chỉ dựa trên tần số ký tự sẽ thất bại ở đây. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là chọn vị trí trong chuỗi của Clair cho mọi ký tự trong chuỗi của Bob. Đối với mỗi lựa chọn vị trí có thể có, chúng tôi xác minh xem các ký tự được chọn có khớp với chuỗi đích theo thứ tự hay không. Điều này đúng vì mọi dãy con hợp lệ đều tương ứng với chính xác một tập hợp các vị trí đã chọn. 

Vấn đề là số lượng lựa chọn. Nếu chuỗi của Clair có độ dài 5000 thì số dãy con có thể có là (2^{5000}). Ngay cả việc tạo ra một phần nhỏ trong số chúng cũng vượt quá giới hạn thời gian, vì vậy ý ​​tưởng vũ phu không thể được sử dụng. 

Quan sát hữu ích là mọi kết quả khớp một phần đều có cấu trúc giống nhau. Khi quét chuỗi của Clair từ trái sang phải, chúng ta chỉ cần biết mỗi tiền tố trong chuỗi của Bob đã được hình thành theo bao nhiêu cách. Tương lai chỉ phụ thuộc vào những con số này chứ không phụ thuộc vào các vị trí chính xác được sử dụng trước đây. 

Giả sử chúng ta đã xử lý một số tiền tố của chuỗi Clair. Cho phép`dp[j]`biểu thị số cách để tạo ra cái đầu tiên`j`các ký tự trong chuỗi của Bob từ tiền tố được xử lý. Khi một ký tự mới từ Clair được đọc, nó có thể mở rộng mọi kết quả khớp đã tồn tại mà ký tự bắt buộc tiếp theo giống nhau. Nó cũng có thể bắt đầu so khớp ký tự đầu tiên trong chuỗi của Bob. 

Điều này biến việc tìm kiếm theo cấp số nhân thành một quá trình lập trình động. Mỗi ký tự từ Clair cập nhật tất cả độ dài tiền tố có thể có của chuỗi Bob một lần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Tối ưu | O(nm) | O(m) | Đã chấp nhận | 

Đây,`n`là độ dài của dây Clair và`m`là độ dài sợi dây của Bob. 

## Hướng dẫn thuật toán 

1. Tạo mảng lập trình động trong đó`dp[j]`lưu trữ số cách để hình thành đầu tiên`j`các ký tự trong chuỗi của Bob sử dụng phần chuỗi của Clair đã được xử lý cho đến nay. Ban đầu,`dp[0] = 1`bởi vì có đúng một cách để tạo thành một chuỗi rỗng: không chọn gì cả. 
2. Quét từng ký tự trong chuỗi của Clair. Đối với ký tự hiện tại, hãy kiểm tra vị trí chuỗi của Bob từ đầu đến cuối. 
3. Nếu nhân vật hiện tại của Clair khớp với`j`-ký tự thứ của Bob, thêm`dp[j-1]`vào trong`dp[j]`. Điều này thể hiện việc lấy ký tự hiện tại làm ký tự cuối cùng của một chuỗi con mới được hình thành. Hướng ngược lại là cần thiết vì ký tự hiện tại chỉ có thể được sử dụng một lần trong bản cập nhật hiện tại. 
4. Sau khi tất cả các ký tự từ Clair đã được xử lý,`dp[m]`chứa số cách hoàn chỉnh để tạo thành toàn bộ chuỗi của Bob. Xuất giá trị này theo modulo (10^9+7). 

Tại sao nó hoạt động: tính bất biến là sau khi xử lý bất kỳ tiền tố nào trong chuỗi của Clair,`dp[j]`chính xác là số cách để tạo thành hình đầu tiên`j`các ký tự trong chuỗi của Bob từ tiền tố đó. Khi một ký tự mới được xem xét, mọi chuỗi con hiện có sẽ bỏ qua nó hoặc sử dụng nó làm ký tự khớp tiếp theo. Bản cập nhật ngược đếm trường hợp thứ hai trong khi vẫn giữ nguyên trường hợp đầu tiên trong các giá trị hiện có. Vì mỗi dãy con hợp lệ đều có một ký tự được chọn cuối cùng duy nhất nên mọi khả năng đều được tính chính xác một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10 ** 9 + 7

def solve():
    s = input().strip()
    t = input().strip()

    m = len(t)
    dp = [0] * (m + 1)
    dp[0] = 1

    for ch in s:
        for j in range(m, 0, -1):
            if ch == t[j - 1]:
                dp[j] += dp[j - 1]
                if dp[j] >= MOD:
                    dp[j] -= MOD

    print(dp[m])

if __name__ == "__main__":
    solve()
```Mảng`dp`đại diện cho tất cả độ dài tiền tố của chuỗi Bob. Kích thước của nó chỉ bằng độ dài của chuỗi đích, vì phần đã xử lý của chuỗi Clair không cần phải lưu trữ. 

Vòng lặp bên ngoài xử lý chuỗi của Clair theo thứ tự giống như các chuỗi con được tạo. Vòng lặp bên trong đi ngược qua chuỗi của Bob sao cho mỗi lần xuất hiện của một ký tự trong Clair chỉ đóng góp một lần trong bước này. Nếu vòng lặp tiếp tục, một giá trị cập nhật có thể được sử dụng lại ngay lập tức và cùng một ký tự có thể điền sai nhiều vị trí trong chuỗi của Bob. 

Phép toán modulo được áp dụng sau mỗi lần cộng vì số dãy con tăng lên rất nhanh. Số nguyên Python không bị tràn, nhưng việc giữ giá trị ở mức giảm sẽ tránh sự tăng trưởng không cần thiết và phù hợp với đầu ra được yêu cầu. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
eeettt
et
```mục tiêu có chiều dài bằng hai, vì vậy`dp[1]`đếm cách để thực hiện`e`Và`dp[2]`đếm cách để thực hiện`et`. 

| Nhân vật hiện tại | dp sau khi xử lý | 
| --- | --- | 
| bắt đầu |`[1, 0, 0]`| 
| e |`[1, 1, 0]`| 
| e |`[1, 2, 0]`| 
| e |`[1, 3, 0]`| 
| t |`[1, 3, 3]`| 
| t |`[1, 3, 6]`| 
| t |`[1, 3, 9]`| 

Giá trị cuối cùng là`9`. Dấu vết cho thấy rằng mọi`e`có thể ghép đôi sau này`t`. 

Đối với mẫu thứ hai:```
eeettt
te
```thứ tự của mục tiêu bị đảo ngược. 

| Nhân vật hiện tại | dp sau khi xử lý | 
| --- | --- | 
| bắt đầu |`[1, 0, 0]`| 
| e |`[1, 0, 0]`| 
| e |`[1, 0, 0]`| 
| e |`[1, 0, 0]`| 
| t |`[1, 1, 0]`| 
| t |`[1, 2, 0]`| 
| t |`[1, 3, 0]`| 

Câu trả lời vẫn là 0 vì không`e`xuất hiện sau một lựa chọn`t`. Trạng thái lập trình động thực thi thứ tự một cách tự nhiên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nm) | Mỗi ký tự trong chuỗi của Clair cập nhật mọi độ dài tiền tố có thể có của chuỗi của Bob một lần. | 
| Không gian | O(m) | Chỉ số lượng tiền tố của Bob được lưu trữ. | 

Với`n = 5000`Và`m = 1000`, thuật toán thực hiện khoảng năm triệu cập nhật trạng thái, vừa vặn trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io

MOD = 10 ** 9 + 7

def solve():
    s = input().strip()
    t = input().strip()

    dp = [0] * (len(t) + 1)
    dp[0] = 1

    for ch in s:
        for j in range(len(t), 0, -1):
            if ch == t[j - 1]:
                dp[j] = (dp[j] + dp[j - 1]) % MOD

    print(dp[-1])

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

assert run("eeettt\net\n") == "9\n", "sample 1"
assert run("eeettt\nte\n") == "0\n", "sample 2"

assert run("a\na\n") == "1\n", "single matching character"
assert run("abc\nabcd\n") == "0\n", "target longer than source"
assert run("aaaa\naa\n") == "6\n", "all equal characters"
assert run("abab\nab\n") == "4\n", "multiple ordered matches"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`a`Và`a`|`1`| Dãy con hợp lệ tối thiểu | 
|`abc`Và`abcd`|`0`| Mục tiêu dài hơn nguồn | 
|`aaaa`Và`aa`|`6`| Kết hợp với các ký tự lặp lại | 
|`abab`Và`ab`|`4`| Nhiều lựa chọn có thứ tự hợp lệ | 

## Vỏ cạnh 

Đối với trường hợp mục tiêu dài hơn nguồn:```
abc
abcd
```thuật toán tạo ra một mảng lập trình động có độ dài bằng 5. Trong quá trình quét, chỉ có ba vị trí mục tiêu đầu tiên mới có thể nhận được đóng góp, vì ký tự mục tiêu thứ tư không bao giờ có ký tự nguồn trùng khớp. Giá trị cuối cùng vẫn còn`0`, điều đó đúng. 

Đối với các ký tự lặp lại:```
ee
e
```trạng thái ban đầu là`[1, 0]`. đầu tiên`e`thay đổi nó thành`[1, 1]`, và thứ hai`e`thay đổi nó thành`[1, 2]`. Câu trả lời cuối cùng là`2`, đếm cả hai lựa chọn có thể về vị trí. 

Để đặt hàng:```
abc
ba
```nhân vật đầu tiên`a`không giúp ích gì vì ký tự mục tiêu được yêu cầu đầu tiên là`b`. Sau khi xử lý`b`, có một cách để hình thành`b`, nhưng khi`a`được xử lý, nó không thể mở rộng kết quả khớp đó vì mục tiêu yêu cầu`a`sau đó`b`. Câu trả lời cuối cùng là`0`. 

Thuật toán xử lý những trường hợp này vì trạng thái lưu trữ tiền tố của chuỗi đích chứ không chỉ số ký tự. Một ký tự chỉ đóng góp khi nó xuất hiện ở đúng vị trí so với tiền tố đã khớp. 

Tôi cũng có thể điều chỉnh bài xã luận này thành một phiên bản ngắn hơn theo phong cách Codeforces nếu bạn muốn có hình thức gửi bài dự thi nhiều hơn.
