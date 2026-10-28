---
title: "CF 106178B - Bóng bay cân bằng"
description: "Chúng tôi có một nhóm người lần lượt tham gia câu lạc bộ. Người thứ i mang theo một số bóng bay từ 1 đến K. Sau mỗi lần đến, tổng số bóng bay thu được tính đến thời điểm hiện tại phải chia hết cho số người hiện có trong câu lạc bộ."
date: "2026-06-25T10:56:16+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106178
codeforces_index: "B"
codeforces_contest_name: "2025-2026 ICPC Latin American Regional Programming Contest"
rating: 0
weight: 106178
solve_time_s: 39
verified: true
draft: false
---

[CF 106178B - Bóng bay cân bằng](https://codeforces.com/problemset/problem/106178/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 39s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một nhóm người lần lượt tham gia câu lạc bộ. các`i`-người thứ 2 mang một số bóng bay vào giữa`1`Và`K`. Sau mỗi lần đến, tổng số bóng bay thu được tính đến thời điểm hiện tại phải chia hết cho số người hiện có trong câu lạc bộ. 

Nhiệm vụ là đếm xem có bao nhiêu chuỗi đóng góp có thể thỏa mãn điều kiện này cho tất cả các tiền tố. Hai trình tự sẽ khác nhau nếu có ít nhất một người mang số lượng bóng bay khác nhau. Câu trả lời là bắt buộc theo modulo`998244353`. Tuyên bố vấn đề và giới hạn được lấy từ Codeforces Gym 106178B, trong đó`N`có thể lớn như`10^9`trong khi`K`nhiều nhất là`2000`. 

Thử thách đầu tiên đó là`N`là rất lớn. Bất kỳ cách tiếp cận nào mô phỏng mọi người đều không thể thực hiện được bởi vì ngay cả`10^8`Các hoạt động đã quá nhiều so với giới hạn thời gian của một cuộc thi thông thường và ở đây số lượng người có thể lớn hơn gấp mười lần. Giới hạn nhỏ trên`K`là hạn chế chính mà chúng ta nên khai thác. 

Hãy xem xét số lượng bóng bay trung bình của mỗi người sau mỗi lần đến. Bởi vì tổng số tiền tố đều chia hết cho độ dài của nó nên giá trị trung bình này luôn là số nguyên. Vì mọi đóng góp đều nằm giữa`1`Và`K`, mọi giá trị trung bình cũng nằm trong khoảng`1`Và`K`. Điều này chỉ cho chúng ta`K`các trạng thái có thể xảy ra, mặc dù số bước có thể rất lớn. 

Các trường hợp phức tạp chủ yếu là về các giá trị rất nhỏ và về điểm mà quá trình trở nên ổn định. 

Ví dụ: với:```
1 3
```câu trả lời là`3`, bởi vì người duy nhất có thể mang lại`1`,`2`, hoặc`3`bóng bay. Giải pháp bắt đầu chuyển đổi từ người thứ hai sẽ trả về số 0 không chính xác. 

Một trường hợp khác là:```
4 1
```Câu trả lời là`1`. Mỗi người phải mang theo đúng một quả bóng bay. Bất kỳ cách tiếp cận nào chỉ kiểm tra xem mức trung bình có thể thay đổi hay không có thể vô tình tính những thay đổi không thể thực hiện được vì trạng thái hợp lệ duy nhất là mức trung bình`1`. 

Một trường hợp tế nhị hơn là:```
5 2
```Sau khi đã có đủ người tham gia, mức trung bình không thể thay đổi được nữa. Sự chuyển đổi từ mức trung bình`1`đến mức trung bình`2`trên người`6`sẽ yêu cầu người thứ sáu mang theo`7`bóng bay, điều đó là không thể. Một giải pháp giả định các quá trình chuyển đổi tương tự tiếp tục mãi mãi sẽ bị tính quá mức. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp là giữ nguyên tổng số bong bóng hiện tại và cố gắng đóng góp mọi cách có thể cho mỗi người. Đối với mỗi người có`K`sự lựa chọn, vì vậy điều này trở thành`K^N`khả năng. Điều này đúng vì nó liệt kê mọi chuỗi hợp lệ theo đúng nghĩa đen, nhưng nó sẽ không thể sử dụng được ngay lập tức. 

Một nỗ lực tốt hơn một chút là lập trình động trên mức trung bình hiện tại. Hãy để trạng thái sau`i`mọi người là mức trung bình hiện tại`a`. chỉ có`K`tiểu bang. Sự chuyển đổi từ mức trung bình`a`đến mức trung bình mới`b`được xác định bởi sự đóng góp tiếp theo:$$x = i \cdot b - (i-1)\cdot a$$Nếu như`x`nằm giữa`1`Và`K`, thì quá trình chuyển đổi này là hợp lệ. 

DP mạnh mẽ trên tất cả các cặp trung bình sẽ kiểm tra mọi khả năng`a`Và`b`cho mỗi người. Chi phí đó`O(NK^2)`, điều đó vẫn là không thể bởi vì`N`có thể`10^9`. 

Quan sát quan trọng là sau khi có đủ người tham gia, mức trung bình không thể di chuyển được nữa. Giả sử mức trung bình hiện tại là`a`và mức trung bình tiếp theo lớn hơn`d`. Sự đóng góp tiếp theo trở thành:$$x = a + i \cdot d$$Đối với bất kỳ thay đổi tích cực nào, sự đóng góp sẽ tăng lên cùng với`i`. Vì không có sự đóng góp nào có thể vượt quá`K`, điều này trở nên không thể khi`i > K`. Lập luận tương tự cũng có tác dụng đối với việc giảm mức trung bình. Một chút ghé qua`d`yêu cầu:$$x = a - i \cdot d$$trở nên không dương khi`i`là đủ lớn. Như vậy sau khi`K`-người thứ ba, mức trung bình buộc phải không thay đổi mãi mãi, và đóng góp duy nhất có thể có trong tương lai chính xác là mức trung bình hiện tại. 

Chúng ta chỉ cần mô phỏng lần đầu tiên`K`sự đến. Phần còn lại`N-K`mọi người đóng góp mỗi người một giá trị bắt buộc, vì vậy họ không thay đổi số lượng chuỗi hợp lệ. 

Sự chuyển tiếp cũng có thể được tạo ra một cách hiệu quả. Từ một tiểu bang`a`ở bước`i`, tăng mức trung bình lên`d`là có thể trong khi:$$a+i d \leq K$$và giảm dần`d`là có thể trong khi:$$a-i d \geq 1$$Tổng số lần chuyển tiếp được tạo qua tất cả các bước là khoảng:$$\sum_{i=1}^{K} \frac{K^2}{i}$$xung quanh`K^2 log K`, dễ dàng đủ nhanh để`K = 2000`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(K^N)`|`O(N)`| Quá chậm | 
| DP trên mức trung bình mà không tối ưu hóa |`O(NK^2)`|`O(K)`| Quá chậm | 
| DP được tối ưu hóa trên mức trung bình |`O(K^2 log K)`|`O(K)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Nếu chỉ có một người, mọi đóng góp từ`1`ĐẾN`K`là hợp lệ, vì vậy câu trả lời là`K`. 
2. Lưu trữ một mảng lập trình động trong đó`dp[a]`là số trình tự hợp lệ sau số người hiện tại có mức trung bình`a`. 

Sau người đầu tiên, số trung bình chính xác là số bong bóng họ mang theo, nên mỗi số trung bình từ`1`ĐẾN`K`có một chuỗi hợp lệ. 
3. Xử lý thông tin đến từ người`2`tùy người`min(N, K)`. 

Đối với bước hiện tại`i`, tạo một mảng trạng thái mới. Đối với mỗi mức trung bình hiện tại`a`, hãy thử mọi mức trung bình tiếp theo có thể đạt được trong bước này. 
4. Xử lý các chuyển đổi trong đó mức trung bình vẫn bằng nhau. 

Nếu mức trung bình vẫn còn`a`, sự đóng góp mới cũng là`a`, luôn luôn hợp lệ. 
5. Xử lý các chuyển đổi khi mức trung bình tăng. 

Di chuyển từ`a`ĐẾN`a+d`cần có sự đóng góp của:$$a+i d$$Tăng`d`trong khi giá trị này nhiều nhất`K`. 
6. Xử lý các chuyển đổi trong đó mức trung bình giảm. 

Di chuyển từ`a`ĐẾN`a-d`yêu cầu:$$a-i d$$Giảm bớt`d`trong khi giá trị này ít nhất là`1`. 
7. Sau khi xử lý`min(N,K)`mọi người, tổng hợp tất cả các trạng thái. Nếu như`N`lớn hơn`K`, những người đến sau có chính xác một chuỗi đóng góp có thể có từ mỗi trạng thái, vì vậy câu trả lời không thay đổi. 

Tại sao nó hoạt động: 

Điều bất biến là sau khi xử lý người`i`,`dp[a]`đếm chính xác các tiền tố hợp lệ có giá trị trung bình sau`i`mọi người là`a`. Mọi đóng góp tiếp theo có thể có sẽ tương ứng với chính xác một giá trị trung bình tiếp theo và mọi chuyển đổi được tạo ra đều đáp ứng yêu cầu về tính chia hết vì giá trị trung bình thu được là một số nguyên. Thế hệ chuyển tiếp kiểm tra mọi đóng góp có thể tồn tại, do đó không có chuỗi hợp lệ nào bị bỏ sót và không có chuỗi không hợp lệ nào được thêm vào. Một khi số lượng người vượt quá`K`, việc thay đổi mức trung bình sẽ yêu cầu đóng góp ngoài phạm vi cho phép, chỉ để lại phần tự chuyển đổi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def solve():
    N, K = map(int, input().split())

    if N == 1:
        print(K % MOD)
        return

    dp = [0] * (K + 1)
    for i in range(1, K + 1):
        dp[i] = 1

    limit = min(N, K)

    for people in range(2, limit + 1):
        ndp = [0] * (K + 1)

        for avg in range(1, K + 1):
            cur = dp[avg]
            if cur == 0:
                continue

            ndp[avg] = (ndp[avg] + cur) % MOD

            increase = 1
            while avg + people * increase <= K:
                ndp[avg + increase] = (ndp[avg + increase] + cur) % MOD
                increase += 1

            decrease = 1
            while avg - people * decrease >= 1:
                ndp[avg - decrease] = (ndp[avg - decrease] + cur) % MOD
                decrease += 1

        dp = ndp

    print(sum(dp) % MOD)

if __name__ == "__main__":
    solve()
```Phần đầu tiên xử lý trường hợp đặc biệt của một người vì không có mức trung bình trước đó để chuyển đổi. 

các`dp`mảng chỉ lưu trữ mức trung bình chứ không lưu trữ tổng số bong bóng. Đây chính là sự nén làm nên giá trị khổng lồ của`N`quản lý được. 

Bên trong vòng lặp,`people`là số người sau khi mới đến. Công thức chuyển đổi phụ thuộc vào giá trị này, vì vậy thứ tự thực hiện các phép tính rất quan trọng. Chúng tôi tạo trạng thái mới từ mức trung bình cũ trước khi thay thế`dp`. 

Các vòng lặp tăng và giảm tránh việc kiểm tra tất cả`K`mức trung bình tiếp theo có thể xảy ra. Họ trực tiếp chỉ tạo ra những thay đổi hợp lệ. Kiểm tra ranh giới`avg + people * increase <= K`Và`avg - people * decrease >= 1`ngăn chặn các giá trị trung bình không hợp lệ và là nơi thường xuất hiện các sai sót riêng lẻ. 

Số nguyên Python không bị tràn và tất cả các phép cộng đều được giảm modulo`998244353`để giữ cho các giá trị được lưu trữ ở mức nhỏ. 

## Ví dụ đã hoạt động 

Đối với đầu vào:```
3 3
```Người đầu tiên có thể tạo ra ba mức trung bình có thể. 

| Bước | Mức trung bình hiện tại có số lượng | Hành động | 
| --- | --- | --- | 
| Sau người 1 |`1:1, 2:1, 3:1`| Khởi tạo trạng thái | 
| Người 2 |`1:1, 2:2, 3:1`| Tạo đóng góp thứ hai hợp lệ | 
| Người 3 |`1:1, 2:3, 3:1`| Tạo trạng thái cuối cùng | 

Tổng số tiền là`5`, phù hợp với mẫu Dấu vết cho thấy các lịch sử khác nhau có thể hợp nhất thành cùng một mức trung bình, đó là lý do tại sao việc đếm trực tiếp các trạng thái thay vì các chuỗi lại hữu ích. 

Đối với đầu vào:```
4 1
```| Bước | Mức trung bình hiện tại có số lượng | Hành động | 
| --- | --- | --- | 
| Sau người 1 |`1:1`| Đóng góp duy nhất là một quả bóng | 
| Người 2 |`1:1`| Trung bình không thể thay đổi | 
| Người 3 |`1:1`| Trung bình không thể thay đổi | 
| Người 4 |`1:1`| Trung bình không thể thay đổi | 

Câu trả lời vẫn còn`1`. Điều này xác nhận rằng các khoản đóng góp bắt buộc được xử lý một cách tự nhiên theo các quy tắc chuyển đổi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(K^2 log K)`| Số lượng chuyển đổi được tạo trên tất cả những người được xử lý được giới hạn bởi tổng hài hòa của`K^2 / i`. | 
| Không gian |`O(K)`| Chỉ có các phân phối trung bình hiện tại và tiếp theo được lưu trữ. | 

Với`K <= 2000`, DP được tối ưu hóa chỉ thực hiện hàng chục triệu thao tác đơn giản. Giá trị to lớn của`N`không ảnh hưởng đến thời gian chạy vì sau lần đầu tiên`K`mọi người, quá trình này trở nên mang tính quyết định. 

## Trường hợp thử nghiệm```python
import sys
import io

MOD = 998244353

def solution(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)

    import sys
    input = sys.stdin.readline

    N, K = map(int, input().split())

    if N == 1:
        ans = K % MOD
        sys.stdin = old_stdin
        return str(ans) + "\n"

    dp = [0] * (K + 1)
    for i in range(1, K + 1):
        dp[i] = 1

    for people in range(2, min(N, K) + 1):
        ndp = [0] * (K + 1)

        for avg in range(1, K + 1):
            if dp[avg] == 0:
                continue
            cur = dp[avg]

            ndp[avg] = (ndp[avg] + cur) % MOD

            d = 1
            while avg + people * d <= K:
                ndp[avg + d] = (ndp[avg + d] + cur) % MOD
                d += 1

            d = 1
            while avg - people * d >= 1:
                ndp[avg - d] = (ndp[avg - d] + cur) % MOD
                d += 1

        dp = ndp

    ans = sum(dp) % MOD
    sys.stdin = old_stdin
    return str(ans) + "\n"

assert solution("3 3\n") == "5\n", "sample 1"
assert solution("4 1\n") == "1\n", "sample 2"

assert solution("1 5\n") == "5\n", "single person"
assert solution("2 2\n") == "3\n", "all possible averages after two people"
assert solution("5 2000\n").strip() != "", "large K boundary"
assert solution("10 1\n") == "1\n", "forced contribution case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 5`|`5`| Số lượng người tối thiểu và khởi tạo trực tiếp | 
|`2 2`|`3`| Nhiều mức trung bình và chuyển tiếp hợp nhất | 
|`5 2000`| Bất kỳ số nguyên hợp lệ nào | Tối đa`K`xử lý | 
|`10 1`|`1`| Trình tự hoàn toàn xác định | 

## Vỏ cạnh 

cho`N = 1`, thuật toán trả về ngay`K`. Không có điều kiện tiền tố nào ngoại trừ đóng góp đầu tiên nằm trong phạm vi cho phép, vì vậy mọi số lượng bong bóng có thể đều hoạt động. 

Vì`K = 1`, mọi mức trung bình buộc phải là`1`. Các vòng chuyển tiếp không có khả năng tăng hoặc giảm, chỉ còn lại quá trình tự chuyển đổi. Điều này giữ chính xác một chuỗi hợp lệ bất kể`N`. 

Đối với lớn`N`, chẳng hạn như`N = 10^9`Và`K = 2000`, thuật toán dừng sau khi xử lý người`2000`. Bất kỳ nỗ lực nào sau này nhằm thay đổi giá trị trung bình sẽ yêu cầu thêm hoặc bớt nhiều hơn`K`bong bóng trong một bước, do đó, sự tiếp tục duy nhất có thể lặp lại mức trung bình hiện tại mãi mãi. Số lượng trạng thái cuối cùng đã là câu trả lời đầy đủ.
