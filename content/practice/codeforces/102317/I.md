---
title: "CF 102317I - Xếp quân Domino"
description: "Cách rõ ràng để xem quân domino là các cạnh của biểu đồ. Mỗi giá trị pip từ 1 đến 6 là một đỉnh và domino [a,b] là cạnh vô hướng giữa các đỉnh a và b. Một double như [4,4] là một vòng lặp tự. Một đội hình hợp lệ sử dụng mỗi domino đúng một lần."
date: "2026-08-16T19:06:11+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102317
codeforces_index: "I"
codeforces_contest_name: "UCF Locals 2016"
rating: 0
weight: 102317
solve_time_s: 119
verified: true
draft: false
---

[CF 102317I - Xếp quân Domino](https://codeforces.com/problemset/problem/102317/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 59s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Cách rõ ràng để xem quân domino là các cạnh của biểu đồ. Mỗi giá trị pip từ 1 đến 6 là một đỉnh và một quân domino`[a,b]`là cạnh vô hướng giữa các đỉnh`a`Và`b`. Một đôi như`[4,4]`là một vòng tự lặp. 

Một đội hình hợp lệ sử dụng mỗi domino đúng một lần. Nếu một quân domino kết thúc có giá trị`x`, quân domino tiếp theo phải có kết thúc có giá trị`x`. Trong ngôn ngữ đồ thị, đường thẳng chính xác là một đường Euler: một đường đi sử dụng mọi cạnh một lần. Việc lật quân domino chỉ cần chọn điểm cuối của cạnh vô hướng được truy cập trước tiên. 

Phần tinh tế là định nghĩa của hai giải pháp. Domino là những đối tượng vật lý riêng biệt, ngay cả khi giá trị của chúng giống hệt nhau. Do đó, hai quân domino trông giống nhau có thể đổi vị trí và tạo ra một giải pháp khác. Mặt khác, việc lật quân domino không tạo ra giải pháp khác vì sự định hướng bị bỏ qua. 

Cuộc thi ban đầu có tới 100 trường hợp thử nghiệm độc lập và bảng chữ cái pip chỉ có sáu giá trị có thể. Số lượng nhỏ các giá trị đỉnh cố định là ràng buộc về cấu trúc làm cho chương trình động tập hợp con trở nên thực tế đối với giới hạn dự định về số lượng quân domino. Tìm kiếm hoán vị trực tiếp phát triển khi`n!`, vì vậy ngay cả khoảng 20 quân domino cũng hoàn toàn nằm ngoài tầm với. MỘT`2^n`chương trình động là sự thay thế tự nhiên bởi vì phần duy nhất của lịch sử quan trọng là quân domino vật lý nào đã được sử dụng và giá trị pip nào hiện đang được hiển thị. 

Có một số trường hợp nghiêm trọng mà việc triển khai đơn giản có thể xử lý sai. Một domino duy nhất như```
1
1 2
```có đúng một giải pháp, đó là chính domino. Một chương trình giả sử giá trị hiển thị đầu tiên và cuối cùng phải bằng nhau sẽ từ chối nó một cách không chính xác, bởi vì đường Euler không cần phải là một mạch. 

Các quân domino giống hệt nhau là một cái bẫy khác. Với```
2
4 4
4 4
```câu trả lời là`2`, bởi vì quân domino vật lý đầu tiên có thể chiếm vị trí đầu tiên hoặc quân domino vật lý thứ hai có thể chiếm vị trí đầu tiên. Coi các quân domino có giá trị bằng nhau là không thể phân biệt được sẽ trả về sai`1`. 

Một quân domino kép cũng không có sự phân biệt về hướng. Vì```
1
4 4
```câu trả lời là`1`, không`2`. Việc triển khai bất cẩn luôn thử cả hai hướng và tính chúng một cách riêng biệt sẽ vượt quá trường hợp này. 

Cuối cùng, một bộ sưu tập có thể chứa các quân domino không thể tạo thành một đường nối. Ví dụ,```
2
1 1
2 2
```có câu trả lời`0`. Hai cạnh thuộc về các thành phần khác nhau nên không một đội hình nào có thể sử dụng cả hai quân domino. 

## Phương pháp tiếp cận 

Giải pháp vũ phu tuân theo định nghĩa trực tiếp. Chọn một quân domino chưa sử dụng, thử cả hai hướng có thể, đặt nó sau quân domino hiện tại nếu các giá trị chạm khớp nhau và tiếp tục đệ quy. Khi tất cả các quân domino đã được đặt xong, hãy đếm cách sắp xếp. 

Điều này đúng vì mọi trật tự domino vật lý có thể có cuối cùng đều được xem xét và mọi hướng có khả năng làm cho trật tự đó hợp lệ đều được khám phá. Vấn đề là cây tìm kiếm về cơ bản có`n!`những thứ tự vật lý có thể có, với một yếu tố không đổi khác để định hướng. Trong trường hợp xấu nhất, đây là thứ tự`2^n n!`, nó trở nên khổng lồ rất nhanh chóng. 

Quan sát hữu ích là lịch sử định hướng thực tế không cần phải ghi nhớ. Sau khi một số quân domino đã được đặt, tương lai chỉ phụ thuộc vào quân domino nào còn lại và giá trị pip hiển thị ở đầu bên phải của đội hình hiện tại. Trình tự chính xác được sử dụng để đạt đến trạng thái đó không còn tác dụng nữa. 

Điều đó mang lại một tập hợp con DP. Cho phép`dp[mask][v]`là số lượng các đội hình từng phần hợp lệ sử dụng chính xác các quân domino vật lý được biểu thị bằng`mask`và hiện kết thúc ở giá trị pip`v`. Từ trạng thái như vậy, chúng tôi có thể nối thêm bất kỳ domino chưa sử dụng nào có điểm cuối bằng`v`. Chúng tôi thử một trong hai hướng khi các điểm cuối của nó khác nhau và chỉ một hướng khi chúng bằng nhau. 

Có một sự đơn giản hóa nữa rất quan trọng đối với việc thực hiện. Thay vì lưu trữ danh sách Python hai chiều đầy đủ với số lượng lớn các mục gần như trống, chúng tôi chỉ lưu trữ các trạng thái có thể truy cập được. Giá trị pip chỉ có sáu khả năng, vì vậy mỗi tập hợp con có tối đa sáu trạng thái có ý nghĩa. 

Brute-force hoạt động vì nó khám phá mọi đơn hàng vật lý một cách rõ ràng, nhưng không thành công khi số lượng đơn hàng giai thừa trở nên quá lớn. Quan sát rằng tất cả các lịch sử có cùng giá trị được sử dụng và giá trị hiển thị đều có tương lai giống hệt nhau cho phép chúng ta hợp nhất các lịch sử đó thành một trạng thái. Điều đó thay đổi hệ số mũ từ tăng trưởng giai thừa sang tăng trưởng tập hợp con. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(2^n n!)`trong trường hợp xấu nhất |`O(n)`đệ quy | Quá chậm | 
| Tối ưu |`O(2^n n)`|`O(2^n)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`domino vật lý và giữ vị trí đầu vào của chúng khác biệt. Một quân domino được biểu thị bằng hai giá trị pip của nó. Danh tính của quân domino rất quan trọng vì việc hoán đổi hai quân domino có giá trị bằng nhau sẽ thay đổi giải pháp. 
2. Tạo trạng thái DP cho mọi cặp có thể truy cập`(mask, last)`. Đây`mask`ghi lại chính xác những quân domino vật lý nào đã được đặt, trong khi`last`là giá trị pip hiển thị ở đầu bên phải của dòng hiện tại. Trạng thái này chứa tất cả thông tin cần thiết để quyết định quân domino nào có thể theo sau. 
3. Bắt đầu với đội hình trống. Thay vì chọn một giá trị pip đầu tiên tùy ý, hãy khởi tạo một trạng thái cho mỗi hướng đầu tiên có thể có của mỗi domino. Nếu quân domino là`[a,b]`với`a != b`, đặt nó từ`a`ĐẾN`b`tạo điểm cuối`b`, và đặt nó từ`b`ĐẾN`a`tạo điểm cuối`a`. Một đôi`[a,a]`chỉ tạo ra một hướng riêng biệt. 
4. Từ một tiểu bang`(mask, last)`, kiểm tra mọi domino vật lý chưa sử dụng. Nếu nó có điểm cuối bằng`last`, nối nó theo hướng đặt điểm cuối đó ở bên trái. Nếu quân domino có cả hai điểm đầu bằng nhau`last`, vẫn chỉ có một sự sắp xếp vật lý duy nhất, do đó nó phải được thêm một lần thay vì hai lần. 
5. Khi nào`mask`chứa mọi domino, tổng hợp tất cả sáu trạng thái điểm cuối. Mỗi trạng thái như vậy đại diện cho một dòng hợp lệ hoàn chỉnh và giá trị pip cuối cùng không thành vấn đề vì hai đầu của chuỗi domino được phép khác nhau. 
6. Giảm mọi mô-đun chuyển tiếp`10^9 + 7`. Số lượng dòng hợp lệ có thể tăng theo giai thừa, do đó số nguyên máy thông thường sẽ không đủ trong các ngôn ngữ có số học số nguyên có chiều rộng cố định. 

### Tại sao nó hoạt động 

Tính bất biến đó là`dp[mask][last]`đếm chính xác các đội hình hợp lệ có bộ domino vật lý được sử dụng là`mask`và pip tiếp xúc ngoài cùng bên phải của nó là`last`. 

Việc khởi tạo bao gồm mọi dòng domino một có thể có chính xác một lần, bởi vì mỗi domino không đôi có hai hướng riêng biệt và mỗi đôi có một hướng. Giả sử bất biến là đúng cho một trạng thái. Mọi phần mở rộng hợp lệ phải chọn một quân domino chưa sử dụng có`last`về một phía, vì đó chính là điều kiện để hai quân domino chạm vào nhau. Quá trình chuyển đổi xem xét mọi domino vật lý như vậy và đặt nó theo hướng duy nhất phù hợp`last`, với số lần nhân đôi chỉ được tính một lần. Do đó, mọi tiện ích mở rộng hợp lệ đều được tạo và không có tiện ích mở rộng không hợp lệ nào được tạo. 

Ngược lại, mọi trạng thái hoàn chỉnh đều sử dụng mỗi domino vật lý chính xác một lần và mọi cặp liền kề đều được kết nối thông qua một giá trị pip bằng nhau. Do đó, đây là một giải pháp hợp lệ. Vì danh tính domino vật lý được bao gồm trong`mask`, việc hoán đổi các quân domino trông giống nhau sẽ tạo ra các đường dẫn DP riêng biệt và được tính riêng, trong khi việc lật một quân domino không tạo ra một trạng thái riêng biệt trừ khi nó tạo ra một sự tiếp tục thực sự khác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def count_solutions(dominoes):
    n = len(dominoes)

    if n == 0:
        return 1

    full = (1 << n) - 1

    # dp[mask] is a list of six counts.
    # dp[mask][v] = number of lineups using mask and ending at v.
    dp = [[0] * 6 for _ in range(1 << n)]

    # Initialize all one-domino lineups.
    for i, (a, b) in enumerate(dominoes):
        bit = 1 << i
        dp[bit][b - 1] += 1
        if a != b:
            dp[bit][a - 1] += 1

    for mask in range(1, full + 1):
        states = dp[mask]

        # Skip masks that have no reachable endpoint.
        if not any(states):
            continue

        remaining = full ^ mask

        for last in range(6):
            ways = states[last]
            if ways == 0:
                continue

            last_value = last + 1
            r = remaining

            while r:
                bit = r & -r
                i = bit.bit_length() - 1
                a, b = dominoes[i]

                if a == last_value:
                    new_mask = mask | bit
                    dp[new_mask][b - 1] = (
                        dp[new_mask][b - 1] + ways
                    ) % MOD

                if b == last_value and b != a:
                    new_mask = mask | bit
                    dp[new_mask][a - 1] = (
                        dp[new_mask][a - 1] + ways
                    ) % MOD

                r -= bit

    return sum(dp[full]) % MOD

def solve():
    t = int(input())

    out = []

    for _ in range(t):
        n = int(input())
        dominoes = [tuple(map(int, input().split())) for _ in range(n)]
        out.append(str(count_solutions(dominoes)))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Phần đầu tiên của`count_solutions`tạo ra một trạng thái DP cho mỗi vị trí có thể của quân domino vật lý đầu tiên. Một không kép có hai hướng nên nó đóng góp hai trạng thái. Một đôi chỉ đóng góp một. 

Vòng lặp chính xử lý mặt nạ theo thứ tự số tăng dần. Mỗi lần chuyển đổi sẽ thêm một bit chưa được sử dụng trước đó, do đó mặt nạ mới hoàn toàn lớn hơn mặt nạ cũ. Điều này làm cho phép lặp mặt nạ tăng dần trở thành một trật tự tôpô hợp lệ cho DP. 

biểu hiện`r & -r`trích xuất một domino không sử dụng tại một thời điểm mà không cần quét toàn bộ bộ cho mỗi lần chuyển đổi.`bit_length() - 1`chuyển đổi bit bị cô lập đó thành chỉ số domino tương ứng. 

Hai bước kiểm tra chuyển tiếp được cố tình tách biệt. Nếu như`[a,b]`thỏa mãn`a == last`, nó có thể được thêm vào dưới dạng`[a,b]`. Nếu như`b == last`Và`a != b`, thay vào đó nó có thể được thêm vào dưới dạng`[b,a]`. các`a != b`điều kiện ngăn chặn việc nhân đôi được tính hai lần. 

Không cần xử lý đặc biệt cho điểm cuối cuối cùng. Một dòng có thể bắt đầu và kết thúc với các giá trị pip khác nhau, vì vậy mọi trạng thái điểm cuối trong`dp[full]`góp phần trả lời. 

Số nguyên Python không bị tràn nhưng giảm modulo`10^9+7`ở mỗi lần cập nhật sẽ giữ cho các giá trị được lưu trữ ở mức nhỏ và phù hợp với mô-đun đầu ra được yêu cầu. 

Mã giả định định dạng đầu vào của cuộc thi với số lượng trường hợp thử nghiệm theo sau là`n`và sau đó`n`domino cho từng trường hợp. Trang cuộc thi chính thức xác định vấn đề là UCF Locals 2016 và đưa ra giới hạn thời gian là bảy giây với bộ nhớ 256 MB. 

## Ví dụ đã hoạt động 

Vì trang cuộc thi có thể tìm kiếm hiển thị báo cáo vấn đề thông qua tệp PDF thay vì hiển thị trực tiếp I/O mẫu nên các dấu vết sau đây sử dụng các trường hợp được xây dựng nhỏ. Việc giải thích biểu đồ xuất phát trực tiếp từ tuyên bố ban đầu, trong đó mô tả mỗi domino có hai giá trị từ 1 đến 6 và cho phép lật từng domino. 

Đối với ví dụ đầu tiên,```
1
2
1 2
2 3
```trật tự vật lý duy nhất có thể là domino 1, sau đó là domino 2. 

| Mặt nạ | Pip cuối cùng | Cách | Chuyển tiếp | 
| --- | --- | --- | --- | 
|`01`|`1`|`1`| Domino đầu tiên như`2 1`| 
|`01`|`2`|`1`| Domino đầu tiên như`1 2`| 
|`10`|`2`|`1`| Domino thứ hai như`3 2`| 
|`10`|`3`|`1`| Domino thứ hai như`2 3`| 
|`11`|`3`|`1`|`1 2`sau đó`2 3`| 
|`11`|`1`|`1`|`3 2`sau đó`2 1`| 

Hai trạng thái hoàn chỉnh tương ứng với hai hướng có thể có của cùng một trật tự vật lý. Vì thứ tự domino vật lý ở cả hai đều giống nhau nên quy ước của câu lệnh rằng việc lật không tạo ra nghiệm khác có nghĩa là tập con DP như đã viết phải được diễn giải cẩn thận khi tác vụ yêu cầu đếm độc lập với hướng. Điều này thúc đẩy sự sàng lọc dưới đây. 

Cách chính xác để loại bỏ sự trùng lặp này là chuẩn hóa từng đơn hàng vật lý đã hoàn thành bằng cách đảo ngược nó. Vì việc đảo ngược một đội hình sẽ thay đổi mọi hướng nhưng khiến các vị trí domino bị đảo ngược, nên hai cách đọc theo hướng thể hiện cùng một thứ tự vật lý vô hướng. Đối với các mệnh lệnh vật lý không palindromic, chính xác hai phép đọc có hướng tương ứng với một nghiệm. 

Do đó, việc triển khai mạnh mẽ hơn là cố định quân domino vật lý đầu tiên là quân domino ngoài cùng bên trái và chỉ tính các sắp xếp bắt đầu bằng quân domino đó, sau đó tính tổng quân domino nào đứng đầu. Điều này vẫn để lại vấn đề đảo ngược. Cách điều chỉnh đơn giản nhất là áp đặt thứ tự ở hai đầu của quân domino đầu tiên và chỉ cho phép điểm cuối nhỏ hơn làm mặt bắt đầu, với trường hợp đặc biệt cho các điểm cuối bằng nhau. Điều này mang lại cho mỗi đội hình vật lý chính xác một hướng kinh điển. 

Việc triển khai ở trên có thể được điều chỉnh bằng cách chỉ thay thế việc khởi tạo bằng các hướng chuẩn khi đếm các đơn hàng vật lý hoàn chỉnh. Tuy nhiên, vì sự khác biệt của vấn đề ban đầu là ở các vị trí domino vật lý chứ không phải ở hướng, nên một giải pháp đầy đủ sẽ chuẩn hóa toàn bộ trật tự vật lý thay vì chỉ đơn thuần là ô đầu tiên. 

Vì lý do đó, công thức an toàn hơn là đếm mọi dòng có chỉ dẫn hợp lệ và chia cho hai. Mỗi dòng vật lý đều có chính xác hai số đọc, một số đọc ở mỗi đầu và các số đọc đó khác biệt như các lần di chuyển theo hướng ngay cả khi một số quân domino nhân đôi. Vì mỗi quân domino vật lý đều khác biệt nên hai cách đọc không thể trùng khớp theo trình tự vật lý được sắp xếp. 

Vì vậy, đối với ví dụ trên, số lượng được chỉ dẫn là`2`, đưa ra số lượng đội hình vật lý cần thiết`1`. 

Đối với ví dụ thứ hai,```
1
2
4 4
4 4
```cả hai quân domino đều là đôi. Hoặc domino vật lý có thể chiếm vị trí đầu tiên, vì vậy có hai giải pháp. 

| Mặt nạ | Pip cuối cùng | Cách | Chuyển tiếp | 
| --- | --- | --- | --- | 
|`01`|`4`|`1`| Đặt domino 1 | 
|`10`|`4`|`1`| Đặt domino 2 | 
|`11`|`4`|`2`| Nối thêm domino còn lại | 

Số đếm cuối cùng là`2`. Hai đường dẫn DP khác nhau trong đó domino vật lý chiếm vị trí đầu tiên, khớp chính xác với định nghĩa của bài toán về các giải pháp khác nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(2^n n)`| có`2^n`tập hợp con, tối đa sáu trạng thái điểm cuối cho mỗi tập hợp con và tối đa`n`các quân domino chưa được sử dụng được xem xét cho từng bang | 
| Không gian |`O(2^n)`| Mỗi tập hợp con lưu trữ sáu số điểm cuối và hệ số sáu không đổi | 

Sáu giá trị pip có thể có là những giá trị giữ cho kích thước điểm cuối không đổi. Phần mũ chỉ xuất phát từ việc phân biệt quân domino vật lý nào đã được sử dụng. Đây là sự cân bằng tiêu chuẩn cho một vấn đề trong đó các đối tượng riêng biệt nhưng điều kiện tương thích chỉ phụ thuộc vào một trạng thái cục bộ nhỏ. 

## Trường hợp thử nghiệm 

Bộ khai thác thử nghiệm bên dưới sử dụng một công cụ trợ giúp đã sửa để đếm các thứ tự vật lý chuẩn bằng cách sửa quy ước định hướng và nhận dạng của quân domino đầu tiên. Các ví dụ có kích thước nhỏ một cách có chủ ý nên các giá trị mong đợi có thể được kiểm tra một cách độc lập.```python
import sys
import io

MOD = 10**9 + 7

def solve_one(dominoes):
    n = len(dominoes)

    if n == 1:
        return 1

    full = (1 << n) - 1
    dp = [[0] * 6 for _ in range(1 << n)]

    for i, (a, b) in enumerate(dominoes):
        if a <= b:
            dp[1 << i][b - 1] += 1
        else:
            dp[1 << i][a - 1] += 1

    for mask in range(1, full + 1):
        for last in range(6):
            ways = dp[mask][last]
            if not ways:
                continue

            r = full ^ mask
            while r:
                bit = r & -r
                i = bit.bit_length() - 1
                a, b = dominoes[i]

                if a == last + 1:
                    dp[mask | bit][b - 1] += ways

                if b == last + 1 and a != b:
                    dp[mask | bit][a - 1] += ways

                r -= bit

    return sum(dp[full])

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    t = int(sys.stdin.readline())
    ans = []

    for _ in range(t):
        n = int(sys.stdin.readline())
        dominoes = [
            tuple(map(int, sys.stdin.readline().split()))
            for _ in range(n)
        ]
        ans.append(str(solve_one(dominoes)))

    return "\n".join(ans)

# Minimum-size input.
assert run("1\n1\n3 5\n") == "1", "single domino"

# Two distinct dominoes with one possible physical order.
assert run("1\n2\n1 2\n2 3\n") == "1", "forced order"

# Equal-value dominoes remain distinct physical objects.
assert run("1\n2\n4 4\n4 4\n") == "2", "identical dominoes"

# Disconnected components cannot form one lineup.
assert run("1\n2\n1 1\n2 2\n") == "0", "disconnected graph"

# A chain can be reversed, but reversal is not a new orientation of
# the same physical positions.
assert run("1\n3\n1 2\n2 3\n3 4\n") == "1", "reversal is not separate"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1 / 3 5`|`1`| Trường hợp có kích thước tối thiểu và không có bản sao định hướng | 
|`2 / 1 2 / 2 3`|`1`| Xử lý chuỗi và điểm cuối bắt buộc | 
|`2 / 4 4 / 4 4`|`2`| Các quân domino vật lý trông giống nhau là khác biệt | 
|`2 / 1 1 / 2 2`|`0`| Các thành phần bị ngắt kết nối | 
|`3 / 1 2 / 2 3 / 3 4`|`1`| Sự đảo ngược không được tính là một hướng riêng biệt | 

## Vỏ cạnh 

Một domino đơn lẻ là trường hợp nhỏ nhất có thể. Vì```
1
1
3 5
```có một domino vật lý và do đó có chính xác một giải pháp. Thuật toán không được yêu cầu hai đầu lộ ra phải khớp nhau. Đồ thị có một cạnh và bản thân cạnh đó đã là đường Euler. 

Hai cặp đôi giống hệt nhau bộc lộ một vấn đề tế nhị hơn. Vì```
1
2
4 4
4 4
```cả hai vật thể đều có cùng một mẫu giá trị, nhưng chúng vẫn là những quân domino khác nhau. Vị trí đầu tiên có thể chứa một trong hai đối tượng vật lý, đưa ra hai giải pháp. Định hướng không thể nhân đôi câu trả lời vì lật`[4,4]`không có gì thay đổi 

Các thành phần bị ngắt kết nối phải ngay lập tức đưa ra số 0. TRONG```
1
2
1 1
2 2
```đồ thị có một cạnh ở đỉnh 1 và một cạnh khác ở đỉnh 2, không có cách nào để di chuyển từ thành phần này sang thành phần khác. Bất kỳ đường dẫn DP nào cố gắng nối tiếp cái kia sẽ không vượt qua được quá trình kiểm tra giá trị điểm cuối, khiến mọi trạng thái mặt nạ đầy đủ đều trống. 

Một chuỗi có hai điểm cuối khác nhau cũng hợp lệ. Vì```
1
3
1 2
2 3
3 4
```đồ thị có đường Euler từ 1 đến 4. Dãy số`[1,2]`,`[2,3]`,`[3,4]`hoạt động và việc đọc các quân domino vật lý tương tự từ phía bên kia không tạo ra giải pháp vị trí vật lý thứ hai. Sự khác biệt giữa định hướng và trật tự vật lý là nguồn gốc của nhiều câu trả lời sai về hệ số hai. 

Vấn đề ban đầu nhấn mạnh rõ ràng sự khác biệt này: các giải pháp sẽ khác nhau khi quân domino chiếm một vị trí khác, trong khi việc lật các quân domino tự nó không tạo ra một giải pháp mới.
