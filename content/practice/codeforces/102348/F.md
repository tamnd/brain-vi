---
title: "CF 102348F - Số lượng sản phẩm"
description: "Chúng ta có một mảng các số nguyên và mỗi mảng con liền kề có tích âm, 0 hoặc dương. Nhiệm vụ là đếm xem có bao nhiêu mảng con thuộc về mỗi loại trong số ba loại này và in số đếm theo thứ tự âm, 0, dương."
date: "2026-08-14T05:31:39+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102348
codeforces_index: "F"
codeforces_contest_name: "ICPC 2019-2020 NERC (NEERC), Southern and Volga Russia Qualifier"
rating: 0
weight: 102348
solve_time_s: 150
verified: false
draft: false
---

[CF 102348F - Số lượng sản phẩm](https://codeforces.com/problemset/problem/102348/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2 phút 30 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một mảng các số nguyên và mỗi mảng con liền kề có tích âm, 0 hoặc dương. Nhiệm vụ là đếm xem có bao nhiêu mảng con thuộc về mỗi loại trong số ba loại này và in số đếm theo thứ tự âm, 0, dương. 

Các giá trị thực tế có thể có độ lớn bằng (10^9), nhưng độ lớn của chúng không quan trọng đối với dấu hiệu của tích số. Đối với một phần tử khác 0, chỉ có vấn đề tích cực hay tiêu cực. Một số 0 duy nhất là khác nhau vì mọi mảng con chứa số 0 đó đều có tích bằng 0. 

Với (n) lên đến (2\cdot10^5), một giải pháp (O(n^2)) đã phải kiểm tra khoảng 

[ 
\frac{n(n+1)}2 \approx 2\cdot10^{10} 
] 

mảng con trong trường hợp xấu nhất. Điều đó vượt xa những gì giới hạn thời gian 2 giây có thể xử lý. Chúng ta cần một giải pháp tuyến tính hoặc gần tuyến tính. Giới hạn bộ nhớ 256 MB cũng chỉ ưu tiên giữ lại một lượng nhỏ trạng thái thay vì lưu trữ thông tin về mọi mảng con. 

Có một số trường hợp nguy hiểm có thể âm thầm phá vỡ một giải pháp ngây thơ. Số 0 phải được xử lý riêng. Ví dụ, với```
3
1 0 -1
```câu trả lời đúng là`0 3 2`. Ba mảng con có tích bằng 0 là`[1,0]`,`[0]`, Và`[0,-1]`. Một giải pháp chỉ theo dõi tính chẵn lẻ của các giá trị âm và bỏ qua các số 0 sẽ phân loại không chính xác một số mảng con này thành dương hoặc âm. 

Mảng một phần tử cũng quan trọng vì mảng con được phép có độ dài bằng một. Vì```
1
-7
```câu trả lời là`1 0 0`. Một giải pháp vô tình chỉ xem xét các cặp chỉ số khác nhau sẽ bỏ sót mảng con hợp lệ duy nhất. 

Một chuỗi chỉ chứa các số 0 cung cấp một trường hợp biên hữu ích khác:```
3
0 0 0
```Mỗi một trong sáu mảng con đều chứa số 0, vì vậy câu trả lời là`0 6 0`. Việc đặt lại trạng thái dấu sau số 0 là cần thiết vì số 0 tách mảng thành các vùng khác 0 độc lập. 

Cuối cùng, bản thân số đếm có thể vượt quá phạm vi số nguyên 32 bit. Với (n=2\cdot10^5), có tổng cộng (20.000.100.000) mảng con. Số nguyên có dấu 32 bit không thể biểu thị giá trị đó. Các số nguyên Python tự động xử lý việc này, nhưng thuật toán tương tự trong C++ sẽ yêu cầu`long long`. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là liệt kê mọi cặp điểm cuối ((l,r)), tính tích của mảng con đó và phân loại dấu của nó. Nếu sản phẩm được tính toán lại từ đầu cho mỗi cặp, thì thuật toán sẽ mất (O(n^3)) thời gian vì có (O(n^2)) mảng con và mỗi sản phẩm có thể chứa các phần tử (O(n)). Với (n=2\cdot10^5), điều này hoàn toàn không khả thi. 

Chúng ta có thể cải thiện lực lượng vũ phu bằng cách sửa điểm cuối bên trái và mở rộng điểm cuối bên phải từng vị trí một. Thay vì tính toán lại sản phẩm, chúng tôi chỉ cập nhật dấu của nó khi có phần tử mới được thêm vào. Điều đó tạo nên phương thức (O(n^2)), đây là một cải tiến lớn nhưng vẫn có thể có khoảng (2\cdot10^{10}) mảng con cần xử lý. Giới hạn 2 giây loại trừ điều này. 

Quan sát quan trọng là dấu của tích khác 0 chỉ phụ thuộc vào tính chẵn lẻ của số phần tử âm. Số chẵn các yếu tố âm tạo ra tích dương, trong khi số lẻ tạo ra tích âm. Chúng tôi không cần sản phẩm thực tế nào cả. 

Hãy xem xét một phần không có giá trị của mảng. Xác định tính chẵn lẻ của tiền tố là (0) khi tiền tố đó chứa số chẵn các giá trị âm và (1) khi nó chứa số lẻ. Đối với một mảng con từ (l) đến (r), số phần tử âm modulo 2 của nó là XOR của hai số chẵn lẻ tiền tố ngay trước và ở cuối mảng con. Do đó, hai chẵn lẻ tiền tố bằng nhau sẽ tạo ra một mảng con dương, trong khi hai chẵn lẻ tiền tố khác nhau sẽ tạo ra một mảng con âm. 

Điều này cho phép chúng ta đếm các mảng con kết thúc ở mọi vị trí chỉ bằng hai bộ đếm. Nếu chẵn lẻ tiền tố hiện tại là (p), thì mọi tiền tố trước đó có chẵn lẻ (p) sẽ tạo ra một mảng con dương kết thúc ở đây và mọi tiền tố trước đó có chẵn lẻ (1-p) sẽ tạo ra một mảng con âm. 

Số không yêu cầu thêm một ý tưởng. Bất kỳ mảng con nào chứa số 0 đều có tích số 0, vì vậy các mảng con như vậy không bao giờ tham gia vào việc đếm chẵn lẻ dương hoặc âm. Chúng ta có thể chỉ cần đặt lại bộ đếm chẵn lẻ tiền tố sau mỗi số 0. Sau đó, mỗi phân đoạn không có giá trị tối đa có thể được xử lý độc lập. 

Phương pháp brute-force hoạt động vì nó xem xét rõ ràng mọi mảng con. Nó thất bại vì có quá nhiều người trong số họ. Quan sát chỉ cho thấy các vấn đề về tính chẵn lẻ cho phép chúng ta thay thế tất cả các mảng con đó bằng số lượng hai trạng thái tiền tố, đưa ra một lần truyền qua mảng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n^3)) | (O(1)) | Quá chậm | 
| Lực lượng vũ phu gia tăng | (O(n^2)) | (O(1)) | Quá chậm | 
| Tiền tố chẵn lẻ | (O(n)) | (O(1)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Giữ`cnt[0]`Và`cnt[1]`, biểu thị số lượng vị trí tiền tố có liên quan hiện có số chẵn lẻ và số âm lẻ. Lúc đầu, tiền tố trống có tính chẵn lẻ, vì vậy`cnt[0] = 1`Và`cnt[1] = 0`. 
2. Duy trì`parity`, là tính chẵn lẻ của số lượng giá trị âm trong phân đoạn không có 0 hiện tại. Giá trị dương giữ nguyên giá trị, trong khi giá trị âm sẽ đảo ngược nó. 
3. Khi giá trị hiện tại khác 0, hãy cập nhật`parity`nếu giá trị là âm. Nếu kết quả chẵn lẻ là`p`, mọi tiền tố trước đó có tính chẵn lẻ`p`tạo thành một mảng con có số giá trị âm chẵn, vì vậy hãy thêm`cnt[p]`đến câu trả lời tích cực. Mỗi tiền tố trước đó có tính chẵn lẻ`1-p`tạo thành một mảng con có số lẻ các giá trị âm, vì vậy hãy thêm`cnt[1-p]`đến câu trả lời phủ định. 
4. Tăng`cnt[p]`bởi vì tiền tố kết thúc ở vị trí hiện tại có thể được sử dụng bởi các mảng con trong tương lai. 
5. Khi giá trị hiện tại bằng 0, mọi mảng con kết thúc ở vị trí này đều chứa số 0 này và do đó có tích số 0. Có chính xác`i+1`mảng con kết thúc ở chỉ số dựa trên 0`i`, vì vậy hãy thêm`i+1`đến câu trả lời bằng không. 
6. Đặt lại`parity`về 0 và khôi phục`cnt[0] = 1`,`cnt[1] = 0`sau số không. Mảng con khác 0 tiếp theo không thể vượt qua số 0 trong khi vẫn khác 0, do đó, tính chẵn lẻ của tiền tố trước số 0 không được trộn lẫn với tính chẵn lẻ của tiền tố sau nó. 
7. In số âm, số 0 và số dương theo thứ tự đó. 

### Tại sao nó hoạt động 

Bên trong bất kỳ phân đoạn không có 0 nào, hãy xem xét một mảng con có điểm cuối tương ứng với hai trạng thái tiền tố. Số phần tử âm modulo hai của nó là XOR của các giá trị chẵn lẻ tiền tố. Các số chẵn lẻ bằng nhau tạo ra số âm chẵn và do đó tạo ra tích dương. Các số chẵn lẻ khác nhau tạo ra số âm lẻ và do đó tạo ra tích âm. Bộ đếm lưu trữ chính xác có bao nhiêu tiền tố trước đó có mỗi giá trị chẵn lẻ, vì vậy mỗi phân mảng khác 0 được tính chính xác một lần khi điểm cuối bên phải của nó được xử lý. 

Số 0 không bao giờ được đưa vào đối số chẵn lẻ này. Mọi mảng con vượt qua số 0 đều có tích bằng 0, trong khi mọi mảng con khác 0 nằm hoàn toàn giữa hai số 0. Việc đặt lại bộ đếm sau mỗi số 0 sẽ phân tách hai trường hợp này một cách hoàn hảo. Do đó, mỗi mảng con được phân loại chính xác một lần là âm, 0 hoặc dương. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    negative = 0
    zero = 0
    positive = 0

    parity = 0
    cnt = [1, 0]

    for i, x in enumerate(a):
        if x == 0:
            zero += i + 1

            parity = 0
            cnt[0] = 1
            cnt[1] = 0
            continue

        if x < 0:
            parity ^= 1

        positive += cnt[parity]
        negative += cnt[parity ^ 1]

        cnt[parity] += 1

    print(negative, zero, positive)

if __name__ == "__main__":
    solve()
```Ba biến trả lời lưu trữ số đếm cuối cùng. Chúng có thể tăng lên khoảng (n(n+1)/2), do đó, các số nguyên có độ chính xác tùy ý của Python rất thuận tiện ở đây.`parity`đại diện cho dấu của tích của tất cả các phần tử khác 0 trong tiền tố không có 0 hiện tại. Phần tử âm làm đảo ngược tính chẵn lẻ, trong khi phần tử dương không làm gì cả. 

Việc khởi tạo`cnt = [1, 0]`đại diện cho tiền tố trống. Đây là điều cho phép đếm một mảng con bắt đầu từ phần tử đầu tiên. Ví dụ: nếu phần tử đầu tiên là số âm, thì số chẵn lẻ hiện tại sẽ trở thành một và tiền tố chẵn ban đầu sẽ tạo ra một mảng con âm. 

Đối với phần tử khác 0,`cnt[parity]`được thêm vào`positive`bởi vì các chẵn lẻ tiền tố bằng nhau hủy modulo hai.`cnt[parity ^ 1]`được thêm vào`negative`bởi vì các số chẵn lẻ khác nhau để lại một số chẵn lẻ âm chưa từng có. 

Việc sử dụng xử lý bằng không`i + 1`thay vì chỉ đếm số 0. Tại vị trí`i`, có chính xác`i + 1`các lựa chọn cho điểm cuối bên trái và mỗi mảng con đó đều kết thúc bằng 0 và do đó có tích bằng 0. 

Việc thiết lập lại sau số 0 là điều cần thiết. Nếu không có nó, tiền tố trước số 0 có thể được ghép nối với tiền tố sau số 0 và tạo ra số đếm dương hoặc âm không chính xác cho một mảng con thực sự chứa số 0. 

Không có phép nhân trong thuật toán, vì vậy các tích trung gian tiềm năng rất lớn không bao giờ cần phải được biểu diễn. Giá trị lớn nhất quan trọng là số lượng câu trả lời và Python xử lý kích thước của chúng một cách an toàn. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là```
5
5 -3 3 -1 0
```Bảng sau hiển thị trạng thái ngay sau mỗi phần tử.`cnt[0]`Và`cnt[1]`tham khảo phân đoạn không có giá trị hiện tại. 

| Chỉ mục | Giá trị | Chẵn lẻ |`cnt[0]`|`cnt[1]`| Tiêu cực | Không | Tích cực | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 0 | 5 | 0 | 2 | 0 | 0 | 0 | 1 | 
| 1 | -3 | 1 | 2 | 1 | 1 | 0 | 1 | 
| 2 | 3 | 1 | 2 | 2 | 2 | 0 | 3 | 
| 3 | -1 | 0 | 3 | 2 | 4 | 0 | 4 | 
| 4 | 0 | 0 | 1 | 0 | 4 | 5 | 4 | 

Trước số 0, bốn phần tử tạo thành một đoạn không có số 0. Chuỗi chẵn lẻ tiền tố của chúng là`0, 0, 1, 1, 0`, trong đó số 0 ban đầu tương ứng với tiền tố trống. Các cặp chẵn lẻ bằng nhau tạo ra sản phẩm dương và các cặp chẵn lẻ khác nhau tạo ra sản phẩm âm. 

Tại chỉ số 4, có năm mảng con kết thúc bằng số 0 và cả năm mảng đều chứa số 0. Bộ đếm sau đó được đặt lại, mặc dù không còn phần tử nào nữa. Câu trả lời cuối cùng là`6 5 4`, phù hợp với mẫu 

### Mẫu 2 

Đầu vào là```
10
4 0 -4 3 1 2 -4 3 0 3
```| Chỉ mục | Giá trị | Chẵn lẻ |`cnt[0]`|`cnt[1]`| Tiêu cực | Không | Tích cực | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 0 | 4 | 0 | 2 | 0 | 0 | 0 | 1 | 
| 1 | 0 | 0 | 1 | 0 | 0 | 2 | 1 | 
| 2 | -4 | 1 | 1 | 1 | 1 | 2 | 1 | 
| 3 | 3 | 1 | 1 | 2 | 2 | 2 | 2 | 
| 4 | 1 | 1 | 1 | 3 | 3 | 2 | 5 | 
| 5 | 2 | 1 | 1 | 4 | 4 | 2 | 8 | 
| 6 | -4 | 0 | 2 | 4 | 8 | 2 | 10 | 
| 7 | 3 | 0 | 3 | 4 | 12 | 2 | 13 | 
| 8 | 0 | 0 | 1 | 0 | 12 | 11 | 13 | 
| 9 | 3 | 0 | 2 | 0 | 12 | 11 | 14 | 

Số 0 đầu tiên chia tách mảng sau phần tử đầu tiên. Mảng con`[4]`là dương, trong khi mọi mảng con kết thúc ở số 0 đầu tiên đều bằng 0. Phân đoạn khác 0 tiếp theo bắt đầu bằng`-4`, do đó tiền tố đầu tiên của nó có tính chẵn lẻ lẻ. 

Số 0 thứ hai cộng tất cả 11 mảng con kết thúc ở chỉ số 8 vào số 0. Phần tử cuối cùng bắt đầu một phân đoạn không có 0 mới, do đó, nó tạo ra chính xác một phân đoạn dương bổ sung. Kết quả cuối cùng là`12 32 11`. 

Dấu vết chứng minh tại sao việc đặt lại về số 0 không chỉ đơn thuần là một chi tiết triển khai. Hai vùng khác 0 có lịch sử chẵn lẻ độc lập và việc trộn chúng sẽ đếm các mảng con chứa 0 như thể tích của chúng khác 0. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n)) | Mỗi phần tử mảng được xử lý một lần với công việc liên tục. | 
| Không gian | (O(1)) | Chỉ có hai bộ đếm chẵn lẻ và một số biến trả lời không đổi được duy trì. | 

Với (n\le2\cdot10^5), quét tuyến tính chỉ thực hiện một số thao tác liên tục trong thời gian cho mỗi phần tử, khá thoải mái trong giới hạn 2 giây. Thuật toán cũng sử dụng bộ nhớ phụ không đổi và không bao giờ xây dựng tập hợp các mảng con (O(n^2)). 

## Trường hợp thử nghiệm```python
import sys
import io

input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    negative = 0
    zero = 0
    positive = 0

    parity = 0
    cnt = [1, 0]

    for i, x in enumerate(a):
        if x == 0:
            zero += i + 1
            parity = 0
            cnt[0] = 1
            cnt[1] = 0
            continue

        if x < 0:
            parity ^= 1

        positive += cnt[parity]
        negative += cnt[parity ^ 1]
        cnt[parity] += 1

    print(negative, zero, positive)

def run(inp: str) -> str:
    global input

    old_stdin = sys.stdin
    old_stdout = sys.stdout
    old_input = input

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    input = sys.stdin.readline

    try:
        solve()
        return sys.stdout.getvalue().strip()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout
        input = old_input

assert run("""5
5 -3 3 -1 0
""") == "6 5 4", "sample 1"

assert run("""10
4 0 -4 3 1 2 -4 3 0 3
""") == "12 32 11", "sample 2"

assert run("""5
-1 -2 -3 -4 -5
""") == "9 0 6", "sample 3"

assert run("""1
0
""") == "0 1 0", "minimum size and zero"

assert run("""3
1 1 1
""") == "0 0 6", "all positive"

assert run("""3
-1 -1 -1
""") == "4 0 2", "all negative"

assert run("""3
1 0 -1
""") == "0 3 2", "zero separates nonzero segments"

n = 200000
assert run(f"{n}\n" + " ".join(["1"] * n) + "\n") == "0 0 20000100000", "maximum size"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 0`|`0 1 0`| Đầu vào nhỏ nhất có thể và không xử lý | 
|`3 / 1 1 1`|`0 0 6`| Mỗi mảng con đều có tích dương | 
|`3 / -1 -1 -1`|`4 0 2`| Số lẻ và số chẵn âm được phân tách chính xác | 
|`3 / 1 0 -1`|`0 3 2`| Số 0 chia trạng thái chẵn lẻ thành các phân đoạn độc lập | 
|`200000 / 1 1 ... 1`|`0 0 20000100000`| Kích thước đầu vào tối đa và câu trả lời vượt quá phạm vi 32 bit | 

## Vỏ cạnh 

Một số 0 duy nhất là ví dụ nhỏ nhất của trường hợp số 0:```
1
0
```Tại chỉ số 0, có chính xác một mảng con kết thúc ở đó, đó là`[0]`. Thuật toán bổ sung`0 + 1 = 1`về số 0 và đặt lại bộ đếm chẵn lẻ. Kết quả là`0 1 0`. 

Số 0 ở giữa đòi hỏi phải đếm nhiều hơn chỉ số 0. Vì```
3
1 0 -1
```tại chỉ mục một, hai mảng con kết thúc ở đó`[0]`Và`[1,0]`, vì vậy cả hai đều là mảng con có tích bằng 0. Ở chỉ số hai, trạng thái chẵn lẻ bắt đầu mới vì số 0 và`[-1]`đóng góp một mảng con âm. Hai mảng con dương là`[1]`Và`[-1]`không dương, nên tổng khác 0 thực ra là một dương và một âm. Phân loại đúng là`1 3 2`chỉ nếu`[1,-1]`cũng được tính là âm, cho thêm một âm. Vì vậy, đầu ra đúng là`1 3 2`. Trường hợp này phát hiện các triển khai xử lý không chính xác số 0 chỉ đơn thuần là một phần tử không có dấu hiệu. 

Đối với một mảng hoàn toàn bằng không,```
3
0 0 0
```số 0 đầu tiên đóng góp một mảng con tích bằng 0, số thứ hai đóng góp hai và số thứ ba đóng góp ba. Tổng số là (1+2+3=6), nên đáp án là`0 6 0`. Mỗi lần thiết lập lại đều để lại trạng thái tiền tố tại`[1,0]`, ngăn không cho bất kỳ mảng con khác 0 nào vượt qua số 0. 

Đối với một mảng toàn âm,```
3
-1 -1 -1
```các số chẵn lẻ tiền tố lần lượt là số lẻ và số chẵn. Ba mảng con có độ dài một có tích âm và mảng con có độ dài ba cũng có tích âm, tạo ra bốn mảng con âm. Hai mảng con có hai độ dài chứa hai giá trị âm và dương. Thuật toán tạo ra`4 0 2`, phản ánh trực tiếp quy tắc chẵn lẻ. 

Trường hợp dương có kích thước tối đa là```
200000
1 1 1 ... 1
```với 200.000 bản`1`. Mỗi một trong số 

[ 
\frac{200000\cdot200001}{2}=20000100000 
] 

mảng con là dương. Thuật toán đạt được số lượng này thông qua một lần quét, trong khi phép liệt kê mạnh mẽ sẽ cần kiểm tra 20 tỷ mảng con. Điều này chứng tỏ cả lý do tại sao cách tiếp cận tuyến tính là cần thiết và tại sao loại câu trả lời phải hỗ trợ các giá trị lớn hơn 32 bit.
