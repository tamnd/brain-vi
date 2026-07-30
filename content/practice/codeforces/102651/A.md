---
title: "CF 102651A - Trận chiến của những người khổng lồ"
description: "Hai đội thi đấu không biết số trận. Một trận thắng mang lại cho đội thắng ba điểm, một trận hòa cho cả hai đội một điểm và trận thua không có điểm. Chúng ta chỉ biết số điểm cuối cùng của đội thứ nhất và đội thứ hai."
date: "2026-07-30T22:36:28+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102651
codeforces_index: "A"
codeforces_contest_name: "Innopolis Open 2020-2021, qualification, contest 1"
rating: 0
weight: 102651
solve_time_s: 402
verified: true
draft: false
---

[CF 102651A - Trận chiến của những người khổng lồ](https://codeforces.com/problemset/problem/102651/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 6 phút 42 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Hai đội thi đấu không biết số trận. Một trận thắng mang lại cho đội thắng ba điểm, một trận hòa cho cả hai đội một điểm và trận thua không có điểm. Chúng ta chỉ biết số điểm cuối cùng của đội thứ nhất và đội thứ hai. Nhiệm vụ là xác định xem liệu số điểm này có đạt được hay không và nếu có, hãy tìm cách chia các trận đấu thành đội một thắng, hòa và đội thứ hai thắng bằng cách sử dụng số trận đấu nhỏ nhất có thể. 

Cho phép`a`là tỷ số cuối cùng của đội đầu tiên và`b`là điểm số cuối cùng của đội thứ hai. Câu trả lời phải chứa ba giá trị: đội thứ nhất thắng bao nhiêu trận, đội hòa bao nhiêu trận và đội thứ hai thắng bao nhiêu. Nếu không có chuỗi trận đấu nào có thể tạo ra điểm số, chúng tôi sẽ xuất ra`-1`. 

Điểm số có thể lớn như`10^9`, vì vậy việc cố gắng mô phỏng các trận đấu hoặc lặp lại số lượng trò chơi có thể có là không thể. Ngay cả một vòng lặp có kích thước`a`hoặc`b`có thể yêu cầu một tỷ lần lặp. Lời giải phải sử dụng các tính chất số học của hệ thống tính điểm và kết thúc trong thời gian không đổi. 

Một số tình huống ranh giới có thể phá vỡ một giải pháp ngây thơ. Ví dụ, đầu vào```
0
1
```không có câu trả lời Sự khác biệt giữa điểm số là`-1`, nhưng mỗi trận đấu đều thay đổi chênh lệch tỷ số theo bội số của ba nên tỷ số này không thể xuất hiện. 

Một trường hợp khác là:```
2
2
```Đầu ra đúng là:```
0 2 0
```Một cách tiếp cận bất cẩn có thể cố gắng thể hiện cả hai điểm bằng chiến thắng vì ba điểm cho mỗi trận thắng có vẻ hiệu quả, nhưng không đội nào có thể giành chiến thắng nếu không cho đối thủ 0 điểm trong trận đấu đó. Hai trận hòa là cách duy nhất để tạo ra hai điểm cho cả hai đội. 

Vụ án```
3
0
```phải sản xuất:```
1 0 0
```Một phương pháp chỉ tìm kiếm kết quả hòa trước tiên có thể bỏ sót rằng một trận thắng duy nhất đã mang lại tỷ số chính xác với số trận đấu tối thiểu. 

## Phương pháp tiếp cận 

Phương pháp bạo lực trực tiếp sẽ thử mọi số trận thắng và hòa có thể có cho đội thứ nhất, sau đó kiểm tra xem liệu điểm số của đội thứ hai có thể được hình thành từ các trận đấu còn lại hay không. Vì điểm số có thể đạt tới`10^9`, việc tìm kiếm như vậy sẽ cần tới hàng tỷ lần lặp trong trường hợp xấu nhất. Cách tiếp cận này đúng về mặt logic vì nó liệt kê các kết hợp phù hợp có thể có, nhưng nó không thể phù hợp với các giới hạn. 

Quan sát hữu ích đến từ việc xem xét sự khác biệt về điểm số. Cho phép`x`là đội đầu tiên chiến thắng,`y`được hòa, và`z`là đội thứ hai giành chiến thắng. Điểm số cuối cùng thỏa mãn:```
3x + y = a
y + 3z = b
```Trừ các phương trình này sẽ loại bỏ các kết quả hòa:```
3x - 3z = a - b
```Vì thế:```
x - z = (a - b) / 3
```Hiệu số giữa các đội phải chia hết cho ba. Sau khi kiểm tra điều kiện đó nhiệm vụ còn lại là chọn số thắng thua. 

Để giảm thiểu tổng số trận đấu, chúng ta nên tối đa hóa số trận thắng có thể thay thế cho trận hòa. Một trận thắng đóng góp ba điểm cho một đội, trong khi một trận hòa chỉ đóng góp một điểm cho mỗi đội, vì vậy việc sử dụng càng nhiều trận thắng càng tốt sẽ làm giảm số trận đấu. 

Nếu đội đầu tiên có lợi thế về điểm số, chúng tôi giữ lại số trận thắng phụ cần thiết cho đội thứ nhất và tối đa hóa chiến thắng của đội thứ hai. Đội thứ hai có thể có nhiều nhất`b // 3`thắng. Trường hợp ngược lại là đối xứng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(max(a, b)) | O(1) | Quá chậm | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính chênh lệch điểm số`a - b`. Nếu nó không chia hết cho 3 thì xuất ra`-1`. Sự khác biệt về điểm số của các đội chỉ được tạo ra bởi các trận thắng và mỗi trận thắng sẽ thay đổi cách biệt đúng ba điểm. 
2. Tính toán`k = (a - b) / 3`. Giá trị này thể hiện số trận thắng mà đội thứ nhất có được nhiều hơn đội thứ hai. Giá trị dương có nghĩa là đội đầu tiên có thêm chiến thắng, trong khi giá trị âm có nghĩa là đội thứ hai có thêm. 
3. Nếu`k`không âm, hãy để đội thứ hai thắng càng nhiều trận càng tốt. Bộ`z = b // 3`, bởi vì mỗi trận thắng của đội thứ hai sẽ tiêu tốn ba điểm từ số điểm của đội thứ hai. Số điểm còn lại của đội thứ hai trở thành kết quả hòa:`y = b - 3z`. Đội đầu tiên sau đó cần`x = z + k`thắng để giữ mức chênh lệch cần thiết. 
4. Nếu`k`là âm, thực hiện phép tính đối xứng. Hãy để đội đầu tiên thắng càng nhiều trận càng tốt bằng cách đặt`x = a // 3`. Số điểm còn lại tính là hòa, đội thứ hai giành chiến thắng`z = x - k`. 
5. Đầu ra`x`,`y`, Và`z`. 

Tại sao nó hoạt động: 

Phương trình chênh lệch tỷ số ấn định sự chênh lệch về số trận thắng của hai đội. Thuật toán không bao giờ thay đổi sự khác biệt đó. Sau khi ấn định số trận thắng tối đa có thể cho đội không có lợi thế cần thiết, số điểm còn lại luôn nhỏ hơn ba nên chỉ có thể tạo ra bằng cách hòa. Việc xây dựng này tạo ra một số điểm hợp lệ. Vì mỗi lần thay thế một trận hòa bằng một trận thắng sẽ làm giảm số trận đấu, nên việc tối đa hóa các trận thắng sẽ giảm thiểu tổng số trận đấu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    a = int(input())
    b = int(input())

    diff = a - b

    if diff % 3 != 0:
        print(-1)
        return

    k = diff // 3

    if k >= 0:
        z = b // 3
        y = b - 3 * z
        x = z + k
    else:
        x = a // 3
        y = a - 3 * x
        z = x - k

    print(x, y, z)

if __name__ == "__main__":
    solve()
```Phần đầu tiên kiểm tra điều kiện không thể duy nhất. Số nguyên Python không bị tràn, vì vậy các giá trị gần`10^9`được an toàn trong suốt quá trình tính toán. 

Khi`k`là dương, mã bắt đầu bằng cách giành được càng nhiều chiến thắng của đội thứ hai càng tốt. Đây là bước giảm thiểu quan trọng vì những chiến thắng đó thay thế một số trận hòa trong khi vẫn duy trì được mức chênh lệch điểm số cần thiết. Số điểm còn lại của đội thứ hai đúng bằng số trận hòa. 

Khi`k`là tiêu cực, ý tưởng tương tự được áp dụng với các đội được đổi chỗ. biểu hiện`z = x - k`hoạt động vì`k`là âm, vì vậy nó sẽ cộng thêm số trận thắng cần thiết cho đội thứ hai. 

Các phép chia sử dụng phép chia sàn số nguyên. Số dư sau khi loại bỏ tất cả các chiến thắng có thể có luôn là`0`,`1`, hoặc`2`, đó là số lần rút thăm hợp lệ. Không cần xử lý đặc biệt đối với điểm bằng 0 vì các công thức tự nhiên tạo ra kết quả bằng 0. 

## Ví dụ đã hoạt động 

Đối với đầu vào:```
2
1
```dấu vết là: 

| một | b | khác biệt | k | x | y | z | 
| --- | --- | --- | --- | --- | --- | --- | 
| 2 | 1 | 1 | không hợp lệ | | | | 

Sự chênh lệch không chia hết cho 3 nên không có chuỗi trận đấu nào có thể tạo nên tỷ số này. Thuật toán từ chối nó ngay lập tức. 

Đối với đầu vào:```
3
0
```dấu vết là: 

| một | b | khác biệt | k | x | y | z | 
| --- | --- | --- | --- | --- | --- | --- | 
| 3 | 0 | 3 | 1 | | | | 
| 3 | 0 | 3 | 1 | 1 | 0 | 0 | 

Đội đầu tiên cần nhiều hơn một chiến thắng so với đội thứ hai. Đội thứ hai không có điểm cho trận thắng hoặc trận hòa, vì vậy câu trả lời là đội thứ nhất thắng một trận. Điều này cũng chứng tỏ rằng công trình xây dựng xử lý một cách tự nhiên một điểm trong đó một đội không có điểm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ các phép tính số học được thực hiện | 
| Không gian | O(1) | Chỉ có một vài biến số nguyên được lưu trữ | 

Điểm tối đa là`10^9`, nhưng thuật toán không bao giờ phụ thuộc vào kích thước của chúng thông qua phép lặp. Nó sử dụng một số phép tính cố định nên dễ dàng phù hợp với giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys
import io

def solution(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)

    def solve():
        input = sys.stdin.readline
        a = int(input())
        b = int(input())

        diff = a - b

        if diff % 3 != 0:
            return "-1"

        k = diff // 3

        if k >= 0:
            z = b // 3
            y = b - 3 * z
            x = z + k
        else:
            x = a // 3
            y = a - 3 * x
            z = x - k

        return f"{x} {y} {z}"

    ans = solve()
    sys.stdin = old_stdin
    return ans

assert solution("2\n1\n") == "-1", "sample 1"
assert solution("3\n0\n") == "1 0 0", "sample 2"
assert solution("0\n0\n") == "0 0 0", "both teams score zero"
assert solution("2\n2\n") == "0 2 0", "only draws"
assert solution("1000000000\n1000000000\n") == "333333333 1 333333333", "large equal scores"
assert solution("1\n4\n") == "0 1 1", "second team advantage"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0 / 0`|`0 0 0`| Kết quả giải đấu trống | 
|`2 / 2`|`0 2 0`| Điểm yêu cầu hòa | 
|`1000000000 / 1000000000`|`333333333 1 333333333`| Giá trị lớn và số học số nguyên | 
|`1 / 4`|`0 1 1`| Xử lý chênh lệch điểm âm | 

## Vỏ cạnh 

cho`0`Và`1`, thuật toán tính toán`diff = -1`. Từ`-1`không chia hết cho ba, nó trả về`-1`ngay lập tức. Điều này xử lý số điểm không thể xảy ra do sự khác biệt mà không có sự kết hợp chiến thắng nào có thể tạo ra. 

Vì`2`Và`2`, hiệu số bằng 0 nên các đội phải có số trận thắng bằng nhau. Thuật toán loại bỏ tất cả các chiến thắng có thể xảy ra với`a // 3 = 0`và để lại hai điểm là trận hòa. Nó xuất ra`0 2 0`, sử dụng số lượng kết quả phù hợp tối thiểu có thể. 

Vì`3`Và`0`, sự khác biệt là ba, vì vậy`k = 1`. Đội thứ nhất phải thắng nhiều hơn đội thứ hai đúng một trận. Đội thứ hai không có điểm, cho`z = 0`Và`y = 0`, và kết quả là`x = 1`. Thuật toán tránh tạo ra các trận hòa không cần thiết và trả về giải đấu nhỏ nhất có thể.
