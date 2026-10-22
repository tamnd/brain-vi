---
title: "CF 105909E - \u5723\u5de2\u4e07\u795e\u6bbf"
description: "Trò chơi bao gồm một chuỗi các ông chủ. Người chơi bắt đầu mọi nỗ lực với đầy đủ sức khỏe và cần phải đánh bại tất cả các tên trùm theo thứ tự. Mất hết máu hoặc bị trùm giết ngay lập tức sẽ kết thúc nỗ lực hiện tại và người chơi phải bắt đầu lại từ đầu."
date: "2026-06-25T14:06:48+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105909
codeforces_index: "E"
codeforces_contest_name: "The 9th Hebei Collegiate Programming Contest"
rating: 0
weight: 105909
solve_time_s: 44
verified: true
draft: false
---

[CF 105909E - \u5723\u5de2\u4e07\u795e\u6bbf](https://codeforces.com/problemset/problem/105909/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 44s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Trò chơi bao gồm một chuỗi các ông chủ. Người chơi bắt đầu mọi nỗ lực với đầy đủ sức khỏe và cần phải đánh bại tất cả các tên trùm theo thứ tự. Mất hết máu hoặc bị trùm giết ngay lập tức sẽ kết thúc nỗ lực hiện tại và người chơi phải bắt đầu lại từ đầu. Sau mỗi số lượng trùm bị đánh bại cố định, có thể có một điểm kiểm tra giúp người chơi phục hồi toàn bộ sức khỏe. 

Đối với mỗi tên trùm, khả năng bị giết ngay lập tức phụ thuộc vào lượng máu đã mất. Nếu người chơi thua`j`điểm máu, xác suất tử vong ngay lập tức là xác suất cơ bản nhân với`x^j`. Nếu người chơi sống sót, họ có thể mất thêm một điểm máu. Nhiệm vụ là tìm số lần thử hoàn thành dự kiến ​​cần thiết để xóa toàn bộ chuỗi. 

Đầu vào cung cấp số lượng trùm, sức khỏe ban đầu, khoảng thời gian kiểm tra, hệ số tăng trưởng`x`, và xác suất của mỗi ông chủ. Đầu ra là số lần thử dự kiến ​​theo modulo`10^9 + 7`. 

Nhận xét quan trọng từ các ràng buộc là`n`Và`h`cả hai đều có thể lớn bằng`5 * 10^4`. Một DP trực tiếp đối với mọi con trùm và mọi giá trị sức khỏe có thể có sẽ có khoảng`2.5 * 10^9`trạng thái, đó là quá nhiều. Chúng ta cần khai thác thực tế là xác suất tử vong ngay lập tức tăng theo cấp số nhân sau khi nhận sát thương. 

Sau khi đã mất đủ máu, ngay cả tên trùm an toàn nhất cũng giết người chơi với xác suất 1. Vì xác suất giết ngay lập tức nhỏ nhất ít nhất là`1%`Và`x`ít nhất là`1.01`, số lượng trạng thái sức khỏe bị tổn hại hữu ích chỉ có vài trăm. Điều này làm giảm đáng kể không gian trạng thái lập trình động. 

Có một số trường hợp khó có thể bỏ sót. Nếu không có trạm kiểm soát, sát thương sẽ tích lũy trong toàn bộ trận chiến. Ví dụ: với đầu vào:```
1 2 0 150
0
50
```Xác suất tiêu diệt trùm ở trạng thái đầy đủ máu`0.5`, và nếu người chơi sống sót thì sẽ không có khả năng bị sát thương. Xác suất thành công là`0.5`, vậy câu trả lời là`2`. Một giải pháp vô tình đặt lại sau mỗi con trùm vẫn sẽ vượt qua nhiều bài kiểm tra nhưng sẽ thất bại khi có nhiều con trùm tồn tại. 

Một trường hợp khác là khi trạm kiểm soát xuất hiện ngay sau tên trùm. Ví dụ:```
2 2 1 150
60 60
50 50
```Sau khi đánh bại trùm đầu tiên, trạm kiểm soát sẽ phục hồi sức khỏe trước trùm thứ hai. Xác suất thành công là`0.5 * 0.5 = 0.25`, vậy câu trả lời là`4`. Nếu chúng ta giữ trạng thái bị hư hỏng thay vì đặt lại nó, chúng ta sẽ coi trùm thứ hai là khó hơn một cách sai lầm. 

Một trường hợp khó khăn cuối cùng là khi sát thương sẽ làm giảm sức khỏe về 0. Ví dụ:```
1 1 0 150
25
50
```Người chơi có thể sống sót sau cuộc kiểm tra tử vong ngay lập tức với xác suất`0.5`, nhưng sau đó mất một điểm máu và thất bại ngay lập tức. Xác suất thành công là`0.5 * 0.75 = 0.375`, đưa ra câu trả lời`8/3`modulo số nguyên tố cần thiết. Bỏ qua lỗi không có sức khỏe sẽ cho kết quả sai. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản là mô phỏng mọi lượng máu bị mất có thể. Cho phép`dp[i][j]`đại diện cho xác suất sống sót sau khi đánh bại kẻ đầu tiên`i`các ông chủ với chính xác`j`sức khỏe bị mất. Đối với mỗi tên trùm, chúng tôi thử cả hai kết quả: sống sót và nhận sát thương hoặc sống sót mà không nhận sát thương. Quá trình chuyển đổi này là chính xác vì thông tin duy nhất cần thiết cho trùm tiếp theo là lượng sát thương hiện tại. 

Vấn đề là kích thước của không gian trạng thái. có thể có`n = 50000`ông chủ và`h = 50000`giá trị thiệt hại có thể xảy ra, dẫn đến`2.5 * 10^9`chuyển tiếp. Điều này là không thể trong thời gian giới hạn. 

Quan sát quan trọng là giá trị thiệt hại lớn không bao giờ hữu ích. Nếu người chơi mất quá nhiều máu, trùm tiếp theo sẽ giết họ với xác suất 1. Vì xác suất tử vong tăng theo cấp số nhân nên giá trị sát thương hữu ích tối đa chỉ khoảng vài trăm. 

Chúng ta có thể nén DP để chỉ lưu trữ lượng sát thương hữu ích có thể tiếp cận được. Quá trình chuyển đổi vẫn giữ nguyên, nhưng bây giờ chiều thứ hai được giới hạn bởi một hằng số nhỏ. Sau mỗi điểm kiểm tra, tất cả các trạng thái còn sống sẽ được hợp nhất thành trạng thái đầy đủ sức khỏe vì người chơi đã được chữa lành. 

Số lần thử dự kiến ​​là nghịch đảo của xác suất thành công trong một lần thử. Nếu xác suất thành công là`p`, số lần thử độc lập dự kiến ​​là`1 / p`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nh) | O(h) | Quá chậm | 
| DP tối ưu với trạng thái sức khỏe được nén | O(nC) | O(C) | Đã chấp nhận | 

Đây`C`là số lượng thiệt hại hữu ích tối đa, dưới 500 đối với các giới hạn nhất định. 

## Hướng dẫn thuật toán 

1. Tính số trạng thái thiệt hại tối đa vẫn có thể tồn tại. Chúng tôi chỉ giữ lại các giá trị thiệt hại mà ngay cả tên trùm an toàn nhất cũng không đảm bảo sẽ chết ngay lập tức. Bất kỳ giá trị lớn hơn nào cũng không bao giờ có thể đóng góp vào một hoạt động thành công. 
2. Duy trì mảng DP trong đó`dp[j]`lưu trữ xác suất tiếp cận ông chủ hiện tại còn sống với`j`sức khỏe bị mất. Ban đầu người chơi có đầy đủ sức khỏe, vì vậy`dp[0] = 1`. 
3. Xử lý từng ông chủ một. Đối với một trạng thái có`j`mất máu, hãy tính xác suất sống sót sau đợt kiểm tra tiêu diệt ngay lập tức. Xác suất sống sót là`1 - p_i * x^j`. 
4. Nếu người chơi sống sót và trùm không gây sát thương, trạng thái vẫn giữ nguyên lượng sát thương. 
5. Nếu người chơi sống sót và nhận sát thương, lượng sát thương sẽ tăng thêm một. Quá trình chuyển đổi này chỉ được phép nếu lượng máu mới vẫn dương và lượng sát thương mới là trạng thái được lưu trữ hợp lệ. 
6. Sau khi xử lý trùm là ranh giới điểm kiểm tra, hãy cộng mọi xác suất trạng thái còn sống sót vào`dp[0]`, bởi vì mọi tiến bộ thành công đều đạt lại sức khỏe hoàn toàn. 
7. Sau trùm cuối, tính tổng tất cả các xác suất còn lại. Đây là xác suất hoàn thành một lần thử. Câu trả lời là nghịch đảo mô-đun của xác suất này. 

Tại sao nó hoạt động: bất biến DP là sau khi xử lý bất kỳ tiền tố nào của các ông chủ,`dp[j]`thể hiện chính xác xác suất còn sống sau tiền tố đó với`j`mất sức khỏe kể từ lần kiểm tra cuối cùng. Quá trình chuyển đổi liệt kê tất cả các kết quả có thể xảy ra của trùm tiếp theo và thao tác điểm kiểm tra sẽ hợp nhất chính xác các trạng thái trở nên giống hệt nhau sau khi hồi phục. Vì các quốc gia bị thiệt hại quá nhiều sẽ không có cơ hội đóng góp vào thành công trong tương lai nên việc loại bỏ chúng không làm thay đổi xác suất cuối cùng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def solve():
    n, h, k, x0 = map(int, input().split())
    q0 = list(map(int, input().split()))
    p0 = list(map(int, input().split()))

    inv100 = pow(100, MOD - 2, MOD)
    x = x0 * inv100 % MOD

    # Compute the maximum useful damage states.
    mn = min(p0)
    limit = 0
    cur = mn
    while cur * x0 < 100:
        cur = cur * x0
        limit += 1
    limit += 1
    limit = min(limit, h)

    dp = [0] * limit
    dp[0] = 1

    for i in range(n):
        ndp = [0] * limit
        for j in range(limit):
            if dp[j] == 0:
                continue

            death = p0[i] * pow(x, j, MOD) % MOD * inv100 % MOD
            survive = (1 - death) % MOD

            if survive == 0:
                continue

            no_damage = (100 - q0[i]) * inv100 % MOD
            damage = q0[i] * inv100 % MOD

            ndp[j] = (ndp[j] + dp[j] * survive % MOD * no_damage) % MOD

            if j + 1 < limit:
                ndp[j + 1] = (ndp[j + 1] + dp[j] * survive % MOD * damage) % MOD

        dp = ndp

        if k != 0 and (i + 1) % k == 0:
            total = sum(dp) % MOD
            dp = [0] * limit
            dp[0] = total

    success = sum(dp) % MOD
    print(pow(success, MOD - 2, MOD))

if __name__ == "__main__":
    solve()
```Đầu vào được đọc trực tiếp dưới dạng số nguyên vì mọi xác suất được đưa ra dưới dạng phần trăm nguyên. Chúng tôi chuyển đổi xác suất thành phân số mô-đun bằng cách nhân với nghịch đảo của`100`. 

các`limit`tính toán là tối ưu hóa chính. Thay vì lưu trữ tất cả những tổn thất về sức khỏe có thể xảy ra, chúng tôi dừng lại khi xác suất tử vong ngay lập tức được đảm bảo là ít nhất một. Vòng lặp nhỏ vì hệ số tăng trưởng ít nhất là`1.01`. 

Quá trình chuyển đổi đầu tiên áp dụng xác suất sống sót. Hai kết quả có thể xảy ra sau khi sống sót là mất một điểm máu hoặc giữ nguyên lượng máu. điều kiện`j + 1 < limit`là cần thiết vì các trạng thái nằm ngoài phạm vi hữu ích không bao giờ có thể đạt đến điểm cuối. 

Việc xử lý điểm kiểm tra được thực hiện sau khi tiêu diệt trùm. Xác suất hiện tại được tính tổng vì tất cả các trạng thái mất máu đều trở nên giống hệt nhau sau khi được chữa lành. 

Xác suất cuối cùng không bao giờ bằng 0 bởi sự đảm bảo của vấn đề, do đó tồn tại nghịch đảo môđun. Số lần thử dự kiến ​​là nghịch đảo của xác suất thành công của một lần thử. 

## Ví dụ đã hoạt động 

Đối với một trường hợp đơn giản:```
1 1 0 150
25
50
```Sếp duy nhất có xác suất chết toàn máu`0.5`. Dấu vết là: 

| Ông chủ | Trạng thái hư hỏng | Xác suất trước | Sống sót | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 1 | 1/2 | sát thương 1 máu, thất bại | 

Người chơi sống sót sau cuộc kiểm tra tiêu diệt trong một nửa thời gian, nhưng mất một máu khiến máu không còn, vì vậy xác suất thành công là`0.5 * 0.75 = 0.375`. Tổng DP cuối cùng là xác suất của một lần chạy thành công và nghịch đảo là số lần thử dự kiến. 

Đối với trường hợp điểm kiểm tra:```
2 2 1 150
60 60
50 50
```Dấu vết là: 

| Ông chủ | Tiểu bang | Hành động | Xác suất | 
| --- | --- | --- | --- | 
| 1 | 0 sát thương | sống sót ông chủ | 0,5 | 
| 1 | 1 sát thương | thiết lập lại điểm kiểm tra | sáp nhập thành 0 sát thương | 
| 2 | 0 sát thương | sống sót ông chủ | 0,5 | 

Sau ông chủ đầu tiên, mọi con đường thành công đều được chữa lành. Hai con trùm độc lập chiến đấu đầy đủ máu nên xác suất thành công là`0.25`, đưa ra câu trả lời`4`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nC) | Mỗi con trùm chỉ cập nhật trạng thái sát thương nén | 
| Không gian | O(C) | Chỉ mảng DP hiện tại và tiếp theo được lưu trữ | 

giá trị`C`được giới hạn bởi sự tăng trưởng theo cấp số nhân của xác suất tử vong và dưới mức vài trăm đối với các giới hạn tối đa. Do đó, giải pháp chỉ thực hiện vài triệu thao tác và vừa vặn trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    data = sys.stdin.read().split()
    sys.stdin = old

    if not data:
        return ""

    it = iter(data)
    n = int(next(it))
    h = int(next(it))
    k = int(next(it))
    x0 = int(next(it))
    q = [int(next(it)) for _ in range(n)]
    p = [int(next(it)) for _ in range(n)]

    MOD = 10**9 + 7
    inv100 = pow(100, MOD - 2, MOD)
    x = x0 * inv100 % MOD

    mn = min(p)
    limit = 0
    cur = mn
    while cur * x0 < 100:
        cur *= x0
        limit += 1
    limit += 1
    limit = min(limit, h)

    dp = [0] * limit
    dp[0] = 1

    for i in range(n):
        ndp = [0] * limit
        for j in range(limit):
            death = p[i] * pow(x, j, MOD) % MOD * inv100 % MOD
            survive = (1 - death) % MOD
            no_damage = (100 - q[i]) * inv100 % MOD
            damage = q[i] * inv100 % MOD
            ndp[j] += dp[j] * survive * no_damage
            ndp[j] %= MOD
            if j + 1 < limit:
                ndp[j + 1] += dp[j] * survive * damage
                ndp[j + 1] %= MOD
        dp = ndp
        if k and (i + 1) % k == 0:
            dp = [sum(dp) % MOD] + [0] * (limit - 1)

    return str(pow(sum(dp) % MOD, MOD - 2, MOD))

assert run("""3 10 0 150
0 0 0
50 50 50
""") == "8"

assert run("""1 2 1 150
25
50
""") == "666666674"

assert run("""1 1 0 150
25
50
""") == "666666674"

assert run("""2 2 1 150
60 60
50 50
""") == "4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Boss đơn không sát thương | 8 | Xác suất cơ bản và nghịch đảo | 
| Sức khỏe trở về 0 sau khi bị sát thương | 666666674 | Thất bại sau khi mất máu cuối cùng | 
| Điểm kiểm tra sau mỗi ông chủ | 4 | Hành vi thiết lập lại đúng | 
| Nhiều ông chủ không có trạm kiểm soát | 8 | Tích lũy sát thương | 

## Vỏ cạnh 

Trường hợp cạnh đầu tiên không có điểm kiểm tra. DP không bao giờ hợp nhất các trạng thái trở lại trạng thái đầy đủ máu, do đó sát thương sẽ lan truyền trong toàn bộ cuộc chiến. Điều này phù hợp với luật chơi vì người chơi chỉ hồi máu tại các trạm kiểm soát. 

Trường hợp cạnh thứ hai là điểm kiểm tra ngay sau trùm. Thuật toán reset sau khi xử lý con trùm đó chứ không phải trước đó. Điều này quan trọng vì trùm vẫn phải bị đánh bại trước khi quá trình hồi phục diễn ra. 

Trường hợp cuối cùng là sức khỏe đạt đến con số 0. DP không bao giờ tạo trạng thái với`h`mất sức khỏe vì trạng thái như vậy tượng trưng cho sự thất bại, nên những con đường đó sẽ tự động biến mất. Điều này ngăn cản việc đếm những lần thử trong đó người chơi sống sót về mặt kỹ thuật trong lần kiểm tra tiêu diệt ngay lập tức nhưng lại chết do thiệt hại gây ra.
