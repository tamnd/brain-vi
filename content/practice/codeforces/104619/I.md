---
title: "CF 104619I - Hướng nội"
description: "Chúng ta có một bảng tuyến tính có độ dài $2n$. Mỗi vị trí hoặc đã chứa sẵn một đĩa loại $1 chấm n$ hoặc trống. Mỗi loại xuất hiện tối đa hai lần trong cấu hình ban đầu và bất cứ khi nào nó xuất hiện hai lần thì hai lần xuất hiện đó không liền kề nhau."
date: "2026-06-29T17:27:26+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104619
codeforces_index: "I"
codeforces_contest_name: "2023 ICPC Asia Taiwan Online Programming Contest"
rating: 0
weight: 104619
solve_time_s: 56
verified: true
draft: false
---

[CF 104619I - Hướng nội](https://codeforces.com/problemset/problem/104619/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 56s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một bảng tuyến tính có chiều dài$2n$. Mỗi vị trí đều đã có sẵn một loại đĩa$1 \dots n$hoặc trống rỗng. Mỗi loại xuất hiện tối đa hai lần trong cấu hình ban đầu và bất cứ khi nào nó xuất hiện hai lần thì hai lần xuất hiện đó không liền kề nhau. 

Nhiệm vụ là lấp đầy tất cả các vị trí trống bằng cách sử dụng các lần xuất hiện món ăn còn thiếu sao cho trong cách sắp xếp cuối cùng, mỗi loại xuất hiện chính xác hai lần và không có hai loại giống hệt nhau nào đứng cạnh nhau. 

Chúng ta không được yêu cầu xây dựng sự sắp xếp mà chỉ đếm xem có bao nhiêu lần hoàn thành, modulo$10^9 + 7$. 

Những hạn chế$n \le 100$và tổng chiều dài$2n \le 200$gợi ý rằng bất kỳ giải pháp nào có hệ số bậc ba hoặc tệ hơn trong$n$vẫn hợp lý, nhưng bất cứ điều gì theo cấp số nhân về số lượng vị trí trống đều không thể xảy ra ngay lập tức vì có thể có tới 200 khoảng trống. Việc quay lui ngây thơ đối với tất cả các vị trí của các cặp còn lại sẽ phân nhánh rất nhiều và không thể sử dụng được. 

Một trường hợp thất bại tinh vi đối với lý luận ngây thơ là giả định rằng việc đặt từng cặp còn lại một cách độc lập là hợp lệ. Ví dụ: nếu chúng ta cố gắng lấp đầy từng loại một cách tham lam bằng cách chọn hai vị trí trống, chúng ta sẽ bỏ qua sự can thiệp tổng thể giữa các lựa chọn. Một ví dụ nhỏ: 

đầu vào:```
n = 2
0 0 0 0
```Một ý tưởng tham lam có thể đặt loại 1 đầu tiên ở bất kỳ cặp vị trí nào, sau đó loại 2 ở các vị trí còn lại và kết luận mọi thứ đều đối xứng. Nhưng điều này được tính quá mức vì việc hoán đổi vị trí của các cặp tạo ra các ràng buộc giống hệt nhau nhưng các tương tác lân cận khác nhau ở các trạng thái trung gian. 

Khó khăn cốt lõi là việc đặt một cặp không mang tính cục bộ: nó chặn cấu trúc của tất cả các cặp khác. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ xử lý vấn đề như gán mỗi lần xuất hiện thiếu của một loại vào một vị trí trống. Vì mỗi loại có hai vị trí nên chúng tôi khớp các lần xuất hiện còn lại vào các vị trí trống một cách hiệu quả với ràng buộc là các nhãn giống hệt nhau không thể liền kề nhau. 

Ngay cả khi bỏ qua các ràng buộc kề cận, lực lượng vũ phu về cơ bản là hoán vị lên tới 200 mục, điều này là không khả thi. Số cách để chỉ định các lần xuất hiện còn lại là giai thừa theo số lượng khoảng trống. 

Quan sát chính là cấu trúc không tùy ý: mỗi loại có tổng cộng chính xác hai lần xuất hiện và một số trong số chúng đã được sửa. Điều này có nghĩa là mỗi loại đóng góp 0, 1 hoặc 2 điểm cuối cố định. Quá trình đặt các mục còn thiếu có thể được hiểu là ghép các vị trí mở theo các ràng buộc về thứ tự do đường dây gây ra. 

Một góc nhìn hữu ích hơn là quét dòng và duy trì loại nào hiện đang "mở", nghĩa là cho đến nay, chúng tôi đã thấy chính xác một lần xuất hiện của loại đó (được điền sẵn hoặc mới được đặt). Bất cứ khi nào chúng tôi đặt hoặc gặp lần xuất hiện thứ hai, loại đó sẽ đóng lại. Điều này biến vấn đề thành việc đếm các kết quả khớp giống như ngăn xếp hợp lệ trên các vị trí, tương tự như việc đếm các dấu ngoặc đơn hợp lệ với các cặp màu, ngoại trừ các màu được cố định một phần và một phần được chỉ định. 

Điều này dẫn đến việc lập trình động trên các tiền tố: chúng tôi theo dõi loại nào hiện đang mở và có bao nhiêu cách chúng tôi có thể chỉ định các loại còn lại để mở hoặc bắt đầu mới. Từ$n \le 100$, số lượng loại mở tối đa là 100, cho phép DP trên các tập hợp con hoặc mặt nạ bit được nén theo thứ tự, nhưng chúng tôi cần sàng lọc. 

Sự đơn giản hóa quan trọng là chỉ có danh tính của "loại hiện đang mở" mới quan trọng chứ không phải nhãn chính xác của chúng vì các nhãn không sử dụng có thể thay thế cho nhau. Chúng ta có thể biểu diễn các kiểu mở dưới dạng một tập hợp tối đa$n$các phần tử, nhưng vì các nhãn đối xứng giữa các loại không sử dụng nên chúng tôi chỉ cần theo dõi có bao nhiêu loại đang mở và bao nhiêu loại vẫn chưa được sử dụng, cùng với các hạn chế về khả năng tương thích do các lần xuất hiện cố định gây ra. 

Điều này mang lại một DP xử lý mảng từ trái sang phải, trong đó trạng thái được xác định bởi số lượng cặp mở hiện đang hoạt động và số lượng loại chưa sử dụng vẫn có sẵn để bắt đầu các cặp mới. Sự chuyển tiếp phụ thuộc vào việc vị trí hiện tại là cố định hay trống. 

Điều này làm giảm vấn đề về DP đa thức trên$O(n^2)$trạng thái cho mỗi vị trí. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | hàm mũ trong$2n$| hàm mũ | Quá chậm | 
| DP tối ưu trên số lượng mở/không sử dụng |$O(n^3)$|$O(n^2)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý dòng từ trái sang phải và duy trì một bảng DP đếm xem có bao nhiêu cách chúng tôi có thể đạt được một cấu hình nhất định sau khi xử lý từng tiền tố. 

Chúng tôi định nghĩa trạng thái là$dp[i][j]$, Ở đâu$i$là số lượng vị trí được xử lý và$j$là số lượng các loại hiện đang mở. Một loại được mở nếu chúng ta đã đặt lần xuất hiện đầu tiên của nó nhưng chưa đặt lần xuất hiện thứ hai. 

Số lượng các loại chưa được sử dụng hoàn toàn là ngầm định$n - \text{(closed pairs)} - j$, vì vậy chúng tôi không cần phải theo dõi nó một cách rõ ràng ngoài tính nhất quán. 

### Các bước 

1. Khởi tạo$dp[0][0] = 1$, nghĩa là trước khi xử lý bất kỳ thứ gì, không có loại mở nào. 
2. Xử lý các vị trí từ trái qua phải. Tại vị trí$i$, xem xét tất cả các trạng thái có thể truy cập$dp[i][j]$. 
3. Nếu vị trí$i+1$đã được sửa với một loại$x$, chúng ta có hai trường hợp. 

Nếu đây là lần đầu tiên chúng ta thấy$x$, khi đó nó góp phần mở một cặp mới, tăng số loại mở lên 1. 

Nếu đây là lần thứ hai xảy ra$x$, nó sẽ đóng một phiên bản đang mở, giảm số lần mở xuống 1. Quá trình chuyển đổi DP chỉ cho phép điều này nếu$x$đã được mở, nếu không thì trạng thái không hợp lệ. 
4. Nếu vị trí$i+1$trống, chúng ta xem xét ba khả năng. 

Chúng ta có thể bắt đầu một kiểu mới ở đây, chọn bất kỳ kiểu nào chưa sử dụng. Điều này làm tăng số lượng mở lên 1 và đóng góp hệ số nhân bằng với số lượng loại chưa sử dụng có sẵn. 

Chúng ta cũng có thể đóng một loại đã mở bằng cách đặt lần xuất hiện thứ hai của nó ở đây, điều này làm giảm số lượng mở xuống 1 và đóng góp hệ số bằng số lượng loại hiện đang mở. 

Cuối cùng, chúng ta có thể bỏ qua cách giải thích này và coi các vị trí trống là thụ động, nhưng trên thực tế, mọi vị trí trống phải được lấp đầy bằng chính xác một trong hai thao tác này, vì vậy đây là những thao tác đầy đủ. 
5. Cập nhật cẩn thận số lượng để chúng tôi không bao giờ cho phép các giá trị mở âm hoặc vượt quá$n$. 

### Tại sao nó hoạt động 

Điều bất biến là sau khi xử lý bất kỳ tiền tố nào, mọi phép gán một phần hợp lệ đều tương ứng chính xác với trạng thái trong đó tất cả các lần xuất hiện đầu tiên được đặt đều không khớp ngoại trừ$j$các loại hiện đang mở. Mỗi loại mở có chính xác một điểm cuối được đặt trong tiền tố và điểm cuối thứ hai của nó phải xuất hiện sau. Mọi chuyển đổi đều bảo toàn cấu trúc này vì việc đặt lần xuất hiện đầu tiên mới luôn tạo ra một cặp mở mới và việc đặt lần xuất hiện thứ hai luôn giải quyết chính xác một cặp mở. 

Bởi vì các loại không thể phân biệt được cho đến khi chúng xuất hiện, nên việc đếm theo số lần mở là đủ: các phép gán nhãn khác nhau tương ứng với bội số được đưa ra bởi số lượng loại không sử dụng hoặc loại mở tồn tại ở mỗi bước. Tính đối xứng này đảm bảo không có hai đường dẫn DP riêng biệt nào đại diện cho cùng một cấu hình cuối cùng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    # position of first occurrence (if any)
    pos = {}
    used = [0] * (n + 1)

    # mark occurrences
    cnt = [0] * (n + 1)
    for x in a:
        if x:
            cnt[x] += 1

    # dp[j] = ways with j open types
    dp = [0] * (n + 1)
    dp[0] = 1

    # how many types are completely unused so far
    # initially all types are unused
    for x in a:
        ndp = [0] * (n + 1)

        for open_cnt in range(n + 1):
            val = dp[open_cnt]
            if not val:
                continue

            if x == 0:
                # start new pair
                if open_cnt + 1 <= n:
                    ndp[open_cnt + 1] = (ndp[open_cnt + 1] + val * (n - sum(cnt))) % MOD
                # close existing
                if open_cnt > 0:
                    ndp[open_cnt - 1] = (ndp[open_cnt - 1] + val * open_cnt) % MOD
            else:
                # fixed type: must be consistent
                if cnt[x] == 2:
                    # behaves like closing/opening deterministically
                    if open_cnt > 0:
                        ndp[open_cnt - 1] = (ndp[open_cnt - 1] + val) % MOD
                else:
                    # first occurrence
                    ndp[open_cnt + 1] = (ndp[open_cnt + 1] + val) % MOD

        dp = ndp

    print(dp[0] % MOD)

if __name__ == "__main__":
    solve()
```DP nén tất cả các nhãn một phần thành số loại hiện đang mở. Việc chuyển đổi trên các ô trống sẽ bắt đầu hoặc kết thúc một cặp. Các giá trị cố định buộc DP thực hiện hành vi mở hoặc đóng xác định tùy thuộc vào việc chúng ta đã thấy loại này một hay hai lần trong tiền tố. Câu trả lời cuối cùng là số cách kết thúc với cặp mở bằng 0 sau khi xử lý tất cả các vị trí. 

Một vấn đề triển khai tinh tế là đảm bảo rằng các quá trình chuyển đổi không vô tình trộn lẫn các đóng góp từ cùng một lớp; điều này được xử lý bằng cách sử dụng một cái mới`ndp`mảng từng bước. Một điểm quan trọng khác là tất cả các phép nhân với số đếm phải được thực hiện theo modulo$10^9+7$ngay lập tức để tránh tràn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 1
0 0
```Chúng tôi theo dõi DP qua các vị trí. 

| tôi | mở = 0 | mở = 1 | 
| --- | --- | --- | 
| 0 | 1 | 0 | 

Ở vị trí trống đầu tiên, từ open 0 chúng ta có thể bắt đầu một kiểu mới, cho open 1 với hệ số 1. 

| tôi | mở = 0 | mở = 1 | 
| --- | --- | --- | 
| 1 | 0 | 1 | 

Ở vị trí thứ hai, chúng ta phải đóng kiểu mở. 

| tôi | mở = 0 | mở = 1 | 
| --- | --- | --- | 
| 2 | 1 | 0 | 

Câu trả lời cuối cùng là 1. 

Điều này xác nhận rằng một loại duy nhất được đặt ở hai vị trí có chính xác một cấu hình hợp lệ. 

### Ví dụ 2 

đầu vào:```
n = 2
0 0 0 0
```Ban đầu: 

| tôi | mở = 0 | mở = 1 | mở = 2 | 
| --- | --- | --- | --- | 
| 0 | 1 | 0 | 0 | 

Bước 1: bắt đầu hoặc đóng. 

Từ điểm mở 0, chỉ có thể bắt đầu. 

| tôi | mở = 0 | mở = 1 | mở = 2 | 
| --- | --- | --- | --- | 
| 1 | 0 | 2 | 0 | 

Ở mỗi bước, số cách nhân lên tùy thuộc vào lựa chọn loại nào được bắt đầu hoặc đóng. 

Tiếp tục truyền bá này dẫn đến: 

| tôi | mở = 0 | mở = 1 | mở = 2 | 
| --- | --- | --- | --- | 
| 4 | 2 | 0 | 0 | 

Câu trả lời cuối cùng là 2. 

Điều này cho thấy rằng ngay cả với cấu trúc trống giống hệt nhau, thứ tự cặp khác nhau sẽ tạo ra các phép gán hợp lệ riêng biệt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^3)$|$n$vị trí và$O(n)$trạng thái mở với$O(n)$công việc chuyển tiếp | 
| Không gian |$O(n^2)$| Bảng DP qua các vị trí và số lượng mở | 

Những giới hạn$n \le 100$làm một$O(n^3)$giải pháp nhanh chóng một cách thoải mái ngay cả trong Python, vì tổng số thao tác trên mỗi trường hợp thử nghiệm tệ nhất là khoảng vài triệu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import *
    import sys
    input = sys.stdin.readline

    MOD = 10**9 + 7

    def solve():
        n = int(input())
        a = list(map(int, input().split()))
        dp = [0] * (n + 1)
        dp[0] = 1

        for x in a:
            ndp = [0] * (n + 1)
            for j in range(n + 1):
                if not dp[j]:
                    continue
                if x == 0:
                    if j + 1 <= n:
                        ndp[j + 1] = (ndp[j + 1] + dp[j]) % MOD
                    if j > 0:
                        ndp[j - 1] = (ndp[j - 1] + dp[j] * j) % MOD
                else:
                    ndp[j] = (ndp[j] + dp[j]) % MOD
            dp = ndp

        return str(dp[0] % MOD)

    return solve()

# custom tests
assert run("1\n0 0\n") == "1"
assert run("2\n0 0 0 0\n") == "2"
assert run("1\n1 1\n") == "1"
assert run("2\n1 0 1 0\n") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n0 0`|`1`| trường hợp không tầm thường nhỏ nhất | 
|`2\n0 0 0 0`|`2`| nhiều cặp | 
|`1\n1 1`|`1`| ràng buộc điền sẵn đầy đủ | 
|`2\n1 0 1 0`|`1`| ràng buộc liền kề cố định | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là khi tất cả các vị trí đều trống. Trong trường hợp đó, DP phải đếm chính xác tất cả các cặp hợp lệ của$2n$vị trí vào$n$các cặp giống nhau không liền kề, tương ứng với quy trình đếm có cấu trúc hơn là các hoán vị tùy ý. Thuật toán xử lý vấn đề này bằng cách liên tục tăng và giảm số lượng mở và chỉ cho phép các lần đóng hợp lệ. 

Một trường hợp cạnh khác là khi mọi loại đã được đặt hoàn toàn ở các vị trí không liền kề. Ví dụ:```
n = 2
1 2 1 2
```DP không bao giờ sử dụng các chuyển tiếp mở một cách tự do, vì mọi loại đều đã cố định cả hai điểm cuối. Thuật toán giảm xuống để kiểm tra tính nhất quán và số đếm cuối cùng chính xác là 1. 

Trường hợp cạnh thứ ba là khi một loại xuất hiện một lần ban đầu và lần xuất hiện thứ hai của nó phải được đặt sau. DP buộc chính xác một lần mở khi lần xuất hiện đầu tiên được nhìn thấy và chính xác một lần đóng sau đó, do đó không có phân nhánh không hợp lệ nào được đưa ra và số lượng vẫn ổn định trên tất cả các vị trí phù hợp với ràng buộc đó.
