---
title: "CF 102625B - Amber Kand"
description: "Chúng ta có hai chuỗi có độ dài bằng nhau. Chuỗi đầu tiên là sự sắp xếp bắt đầu và chuỗi thứ hai là sự sắp xếp mục tiêu mà chúng ta muốn đạt được."
date: "2026-08-03T15:17:43+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102625
codeforces_index: "B"
codeforces_contest_name: "IIT(ISM) Virtual Farewell"
rating: 0
weight: 102625
solve_time_s: 151
verified: true
draft: false
---

[CF 102625B - Amber Kand](https://codeforces.com/problemset/problem/102625/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 31s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai chuỗi có độ dài bằng nhau. Chuỗi đầu tiên là sự sắp xếp bắt đầu và chuỗi thứ hai là sự sắp xếp mục tiêu mà chúng ta muốn đạt được. Chỉ được phép di chuyển khi hai ký tự lân cận thuộc các nhóm khác nhau: một nhóm chứa các chữ cái như`a`,`c`,`e`, trong đó vị trí trong bảng chữ cái là số lẻ và bảng chữ cái còn lại chứa các chữ cái như`b`,`d`,`f`, trong đó vị trí trong bảng chữ cái là chẵn. Các ký tự lân cận như vậy có thể được hoán đổi. 

Câu hỏi đặt ra là liệu một số chuỗi hoán đổi hợp lệ có thể chuyển đổi chuỗi đầu tiên thành chuỗi thứ hai hay không. 

Độ dài của mỗi chuỗi có thể đạt tới$10^5$. Điều này ngay lập tức loại trừ các phương pháp cố gắng mô phỏng các chuỗi hoán đổi có thể có hoặc khám phá các trạng thái, bởi vì ngay cả một số lượng lựa chọn tuyến tính ở mỗi vị trí cũng sẽ tạo ra một không gian tìm kiếm khổng lồ. Một giải pháp hợp lệ cần kiểm tra chuỗi một số lần nhỏ, điều đó có nghĩa là$O(n)$hoặc$O(n \log n)$cách tiếp cận phù hợp. 

Các bẫy chính xuất phát từ việc giả định rằng mọi hoán vị ký tự đều có thể thực hiện được. Ví dụ:```
Special:  ab
Elegant:  ba
```Câu trả lời là`Yes`, bởi vì`a`Và`b`đến từ các nhóm khác nhau và có thể được hoán đổi trực tiếp. 

Một trường hợp tế nhị hơn là:```
Special:  abc
Elegant:  cba
```Câu trả lời là`No`. các chữ cái`a`Và`c`thuộc cùng một nhóm nên chúng không bao giờ có thể giao nhau. Một giải pháp bất cẩn chỉ kiểm tra xem hai chuỗi có chứa các chữ cái giống nhau hay không sẽ chấp nhận trường hợp này một cách sai lầm. 

Một trường hợp đặc biệt khác là khi các chuỗi đã có cùng thứ tự nhóm nhưng vị trí của các nhóm khác nhau:```
Special:  abcde
Elegant:  baced
```Câu trả lời là`Yes`. Thứ tự tương đối của các chữ cái trong bảng chữ cái vị trí lẻ là`ace`trong cả hai chuỗi và thứ tự tương đối của các chữ cái trong bảng chữ cái ở vị trí chẵn là`bd`trong cả hai chuỗi. Các giao dịch hoán đổi được phép có thể sắp xếp lại cách hai nhóm này được xen kẽ. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ cố gắng thực hiện các giao dịch hoán đổi hợp lệ và tìm kiếm trong tất cả các chuỗi có thể được tạo. Điều này đúng vì cuối cùng mọi trạng thái có thể tiếp cận đều sẽ được khám phá, nhưng số lượng trạng thái tăng lên cực kỳ nhanh chóng. Một chuỗi có độ dài$10^5$có quá nhiều sự sắp xếp có thể thực hiện được nên cách tiếp cận này không thể sử dụng được. 

Một ý tưởng đơn giản hơn nhưng vẫn không hiệu quả là liên tục tìm kiếm một ký tự hiện đang ở sai vị trí và di chuyển nó đến đích bằng cách sử dụng các phép hoán đổi liền kề. Mỗi chuyển động có thể mất$O(n)$thời gian và làm điều này cho nhiều nhân vật có thể dẫn đến$O(n^2)$hoạt động. Với$10^5$các ký tự, điều đó sẽ ở xung quanh$10^{10}$hoạt động trong trường hợp xấu nhất. 

Quan sát chính là thao tác chỉ hoán đổi các ký tự từ các nhóm khác nhau. Các nhân vật trong cùng một nhóm không bao giờ vượt qua nhau. Nếu chúng ta loại bỏ tất cả các chữ cái ở vị trí chẵn khỏi cả hai chuỗi thì thứ tự còn lại của chúng phải giống hệt nhau. Điều tương tự cũng phải đúng sau khi loại bỏ tất cả các chữ cái ở vị trí bảng chữ cái lẻ. 

Hai chuỗi con này là thông tin duy nhất không thể thay đổi. Một khi chúng khớp nhau, hai chuỗi chỉ đơn giản là sự đan xen khác nhau của hai chuỗi giống nhau. Bất kỳ sự xen kẽ nào như vậy có thể đạt được bằng cách hoán đổi liên tục các ký tự liền kề từ các nhóm khác nhau, bởi vì thao tác này chính xác là di chuyển một phần tử nhóm qua các phần tử của nhóm khác. 

Lực lượng vũ phu hoạt động vì nó tuân theo mọi động thái hợp pháp, nhưng nó thất bại vì có quá nhiều động thái có thể xảy ra. Việc quan sát các chuỗi con được bảo toàn sẽ giảm toàn bộ vấn đề xuống việc kiểm tra hai lần quét tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Chuyển động liền kề lặp đi lặp lại | O(n²) | O(1) | Quá chậm | 
| Dãy con được bảo toàn | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tách cả hai chuỗi thành hai chuỗi con. Dãy con đầu tiên chứa các chữ cái có vị trí bảng chữ cái lẻ và dãy thứ hai chứa các chữ cái có vị trí bảng chữ cái chẵn. Làm điều này cho cả chuỗi bắt đầu và chuỗi đích. 
2. So sánh dãy con chữ cái vị trí lẻ ở chuỗi đầu tiên với dãy con chữ cái vị trí lẻ ở chuỗi thứ hai. 

Những ký tự này không bao giờ có thể thay đổi thứ tự tương đối của chúng vì các hoán đổi hợp lệ chỉ di chuyển chúng xung quanh các ký tự từ nhóm khác. 
3. So sánh dãy con chữ cái ở vị trí chẵn theo cách tương tự. 

Lý do tương tự cũng được áp dụng: hai chữ cái ở vị trí chẵn không bao giờ có thể hoán đổi cho nhau. 
4. Nếu cả hai cặp dãy con giống nhau thì in ra`Yes`. Ngược lại, in`No`. 

Các chuỗi con trùng khớp có nghĩa là hai chuỗi chứa cùng một thứ tự cố định trong cả hai nhóm. Sự khác biệt còn lại chỉ là cách hai nhóm này được trộn lẫn với nhau và những thay đổi đó chính xác là những gì các giao dịch hoán đổi được phép có thể tạo ra. 

Tại sao nó hoạt động: 

Mỗi hoán đổi hợp lệ sẽ trao đổi một chữ cái trong bảng chữ cái vị trí lẻ với một chữ cái trong bảng chữ cái vị trí chẵn. Vì điều này, không nhóm nào có thể thay đổi trật tự nội bộ của mình. Bất kỳ mục tiêu nào có thể tiếp cận đều phải bảo toàn cả hai chuỗi con. 

Hướng ngược lại cũng được giữ. Giả sử cả hai dãy con đều khớp nhau. Hãy coi các chữ cái chẵn và lẻ là hai hàng đợi phải được hợp nhất vào thứ tự mục tiêu. Bắt đầu từ chuỗi gốc, một hoán đổi liền kề có thể di chuyển chữ cái được yêu cầu tiếp theo từ một trong hai hàng đợi qua các chữ cái của hàng đợi khác cho đến khi nó đạt đến vị trí mong muốn. Vì tất cả các chữ cái gạch chéo đều thuộc nhóm đối diện nên mọi hoán đổi như vậy đều hợp lệ. Việc lặp lại quá trình này sẽ tạo nên chuỗi đích, vì vậy các chuỗi con phù hợp cũng là đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    s = input().strip()
    t = input().strip()

    s_odd = []
    s_even = []
    t_odd = []
    t_even = []

    for c in s:
        if (ord(c) - ord('a') + 1) % 2 == 1:
            s_odd.append(c)
        else:
            s_even.append(c)

    for c in t:
        if (ord(c) - ord('a') + 1) % 2 == 1:
            t_odd.append(c)
        else:
            t_even.append(c)

    if s_odd == t_odd and s_even == t_even:
        print("Yes")
    else:
        print("No")

if __name__ == "__main__":
    solve()
```Mã xây dựng bốn danh sách. Hai ký tự đầu tiên lưu trữ các ký tự từ chuỗi bắt đầu được phân tách bằng nhóm của chúng và hai ký tự còn lại thực hiện tương tự đối với chuỗi đích. 

Việc tính toán vị trí bảng chữ cái sử dụng`ord(c) - ord('a') + 1`bởi vì`ord`bắt đầu đếm từ 0 cho`a`, trong khi việc nhóm vấn đề bắt đầu từ một. Một phép tính chẵn lẻ sai ở đây sẽ hoán đổi hai nhóm và khiến mọi câu trả lời đều sai. 

So sánh cuối cùng là đủ vì không có thuộc tính nào khác của chuỗi có thể ảnh hưởng đến khả năng tiếp cận. Thứ tự tương đối bên trong mỗi nhóm là thông tin duy nhất tồn tại trong tất cả các hoạt động hợp lệ. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
cheel
naara
```Dấu vết là: 

| Nhóm nhân vật | Chuỗi bắt đầu | Chuỗi mục tiêu | Kết quả | 
| --- | --- | --- | --- | 
| Vị trí bảng chữ cái lẻ | cee | aea | Khác nhau | 
| Ngay cả vị trí bảng chữ cái | hl | nr | Khác nhau | 

Nhóm lẻ vốn đã khác nhau nên những ký tự này sẽ cần phải giao thoa với nhau. Vì chúng thuộc cùng một nhóm nên không có chuỗi hoán đổi nào được phép có thể khắc phục được. Câu trả lời là`No`. 

Đối với mẫu thứ hai:```
potha
opath
```Dấu vết là: 

| Nhóm nhân vật | Chuỗi bắt đầu | Chuỗi mục tiêu | Kết quả | 
| --- | --- | --- | --- | 
| Vị trí bảng chữ cái lẻ | ôi | ôi | Tương tự | 
| Ngay cả vị trí bảng chữ cái | pt | pt | Tương tự | 

Cả hai nhóm đều giữ trật tự nội bộ của mình. Các chuỗi chỉ khác nhau ở cách hai nhóm được xen kẽ, có thể thay đổi bằng cách sử dụng các hoán đổi được phép. Câu trả lời là`Yes`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi chuỗi được quét một lần và các chuỗi con được so sánh | 
| Không gian | O(n) | Các chuỗi con được tách ra đều chứa tất cả các ký tự | 

Chiều dài tối đa là$10^5$, do đó quét tuyến tính dễ dàng phù hợp trong giới hạn thời gian. Việc sử dụng bộ nhớ cũng nằm trong giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve_case(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)

    s = sys.stdin.readline().strip()
    t = sys.stdin.readline().strip()

    s_odd = []
    s_even = []
    t_odd = []
    t_even = []

    for c in s:
        if (ord(c) - ord('a') + 1) % 2:
            s_odd.append(c)
        else:
            s_even.append(c)

    for c in t:
        if (ord(c) - ord('a') + 1) % 2:
            t_odd.append(c)
        else:
            t_even.append(c)

    ans = "Yes" if s_odd == t_odd and s_even == t_even else "No"

    sys.stdin = old_stdin
    return ans + "\n"

assert solve_case("cheel\nnaara\n") == "No\n", "sample 1"
assert solve_case("potha\nopath\n") == "Yes\n", "sample 2"

assert solve_case("a\na\n") == "Yes\n", "single character"
assert solve_case("ab\nba\n") == "Yes\n", "direct valid swap"
assert solve_case("abc\ncba\n") == "No\n", "same-group order cannot change"
assert solve_case("aaaaa\naaaaa\n") == "Yes\n", "all equal characters"
assert solve_case("aceg\ngeca\n") == "No\n", "large same-group reversal"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`a`ĐẾN`a`| Có | Kích thước tối thiểu và không có giao dịch hoán đổi | 
|`ab`ĐẾN`ba`| Có | Trao đổi trực tiếp giữa các nhóm khác nhau | 
|`abc`ĐẾN`cba`| Không | Bảo quản trật tự cùng nhóm | 
|`aaaaa`ĐẾN`aaaaa`| Có | Tất cả các ký tự giống hệt nhau | 
|`aceg`ĐẾN`geca`| Không | Trường hợp ranh giới trong đó tất cả các chữ cái nằm trong một nhóm | 

## Vỏ cạnh 

Đối với trường hợp:```
Special:
abc

Elegant:
cba
```Thuật toán tách cả hai chuỗi thành các nhóm lẻ và chẵn. Dãy số lẻ của`abc`là`ac`, trong khi dãy con lẻ của`cba`là`ca`. Vì chúng khác nhau nên thuật toán trả về`No`. Thất bại đến từ việc cố gắng đảo ngược hai chữ cái không bao giờ có thể hoán đổi được. 

Đối với trường hợp:```
Special:
ab

Elegant:
ba
```Các dãy con lẻ đều là`a`, và các dãy con chẵn đều là`b`. Thuật toán trả về`Yes`. Các nhân vật có thể đổi chỗ cho nhau vì họ đến từ các nhóm đối lập nhau. 

Đối với trường hợp:```
Special:
aaaa

Elegant:
aaaa
```Cả hai chuỗi đều giống hệt nhau vì mọi ký tự đều thuộc cùng một nhóm. Thuật toán trả về`Yes`ngay lập tức. Điều này xử lý các ký tự lặp đi lặp lại mà không vô tình yêu cầu các vị trí duy nhất. 

Đối với trường hợp:```
Special:
potha

Elegant:
opath
```Nhóm lẻ là`oah`trong cả hai chuỗi và nhóm chẵn là`pt`trong cả hai chuỗi. Thuật toán chấp nhận chuyển đổi vì những thay đổi duy nhất cần thiết là chuyển động của nhóm này sang nhóm khác.
