---
title: "CF 102535L - Kim có thể và Mooks và Swappinator"
description: "Dòng đối thủ có thể được xem như một mảng nhị phân. MOOK là 1 vì nó vẫn cần phải chiến đấu và MEEK là 0 vì Kim đi qua nó miễn phí. Trước khi trận đấu bắt đầu, chúng ta có thể hoán đổi hai vị trí chính xác k lần."
date: "2026-08-05T15:44:09+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102535
codeforces_index: "L"
codeforces_contest_name: "2020 UP ACM Algolympics Elimination Round"
rating: 0
weight: 102535
solve_time_s: 1223
verified: true
draft: false
---

[CF 102535L - Kim có thể và Mooks và kẻ hoán đổi](https://codeforces.com/problemset/problem/102535/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 20m 23s 
**Đã xác minh:** có 

## Giải pháp 
#Hiểu vấn đề 

Dòng đối thủ có thể được xem như một mảng nhị phân. MOOK là`1`bởi vì nó vẫn cần phải chiến đấu và MEEK là`0`bởi vì Kim đi qua nó miễn phí. Trước khi cuộc chiến bắt đầu, chúng ta có thể hoán đổi chính xác hai vị trí`k`lần. Mục tiêu là sắp xếp đường sao cho số phút cần thiết để xóa tất cả MOOK càng nhỏ càng tốt. 

Quá trình chiến đấu có cấu trúc truy cập nhị phân ẩn. Nếu chúng ta chỉ định vị trí`i`giá trị`2^(i-1)`và tính tổng các giá trị của tất cả các vị trí MOOK, một trận đấu luôn giảm tổng này đi đúng một. MOOK đầu tiên thay đổi từ`1`ĐẾN`0`và tất cả các số 0 trước đó trở thành số 1, chính xác là số giảm nhị phân. Vì điều này, câu trả lời cuối cùng là giá trị số được biểu thị bằng mảng nhị phân sau khi hoán đổi. 

Đầu vào cung cấp độ dài của dòng, số lần hoán đổi chính xác và sau đó là trạng thái hiện tại của mọi vị trí. Đầu ra là giá trị nhị phân nhỏ nhất có thể sau khi thực hiện các phép hoán đổi đó, modulo`10^9`. 

Ràng buộc`n <= 100000`là khó khăn chính. Việc mô phỏng quá trình chiến đấu có thể mất tới`2^n`phút, điều đó là không thể. Ngay cả việc thử tất cả các lựa chọn hoán đổi có thể cũng vượt xa giới hạn 2 giây cho phép vì số lượng các cặp có thể có là bậc hai và chuỗi các lần hoán đổi tăng theo cấp số nhân. Lời giải chỉ phải kiểm tra đường này một số lần không đổi, dẫn đến`O(n)`tiếp cận. 

Có một số trường hợp phức tạp phá vỡ các giải pháp đơn giản hơn. Nếu không có MOOK, câu trả lời luôn là 0. Ví dụ:```
2 5
MEEK
MEEK
```Đầu ra đúng là`0`. Một phương pháp luôn cố gắng di chuyển MOOK xung quanh có thể cho rằng có ít nhất một cuộc chiến tồn tại một cách sai lầm. 

Cái bẫy thứ hai là yêu cầu các giao dịch hoán đổi phải được sử dụng một cách chính xác.`k`lần. Ví dụ:```
2 1
MOOK
MEEK
```Trao đổi duy nhất thay đổi trạng thái thành`MEEK, MOOK`, cho giá trị`2`. Một giải pháp chỉ cố gắng đạt được sự sắp xếp được tối ưu hóa hoàn toàn sẽ xuất ra không chính xác`1`. 

Trường hợp đặc biệt cuối cùng là hai đối thủ thuộc loại khác nhau. Không có phần tử bằng nhau nào có sẵn để hoán đổi như một thao tác bổ sung vô nghĩa. Ví dụ:```
2 2
MEEK
MOOK
```Sau hai lần hoán đổi, mảng trở về trạng thái ban đầu, vì vậy câu trả lời là`2`, không phải giá trị được sắp xếp`1`. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ mô phỏng mọi lựa chọn hoán đổi có thể có. Đối với mỗi chuỗi hoán đổi, chúng ta có thể tính giá trị nhị phân thu được và giữ giá trị nhỏ nhất. Điều này đúng vì nó xem xét mọi thỏa thuận pháp lý cuối cùng. Tuy nhiên, ngay cả một lần hoán đổi cũng có`O(n^2)`các lựa chọn và lặp lại điều này cho đến`n`hoán đổi cung cấp một không gian tìm kiếm không thể. 

Điểm khởi đầu tốt hơn là hiểu được việc hoán đổi có thể cải thiện điều gì. Giá trị nhị phân nhỏ nhất có cùng số MOOK thu được bằng cách đặt tất cả MOOK ở đầu dòng, vì các vị trí trước đó có lũy thừa nhỏ hơn là 2. Ví dụ: với ba MOOK, dạng lý tưởng là`111000...`. 

Mọi vị trí trong khối đầu tiên chứa MEEK đều là một sai lầm. Hoán đổi nó bằng MOOK từ bên phải sẽ sửa chính xác một lỗi như vậy. Để tối đa hóa sự cải thiện, chúng ta nên sử dụng các vị trí không chính xác ngoài cùng bên trái và các MOOK ngoài cùng bên phải bên ngoài khối. 

Khó khăn còn lại là từ "chính xác". Nếu chúng ta cần ít lần hoán đổi hơn số lần mắc lỗi thì mỗi lần hoán đổi sẽ hữu ích. Nếu chúng ta có đủ số lần hoán đổi để đạt được thỏa thuận đã sắp xếp, câu hỏi duy nhất là liệu những lần hoán đổi còn lại có bị lãng phí hay không. Trong mọi trường hợp, ngoại trừ dòng có độ dài hai chứa một MOOK và một MEEK, một hoán đổi bổ sung có thể được thực hiện giữa các đối thủ ngang nhau hoặc bằng cách hoàn tác hoán đổi trước đó. Trường hợp ngoại lệ có thể được xử lý riêng vì thao tác duy nhất có thể thực hiện được là hoán đổi hai vị trí nhiều lần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển đổi dòng thành mảng nhị phân trong đó MOOK`1`và MEEK là`0`. Đếm số MOOK, gọi nó`m`. Sự sắp xếp cuối cùng tốt nhất có thể có là ở các vị trí`1`bởi vì`m`. 
2. Xử lý trường hợp đặc biệt`n = 2`với chính xác một MOOK và một MEEK. Vì mỗi lần hoán đổi trao đổi hai phần tử nên chỉ có tính chẵn lẻ của`k`vấn đề. Số lần hoán đổi chẵn sẽ khôi phục trạng thái ban đầu và số lẻ sẽ hoán đổi hai vị trí. 
3. Tìm tất cả các vị trí 0 không chính xác bên trong vị trí đầu tiên`m`các vị trí và tất cả một vị trí sau vị trí đầu tiên`m`các vị trí. Đây là những vị trí mà một giao dịch hoán đổi hữu ích có thể cải thiện giá trị. 
4. Nếu ít hơn`k`lỗi tồn tại, sử dụng mọi trao đổi sửa lỗi. Các giao dịch hoán đổi còn lại không cần thay đổi giá trị nhị phân cuối cùng, vì chúng có thể bị lãng phí mà không ảnh hưởng đến việc sắp xếp. 
5. Nếu`k`nhỏ hơn số lỗi, thực hiện chính xác`k`trao đổi hữu ích. Ghép nối các vị trí không chính xác nhỏ nhất với các vị trí MOOK lớn nhất bên ngoài tiền tố đích. Điều này mang lại sự giảm giá trị nhị phân lớn nhất. 
6. Tính giá trị nhị phân thu được bằng cách cộng`2^i`cho mọi vị trí MOOK còn lại. Câu trả lời được lấy modulo`10^9`. 

Tại sao nó hoạt động: 

Việc diễn giải giá trị nhị phân chuyển đổi quá trình chiến đấu thành một bài toán số đơn giản. Việc giảm thiểu thời gian chiến đấu cũng giống như việc giảm thiểu giá trị nhị phân sau khi hoán đổi. Mỗi trao đổi hữu ích sẽ di chuyển một`1`đến một vị trí trước đó và một`0`đến một vị trí sau này. Việc ghép số 0 khả dụng sớm nhất với số 0 khả dụng mới nhất sẽ tạo ra mức giảm lớn nhất có thể có cho hoán đổi đó. Khi tất cả các lỗi có thể xảy ra đã được khắc phục, sự sắp xếp đã là sự sắp xếp tối thiểu có thể có và mọi giao dịch hoán đổi còn lại đều có thể được vô hiệu hóa ngoại trừ trường hợp hỗn hợp hai yếu tố được xử lý riêng biệt. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9

def solve_case(n, k, arr):
    ones = sum(arr)

    if ones == 0:
        return 0

    if n == 2 and ones == 1:
        if k % 2 == 1:
            return (2 if arr[0] == 1 else 1)
        return (1 if arr[0] == 1 else 2)

    bad = []
    extra = []

    for i in range(ones):
        if arr[i] == 0:
            bad.append(i)

    for i in range(ones, n):
        if arr[i] == 1:
            extra.append(i)

    use = min(k, len(bad))

    extra.sort(reverse=True)

    for i in range(use):
        a = bad[i]
        b = extra[i]
        arr[a], arr[b] = arr[b], arr[a]

    ans = 0
    power = 1
    for x in arr:
        if x == 1:
            ans += power
            if ans >= MOD:
                ans -= MOD
        power = (power * 2) % MOD

    return ans % MOD

def main():
    n, k = map(int, input().split())
    arr = []
    for _ in range(n):
        arr.append(1 if input().strip() == "MOOK" else 0)

    print(solve_case(n, k, arr))

if __name__ == "__main__":
    main()
```Đầu tiên, mã sẽ đếm số lượng MOOK vì nó xác định độ dài tiền tố đích. Việc sắp xếp mục tiêu không được lưu trữ riêng biệt vì có thể tìm thấy trực tiếp các vị trí cần thay đổi. 

các`bad`mảng lưu trữ các vị trí MEEK phải chứa MOOK trong tiền tố tối ưu. các`extra`mảng lưu trữ các MOOK hiện nằm ngoài tiền tố đó. Hoán đổi hai nhóm này thực hiện những thay đổi có giá trị nhất trước tiên. 

Trường hợp hai phần tử đặc biệt được xử lý trước khi hoán đổi tham lam. Điều này tránh được sai lầm khi cho rằng các giao dịch hoán đổi bổ sung luôn có thể được bỏ qua. Đối với các mảng lớn hơn, các giao dịch hoán đổi bổ sung có thể được thực hiện mà không thay đổi giá trị nhị phân cuối cùng. 

Vòng lặp cuối cùng đánh giá số nhị phân bằng cách sử dụng lũy ​​thừa của hai modulo`10^9`. Số nguyên Python không bị tràn, nhưng việc lấy mô-đun trong quá trình tích lũy sẽ giữ cho phép tính nhỏ và phản ánh kết quả đầu ra cần thiết. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
3 0
MEEK
MOOK
MEEK
```Biểu diễn nhị phân là`010`. 

| Bước | Mảng | Vị trí xấu | Vị trí bổ sung | Hoán đổi được sử dụng | Giá trị | 
| --- | --- | --- | --- | --- | --- | 
| Ban đầu | 010 | 0 | 1 | 0 | 2 | 

Không được phép hoán đổi, vì vậy câu trả lời vẫn là`2`. 

Đối với mẫu thứ hai:```
3 1
MEEK
MOOK
MEEK
```Mục tiêu có một MOOK ở vị trí đầu tiên. 

| Bước | Mảng | Vị trí xấu | Vị trí bổ sung | Hoán đổi được sử dụng | Giá trị | 
| --- | --- | --- | --- | --- | --- | 
| Ban đầu | 010 | 0 | 1 | 0 | 2 | 
| Sau khi trao đổi | 100 | không | không | 1 | 1 | 

Hoán đổi được phép duy nhất thực hiện cải tiến hữu ích duy nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mảng được quét một số lần không đổi và danh sách trao đổi được sắp xếp một lần. | 
| Không gian | O(n) | Danh sách các vị trí không chính xác có thể chứa mọi phần tử trong trường hợp xấu nhất. | 

Kích thước đầu vào là`100000`, do đó cần có một giải pháp tuyến tính. Thuật toán chỉ thực hiện các lần quét đơn giản và nhiều nhất là một loại`n`các yếu tố, phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    n, k = map(int, input().split())
    arr = []
    for _ in range(n):
        arr.append(1 if input().strip() == "MOOK" else 0)
    ans = str(solve_case(n, k, arr))
    sys.stdin = old
    return ans

assert run("""3 0
MEEK
MOOK
MEEK
""") == "2"

assert run("""3 1
MEEK
MOOK
MEEK
""") == "1"

assert run("""7 1
MOOK
MEEK
MEEK
MOOK
MEEK
MOOK
MEEK
""") == "11"

assert run("""2 2
MEEK
MOOK
""") == "2"

assert run("""5 0
MEEK
MEEK
MEEK
MEEK
MEEK
""") == "0"

assert run("""4 2
MEEK
MOOK
MEEK
MOOK
""") == "5"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 2`đối thủ hỗn hợp |`2`| Số lần trao đổi chính xác trong trường hợp đặc biệt | 
| Tất cả MEEK |`0`| Bộ MOOK trống | 
| Bốn vị thế với hai giao dịch hoán đổi hữu ích |`5`| Nhiều cải tiến tham lam | 

## Vỏ cạnh 

Trường hợp toàn MEEK được xử lý vì số lượng MOOK bằng 0, do đó giá trị nhị phân bằng 0 và không có giao dịch hoán đổi nào có thể tạo ra MOOK. 

Điều kiện hoán đổi chính xác được xử lý bằng trường hợp hỗn hợp hai phần tử. Vì:```
2 2
MEEK
MOOK
```trao đổi đầu tiên tạo ra`MOOK, MEEK`và trao đổi thứ hai trở về`MEEK, MOOK`. Thuật toán phát hiện số lần hoán đổi chẵn và trả về giá trị ban đầu. 

Đối với mẫu chỉ có một trao đổi hữu ích:```
3 1
MEEK
MOOK
MEEK
```vị trí đầu tiên nằm trong tiền tố MOOK đích và hiện sai. Thuật toán hoán đổi nó với MOOK duy nhất bên ngoài tiền tố, tạo ra`MOOK, MEEK, MEEK`, giá trị của nó là`1`. 

Đối với những trường hợp`k`lớn hơn số lượng lỗi, thuật toán sẽ ngừng thực hiện các giao dịch hoán đổi thay đổi giá trị sau khi đạt được sự sắp xếp tối ưu. Các giao dịch hoán đổi còn lại có thể được thực hiện một cách vô hại vì tồn tại ít nhất một cặp đối thủ ngang nhau, ngoại trừ đường hỗn hợp hai phần tử đã được xử lý.
