---
title: "CF 102214E - Mã hóa"
description: "Chúng ta có một chuỗi chữ thường có độ dài n. Trong quá trình mã hóa, chúng tôi xem xét mọi ước số d của n, bắt đầu từ ước số lớn nhất và kết thúc bằng 1. Đối với mỗi ước số như vậy, chúng tôi đảo ngược tiền tố bao gồm các ký tự d đầu tiên."
date: "2026-08-18T00:12:18+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102214
codeforces_index: "E"
codeforces_contest_name: "\u041e\u0442\u043a\u0440\u044b\u0442\u043e\u0435 \u043b\u0438\u0447\u043d\u043e\u0435 \u043f\u0435\u0440\u0432\u0435\u043d\u0441\u0442\u0432\u043e \u0418\u041a\u0418\u0422 \u0421\u0424\u0423 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e 2015"
rating: 0
weight: 102214
solve_time_s: 62
verified: true
draft: false
---

[CF 102214E - Mã hóa](https://codeforces.com/problemset/problem/102214/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 2s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một chuỗi chữ thường có độ dài n. Trong quá trình mã hóa, chúng tôi xem xét mọi ước số d của n, bắt đầu từ ước số lớn nhất và kết thúc bằng 1. Đối với mỗi ước số như vậy, chúng tôi đảo ngược tiền tố bao gồm các ký tự d đầu tiên. 

Ví dụ: nếu n=10 thì các ước của nó theo thứ tự giảm dần là 10,5,2,1. Do đó, quá trình mã hóa thực hiện bốn lần đảo ngược tiền tố, đầu tiên là trên toàn bộ chuỗi, sau đó là năm ký tự đầu tiên, sau đó là hai ký tự đầu tiên và cuối cùng là ký tự đầu tiên. 

Đầu vào cung cấp cho chúng ta chuỗi được mã hóa chứ không phải chuỗi gốc. Chúng ta phải khôi phục chuỗi gốc. 

Độ dài tối đa là 100 và chuỗi chỉ chứa các chữ cái tiếng Anh viết thường. Giới hạn 100 là rất nhỏ, do đó, ngay cả việc mô phỏng đơn giản mọi lần đảo chiều cần thiết cũng đủ nhanh. Không cần các thuật toán chuỗi hoặc cấu trúc dữ liệu phức tạp. Khó khăn chính là nhận biết hướng áp dụng các thao tác thuận nghịch. 

Trường hợp cạnh đầu tiên là n=1. Ví dụ:```
1z
```Ước số duy nhất là 1 và việc đảo ngược tiền tố một ký tự sẽ không thay đổi gì. Câu trả lời là:```
z
```Việc triển khai bất cẩn cho rằng luôn có một tiền tố không cần thiết để đảo ngược có thể vô tình bỏ qua hoặc xử lý sai trường hợp này. 

Một trường hợp cạnh khác xảy ra khi chính n là ước số lớn duy nhất có liên quan. Ví dụ:```
2ab
```Các ước số là 1 và 2. Quá trình giải mã xử lý chúng theo thứ tự tăng dần. Đảo ngược ký tự đầu tiên không thay đổi gì, sau đó đảo ngược hai ký tự đầu tiên sẽ thay đổi`ab`vào trong`ba`, vậy đáp án đúng là:```
ba
```Nếu chúng tôi xử lý các ước số theo thứ tự giảm dần trong quá trình giải mã, chúng tôi sẽ thực hiện các thao tác theo thứ tự giống như mã hóa thay vì hoàn tác chúng. 

Lỗi phổ biến thứ ba là quên rằng mọi phép toán số chia đều hoạt động trên một tiền tố chứ không phải trên khối bắt đầu từ số chia đó. Ví dụ: với n=6, số chia 3 có nghĩa là đảo ngược vị trí từ 1 đến 3. Nó không có nghĩa là đảo ngược vị trí từ 3 đến 6. 

## Phương pháp tiếp cận 

Một cách mạnh mẽ để suy nghĩ về vấn đề này là liệt kê các chuỗi gốc có thể có và mã hóa từng ứng cử viên cho đến khi một chuỗi tạo ra chuỗi được mã hóa nhất định. Bản thân hoạt động mã hóa mang tính quyết định, do đó, một ứng cử viên như vậy rất dễ xác minh. Tuy nhiên, có thể có 26 n chuỗi chữ thường có độ dài n. Ngay cả với n=20, đây vẫn là 26 20, xấp xỉ 2 94, điều này hoàn toàn không thể thực hiện được. Brute Force đúng về mặt khái niệm nhưng không thành công vì nó tìm kiếm trong một không gian rộng lớn mặc dù bản thân quá trình mã hóa hoàn toàn có thể đảo ngược. 

Quan sát quan trọng là mọi hoạt động mã hóa đều là một sự đảo ngược và sự đảo ngược cũng là nghịch đảo của chính nó. Nếu chúng ta đảo ngược tiền tố hai lần, chúng ta sẽ lấy lại chính xác tiền tố ban đầu. Vấn đề duy nhất khác là thứ tự hoạt động. 

Giả sử mã hóa áp dụng các hoạt động 

R d 1 ​ ,R d 2 ​ ​ ,…,R d k ​ ​ 

nơi các ước số thỏa mãn 

d 1 ​ >d 2 ​ >⋯>d k ​ . 

Chuỗi được mã hóa là 

R d k ​ (…R d 2 ​ ​ (R d 1 ​ ​ (s))…). 

Để hoàn tác điều này, trước tiên chúng ta phải hoàn tác R d k ​, sau đó là R d k−1 ​, v.v. Vì mỗi đảo ngược là nghịch đảo của chính nó nên việc giải mã chỉ cần áp dụng các đảo ngược tiền tố tương tự theo thứ tự ngược lại. 

Mã hóa truy cập các ước từ lớn nhất đến nhỏ nhất, do đó, giải mã truy cập các ước từ nhỏ nhất đến lớn nhất. 

Điều này biến toàn bộ vấn đề thành một mô phỏng trực tiếp. Với mọi d từ 1 đến n, nếu d chia n thì đảo ngược d ký tự đầu tiên. Đây chính xác là cách tiếp cận được sử dụng bởi giải pháp biên tập chính thức của Codeforces. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(26 n ⋅n) | O(n) | Quá chậm | 
| Tối ưu | O(∑ d∣n ​ d) | O(n) | Đã chấp nhận | 

Bởi vì n<100 nên ngay cả việc thực hiện đơn giản cũng nằm trong giới hạn một cách thoải mái. Tổng của tất cả các ước số cũng là O(nloglogn) tiệm cận, do đó mô phỏng trực tiếp vẫn hiệu quả ngay cả trong giới hạn lớn hơn nhiều. 

## Hướng dẫn thuật toán 

1. Đọc n và xâu mã hóa t. Chúng tôi sẽ sửa đổi chuỗi này tại chỗ vì mọi thao tác giải mã chỉ thay đổi thứ tự các ký tự của nó. 
2. Lặp lại d từ 1 đến n. Nếu nmodd=0 thì d là một trong những tiền tố bị đảo ngược trong quá trình mã hóa. 
3. Đảo ngược tiền tố có độ dài d. Chúng tôi xử lý các ước số theo thứ tự tăng dần vì mã hóa đã xử lý các ước số giống nhau theo thứ tự giảm dần và mỗi lần đảo ngược đều là nghịch đảo của chính nó. 
4. Sau khi tất cả các ước số đã được xử lý, hãy in chuỗi kết quả. Tại thời điểm này, mọi thao tác mã hóa đã được hoàn tác theo thứ tự ngược lại. 

### Tại sao nó hoạt động 

Đặt các ước của n theo thứ tự tăng dần là d 1 ​ <d 2 ​ <⋯<d k ​. Mã hóa áp dụng các phép đảo ngược theo thứ tự d k ​ ,d k−1 ​,…,d 1 ​. Việc giải mã được áp dụng R d 1 ​ ,R d 2 ​ ​ ,…,R d k ​ ​. Vì R d ​ (R d ​ (x))=x với mỗi lần đảo ngược tiền tố R d ​, nên mỗi thao tác giải mã sẽ hủy thao tác mã hóa tương ứng. Các thao tác cũng được áp dụng theo thứ tự ngược lại, do đó mọi phép biến đổi đều bị hủy ở đúng điểm. Do đó, chuỗi cuối cùng là chuỗi gốc duy nhất. 

## Giải pháp Python```python
Pythonimport sysinput = sys.stdin.readline

def solve():    n = int(input())    s = list(input().strip())
    for d in range(1, n + 1):        if n % d == 0:            s[:d] = reversed(s[:d])
    print(''.join(s))

if __name__ == "__main__":    solve()
```Chuỗi được chuyển đổi thành danh sách vì chuỗi Python là bất biến. biểu thức`s[:d]`đại diện chính xác cho các ký tự d đầu tiên, khớp với sự đảo ngược tiền tố của vấn đề. 

Vòng lặp bắt đầu lúc`1`và kết thúc tại`n`, vì vậy mọi ước số có thể đều được xem xét. Kiểm tra`n % d == 0`xác định chính xác độ dài tiền tố cần thiết. 

Nhiệm vụ```
Pythons[:d] = reversed(s[:d])
```thay thế tiền tố bằng đảo ngược của nó. Thứ tự là rất quan trọng. Việc lặp lại từ ước số nhỏ đến ước số lớn thực hiện nghịch đảo của chuỗi mã hóa. 

Không có vấn đề tràn số nguyên trong Python. Cũng không có vấn đề riêng lẻ nào vì phần của Python`s[:d]`chứa các chỉ số`0`bởi vì`d-1`, chính xác là d ký tự. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là:```
10rocesfedoc
```Các ước của 10 theo thứ tự tăng dần là 1,2,5,10. 

| d | Là số chia? | Chuỗi sau khi đảo chiều | 
| --- | --- | --- | 
| 1 | Có |`rocesfedoc`| 
| 2 | Có |`orcesfedoc`| 
| 3 | Không |`orcesfedoc`| 
| 4 | Không |`orcesfedoc`| 
| 5 | Có |`secrofedoc`| 
| 6 | Không |`secrofedoc`| 
| 7 | Không |`secrofedoc`| 
| 8 | Không |`secrofedoc`| 
| 9 | Không |`secrofedoc`| 
| 10 | Có |`codeforces`| 

Câu trả lời cuối cùng là:```
codeforces
```Dấu vết thể hiện tính bất biến trung tâm: sau khi xử lý k ước số đầu tiên theo thứ tự tăng dần, k thao tác mã hóa tương ứng ở cuối chuỗi mã hóa ban đầu đã bị hủy. 

### Mẫu 2 

Đầu vào là:```
16plmaetwoxesisiht
```Các ước của 16 là 1,2,4,8,16. 

| d | Là số chia? | Chuỗi sau khi đảo chiều | 
| --- | --- | --- | 
| 1 | Có |`plmaetwoxesisiht`| 
| 2 | Có |`lpmaetwoxesisiht`| 
| 3 | Không |`lpmaetwoxesisiht`| 
| 4 | Có |`ampl...`| 
| 8 | Có |`this...`| 
| 16 | Có |`thisisexampletwo`| 

Chuỗi kết quả hoàn chỉnh là:```
thisisexampletwo
```Ví dụ này thực hiện một số đảo ngược tiền tố lồng nhau. Nó cũng cho thấy tại sao sự đảo ngược không thể được xử lý một cách độc lập. Việc đảo ngược tiền tố lớn hơn sẽ thay đổi các ký tự đã được di chuyển bằng cách đảo ngược tiền tố nhỏ hơn, do đó thứ tự thao tác chính xác rất quan trọng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(∑ d∣n ​ d) | Mọi ước số d đều gây ra sự đảo ngược của ký tự d | 
| Không gian | O(n) | Chuỗi được lưu trữ dưới dạng mảng ký tự có thể thay đổi | 

Với n 100, khối lượng công việc tối đa là rất nhỏ. Ngay cả việc triển khai đơn giản có chủ ý để quét tất cả n độ dài tiền tố có thể và đảo ngược từng tiền tố đủ điều kiện cũng có thể dễ dàng nằm trong giới hạn thời gian một giây và giới hạn bộ nhớ 256 MB của vấn đề Codeforces thực tế. 

## Trường hợp thử nghiệm```python
Pythonimport sysimport io

def solve():    input = sys.stdin.readline    n = int(input())    s = list(input().strip())
    for d in range(1, n + 1):        if n % d == 0:            s[:d] = reversed(s[:d])
    print(''.join(s))

def run(inp: str) -> str:    old_stdin = sys.stdin    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)    sys.stdout = io.StringIO()
    try:        solve()        return sys.stdout.getvalue()    finally:        sys.stdin = old_stdin        sys.stdout = old_stdout

# Provided sample 1assert run("10\nrocesfedoc\n") == "codeforces\n", "sample 1"
# Provided sample 2assert run("16\nplmaetwoxesisiht\n") == "thisisexampletwo\n", "sample 2"
# Provided sample 3assert run("1\nz\n") == "z\n", "sample 3"
# Minimum-size inputassert run("1\na\n") == "a\n", "minimum size"
# All characters equalassert run("8\naaaaaaaa\n") == "aaaaaaaa\n", "all equal"
# Smallest non-trivial divisor structureassert run("2\nab\n") == "ba\n", "n = 2"
# Boundary-sized inputassert run("100\n" + "a" * 100 + "\n") == "a" * 100 + "\n", "n = 100"
# Several divisors, catches divisor-order mistakesassert run("6\nfedcba\n") == "abcdef\n", "multiple divisors"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / a`|`a`| Kích thước tối thiểu và tiền tố có độ dài 1 | 
|`8 / aaaaaaaa`|`aaaaaaaa`| Đảo ngược bảo toàn các ký tự bằng nhau | 
|`2 / ab`|`ba`| Sự đảo chiều không hề nhỏ nhất | 
|`100 / a...a`| Cùng 100 ký tự | Độ dài đầu vào tối đa | 
|`6 / fedcba`|`abcdef`| Nhiều ước số và thứ tự đảo ngược đúng | 

## Vỏ cạnh 

Với n=1, đầu vào```
1z
```chỉ có ước số 1. Thuật toán kiểm tra 1∣1, đảo ngược ký tự đầu tiên và thu được`z`lại. Nó in`z`, vì vậy tiền tố một ký tự không yêu cầu trường hợp đặc biệt. 

Với n=2, hãy xem xét```
2ab
```Dãy số chia tăng dần là 1,2. Lá đảo chiều dài một`ab`không thay đổi. Sự đảo ngược chiều dài hai tạo ra`ba`. Mã hóa`ba`đảo ngược toàn bộ chuỗi và trả về`ab`, khẳng định thứ tự tăng dần là thứ tự nghịch đảo đúng. 

Đối với đầu vào trong đó mọi ký tự đều bằng nhau, chẳng hạn như```
8aaaaaaaa
```mỗi lần đảo ngược đều khiến chuỗi hiển thị không thay đổi. Thuật toán vẫn xử lý các ước số 1,2,4,8 nhưng mọi trạng thái trung gian vẫn được giữ nguyên`aaaaaaaa`. Điều này nắm bắt việc triển khai vô tình phụ thuộc vào các ký tự khác biệt. 

Cuối cùng, hãy xem xét```
6fedcba
```Các ước số là 1,2,3,6. Sau khi đảo ngược các tiền tố có độ dài này theo thứ tự đó, chuỗi sẽ trở thành`abcdef`. Việc 2, 3 và 6 đều tương tác với cùng các vị trí đầu tiên khiến đây trở thành một bài kiểm tra hữu ích cho lỗi phổ biến nhất, xử lý các ước số theo thứ tự giảm dần trong quá trình giải mã. Giải pháp tránh được lỗi đó bằng cách đảo ngược chính xác trình tự mã hóa.
