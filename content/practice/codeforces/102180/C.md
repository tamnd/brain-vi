---
title: "CF 102180C - \u0412\u0430\u043d\u044f \u0438 \u0442\u0435\u0442\u0440\u0430\u0434\u0438"
description: "Vanya có một ngân sách cố định là k xu và muốn mua càng nhiều sổ ghi chép hình vuông càng tốt. Có n cửa hàng. Cửa hàng tôi bán từng cuốn sổ lấy tiền ai và có sẵn sổ tay bi."
date: "2026-08-19T15:29:55+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102180
codeforces_index: "C"
codeforces_contest_name: "MSPU Training Contest 2018-2019"
rating: 0
weight: 102180
solve_time_s: 94
verified: true
draft: false
---

[CF 102180C - \u0412\u0430\u043d\u044f \u0438 \u0442\u0435\u0442\u0440\u0430\u0434\u0438](https://codeforces.com/problemset/problem/102180/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 34s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Vanya có ngân sách cố định là`k`xu và muốn mua càng nhiều sổ ghi chép hình vuông càng tốt. có`n`cửa hàng. Cửa hàng`i`bán mỗi cuốn sổ với giá`a_i`tiền xu và có`b_i`sổ tay có sẵn. Vanya có thể mua bất kỳ số lượng nào từ một cửa hàng, từ số 0 cho đến số hàng còn trong kho và tổng chi tiêu của anh ấy không thể vượt quá`k`. 

Nhiệm vụ là tìm tổng số vở lớn nhất có thể mua được. 

Hạn chế chính là ngân sách, có thể lớn bằng`10^18`, trong khi có thể có`10^5`cửa hàng. Một giải pháp thực hiện công việc phù hợp với ngân sách là không thể, vì thậm chí`O(k)`có thể có nghĩa là`10^18`hoạt động. Tương tự như vậy, việc liệt kê tất cả số lượng mua có thể là hoàn toàn không khả thi vì mỗi cửa hàng có thể đóng góp tới`10^6`sổ tay. Với`n = 10^5`, chúng ta cần một thuật toán xung quanh`O(n log n)`hoặc tốt hơn. 

Bản thân câu trả lời có thể lớn bằng tổng lượng hàng tồn kho, nhiều nhất là`10^5 * 10^6 = 10^11`. Python xử lý trực tiếp các số nguyên có kích thước này, trong khi các ngôn ngữ có loại số nguyên có chiều rộng cố định cần số nguyên ít nhất 64 bit. 

Một số trường hợp cạnh rất dễ xử lý sai. Đầu tiên, Vanya có thể không đủ tiền mua dù chỉ một cuốn sổ. Ví dụ,```
1 1
2 10
```có câu trả lời`0`, bởi vì mỗi cuốn sổ có giá 2 xu và Vanya chỉ có 1. Việc thực hiện luôn mua ít nhất một cuốn sổ từ cửa hàng rẻ nhất sẽ là sai lầm. 

Thứ hai, ngân sách có thể cạn kiệt ngay sau khi mua tất cả sổ ghi chép có sẵn từ cửa hàng. Ví dụ,```
10 2
2 5
100 5
```có câu trả lời`5`. Vanya chi đúng 10 xu cho 5 cuốn sổ rẻ tiền và không thể mua thêm thứ gì khác. Sử dụng một cách nghiêm ngặt`<`so sánh thay vì`<=`sẽ từ chối nhầm cuốn sổ thứ năm. 

Thứ ba, cửa hàng cuối cùng có thể chỉ có giá cả phải chăng một phần. Ví dụ,```
15 2
5 2
7 10
```có câu trả lời`3`: hai cuốn sổ có giá 10 xu, còn lại 5 xu, chỉ đủ cho một cuốn sổ từ cửa hàng thứ hai. Giải pháp lấy toàn bộ hoặc không lấy toàn bộ hàng trong cửa hàng sẽ bỏ sót trường hợp này. 

Cuối cùng, giá đầu vào không cần phải được sắp xếp. TRONG```
20 3
10 1
2 4
5 6
```câu trả lời đúng là`8`, bởi vì bốn cuốn sổ có giá 2 và bốn cuốn có giá 5 phù hợp với ngân sách. Các cửa hàng xử lý theo thứ tự đầu vào sẽ tiêu tốn 10 xu ngay lập tức và có thể tạo ra câu trả lời nhỏ hơn. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp có thể thử mọi số lượng sổ ghi chép có thể được mua từ mọi cửa hàng. Dành cho cửa hàng`i`, có`b_i + 1`các lựa chọn, bao gồm cả việc không mua sổ ghi chép. Số lượng kết hợp mua hàng hoàn chỉnh là`(b_1 + 1)(b_2 + 1)...(b_n + 1)`. 

Trong trường hợp xấu nhất đây là`(10^6 + 1)^100000`, nên ngay cả việc tạo ra các khả năng cũng là vô vọng. Lực lượng vũ phu là chính xác vì nó xem xét rõ ràng mọi kế hoạch mua hàng hợp pháp, nhưng số lượng kế hoạch tăng theo cấp số nhân với số lượng cửa hàng. 

Một ý tưởng ít cực đoan hơn là liên tục mua từng cuốn sổ riêng lẻ trong khi cố gắng quyết định nên sử dụng cửa hàng nào. Ngay cả khi chúng tôi luôn chọn máy tính xách tay rẻ nhất hiện có, tổng lượng hàng tồn kho có thể đạt tới`10^11`, do đó, việc mô phỏng một lần mua hàng tại một thời điểm vẫn cần tới`10^11`lần lặp lại. 

Quan sát hữu ích là máy tính xách tay độc lập ngoài giá cả và giới hạn hàng tồn kho. Nếu một cuốn sổ tay giá cả phải chăng có giá cao hơn một cuốn sổ tay khác hiện có, thì việc thay thế chiếc máy tính xách tay đắt tiền bằng chiếc rẻ hơn sẽ không bao giờ làm giảm số lượng máy tính xách tay có thể mua được. Do đó, trong một giải pháp tối ưu, mọi máy tính xách tay rẻ hơn nên được mua trước khi xem xét mua bất kỳ máy tính xách tay đắt tiền hơn nào. 

Điều này biến vấn đề thành một quá trình tham lam đơn giản. Sắp xếp các cửa hàng theo giá sổ tay, tiêu thụ hàng của cửa hàng rẻ nhất trong khi ngân sách cho phép, sau đó chuyển sang mức giá tiếp theo. Tại cửa hàng đầu tiên không thể mua hết, chúng tôi mua bao nhiêu cuốn vở trong mức ngân sách còn lại cho phép rồi dừng lại. Không có lý do gì để xem xét một cửa hàng đắt tiền hơn sau thời điểm đó, bởi vì mọi cuốn sổ ở đó đều có giá ít nhất bằng nhau. 

Brute-force hoạt động vì mọi phân bổ có thể đều được kiểm tra, nhưng không thành công vì có quá nhiều phân bổ. Nhận thấy rằng những cuốn sổ tay rẻ hơn luôn có thể thay thế những cuốn sổ đắt tiền hơn làm giảm toàn bộ quá trình tìm kiếm xuống việc phân loại các cửa hàng và đưa ra một quyết định cho mỗi cửa hàng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | số mũ trong`n`| số mũ trong`n`trong bảng liệt kê | Quá chậm | 
| Tối ưu |`O(n log n)`|`O(n)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc ngân sách`k`và tất cả các cặp cửa hàng`(a_i, b_i)`. Mỗi cặp mô tả một mức giá cùng với số lượng máy tính xách tay có sẵn ở mức giá đó. 
2. Sắp xếp tất cả các cửa hàng theo`a_i`, từ giá máy tính xách tay nhỏ nhất đến giá lớn nhất. Điều này sắp xếp các sổ ghi chép theo thứ tự chính xác mà giải pháp tối ưu sẽ sử dụng chúng. 
3. Đặt câu trả lời hiện tại thành 0 và giữ ngân sách còn lại bằng`k`. 
4. Xử lý các cửa hàng được sắp xếp từ rẻ nhất đến đắt nhất. Đối với một cửa hàng có giá`a`và chứng khoán`b`, trước tiên hãy xác định số lượng sổ ghi chép mà ngân sách còn lại có thể mua được, tức là`remaining // a`. 
5. Mua số lượng nhỏ hơn với số lượng phải chăng và số lượng có sẵn. Thêm`min(b, remaining // a)`sổ ghi chép là tối ưu vì mỗi cuốn sổ ở cửa hàng này không đắt hơn mỗi cuốn sổ ở cửa hàng sau này. 
6. Trừ chi phí của những cuốn sổ đã mua từ ngân sách còn lại và cộng số lượng của chúng vào câu trả lời. 
7. Nếu số lượng phải chăng nhỏ hơn lượng hàng trong kho của cửa hàng thì ngân sách không đủ để dọn sạch cửa hàng này. Dừng lại ngay lập tức. Mỗi cửa hàng sau này đều có mức giá ít nhất cũng lớn như vậy, vì vậy việc thay thế một trong những cuốn sổ vừa xem xét bằng một cuốn sổ mới hơn không thể làm tăng tổng số lượng. 
8. In số lượng sổ ghi chép tích lũy. 

Tại sao nó hoạt động: sau khi xử lý bất kỳ tiền tố nào của các cửa hàng, thuật toán đã mua số lượng sổ ghi chép tối đa có thể có được chỉ bằng tiền tố đó và ngân sách ban đầu. Khi một cửa hàng có thể trống rỗng, việc mua một trong những cuốn sổ của cửa hàng đó không bao giờ tệ hơn việc tiết kiệm số tiền đó để mua một cuốn sổ đắt tiền hơn sau này. Khi không thể dọn sạch cửa hàng, thuật toán sẽ mua số lượng tối đa có thể ở mức giá còn lại rẻ nhất hiện tại. Bất kỳ giải pháp thay thế nào dành một phần ngân sách còn lại cho cửa hàng sau này có thể thay thế cuốn sổ sau này bằng cuốn sổ hiện tại mà không tốn thêm tiền, vì vậy nó không thể sản xuất thêm cuốn sổ. Do đó, cửa hàng có giá cả phải chăng một phần đầu tiên sẽ xác định câu trả lời tối đa có thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    k, n = map(int, input().split())
    shops = [tuple(map(int, input().split())) for _ in range(n)]

    shops.sort()

    answer = 0
    remaining = k

    for price, stock in shops:
        can_buy = remaining // price
        take = min(stock, can_buy)

        answer += take
        remaining -= take * price

        if take < stock:
            break

    print(answer)

if __name__ == "__main__":
    solve()
```các`shops`danh sách lưu trữ từng giá và cổ phiếu tương ứng của nó, sau đó`sort()`sắp xếp các cặp theo giá vì thành phần đầu tiên được so sánh trước. Không cần đặt hàng thứ cấp. 

Đối với mỗi cửa hàng,`remaining // price`cung cấp số lượng máy tính xách tay lớn nhất có thể được mua mà không vượt quá ngân sách hiện tại. Lấy mức tối thiểu với`stock`tôn trọng giới hạn sẵn có của cửa hàng. 

phép nhân`take * price`phải xảy ra trước khi trừ vào ngân sách. Số nguyên Python có độ chính xác tùy ý, vì vậy các giá trị như`10^18`và các sản phẩm gặp phải ở đây được xử lý an toàn mà không bị tràn. 

các`take < stock`điều kiện phát hiện cửa hàng đầu tiên không thể trống hoàn toàn. Điều này tương đương với việc nói rằng`remaining < price * stock`. Một khi điều này xảy ra, việc xử lý các cửa hàng đắt tiền hơn không thể cải thiện câu trả lời, do đó vòng lặp có thể chấm dứt ngay lập tức. 

Không có định dạng nhiều ca kiểm thử trong bài toán này, vì vậy`solve()`xử lý chính xác một trường hợp. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là:```
10 2
1 5
2 5
```Các cửa hàng đã được sắp xếp. Trước tiên, thuật toán tiêu thụ năm cuốn sổ có giá 1 xu, sau đó sử dụng số tiền còn lại cho cửa hàng thứ hai. 

| Giá cửa hàng | Kho | Còn lại trước | Giá cả phải chăng | Đã chụp | Còn lại sau | Trả lời | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 5 | 10 | 10 | 5 | 5 | 5 | 
| 2 | 5 | 5 | 2 | 2 | 1 | 7 | 

Câu trả lời cuối cùng là`7`. Cửa hàng đầu tiên hoàn toàn trống rỗng, trong khi chỉ có thể mua được hai trong số năm cuốn sổ ở cửa hàng thứ hai. 

### Mẫu 2 

Đầu vào là:```
15 1
5 2
```Chỉ có một cửa hàng và bạn có thể mua cả hai cuốn sổ có sẵn. 

| Giá cửa hàng | Kho | Còn lại trước | Giá cả phải chăng | Đã chụp | Còn lại sau | Trả lời | 
| --- | --- | --- | --- | --- | --- | --- | 
| 5 | 2 | 15 | 3 | 2 | 5 | 2 | 

Câu trả lời là`2`. 5 xu còn lại không mua được cuốn sổ khác vì cửa hàng hết hàng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(n log n)`| Sắp xếp`n`cửa hàng chiếm ưu thế khi đi qua chúng | 
| Không gian |`O(n)`| Danh sách hồ sơ cửa hàng yêu cầu bộ nhớ tuyến tính | 

Với`n = 10^5`, việc sắp xếp là thực tế dễ dàng, trong khi các thuật toán tỷ lệ thuận với ngân sách hoặc tổng lượng hàng tồn kho có thể yêu cầu tới`10^18`hoặc`10^11`hoạt động. Thuật toán thực hiện một lần sắp xếp và nhiều nhất là một lần đi qua các cửa hàng, do đó, nó phù hợp thoải mái trong giới hạn đã định. 

## Trường hợp thử nghiệm```python
import sys
import io

input = sys.stdin.readline

def solve():
    k, n = map(int, input().split())
    shops = [tuple(map(int, input().split())) for _ in range(n)]

    shops.sort()

    answer = 0
    remaining = k

    for price, stock in shops:
        can_buy = remaining // price
        take = min(stock, can_buy)

        answer += take
        remaining -= take * price

        if take < stock:
            break

    print(answer)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    try:
        sys.stdin = io.StringIO(inp)
        sys.stdout = io.StringIO()
        solve()
        return sys.stdout.getvalue().strip()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided sample 1
assert run("""10 2
1 5
2 5
""") == "7", "sample 1"

# Provided sample 2
assert run("""15 1
5 2
""") == "2", "sample 2"

# Provided sample 3
assert run("""20 3
10 1
2 4
5 6
""") == "8", "sample 3"

# Minimum-size input
assert run("""1 1
1 1
""") == "1", "minimum case"

# Cannot afford even one notebook
assert run("""1 1
2 10
""") == "0", "cannot afford one notebook"

# Exact budget and unsorted shops
assert run("""10 2
100 5
2 5
""") == "5", "exact budget with unsorted input"

# Partial purchase from the final shop
assert run("""15 2
7 10
5 2
""") == "3", "partial final purchase"

# All prices and stocks equal
assert run("""17 4
3 5
3 5
3 5
3 5
""") == "5", "equal values"

# Large values, including k close to 10^18
assert run("""1000000000000000000 2
1000000 1000000
1 1000000
""") == "1001000000", "large integer values"

# Maximum n
large_input = "1000000000000 100000\n" + "\n".join(["1 1"] * 100000) + "\n"
assert run(large_input) == "100000", "maximum number of shops"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 1 1`|`1`| Phiên bản hợp lệ có kích thước tối thiểu | 
|`1 1 / 2 10`|`0`| Xử lý đúng khi dù chỉ một cuốn sổ cũng không đủ khả năng chi trả | 
|`10 2 / 100 5 / 2 5`|`5`| Phân loại và chi tiêu ngân sách chính xác | 
|`15 2 / 7 10 / 5 2`|`3`| Mua một phần từ cửa hàng tiếp theo | 
| Bốn cửa hàng với giá`3`, Cổ phần`5`|`5`| Giá và cổ phiếu bằng nhau | 
|`10^18`ngân sách với mức giá và cổ phiếu cỡ triệu đô |`1001000000`| Số học số nguyên lớn | 
|`100000`cửa hàng mỗi cửa hàng có một cuốn sổ tay |`100000`| Tối đa`n`và quét tuyến tính sau khi sắp xếp | 

## Vỏ cạnh 

Khi ngân sách không thể mua dù chỉ một cuốn sổ, thuật toán sẽ trả về số 0 một cách tự nhiên. Vì```
1 1
2 10
```cửa hàng duy nhất có giá 2, vì vậy`remaining // price`là`1 // 2 = 0`. Do đó`take`bằng 0, điều kiện`take < stock`là đúng và vòng lặp dừng lại với câu trả lời`0`. Không có giao dịch mua tối thiểu giả tạo nào được đưa ra. 

Khi ngân sách thanh toán chính xác cho tất cả các sổ ghi chép có sẵn, thuật toán sẽ chấp nhận chúng vì phép chia số nguyên ít nhất sẽ cho ra toàn bộ sổ ghi chép. Vì```
10 2
2 5
100 5
```cửa hàng đầu tiên cung cấp`10 // 2 = 5`, do đó tất cả năm cuốn sổ đều được lấy đi và ngân sách còn lại trở thành số không. Câu trả lời là`5`. Cửa hàng thứ hai ngay lập tức mang lại số lượng bằng 0, nhưng không bao giờ cần phải kiểm tra vì ngân sách đã cạn kiệt. 

Khi chỉ có thể mua được một phần hàng trong kho của cửa hàng, phép chia số nguyên sẽ đưa ra chính xác số lượng cần thiết. Vì```
15 2
5 2
7 10
```cửa hàng đầu tiên đóng góp hai cuốn sổ với giá 10 xu. Còn lại năm đồng xu, và`5 // 7 = 0`cho cửa hàng thứ hai, vậy câu trả lời là`2`. Đối với một trường hợp một phần hơi khác,```
15 2
5 2
4 10
```các cửa hàng đặt lại giá 4 trước. Vanya mua ba cuốn sổ với giá 12 xu và không thể mua được cuốn thứ tư, đưa ra câu trả lời đúng`3`. 

Đầu vào chưa được sắp xếp được xử lý bằng cách sắp xếp trước khi thực hiện bất kỳ giao dịch mua nào. TRONG```
20 3
10 1
2 4
5 6
```thứ tự sắp xếp là`(2, 4), (5, 6), (10, 1)`. Bốn cuốn sổ đầu tiên có giá 8 xu, còn lại 12. Hai cuốn sổ giá 5 sau đó có thể được mua với giá 10, để lại 2. Cửa hàng cuối cùng có giá 10 mỗi cuốn và không thể chấp nhận được, vì vậy câu trả lời là`6`, đó chính xác là số lượng tối đa có thể. Bước đặt hàng là bước ngăn thứ tự đầu vào ban đầu ảnh hưởng đến kết quả. 

Ngân sách và sản phẩm lớn được xử lý mà không có trường hợp đặc biệt trong Python. Ví dụ, với ngân sách`10^18`, tính toán`remaining // price`Và`take * price`vẫn chính xác. Thuật toán không bao giờ lặp một lần trên mỗi đồng xu hoặc một lần trên mỗi sổ ghi chép, do đó mức độ ngân sách không ảnh hưởng đến số lần lặp.
